---
title: "Introduction to Statistics with Python Guide"
date: 2023-05-07
draft: false
description: "a guide for importing datasets in python"
showAuthor: true
authors:
  - "jeradacosta"
slug:
tags: ["Google Advanced Data Analytics Professional Certificate", "python", "guide"]
series: ["The Power of Statistics"]
series_order: 9
---

# Introduction to statistics with Python

Throughout the following exercises, you will learn to compute descriptive statistics to explore and summarize a dataset. Before starting on this programming exercise, we strongly recommend watching the video lecture and completing the IVQ for the associated topics.

All the information you need for solving this assignment is in this notebook, and all the code you will be implementing will take place within this notebook. 

As we move forward, you can find instructions on how to install required libraries as they arise in this notebook. Before we begin with the exercises and analyzing the data, we need to import all libraries and extensions required for this programming exercise. Throughout the course, we will be using pandas and numpy for operations, and matplotlib for plotting.


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```


```python
education_districtwise = pd.read_csv('education_districtwise.csv')
```

## Explore your data##

Earlier in the program, you learned about the process of exploratory data analysis, or EDA, from discovering to presenting your data. Whenever a data professional works with a new dataset, the first step is to understand the context of the data during the discovering stage. Often, this is done by discussing the data with project stakeholders, and reading documentation about the dataset and the data collection process. After that, a data professional moves on to data cleaning, and deals with issues like missing data, incorrect values, and irrelevant data. Computing descriptive stats is a common step to take after data cleaning. 
 

You’ll use descriptive stats to get a basic understanding of the literacy rate data for each district in your education dataset. 


Start with the `head() `function to get a quick overview of your dataset. Recall that `head()` will return as many rows of data as you input into the variable field.


```python
education_districtwise.head(10)
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
      <th>DISTNAME</th>
      <th>STATNAME</th>
      <th>BLOCKS</th>
      <th>VILLAGES</th>
      <th>CLUSTERS</th>
      <th>TOTPOPULAT</th>
      <th>OVERALL_LI</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>DISTRICT32</td>
      <td>STATE1</td>
      <td>13</td>
      <td>391</td>
      <td>104</td>
      <td>875564.0</td>
      <td>66.92</td>
    </tr>
    <tr>
      <th>1</th>
      <td>DISTRICT649</td>
      <td>STATE1</td>
      <td>18</td>
      <td>678</td>
      <td>144</td>
      <td>1015503.0</td>
      <td>66.93</td>
    </tr>
    <tr>
      <th>2</th>
      <td>DISTRICT229</td>
      <td>STATE1</td>
      <td>8</td>
      <td>94</td>
      <td>65</td>
      <td>1269751.0</td>
      <td>71.21</td>
    </tr>
    <tr>
      <th>3</th>
      <td>DISTRICT259</td>
      <td>STATE1</td>
      <td>13</td>
      <td>523</td>
      <td>104</td>
      <td>735753.0</td>
      <td>57.98</td>
    </tr>
    <tr>
      <th>4</th>
      <td>DISTRICT486</td>
      <td>STATE1</td>
      <td>8</td>
      <td>359</td>
      <td>64</td>
      <td>570060.0</td>
      <td>65.00</td>
    </tr>
    <tr>
      <th>5</th>
      <td>DISTRICT323</td>
      <td>STATE1</td>
      <td>12</td>
      <td>523</td>
      <td>96</td>
      <td>1070144.0</td>
      <td>64.32</td>
    </tr>
    <tr>
      <th>6</th>
      <td>DISTRICT114</td>
      <td>STATE1</td>
      <td>6</td>
      <td>110</td>
      <td>49</td>
      <td>147104.0</td>
      <td>80.48</td>
    </tr>
    <tr>
      <th>7</th>
      <td>DISTRICT438</td>
      <td>STATE1</td>
      <td>7</td>
      <td>134</td>
      <td>54</td>
      <td>143388.0</td>
      <td>74.49</td>
    </tr>
    <tr>
      <th>8</th>
      <td>DISTRICT610</td>
      <td>STATE1</td>
      <td>10</td>
      <td>388</td>
      <td>80</td>
      <td>409576.0</td>
      <td>65.97</td>
    </tr>
    <tr>
      <th>9</th>
      <td>DISTRICT476</td>
      <td>STATE1</td>
      <td>11</td>
      <td>361</td>
      <td>86</td>
      <td>555357.0</td>
      <td>69.90</td>
    </tr>
  </tbody>
</table>
</div>



**Note**: To interpret this data correctly, it’s important to understand that each row, or observation, refers to a different *district* – and not, for example, to a state or a village. So, the `VILLAGES` column shows how many villages are in each district, the `TOTPOPULAT` column shows the population for each district, and the `OVERALL_LI` column shows the literacy rate for each district. 

### Describe()

Now that you have a better understanding of your dataset, use Python to compute descriptive stats. 

When computing descriptive stats in Python, the most useful function to know is `describe()`. Data professionals use the `describe()` function as a convenient way to calculate many key stats all at once. For a numeric column, `describe()` gives you the following output: 

*   `count`: Number of non-NA/null observations
*   `mean`: The arithmetic average
*   `std`: The standard deviation
*   `min`: The smallest value (minimum)
*   `25%`: The first quartile (25th percentile)
*   `50%`: The median (50th percentile) 
*   `75%`: The third quartile (75th percentile)
*   `max`: The largest value (maximum)


**Reference**: https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.describe.html.

Your main interest is the literacy rate. This data is contained in the `OVERALL_LI` column, which shows the literacy rate for each district in the nation. Use the `describe()` function to show key stats about literacy rate. 


```python
education_districtwise['OVERALL_LI'].describe()
```




    count    634.000000
    mean      73.395189
    std       10.098460
    min       37.220000
    25%       66.437500
    50%       73.490000
    75%       80.815000
    max       98.760000
    Name: OVERALL_LI, dtype: float64



The summary of stats gives you valuable information about the overall literacy rate. For example, the mean helps to clarify the center of your dataset – you now know the average literacy rate is about 73% for all districts. This information is useful in itself, and also as a basis for comparison. Knowing the mean literacy rate for *all* districts helps you understand which individual districts are significantly above or below the mean. 

**Note**: `describe()` excludes missing values (`NaN`) in the dataset from consideration. You may notice that the count, or the number of observations for `OVERALL_LI` (634), is fewer than the number of rows in the dataset (680). Dealing with missing values is a complex issue outside the scope of this course.

You can also use the `describe()` function for a column with categorical data, like the `STATNAME` column. 

For a categorical column, `describe()` gives you the following output: 

*   `count`: Number of non-NA/null observations
*  `unique`: Number of unique values
*   `top`: The most common value (the mode)
*   `freq`: The frequency of the most common value



```python
education_districtwise['STATNAME'].describe()
```




    count         680
    unique         36
    top       STATE21
    freq           75
    Name: STATNAME, dtype: object



The `unique` category shows you that there are 36 states in your dataset. The `top` category shows you that `STATE21` is the most commonly occuring value, or mode. The `frequency` category tells you that `STATE21` appears in 75 rows, which means it includes 75 different districts. 

This information may be helpful in determining which states will need more educational resources based on their number of districts. 


### Functions for stats

The `describe()` function is so useful because it shows you a variety of key stats all at once. Python also has separate functions for the mean, median, standard deviation, minimum, and maximum. Earlier in the program, you used `mean()` and `median()` to detect outliers. These individual functions are also useful if you want to do further computations based on descriptive stats. For example, you can use the `min()` and `max()` functions together to compute the range of your data.


#### Range


Recall that the **range** is the difference between the largest and smallest values in a dataset. In other words, range = max - min. You can use `max()` and `min()` to compute the range for the literacy rate of all districts in your dataset. 


```python
range_overall_li = education_districtwise['OVERALL_LI'].max() - education_districtwise['OVERALL_LI'].min()
range_overall_li
```




    61.540000000000006



The range in literacy rates for all districts is about 61.5 percentage points. 

The large difference tells you that some districts have much higher literacy rates than others. Later on, you’ll continue to analyze this data, and you can discover which districts have the lowest literacy rates. This will help the government better understand literacy rates nationally and build on their successful educational programs. 

If you have successfully completed the material above, congratulations! You now understand how to compute descriptive statistics with Python. Going forward, you can start using descriptive statistics to explore and summarize your own datasets.
