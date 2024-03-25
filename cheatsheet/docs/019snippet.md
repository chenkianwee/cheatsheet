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