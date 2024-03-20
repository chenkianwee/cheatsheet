# Useful code snippets

- Python subprocess
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

- Python datetime
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