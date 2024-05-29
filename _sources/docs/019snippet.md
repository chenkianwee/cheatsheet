# Useful code snippets

## Python subprocess
```{dropdown} code snippet
    import sys
    import subprocess

    pyexe = sys.executable
    pyscript = os.path.join(self.current_path, "gcw_answer.py" )
    call_list = [pyexe, pyscript, parm1, parm2, parm3] 
    res = subprocess.run(call_list, capture_output=True, text=True)
    print(res.stdout)
    print(res.stderr)
    res = subprocess.call(call_list)
```

## Python argparse
```{dropdown} code snippet
    def parse_args():
        # create parser object
        parser = argparse.ArgumentParser(description = "Project & Caculate MRT of your Chaosense Data")
    
        # defining arguments for parser object
        # a file path
        parser.add_argument('-s', '--scan', type = str,
                            metavar = 'filepath', default = None,
                            help = 'The ply file to process')

        # when nargs is added you will get a list
        parser.add_argument('-x', '--xyz', type = float, nargs = 3,
                            metavar = ('posX', 'posY', 'posZ'), default = None,
                            help = "The position of the sensor")
        
        parser.add_argument('-z', '--voxel_size', type = float, nargs = 1,
                            metavar = 'voxel size', default = None,
                            help = "The size of the voxel")
        
        parser.add_argument('-g', '--grid', type = float, nargs = 4,
                            metavar = ('Length(m)','Width(m)','Height(m)', 'Buffer(m)'), 
                            help = 'Defining the grid with these dimensions')
        
        # boolean flag
        parser.add_argument('-v', '--viz', action = 'store_true', default=False,
                            help = 'visualize the calculation procedure if turned on')
        
        # parse the arguments from standard input
        args = parser.parse_args()
        return args

    def main():
        args = parse_args()
        scan_path = args.scan
        sensor_pos = args.xyz
        rm_dim = args.space_dim
        voxel_size = args.voxel_size[0]
        grid_dim = args.grid
        viz = args.viz
```

## Python datetime
```{dropdown} code snippet
    import datetime
    from dateutil.parser import parse
    import pytz

    print(pytz.common_timezones)
    print(pytz.all_timezones)
    datestr = '2024-03-20T03:25:00+00:00'
    dt = parse(datestr, yearfirst=True)
    # converts the datetime to new tz
    dt_tz = dt.astimezone(pytz.timezone('America/New_York'))
    dt_str = dt_tz.isoformat()
    print(dt_str)
    # replace the datetime tz
    dt_utc = dt.replace(tzinfo=pytz.utc)
    dt_str = dt_utc.isoformat(timespec='minutes')
print(dt_str)
```

```{dropdown} gen days function
    def gen_days(start_yr: int, start_mth: int, start_day:int, end_yr: int, end_mth: int, end_day:int):
        start_date = datetime(start_yr, start_mth, start_day)
        end_date = datetime(end_yr, end_mth, end_day)
        delta = timedelta(days=1)

        date_list = []
        current_date = start_date
        while current_date <= end_date:
            pd_datetime = pd.to_datetime(current_date)
            date_list.append(pd_datetime)
            current_date += delta
        return date_list
```

## Python Pathlib
```{dropdown} code snippet
    from pathlib import Path

    join_path = Path('/home/chenkianwee').joinpath('this','is','a','path')
    parent = Path(__file__).parent
    level2up = Path(__file__).parent.parent
    filename = Path(__file__).name
    cwd = Path.cwd()
    print(cwd)
    print(join_path)
    # to turn the path into str you can either to str() or .__str__()
    print(str(parent))
    print(filename.__str__())
```

## Python Chain to flatten one dimension of a list
```{dropdown} code snippet
    from itertools import chain
    xyz_2dlist = [[[0,0,0], [1,1,1]], [[2,2,2]]]
    flat = list(chain.from_iterable(xyz_2dlist))
```

## Python parallel computing
```{dropdown} code snippet
    import multiprocessing as mp
    def addition(x: float, y: float ) -> float:
        z = x + y
        return z

    pool = mp.Pool(mp.cpu_count())
    results = pool.starmap(addition, [[1.5, 2.2], [3.1, 4.2], [5.6, 6.2]])
    pool.close()
    print(results)
```

## Python xml.etree.ElementTree
```{dropdown} code snippet
    import xml.etree.ElementTree as ET

    measure_xmlpath = str(Path(measure_dir_orig).joinpath('measure.xml'))
    tree = ET.parse(measure_xmlpath)
    root = tree.getroot()
    for child in root:
        child_name = child.tag
        if child_name == 'attributes':
            for child2 in child:
                name = child2.find('name').text
```

## Numpy create empty nan array
```{dropdown} code snippet
    import numpy as np
    a = np.empty((3,3,))
    a.fill(np.nan)
```

## Pandas 
```{dropdown} read csv with no header and resample
    import pandas as pd
    from pathlib import Path

    csv_path = Path(__file__).parent.parent.parent.joinpath('data', 'Alex_Thesis_Data', 'Panel_Data', 'Run2And.csv')
    df = pd.read_csv(csv_path, header=None)
    print(df.iloc[:, 0])
    # df.iloc[:, 0] = pd.to_datetime(df.iloc[:, 0])
    df[0] = pd.to_datetime(df[0])
    df.set_index(0, inplace=True)
    df = df.resample('5min').median()
```

```{dropdown} read csv with header and resample
    import pandas as pd 

    df = pd.read_csv(path, sep = ',')
    df['datetime'] = pd.to_datetime(df['datetime'], yearfirst=True)
    df.set_index('datetime', inplace=True)
    df = df.resample('5min').median()
```

## OpenStudio Python
```{dropdown} code snippet
    import openstudio
    from openstudio import model as osmod

    m = osmod.Model()
    mat = osmod.StandardOpaqueMaterial(m)
    mat.setThickness(0.3)
    print(mat)

    epw_path = '/home/chenkianwee/kianwee_work/get/projects/grundfos/model/epw/SGP_SG_Changi.Intl.AP.486980_TMYx.2007-2021.epw'
    epwfile = openstudio.openstudioutilitiesfiletypes.EpwFile(epw_path)

    m = osmod.exampleModel()
    oswf = m.getWeatherFile()
    oswf.setWeatherFile(m, epwfile)

    things = osmod.getSurfaces(m)
    for thing in things:
        print('construction', thing.isConstructionDefaulted())
        print('ufactor', thing.uFactor())
        print('type', thing.surfaceType())
        
        vs = thing.vertices()
        for v in vs:
            print(type(v))
            print(v.x(), v.y(), v.z())
        
    things = osmod.getBuildingStorys(m)
    for thing in things:
        print(thing)
        
    ft = openstudio.energyplus.ForwardTranslator()
    idf = ft.translateModel(m)
    path = '/home/chenkianwee/kianwee_work/out.idf'
    idf.save(path, True)

    path = '/home/chenkianwee/kianwee_work/test.osm'
    m.save(path, True)
```

## IFCopenShell Python
```{dropdown} code snippet
    import ifcopenshell
    import ifcopenshell.geom
    import numpy as np
    from pathlib import Path

    ifc_path = Path(__file__).parent.parent
    ifc_path = ifc_path.joinpath('ifc', 'arch_eg-Building.ifc')
    # ifc_path = '/home/chenkianwee/kianwee_work/get/projects/grundfos/model/ifc/grundfos-Building.ifc'
    print(ifc_path)
    model = ifcopenshell.open(ifc_path)

    print(model.schema) # May return IFC2X3, IFC4, or IFC4X3.
    print(type(model.by_id(1)))

    walls = model.by_type('IfcWall')
    mats = model.by_type('IfcSurfaceStyle')
    print(mats)
    for w in walls:
        print(w)
        print(w.get_info())
        psets = ifcopenshell.util.element.get_psets(w)
        print(psets)

        container = ifcopenshell.util.element.get_container(w)
        print(container)

        mat = ifcopenshell.util.element.get_materials(w)
        print(mat)

        settings = ifcopenshell.geom.settings()
        shape = ifcopenshell.geom.create_shape(settings, w)
        verts = shape.geometry.verts # X Y Z of vertices in flattened list e.g. [v1x, v1y, v1z, v2x, v2y, v2z, ...]
        faces = shape.geometry.faces # Indices of vertices per triangle face e.g. [f1v1, f1v2, f1v3, f2v1, f2v2, f2v3, ...]
        edges = shape.geometry.edges # closer to the original geometry
        # print(len(verts))
        verts3 = np.reshape(verts, (int(len(verts)/3), 3))
        att_ls = []
        for vc,v in enumerate(verts3):
            att_ls.append({'id':vc})

        edge_pts = np.reshape(edges, [int(len(edges)/2),2])
```

## Ladybug Tool Python
```{dropdown} code snippet
    from ladybug.sql import SQLiteResult
    from pathlib import Path

    # eplusout_sql_path = Path(__file__).parent.joinpath('ifc2osm_eg_result', 'ifc2osm_eg_result_wrkflw', 'run', 'eplusout.sql')
    eplusout_sql_path = Path('/home/chenkianwee/kianwee_work/get/projects/grundfos/model/osmod/grundfos/grundfos_wrkflw/run/eplusout.sql')
    sql_obj = SQLiteResult(eplusout_sql_path.__str__())
    avail_output_names = sql_obj.available_outputs
    run_indxs = sql_obj.run_period_indices
    print(avail_output_names)
    # print(run_indxs)
    data = sql_obj.data_collections_by_output_name_run_period('Zone Air Temperature', 3)
    for d in data:
        print(d)
    print(len(data))
    print(data[1].header.analysis_period)
    print(data[1].header.metadata)
    # data_collect = sql_obj.data_collections_by_output_name('Zone Air Relative Humidity')
    # print(data_collect)
    # zone_air_year = data_collect[2]
    # dts = zone_air_year.datetimes
    # print(dts[10].isoformat())
    # for a_zair in zone_air_year:
    #     print(a_zair)
```

