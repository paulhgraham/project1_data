
## Team Youza: Motivate bike data analysis

### import dependencies


```python
from reader import load_dataframe
import pandas as pd
import numpy as np
import os
import glob
from datetime import date, datetime
import time
%matplotlib inline
import matplotlib.pyplot as plt
import seaborn as sns
import requests
import math
import matplotlib.ticker as mtick
import matplotlib
matplotlib.style.use('ggplot')
import matplotlib.patches as mpatches
import folium
from folium.plugins import HeatMap

plot_colors=["#3498db","#9b59b6", "#95a5a6", "#e74c3c", "#34495e", "#2ecc71", '#ffce44']
```

### import data


```python
df = pd.read_csv('ny_sf_all_data_0703.csv', parse_dates=[1,2])
df.sample(15)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>bikeid</th>
      <th>starttime</th>
      <th>stoptime</th>
      <th>tripduration</th>
      <th>start station name</th>
      <th>start station latitude</th>
      <th>start station longitude</th>
      <th>end station name</th>
      <th>end station latitude</th>
      <th>end station longitude</th>
      <th>usertype</th>
      <th>birth year</th>
      <th>gender</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10511</th>
      <td>New York</td>
      <td>29586</td>
      <td>2018-01-27 16:31:54.000</td>
      <td>2018-01-27 16:34:27</td>
      <td>152</td>
      <td>Essex Light Rail</td>
      <td>40.712774</td>
      <td>-74.036486</td>
      <td>Exchange Place</td>
      <td>40.716247</td>
      <td>-74.033459</td>
      <td>Subscriber</td>
      <td>1969.0</td>
      <td>Male</td>
      <td>49.0</td>
    </tr>
    <tr>
      <th>212933</th>
      <td>San Francisco</td>
      <td>3499</td>
      <td>2018-03-30 19:27:56.546</td>
      <td>2018-03-30 19:38:01.8940</td>
      <td>605</td>
      <td>Spear St at Folsom St</td>
      <td>37.789677</td>
      <td>-122.390428</td>
      <td>4th St at Mission Bay Blvd S</td>
      <td>37.770407</td>
      <td>-122.391198</td>
      <td>Subscriber</td>
      <td>1973.0</td>
      <td>Male</td>
      <td>45.0</td>
    </tr>
    <tr>
      <th>457751</th>
      <td>San Francisco</td>
      <td>792</td>
      <td>2018-05-31 08:49:27.033</td>
      <td>2018-05-31 09:04:40.8180</td>
      <td>913</td>
      <td>Laguna St at Hayes St</td>
      <td>37.776435</td>
      <td>-122.426244</td>
      <td>Howard St at Beale St</td>
      <td>37.789756</td>
      <td>-122.394643</td>
      <td>Subscriber</td>
      <td>1981.0</td>
      <td>Male</td>
      <td>37.0</td>
    </tr>
    <tr>
      <th>457642</th>
      <td>San Francisco</td>
      <td>2472</td>
      <td>2018-05-31 09:05:17.288</td>
      <td>2018-05-31 09:10:49.1320</td>
      <td>331</td>
      <td>Shattuck Ave at 55th Ave</td>
      <td>37.840364</td>
      <td>-122.264488</td>
      <td>MacArthur BART Station</td>
      <td>37.828410</td>
      <td>-122.266315</td>
      <td>Subscriber</td>
      <td>1981.0</td>
      <td>Female</td>
      <td>37.0</td>
    </tr>
    <tr>
      <th>478000</th>
      <td>San Francisco</td>
      <td>1564</td>
      <td>2018-05-27 16:59:04.907</td>
      <td>2018-05-27 17:06:32.5690</td>
      <td>447</td>
      <td>San Francisco Public Library (Grove St at Hyde...</td>
      <td>37.778768</td>
      <td>-122.415929</td>
      <td>Laguna St at Hayes St</td>
      <td>37.776435</td>
      <td>-122.426244</td>
      <td>Subscriber</td>
      <td>1995.0</td>
      <td>Female</td>
      <td>23.0</td>
    </tr>
    <tr>
      <th>235608</th>
      <td>San Francisco</td>
      <td>281</td>
      <td>2018-03-26 14:20:02.253</td>
      <td>2018-03-26 14:24:26.4790</td>
      <td>264</td>
      <td>Santa Clara St at 7th St</td>
      <td>37.339146</td>
      <td>-121.884105</td>
      <td>Julian St at 6th St</td>
      <td>37.342997</td>
      <td>-121.888889</td>
      <td>Subscriber</td>
      <td>1991.0</td>
      <td>Male</td>
      <td>27.0</td>
    </tr>
    <tr>
      <th>416047</th>
      <td>San Francisco</td>
      <td>3300</td>
      <td>2018-04-10 09:35:00.778</td>
      <td>2018-04-10 09:47:49.0700</td>
      <td>768</td>
      <td>Berry St at 4th St</td>
      <td>37.775880</td>
      <td>-122.393170</td>
      <td>Davis St at Jackson St</td>
      <td>37.797280</td>
      <td>-122.398436</td>
      <td>Subscriber</td>
      <td>1985.0</td>
      <td>Male</td>
      <td>33.0</td>
    </tr>
    <tr>
      <th>338876</th>
      <td>San Francisco</td>
      <td>3893</td>
      <td>2018-04-27 09:01:43.926</td>
      <td>2018-04-27 09:10:09.4580</td>
      <td>505</td>
      <td>Beale St at Harrison St</td>
      <td>37.788059</td>
      <td>-122.391865</td>
      <td>Townsend St at 7th St</td>
      <td>37.771058</td>
      <td>-122.402717</td>
      <td>Subscriber</td>
      <td>1984.0</td>
      <td>Female</td>
      <td>34.0</td>
    </tr>
    <tr>
      <th>621325</th>
      <td>San Francisco</td>
      <td>11</td>
      <td>2018-05-02 14:32:41.549</td>
      <td>2018-05-02 15:03:50.8360</td>
      <td>1869</td>
      <td>West Oakland BART Station</td>
      <td>37.805318</td>
      <td>-122.294837</td>
      <td>West Oakland BART Station</td>
      <td>37.805318</td>
      <td>-122.294837</td>
      <td>Subscriber</td>
      <td>1976.0</td>
      <td>Male</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>24328</th>
      <td>New York</td>
      <td>29611</td>
      <td>2018-02-23 17:15:33.000</td>
      <td>2018-02-23 17:18:37</td>
      <td>184</td>
      <td>Grove St PATH</td>
      <td>40.719586</td>
      <td>-74.043117</td>
      <td>Dixon Mills</td>
      <td>40.721630</td>
      <td>-74.049968</td>
      <td>Subscriber</td>
      <td>1976.0</td>
      <td>Male</td>
      <td>42.0</td>
    </tr>
    <tr>
      <th>178704</th>
      <td>San Francisco</td>
      <td>2054</td>
      <td>2018-02-08 10:10:14.385</td>
      <td>2018-02-08 10:14:54.2220</td>
      <td>279</td>
      <td>Market St at Franklin St</td>
      <td>37.773793</td>
      <td>-122.421239</td>
      <td>Folsom St at 9th St</td>
      <td>37.773717</td>
      <td>-122.411647</td>
      <td>Subscriber</td>
      <td>1989.0</td>
      <td>Female</td>
      <td>29.0</td>
    </tr>
    <tr>
      <th>359870</th>
      <td>San Francisco</td>
      <td>3127</td>
      <td>2018-04-23 17:45:18.582</td>
      <td>2018-04-23 17:54:09.8710</td>
      <td>531</td>
      <td>2nd St at Folsom St</td>
      <td>37.785000</td>
      <td>-122.395936</td>
      <td>Powell St BART Station (Market St at 5th St)</td>
      <td>37.783899</td>
      <td>-122.408445</td>
      <td>Subscriber</td>
      <td>1963.0</td>
      <td>Male</td>
      <td>55.0</td>
    </tr>
    <tr>
      <th>523312</th>
      <td>San Francisco</td>
      <td>2876</td>
      <td>2018-05-19 09:17:40.846</td>
      <td>2018-05-19 09:35:45.1590</td>
      <td>1084</td>
      <td>Broadway at 30th St</td>
      <td>37.819381</td>
      <td>-122.261928</td>
      <td>MacArthur Blvd at Telegraph Ave</td>
      <td>37.826286</td>
      <td>-122.265100</td>
      <td>Subscriber</td>
      <td>1987.0</td>
      <td>Female</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>286851</th>
      <td>San Francisco</td>
      <td>3345</td>
      <td>2018-03-10 19:08:44.003</td>
      <td>2018-03-10 19:31:01.2530</td>
      <td>1337</td>
      <td>The Embarcadero at Bryant St</td>
      <td>37.787168</td>
      <td>-122.388098</td>
      <td>Mission Playground</td>
      <td>37.759210</td>
      <td>-122.421339</td>
      <td>Customer</td>
      <td>1987.0</td>
      <td>Male</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>488956</th>
      <td>San Francisco</td>
      <td>2050</td>
      <td>2018-05-25 09:01:47.463</td>
      <td>2018-05-25 09:08:09.8340</td>
      <td>382</td>
      <td>16th St Mission BART Station 2</td>
      <td>37.764765</td>
      <td>-122.420091</td>
      <td>20th St at Bryant St</td>
      <td>37.759200</td>
      <td>-122.409851</td>
      <td>Subscriber</td>
      <td>1970.0</td>
      <td>Male</td>
      <td>48.0</td>
    </tr>
  </tbody>
</table>
</div>



### review data


```python
print(df.shape)
print(df.dtypes)
print()
print(df.describe())
```

    (631373, 15)
    city                               object
    bikeid                             object
    starttime                  datetime64[ns]
    stoptime                           object
    tripduration                        int64
    start station name                 object
    start station latitude            float64
    start station longitude           float64
    end station name                   object
    end station latitude              float64
    end station longitude             float64
    usertype                           object
    birth year                        float64
    gender                             object
    age                               float64
    dtype: object
    
           tripduration  start station latitude  start station longitude  \
    count  6.313730e+05           631373.000000            631373.000000   
    mean   8.332047e+02               38.247734              -114.471648   
    std    3.752489e+03                1.096867                17.846863   
    min    6.100000e+01               37.312854              -122.444293   
    25%    3.140000e+02               37.773717              -122.408445   
    50%    5.080000e+02               37.787290              -122.394430   
    75%    8.180000e+02               37.827757              -122.258764   
    max    1.037896e+06               40.748716               -74.032108   
    
           end station latitude  end station longitude     birth year  \
    count         631373.000000          631373.000000  587914.000000   
    mean              38.247778            -114.471008    1981.639522   
    std                1.096648              17.846793      10.553349   
    min               37.312854            -122.444293    1887.000000   
    25%               37.774520            -122.406432    1976.000000   
    50%               37.787522            -122.394430    1984.000000   
    75%               37.824931            -122.258801    1989.000000   
    max               40.813358             -73.956461    2002.000000   
    
                     age  
    count  587914.000000  
    mean       36.360478  
    std        10.553349  
    min        16.000000  
    25%        29.000000  
    50%        34.000000  
    75%        42.000000  
    max       131.000000  
    

### clean data


```python
df.fillna('Other')
df['start hour'] = df['starttime'].apply(lambda x: x.hour + (round((x.minute/60)*4))/4) #rounds to quarter hour

#convert andsplit date/time
df['starttime'] = pd.to_datetime(df['starttime'])
df.dtypes['starttime']
#will display month
df['month'] = df['starttime'].dt.month
#will display hour
df['hour'] = df['starttime'].dt.hour
#will display name of the day
df['day_of_week'] = df['starttime'].dt.weekday_name

#age grouping
age_bins = [0, 20, 30, 40, 50, 60, 100]
age_groups = ['0-20', '21-30', '31-40', '41-50', '51-60', '61+']
df['age groups'] = pd.cut(df['age'], age_bins, labels=age_groups)

df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>bikeid</th>
      <th>starttime</th>
      <th>stoptime</th>
      <th>tripduration</th>
      <th>start station name</th>
      <th>start station latitude</th>
      <th>start station longitude</th>
      <th>end station name</th>
      <th>end station latitude</th>
      <th>end station longitude</th>
      <th>usertype</th>
      <th>birth year</th>
      <th>gender</th>
      <th>age</th>
      <th>start hour</th>
      <th>month</th>
      <th>hour</th>
      <th>day_of_week</th>
      <th>age groups</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>New York</td>
      <td>29590</td>
      <td>2018-01-01 00:01:46</td>
      <td>2018-01-01 00:03:58</td>
      <td>132</td>
      <td>Grove St PATH</td>
      <td>40.719586</td>
      <td>-74.043117</td>
      <td>Newark Ave</td>
      <td>40.721525</td>
      <td>-74.046305</td>
      <td>Subscriber</td>
      <td>1964.0</td>
      <td>Male</td>
      <td>54.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>0</td>
      <td>Monday</td>
      <td>51-60</td>
    </tr>
    <tr>
      <th>1</th>
      <td>New York</td>
      <td>29613</td>
      <td>2018-01-01 01:27:17</td>
      <td>2018-01-01 01:36:38</td>
      <td>560</td>
      <td>Marin Light Rail</td>
      <td>40.714584</td>
      <td>-74.042817</td>
      <td>Brunswick &amp; 6th</td>
      <td>40.726012</td>
      <td>-74.050389</td>
      <td>Subscriber</td>
      <td>1989.0</td>
      <td>Female</td>
      <td>29.0</td>
      <td>1.5</td>
      <td>1</td>
      <td>1</td>
      <td>Monday</td>
      <td>21-30</td>
    </tr>
    <tr>
      <th>2</th>
      <td>New York</td>
      <td>31940</td>
      <td>2018-01-01 01:29:03</td>
      <td>2018-01-01 01:33:58</td>
      <td>294</td>
      <td>Sip Ave</td>
      <td>40.730743</td>
      <td>-74.063784</td>
      <td>Baldwin at Montgomery</td>
      <td>40.723659</td>
      <td>-74.064194</td>
      <td>Subscriber</td>
      <td>1994.0</td>
      <td>Male</td>
      <td>24.0</td>
      <td>1.5</td>
      <td>1</td>
      <td>1</td>
      <td>Monday</td>
      <td>21-30</td>
    </tr>
    <tr>
      <th>3</th>
      <td>New York</td>
      <td>31949</td>
      <td>2018-01-01 01:59:32</td>
      <td>2018-01-01 02:02:49</td>
      <td>197</td>
      <td>Newark Ave</td>
      <td>40.721525</td>
      <td>-74.046305</td>
      <td>Monmouth and 6th</td>
      <td>40.725685</td>
      <td>-74.048790</td>
      <td>Subscriber</td>
      <td>1964.0</td>
      <td>Male</td>
      <td>54.0</td>
      <td>2.0</td>
      <td>1</td>
      <td>1</td>
      <td>Monday</td>
      <td>51-60</td>
    </tr>
    <tr>
      <th>4</th>
      <td>New York</td>
      <td>31929</td>
      <td>2018-01-01 02:06:18</td>
      <td>2018-01-01 02:21:50</td>
      <td>932</td>
      <td>Exchange Place</td>
      <td>40.716247</td>
      <td>-74.033459</td>
      <td>Newport Pkwy</td>
      <td>40.728745</td>
      <td>-74.032108</td>
      <td>Subscriber</td>
      <td>1992.0</td>
      <td>Male</td>
      <td>26.0</td>
      <td>2.0</td>
      <td>1</td>
      <td>2</td>
      <td>Monday</td>
      <td>21-30</td>
    </tr>
  </tbody>
</table>
</div>




```python
#group data by city
sf_df=df.loc[df['city']=='San Francisco']
ny_df=df.loc[df['city']=='New York']
```

### Parsing Data - Gender


```python
#create df based on gender
nyfemale_df = ny_df.loc[ny_df["gender"] == "Female"]
nymale_df = ny_df.loc[ny_df["gender"] == "Male"]
sffemale_df = sf_df.loc[sf_df["gender"] == "Female"]
sfmale_df = sf_df.loc[sf_df["gender"] == "Male"]
```


```python
#the same analysis was conducted for San Francisco and New York Males and Females
#NY female stats:
ny_female_percent = len(nyfemale_df) / (len(nyfemale_df) + len(nymale_df))
ny_female_subscriber_percent = len(nyfemale_df['usertype'] == 'Subscriber') / len(ny_df['usertype'] == 'Subscriber')
ny_female_avg_age = round(nyfemale_df.age.mean())
ny_female_avg_duration = round(nyfemale_df.tripduration.mean())
ny_female_pop_start = nyfemale_df['start station name'].value_counts().idxmax()
ny_female_pop_stop = nyfemale_df['end station name'].value_counts().idxmax()

print('Percentage of total NY riders that are female: ', ny_female_percent)
print('Percentage of total NY subscribers that are female: ', ny_female_subscriber_percent)
print('Avg. age of female riders in NY is: ', ny_female_avg_age)
print('Avg. trip duration for females in NY is: ', ny_female_avg_duration, ' minutes')
print('The most popular start station in NY for females is:', ny_female_pop_start)
print('The most popular end station in NY for females is:', ny_female_pop_stop)
```

    Percentage of total NY riders that are female:  0.20733680251429748
    Percentage of total NY subscribers that are female:  0.19538935122694917
    Avg. age of female riders in NY is:  37
    Avg. trip duration for females in NY is:  605  minutes
    The most popular start station in NY for females is: Grove St PATH
    The most popular end station in NY for females is: Grove St PATH
    


```python
#NY Gender Charts - Pie charts are great
sns.set()
ny_df.gender.value_counts().plot(kind='pie', autopct="%1.1f%%", colors = plot_colors)
plt.title('New York Gender Breakdown', fontsize=18)
plt.axis("equal")
plt.show()
```


![png](output_13_0.png)


### Parsing Data - Age


```python
avg_age = df['age'].mean()
sf_avg_age = sf_df['age'].mean()
ny_avg_age = ny_df['age'].mean()
print('the overall average age is: {}'.format(avg_age))
print('the average age in New York is: {}'.format(ny_avg_age))
print('the average age in San Francisco is: {}'.format(sf_avg_age))
```

    the overall average age is: 36.36047789302517
    the average age in New York is: 38.046535888886616
    the average age in San Francisco is: 36.02414004162076
    


```python
#Age group breakdown by Age and Gender
sf_age_gender = sf_df.groupby(['age groups', 'gender']).gender.count().unstack().fillna(0)
sf_age_gender =sf_age_gender.drop(['Other'], axis =1).reset_index()
sf_age_gender = sf_age_gender[['age groups', 'Male', 'Female']]
sf_age_gender
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>gender</th>
      <th>age groups</th>
      <th>Male</th>
      <th>Female</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0-20</td>
      <td>4694</td>
      <td>1595</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21-30</td>
      <td>115762</td>
      <td>48292</td>
    </tr>
    <tr>
      <th>2</th>
      <td>31-40</td>
      <td>136400</td>
      <td>45198</td>
    </tr>
    <tr>
      <th>3</th>
      <td>41-50</td>
      <td>63078</td>
      <td>16066</td>
    </tr>
    <tr>
      <th>4</th>
      <td>51-60</td>
      <td>31093</td>
      <td>8818</td>
    </tr>
    <tr>
      <th>5</th>
      <td>61+</td>
      <td>9509</td>
      <td>2079</td>
    </tr>
  </tbody>
</table>
</div>




```python
# age group break down by gender SF
sf_age_gender.plot(kind='bar', color =plot_colors, stacked=True, figsize=(10,8))
plt.legend(fontsize=14)
plt.title('San Francisco Users by Age & Gender', fontsize=18)
plt.xlabel("Age Group", fontsize = 14)
plt.ylabel("Number of Users", fontsize = 14)
plt.xticks(np.arange(6),('0-20', '21-30','31-40','41-50','51-60', '61+'), fontsize= 14, rotation =0)
plt.show()
```


![png](output_17_0.png)



```python
zero = sf_df[sf_df['age groups'] == '0-20']
twenty1= sf_df[sf_df['age groups'] == '31-40' ]
forty1 = sf_df[sf_df['age groups'] == '41-50' ]
fifty1 = sf_df[sf_df['age groups'] == '51-60' ]
sixty1 = sf_df[sf_df['age groups'] == '61+' ] 
thirty1 = sf_df[sf_df['age groups'] == '21-30' ]
```


```python
plt.figure(figsize=(20,8))
sns.set(rc={'lines.linewidth':4.5}, font_scale=2)
sns.distplot(zero['start hour'], hist=False, color ='#95a5a6', label = '0-20')
sns.distplot(twenty1['start hour'], hist=False, color ='#9b59b6', label = '21-30')
sns.distplot(thirty1['start hour'], hist=False, color = '#3498db', label = '31-40')
sns.distplot(forty1['start hour'], hist=False, color = '#e74c3c',label = '41-50')
sns.distplot(fifty1['start hour'], hist=False, color = '#34495e',label = '51-60')
sns.distplot(sixty1['start hour'], hist=False, color = '#2ecc71',label = '61+')
plt.legend()
plt.title("Start Time by Age Group_San Francisco")
plt.xlabel("Time")
plt.xticks(np.arange(0, 25, step=4), ('midnight', '4am', '8am', 'noon', '4pm', '8pm', 'midnight'))
plt.ylabel("User Count")
plt.show()
```


![png](output_19_0.png)



```python
#customer type by age
#NewYork
ny_total_riders = ny_df['usertype'].count()
print('total number of users in New York is: {}'.format(ny_total_riders))

ny_usertype = ny_df.groupby('usertype')['age groups'].value_counts(normalize=True)
ny_usertype
ny_usertype.unstack(0).plot(kind='bar', figsize=(10,8))
plt.legend(fontsize=14)
plt.title('Customer vs Subscriber - New York', fontsize=18)
plt.xlabel("Age Group", fontsize = 14)
plt.ylabel("Number of Users", fontsize = 14)
plt.gca().set_yticklabels(['{:.0f}%'.format(x*100) for x in plt.gca().get_yticks()], fontsize=12) 
plt.xticks(np.arange(6),('0-20', '21-30','31-40','41-50','51-60', '61+'), fontsize= 12, rotation=0)
plt.show()
```

    total number of users in New York is: 102979
    


![png](output_20_1.png)


### Parsing Data - Trip Analysis


```python
#New York most popular times/days
ny_hour_counts=ny_df.hour.value_counts()
print("the most popular hours to ride are: {}".format(ny_hour_counts.head(5).reset_index()))
print()
ny_day_counts=ny_df.day_of_week.value_counts()
print("the most popular days to ride are: {}".format(ny_day_counts.head(5).reset_index()))
print()
ny_month_counts=ny_df.month.value_counts()
print("the most popular months to ride are: {}".format(ny_month_counts.head(5).reset_index()))
```

    the most popular hours to ride are:    index   hour
    0      8  12163
    1     18  10978
    2     17   9984
    3     19   7512
    4      7   6976
    
    the most popular days to ride are:        index  day_of_week
    0    Tuesday        17530
    1   Thursday        16649
    2  Wednesday        16553
    3     Monday        16164
    4     Friday        14884
    
    the most popular months to ride are:    index  month
    0      5  34455
    1      4  23634
    2      3  17109
    3      2  15104
    4      1  12677
    


```python
#graph size#graph s 
sns.set()
plt.figure(figsize=(10,8))

#graph type with no. of bins
plt.hist(sf_df['hour'], color="#2ecc71", bins = 24)
plt.hist(ny_df['hour'], color= '#34495e', bins = 24)

#lables
plt.xlabel('Hour', fontsize=14)
plt.ylabel('No. of users', fontsize=14)
plt.xticks(np.arange(0,24,1))

New_York =  mpatches.Patch(color='#34495e', label='New York')
Bay_Area = mpatches.Patch(color= "#2ecc71", label='San Francisco')

#title
plt.title('Distribution of Riders/Hour in San Francisco', fontsize=18)
plt.legend(handles =[New_York,Bay_Area])
#displaying of subscribers
plt.show()
```


![png](output_23_0.png)



```python
#Riders by day of week
#create dataframe
sf_riders=[93800,91640,90066,82198,80114,48338,42238]
ny_riders=[17530, 16649,16553,16164,14884,11785,9414]
sf_ny = list(zip(sf_riders, ny_riders))
riders=pd.DataFrame(sf_ny)
riders['day'] = ['Tuesday', 'Thursday', 'Wednesday', 'Monday', 'Friday', 'Saturday', 'Sunday']
riders.columns=['sf','ny', 'day']

#create graph
sns.set()
plt.figure(figsize=(10,8))

riders.plot(kind='bar', color=["#2ecc71", '#34495e'] )

#lables
plt.xlabel('Week Days', fontsize=14)
plt.ylabel('No. of users', fontsize=14)

#title
plt.title('Distribution of Riders/Day', fontsize=18)
plt.xticks(np.arange(0,7,1), ('Tuesday', 'Thursday', 'Wednesday', 'Monday', 'Friday','Saturaday', 'Sunday'))

plt.show()
```


    <matplotlib.figure.Figure at 0x1f7120b7b00>



![png](output_24_1.png)



```python
#usage by month
#graph size
sns.set()
plt.figure(figsize=(10,8))

#graph type with no. of bins
plt.hist(ny_df['month'], bins = 5, color='#34495e')

#lables
plt.xlabel('Months', fontsize=14)
plt.ylabel('No. of users', fontsize=14)
plt.xticks(np.arange(1,6,1), ('January', 'February', 'March', 'April', 'May'))


#title
plt.title('Distribution of Riders/Month in New York', fontsize=18)
#displaying of subscribers
plt.show()
```


![png](output_25_0.png)


### Data Parsing - Location Analysis


```python
ny_data = ny_df.sample(n=100000)
sf_data = sf_df.sample(n=100000)
```


```python
#New York Start Station popularity
map_ny_start = folium.Map(location=[40.7270, -74.0367],
                    zoom_start = 14) 

# Ensure you're handing it floats
ny_data['start station latitude'] = ny_data['start station latitude'].astype(float)
ny_data['start station longitude'] = ny_data['start station longitude'].astype(float)

# Filter the DF for rows, then columns, then remove NaNs
heat_ny = ny_data[['start station latitude', 'start station longitude']].iloc[3000:46000]

# List comprehension to make out list of lists
heat_data_ny = [[row['start station latitude'],row['start station longitude']] for index, row in heat_ny.iterrows()]

# Plot it on the map
HeatMap(heat_data_ny).add_to(map_ny_start)
map_ny_start
```








```python
#End Sation New York
map_ny_end = folium.Map(location=[40.7270, -74.0367],
                    zoom_start = 14) 

# Ensure you're handing it floats
ny_data['end station latitude'] = ny_data['end station latitude'].astype(float)
ny_data['end station longitude'] = ny_data['end station longitude'].astype(float)

# Filter the DF for rows, then columns, then remove NaNs
heat_ny = ny_data[['end station latitude', 'end station longitude']].iloc[3000:46000]

# List comprehension to make out list of lists
heat_data_ny = [[row['end station latitude'],row['end station longitude']] for index, row in heat_ny.iterrows()]

# Plot it on the map
HeatMap(heat_data_ny).add_to(map_ny_end)
map_ny_end
```





