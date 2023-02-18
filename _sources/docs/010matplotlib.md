# Matplotlib Scripts

## Single-axis time-series visualization

```
def viz1axis_timeseries(data_dict_ls, plot_title, xaxis_label, yaxis_label, filepath,
                        yaxis_lim = None, dateformat = None, legend_loc = None, tight_layout = True):
    """
    Viz timeseries data in a 1 axis x-y plot
    
    Parameters
    ----------
    data_dict_ls : list of dictionary
        dictionary with the following keys.
        'datax', list of timestamps of the data
        'datay', list of y-values of the data
        'linestyle', tuple of str, 'solid', 'dotted', 'dashed', 'dashdot', (0, (1, 5, 5, 5))
        'linewidth', float, width of the line
        'marker', str, empty string for no markers '',the markers viz of the data set, https://matplotlib.org/stable/api/markers_api.html
        'marker_size', int, the size of the marker
        'color', str, color of the data set viz, https://matplotlib.org/stable/tutorials/colors/colors.html
        'label', str the label of the data set in the legend
    
    plot_title : str
        tite of the plot
    
    xaxis_label : str
        label of the xaxis
    
    yaxis_label : str
        label of the yaxis
        
    filepath : str
        filepath to save the generated graph.
    
    yaxis_lim : tuple, optional
        tuple specifying the lower and upper limit of the yaxis (lwr, uppr)
    
    dateformat : str, optional
        specify how to viz the date time on the x-axis, e.g.'%Y-%m-%dT%H:%M:%S'. Default 'H:%M:%S' 
    
    legend_loc : dictionary, optional
        specify the location of the legend
        'loc', str, e.g. upper right, center left, lower center
        'bbox_to_anchor',tuple, (x, y)
        'ncol', int, number of columns for the legend box
        
    tight_layout : bool, optional
        turn on tight layout. default True
    """
    fig, ax1 = plt.subplots()
    plt.title(plot_title, fontsize=10 )
    ax1.set_xlabel(xaxis_label, fontsize=10)
    ax1.set_ylabel(yaxis_label, fontsize=10)
    if yaxis_lim is not None:
        plt.ylim(yaxis_lim[0],yaxis_lim[1])
    
    for dd in data_dict_ls:
        ax1.plot(list(dd['datax']), list(dd['datay']),
                  linestyle=dd['linestyle'], linewidth=dd['linewidth'], marker = dd['marker'], 
                  markersize = dd['marker_size'], 
                  c = dd['color'], label = dd['label'])
    
    if dateformat is None:
        dateformat = '%H:%M:%S'
        
    formatter = DateFormatter(dateformat)
    ax1.xaxis.set_major_formatter(formatter)
    plt.gcf().autofmt_xdate()
    for label in ax1.get_xticklabels():
        label.set_rotation(40)
    
    if legend_loc is None:
        ax1.legend(loc='best')
    else:
        ax1.legend(loc=legend_loc['loc'], 
                   bbox_to_anchor=legend_loc['bbox_to_anchor'], 
                   fancybox=True, ncol=legend_loc['ncol'])
    
    ax1.grid(True, axis = 'y', linestyle='--', linewidth = 0.3)
    ax1.grid(True, axis = 'x', linestyle='--', linewidth = 0.3)
    
    if tight_layout==True:
        plt.tight_layout()
        
    plt.savefig(filepath, bbox_inches = "tight", dpi = 300, transparent=False)
    plt.show()

```
Example
```
from dateutil.parser import parse
import matplotlib.pyplot as plt
from matplotlib.dates import DateFormatter

dates = [ parse('2022-11-28T09:00:00'), parse('2022-11-28T10:00:00'), parse('2022-11-28T11:00:00'),
          parse('2022-11-28T12:00:00')]

data_dict_ls = [{ 'datax': dates,
                  'datay': [10, 11, 12, 10],
                  'linestyle':'solid',
                  'linewidth':1,
                  'marker': '.',
                  'marker_size':3, 'color': 'b', 'label': 'some data1'},
                { 'datax': dates,
                  'datay': [1,2,3,4],
                  'linestyle':'solid',
                  'linewidth':1,
                  'marker': '^',
                  'marker_size':3, 'color': 'k', 'label': 'some data2'}]

plot_title = 'some data'
xaxis_label = 'Time'
yaxis_label = 'unitx'
filepath = 'some/filepath'
viz1axis_timeseries(data_dict_ls, plot_title, xaxis_label, yaxis_label, filepath,
                    yaxis_lim = None, dateformat = None)
```

## Double-axis time-series visualization

```
def viz2axis_timeseries(y1_data_dict_ls, y2_data_dict_ls, plot_title, xaxis_label, yaxis1_label,
                        yaxis2_label, filepath, yaxis1_lim = None, yaxis2_lim = None, yaxis2_color = 'b',
                        dateformat = None, legend_loc1 = None, legend_loc2 = None, tight_layout = True):
    """
    Viz timeseries data in a 1 axis x-y plot

    Parameters
    ----------
    y1_data_dict_ls : list of dictionary
        dictionary with the following keys.
        'datax', list of timestamps of the data
        'datay', list of y-values of the data
        'linestyle', tuple of str, 'solid', 'dotted', 'dashed', 'dashdot', (0, (1, 5, 5, 5))
        'linewidth', float, width of the line
        'marker', str, '' for no markers, the markers viz of the data set, https://matplotlib.org/stable/api/markers_api.html
        'marker_size', int, the size of the marker
        'color', str, color of the data set viz, https://matplotlib.org/stable/tutorials/colors/colors.html
        'label', str the label of the data set in the legend

    y2_data_dict_ls : list of dictionary
        dictionary with the same keys as y1_data_dict_ls. Data here will be viz on y2 axis.

    plot_title : str
        tite of the plot

    xaxis_label : str
        label of the xaxis

    yaxis1_label : str
        label of the yaxis1

    yaxis2_label : str
        label of the yaxis2

    filepath : str
        filepath to save the generated graph.

    yaxis1_lim : tuple, optional
        tuple specifying the lower and upper limit of the yaxis1 (lwr, uppr)

    yaxis2_lim : tuple, optional
        tuple specifying the lower and upper limit of the yaxis2 (lwr, uppr)
        
    yaxis2_color : str, optional
        str specifying the color of the second yaxis, default is 'b' blue

    dateformat : str, optional
        specify how to viz the date time on the x-axis, e.g.'%Y-%m-%dT%H:%M:%S'. Default 'H:%M:%S'

    legend_loc1 : dictionary, optional
        specify the location of the legend of the first y-axis
        'loc', str, e.g. upper right, center left, lower center
        'bbox_to_anchor',tuple, (x, y)
        'ncol', int, number of columns for the legend box

    legend_loc2 : dictionary, optional
        specify the location of the legend of the 2nd y-axis
        'loc', str, e.g. upper right, center left, lower center
        'bbox_to_anchor',tuple, (x, y)
        'ncol', int, number of columns for the legend box

    tight_layout : bool, optional
        turn on tight layout. default True

    """
    fig, ax1 = plt.subplots()
    plt.title(plot_title, fontsize=10 )
    ax1.set_xlabel(xaxis_label, fontsize=10)
    ax1.set_ylabel(yaxis1_label, fontsize=10)
    if yaxis1_lim is not None:
        plt.ylim(yaxis1_lim[0],yaxis1_lim[1])

    for dd in y1_data_dict_ls:
        ax1.plot(list(dd['datax']), list(dd['datay']),
                  marker = dd['marker'], markersize = dd['marker_size'],
                  linestyle = dd['linestyle'], linewidth=dd['linewidth'],
                  c = dd['color'], label = dd['label'])

    if dateformat is None:
        dateformat = '%H:%M:%S'

    formatter = DateFormatter(dateformat)
    ax1.xaxis.set_major_formatter(formatter)
    plt.gcf().autofmt_xdate()
    for label in ax1.get_xticklabels():
        label.set_rotation(40)

    if legend_loc1 is None:
        ax1.legend(loc='best')
    else:
        ax1.legend(loc=legend_loc1['loc'],
                   bbox_to_anchor=legend_loc1['bbox_to_anchor'],
                   fancybox=True, ncol=legend_loc1['ncol'])

    ax1.grid(True, axis = 'y', linestyle='--', linewidth = 0.3)
    ax1.grid(True, axis = 'x', linestyle='--', linewidth = 0.3)

    #=================================================================================================================================
    #THE SECOND AXIS
    #=================================================================================================================================
    ax2 = ax1.twinx()

    for dd in y2_data_dict_ls:
        ax2.plot(list(dd['datax']), list(dd['datay']),
                  linestyle = dd['linestyle'], linewidth=dd['linewidth'],
                  marker = dd['marker'], markersize = dd['marker_size'],
                  c = dd['color'], label = dd['label'])

    ax2.set_ylabel(yaxis2_label, color='b', fontsize=10)
    ax2.tick_params('y', colors = yaxis2_color)

    if legend_loc2 is None:
        ax2.legend(loc='best')
    else:
        ax2.legend(loc=legend_loc2['loc'],
                   bbox_to_anchor=legend_loc2['bbox_to_anchor'],
                   fancybox=True, ncol=legend_loc2['ncol'])

    if tight_layout == True:
      plt.tight_layout()

    plt.savefig(filepath, bbox_inches = "tight", dpi = 300, transparent=False)
    plt.show()
```
Example
```
from dateutil.parser import parse
import matplotlib.pyplot as plt
from matplotlib.dates import DateFormatter

dates = [ parse('2022-11-28T09:00:00'), parse('2022-11-28T10:00:00'), parse('2022-11-28T11:00:00'),
          parse('2022-11-28T12:00:00')]

y1data_dict_ls = [{ 'datax': dates,
                  'datay': [10, 11, 12, 10],
                  'linestyle':'solid',
                  'linewidth':1,
                  'marker': '.',
                  'marker_size':3, 'color': 'k', 'label': 'some data1'}]

y2data_dict_ls = [{ 'datax': dates,
                  'datay': [2, 4, 14, 8],
                  'linestyle':'solid',
                  'linewidth':1,
                  'marker': '.',
                  'marker_size':3, 'color': 'b', 'label': 'some data2'}]

plot_title = 'some data'
xaxis_label = 'Time'
yaxis1_label = 'unitx'
yaxis2_label = 'unitx2'
filepath = 'some/filepath'
viz2axis_timeseries(y1data_dict_ls, y2data_dict_ls, plot_title, xaxis_label, yaxis1_label, yaxis2_label, filepath,
                    yaxis1_lim = None, yaxis2_lim = None, dateformat = None)
```
