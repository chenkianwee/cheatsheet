# Experiment procedure

## Post processing data of IoT sensor using FROST-Server and Mini & High RES Sensor
Here are some python scripts that might be useful for post processing the data after an experiment.

### Script to extract data from the FROST Server through timescaledb interface.
    ```
    import psycopg2
    from sshtunnel import SSHTunnelForwarder
    import csv
    import pytz
    
    # input = [1, 'a006_panel_temp'] # only for day 19072023 to 20072023
    # input = [2, 'arg001_panel_temp']
    # input = [5, 'bo001_outlet_air_temp']
    # input = [6, 'bo001_outlet_rel_humid']
    # input = [7, 'bo001_outlet_abs_humid']
    # input = [11, 'ph021_globe_temp']
    # input = [13, 'a010_air_temp']
    # input = [14, 'a010_rel_humid']
    input = [15, 'a010_abs_humid']
    
    ds_id = input[0]
    res_path = '/home/usr/input[1] + '.csv'
    rows2d = [['datetime', input[1]]]
    st_time_str = '2023-07-19 00:00:00-4'
    end_time_str = '2023-07-20 17:30:00-4'
    #%% Function
    def get_data(ds_id, st_time_str, end_time_str):
        PORT=5432
        with SSHTunnelForwarder(('chaosbox.edu'),
                                ssh_username='',
                                ssh_password='',
                                remote_bind_address=('localhost', PORT),
                                local_bind_address=('localhost', PORT)):
        
            try:
                conn = psycopg2.connect(dbname='spatempdb',
                                        user='postgres',
                                        host='localhost',
                                        port = 5432,
                                        password='postgres')
                cursor = conn.cursor()
                print('connected', cursor)
            except:
                print('notconnected')
        
            ds_id = str(ds_id) # the datastream id you want to query
            st_time = st_time_str #'2021-03-30 12:29:00-4'
            end_time = end_time_str #'2021-03-30 13:29:00-4'
            
            command =   """
                        SELECT "RESULT_TIME", 
                        "RESULT_NUMBER"
                        FROM public."OBSERVATIONS"
                        WHERE "DATASTREAM_ID" = %s and "RESULT_TIME" > '%s' and "RESULT_TIME" < '%s'
                        ORDER BY "RESULT_TIME" ASC;
                        """ %(ds_id, st_time, end_time)
        
            cursor.execute(command)
            rows = cursor.fetchall()
            cursor.close()
            return rows
    #%% Main
    rows1 = get_data(ds_id, st_time_str, end_time_str)
    for row in rows1:
        local_time = row[0].astimezone(pytz.timezone('America/New_York'))
        lt_str = local_time.isoformat()
        rows2d.append([lt_str, row[1]])
    
    with open(res_path, 'w', newline='') as csvfile: 
        # creating a csv writer object 
        csvwriter = csv.writer(csvfile) 
        # writing the data rows 
        csvwriter.writerows(rows2d)
    
    ```
### Combine all the extracted data from FROST
    ```
    import os
    import pandas as pd

    iot_dir = '/home/raw/'
    res_path = '/home/usrname/raw.csv'

    file_ls = sorted(os.listdir(iot_dir))
    df_ls = []
    for f in file_ls:
        fsplit = f.split('.')
        ext = fsplit[-1]
        if ext == 'csv':
            filepath = os.path.join(iot_dir, f)
            df = pd.read_csv(filepath, sep=',')
            df['datetime'] = pd.to_datetime(df['datetime'])
            df.set_index('datetime', inplace=True)
            df_ls.append(df)
            
    new_df = pd.concat(df_ls, axis=1)
    new_df.to_csv(res_path)
    ```
### Process the combine file
    ```
    import pandas as pd

    iot_path = '/home/usrname/raw.csv'
    res_path = '/home/usrname/iot.csv'

    df = pd.read_csv(iot_path, sep=',')
    df['datetime'] = pd.to_datetime(df['datetime'])
    df.set_index('datetime', inplace=True)
    df = df.resample('1T').median()
    df.to_csv(res_path)
    ```
### Process the mini-res data
    ```
    import pytz
    import pandas as pd

    mini_res_path = '/home/usrname//RADICUBE.csv'
    res_path = '/home/usrname/minires.csv'
    # read the csv
    df_mini = pd.read_csv(mini_res_path, sep = ',')
    # convert the datetime string to datetime object
    df_mini['datetime'] = df_mini['DD-MM-YYYY'] + 'T' + df_mini[' HH:MM:SS']
    df_mini['datetime'] = pd.to_datetime(df_mini['datetime'], dayfirst=True, errors='coerce')
    df_mini = df_mini.dropna(subset=['datetime'])
    df_mini['datetime'] = df_mini['datetime'] + pd.to_timedelta('2hour') # for mini res
    df_mini['datetime'] = df_mini['datetime'].dt.tz_localize(pytz.timezone('America/New_York'))
    # set index with the datetime column
    df_mini.set_index('datetime', inplace=True)
    start_time_str = ['21-07-2023 00:00:00', '21-07-2023 18:30:00']
    se_time_dt = pd.to_datetime(start_time_str, dayfirst =True).tz_localize(pytz.timezone('America/New_York'))
    df_mini = df_mini.loc[se_time_dt[0]:se_time_dt[1]]

    # resample the timeseries into 1 min interval taking the median of that minute
    df_mini = df_mini.resample('1T').median(numeric_only=True)
    df_mini.to_csv(res_path)
    ```
### Process the hiRES sensor data
    ```
    import pytz
    import pandas as pd

    mini_res_path = '/home/usrname/ThermalArrayDatalog.csv'
    res_path = '/home/usrname/highres.csv'
    start_time_str = ['21-07-2023 00:00:00', '21-07-2023 19:00:00']
    # read the csv
    df_mini = pd.read_csv(mini_res_path, sep = ',')
    # convert the datetime string to datetime object
    df_mini['datetime'] = df_mini['MM-DD-YYYY'] + 'T' + df_mini[' HH:MM:SS']
    df_mini['datetime'] = pd.to_datetime(df_mini['datetime'], dayfirst=True, errors='coerce')
    df_mini = df_mini.dropna(subset=['datetime'])
    df_mini['datetime'] = df_mini['datetime'] - pd.to_timedelta('6hour') # for mini res
    df_mini['datetime'] = df_mini['datetime'].dt.tz_localize(pytz.timezone('America/New_York'))
    # set index with the datetime column
    df_mini.set_index('datetime', inplace=True)
    se_time_dt = pd.to_datetime(start_time_str, dayfirst =True).tz_localize(pytz.timezone('America/New_York'))
    df_mini = df_mini.loc[se_time_dt[0]:se_time_dt[1]]

    # resample the timeseries into 1 min interval taking the median of that minute
    df_mini = df_mini.resample('5T').median(numeric_only=True)
    df_mini.to_csv(res_path)
    ```
### Process the hiRES ply files if necessary
    ```
    import os 
    import dateutil
    import datetime

    hires_dir = '/home/usrname/20230721_highres'
    se_dt_string = ['2023-07-21T00:00:00', '2023-07-21T23:59:59']

    filenames = sorted(os.listdir(hires_dir))
    # print(filenames)
    for filename in filenames:
        path_split = filename.split('.')
        ext = path_split[-1]
        if ext == 'PLY' or ext == 'BMP':
            filename_only = path_split[0]
            fn_split = filename_only.split('_')
            date_str = fn_split[1]#.replace()
            time_str = fn_split[2].replace('-', ':')
            datetime_str = date_str + ' ' + time_str
            dt = dateutil.parser.parse(datetime_str)
            dt = dt - datetime.timedelta(hours=6)
            st_dt = dateutil.parser.parse(se_dt_string[0])
            end_dt = dateutil.parser.parse(se_dt_string[1])
            orig_path = os.path.join(hires_dir, filename)
            if st_dt <= dt <= end_dt:
                new_dt_str = dt.strftime('%m-%d-%y_%H-%M-%S')
                new_filename = fn_split[0] + '_' + new_dt_str + '_' + fn_split[3] + '.' + ext
                new_path = os.path.join(hires_dir, new_filename)
                os.rename(orig_path, new_path)
            else:
                os.remove(orig_path)
            
    ```
### Create the 3d model of the space for projecting hiRES data 
    ```
    import ezdxf
    import geomie3d

    dxf_path = '/home/usrname/floorplan.dxf'
    res_path1 = '/home/usrname/room215.geomie3d'
    res_path2 = '/home/usrname/room025.geomie3d'
    #%%Function
    def create_scene215():
        doc = ezdxf.readfile(dxf_path)
        msp = doc.modelspace()
        
        poly_pts = []
        for e in msp:
            if e.dxf.layer == 'walls':
                if e.dxftype() == 'LINE':
                    st_pt = [round(e.dxf.start[0],6), round(e.dxf.start[1],6), round(e.dxf.start[2],6)]
                    end_pt = [round(e.dxf.end[0],6), round(e.dxf.end[1],6), round(e.dxf.end[2],6)]
                    poly_pts.append(st_pt)
                    poly_pts.append(end_pt)
        
        vlist = geomie3d.create.vertex_list(poly_pts)
        vlist = geomie3d.modify.fuse_vertices(vlist)
        poly = geomie3d.create.polygon_face_frm_verts(vlist)
        ext = geomie3d.create.extrude_polygon_face(poly, [0,0,1], 2.4)
        faces = geomie3d.get.faces_frm_solid(ext)
        
        rev_faces = []
        for cnt,f in enumerate(faces):
            f = geomie3d.modify.reverse_face_normal(f)
            geomie3d.modify.update_topo_att(f, {'name': 'surface' + str(cnt)})
            rev_faces.append(f)
        
        geomie3d.utility.write2geomie3d(rev_faces, res_path1)

    def create_scene025():
        box = geomie3d.create.box(4.1, 3.8, 2.4)
        mv_box = geomie3d.modify.move_topo(box, [0,0,0], ref_xyz = [0,0,-1.2])
        srfs = geomie3d.get.faces_frm_solid(mv_box)
        bdry_srfs = []
        for cnt,s in enumerate(srfs):
            new_s = geomie3d.modify.reverse_face_normal(s)
            geomie3d.modify.update_topo_att(new_s, {'name': 'surface' + str(cnt)})
            bdry_srfs.append(new_s)
        
        geomie3d.utility.write2geomie3d(bdry_srfs, res_path2)

    #%%Main
    if __name__ == '__main__':
        create_scene215()
        create_scene025()
    ```
    
### Project the hiRES data onto the 3d model 
    ```
    import os
    import copy
    import statistics
    from pathlib import Path
    from time import perf_counter
    import geomie3d
    #%%Parameter
    therm_scan_dir = '/home/usrname/_highres/'
    scene_path = '/home/usrname/model.geomie3d'
    sensor_pos = [2.2, 2.3, 1]

        
    viz = False
    #%%Function
    def write2ply(res_path, xyz_ls, temp_ls, header):
        nvs = len(xyz_ls)
        nvs_line = 'element vertex ' + str(nvs) + '\n'
        v_cnt = 0
        for cnt, h in enumerate(header):
            hsplit = h.split(' ')
            if hsplit[0] == 'element':
                if hsplit[1] == 'vertex':
                    v_cnt = cnt
        
        header_w = header[:]
        header_w[v_cnt] = nvs_line
        for cnt,xyz in enumerate(xyz_ls):
            temp = temp_ls[cnt]
            v_str = str(xyz[0]) + ' ' + str(xyz[1]) + ' ' + str(xyz[2]) + ' ' + str(temp) + '\n'
            header_w.append(v_str)
        
        f = open(res_path, "w")
        f.writelines(header_w)
        f.close()
        
    def check_ply_file(header):
        ref_h = ['ply', 'format ascii 1.0',
                 'element vertex',
                 'property float x', 
                 'property float y', 
                 'property float z', 
                 'property float temperature', 
                 'end_header']
        
        check = 0
        for h in header:
            h = h.lower()
            h = h.replace('\n','')
            
            hsplit = h.split(' ')
            if hsplit[0] == 'comment':
                ctype = hsplit[1]
                if ctype=='date' or ctype=='time' or ctype=='sensorid' or ctype=='sensortype':
                    h = hsplit[0] + ' ' + ctype
            
            elif hsplit[0] == 'element':
                if hsplit[1] == 'vertex':
                    h = hsplit[0] + ' ' + hsplit[1]
                    
            if h in ref_h:
                # print(h)
                check+=1
        
        if check == 8:
            return True
        else:
            return False
        
    def read_therm_arr_ply(file_path):    
        with open(file_path) as f:
            lines = f.readlines()
        nheaders = 8
        #check if this is a valid chaosense file
        headers = lines[0:nheaders]
        isValid = check_ply_file(headers)
        if isValid:
            xyzs = []
            verts_data = lines[nheaders:]
            temp_ls = []
            temp_dls = []
            for v in verts_data:
                v = v.replace('\n', '')
                v = v.replace('\t', ' ')
                vsplit = v.split(' ')
                vsplit = list(map(float, vsplit))
                xyzs.append(vsplit[0:3])
                temp_dls.append({'temperature':vsplit[3]})
                temp_ls.append(vsplit[3])
            
            v_ls = geomie3d.create.vertex_list(xyzs, attributes_list=temp_dls)
            return v_ls, temp_ls, headers
        else:
            return [], [], headers
        
    def project(therm_scan_path, sensor_pos, scene_faces, viz = False):
        verts_data, temp_ls, headers = read_therm_arr_ply(therm_scan_path)
        cmp = geomie3d.create.composite(verts_data)
        # axis = [0,0,1]
        # rotation = 0
        # cmp = geomie3d.modify.rotate_topo(cmp, axis, rotation)
        verts_data = geomie3d.get.vertices_frm_composite(cmp)
        #convert the verts to rays and 
        rays = []
        for vcnt,v in enumerate(verts_data):
            temp = v.attributes['temperature']
            ray = geomie3d.create.ray(sensor_pos, v.point.xyz, attributes = {'temperature':temp, 'id': vcnt})
            rays.append(ray)
            
        hrays,mrays,hit_faces,miss_faces = geomie3d.calculate.rays_faces_intersection(rays, scene_faces)
        if viz == True:
            geomie3d.viz.viz_falsecolour(verts_data, temp_ls)
            vcmp = geomie3d.create.composite(verts_data)
            mv_v = geomie3d.modify.move_topo(vcmp, sensor_pos)
            mv_vs = geomie3d.get.vertices_frm_composite(mv_v)
            
            cmp = geomie3d.create.composite(scene_faces)
            edges = geomie3d.get.edges_frm_composite(cmp)
            geomie3d.viz.viz_falsecolour(mv_vs, temp_ls, other_topo_dlist=[{'topo_list':edges, 'colour':'white'}])
            viz_projection(hrays, mrays, scene_faces)
        return hrays,mrays,hit_faces,miss_faces, headers

    def calc_avg_temp_srf(proj_face):
        att = proj_face.attributes
        if 'rays_faces_intersection' in att:
            int_att = att['rays_faces_intersection']
            rays = int_att['ray']
            if len(rays) !=0:
                temp_ls = [r.attributes['temperature'] for r in rays]
                avg_temp = statistics.median(temp_ls)
                geomie3d.modify.update_topo_att(proj_face, {'temperature':avg_temp})
                return avg_temp
            else:
                return None

    def viz_projection(hrays, mrays, scene_faces):
        v_ls = []
        temp_ls = []
        for hray in hrays:
            att = hray.attributes
            att2 = att['rays_faces_intersection']
            intersects = att2['intersection']
            temp = att['temperature']
            for intersect in intersects:
                v = geomie3d.create.vertex(intersect, attributes={'temperature':temp})
                temp_ls.append(temp)
                v_ls.append(v)
        
        cmp = geomie3d.create.composite(scene_faces)
        edges = geomie3d.get.edges_frm_composite(cmp)
        geomie3d.viz.viz_falsecolour(v_ls, temp_ls, 
                                         other_topo_dlist=[{'topo_list': edges, 'colour': 'white'}], 
                                         false_min_max_val=None)
        
    #%%Main
    t0 = perf_counter()
    model_dict = geomie3d.utility.read_geomie3d(scene_path)
    bdry_srfs = model_dict['topo_list']

    if viz == True:
        geomie3d.viz.viz([{'topo_list': bdry_srfs, 'colour': 'blue'}])

    #read the file and get all the vert data
    parent_path = Path(therm_scan_dir).parent.absolute()
    res_dir = os.path.join(parent_path, 'projected')
    if not os.path.isdir(res_dir): 
        os.mkdir(res_dir)
        
    ff_ls = sorted(os.listdir(therm_scan_dir))
    ply_name_ls = []
    for d in ff_ls:
        file_ext = d.split('.')[-1].lower()
        if file_ext == 'ply':
            ply_name_ls.append(d)

    t1 = perf_counter()
    print('Time taken to process base scene (mins):', round((t1-t0)/60, 1))

    for cnt, ply_name in enumerate(ply_name_ls):
        new_bdry_srfs = copy.deepcopy(bdry_srfs)
        t2 = perf_counter()
        ply_path = os.path.join(therm_scan_dir, ply_name)
        hrays,mrays,hit_faces,miss_faces,header = project(ply_path, sensor_pos, new_bdry_srfs, viz=viz)
        xyz_ls = []
        temp_ls = []
        for hray in hrays:
            att = hray.attributes
            int_att = att['rays_faces_intersection']
            xyz = int_att['intersection'][0]
            fs = int_att['hit_face']
            temp = att['temperature']
            xyz_ls.append(xyz)
            temp_ls.append(temp)
        
        for hf in hit_faces:
            avg_temp = calc_avg_temp_srf(hf)
            att = {'name': hf.attributes['name'], 'temperature': avg_temp}
            geomie3d.modify.overwrite_topo_att(hf, att)
            
        # print(ttl_rays_cnt)
        nsplit = ply_name.split('.')
        res_path1 = os.path.join(res_dir, nsplit[0] + '_projected.ply' )
        res_path2 = os.path.join(res_dir, nsplit[0] + '_projected.geomie3d' )
        geomie3d.utility.write2geomie3d(hit_faces, res_path2)
        write2ply(res_path1, xyz_ls, temp_ls, header)
        t3 = perf_counter()
        time_take = round((t3-t2)/60,1)
        print('Time taken to project one file (mins):', time_take, cnt)

    ```
### Visualize the mrt and air temperature 
    ```
    import pytz
    from dateutil import parser
    import pandas as pd
    import geomie3d 

    #%% settings
    mini_res_path = '/home/usrname/minires.csv'
    high_res_path = '/home/usrname/highres.csv'
    iot_path = '/home/usrname/iot.csv'
    res_path = '/home/usrname/res.png'

    graph_title = 'Air vs MRT 2023-07-19'
    ylim = [21.5, 25]
    start_time_str = ['19-07-2023 08:00:00', '19-07-2023 18:30:00']
    regions = [{'label': 'vav on rad off', 'orientation': 'vertical', 'range': ['19-07-2023 08:00:00', '19-07-2023 12:30:00'],'colour': (1,0,0,0.3)},
                {'label': 'vav on rad on', 'orientation': 'vertical', 'range': ['19-07-2023 12:30:00', '19-07-2023 14:54:00'],'colour': (0,1,0,0.3)},
                {'label': 'vav off rad on', 'orientation': 'vertical', 'range': ['19-07-2023 14:54:00', '19-07-2023 16:35:00'],'colour': (0,0,1,0.3)},
                {'label': '', 'orientation': 'vertical', 'range': ['19-07-2023 16:35:00', '19-07-2023 18:30:00'],'colour': (1,0,0,0.3)}]
        
    #%% functions
    def naive_dt(dt):
        ndt = []
        for d in dt:
            ndt.append(d.replace(tzinfo=None))
        return ndt

    def convert_region_dates(regions):
        for reg in regions:
            new_range = [parser.parse(reg['range'][0], dayfirst = True), parser.parse(reg['range'][1], dayfirst = True)]
            reg['range'] = new_range
        
    #%% Main
    se_time_dt = pd.to_datetime(start_time_str, dayfirst =True).tz_localize(pytz.timezone('America/New_York'))

    #read mini_res
    df_mini = pd.read_csv(mini_res_path, sep = ',')
    df_mini['datetime'] = pd.to_datetime(df_mini['datetime'])
    df_mini.set_index('datetime', inplace=True)
    df_mini = df_mini.loc[se_time_dt[0]:se_time_dt[1]]
    mrt = df_mini[' TOTAL MLX LW']
    air_temp = df_mini[' TEMP']
    diff = air_temp - mrt
    dt1 = df_mini.index.to_pydatetime()
    dt1 = naive_dt(dt1)

    # read highres
    df_high = pd.read_csv(high_res_path, sep = ',')
    df_high['datetime'] = pd.to_datetime(df_high['datetime'])
    df_high.set_index('datetime', inplace=True)
    df_high = df_high.loc[se_time_dt[0]:se_time_dt[1]]
    mrt_highres = df_high[' TOTAL HTPA LW']
    mrt_highres_mlx = df_high[' TOTAL MLX LW']
    dt2 = df_high.index.to_pydatetime()
    dt2 = naive_dt(dt2)

    #read sht31
    df_air = pd.read_csv(iot_path, sep = ',')
    df_air['datetime'] = pd.to_datetime(df_air['datetime'])
    df_air.set_index('datetime', inplace=True)
    df_air = df_air.loc[se_time_dt[0]:se_time_dt[1]]
    air_temp_sht31 = df_air['a010_air_temp']
    dt3 =  df_air.index.to_pydatetime()
    dt3 = naive_dt(dt3)


    #%% matplotlib
    data_dict_ls = []
    data_d = {'datax':dt1, 'datay':mrt, 'linestyle':'solid', 'linewidth': 1, 'marker':'',
              'marker_size':2, 'color': 'r', 'label': 'mini_mrt'}
    data_dict_ls.append(data_d)

    data_d = {'datax':dt2, 'datay':mrt_highres, 'linestyle':'solid', 'linewidth': 1, 'marker':'',
              'marker_size':2, 'color': 'b', 'label': 'mrt_highres'}
    data_dict_ls.append(data_d)

    data_d = {'datax':dt2, 'datay':mrt_highres_mlx, 'linestyle':'solid', 'linewidth': 1, 'marker':'',
              'marker_size':2, 'color': 'k', 'label': 'mrt_highres_mlx'}
    data_dict_ls.append(data_d)

    data_d = {'datax':dt1, 'datay':air_temp, 'linestyle':'dashed', 'linewidth': 1, 'marker':'',
              'marker_size':2, 'color': 'g', 'label': 'mini_air'}
    data_dict_ls.append(data_d)

    data_d = {'datax':dt3, 'datay':air_temp_sht31, 'linestyle':'dashed', 'linewidth': 1, 'marker':'',
              'marker_size':2, 'color': 'c', 'label': 'air_temp_sht31'}
    data_dict_ls.append(data_d)

    xaxis_label = 'Time (H)'
    yaxis_label = 'Temp (degC)'
    dateformat = '%H:%M'
    legend_loc = {'loc': 'upper center', 'bbox_to_anchor': (0.45, -0.25), 'ncol':3}

    convert_region_dates(regions)
    geomie3d.utility.viz1axis_timeseries(data_dict_ls, graph_title , xaxis_label, yaxis_label, res_path, yaxis_lim = ylim, 
                                          legend_loc = legend_loc, dateformat = dateformat, regions = regions, tight_layout = False)


    ```
### Visualize spatial time-series data 
    ```
    import os
    import pytz
    from time import perf_counter
    from dateutil.parser import parse

    import geomie3d
    import pandas as pd
    #%%Parameter
    proj_dir = '/home/usrname/projected/'
    highres_path = '/home/usrname/highres.csv'
    minires_path = '/home/usrname/minires.csv'

    #%%Function 
    def get_datetime_ply(ply_name):
        nsplit = ply_name.split('_')
        d = nsplit[1]
        t = nsplit[2]
        t = t.replace('-', ':')
        dt_str = d + 'T' + t
        dt = parse(dt_str)
        local_time = dt.astimezone(pytz.timezone('America/New_York'))
        return local_time

    def convert_region_dates(regions):
        for reg in regions:
            new_range = [parse(reg['range'][0] + '-04:00', dayfirst = True), parse(reg['range'][1] + '-04:00', dayfirst = True)]
            reg['range'] = new_range
            
    #%%Main
    t0 = perf_counter()
    #----------------------------------------------------------------------------------------------------------
    # read all the projected ply files and viz them
    ff_ls = sorted(os.listdir(proj_dir))

    topo_2dlist = []
    results2d = []
    topo_datetime_ls = []
    other_topo_2ddlist = []
    for d in ff_ls:
        file_ext = d.split('.')[1].lower()
        if file_ext == 'geomie3d':
            dt = get_datetime_ply(d)
            geomie_path = os.path.join(proj_dir, d)
            model_dict = geomie3d.utility.read_geomie3d(geomie_path)
            topo_list = model_dict['topo_list']
            topo_list = [geomie3d.modify.reverse_face_normal(topo) for topo in topo_list]
            temps = [topo.attributes['temperature'] for topo in topo_list]
            
            topo_datetime_ls.append(dt)
            topo_2dlist.append(topo_list)
            results2d.append(temps)
        
    #----------------------------------------------------------------------------------------------------------
    # get the high.res mrt
    df_hr = pd.read_csv(highres_path, sep = ',')
    high_res_mrt = df_hr[' TOTAL HTPA LW']
    datetime = pd.to_datetime(df_hr['datetime'])
    datetime_hr  = datetime.dt.to_pydatetime()

    # get minires mrt and air temperature
    df_mr = pd.read_csv(minires_path, sep = ',')
    mr_mrt = df_mr[' TOTAL MLX LW']
    mr_air = df_mr[' TEMP']
    datetime = pd.to_datetime(df_mr['datetime'])
    datetime_mr  = datetime.dt.to_pydatetime()

    xvalues2d = [datetime_mr, datetime_mr, datetime_hr]
    yvalues2d = [mr_mrt, mr_air, high_res_mrt]
    colour_ls = [[255,255,255,150], [255,0,0,150], [0,255,0,150]]

    legend = ['mini_mrt', 'mini_air', 'highres_mrt']

    regions = [{'label': 'vav on rad off', 'orientation': 'vertical', 'range': ['19-07-2023 08:00:00', '19-07-2023 12:30:00'],
                'colour': (255,0,0,80)},
                {'label': 'vav on rad on', 'orientation': 'vertical', 'range': ['19-07-2023 12:30:00', '19-07-2023 14:54:00'],
                 'colour': (0,255,0,80)},
                {'label': 'vav off rad on', 'orientation': 'vertical', 'range': ['19-07-2023 14:54:00', '19-07-2023 16:35:00'],
                 'colour': (0,0,255,80)},
                {'label': 'vav on rad off', 'orientation': 'vertical', 'range': ['19-07-2023 16:35:00', '19-07-2023 18:30:00'],
                 'colour': (255,0,0,80)}]

    convert_region_dates(regions)

    geomie3d.viz.viz_st(topo_2dlist, results2d, topo_datetime_ls, xvalues2d, yvalues2d, colour_ls, false_min_max_val=[15,28], 
                        legend = legend, regions=regions, other_topo_2ddlist=other_topo_2ddlist, ylabel='Temperature', yunit='^oC')

    ```

### calculate the aust of the space
    ```
    import os
    import dateutil

    import geomie3d
    #%% Parameters
    geomie3d_dir = '/home/usrname/projected/'
    aust_path = '/home/usrname/aust.csv'

    is_215 = False
    #%% Functions
    def calc_aust(face_list):
        ttl_area = 0
        for f in face_list:
            area = round(geomie3d.calculate.face_area(f),2)
            geomie3d.modify.update_topo_att(f, {'area':area})
            ttl_area += area
        
        aust = 0
        for f in face_list:
            area_weighted = round(f.attributes['area']/ttl_area,2)
            area_temp = round(area_weighted * f.attributes['temperature'],2)
            aust += area_temp
            geomie3d.modify.update_topo_att(f, {'area_weighted_aust':area_weighted, 'area_temperature_aust': area_temp})
        return round(aust,2)

    def calc_avg_srf_temp(face_list):
        ttl_area = 0
        for f in face_list:
            area = round(geomie3d.calculate.face_area(f),2)
            geomie3d.modify.update_topo_att(f, {'area':area})
            ttl_area += area
        
        avg = 0
        for f in face_list:
            area_weighted = round(f.attributes['area']/ttl_area,2)
            area_temp = round(area_weighted * f.attributes['temperature'],2)
            avg += area_temp
            geomie3d.modify.update_topo_att(f, {'area_weighted':area_weighted, 'area_temperature': area_temp})
        return round(avg,2)
        
    #%% Main
    file_exts = sorted(os.listdir(geomie3d_dir))
    rows2d = [['datetime', 'aust', 'avg_srf_temp']]
    for fe in file_exts:
        fe_split = fe.split('.')
        ext = fe_split[-1]
        if ext.lower() == 'geomie3d':
            filename_only = fe_split[0]
            fn_split = filename_only.split('_')
            date_str = fn_split[1]#.replace()
            time_str = fn_split[2].replace('-', ':')
            datetime_str = date_str + ' ' + time_str + '-04:00'
            dt = dateutil.parser.parse(datetime_str)
            dt_str = dt.isoformat()

            geomie3d_path = os.path.join(geomie3d_dir, fe)
            model_dict = geomie3d.utility.read_geomie3d(geomie3d_path)
            topo_list = model_dict['topo_list']
            if is_215:
                non_pnl_srfs = topo_list[0:6]
            else:
                non_pnl_srfs = topo_list[0:5]
                
            aust = calc_aust(non_pnl_srfs)
          
            avg = calc_avg_srf_temp(topo_list)
            rows2d.append([dt_str, aust, avg])
            geomie3d.utility.write2geomie3d(topo_list, geomie3d_path)
            # geomie3d.viz.viz([{'topo_list': non_pnl_srfs , 'colour': 'blue', 'attribute': 'name'}])

    geomie3d.utility.write2csv(rows2d, aust_path)
    ```
### Calculate radiant panels load 
    ```
    import pandas as pd 

    import geomie3d
    #%% Parameters
    iot_path = '/home/usrname/iot.csv'
    mini_path = '/home/usrname/minires.csv'
    aust_path = '/home/usrname/aust.csv'
    geomie3d_path = '/home/usrname/room.geomie3d'
    load_path = '/home/usrname/load.csv'

    #%% Functions
    def read_csv(path):
        df = pd.read_csv(path, sep = ',')
        df['datetime'] = pd.to_datetime(df['datetime'], yearfirst=True)
        df.set_index('datetime', inplace=True)
        return df
        
    #%% Main
    # read iot
    iot_df = read_csv(iot_path)
    iot_df = iot_df.resample('5T').median()
    # read mini
    mini_df = read_csv(mini_path)
    mini_df = mini_df.resample('5T').median()
    # read aust
    aust_df = read_csv(aust_path)
    aust_df = aust_df.resample('5T').median()
    # read model
    model_dict = geomie3d.utility.read_geomie3d(geomie3d_path)
    topo_list = model_dict['topo_list']
    pnl_srf = topo_list[-1]
    ceil_area = geomie3d.calculate.face_area(pnl_srf)
    pnl_area = ceil_area*0.8
    # calculate the rad loads
    iot_df['pnl_temp'] = (iot_df['a006_panel_temp'] + iot_df['arg001_panel_temp'])/2
    aust_df['pnl_temp'] = iot_df['pnl_temp']
    aust_df['qrad'] = 5e-08 * (((iot_df['pnl_temp'] + 273.15)**4) - ((aust_df['aust'] + 273.15)**4)) 
    aust_df['qrad_area'] = aust_df['qrad'] * pnl_area
    aust_df = aust_df.drop(['avg_srf_temp'], axis=1)
    # calculate the cnv loads
    aust_df['pnl_air_diff'] = aust_df['pnl_temp'] - mini_df[' TEMP']
    aust_df['qconv'] = 2.13 * (aust_df['pnl_air_diff'].abs()) ** 0.31 * aust_df['pnl_air_diff'] 
    aust_df['qconv_area'] = aust_df['qconv'] * pnl_area

    aust_df.to_csv(load_path)
    ```