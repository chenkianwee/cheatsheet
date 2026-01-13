# Useful code snippets

## Python dictionary
```{dropdown} code snippet
    # move a key to the front 
    chosen_pset = {'Name': chosen_pset.pop('Name'), **chosen_pset}
    # move a key to the back
    chosen_pset['Name] = chosen_pset.pop('Name')
```

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
```{dropdown} timezone with datetime.timezone
    import datetime
    utc_tz = datetime.timezone(datetime.timedelta(hours=0))
    tz = datetime.timezone(datetime.timedelta(hours=8))
    pydt = datetime.datetime.now()
    dt_utc = pydt.replace(tzinfo=utc_tz)
    dt_spore = dt_utc.astimezone(tz)
    dtstr = dt_spore.isoformat()
```


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
    
    # get all the file in the dirctory
    files = Path(json_dir).glob('*.json')

    # get all file and folder
    p = Path(r'C:\Users\akrio\Desktop\Test').glob('**/*')
    files = [x for x in p if x.is_file()]

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

## Python json 
```{dropdown} code snippet
    with open(wrkflw_path) as wrkflw_f:
        data = json.load(wrkflw_f)
        steps = data['steps']
        for step in steps:
            dirname = step['measure_dir_name']

    with open(wrkflw_path, "w") as out_file:
        json.dump(data, out_file)

    # pretty json
    pretty_json_data = json.dumps(mat_json, indent=4)
    with open(csv_path, 'w') as f:
        f.write(pretty_json_data)
```
```{dropdown} validate jsonschema
    import json
    from jsonschema import validate

    path = 'schema.json'
    with open(path) as f:
        data = json.load(f)

    path = 'instance.json'
    with open(path) as f:
        data2val = json.load(f)

    is_valid = validate(instance=data2val, schema=data)
    print(is_valid)
    # if it is valid will not raise any error and give a None
```

## Numpy create empty nan array
```{dropdown} code snippet
    import numpy as np
    a = np.empty((3,3,))
    a.fill(np.nan)
```

## Numpy indexing 
```{dropdown} code snippet
    x = np.array([[0,0,1],
                 [1,1,2]])
    # get the first 2 columns of the array
    print(x[:, 1:3])
```

```{dropdown} code snippet
    x = [[
            [0,0,1],
            [1,1,2]
        ],
        [
            [1,1,0],
            [2,2,2] 
        ]]

    x = np.array(x)
    # get the first of each set and last 2 columns of each point
    print(x[:, 0:1, 1:3])
```

## Numpy vectorize double for loop
```{dropdown} code snippet
    xyzs = [[0,0,1], [0,0,2], [0,0,3]]
    polyxyzs = [[[1,1,1], [2,2,2], [3,3,3]], [[4,4,4], [5,5,5], [6,6,6]]]

    xyzs_rep = np.repeat(xyzs, repeats=len(polyxyzs), axis=0)
    print(xyzs_rep)

    polyxyzs_rep = np.tile(polyxyzs, (len(xyzs), 1, 1))
    print(polyxyzs_rep)

    xyzs1 = [[0,0,1], [0,0,2], [0,0,3]]
    xyzs2 = [[1,1,1], [2,2,2], [3,3,3]]

    xyzs1_rep = np.repeat(xyzs1, repeats=len(xyzs2), axis=0)
    print(xyzs1_rep)

    xyzs2_rep = np.tile(xyzs2, (len(xyzs1), 1))
    print(xyzs2_rep)
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

    os_model = osmod.Model.load(osmod_path).get() # read from existing file

    m = osmod.Model() # create new file
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
```{dropdown} code snippet
    # setup time
    time9 = openstudio.openstudioutilitiestime.Time(0,9,0,0)
    time17 = openstudio.openstudioutilitiestime.Time(0,17,0,0)
    time24 = openstudio.openstudioutilitiestime.Time(0,24,0,0)
    # region:setup schedule type limits
    sch_type_lim_frac = osmod.ScheduleTypeLimits(osmodel)
    sch_type_lim_frac.setName('fractional')
    sch_type_lim_frac.setLowerLimitValue(0.0)
    sch_type_lim_frac.setUpperLimitValue(1.0)
    sch_type_lim_frac.setNumericType('Continuous')

    sch_type_lim_temp = osmod.ScheduleTypeLimits(osmodel)
    sch_type_lim_temp.setName('temperature')
    sch_type_lim_temp.setLowerLimitValue(-60)
    sch_type_lim_temp.setUpperLimitValue(200)
    sch_type_lim_temp.setNumericType('Continuous')
    sch_type_lim_temp.setUnitType('Temperature')

    sch_type_lim_act = osmod.ScheduleTypeLimits(osmodel)
    sch_type_lim_act.setName('activity')
    sch_type_lim_act.setLowerLimitValue(0)
    sch_type_lim_act.setNumericType('Continuous')
    sch_type_lim_act.setUnitType('ActivityLevel')
    # endregion:setup schedule type limits
    # region: setup occ schedule
    sch_day_occ = osmod.ScheduleDay(osmodel)
    sch_day_occ.setName('weekday occupancy')
    sch_day_occ.setScheduleTypeLimits(sch_type_lim_frac)
    sch_day_occ.addValue(time9, 0.0)
    sch_day_occ.addValue(time17, 1.0)

    sch_ruleset_occ = osmod.ScheduleRuleset(osmodel)
    sch_ruleset_occ.setName('occupancy schedule')
    sch_ruleset_occ.setScheduleTypeLimits(sch_type_lim_frac)

    sch_rule_occ = osmod.ScheduleRule(sch_ruleset_occ, sch_day_occ)
    sch_rule_occ.setName('occupancy weekdays')
    sch_rule_occ.setApplyWeekdays(True)
    # endregion: setup occ schedule
    # region: setup activity schedule
    sch_day_act = osmod.ScheduleDay(osmodel)
    sch_day_act.setName('weekday activity')
    sch_day_act.setScheduleTypeLimits(sch_type_lim_act)
    sch_day_act.addValue(time24, 70)

    sch_ruleset_act = osmod.ScheduleRuleset(osmodel)
    sch_ruleset_act.setName('activity schedule')
    sch_ruleset_act.setScheduleTypeLimits(sch_type_lim_act)
    sch_ruleset_act.setSummerDesignDaySchedule(sch_day_act)
    sch_ruleset_act.setWinterDesignDaySchedule(sch_day_act)
    sch_ruleset_act.setHolidaySchedule(sch_day_act)
    sch_ruleset_act.setCustomDay1Schedule(sch_day_act)
    sch_ruleset_act.setCustomDay2Schedule(sch_day_act)

    sch_rule_act = osmod.ScheduleRule(sch_ruleset_act, sch_day_act)
    sch_rule_act.setName('activity weekdays')
    sch_rule_act.setApplyAllDays(True)
    # endregion: setup activity schedule
    # region: setup thermostat cooling setpoint
    sch_day_cool_tstat = osmod.ScheduleDay(osmodel)
    sch_day_cool_tstat.setName('thermostat cooling weekday schedule')
    sch_day_cool_tstat.setScheduleTypeLimits(sch_type_lim_temp)
    sch_day_cool_tstat.addValue(time9, 60.0)
    sch_day_cool_tstat.addValue(time17, 25.0)
    sch_day_cool_tstat.addValue(time24, 60.0)

    sch_day_cool_tstat2 = osmod.ScheduleDay(osmodel)
    sch_day_cool_tstat2.setName('thermostat cooling weekends schedule')
    sch_day_cool_tstat2.setScheduleTypeLimits(sch_type_lim_temp)
    sch_day_cool_tstat2.addValue(time24, 60.0)

    sch_day_cool_tstat3 = osmod.ScheduleDay(osmodel)
    sch_day_cool_tstat3.setName('thermostat cooling design day schedule')
    sch_day_cool_tstat3.setScheduleTypeLimits(sch_type_lim_temp)
    sch_day_cool_tstat3.addValue(time9, 25.0)
    sch_day_cool_tstat3.addValue(time17, 25.0)
    sch_day_cool_tstat3.addValue(time24, 25.0)

    sch_ruleset_cool_tstat = osmod.ScheduleRuleset(osmodel)
    sch_ruleset_cool_tstat.setName('thermostat cooling ruleset')
    sch_ruleset_cool_tstat.setScheduleTypeLimits(sch_type_lim_temp)
    sch_ruleset_cool_tstat.setSummerDesignDaySchedule(sch_day_cool_tstat3)

    sch_rule_cool_tstat = osmod.ScheduleRule(sch_ruleset_cool_tstat, sch_day_cool_tstat)
    sch_rule_cool_tstat.setName('thermostat cooling weekday rule')
    sch_rule_cool_tstat.setApplyWeekdays(True)

    sch_rule_cool_tstat = osmod.ScheduleRule(sch_ruleset_cool_tstat, sch_day_cool_tstat2)
    sch_rule_cool_tstat.setName('thermostat cooling weekend rule')
    sch_rule_cool_tstat.setApplyWeekends(True)
    # endregion: setup thermostat cooling setpoint
    # region: setup thermostat heating setpoint
    sch_day_hot_tstat = osmod.ScheduleDay(osmodel)
    sch_day_hot_tstat.setName('thermostat heating weekday schedule')
    sch_day_hot_tstat.setScheduleTypeLimits(sch_type_lim_temp)
    sch_day_hot_tstat.addValue(time9, 20.0)
    sch_day_hot_tstat.addValue(time17, 20.0)
    sch_day_hot_tstat.addValue(time24, 20.0)

    sch_ruleset_hot_tstat = osmod.ScheduleRuleset(osmodel)
    sch_ruleset_hot_tstat.setName('thermostat heating ruleset')
    sch_ruleset_hot_tstat.setScheduleTypeLimits(sch_type_lim_temp)
    sch_ruleset_cool_tstat.setWinterDesignDaySchedule(sch_day_hot_tstat)

    sch_rule_hot_tstat = osmod.ScheduleRule(sch_ruleset_hot_tstat, sch_day_hot_tstat)
    sch_rule_hot_tstat.setName('thermostat heating weekday rule')
    sch_rule_hot_tstat.setApplyAllDays(True)
    # endregion: setup thermostat heating setpoint
    tstat = osmod.ThermostatSetpointDualSetpoint(osmodel)
    tstat.setCoolingSetpointTemperatureSchedule(sch_ruleset_cool_tstat)
    tstat.setHeatingSetpointTemperatureSchedule(sch_ruleset_hot_tstat)
    # endregion: setup the schedules 
    
    # region: setup the thermalzones 
    #------------------------------------------------------------------------------------------------------
    # set the internal loads of the space
    # setup the lighting schedule
    light = openstudio_utils.setup_light_schedule(osmodel, sch_ruleset_occ)
    # setup electric equipment schedule
    elec_equip = openstudio_utils.setup_elec_equip_schedule(osmodel, sch_ruleset_occ)

    spaces = osmod.getSpaces(osmodel)
    # occ_numbers = [34, 71]
    occ_numbers = [25, 50]
    light_watts_m2 = 5
    elec_watts_m2 = 10
    thermal_zones =[]
    for cnt, space in enumerate(spaces):
        space_name = space.nameString()
        space.autocalculateFloorArea()
        outdoor_air = osmod.DesignSpecificationOutdoorAir(osmodel)
        outdoor_air.setOutdoorAirFlowperPerson(0.006) #m3/s
        space.setDesignSpecificationOutdoorAir(outdoor_air)
        # setup people schedule
        ppl = openstudio_utils.setup_ppl_schedule(osmodel, sch_ruleset_occ, sch_ruleset_act, name = space_name + '_people')
        space.setNumberOfPeople(occ_numbers[cnt], ppl)
        space.setLightingPowerPerFloorArea(light_watts_m2, light)
        space.setElectricEquipmentPowerPerFloorArea(elec_watts_m2, elec_equip)
        # setup the thermostat schedule
        thermalzone = space.thermalZone()
        if thermalzone.empty() == False:
            thermalzone_real = thermalzone.get()
            thermalzone_real.setThermostatSetpointDualSetpoint(tstat)
            thermal_zones.append(thermalzone_real)
    #------------------------------------------------------------------------------------------------------
    # endregion: setup the thermalzones 
    
    #------------------------------------------------------------------------------------------------------
    # endregion: setup openstudio model
    #------------------------------------------------------------------------------------------------------
    osmodel.save(osmod_path, True)
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

## Asyncio Python
- https://www.pythontutorial.net/python-concurrency/python-asyncio-create_task/
- https://www.slingacademy.com/article/python-using-async-await-with-loops/
- https://superfastpython.com/python-async-requests/

```{dropdown} code snippet
    import asyncio
    import time

    async def call_api(json_payload, cookies):
        url = 'https://example.com'
        r1 = await asyncio.to_thread(requests.post, url, json=json_payload, cookies=cookies)
        res_json = r1.json()
        r1.close()

    async def main():
        start = time.perf_counter()
        json_payload = {}
        cookies = {}
        task_1 = asyncio.create_task(
            call_api(json_payload, cookies)
        )

        task_2 = asyncio.create_task(
            call_api(json_payload, cookies)
        )

        tasks = [task1, task2]

        prices = await asyncio.gather(*tasks)

    asyncio.run(main())
```