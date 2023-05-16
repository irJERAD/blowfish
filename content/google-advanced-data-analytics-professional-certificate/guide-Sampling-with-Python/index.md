---
title: "Sampling with Python Guide"
date: 2023-05-16
draft: false
description: "a guide on sampling with python"
showAuthor: true
authors:
  - "jeradacosta"
slug:
tags: ["Google Advanced Data Analytics Professional Certificate", "lab", "jupyter", "python"]
series: ["The Power of Statistics"]
series_order: 11
---

# Sampling with Python

Throughout the following exercises, you will learn to use Python to simulate random sampling and make a point estimate of a population mean based on your sample data. Before starting on this programming exercise, we strongly recommend watching the video lecture and completing the IVQ for the associated topics.

All the information you need for solving this assignment is in this notebook, and all the code you will be implementing will take place within this notebook. 

As we move forward, you can find instructions on how to install required libraries as they arise in this notebook. Before we begin with the exercises and analyzing the data, we need to import all libraries and extensions required for this programming exercise. Throughout the course, we will be using numpy, pandas, scipy stats, and statsmodels for operations, and matplotlib for plotting.


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

We’ll continue with our previous scenario, in which you’re a data professional working for the Department of Education of a large nation. Recall that you’re analyzing data on the literacy rate for each district.

Now imagine that you are asked to *collect* the data on district literacy rates, and that you have limited time to do so. You can only survey 50 randomly chosen districts, instead of the 634 districts included in your original dataset. The goal of your research study is to estimate the mean literacy rate for *all* 634 districts based on your sample of 50 districts. 




## Simulate random sampling

You can use Python to simulate taking a random sample of 50 districts from your dataset. To do this, use`pandas.DataFrame.sample()`. The following arguments in the `sample()` function will help you simulate random sampling: 

*   `n`: Refers to the desired sample size
*   `replace`: Indicates whether you are sampling with or without replacement
*   `random_state`: Refers to the seed of the random number

Reference: https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.sample.html.

**Note:**  A **random seed** is a starting point for generating random numbers. You can use any arbitrary number to fix the random seed, and give the random number generator a starting point. Also, going forward, you can use the same random seed to generate the same set of numbers.

Now you’re ready to write your code. First, name a new variable `sampled_data`. Then, set the arguments for the `sample()` function:  

*   `n`: You're sampling from 50 districts, so your sample size is `50`. 
*   `replace`: For the purpose of our example, you'll sample *with* replacement. `True` indicates sampling with replacement. 
*   `random_state`: Choose an arbitrary number for your random seed. Say, `31208`. 




```python
sampled_data = education_districtwise.sample(n=50, replace=True, random_state=31208)
sampled_data 
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
      <th>661</th>
      <td>DISTRICT528</td>
      <td>STATE6</td>
      <td>9</td>
      <td>112</td>
      <td>89</td>
      <td>1863174.0</td>
      <td>92.14</td>
    </tr>
    <tr>
      <th>216</th>
      <td>DISTRICT291</td>
      <td>STATE28</td>
      <td>14</td>
      <td>1188</td>
      <td>165</td>
      <td>3273127.0</td>
      <td>52.49</td>
    </tr>
    <tr>
      <th>367</th>
      <td>DISTRICT66</td>
      <td>STATE23</td>
      <td>12</td>
      <td>1169</td>
      <td>116</td>
      <td>1042304.0</td>
      <td>62.14</td>
    </tr>
    <tr>
      <th>254</th>
      <td>DISTRICT458</td>
      <td>STATE3</td>
      <td>3</td>
      <td>157</td>
      <td>19</td>
      <td>82839.0</td>
      <td>76.33</td>
    </tr>
    <tr>
      <th>286</th>
      <td>DISTRICT636</td>
      <td>STATE35</td>
      <td>3</td>
      <td>187</td>
      <td>44</td>
      <td>514683.0</td>
      <td>86.70</td>
    </tr>
    <tr>
      <th>369</th>
      <td>DISTRICT512</td>
      <td>STATE23</td>
      <td>6</td>
      <td>589</td>
      <td>30</td>
      <td>717169.0</td>
      <td>68.35</td>
    </tr>
    <tr>
      <th>258</th>
      <td>DISTRICT156</td>
      <td>STATE3</td>
      <td>6</td>
      <td>80</td>
      <td>9</td>
      <td>35289.0</td>
      <td>59.94</td>
    </tr>
    <tr>
      <th>10</th>
      <td>DISTRICT412</td>
      <td>STATE1</td>
      <td>11</td>
      <td>187</td>
      <td>95</td>
      <td>476820.0</td>
      <td>68.69</td>
    </tr>
    <tr>
      <th>512</th>
      <td>DISTRICT277</td>
      <td>STATE9</td>
      <td>10</td>
      <td>558</td>
      <td>179</td>
      <td>2298934.0</td>
      <td>84.31</td>
    </tr>
    <tr>
      <th>144</th>
      <td>DISTRICT133</td>
      <td>STATE21</td>
      <td>14</td>
      <td>1672</td>
      <td>136</td>
      <td>3673849.0</td>
      <td>69.61</td>
    </tr>
    <tr>
      <th>325</th>
      <td>DISTRICT1</td>
      <td>STATE33</td>
      <td>4</td>
      <td>534</td>
      <td>98</td>
      <td>957853.0</td>
      <td>69.37</td>
    </tr>
    <tr>
      <th>227</th>
      <td>DISTRICT159</td>
      <td>STATE28</td>
      <td>18</td>
      <td>870</td>
      <td>134</td>
      <td>2954367.0</td>
      <td>66.23</td>
    </tr>
    <tr>
      <th>86</th>
      <td>DISTRICT667</td>
      <td>STATE25</td>
      <td>5</td>
      <td>396</td>
      <td>75</td>
      <td>896129.0</td>
      <td>82.23</td>
    </tr>
    <tr>
      <th>425</th>
      <td>DISTRICT144</td>
      <td>STATE31</td>
      <td>7</td>
      <td>1064</td>
      <td>108</td>
      <td>2662077.0</td>
      <td>71.59</td>
    </tr>
    <tr>
      <th>260</th>
      <td>DISTRICT305</td>
      <td>STATE3</td>
      <td>2</td>
      <td>62</td>
      <td>6</td>
      <td>145538.0</td>
      <td>69.88</td>
    </tr>
    <tr>
      <th>281</th>
      <td>DISTRICT385</td>
      <td>STATE35</td>
      <td>6</td>
      <td>531</td>
      <td>30</td>
      <td>354972.0</td>
      <td>75.00</td>
    </tr>
    <tr>
      <th>262</th>
      <td>DISTRICT552</td>
      <td>STATE3</td>
      <td>3</td>
      <td>103</td>
      <td>4</td>
      <td>111997.0</td>
      <td>52.23</td>
    </tr>
    <tr>
      <th>253</th>
      <td>DISTRICT168</td>
      <td>STATE3</td>
      <td>5</td>
      <td>312</td>
      <td>16</td>
      <td>176385.0</td>
      <td>82.14</td>
    </tr>
    <tr>
      <th>301</th>
      <td>DISTRICT551</td>
      <td>STATE14</td>
      <td>9</td>
      <td>103</td>
      <td>63</td>
      <td>693281.0</td>
      <td>88.29</td>
    </tr>
    <tr>
      <th>356</th>
      <td>DISTRICT494</td>
      <td>STATE34</td>
      <td>25</td>
      <td>2179</td>
      <td>223</td>
      <td>3596292.0</td>
      <td>70.95</td>
    </tr>
    <tr>
      <th>165</th>
      <td>DISTRICT196</td>
      <td>STATE21</td>
      <td>10</td>
      <td>1354</td>
      <td>119</td>
      <td>1795092.0</td>
      <td>77.52</td>
    </tr>
    <tr>
      <th>565</th>
      <td>DISTRICT308</td>
      <td>STATE17</td>
      <td>8</td>
      <td>721</td>
      <td>144</td>
      <td>848868.0</td>
      <td>86.54</td>
    </tr>
    <tr>
      <th>388</th>
      <td>DISTRICT281</td>
      <td>STATE23</td>
      <td>6</td>
      <td>392</td>
      <td>58</td>
      <td>949159.0</td>
      <td>73.92</td>
    </tr>
    <tr>
      <th>461</th>
      <td>DISTRICT619</td>
      <td>STATE22</td>
      <td>5</td>
      <td>859</td>
      <td>57</td>
      <td>1064989.0</td>
      <td>68.36</td>
    </tr>
    <tr>
      <th>384</th>
      <td>DISTRICT455</td>
      <td>STATE23</td>
      <td>9</td>
      <td>1217</td>
      <td>55</td>
      <td>1063458.0</td>
      <td>68.85</td>
    </tr>
    <tr>
      <th>590</th>
      <td>DISTRICT70</td>
      <td>STATE20</td>
      <td>7</td>
      <td>427</td>
      <td>84</td>
      <td>1846993.0</td>
      <td>80.30</td>
    </tr>
    <tr>
      <th>343</th>
      <td>DISTRICT354</td>
      <td>STATE33</td>
      <td>2</td>
      <td>192</td>
      <td>46</td>
      <td>1260419.0</td>
      <td>88.66</td>
    </tr>
    <tr>
      <th>539</th>
      <td>DISTRICT440</td>
      <td>STATE17</td>
      <td>15</td>
      <td>1465</td>
      <td>167</td>
      <td>2887826.0</td>
      <td>88.23</td>
    </tr>
    <tr>
      <th>459</th>
      <td>DISTRICT431</td>
      <td>STATE22</td>
      <td>9</td>
      <td>1778</td>
      <td>143</td>
      <td>2363744.0</td>
      <td>73.42</td>
    </tr>
    <tr>
      <th>667</th>
      <td>DISTRICT123</td>
      <td>STATE11</td>
      <td>3</td>
      <td>80</td>
      <td>16</td>
      <td>237586.0</td>
      <td>88.49</td>
    </tr>
    <tr>
      <th>387</th>
      <td>DISTRICT231</td>
      <td>STATE23</td>
      <td>6</td>
      <td>657</td>
      <td>63</td>
      <td>530299.0</td>
      <td>64.51</td>
    </tr>
    <tr>
      <th>306</th>
      <td>DISTRICT37</td>
      <td>STATE4</td>
      <td>7</td>
      <td>1083</td>
      <td>92</td>
      <td>642923.0</td>
      <td>68.38</td>
    </tr>
    <tr>
      <th>213</th>
      <td>DISTRICT347</td>
      <td>STATE28</td>
      <td>11</td>
      <td>623</td>
      <td>94</td>
      <td>2228397.0</td>
      <td>59.65</td>
    </tr>
    <tr>
      <th>97</th>
      <td>DISTRICT22</td>
      <td>STATE2</td>
      <td>7</td>
      <td>182</td>
      <td>7</td>
      <td>2531583.0</td>
      <td>87.12</td>
    </tr>
    <tr>
      <th>78</th>
      <td>DISTRICT247</td>
      <td>STATE25</td>
      <td>7</td>
      <td>314</td>
      <td>60</td>
      <td>1332042.0</td>
      <td>72.73</td>
    </tr>
    <tr>
      <th>394</th>
      <td>DISTRICT640</td>
      <td>STATE24</td>
      <td>17</td>
      <td>1857</td>
      <td>191</td>
      <td>1802777.0</td>
      <td>69.00</td>
    </tr>
    <tr>
      <th>184</th>
      <td>DISTRICT596</td>
      <td>STATE21</td>
      <td>11</td>
      <td>1281</td>
      <td>108</td>
      <td>2149066.0</td>
      <td>51.76</td>
    </tr>
    <tr>
      <th>147</th>
      <td>DISTRICT335</td>
      <td>STATE21</td>
      <td>17</td>
      <td>1945</td>
      <td>138</td>
      <td>4380793.0</td>
      <td>69.44</td>
    </tr>
    <tr>
      <th>542</th>
      <td>DISTRICT489</td>
      <td>STATE17</td>
      <td>7</td>
      <td>749</td>
      <td>63</td>
      <td>1198810.0</td>
      <td>85.14</td>
    </tr>
    <tr>
      <th>105</th>
      <td>DISTRICT157</td>
      <td>STATE13</td>
      <td>14</td>
      <td>1994</td>
      <td>508</td>
      <td>3671999.0</td>
      <td>71.68</td>
    </tr>
    <tr>
      <th>254</th>
      <td>DISTRICT458</td>
      <td>STATE3</td>
      <td>3</td>
      <td>157</td>
      <td>19</td>
      <td>82839.0</td>
      <td>76.33</td>
    </tr>
    <tr>
      <th>109</th>
      <td>DISTRICT158</td>
      <td>STATE13</td>
      <td>6</td>
      <td>769</td>
      <td>211</td>
      <td>1338114.0</td>
      <td>66.19</td>
    </tr>
    <tr>
      <th>609</th>
      <td>DISTRICT17</td>
      <td>STATE20</td>
      <td>4</td>
      <td>359</td>
      <td>59</td>
      <td>9588910.0</td>
      <td>88.48</td>
    </tr>
    <tr>
      <th>53</th>
      <td>DISTRICT126</td>
      <td>STATE26</td>
      <td>3</td>
      <td>197</td>
      <td>21</td>
      <td>596294.0</td>
      <td>68.90</td>
    </tr>
    <tr>
      <th>81</th>
      <td>DISTRICT45</td>
      <td>STATE25</td>
      <td>9</td>
      <td>351</td>
      <td>130</td>
      <td>1742815.0</td>
      <td>73.24</td>
    </tr>
    <tr>
      <th>516</th>
      <td>DISTRICT300</td>
      <td>STATE9</td>
      <td>5</td>
      <td>651</td>
      <td>84</td>
      <td>590379.0</td>
      <td>73.29</td>
    </tr>
    <tr>
      <th>641</th>
      <td>DISTRICT484</td>
      <td>STATE6</td>
      <td>15</td>
      <td>333</td>
      <td>83</td>
      <td>1721179.0</td>
      <td>74.92</td>
    </tr>
    <tr>
      <th>650</th>
      <td>DISTRICT145</td>
      <td>STATE6</td>
      <td>11</td>
      <td>489</td>
      <td>100</td>
      <td>1614069.0</td>
      <td>84.09</td>
    </tr>
    <tr>
      <th>70</th>
      <td>DISTRICT99</td>
      <td>STATE25</td>
      <td>4</td>
      <td>279</td>
      <td>43</td>
      <td>558890.0</td>
      <td>83.44</td>
    </tr>
    <tr>
      <th>163</th>
      <td>DISTRICT366</td>
      <td>STATE21</td>
      <td>9</td>
      <td>1330</td>
      <td>86</td>
      <td>1579160.0</td>
      <td>79.99</td>
    </tr>
  </tbody>
</table>
</div>



The output shows 50 districts selected randomly from your dataset. Each has a different literacy rate, but note that row 254 was sampled twice, which is possible because you sampled with replacement. 

### Compute the sample mean

Now that you have your random sample, use the mean function to compute the sample mean. First, name a new variable `estimate1`. Next, use `mean()` to compute the mean for your sample data. 


```python
estimate1 = sampled_data['OVERALL_LI'].mean()
estimate1
```




    74.22359999999999



The sample mean for district literacy rate is about 74.22%. This is a point estimate of the population mean based on your random sample of 50 districts. Remember that the population mean is the literacy rate for *all* districts. Due to sampling variability, the sample mean is usually not exactly the same as the population mean. 



Next, let’s find out what will happen if you compute the sample mean based on another random sample of 50 districts. 

To generate another random sample, name a new variable `estimate2`. Then, set the arguments for the sample function. Once again, `n` is `50` and `replace` is "True." This time, choose a different number for your random seed to generate a different sample: 56,810. Finally, add `mean()` at the end of your line of code to compute the sample mean. 


```python
estimate2 = education_districtwise['OVERALL_LI'].sample(n=50, replace=True, random_state=56810).mean()
estimate2
```




    74.24780000000001



For your second estimate, the sample mean for district literacy rate is about 74.25%. 

Due to sampling variability, this sample mean is different from the sample mean of your previous estimate, 74.22% – but they’re really close.

## The central limit theorem 

Recall that the **central limit theorem** tells you that when the sample size is large enough, the sample mean approaches a normal distribution. And, as you sample more observations from a population, the sample mean gets closer to the population mean. The larger your sample size, the more accurate your estimate of the population mean is likely to be. 

In this case, the population mean is the overall literacy rate for *all* districts in the nation. Earlier, you found that the population mean literacy rate is 73.39%. Based on sampling, your first estimated sample mean was 74.22%, and your second estimate was 74.24%. Each estimate is relatively close to the population mean. 


### Compute the mean of a sampling distribution with 10,000 samples

Now, imagine you repeat the study 10,000 times and obtain 10,000 point estimates of the mean. In other words, you take 10,000 random samples of 50 districts, and compute the mean for each sample. According to the central limit theorem, the mean of your sampling distribution will be roughly equal to the population mean. 



You can use Python to compute the mean of the sampling distribution with 10,000 samples. 

Let’s go over the code step by step: 


1. Create an empty list to store the sample mean from each sample. Name this `estimate_list`.
2. Set up a for-loop with the `range() `function. The `range()` function generates a sequence of numbers from 1 to 10,000. The loop will run 10,000 times, and iterate over each number in the sequence.
3. Specify what you want to do in each iteration of the loop. The `sample()` function tells the computer to take a random sample of 50 districts with replacement–the argument `n` equals `50`, and the argument `replace` equals `True`. The `append() `function adds a single item to an existing list. In this case, it appends the value of the sample mean to each item in the list. Your code generates a list of 10,000 values, each of which is the sample mean from a random sample. 
4. Create a new data frame for your list of 10,000 estimates. Name a new variable `estimate_df` to store your data frame. 







```python
estimate_list = []
for i in range(10000):
    estimate_list.append(education_districtwise['OVERALL_LI'].sample(n=50, replace=True).mean())
estimate_df = pd.DataFrame(data={'estimate': estimate_list})
```

Note that, because you didn't specify a random seed for each loop iteration, by default the rows sampled will be different each time.

Now, name a new variable `mean_sample_means` and compute the mean for your sampling distribution of 10,000 random samples. 


```python
mean_sample_means = estimate_df['estimate'].mean()
mean_sample_means
```




    73.39176734000026



The mean of your sampling distribution is about 73.4%.

Compare this with the population mean of your complete dataset:


```python
population_mean = education_districtwise['OVERALL_LI'].mean()
population_mean
```




    73.39518927444797



The mean of your sampling distribution is essentially identical to the population mean, which is also about 73.4%! 

### Visualize your data

To visualize the relationship between your sampling distribution of 10,000 estimates and the normal distribution, we can plot both at the same time. 

**Note**: The code for this plot is beyond the scope of this course. 

 



```python
plt.hist(estimate_df['estimate'], bins=25, density=True, alpha=0.4, label = "histogram of sample means of 10000 random samples")
xmin, xmax = plt.xlim()
x = np.linspace(xmin, xmax, 100) # generate a grid of 100 values from xmin to xmax.
p = stats.norm.pdf(x, mean_sample_means, stats.tstd(estimate_df['estimate']))
plt.plot(x, p,'k', linewidth=2, label = 'normal curve from central limit theorem')
plt.axvline(x=population_mean, color='g', linestyle = 'solid', label = 'population mean')
plt.axvline(x=estimate1, color='r', linestyle = '--', label = 'sample mean of the first random sample')
plt.axvline(x=mean_sample_means, color='b', linestyle = ':', label = 'mean of sample means of 10000 random samples')
plt.title("Sampling distribution of sample mean")
plt.xlabel('sample mean')
plt.ylabel('density')
plt.legend(bbox_to_anchor=(1.04,1))
plt.show()
```


![png](output_27_0.png)


There are three key takeaways from this graph:

1.  As the central limit theorem predicts, the histogram of the sampling distribution is well approximated by the normal distribution. The outline of the histogram closely follows the normal curve.
2. The mean of the sampling distribution, the blue dotted line, overlaps with the population mean, the green solid line. This shows that the two means are essentially equal to each other.  
3. The sample mean of your first estimate of 50 districts, the red dashed line, is farther away from the center. This is due to sampling variability. 


The central limit theorem shows that as you increase the sample size, your estimate becomes more accurate. For a large enough sample, the sample mean closely follows a normal distribution. 

Your first sample of 50 districts estimated the mean district literacy rate as 74.22%, which is relatively close to the population mean of 73.4%. 

To ensure your estimate will be useful to the government, you can compare the nation’s literacy rate to other benchmarks, such as the global literacy rate, or the literacy rate of peer nations. If the nation’s literacy rate is below these benchmarks, this may help convince the government to devote more resources to improving literacy across the country. 


If you have successfully completed the material above, congratulations! You now understand how to use Python to simulate random sampling and make a point estimate of a population mean. Going forward, you can start using Python to work with your own sample data.
