---
title: "Exploratory Data Analysis Using Data Visualization Techniques"
datePublished: Tue Oct 24 2023 05:14:11 GMT+0000 (Coordinated Universal Time)
cuid: clo3vgcx7000109js4y5j8klz
slug: exploratory-data-analysis-using-data-visualization-techniques
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1698120563424/f871de70-a44f-471a-bf5f-17be0ce169a6.jpeg
tags: data-visualisation-1

---

Exploratory data analysis (EDA) is the process of performing initial investigations on data to discover patterns, spot anomalies, test hypotheses, and check assumptions with the help of summary statistics and graphical representations.

EDA enables the practitioners to understand the data first and gather as many insights from it. There is no right or wrong way to conduct an EDA process. The key is to keep an open mind and to test different modeling techniques until new information about the data is uncovered.

> “EDA is an attitude, a state of flexibility, a willingness to look for those things that we believe are not there, as well as those we believe to be there”
> 
> * John Tukey
>     

### Description

Imagine it's just after the end month and your salary, at least, this time was paid on time. You plan a visit to a new city, Honolulu. When you first arrive there, you may not know where to go or what to see. You may start by wandering around aimlessly, but this is not the most efficient way to explore. A better approach is to start by getting a general overview of the city. This can be done by looking at a map, reading a guidebook, or talking to locals. Once you have a basic understanding of the city, you can start to explore specific areas of interest.

EDA is analogous to exploring a new city in these ways:

1. Data Collection - Today, data about different aspects of life is generated in high volumes. Ideally, practitioners take time to secure a wide variety of data around the subject they wish to examine. However, this depends on collecting the required data from various sources through suitable means. Without collecting sufficient and relevant data, further activities cannot begin.
    
2. Data Cleaning - Care should be taken to prepare the data in a way that is clean and can be easily interpreted. Before you start to explore your data, you need to make sure that it is clean and accurate by removing errors, correcting inconsistencies, and filling in missing values. Training messy data will lead to erroneous results which cannot be comprehended even by Data experts. There can be numerous types of issues in the data such as
    
    Missing Values
    
    Presence of Outliers
    
    Invalid form of data type (say instead of entering age as an integer it is present as a string like twenty-one)
    
    Duplicate data
    
    Inconsistent data (for example in cab fare prediction 12 passengers are enlisted passengers’ column which is not possible in a real-world scenario)
    
    Hidden data
    
    Large data
    
3. Data Exploration :
    
    a. Univariate Analysis - Here, the output is a single variable and all data collected is for it. There is no cause-and-effect relationship at all. An individual variable in the dataset is examined. This is done by creating histograms, boxplots, etc.
    
    b. Bivariate Analysis - Two variables are examined at the same time. This can be done by creating scatterplots, correlation matrices, etc.
    
    c. Multivariate Analysis - Three or more variables are examined at the same time. This can be done using a variety of statistical techniques, such as principal component analysis and cluster analysis.
    

You may pause and ask yourself why even bother performing an EDA, after all, we are in the age of AI. While software is useful for spotting typos and grammatical errors, only a critical human eye can detect the nuance.

An EDA is similar in this respect—tools can help you, but it requires our intuition to make sense of it. This personal, in-depth insight will support detailed data analysis further down the line. Effective EDA provides invaluable insights that an algorithm cannot.

### Performing an EDA (Python)

This example will investigate factors that influence customer purchase behavior. We start by importing the necessary libraries.

```python
import NumPy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
```

Then we read in the data as a pandas data frame.

```python
df = pd.read_csv('customer_purchase_history.csv')
```

Here, we could check the data types using;

```python
df.dtypes
```

```python
df['DataFrame Column'].dtypes
```

We can print the first five rows and the last five rows to identify the different features using the following commands respectively.

```python
df.head()
```

```python
df.tail()
```

We can also run the info command to get basic information on the dataset.

```python
df.info()
```

If you are working with integers but they appear with the data type object, removing the quotes for all the values in that column would change them to integer.

Now we can conduct the EDA.

First, we will find the missing values using this command:

```python
print("There are {} missing values in the data.".format(df.isna().sum().sum()))
```

Missing values affect the outcome during analysis. You can replace the null values of a column with the mean of the column and check again to ensure all missing values have been replaced.

```python
df.age.replace(to_replace=np.nan,value=df.age.mean(), inplace=True)
print("There are {} missing values in the data.".format(df.isna().sum().sum()))
```

It is necessary to look for outliers(extreme values) in a variable and identify how many of them have outcomes associated with it.

```python
age_outliers=df[df['age']>60]
age_outliers['age'].shape
```

You can replace the column outliers with the mean. There are numerous methods to deal with missing values and it depends on the dataset and the requirement of the method we choose.

Imputing the missing data with mean: In this method, we replace the missing data with the mean of that column. This method is biased for skewed distributions because for skewed distributions mean is shifted towards the extreme values which leads to improper results in the further processing stage.

Imputing with median: In this method, we replace the missing values with the median of the column. It is better than replacing with mean in the sense that it is not affected by the skewness in the data. It has its limitations though because the imputed data might not be close to the real value as the imputed missing value doesn't need to be equal to the median it might be some extreme value as well.

Imputing with the KNN method: in this method, we use the nearest neighbors of the data to compute its value as opposed to mean and median which works on the statistical parameters. This method computes the mean of its nearest neighbors and replaces the missing value with their mean.

```python
df["age"] = df["age"].apply(lambda x: df.age.mean() if x>60 else x)
```

We use Python's visualization library Seaborn to visualize our data. A seaborn heatmap is used to visualize the correlation. Darker shades represent positive correlation while lighter shades represent negative correlation. If you set annot=True you'll get values by which features are correlated to each other in grid cells. To use Linear Regression for modeling, it is necessary to remove correlated variables to improve the model. To find correlation, use:

```python
.corr()
```

We can use principal component analysis to identify the most important factors that influence customer purchase behavior

```python
from sklearn.decomposition import PCA

pca = PCA(n_components=2)
pca_components = pca.fit_transform(df)

# Create a scatterplot to examine the relationship between the first two principal components
plt.scatter(pca_components[:, 0], pca_components[:, 1])
plt.xlabel('Principal component 1')
plt.ylabel('Principal component 2')
plt.title('Relationship between the first two principal components')
plt.show()
```

A box plot can be used to show the distribution of data in a way that facilitates comparison between the variables. It is a standardized way of displaying the distribution of data based on the five-number summary:

1. **The minimum** (the smallest observation)
    
2. **The lower quartile** (the median of the lower half of the data)
    
3. **The median** (the average/middle value)
    
4. **The upper quartile** (the median of the upper half of the data)
    
5. **The maximum** (the largest observation)
    

### Conclusion

* EDA is subjective as it summarizes the features and characteristics of a dataset. So, depending on the project, data scientists can choose from the various plots to explore the data before applying machine learning algorithms. 
    
* Since the nature of EDA depends on the data, we can say that it is an approach instead of a defined process. 
    
* EDA presents hidden insights from data through visualizations such as graphs and plots.
    
* Graphical and non-graphical statistical methods can be used to perform EDA. 
    
* Univariate analysis is simpler than multivariate analysis.