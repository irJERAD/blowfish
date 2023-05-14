---
title: "Work with Probability Distributions in Python Guide"
date: 2023-05-14
draft: false
description: "a guide to work with probability distributions on school district performance scores"
showAuthor: true
authors:
  - "jeradacosta"
slug:
tags: ["Google Advanced Data Analytics Professional Certificate", "lab", "jupyter", "python"]
series: ["The Power of Statistics"]
series_order: 8
---

# Annotated follow-along guide: Work with probability distributions in Python

This notebook contains the code used in the following instructional video: [Work with probability distributions in Python](https://www.coursera.org/learn/the-power-of-statistics/lecture/loR42/work-with-probability-distributions-in-python).


## Introduction

Throughout this notebook, we will use the normal distribution to model our data. We will also compute z-scores to find any outliers in our data. Before getting started, watch the associated instructional video and complete the in-video question. All of the code we will be implementing and related instructions are contained in this notebook.

## Overview

In this notebook, we will continue with the previous scenario in which you’re a data professional working for the Department of Education of a large nation. Recall that we are analyzing data on the literacy rate for each district, and we have already computed descriptive statistics to summarize your data. For the next part of our analysis, we want to find out if the data on district literacy rate fits a specific type of probability distribution. 

## Import packages and libraries

Before getting started, we will need to import all the required libraries and extensions. Throughout the course, we will be using pandas and numpy for operations, and matplotlib for plotting. We will also be using two Python packages that may be new to you: SciPy stats and Statsmodels.

SciPy is an open-source software you can use for solving mathematical, scientific, engineering, and technical problems. It allows you to manipulate and visualize data with a wide range of Python commands. SciPy stats is a module designed specifically for statistics.

Statsmodels is a Python package that lets you explore data, work with statistical models, and perform statistical tests. It includes an extensive list of stats functions for different types of data.


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from scipy import stats
import statsmodels.api as sm
```


```python
education_districtwise = pd.read_csv('education_districtwise.csv')
education_districtwise = education_districtwise.dropna()
```

**NOTE:** You can use `dropna()` to remove missing values in your data.

### Plot a histogram

The first step in trying to model your data with a probability distribution is to plot a histogram. This will help you visualize the shape of your data and determine if it resembles the shape of a specific distribution. 

Let's use matplotlib’s histogram function to plot a histogram of the district literacy rate data. Recall that the `OVERALL_LI` column contains this data. 


```python
education_districtwise['OVERALL_LI'].hist()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fcd69819f10>




![png](output_10_1.png)


### Normal distribution


The histogram shows that the distribution of the literacy rate data is bell-shaped and symmetric about the mean. The mean literacy rate, which is around 73%, is located in the center of the plot. Recall that the **normal distribution** is a continuous probability distribution that is bell-shaped and symmetrical on both sides of the mean. The shape of the histogram suggests that the normal distribution might be a good modeling option for the data. 

### Empirical rule

Since the normal distribution seems like a good fit for the district literacy rate data, we can expect the empirical rule to apply relatively well. Recall that the **empirical rule** says that for a normal distribution:

*   **68%** of the values fall within +/- 1 SD from the mean
*   **95%** of the values fall within +/- 2 SD from the mean
*   **99.7%** of the values fall within +/- 3 SD from the mean

**NOTE**: "SD" stands for standard deviation.

 In other words, we can expect that about:

*   **68%** of district literacy rates will fall within +/- 1 SD from the mean.
*   **95%** of district literacy rates will fall within +/- 2 SD from the mean.
*   **99.7%** of district literacy rates will fall within +/- 3 SD from the mean.



First, we will name two new variables to store the values for the mean and standard deviation of the district literacy rate: `mean_overall_li` and `std_overall_li`. 


```python
mean_overall_li = education_districtwise['OVERALL_LI'].mean()
mean_overall_li
```




    73.39518927444797



The mean district literacy rate is about 73.4%.


```python
std_overall_li = education_districtwise['OVERALL_LI'].std()
std_overall_li
```




    10.098460413782469



The standard deviation is about 10%.

Now, let's compute the actual percentage of district literacy rates that fall within +/- 1 SD from the mean. 

To do this, we will first name two new variables: `lower_limit` and `upper_limit`. The lower limit will be one SD *below* the mean, or the mean - (1 * SD). The upper limit will be one SD *above* the mean, or the mean + (1 * SD). To write the code for the calculations, ww will use our two previous variables, `mean_overall_li` and `std_overall_li`, for the mean and standard deviation.

Then, we will add a new line of code that tells the computer to decide if each value in the `OVERALL_LI` column is between the lower limit and upper limit. To do this, we will use the relational operators greater than or equal to (`>=`) and less than or equal to (`<=`), and the bitwise operator AND (`&`). Finally, we will use `mean()` to divide the number of values that are within 1 SD of the mean by the total number of values. 



```python
lower_limit = mean_overall_li - 1 * std_overall_li
upper_limit = mean_overall_li + 1 * std_overall_li
((education_districtwise['OVERALL_LI'] >= lower_limit) & (education_districtwise['OVERALL_LI'] <= upper_limit)).mean()
```




    0.6640378548895899



Next, let's use the same code structure to compute the actual percentage of district literacy rates that fall within +/- 2 SD from the mean.


```python
lower_limit = mean_overall_li - 2 * std_overall_li
upper_limit = mean_overall_li + 2 * std_overall_li
((education_districtwise['OVERALL_LI'] >= lower_limit) & (education_districtwise['OVERALL_LI'] <= upper_limit)).mean()
```




    0.9542586750788643



Finally, we will use the same code structure to compute the actual percentage of district literacy rates that fall within +/- 3 SD from the mean.


```python
lower_limit = mean_overall_li - 3 * std_overall_li
upper_limit = mean_overall_li + 3 * std_overall_li
((education_districtwise['OVERALL_LI'] >= lower_limit) & (education_districtwise['OVERALL_LI'] <= upper_limit)).mean()
```




    0.9968454258675079



Our values agree quite well with the empirical rule!

Our values of 66.4%, 95.4%, and 99.6% are very close to the values the empirical rule suggests: roughly 68%, 95%, and 99.7%.


Knowing that your data is normally distributed is useful for analysis because many statistical tests and machine learning models assume a normal distribution. Plus, when your data follows a normal distribution, you can use z-scores to measure the relative position of your values and find outliers in your data.

### Compute z-scores to find outliers

Recall that a **z-score** is a measure of how many standard deviations below or above the population mean a data point is. A z-score is useful because it tells you where a value lies in a distribution. 

Data professionals often use z-scores for outlier detection. Typically, they consider observations with a z-score smaller than -3 or larger than +3 as outliers. In other words, these are values that lie more than +/- 3 SDs from the mean. 

To find outliers in the data, we will first create a new column called `Z_SCORE` that includes the z-scores for each district literacy rate in your dataset. Recall that the `OVERALL_LI` column lists all the district literacy rates.  

Then, we will compute the z-scores using the function `scipy.stats.zscore()`. 

**Reference**: [scipy.stats.zscore](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.zscore.html)


```python
education_districtwise['Z_SCORE'] = stats.zscore(education_districtwise['OVERALL_LI'])
education_districtwise
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
      <th>Z_SCORE</th>
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
      <td>-0.641712</td>
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
      <td>-0.640721</td>
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
      <td>-0.216559</td>
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
      <td>-1.527694</td>
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
      <td>-0.831990</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>675</th>
      <td>DISTRICT522</td>
      <td>STATE29</td>
      <td>37</td>
      <td>876</td>
      <td>137</td>
      <td>5296396.0</td>
      <td>78.05</td>
      <td>0.461307</td>
    </tr>
    <tr>
      <th>676</th>
      <td>DISTRICT498</td>
      <td>STATE29</td>
      <td>64</td>
      <td>1458</td>
      <td>230</td>
      <td>4042191.0</td>
      <td>56.06</td>
      <td>-1.717972</td>
    </tr>
    <tr>
      <th>677</th>
      <td>DISTRICT343</td>
      <td>STATE29</td>
      <td>59</td>
      <td>1117</td>
      <td>216</td>
      <td>3483648.0</td>
      <td>65.05</td>
      <td>-0.827035</td>
    </tr>
    <tr>
      <th>678</th>
      <td>DISTRICT130</td>
      <td>STATE29</td>
      <td>51</td>
      <td>993</td>
      <td>211</td>
      <td>3522644.0</td>
      <td>66.16</td>
      <td>-0.717030</td>
    </tr>
    <tr>
      <th>679</th>
      <td>DISTRICT341</td>
      <td>STATE29</td>
      <td>41</td>
      <td>783</td>
      <td>185</td>
      <td>2798214.0</td>
      <td>65.46</td>
      <td>-0.786403</td>
    </tr>
  </tbody>
</table>
<p>634 rows × 8 columns</p>
</div>



Now that we have computed z-scores for our dataset,we will write some code to identify outliers, or districts with z-scores that are more than +/- 3 SDs from the mean. Let's use the relational operators greater than (`>`) and less than (`<`), and the bitwise operator OR (`|`). 


```python
education_districtwise[(education_districtwise['Z_SCORE'] > 3) | (education_districtwise['Z_SCORE'] < -3)]
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
      <th>Z_SCORE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>434</th>
      <td>DISTRICT461</td>
      <td>STATE31</td>
      <td>4</td>
      <td>360</td>
      <td>53</td>
      <td>532791.0</td>
      <td>42.67</td>
      <td>-3.044964</td>
    </tr>
    <tr>
      <th>494</th>
      <td>DISTRICT429</td>
      <td>STATE22</td>
      <td>6</td>
      <td>612</td>
      <td>62</td>
      <td>728677.0</td>
      <td>37.22</td>
      <td>-3.585076</td>
    </tr>
  </tbody>
</table>
</div>



Using z-scores, we can identify two outlying districts that have unusually low literacy rates: `DISTRICT461` and `DISTRICT429`. The literacy rates in these two districts are more than 3 SDs *below* the  overall mean literacy rate. 

Our analysis gives us important information to share. The government may want to provide more funding and resources to these two districts in the hopes of significantly improving literacy. 

## Conclusion

If you have successfully completed the material above, congratulations! You now understand how to use Python to model your data with the normal distribution and compute z-scores to find outliers in your data. Going forward, you can start using probability distributions to model your own datasets.
