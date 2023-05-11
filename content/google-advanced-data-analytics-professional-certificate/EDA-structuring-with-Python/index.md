---
title: "EDA Structuing with Python"
date: 2023-04-27
draft: false
description: "exploratory data analysis structuring using python"
showAuthor: true
authors:
  - "jeradacosta"
slug:
tags: ["Google Advanced Data Analytics Professional Certificate", "lab", "jupyter", "python"]
series: ["Go Beyond the Numbers: Translate Data into Insights"]
series_order: 10
---

# Data Structuring

Throughout the following exercises, you will practice structuring data in Python.  Before starting on this programming exercise, we strongly recommend watching the video lecture and completing the IVQ for the associated topics. 

All the information you need for solving this assignment is in this notebook, and all the code you will be implementing will take place within this notebook.

As we move forward, you can find instructions on how to install required libraries as they arise in this notebook. Before we begin with the exercises and analyzing the data, we need to import all libraries and extensions required for this programming exercise. Throughout the course, we will be using pandas for operations, and matplotlib and seaborn for plotting.

## Objective 

We will be examining lightning strike data collected by the National Oceanic and Atmospheric Association (NOAA) for the year of 2018. 

First, we will find the locations with the greatest number of strikes within a single day.  
Then, we will examine the locations that had the greatest number of days with at least one lightning strike.  
Next, we will determine whether certain days of the week had more lightning strikes than others.  
Finally, we will add data from 2016 and 2017 and, for each month, calculate the percentage of total lightning strikes for that year that occurred in that month. We will then plot this data on a bar graph.


```python
# Import statements
import pandas as pd
import numpy as np
import seaborn as sns
import datetime
from matplotlib import pyplot as plt
```


```python
# Read in the 2016 data
df = pd.read_csv('eda_structuring_with_python_dataset1.csv') 
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
      <th>date</th>
      <th>number_of_strikes</th>
      <th>center_point_geom</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-03</td>
      <td>194</td>
      <td>POINT(-75 27)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-03</td>
      <td>41</td>
      <td>POINT(-78.4 29)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-03</td>
      <td>33</td>
      <td>POINT(-73.9 27)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-03</td>
      <td>38</td>
      <td>POINT(-73.8 27)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-03</td>
      <td>92</td>
      <td>POINT(-79 28)</td>
    </tr>
  </tbody>
</table>
</div>



Just like the data you encountered previously, the dataset has three columns: `date`, `number_of_strikes` and `center_point_geom`. Start by converting the `date` column to datetime. 


```python
# Convert the `date` column to datetime
df['date'] = pd.to_datetime(df['date']) 
```

Now, let's check the shape of the dataframe. 


```python
df.shape
```




    (3401012, 3)



Let's do a quick check for duplicates. If the shape of the data is different after running this code, we'll know there were duplicate rows.


```python
df.drop_duplicates().shape
```




    (3401012, 3)



The shape of the dataset after dropping duplicates is the same, so we can assume no duplicates. Hence, there is at most one row per date, per area, per number of strikes. 

### Locations with most strikes in a single day

To identify the locations with the most strikes in a single day, sort the `number_of_strikes` column in descending value, or most to least.


```python
# Sort by number of strikes in descending order
df.sort_values(by='number_of_strikes', ascending=False).head(10)
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
      <th>date</th>
      <th>number_of_strikes</th>
      <th>center_point_geom</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>302758</th>
      <td>2018-08-20</td>
      <td>2211</td>
      <td>POINT(-92.5 35.5)</td>
    </tr>
    <tr>
      <th>278383</th>
      <td>2018-08-16</td>
      <td>2142</td>
      <td>POINT(-96.1 36.1)</td>
    </tr>
    <tr>
      <th>280830</th>
      <td>2018-08-17</td>
      <td>2061</td>
      <td>POINT(-90.2 36.1)</td>
    </tr>
    <tr>
      <th>280453</th>
      <td>2018-08-17</td>
      <td>2031</td>
      <td>POINT(-89.9 35.9)</td>
    </tr>
    <tr>
      <th>278382</th>
      <td>2018-08-16</td>
      <td>1902</td>
      <td>POINT(-96.2 36.1)</td>
    </tr>
    <tr>
      <th>11517</th>
      <td>2018-02-10</td>
      <td>1899</td>
      <td>POINT(-95.5 28.1)</td>
    </tr>
    <tr>
      <th>277506</th>
      <td>2018-08-16</td>
      <td>1878</td>
      <td>POINT(-89.7 31.5)</td>
    </tr>
    <tr>
      <th>24906</th>
      <td>2018-02-25</td>
      <td>1833</td>
      <td>POINT(-98.7 28.9)</td>
    </tr>
    <tr>
      <th>284320</th>
      <td>2018-08-17</td>
      <td>1767</td>
      <td>POINT(-90.1 36)</td>
    </tr>
    <tr>
      <th>24825</th>
      <td>2018-02-25</td>
      <td>1741</td>
      <td>POINT(-98 29)</td>
    </tr>
  </tbody>
</table>
</div>



### Locations with most days with at least one lightning strike


To find the number of days that a given geographic location had at least one lightning strike, we'll use the `value_counts()` function on the `center_point_geom` column. The logic is that if each row represents a location-day, then counting the number of times each location occurs in the data will give us the number of days that location had lightning. 


```python
# Identify locations that appear most in the dataset
df.center_point_geom.value_counts()
```




    POINT(-81.5 22.5)     108
    POINT(-84.1 22.4)     108
    POINT(-82.5 22.9)     107
    POINT(-82.7 22.9)     107
    POINT(-82.5 22.8)     106
                         ... 
    POINT(-119.3 35.1)      1
    POINT(-119.3 35)        1
    POINT(-119.6 35.6)      1
    POINT(-119.4 35.6)      1
    POINT(-58.5 45.3)       1
    Name: center_point_geom, Length: 170855, dtype: int64



We find that the locations with the most days with lightning strikes had at least one strike on 108 days—nearly one out of every three days of the year. They are all rather close to each other geographically, which makes sense. Notice also that the `value_counts()` function automatically sorts the results in descending order. 

Let's examine whether there is an even distribution of values, or whether 106+ strikes are unusually high value for days with lightning strikes. We'll use the `value_counts()` function again, but this time we'll output the top 20 results. We'll also rename the columns and apply a color gradient.


```python
# Identify top 20 locations with most days of lightning
df.center_point_geom.value_counts()[:20].rename_axis('unique_values').reset_index(name='counts').style.background_gradient()
```




<style type="text/css">
#T_43838_row0_col1, #T_43838_row1_col1 {
  background-color: #023858;
  color: #f1f1f1;
}
#T_43838_row2_col1, #T_43838_row3_col1 {
  background-color: #045d92;
  color: #f1f1f1;
}
#T_43838_row4_col1, #T_43838_row5_col1 {
  background-color: #1379b5;
  color: #f1f1f1;
}
#T_43838_row6_col1, #T_43838_row7_col1 {
  background-color: #509ac6;
  color: #f1f1f1;
}
#T_43838_row8_col1, #T_43838_row9_col1 {
  background-color: #91b5d6;
  color: #000000;
}
#T_43838_row10_col1 {
  background-color: #c4cbe3;
  color: #000000;
}
#T_43838_row11_col1, #T_43838_row12_col1, #T_43838_row13_col1, #T_43838_row14_col1, #T_43838_row15_col1 {
  background-color: #e8e4f0;
  color: #000000;
}
#T_43838_row16_col1, #T_43838_row17_col1, #T_43838_row18_col1, #T_43838_row19_col1 {
  background-color: #fff7fb;
  color: #000000;
}
</style>
<table id="T_43838_">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th class="col_heading level0 col0" >unique_values</th>
      <th class="col_heading level0 col1" >counts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_43838_level0_row0" class="row_heading level0 row0" >0</th>
      <td id="T_43838_row0_col0" class="data row0 col0" >POINT(-81.5 22.5)</td>
      <td id="T_43838_row0_col1" class="data row0 col1" >108</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row1" class="row_heading level0 row1" >1</th>
      <td id="T_43838_row1_col0" class="data row1 col0" >POINT(-84.1 22.4)</td>
      <td id="T_43838_row1_col1" class="data row1 col1" >108</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row2" class="row_heading level0 row2" >2</th>
      <td id="T_43838_row2_col0" class="data row2 col0" >POINT(-82.5 22.9)</td>
      <td id="T_43838_row2_col1" class="data row2 col1" >107</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row3" class="row_heading level0 row3" >3</th>
      <td id="T_43838_row3_col0" class="data row3 col0" >POINT(-82.7 22.9)</td>
      <td id="T_43838_row3_col1" class="data row3 col1" >107</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row4" class="row_heading level0 row4" >4</th>
      <td id="T_43838_row4_col0" class="data row4 col0" >POINT(-82.5 22.8)</td>
      <td id="T_43838_row4_col1" class="data row4 col1" >106</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row5" class="row_heading level0 row5" >5</th>
      <td id="T_43838_row5_col0" class="data row5 col0" >POINT(-84.2 22.3)</td>
      <td id="T_43838_row5_col1" class="data row5 col1" >106</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row6" class="row_heading level0 row6" >6</th>
      <td id="T_43838_row6_col0" class="data row6 col0" >POINT(-76 20.5)</td>
      <td id="T_43838_row6_col1" class="data row6 col1" >105</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row7" class="row_heading level0 row7" >7</th>
      <td id="T_43838_row7_col0" class="data row7 col0" >POINT(-75.9 20.4)</td>
      <td id="T_43838_row7_col1" class="data row7 col1" >105</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row8" class="row_heading level0 row8" >8</th>
      <td id="T_43838_row8_col0" class="data row8 col0" >POINT(-82.2 22.9)</td>
      <td id="T_43838_row8_col1" class="data row8 col1" >104</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row9" class="row_heading level0 row9" >9</th>
      <td id="T_43838_row9_col0" class="data row9 col0" >POINT(-78 18.2)</td>
      <td id="T_43838_row9_col1" class="data row9 col1" >104</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row10" class="row_heading level0 row10" >10</th>
      <td id="T_43838_row10_col0" class="data row10 col0" >POINT(-83.9 22.5)</td>
      <td id="T_43838_row10_col1" class="data row10 col1" >103</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row11" class="row_heading level0 row11" >11</th>
      <td id="T_43838_row11_col0" class="data row11 col0" >POINT(-84 22.4)</td>
      <td id="T_43838_row11_col1" class="data row11 col1" >102</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row12" class="row_heading level0 row12" >12</th>
      <td id="T_43838_row12_col0" class="data row12 col0" >POINT(-82 22.8)</td>
      <td id="T_43838_row12_col1" class="data row12 col1" >102</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row13" class="row_heading level0 row13" >13</th>
      <td id="T_43838_row13_col0" class="data row13 col0" >POINT(-82 22.4)</td>
      <td id="T_43838_row13_col1" class="data row13 col1" >102</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row14" class="row_heading level0 row14" >14</th>
      <td id="T_43838_row14_col0" class="data row14 col0" >POINT(-82.3 22.9)</td>
      <td id="T_43838_row14_col1" class="data row14 col1" >102</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row15" class="row_heading level0 row15" >15</th>
      <td id="T_43838_row15_col0" class="data row15 col0" >POINT(-78 18.3)</td>
      <td id="T_43838_row15_col1" class="data row15 col1" >102</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row16" class="row_heading level0 row16" >16</th>
      <td id="T_43838_row16_col0" class="data row16 col0" >POINT(-84.1 22.5)</td>
      <td id="T_43838_row16_col1" class="data row16 col1" >101</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row17" class="row_heading level0 row17" >17</th>
      <td id="T_43838_row17_col0" class="data row17 col0" >POINT(-75.5 20.6)</td>
      <td id="T_43838_row17_col1" class="data row17 col1" >101</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row18" class="row_heading level0 row18" >18</th>
      <td id="T_43838_row18_col0" class="data row18 col0" >POINT(-84.2 22.4)</td>
      <td id="T_43838_row18_col1" class="data row18 col1" >101</td>
    </tr>
    <tr>
      <th id="T_43838_level0_row19" class="row_heading level0 row19" >19</th>
      <td id="T_43838_row19_col0" class="data row19 col0" >POINT(-76 20.4)</td>
      <td id="T_43838_row19_col1" class="data row19 col1" >101</td>
    </tr>
  </tbody>
</table>




###  Lightning strikes by day of week

One useful grouping is categorizing lightning strikes by day of the week, which will tell us whether any particular day of the week had fewer or more lightning strikes than others. To calculate this, we'll take advantage of the fact that the data in our `date` column is of the `datetime` class. Because these entries are datetime objects, we can extract date-related information from them and create new columns.

First, we'll create a column called `week` using `dt.isocalendar()` on the `date` column. This function is designed to be used on a pandas series, and it will return a new dataframe with year, week, and day columns. The information is formatted numerically, so, for example, 3 January 1950 would be respresented as:

| Year | Week | Day |
| ---- | :--: | :-: |
| 1950 | 1    | 3   |

Because we only want to extract the week number, we'll add `.week` to the end. You can learn more about `dt.isocalendar()` in the [dt.isocalendar() pandas documentation](https://pandas.pydata.org/pandas-docs/dev/reference/api/pandas.Series.dt.isocalendar.html).

We'll also add a `weekday` column using `dt.day_name()`. This is another pandas function designed to be used on a pandas series. It extracts the text name of the day for any given datetime date. You can learn more about this function in the [dt.day_name() pandas documentation](https://pandas.pydata.org/pandas-docs/dev/reference/api/pandas.Series.dt.day_name.html).



```python
# Create two new columns
df['week'] = df.date.dt.isocalendar().week
df['weekday'] = df.date.dt.day_name()
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
      <th>date</th>
      <th>number_of_strikes</th>
      <th>center_point_geom</th>
      <th>week</th>
      <th>weekday</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-03</td>
      <td>194</td>
      <td>POINT(-75 27)</td>
      <td>1</td>
      <td>Wednesday</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-03</td>
      <td>41</td>
      <td>POINT(-78.4 29)</td>
      <td>1</td>
      <td>Wednesday</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-03</td>
      <td>33</td>
      <td>POINT(-73.9 27)</td>
      <td>1</td>
      <td>Wednesday</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-03</td>
      <td>38</td>
      <td>POINT(-73.8 27)</td>
      <td>1</td>
      <td>Wednesday</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-03</td>
      <td>92</td>
      <td>POINT(-79 28)</td>
      <td>1</td>
      <td>Wednesday</td>
    </tr>
  </tbody>
</table>
</div>



Now, we can calculate the mean number of lightning strikes for each weekday of the year. We'll use the `groupby()` function to do this.


```python
# Calculate mean count of lightning strikes for each weekday
df[['weekday','number_of_strikes']].groupby(['weekday']).mean()
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
      <th>number_of_strikes</th>
    </tr>
    <tr>
      <th>weekday</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Friday</th>
      <td>13.349972</td>
    </tr>
    <tr>
      <th>Monday</th>
      <td>13.152804</td>
    </tr>
    <tr>
      <th>Saturday</th>
      <td>12.732694</td>
    </tr>
    <tr>
      <th>Sunday</th>
      <td>12.324717</td>
    </tr>
    <tr>
      <th>Thursday</th>
      <td>13.240594</td>
    </tr>
    <tr>
      <th>Tuesday</th>
      <td>13.813599</td>
    </tr>
    <tr>
      <th>Wednesday</th>
      <td>13.224568</td>
    </tr>
  </tbody>
</table>
</div>



Interesting! It seems that Saturday and Sunday has fewer average lightning strikes than the other five weekdays. Let's plot the distributions of the strike counts for each day of the week. We want each distribution to be represented as a boxplot. 

Let's begin by defining the order of the days. We'll begin with Monday and end with Sunday. This is how the days will be ordered in the plot we create.


```python
# Define order of days for the plot
weekday_order = ['Monday','Tuesday', 'Wednesday', 'Thursday','Friday','Saturday','Sunday']
```

Now, we'll code the plot. Remember that `showfliers` is the parameter that controls whether or not outliers are displayed in the plot. If you input `True`, outliers are included; if you input `False`, outliers are left off of the box plot. Keep in mind, we aren’t *deleting* any outliers from the dataset when we create this chart, we are only excluding them from the visualization.



```python
# Create boxplots of strike counts for each day of week
g = sns.boxplot(data=df, 
            x='weekday',
            y='number_of_strikes', 
            order=weekday_order, 
            showfliers=False 
            );
g.set_title('Lightning distribution per weekday (2018)');
```


![png](output_30_0.png)


Notice that the median remains the same on all of the days of the week. As for Saturday and Sunday, however, the distributions are *both* lower than the rest of the week. We also know that the mean numbers of strikes that occurred on Saturday and Sunday were lower than the other weekdays. Why might this be? Perhaps the aerosol particles emitted by factories and vehicles increase the likelihood of lightning strike. In the U.S., Saturday and Sunday are days that many people don't work, so there may be fewer factories operating and fewer cars on the road. This is only speculation, but it's one possible path for further exploration. 

### Monthly lightning strikes 2016–2018

Finally, we'll examine monthly lightning strike data from 2016–2018. We'll calculate the percentage of total lightning strikes for each year that occurred in a given month. We will then plot this data on a bar graph.


```python
# Import 2016–2017 data
df_2 = pd.read_csv('eda_structuring_with_python_dataset2.csv')
df_2.head()
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
      <th>date</th>
      <th>number_of_strikes</th>
      <th>center_point_geom</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-01-04</td>
      <td>55</td>
      <td>POINT(-83.2 21.1)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-01-04</td>
      <td>33</td>
      <td>POINT(-83.1 21.1)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-01-05</td>
      <td>46</td>
      <td>POINT(-77.5 22.1)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-01-05</td>
      <td>28</td>
      <td>POINT(-76.8 22.3)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016-01-05</td>
      <td>28</td>
      <td>POINT(-77 22.1)</td>
    </tr>
  </tbody>
</table>
</div>



The data is in the same format as the 2018 data when we first imported it above. Let's convert the `date` column to datetime.


```python
# Convert `date` column to datetime
df_2['date'] = pd.to_datetime(df_2['date'])
```

Now we can combine the 2016–2017 dataframe with the 2018 dataframe. There are several functions that can do this. We'll use `concat()`. Remember that the 2018 data has two added columns, `week` and `weekday`. To simplify the results of our combined dataframe, we'll drop these added columns during the concatenation. Note that the following code doesn't permanently modify `df`. The columns drop only for this operation. You can learn more about the `concat()` function in the [concat() pandas documentation](https://pandas.pydata.org/docs/reference/api/pandas.concat.html).


```python
# Create new dataframe combining 2016–2017 data with 2018 data
union_df = pd.concat([df.drop(['weekday','week'],axis=1), df_2], ignore_index=True)
union_df.head()
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
      <th>date</th>
      <th>number_of_strikes</th>
      <th>center_point_geom</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-03</td>
      <td>194</td>
      <td>POINT(-75 27)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-03</td>
      <td>41</td>
      <td>POINT(-78.4 29)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-03</td>
      <td>33</td>
      <td>POINT(-73.9 27)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-03</td>
      <td>38</td>
      <td>POINT(-73.8 27)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-03</td>
      <td>92</td>
      <td>POINT(-79 28)</td>
    </tr>
  </tbody>
</table>
</div>



To help us with naming the bars of the bar plot, we'll create three new columns that isolate the year, month number, and month name. 


```python
# add 3 new columns
union_df['year'] = union_df.date.dt.year
union_df['month'] = union_df.date.dt.month
union_df['month_txt'] = union_df.date.dt.month_name()
union_df.head()
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
      <th>date</th>
      <th>number_of_strikes</th>
      <th>center_point_geom</th>
      <th>year</th>
      <th>month</th>
      <th>month_txt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-03</td>
      <td>194</td>
      <td>POINT(-75 27)</td>
      <td>2018</td>
      <td>1</td>
      <td>January</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-03</td>
      <td>41</td>
      <td>POINT(-78.4 29)</td>
      <td>2018</td>
      <td>1</td>
      <td>January</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-03</td>
      <td>33</td>
      <td>POINT(-73.9 27)</td>
      <td>2018</td>
      <td>1</td>
      <td>January</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-03</td>
      <td>38</td>
      <td>POINT(-73.8 27)</td>
      <td>2018</td>
      <td>1</td>
      <td>January</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-03</td>
      <td>92</td>
      <td>POINT(-79 28)</td>
      <td>2018</td>
      <td>1</td>
      <td>January</td>
    </tr>
  </tbody>
</table>
</div>



Let's check the overall lightning strike count for each year.


```python
# Calculate total number of strikes per year
union_df[['year','number_of_strikes']].groupby(['year']).sum()
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
      <th>number_of_strikes</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016</th>
      <td>41582229</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>35095195</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>44600989</td>
    </tr>
  </tbody>
</table>
</div>



Now we'll calculate the percentage of total lightning strikes for each year that occurred in a given month and assign the results to a new dataframe called `lightning_by_month`. 


```python
# Calculate total lightning strikes for each month of each year
lightning_by_month = union_df.groupby(['month_txt','year']).agg(
    number_of_strikes = pd.NamedAgg(column='number_of_strikes',aggfunc=sum)
    ).reset_index()

lightning_by_month.head()
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
      <th>month_txt</th>
      <th>year</th>
      <th>number_of_strikes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>April</td>
      <td>2016</td>
      <td>2636427</td>
    </tr>
    <tr>
      <th>1</th>
      <td>April</td>
      <td>2017</td>
      <td>3819075</td>
    </tr>
    <tr>
      <th>2</th>
      <td>April</td>
      <td>2018</td>
      <td>1524339</td>
    </tr>
    <tr>
      <th>3</th>
      <td>August</td>
      <td>2016</td>
      <td>7250442</td>
    </tr>
    <tr>
      <th>4</th>
      <td>August</td>
      <td>2017</td>
      <td>6021702</td>
    </tr>
  </tbody>
</table>
</div>



By the way, we can use the `agg()` function to calculate the same yearly totals we found before, with 2017 having fewer strikes than the other two years.


```python
# Calculate total lightning strikes for each year
lightning_by_year = union_df.groupby(['year']).agg(
  year_strikes = pd.NamedAgg(column='number_of_strikes',aggfunc=sum)
).reset_index()

lightning_by_year.head()
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
      <th>year</th>
      <th>year_strikes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016</td>
      <td>41582229</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2017</td>
      <td>35095195</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018</td>
      <td>44600989</td>
    </tr>
  </tbody>
</table>
</div>



Back to our bar plot... We need to use the monthly totals to calculate percentages. For each month, we'll need the monthly total strike count and the total strike count for that year. Let's create another dataframe called `percentage_lightning` that adds a new column called `year_strikes`, which represents the total number of strikes for each year. We can do this using the `merge()` function. We'll merge the `lightning_by_month` dataframe with the `lightning_by_year` dataframe, specifying to merge on the `year` column. This means that wherever the `year` columns contain the same value in both dataframes, a row is created in our new dataframe with all the other columns from both dataframes being merged. To learn more about this function, refer to the [merge() pandas documentation](https://pandas.pydata.org/docs/reference/api/pandas.merge.html).



```python
# Combine `lightning_by_month` and `lightning_by_year` dataframes into single dataframe
percentage_lightning = lightning_by_month.merge(lightning_by_year,on='year')
percentage_lightning.head()
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
      <th>month_txt</th>
      <th>year</th>
      <th>number_of_strikes</th>
      <th>year_strikes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>April</td>
      <td>2016</td>
      <td>2636427</td>
      <td>41582229</td>
    </tr>
    <tr>
      <th>1</th>
      <td>August</td>
      <td>2016</td>
      <td>7250442</td>
      <td>41582229</td>
    </tr>
    <tr>
      <th>2</th>
      <td>December</td>
      <td>2016</td>
      <td>316450</td>
      <td>41582229</td>
    </tr>
    <tr>
      <th>3</th>
      <td>February</td>
      <td>2016</td>
      <td>312676</td>
      <td>41582229</td>
    </tr>
    <tr>
      <th>4</th>
      <td>January</td>
      <td>2016</td>
      <td>313595</td>
      <td>41582229</td>
    </tr>
  </tbody>
</table>
</div>



Now we'll create a new column in our new dataframe that represents the percentage of total lightning strikes for each year that occurred during each month. We do this by dividing the `number_of_strikes` column by the `year_strikes` column and multiplying the result by 100.


```python
# Create new `percentage_lightning_per_month` column
percentage_lightning['percentage_lightning_per_month'] = (percentage_lightning.number_of_strikes/
                                                          percentage_lightning.year_strikes * 100.0)
percentage_lightning.head()
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
      <th>month_txt</th>
      <th>year</th>
      <th>number_of_strikes</th>
      <th>year_strikes</th>
      <th>percentage_lightning_per_month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>April</td>
      <td>2016</td>
      <td>2636427</td>
      <td>41582229</td>
      <td>6.340273</td>
    </tr>
    <tr>
      <th>1</th>
      <td>August</td>
      <td>2016</td>
      <td>7250442</td>
      <td>41582229</td>
      <td>17.436396</td>
    </tr>
    <tr>
      <th>2</th>
      <td>December</td>
      <td>2016</td>
      <td>316450</td>
      <td>41582229</td>
      <td>0.761022</td>
    </tr>
    <tr>
      <th>3</th>
      <td>February</td>
      <td>2016</td>
      <td>312676</td>
      <td>41582229</td>
      <td>0.751946</td>
    </tr>
    <tr>
      <th>4</th>
      <td>January</td>
      <td>2016</td>
      <td>313595</td>
      <td>41582229</td>
      <td>0.754156</td>
    </tr>
  </tbody>
</table>
</div>



Now we can plot the percentages by month in a bar graph! 


```python
plt.figure(figsize=(10,6));

month_order = ['January', 'February', 'March', 'April', 'May', 'June', 
               'July', 'August', 'September', 'October', 'November', 'December']

sns.barplot(
    data = percentage_lightning,
    x = 'month_txt',
    y = 'percentage_lightning_per_month',
    hue = 'year',
    order = month_order );
plt.xlabel("Month");
plt.ylabel("% of lightning strikes");
plt.title("% of lightning strikes each Month (2016-2018)");
```


![png](output_52_0.png)


For all three years, there is a clear pattern over the course of each year. One month stands out. More than 1/3 of the lightning in 2018 happened in August. 

If you have successfully completed the material above, congratulations! You now understand how to structure data in Python and should be able to start using it on your own datasets.
