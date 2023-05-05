---
title: "Unicorn Dataset Discovery"
date: 2023-04-29
draft: false
description: "exploratory data analysis on the Unicorn dataset"
showAuthor: true
authors:
  - "jeradacosta"
slug:
tags: ["Google Advanced Data Analytics Professional Certificate", "lab", "jupyter", "python"]
series: ["Foundations of Data Science"]
series_order: 11
---

# Exemplar: Discover what is in your dataset

## Introduction

In this activity, you will discover characteristics of a dataset and use visualizations to analyze the data. This will develop and strengthen your skills in **exploratory data analysis (EDA)** and your knowledge of functions that allow you to explore and visualize data. 

EDA is an essential process in a data science workflow. As a data professional, you will need to conduct this process to better understand the data at hand and determine how it can be used to solve the problem you want to address. This activity will give you an opportunity to practice that process and prepare you for EDA in future projects.

In this activity, you are a member of an analytics team that provides insights to an investing firm. To help them decide which companies to invest in next, the firm wants insights into **unicorn companies**–companies that are valued at over one billion dollars. The data you will use for this task provides information on over 1,000 unicorn companies, including their industry, country, year founded, and select investors. You will use this information to gain insights into how and when companies reach this prestigious milestone and to make recommentations for next steps to the investing firm.

## Step 1: Imports

### Import libraries and packages 

First, import the relevant Python libraries and modules. Use the `pandas` library and the `matplotlib.pyplot` module.


```python
# Import libraries and packages

### YOUR CODE HERE ###

import pandas as pd
import matplotlib.pyplot as plt
```

### Load the dataset into a DataFrame

The dataset provided is in the form of a csv file named `Unicorn_Companies.csv` and contains a subset of data on unicorn companies. Load the data from the csv file into a DataFrame and save it in a variable.


```python
# Load data from the csv file into a DataFrame and save in a variable

### YOUR CODE HERE ###

companies = pd.read_csv("Unicorn_Companies.csv")
```

<details>
  <summary><h4><strong>Hint 1</strong></h4></summary>

Refer to what you learned about [loading data](https://www.coursera.org/learn/go-beyond-the-numbers-translate-data-into-insight/supplement/MdTG2/reference-guide-import-datasets-using-python) in Python.

</details>

<details>
  <summary><h4><strong>Hint 2</strong></h4></summary>

Use the function in the `pandas` library that allows you to read data from a csv file and load the data into a DataFrame.
 

</details>

<details>
  <summary><h4><strong>Hint 3</strong></h4></summary>

Use the `read_csv()` function from the `pandas` library. 

</details>

## Step 2: Data exploration


### Display the first 10 rows of the data

Next, explore the dataset and answer questions to guide your exploration and analysis of the data. To begin, display the first 10 rows of the data to get an understanding of how the dataset is structured.


```python
# Display the first 10 rows of the data

### YOUR CODE HERE ###

companies.head(10)
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
      <th>Company</th>
      <th>Valuation</th>
      <th>Date Joined</th>
      <th>Industry</th>
      <th>City</th>
      <th>Country/Region</th>
      <th>Continent</th>
      <th>Year Founded</th>
      <th>Funding</th>
      <th>Select Investors</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bytedance</td>
      <td>$180B</td>
      <td>4/7/17</td>
      <td>Artificial intelligence</td>
      <td>Beijing</td>
      <td>China</td>
      <td>Asia</td>
      <td>2012</td>
      <td>$8B</td>
      <td>Sequoia Capital China, SIG Asia Investments, S...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>SpaceX</td>
      <td>$100B</td>
      <td>12/1/12</td>
      <td>Other</td>
      <td>Hawthorne</td>
      <td>United States</td>
      <td>North America</td>
      <td>2002</td>
      <td>$7B</td>
      <td>Founders Fund, Draper Fisher Jurvetson, Rothen...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>SHEIN</td>
      <td>$100B</td>
      <td>7/3/18</td>
      <td>E-commerce &amp; direct-to-consumer</td>
      <td>Shenzhen</td>
      <td>China</td>
      <td>Asia</td>
      <td>2008</td>
      <td>$2B</td>
      <td>Tiger Global Management, Sequoia Capital China...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Stripe</td>
      <td>$95B</td>
      <td>1/23/14</td>
      <td>Fintech</td>
      <td>San Francisco</td>
      <td>United States</td>
      <td>North America</td>
      <td>2010</td>
      <td>$2B</td>
      <td>Khosla Ventures, LowercaseCapital, capitalG</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Klarna</td>
      <td>$46B</td>
      <td>12/12/11</td>
      <td>Fintech</td>
      <td>Stockholm</td>
      <td>Sweden</td>
      <td>Europe</td>
      <td>2005</td>
      <td>$4B</td>
      <td>Institutional Venture Partners, Sequoia Capita...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Canva</td>
      <td>$40B</td>
      <td>1/8/18</td>
      <td>Internet software &amp; services</td>
      <td>Surry Hills</td>
      <td>Australia</td>
      <td>Oceania</td>
      <td>2012</td>
      <td>$572M</td>
      <td>Sequoia Capital China, Blackbird Ventures, Mat...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Checkout.com</td>
      <td>$40B</td>
      <td>5/2/19</td>
      <td>Fintech</td>
      <td>London</td>
      <td>United Kingdom</td>
      <td>Europe</td>
      <td>2012</td>
      <td>$2B</td>
      <td>Tiger Global Management, Insight Partners, DST...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Instacart</td>
      <td>$39B</td>
      <td>12/30/14</td>
      <td>Supply chain, logistics, &amp; delivery</td>
      <td>San Francisco</td>
      <td>United States</td>
      <td>North America</td>
      <td>2012</td>
      <td>$3B</td>
      <td>Khosla Ventures, Kleiner Perkins Caufield &amp; By...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>JUUL Labs</td>
      <td>$38B</td>
      <td>12/20/17</td>
      <td>Consumer &amp; retail</td>
      <td>San Francisco</td>
      <td>United States</td>
      <td>North America</td>
      <td>2015</td>
      <td>$14B</td>
      <td>Tiger Global Management</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Databricks</td>
      <td>$38B</td>
      <td>2/5/19</td>
      <td>Data management &amp; analytics</td>
      <td>San Francisco</td>
      <td>United States</td>
      <td>North America</td>
      <td>2013</td>
      <td>$3B</td>
      <td>Andreessen Horowitz, New Enterprise Associates...</td>
    </tr>
  </tbody>
</table>
</div>



<details>
  <summary><h4><strong>Hint 1</strong></h4></summary>

Refer to [the content about exploratory data analysis in Python](https://www.coursera.org/learn/go-beyond-the-numbers-translate-data-into-insight/lecture/kfl9b/find-stories-using-the-six-exploratory-data-analysis-practices).

</details>

<details>
  <summary><h4><strong>Hint 2</strong></h4></summary>

Use the function in the `pandas` library that allows you to get a specific number of rows from the top of a DataFrame.
 

</details>

<details>
  <summary><h4><strong>Hint 3</strong></h4></summary>

Use the `head()` function from the `pandas` library. 

</details>

**Question:** What do you think the "Date Joined" column represents?

- The "Date Joined" column represents when the company became a "unicorn," reaching one billion dollars in valuation.

**Question:** What do you think the "Select Investors" column represents?

- The "Select Investors" column represents the top investors in the company.

### Assess the size of the dataset

Get a sense of how large the dataset is. The `size` property that DataFrames have can help.


```python
# How large the dataset is

### YOUR CODE HERE ###

companies.size
```




    10740



**Question:** What do you notice about the size of the dataset?

- The size of the dataset is 10740. This means that there are 10740 values in total across the whole dataset.

### Determine the shape of the dataset

Identify the number of rows and columns in the dataset. The `shape` property that DataFrames have can help.


```python
# Shape of the dataset

### YOUR CODE HERE ###

companies.shape
```




    (1074, 10)



**Question:** What do you notice about the shape of the dataset?

- The shape of the dataset is (1074, 10). The first number, 1074, represents the number of rows (also known as entries). The second number, 10, represents the number of columns. According to this dataset, there are 1074 unicorn companies as of March 2022, and this dataset also shows 10 aspects of each company. 

### Get basic information about the dataset

To further understand what the dataset entails, get basic information about the dataset, including the data type of values in each column. There is more than one way to approach this task. In this instance, use the `info()` function from `pandas`.


```python
# Get information

### YOUR CODE HERE ###

companies.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1074 entries, 0 to 1073
    Data columns (total 10 columns):
     #   Column            Non-Null Count  Dtype 
    ---  ------            --------------  ----- 
     0   Company           1074 non-null   object
     1   Valuation         1074 non-null   object
     2   Date Joined       1074 non-null   object
     3   Industry          1074 non-null   object
     4   City              1058 non-null   object
     5   Country/Region    1074 non-null   object
     6   Continent         1074 non-null   object
     7   Year Founded      1074 non-null   int64 
     8   Funding           1074 non-null   object
     9   Select Investors  1073 non-null   object
    dtypes: int64(1), object(9)
    memory usage: 84.0+ KB


**Question:** What do you notice about the type of data in the `Year Founded` column? Refer to the output from using `info()` above. Knowing the data type of this variable is helpful because it indicates what types of analysis can be done with that variable, how it can be aggregated with other variables, and so on.

- `Dtype` is listed as `int64` in the `Year Founded` column. This means that the year a company was founded is represented as an integer. 

**Question:** What do you notice about the type of data in the `Date Joined` column? Refer to the output from using `info()` above. Knowing the data type of this variable is helpful because it indicates what types of analysis can be done with that variable and how the variable can be transformed to suit specific tasks.

- `Dtype` is listed as `object` for the `Date Joined` column. This means that the date a company became a unicorn is represented as an object. 

## Step 3: Statistical tests

### Find descriptive statistics

Find descriptive statistics and structure your dataset. The `describe()` function from the `pandas` library can help. This function generates statistics for the numeric columns in a dataset.  


```python
### Get descriptive statistics

### YOUR CODE HERE ###

companies.describe()
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
      <th>Year Founded</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1074.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2012.895717</td>
    </tr>
    <tr>
      <th>std</th>
      <td>5.698573</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1919.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2011.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2014.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2016.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2021.000000</td>
    </tr>
  </tbody>
</table>
</div>



**Question:** Based on the table of descriptive stats generated above, what do you notice about the minimum value in the `Year Founded` column? This is important to know because it helps you understand how early the entries in the data begin.

- The minimum value in the `Year Founded` column is 1919. This means that this dataset does not contain data on unicorn companies founded before 1919.

**Question:** What do you notice about the maximum value in the `Year Founded` column? This is important to know because it helps you understand the most recent year captured by the data. 

- The maximum value in the `Year Founded` column is 2021. This means that this dataset does not include data on unicorn companies founded after 2021.

### Convert the `Date Joined` column to datetime

- Use pd.to_datetime() to convert the "Date Joined" column to datetime. 
- Update the column with the converted values.
- Use .info() to confirm that the update actually took place

You can use the `to_datetime()` function from the `pandas` library. This splits each value into year, month, and date components. This is an important step in data cleaning, as it makes the data in this column easier to use in tasks you may encounter. To name a few examples, you may need to compare "date joined" between companies or determine how long it took a company to become a unicorn. Having "date joined" in datetime form would help you complete such tasks.


```python
# Step 1. Use pd.to_datetime() to convert Date Joined column to datetime 
# Step 2. Update the column with the converted values

### YOUR CODE HERE ###

companies["Date Joined"] = pd.to_datetime(companies["Date Joined"])
```


```python
# Use .info() to confirm that the update actually took place

### YOUR CODE HERE ###

companies.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1074 entries, 0 to 1073
    Data columns (total 10 columns):
     #   Column            Non-Null Count  Dtype         
    ---  ------            --------------  -----         
     0   Company           1074 non-null   object        
     1   Valuation         1074 non-null   object        
     2   Date Joined       1074 non-null   datetime64[ns]
     3   Industry          1074 non-null   object        
     4   City              1058 non-null   object        
     5   Country/Region    1074 non-null   object        
     6   Continent         1074 non-null   object        
     7   Year Founded      1074 non-null   int64         
     8   Funding           1074 non-null   object        
     9   Select Investors  1073 non-null   object        
    dtypes: datetime64[ns](1), int64(1), object(8)
    memory usage: 84.0+ KB


### Create a `Year Joined` column

It is common to encounter situations where you will need to compare the year joined with the year founded. The `Date Joined` column does not just have year—it has the year, month, and date. Extract the year component from the `Date Joined` column and add those year components into a new column to keep track of each company's year joined.


```python
# Step 1: Use .dt.year to extract year component from Date Joined column
# Step 2: Add the result as a new column named Year Joined to the DataFrame

### YOUR CODE HERE ###

companies["Year Joined"] = companies["Date Joined"].dt.year
```


```python
# Use .head() to confirm that the new column did get added

### YOUR CODE HERE ###

companies.head()
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
      <th>Company</th>
      <th>Valuation</th>
      <th>Date Joined</th>
      <th>Industry</th>
      <th>City</th>
      <th>Country/Region</th>
      <th>Continent</th>
      <th>Year Founded</th>
      <th>Funding</th>
      <th>Select Investors</th>
      <th>Year Joined</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bytedance</td>
      <td>$180B</td>
      <td>2017-04-07</td>
      <td>Artificial intelligence</td>
      <td>Beijing</td>
      <td>China</td>
      <td>Asia</td>
      <td>2012</td>
      <td>$8B</td>
      <td>Sequoia Capital China, SIG Asia Investments, S...</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>1</th>
      <td>SpaceX</td>
      <td>$100B</td>
      <td>2012-12-01</td>
      <td>Other</td>
      <td>Hawthorne</td>
      <td>United States</td>
      <td>North America</td>
      <td>2002</td>
      <td>$7B</td>
      <td>Founders Fund, Draper Fisher Jurvetson, Rothen...</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>2</th>
      <td>SHEIN</td>
      <td>$100B</td>
      <td>2018-07-03</td>
      <td>E-commerce &amp; direct-to-consumer</td>
      <td>Shenzhen</td>
      <td>China</td>
      <td>Asia</td>
      <td>2008</td>
      <td>$2B</td>
      <td>Tiger Global Management, Sequoia Capital China...</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Stripe</td>
      <td>$95B</td>
      <td>2014-01-23</td>
      <td>Fintech</td>
      <td>San Francisco</td>
      <td>United States</td>
      <td>North America</td>
      <td>2010</td>
      <td>$2B</td>
      <td>Khosla Ventures, LowercaseCapital, capitalG</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Klarna</td>
      <td>$46B</td>
      <td>2011-12-12</td>
      <td>Fintech</td>
      <td>Stockholm</td>
      <td>Sweden</td>
      <td>Europe</td>
      <td>2005</td>
      <td>$4B</td>
      <td>Institutional Venture Partners, Sequoia Capita...</td>
      <td>2011</td>
    </tr>
  </tbody>
</table>
</div>



## Step 4: Results and evaluation

### Take a sample of the data

It is not necessary to take a sample of the data in order to conduct the visualizations and EDA that follow. But you may encounter scenarios in the future where you will need to take a sample of the data due to time and resource limitations. For the purpose of developing your skills around sampling, take a sample of the data and work with that sample for the next steps of analysis you want to conduct. Use the `sample()` function for this task.


```python
# Step 1: Use sample() with the n parameter set to 50 to randomly sample 50 unicorn companies from the data. 
# Specify the random_state parameter so that if you run this cell multiple times, you get the same sample each time. 
# Step 2: Save the result in a new variable.

### YOUR CODE HERE ###

companies_sample = companies.sample(n = 50, random_state = 42)
```

### Visualize the time it took companies to reach unicorn status

Visualize the longest time it took companies to reach unicorn status for each industry represented in the sample. To create a bar plot to visualize this, use the `bar()` function from the `matplotlib.pyplot` module.


```python
# Create bar plot
# with Industry column as the categories of the bars
# and the difference in years between Year Joined column and Year Founded column as the heights of the bars

### YOUR CODE HERE ###

plt.bar(companies_sample["Industry"], companies_sample["Year Joined"] - companies_sample["Year Founded"])

# Set title

### YOUR CODE HERE ###

plt.title("Bar plot of maximum years taken by company to become unicorn per industry (from sample)")

# Set x-axis label

### YOUR CODE HERE ###

plt.xlabel("Industry")

# Set y-axis label

### YOUR CODE HERE ###

plt.ylabel("Maximum number of years")

# Rotate labels on the x-axis as a way to avoid overlap in the positions of the text  

### YOUR CODE HERE ###

plt.xticks(rotation=45, horizontalalignment='right')

# Display the plot

### YOUR CODE HERE ###

plt.show()
```


![png](output_43_0.png)


<details>
  <summary><h4><strong>Hint 1</strong></h4></summary>

Refer to what you have learned about creating bar plots as part of [exploratory data analysis](https://www.coursera.org/learn/go-beyond-the-numbers-translate-data-into-insight/lecture/4k4Vg/eda-using-basic-data-functions-with-python).

</details>

<details>
  <summary><h4><strong>Hint 2</strong></h4></summary>

Use the function in the `matplotlib.pyplot` module that allows you to create a bar plot, specifying the category and height for each bar. 

Use the functions in the `matplotlib.pyplot` module that allow you to set the title, x-axis label, and y-axis label of plots. In that module, there are also functions for rotating the labels on the x-axis and displaying the plot. 

</details>

<details>
  <summary><h4><strong>Hint 3</strong></h4></summary>

Use the `plt.bar()` to create the bar plot, passing in the categories and heights of the bars.

Use `plt.title()`, `plt.xlabel()`, and `plt.ylabel()` to set the title, x-axis label, and y-axis label, respectively. 

Use `plt.xticks()` to rotate labels on the x-axis of a plot. The parameters `rotation=45, horizontalalignment='right'` can be passed in to rotate the labels by 45 degrees and align the labels to the right. 

Use `plt.show()` to display a plot.

</details>

**Question:** What do you observe from this bar plot?

- This bar plot shows that for this sample of unicorn companies, the largest value for maximum time taken to become a unicorn occurred in the Heath and Fintech industries, while the smallest value occurred in the Consumer & Retail industry.

### Visualize the maximum unicorn company valuation per industry

Visualize unicorn companies' maximum valuation for each industry represented in the sample. To create a bar plot to visualize this, use the `bar()` function from the `matplotlib.pyplot` module. Before plotting, create a new column that represents the companies' valuations as numbers (instead of strings, as they're currently represented). Then, use this new column to plot your data.


```python
# Create a column representing company valuation as numeric data

# Create new column
companies_sample['valuation_billions'] = companies_sample['Valuation']
# Remove the '$' from each value
companies_sample['valuation_billions'] = companies_sample['valuation_billions'].str.replace('$', '')
# Remove the 'B' from each value
companies_sample['valuation_billions'] = companies_sample['valuation_billions'].str.replace('B', '')
# Convert column to type int
companies_sample['valuation_billions'] = companies_sample['valuation_billions'].astype('int')
companies_sample.head()
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
      <th>Company</th>
      <th>Valuation</th>
      <th>Date Joined</th>
      <th>Industry</th>
      <th>City</th>
      <th>Country/Region</th>
      <th>Continent</th>
      <th>Year Founded</th>
      <th>Funding</th>
      <th>Select Investors</th>
      <th>Year Joined</th>
      <th>valuation_billions</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>542</th>
      <td>Aiven</td>
      <td>$2B</td>
      <td>2021-10-18</td>
      <td>Internet software &amp; services</td>
      <td>Helsinki</td>
      <td>Finland</td>
      <td>Europe</td>
      <td>2016</td>
      <td>$210M</td>
      <td>Institutional Venture Partners, Atomico, Early...</td>
      <td>2021</td>
      <td>2</td>
    </tr>
    <tr>
      <th>370</th>
      <td>Jusfoun Big Data</td>
      <td>$2B</td>
      <td>2018-07-09</td>
      <td>Data management &amp; analytics</td>
      <td>Beijing</td>
      <td>China</td>
      <td>Asia</td>
      <td>2010</td>
      <td>$137M</td>
      <td>Boxin Capital, DT Capital Partners, IDG Capital</td>
      <td>2018</td>
      <td>2</td>
    </tr>
    <tr>
      <th>307</th>
      <td>Innovaccer</td>
      <td>$3B</td>
      <td>2021-02-19</td>
      <td>Health</td>
      <td>San Francisco</td>
      <td>United States</td>
      <td>North America</td>
      <td>2014</td>
      <td>$379M</td>
      <td>M12, WestBridge Capital, Lightspeed Venture Pa...</td>
      <td>2021</td>
      <td>3</td>
    </tr>
    <tr>
      <th>493</th>
      <td>Algolia</td>
      <td>$2B</td>
      <td>2021-07-28</td>
      <td>Internet software &amp; services</td>
      <td>San Francisco</td>
      <td>United States</td>
      <td>North America</td>
      <td>2012</td>
      <td>$334M</td>
      <td>Accel, Alven Capital, Storm Ventures</td>
      <td>2021</td>
      <td>2</td>
    </tr>
    <tr>
      <th>350</th>
      <td>SouChe Holdings</td>
      <td>$3B</td>
      <td>2017-11-01</td>
      <td>E-commerce &amp; direct-to-consumer</td>
      <td>Hangzhou</td>
      <td>China</td>
      <td>Asia</td>
      <td>2012</td>
      <td>$1B</td>
      <td>Morningside Ventures, Warburg Pincus, CreditEa...</td>
      <td>2017</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create bar plot
# with Industry column as the categories of the bars
# and new valuation column as the heights of the bars

### YOUR CODE HERE ###

plt.bar(companies_sample["Industry"], companies_sample["valuation_billions"])

# Set title

### YOUR CODE HERE ###

plt.title("Bar plot of maximum unicorn company valuation per industry (from sample)")

# Set x-axis label

### YOUR CODE HERE ###

plt.xlabel("Industry")

# Set y-axis label

### YOUR CODE HERE ###

plt.ylabel("Maximum valuation in billions of dollars")

# Rotate labels on the x-axis as a way to avoid overlap in the positions of the text  

### YOUR CODE HERE ###

plt.xticks(rotation=45, horizontalalignment='right')

# Display the plot

### YOUR CODE HERE ###

plt.show()
```


![png](output_50_0.png)


<details>
  <summary><h4><strong>Hint 1</strong></h4></summary>

Refer to what you have learned about creating bar plots as part of [exploratory data analysis](https://www.coursera.org/learn/go-beyond-the-numbers-translate-data-into-insight/lecture/4k4Vg/eda-using-basic-data-functions-with-python).

</details>

<details>
  <summary><h4><strong>Hint 2</strong></h4></summary>

Use the function in the `matplotlib.pyplot` module that allows you to create a bar plot, specifying the category and height for each bar. 

Use the functions in the `matplotlib.pyplot` module that allow you to set the title, x-axis label, and y-axis label of plots. In that module, there are also functions for rotating the labels on the x-axis and displaying the plot. 

</details>

<details>
  <summary><h4><strong>Hint 3</strong></h4></summary>

You can use the `plt.bar()` to create the bar plot, passing in the categories and heights of the bars.

You can use `plt.title()`, `plt.xlabel()`, and `plt.ylabel()` to set the title, x-axis label, and y-axis label, respectively. 

You can use `plt.xticks()` to rotate labels on the x-axis of a plot. The parameters `rotation=45, horizontalalignment='right'` can be passed in to rotate the labels by 45 degrees and align the labels to the right. 

You can use `plt.show()` to display a plot.

</details>

**Question:** What do you observe from this bar plot? 

- This bar plot shows that for this sample of unicorn companies, the highest maximum valuation occurred in the Artificial Intelligence industry, while the lowest maximum valuation occurred in the Cybersecurity industry.

## Considerations

**What are some key takeaways that you learned from this lab?**

- Functions in the `pandas` library can be used to gather characteristics about the data at hand.
  - The `info()` and `describe()` functions were especially useful for gathering basic information about a dataset and finding descriptive statistics, respectively.
- Functions in the `matplotlib.pyplot` module can be used to create visualizations to further understand specific aspects of the data.
  - The `bar()` function allowed you to create bar plots that helped visualize categorical information about the data. You were able to visualize the maximum years to become a unicorn and maximum valuation for each industry represented in the sample taken from the data.

**What findings would you share with others?**

- There are 1074 unicorn companies represented in this dataset.
- Some companies took longer to reach unicorn status but have accrued high valuation as of March 2022. Companies could take longer to hit unicorn status for a number of reasons, including requiring more funding or taking longer to develop a business model. 

**What recommendations would you share with stakeholders based on these findings?**

It may be helpful to focus more on industry specifics. Next steps to consider:
- Identify the main industries that the investing firm is interested in investing in. 
- Select a subset of this data that includes only companies in those industries. 
- Analyze that subset more closely. Determine which companies have higher valuation but do not have as many investors currently. They may be good candidates to consider investing in. 

**References**

Bhat, M.A. (2022, March). [*Unicorn Companies*](https://www.kaggle.com/datasets/mysarahmadbhat/unicorn-companies). 

