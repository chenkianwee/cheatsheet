# Useful code snippets

## Python subprocess
```
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

## Python datetime
```
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

## Python Pathlib
```
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
```
from itertools import chain
xyz_2dlist = [[[0,0,0], [1,1,1]], [[2,2,2]]]
flat = list(chain.from_iterable(xyz_2dlist))
```

## Python parallel computing
```
import multiprocessing as mp
def addition(x: float, y: float ) -> float:
    z = x + y
    return z

pool = mp.Pool(mp.cpu_count())
results = pool.starmap(addition, [[1.5, 2.2], [3.1, 4.2], [5.6, 6.2]])
pool.close()
print(results)
```

## Numpy create empty nan array
```
import numpy as np
a = np.empty((3,3,))
a.fill(np.nan)
```

## OpenStudio Python
```
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
```
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
```
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