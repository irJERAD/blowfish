---
title: "Exploring Sampling in Python Lab"
date: 2023-05-16
draft: false
description: "a lab that explores sampling with python"
showAuthor: true
authors:
  - "jeradacosta"
slug:
tags: ["Google Advanced Data Analytics Professional Certificate", "lab", "jupyter", "python"]
series: ["The Power of Statistics"]
series_order: 12
---

# Exemplar: Explore sampling

## Introduction
In this activity, you will engage in effective sampling of a dataset in order to make it easier to analyze. As a data professional you will often work with extremely large datasets, and utilizing proper sampling techniques helps you improve your efficiency in this work. 

For this activity, you are a member of an analytics team for the Environmental Protection Agency. You are assigned to analyze data on air quality with respect to carbon monoxide—a major air pollutant—and report your findings. The data utilized in this activity includes information from over 200 sites, identified by their state name, county name, city name, and local site name. You will use effective sampling within this dataset. 

## Step 1: Imports

### Import packages

Import `pandas`,  `numpy`, `matplotlib`, `statsmodels`, and `scipy`. 


```python
# Import libraries and packages

### YOUR CODE HERE ###

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import statsmodels.api as sm
from scipy import stats
```

### Load the dataset

Load the dataset provided. The dataset is in the form of a csv file.


```python
# Load data

### YOUR CODE HERE ###

epa_data = pd.read_csv("c4_epa_air_quality.csv", index_col = 0)
```

<details>
  <summary><h4>Hint 1</h4></summary>

Use the function in the `pandas` library that allows you to read in data from a csv file and load it into a DataFrame. 

</details>

<details>
  <summary><h4>Hint 2</h4></summary>

Use the `read_csv` function from the pandas `library`. Set the `index_col` parameter to `0` to read in the first column as an index (and to avoid `"Unnamed: 0"` appearing as a column in the resulting Dataframe).

</details>

## Step 2: Data exploration

### Examine the data

To understand how the dataset is structured, examine the first 10 rows of the data.


```python
# First 10 rows of the data

### YOUR CODE HERE ###

epa_data.head(10)
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
      <th>date_local</th>
      <th>state_name</th>
      <th>county_name</th>
      <th>city_name</th>
      <th>local_site_name</th>
      <th>parameter_name</th>
      <th>units_of_measure</th>
      <th>arithmetic_mean</th>
      <th>aqi</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-01</td>
      <td>Arizona</td>
      <td>Maricopa</td>
      <td>Buckeye</td>
      <td>BUCKEYE</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.473684</td>
      <td>7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-01</td>
      <td>Ohio</td>
      <td>Belmont</td>
      <td>Shadyside</td>
      <td>Shadyside</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.263158</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-01</td>
      <td>Wyoming</td>
      <td>Teton</td>
      <td>Not in a city</td>
      <td>Yellowstone National Park - Old Faithful Snow ...</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.111111</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-01</td>
      <td>Pennsylvania</td>
      <td>Philadelphia</td>
      <td>Philadelphia</td>
      <td>North East Waste (NEW)</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.300000</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-01</td>
      <td>Iowa</td>
      <td>Polk</td>
      <td>Des Moines</td>
      <td>CARPENTER</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.215789</td>
      <td>3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2018-01-01</td>
      <td>Hawaii</td>
      <td>Honolulu</td>
      <td>Not in a city</td>
      <td>Kapolei</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.994737</td>
      <td>14</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2018-01-01</td>
      <td>Hawaii</td>
      <td>Honolulu</td>
      <td>Not in a city</td>
      <td>Kapolei</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.200000</td>
      <td>2</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2018-01-01</td>
      <td>Pennsylvania</td>
      <td>Erie</td>
      <td>Erie</td>
      <td>NaN</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.200000</td>
      <td>2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2018-01-01</td>
      <td>Hawaii</td>
      <td>Honolulu</td>
      <td>Honolulu</td>
      <td>Honolulu</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.400000</td>
      <td>5</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2018-01-01</td>
      <td>Colorado</td>
      <td>Larimer</td>
      <td>Fort Collins</td>
      <td>Fort Collins - CSU - S. Mason</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.300000</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>



<details>
  <summary><h4><strong> Hint 1 </STRONG></h4></summary>

Use the function in the `pandas` library that allows you to get a specific number of rows from the top of a DataFrame. 

</details>

<details>
  <summary><h4><strong> Hint 2 </STRONG></h4></summary>

Use the `head` function from the `pandas` library. Set the `n` parameter to `10` to print out the first 10 rows.

</details>

**Question:** What does the `aqi` column represent?

- The `aqi` column represents the Air Quality Index.

**Question:** Based on the first few rows of the data, in what unit are `aqi` values expressed?

- As the `units_of_measure` column contains parts per million, you can infer that the aqi values are measured in parts per million.

### Generate a table of descriptive statistics

Generate a table of some descriptive statistics about the data. Specify that all columns of the input be included in the output.


```python
### YOUR CODE HERE ###

epa_data.describe(include='all')
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
      <th>date_local</th>
      <th>state_name</th>
      <th>county_name</th>
      <th>city_name</th>
      <th>local_site_name</th>
      <th>parameter_name</th>
      <th>units_of_measure</th>
      <th>arithmetic_mean</th>
      <th>aqi</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>260</td>
      <td>260</td>
      <td>260</td>
      <td>260</td>
      <td>257</td>
      <td>260</td>
      <td>260</td>
      <td>260.000000</td>
      <td>260.000000</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>1</td>
      <td>52</td>
      <td>149</td>
      <td>190</td>
      <td>253</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>top</th>
      <td>2018-01-01</td>
      <td>California</td>
      <td>Los Angeles</td>
      <td>Not in a city</td>
      <td>Kapolei</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>260</td>
      <td>66</td>
      <td>14</td>
      <td>21</td>
      <td>2</td>
      <td>260</td>
      <td>260</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.403169</td>
      <td>6.757692</td>
    </tr>
    <tr>
      <th>std</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.317902</td>
      <td>7.061707</td>
    </tr>
    <tr>
      <th>min</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.200000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.276315</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.516009</td>
      <td>9.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.921053</td>
      <td>50.000000</td>
    </tr>
  </tbody>
</table>
</div>



<details>
  <summary><h4>Hint 1</h4></summary>

Use function in the `pandas` library that allows you to generate a table of basic descriptive statistics in a DataFrame.

</details>

<details>
  <summary><h4>Hint 2</h4></summary>

Use the `describe` function from the `pandas` library. Set the `include` parameter passed in to this function to 'all' to specify that all columns of the input be included in the output.

</details>

**Question:** Based on the preceding table of descriptive statistics, what is the mean value of the `aqi` column? 

- The value is `6.757692`. This value will be compared to the mean value after sampling with replacement later in the notebook.

**Question:** Based on the preceding table of descriptive statistics, what do you notice about the count value for the `aqi` column?**

- The count value for the `aqi` column is 260. This means there are 260 AQI measurements represented in this dataset.

### Use the `mean()` function on the `aqi`  column

Now, use the `mean()` function on the `aqi`  column and assign the value to a variable `population_mean`. The value should be the same mean value the describe() function produced for the 'aqi' column.


```python
### YOUR CODE HERE ###

population_mean = epa_data['aqi'].mean()
```

<details>
  <summary><h4><strong> Hint 1 </STRONG></h4></summary>

Use the function in the `pandas` library that allows you to generate a mean value for a column in a DataFrame.

</details>

<details>
  <summary><h4><strong> Hint 2 </STRONG></h4></summary>

Use the `mean()` function.

</details>

## Step 3: Statistical tests

### Sample with replacement

First, name a new variable `sampled_data`. Then, set the arguments for the sample function `N`, sample size, equal to `50`. Set `replace` equal to `“True”` to specify sampling with replacement. For `random_state`, choose an arbitrary number for random seed. Make that arbitrary number `42`.


```python
### YOUR CODE HERE ###

sampled_data = epa_data.sample(n=50, replace=True, random_state=42)
```

### Output the first 10 rows

Output the first 10 rows of the DataFrame. 


```python
### YOUR CODE HERE ###

sampled_data.head(10)
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
      <th>date_local</th>
      <th>state_name</th>
      <th>county_name</th>
      <th>city_name</th>
      <th>local_site_name</th>
      <th>parameter_name</th>
      <th>units_of_measure</th>
      <th>arithmetic_mean</th>
      <th>aqi</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>102</th>
      <td>2018-01-01</td>
      <td>Texas</td>
      <td>Harris</td>
      <td>Houston</td>
      <td>Clinton</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.157895</td>
      <td>2</td>
    </tr>
    <tr>
      <th>106</th>
      <td>2018-01-01</td>
      <td>California</td>
      <td>Imperial</td>
      <td>Calexico</td>
      <td>Calexico-Ethel Street</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>1.183333</td>
      <td>26</td>
    </tr>
    <tr>
      <th>71</th>
      <td>2018-01-01</td>
      <td>Alabama</td>
      <td>Jefferson</td>
      <td>Birmingham</td>
      <td>Arkadelphia/Near Road</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.200000</td>
      <td>2</td>
    </tr>
    <tr>
      <th>188</th>
      <td>2018-01-01</td>
      <td>Arizona</td>
      <td>Maricopa</td>
      <td>Tempe</td>
      <td>Diablo</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.542105</td>
      <td>10</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2018-01-01</td>
      <td>Virginia</td>
      <td>Roanoke</td>
      <td>Vinton</td>
      <td>East Vinton Elementary School</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.100000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>102</th>
      <td>2018-01-01</td>
      <td>Texas</td>
      <td>Harris</td>
      <td>Houston</td>
      <td>Clinton</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.157895</td>
      <td>2</td>
    </tr>
    <tr>
      <th>121</th>
      <td>2018-01-01</td>
      <td>North Carolina</td>
      <td>Mecklenburg</td>
      <td>Charlotte</td>
      <td>Garinger High School</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.200000</td>
      <td>2</td>
    </tr>
    <tr>
      <th>214</th>
      <td>2018-01-01</td>
      <td>Florida</td>
      <td>Broward</td>
      <td>Davie</td>
      <td>Daniela Banu NCORE</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.273684</td>
      <td>5</td>
    </tr>
    <tr>
      <th>87</th>
      <td>2018-01-01</td>
      <td>California</td>
      <td>Humboldt</td>
      <td>Eureka</td>
      <td>Jacobs</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.393750</td>
      <td>5</td>
    </tr>
    <tr>
      <th>99</th>
      <td>2018-01-01</td>
      <td>California</td>
      <td>Santa Barbara</td>
      <td>Goleta</td>
      <td>Goleta</td>
      <td>Carbon monoxide</td>
      <td>Parts per million</td>
      <td>0.222222</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



<details>
  <summary><h4><strong> Hint 1 </STRONG></h4></summary>

Use the function in the `pandas` library that allows you to get a specific number of rows from the top of a DataFrame. 

</details>

<details>
  <summary><h4><strong> Hint 2 </STRONG></h4></summary>

Use the `head` function from the `pandas` library. Set the `n` parameter to `10` to print out the first 10 rows.

</details>

**Question:** In the DataFrame output, why is the row index 102 repeated twice? 

- Sampling with replacement is random, allowing sampling units to occur more than once. Row index 102 just happened to be sampled more than once. 

**Question:** What does `random_state` do?

- The parameter allows for the reproduction of the same exact sample (i.e., the same set of numbers). This means that the same rows in the dataset will be sampled with replacement each time the command is run. 

### Compute the mean value from the `aqi` column

Compute the mean value from the `aqi` column in `sampled_data` and assign the value to the variable `sample_mean`.


```python
### YOUR CODE HERE ###

sample_mean = sampled_data['aqi'].mean()
```

 **Question:** Why is `sample_mean` different from `population_mean`?

- Due to sampling variability, the sample mean (`sample_mean`) is usually not the same as the population mean (`population_mean`). In this case, the sample mean is a point estimate of the population mean based on a random sample of 50 AQI values rather than the 260 AQI values from the original population in `epa_data`.  

### Apply the central limit theorem

Imagine repeating the the earlier sample with replacement 10,000 times and obtaining 10,000 point estimates of the mean. In other words, imagine taking 10,000 random samples of 50 AQI values and computing the mean for each sample. According to the **central limit theorem**, the mean of a sampling distribution should be roughly equal to the population mean. Complete the following steps to compute the mean of the sampling distribution with 10,000 samples. 

* Create an empty list and assign it to a variable called `estimate_list`. 
* Iterate through a `for` loop 10,000 times. To do this, make sure to utilize the `range()` function to generate a sequence of numbers from 0 to 9,999. 
* In each iteration of the loop, use the `sample()` function to take a random sample (with replacement) of 50 AQI values from the population. Do not set `random_state` to a value.
* Use the list `append()` function to add the value of the sample `mean` to each item in the list.



```python
### YOUR CODE HERE ###

estimate_list = []
for i in range(10000):
    estimate_list.append(epa_data['aqi'].sample(n=50,replace=True).mean())
```

<details>
  <summary><h4><strong> Hint 1 </STRONG></h4></summary>

Review [the content about sampling in Python](https://www.coursera.org/learn/the-power-of-statistics/lecture/SNOE0/sampling-distributions-with-python). 

</details>

### Create a new DataFrame

Next, create a new DataFrame from the list of 10,000 estimates. Name the new variable `estimate_df`.


```python
### YOUR CODE HERE ###

estimate_df = pd.DataFrame(data={'estimate': estimate_list})
estimate_df
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
      <th>estimate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6.10</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.04</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7.50</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7.58</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>9995</th>
      <td>5.56</td>
    </tr>
    <tr>
      <th>9996</th>
      <td>6.96</td>
    </tr>
    <tr>
      <th>9997</th>
      <td>6.92</td>
    </tr>
    <tr>
      <th>9998</th>
      <td>7.78</td>
    </tr>
    <tr>
      <th>9999</th>
      <td>6.02</td>
    </tr>
  </tbody>
</table>
<p>10000 rows × 1 columns</p>
</div>



<details>
  <summary><h4><strong> Hint 1 </STRONG></h4></summary>

Review [the content about sampling in Python](https://www.coursera.org/learn/the-power-of-statistics/lecture/SNOE0/sampling-distributions-with-python). 

</details>

<details>
<summary><h4><strong> Hint 2 </STRONG></h4></summary>

Use the `mean()` function.

</details>

### Compute the mean() of the sampling distribution

Next, compute the `mean()` of the sampling distribution of 10,000 random samples and store the result in a new variable `mean_sample_means`.


```python
### YOUR CODE HERE ###

mean_sample_means = estimate_df['estimate'].mean()

mean_sample_means
```




    6.75664599999999



<details>
  <summary><h4><strong> Hint 1 </STRONG></h4></summary>

Use the function in the `pandas` library that allows you to generate a mean value for a column in a DataFrame.

</details>

<details>
  <summary><h4><strong> Hint 2 </STRONG></h4></summary>

Use the `mean()` function.

</details>

**Question:** What is the mean for the sampling distribution of 10,000 random samples?

This number will vary as `random_state` was not set to a value. 

<details>
  <summary><h4><strong> Hint 3 </STRONG></h4></summary>

This value is contained in `mean_sample_means`.

</details>

<details>
  <summary><h4><strong> Hint 4 </STRONG></h4></summary>

According to the central limit theorem, the mean of the preceding sampling distribution should be roughly equal to the population mean. 

</details>

**Question:** How are the central limit theorem and random sampling (with replacement) related?

Random sampling with replacement is related to the central limit theorem because it means you are drawing observations independently from a population. The central limit theorem states that if a sample size is large enough and the observations are drawn independently—or with replacement—the sampling distribution of the sample mean is approximately the normal distribution. Furthermore, the mean parameter is the population mean and the variance parameter is the standard error. 

### Output the distribution using a histogram

Output the distribution of these estimates using a histogram. This provides an idea of the sampling distribution.


```python
### YOUR CODE HERE ###

estimate_df['estimate'].hist()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f80120041d0>




![png](output_54_1.png)


<details>
  <summary><h4><strong> Hint 1 </STRONG></h4></summary>

Use the `hist()` function. 

</details>

### Calculate the standard error

Calculate the standard error of the statistic. The **standard error** of a statistic is the standard deviation of the sampling distribution associated with the statistic. It provides a numerical measure of sampling variability and answers the question: How far is a statistic based on one particular sample from the typical value of the statistic?


```python
### YOUR CODE HERE ###

standard_error = estimate_df['estimate'].std()
standard_error
```




    0.986096034482923



<details>
  <summary><h4><strong> Hint 1 </STRONG></h4></summary>

Use the `std()` function. 

</details>

## Step 4: Results and evaluation

###  Visualize the relationship between the sampling and normal distributions

Visualize the relationship between your sampling distribution of 10,000 estimates and the normal distribution. The following code overlays the density curve of the normal distribution described in the theorem on top of the histogram of the sampling distribution obtained by repeated sampling. The solid green line in the graph is the population mean and the red dashed line is the estimate.


```python
 # Generate a grid of 100 values from xmin to xmax.

 ### YOUE CODE HERE ###

plt.hist(estimate_df['estimate'], bins=25, density=True, alpha=0.4, label = "histogram of sample means of 10000 random samples")
xmin, xmax = plt.xlim()
x = np.linspace(xmin, xmax, 100) # generate a grid of 100 values from xmin to xmax.
p = stats.norm.pdf(x, population_mean, standard_error)
plt.plot(x, p, 'k', linewidth=2, label = 'normal curve from central limit theorem')
plt.axvline(x=population_mean, color='g', linestyle = 'solid', label = 'population mean')
plt.axvline(x=sample_mean, color='r', linestyle = '--', label = 'sample mean of the first random sample')
plt.axvline(x=mean_sample_means, color='b', linestyle = ':', label = 'mean of sample means of 10000 random samples')
plt.title("Sampling distribution of sample mean")
plt.xlabel('sample mean')
plt.ylabel('density')
plt.legend(bbox_to_anchor=(1.04,1))
```




    <matplotlib.legend.Legend at 0x7f800fef6590>




![png](output_61_1.png)


**Question:** What insights did you gain from the preceding sampling distribution?

1.  The histogram of the sampling distribution is well-approximated by the normal distribution described by the central limit theorem.
2.  The estimate based on one particular sample (red dashed line) is off-center. This is expected due to sampling variability. The red dashed line would be in a different location if `epa_data.sample(n=50, replace=True, random_state=42)` had a different value for `random_state`.
3.  The population mean (green solid line) and the mean of the sample means (blue dotted line) overlap, meaning that they are essentially equal to each other.

# Considerations

**What are some key takeaways that you learned from this lab?**
- Sampling with replacement on a dataset leads to duplicate rows.
- Sample means are different from population means due to sampling variability.
- The central limit theorem helps describe the sampling distribution of the sample mean for many different types of datasets.


**What findings would you share with others?**
- The mean AQI in a sample of 50 observations was below 100 in a statistically significant sense (at least 2–3 standard errors away). For reference, AQI values at or below 100 are generally thought of as satisfactory.
- This notebook didn't examine values outside the "satisfactory" range so analysis should be done to investigate unhealthy AQI values.

**What would you convey to external stakeholders?**
- Carbon monoxide levels are satisfactory in general.  
- Funding should be allocated to further investigate regions with unhealthy levels of carbon monoxide and improve the conditions in those regions.


