# Pandas scripts
## Pandas script for processing data 
```
import pytz
import pandas as pd

mini_res_path = '/home/usr/RADICUBE.csv'
res_path = '/home/usr/res.csv'

# read the csv
df_mini = pd.read_csv(mini_res_path, sep = ',')

# convert the datetime string to datetime object
df_mini['datetime'] = df_mini['DD-MM-YYYY'] + 'T' + df_mini[' HH:MM:SS']
df_mini['datetime'] = pd.to_datetime(df_mini['datetime'], dayfirst=True, errors='coerce')
df_mini = df_mini.dropna(subset=['datetime'])

#correct the dates if required
df_mini['datetime'] = df_mini['datetime'] + pd.to_timedelta('2hour') # for mini res
df_mini['datetime'] = df_mini['datetime'].dt.tz_localize(pytz.timezone('America/New_York'))

# set index with the datetime column
df_mini.set_index('datetime', inplace=True)

# only extract the date of interest
start_time_str = ['19-07-2023 10:00:00', '19-07-2023 18:00:00']
se_time_dt = pd.to_datetime(start_time_str, dayfirst =True).tz_localize(pytz.timezone('America/New_York'))
df_mini = df_mini.loc[se_time_dt[0]:se_time_dt[1]]
# resample the timeseries into 1 min interval taking the median of that minute
df_mini = df_mini.resample('1T').median()

# extract the columns of interest
mrt = df_mini[' TOTAL MLX LW']
air_temp = df_mini[' TEMP']

# change the index to python datetime if required
dt1 = df_mini.index.to_pydatetime()

# write to csv
df_mini.to_csv(res_path)
```

## Combine the multiple dataframe in terms of columns
```
import os
import pandas as pd

iot_dir = 'dir/with/all/your/file/'
res_path = 'your_folder/result.csv'

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
## Viz data with matplotlib
```
import pytz
from dateutil import parser
import pandas as pd
import geomie3d 

#%% settings for 19-07-2023
mini_res_path = '/home/20230719_to_20230720_minires.csv'
high_res_path = '/home/20230719_to_20230720_highres.csv'
iot_path = '/home/20230719_to_20230720_iot.csv'
res_path = '/home/img/20230719.png'

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