# Uber-New-York-data-analysis-python

import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
uber_15=pd.read_csv(r'C:\Users\Admin\OneDrive\Desktop\data analysis\uber-pickups-in-new-york-city/uber-raw-data-janjune-15.csv',encoding='utf-8')

uber_15.head(2)

uber_15.shape

uber_15.duplicated().sum()

uber_15.drop_duplicates(inplace=True)


uber_15.shape

uber_15.dtypes

uber_15['Pickup_date']=pd.to_datetime(uber_15['Pickup_date'],format='%Y-%m-%d %H:%M:%S')


uber_15['Pickup_date'].dtype

uber_15['month']=uber_15['Pickup_date'].dt.month

uber_15['month'].value_counts().plot(kind='bar')

uber_15['weekday']=uber_15['Pickup_date'].dt.day_name()
uber_15['day']=uber_15['Pickup_date'].dt.day
uber_15['hour']=uber_15['Pickup_date'].dt.hour
uber_15['month']=uber_15['Pickup_date'].dt.month
uber_15['minute']=uber_15['Pickup_date'].dt.minute
uber_15.head(2)

uber_15.groupby(['month','weekday']).size()

temp=uber_15.groupby(['month','weekday'],as_index=False).size()
temp['month'].unique()


dict_month={1:'Jan', 2:'Feb', 3:'Mar', 4:'Apr', 5:'May', 6:'Jun'}
temp['month']=temp['month'].map(dict_month)
temp['month']

plt.figure(figsize=(12,8))
sns.barplot(x='month',y='size',hue='weekday',data=temp)



summary=uber_15.groupby(['weekday','hour'],as_index=False).size()

summary

plt.figure(figsize=(12,8))
sns.pointplot(x='hour',y='size',hue='weekday',data=summary)

uber_foil=pd.read_csv(r'C:\Users\Admin\OneDrive\Desktop\data analysis\uber-pickups-in-new-york-city/Uber-Jan-Feb-FOIL.csv')

uber_foil.head()

import chart_studio.plotly as py
import plotly.graph_objs as go
import plotly.express as px
from plotly.offline import download_plotlyjs, plot, iplot, init_notebook_mode
init_notebook_mode(connected=True)

px.box(x='dispatching_base_number',y='active_vehicles',data_frame=uber_foil)

import os
files=os.listdir(r'C:\Users\Admin\OneDrive\Desktop\data analysis\uber-pickups-in-new-york-city')[-7:]


files


files.remove('uber-raw-data-janjune-15.csv')

files

path=r'C:\Users\Admin\OneDrive\Desktop\data analysis\uber-pickups-in-new-york-city'

final=pd.DataFrame()
for file in files:
        current_df=pd.read_csv(path+'/'+file,encoding='utf-8')
        final=pd.concat([current_df,final])
    

final.shape

final.head(5)

final.duplicated().sum()

final.drop_duplicates(inplace=True)

final.shape

rush_uber=final.groupby(['Lat','Lon'],as_index=False).size()

rush_uber

import folium
basemap=folium.Map()

from folium.plugins import HeatMap
HeatMap(rush_uber).add_to(basemap)
basemap

final['Date/Time']=pd.to_datetime(final['Date/Time'],format='%m/%d/%Y %H:%M:%S')


final.head(4)

final['weekday']=final['Date/Time'].dt.day
final['hour']=final['Date/Time'].dt.hour

final.head(4)

pivot=final.groupby(['weekday','hour']).size().unstack()


pivot.style.background_gradient()



