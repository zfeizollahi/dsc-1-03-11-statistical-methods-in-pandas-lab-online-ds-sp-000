
# Statistical Methods in Pandas - Lab

## Introduction

In this lesson you'll get some hands on experience using some of the key summary statistics methods in Pandas.

## Objectives:
You will be able to:

* Understand and use the df.describe() and df.info() summary statistics methods
* Use built-in Pandas methods for calculating summary statistics (.mean(), .std(), .count(), .sum(), .mean(), .median(), .std(), .var() and .quantile())
* Apply a function to every element in a Series or DataFrame using s.apply() and df.applymap()


## Getting Started

For this lab, we'll be working with a dataset containing information on various lego datasets.  You will find this dataset in the file `lego_sets.csv`.  

In the cell below:

* Import pandas and set the standard alias of `pd`
* Load in the `lego_sets.csv`dataset using the `read_csv()` function
* Display the head of the DataFrame to get a feel for what we'll be working with


```python
import pandas as pd
```


```python
df = pd.read_csv('lego_sets.csv')
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
      <th>ages</th>
      <th>list_price</th>
      <th>num_reviews</th>
      <th>piece_count</th>
      <th>play_star_rating</th>
      <th>prod_desc</th>
      <th>prod_id</th>
      <th>prod_long_desc</th>
      <th>review_difficulty</th>
      <th>set_name</th>
      <th>star_rating</th>
      <th>theme_name</th>
      <th>val_star_rating</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6-12</td>
      <td>29.99</td>
      <td>2.0</td>
      <td>277.0</td>
      <td>4.0</td>
      <td>Catapult into action and take back the eggs fr...</td>
      <td>75823.0</td>
      <td>Use the staircase catapult to launch Red into ...</td>
      <td>Average</td>
      <td>Bird Island Egg Heist</td>
      <td>4.5</td>
      <td>Angry Birds™</td>
      <td>4.0</td>
      <td>US</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6-12</td>
      <td>19.99</td>
      <td>2.0</td>
      <td>168.0</td>
      <td>4.0</td>
      <td>Launch a flying attack and rescue the eggs fro...</td>
      <td>75822.0</td>
      <td>Pilot Pig has taken off from Bird Island with ...</td>
      <td>Easy</td>
      <td>Piggy Plane Attack</td>
      <td>5.0</td>
      <td>Angry Birds™</td>
      <td>4.0</td>
      <td>US</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6-12</td>
      <td>12.99</td>
      <td>11.0</td>
      <td>74.0</td>
      <td>4.3</td>
      <td>Chase the piggy with lightning-fast Chuck and ...</td>
      <td>75821.0</td>
      <td>Pitch speedy bird Chuck against the Piggy Car....</td>
      <td>Easy</td>
      <td>Piggy Car Escape</td>
      <td>4.3</td>
      <td>Angry Birds™</td>
      <td>4.1</td>
      <td>US</td>
    </tr>
    <tr>
      <th>3</th>
      <td>12+</td>
      <td>99.99</td>
      <td>23.0</td>
      <td>1032.0</td>
      <td>3.6</td>
      <td>Explore the architecture of the United States ...</td>
      <td>21030.0</td>
      <td>Discover the architectural secrets of the icon...</td>
      <td>Average</td>
      <td>United States Capitol Building</td>
      <td>4.6</td>
      <td>Architecture</td>
      <td>4.3</td>
      <td>US</td>
    </tr>
    <tr>
      <th>4</th>
      <td>12+</td>
      <td>79.99</td>
      <td>14.0</td>
      <td>744.0</td>
      <td>3.2</td>
      <td>Recreate the Solomon R. Guggenheim Museum® wit...</td>
      <td>21035.0</td>
      <td>Discover the architectural secrets of Frank Ll...</td>
      <td>Challenging</td>
      <td>Solomon R. Guggenheim Museum®</td>
      <td>4.6</td>
      <td>Architecture</td>
      <td>4.1</td>
      <td>US</td>
    </tr>
  </tbody>
</table>
</div>



## Getting DataFrame-Level Statistics

We'll begin by getting some overall summary statistics on the dataset.  There are two ways we'll get this information-- `.info()` and `.describe()`.

### Using `.info()`

The `.info()` method provides us metadata on the DataFrame itself.  This allows to answer questions such as:

* What data type does each column contain?

-- either float or objects. Inspecting them visually the objects look like strings. But the objectx look like strings mostly, as is confirmed by type(df.prod_long_desc[0])

* How many rows are in my dataset?

-- 12261 entries
* How many total non-missing values does each column contain?

-- there are 8865 total missing values (across all columns)

* How much memory does the DataFrame take up?

-- 1.3 MB
In the cell below, call our DataFrame's `.info()` method. 


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 12261 entries, 0 to 12260
    Data columns (total 14 columns):
    ages                 12261 non-null object
    list_price           12261 non-null float64
    num_reviews          10641 non-null float64
    piece_count          12261 non-null float64
    play_star_rating     10486 non-null float64
    prod_desc            11884 non-null object
    prod_id              12261 non-null float64
    prod_long_desc       12261 non-null object
    review_difficulty    10206 non-null object
    set_name             12261 non-null object
    star_rating          10641 non-null float64
    theme_name           12258 non-null object
    val_star_rating      10466 non-null float64
    country              12261 non-null object
    dtypes: float64(7), object(7)
    memory usage: 1.3+ MB



```python
type(df.prod_long_desc[0])
```




    str




```python
missing_numreviews = len(df[df.num_reviews.isna() == True])
missing_playstar = len(df[df.play_star_rating.isna() == True])
missing_prod_desc = len(df[df.review_difficulty.isna() == True])
missing_star_rating = len(df[df.star_rating.isna() == True])
missing_val_star_rating = len(df[df.val_star_rating.isna() == True])
total_missing = sum([missing_numreviews, missing_playstar, missing_prod_desc, missing_star_rating, missing_val_star_rating])
print("Total missing values: " + str(total_missing))
```

    Total missing values: 8865


#### Interpreting the Results

Read the output above, and then answer the following questions:

How many total rows are in this DataFrame?  How many columns contain numeric data? How many contain categorical data?  Identify at least 3 columns that contain missing values. 

Write your answer below this line:
________________________________________________________________________________________________________________________________



Answer question here...

## Using `.describe()`

Whereas `.info()` provides statistics about the DataFrame itself, `.describe()` returns output containing basic summary statistics about the data contained with the DataFrame.  

In the cell below, call the DataFrame's `.describe()` method. 


```python
df.describe()
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
      <th>list_price</th>
      <th>num_reviews</th>
      <th>piece_count</th>
      <th>play_star_rating</th>
      <th>prod_id</th>
      <th>star_rating</th>
      <th>val_star_rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>12261.000000</td>
      <td>10641.000000</td>
      <td>12261.000000</td>
      <td>10486.000000</td>
      <td>1.226100e+04</td>
      <td>10641.000000</td>
      <td>10466.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>65.141998</td>
      <td>16.826238</td>
      <td>493.405921</td>
      <td>4.337641</td>
      <td>5.983675e+04</td>
      <td>4.514134</td>
      <td>4.228960</td>
    </tr>
    <tr>
      <th>std</th>
      <td>91.980429</td>
      <td>36.368984</td>
      <td>825.364580</td>
      <td>0.652051</td>
      <td>1.638115e+05</td>
      <td>0.518865</td>
      <td>0.660282</td>
    </tr>
    <tr>
      <th>min</th>
      <td>2.272400</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>6.300000e+02</td>
      <td>1.800000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>19.990000</td>
      <td>2.000000</td>
      <td>97.000000</td>
      <td>4.000000</td>
      <td>2.103400e+04</td>
      <td>4.300000</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>36.587800</td>
      <td>6.000000</td>
      <td>216.000000</td>
      <td>4.500000</td>
      <td>4.206900e+04</td>
      <td>4.700000</td>
      <td>4.300000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>70.192200</td>
      <td>13.000000</td>
      <td>544.000000</td>
      <td>4.800000</td>
      <td>7.092200e+04</td>
      <td>5.000000</td>
      <td>4.700000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1104.870000</td>
      <td>367.000000</td>
      <td>7541.000000</td>
      <td>5.000000</td>
      <td>2.000431e+06</td>
      <td>5.000000</td>
      <td>5.000000</td>
    </tr>
  </tbody>
</table>
</div>



#### Interpreting the Results

The output contains descriptive statistics corresponding to the columns.  Use these to answer the following questions:

How much is the standard deviation for piece count?  How many pieces are in the largest lego set?  How many in the smallest lego set? What is the median `val_star_rating`?

________________________________________________________________________________________________________________________________

Standard Deviation of piece count: 825.36

Largest Lego set: 7541

Smallest Lego set: 1

Val Star Rating: 4.3

## Getting Summary Statistics

Pandas also allows us to easily compute individual summary statistics using built-in methods.  Next, we'll get some practice using these methods. 

In the cell below, compute the median value of the `star_rating` column.


```python
df.star_rating.median()
```




    4.7



Next, get a count of the total number of values in `play_star_rating`.


```python
df.play_star_rating.count()
```




    10486



Now, compute the standard deviation of the `list_price` column.


```python
df.list_price.std()
```




    91.9804293059243



If we bought every single lego set in this dataset, how many pieces would we have?  Use the `.sum()` method on the correct column to compute this. 


```python
df.piece_count.sum()
```




    6049650.0



Now, let's try getting the value for the 90% quantile.  Do this in the cell below.


```python
df.piece_count.quantile(q=0.9)
```




    1077.0



## Getting Summary Statistics on Categorical Data

For obvious reasons, most of the methods we've used so far only work with numerical data--there's no way to calculate the standard deviation of a column containing string values. However, there are some things that we can discover about columns containing categorical data. 

In the cell below, get the `.unique()` values contained within the `review_difficulty` column. 


```python
df.review_difficulty.unique()
```




    array(['Average', 'Easy', 'Challenging', 'Very Easy', nan,
           'Very Challenging'], dtype=object)



Now, let's get the `value_counts` for this column, to see how common each is. 


```python
df.review_difficulty.value_counts()
```




    Easy                4236
    Average             3765
    Very Easy           1139
    Challenging         1058
    Very Challenging       8
    Name: review_difficulty, dtype: int64



As you can see, these provide us quick and easy ways to get information on columns containing categorical information.  


## Using `.applymap()`

When working with pandas DataFrames, we can quickly compute functions on the data contained by using the `applymap()` function and passing in a lambda function. 

For instance, we can use `applymap()` to return a version of the DataFrame where every value has been converted to a string.

In the cell below:

* Call our DataFrame's `.applymap()` function and pass in `lambda x: str(x)`
* Call our new `string_df` object's `.info()` method to confirm that everything has been cast to a string


```python
string_df = df.applymap(lambda x: str(x))
string_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 12261 entries, 0 to 12260
    Data columns (total 14 columns):
    ages                 12261 non-null object
    list_price           12261 non-null object
    num_reviews          12261 non-null object
    piece_count          12261 non-null object
    play_star_rating     12261 non-null object
    prod_desc            12261 non-null object
    prod_id              12261 non-null object
    prod_long_desc       12261 non-null object
    review_difficulty    12261 non-null object
    set_name             12261 non-null object
    star_rating          12261 non-null object
    theme_name           12261 non-null object
    val_star_rating      12261 non-null object
    country              12261 non-null object
    dtypes: object(14)
    memory usage: 1.3+ MB



```python
type(df.prod_desc[0])
```




    str



Note that everything--even the `NaN` values, have been cast to a string in the example above. 

Note that for pandas Series objects (such as a single column in a DataFrame), we can do the same thing using the `apply()` method.  

This is just one example of how we can quickly compute custom functions on our DataFrame--this will become especially useful when we learn how to **_normalize_** our datasets in a later section!

# Conclusion

In this lab, we learned how to:

* Understand and use the df.describe() and df.info() summary statistics methods
* Use built-in Pandas methods for calculating summary statistics (.mean(), .std(), .count(), .sum(), .mean(), .median(), .std(), .var() and .quantile())
* Apply a function to every element in a Series or DataFrame using s.apply() and df.applymap()
