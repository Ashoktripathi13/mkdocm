## Pandas

Pandas is a Python library used for working with data sets. It has functions for analyzing,cleaning.exploring and manipulating data. Pandas allows us to analyze big data and make conclusions based on statistical theories.Pandas can clean messy data sets, and make them readable and relevant. 

**Import Pandas**<br>
Once Pandas is installed,import it in your applications by adding the import keyword:
```
import pandas as pd 
```
**Check Pandas Version**

```
import pandas as pd
print(pd.__version__)
```
**Pandas Series**

A Pandas Series is like a column in a table. It is a one-dimensional array holding data of any type.
```
import pandas as pd
a=[1,23,34]
var=pd.Series(a)
print(var)
```
## 1.  DataFrames
Data sets in Pandas are usually multi-dimensional tables, called DataFrames.A DataFrame is a data structure that organizes data into a 2-dimensional table of rows and columns.Series is like a column, DataFrame is the whole table.
```
arr = np.array([[1, 2, 3], [4, 5, 6]])
df=pd.DataFrame(arr)
print(df)
```
**Locate Row**

Pandas use the loc attribute to return one or more specified row(s).
```
print(df.loc[0])
```

### - Basic Information about the DataFrame

**df.head() :**  Display the first few rows

**df.info() :** Display information about the DataFrame

**df.describe() :** Display summary statistics

**df.shape :** Display the shape of the DataFrame (rows, columns)

**df.tail() :** display the last few rows.

 ### - Selection and Indexing

**df['ColumnName']:** Selects a single column.
 **df[['Col1', 'Col2']]** selects multiple columns.

**df[start:end]:** Selects rows using slicing (similar to Python lists).

**df.loc[]:** Allows selection by label/index and columns by label.

**df.iloc[]:** Allows selection by index position and columns by index position.

### - Index Basics

**df.set_index('Column'):** Sets the specified column as the index. 

**df.reset_index():** Resets the index to default integer index. 

**df.loc['Label']:** Accesses rows using the index label after setting the index.

## 2. Filtering
 
  - filtering with selection brackets
    ```
    # filtering with selection bracket
    filtered_df=df[df['Age']>30] #filtering people above 30 yrs
    print(filtered_df)
    ```
- isin()
  
  Pandas isin() method is used to filter data frames. isin() method helps in selecting rows with having a particular(or Multiple) value in a particular column. 
  ```
  Syntax: DataFrame.isin(values) 
  ```
- between()
  ``` 
  df.between(left, right, inclusive='both')
  ```

    It return boolean Series equivalent to left <= df <= right.
    This function returns a boolean vector containing True wherever the corresponding Series element is between the boundary values left and right. NA values are treated as False.

- contains()
  
  str.contains() function is used to find if a pattern is present in the strings of the underlying data in the given series object.
    ```
    str.contains(pat) #pat : Character sequence or regular expression. 
    ```
- query()

    Pandas Dataframe provide many methods to filter a Data frame and Dataframe.query() is one of them.Dataframe.query() method only works if the column name doesn’t have any empty spaces.
  ```
  Syntax: DataFrame.query(expr, inplace=False, ) #expr: Expression in string form to filter data.
  ```
  

- loc()
  
  The loc() function helps us to retrieve data values from a dataset at an ease.
  Using the loc() function, we can access the data values fitted in the particular row or column based on the index value passed to the function.
  ```
  pandas.DataFrame.loc[index label]
  ```
- pandas filter()
  
  The Pandas filter() function is used to filter a dataframe based on the column names, rather than the column values, and is useful in creating a subset dataframe containing only those columns in which you are interested.
  ```
  DataFrame.filter(self, items=None, like=None, regex=None, axis=None)
  ```

## 3. Useful Methods

- apply()
  
  The apply() function in Pandas applies a function along a specific axis of a DataFrame.
  ```
  df.apply(func, axis=0)
  ```

- sort_values()
  
  ```
  df.sort_values(by='Column', ascending=True)

  ```
- corr()

  corr() computes the correlation among columns in a DataFrame.
- idxmin() and idxmax()
  
  idxmin() and idxmax() return the index labels where the minimum and maximum values occur.
  ```
  df['Column'].idxmin()
  df['Column'].idxmax()
  ```
- value_counts()
  
  value_counts() returns a Series containing counts of unique values in a column.
  ```
  df['Column'].value_counts()
  ```
- replace()
  
  replace() replaces specified values with other values.
  ```
  df['Column'].replace(to_replace, value)
  ```
- unique() and nunique()
  
  unique() returns unique values in a column, while nunique() returns the number of unique values.
  ```
  df['Column'].unique()
  ```
- map()
  
  map() applies a function to each element of a column.
  ```
  df['Column'].map(func) #func: Function to apply to each element.

  ```

- duplicated() and drop_duplicates()
  
  duplicated() identifies duplicated rows, while drop_duplicates() removes duplicated rows. 
  ```
  df.duplicated()
  df.drop_duplicates()

  ```
- between()

  between() returns a boolean Series indicating whether each element falls between two values.
  ```
  df['Column'].between(start, end)

  ```

## 4. Handling  Missing Data

- Checking for missing values using isnull() and notnull()
  
  In order to check missing values in Pandas DataFrame, we use a function isnull() and notnull(). Both function help in checking whether a value is NaN or not.
  ```
  df.isnull() #return df of Boolean values which are true for NaN values
  df.notnull() #return df of Boolean values which are False for NaN values.
  ```
- Filling missing values using fillna(), replace() and interpolate()
  
  In order to fill null values in a datasets, we use fillna(), replace() and interpolate() function these function replace NaN values with some value of their own. All these function help in filling a null values in datasets of a DataFrame. Interpolate() function is basically used to fill NA values in the dataframe but it uses various interpolation technique to fill the missing values rather than hard-coding the value.
  
  - fillna()
    ```
    # filling missing value using fillna()   
    df.fillna(values) 

    # filling a missing value with  previous ones   
    df.fillna(method ='pad') 

    # Filling null value with the next ones 
    df.fillna(method ='bfill') 

    ```
  - replace()
    ```
    # will replace  Nan value in dataframe with value -1  
    data.replace(to_replace = np.nan, value = -1)  
    ```
  - interpolate()
    ```
    # interpolate the missing values
    df.interpolate(method ='linear', limit_direction ='forward') 
    ```
- Dropping Missing values using Dropna() 
  
  In order to drop a null values from a dataframe, we used dropna() function this function drop Rows/Columns of datasets with Null values in different ways.

  ```
  df.dropna()
  df.dropna(axis = 1) # drop a columns which have at least 1 missing values 
  data.dropna(axis = 0, how ='any') # Dropping Rows with at least 1 null value 
  
  ```
  
  

### References

- https://www.geeksforgeeks.org/pandas-tutorial/
- https://www.w3schools.com/python/pandas/default.asp
- https://www.tutorialspoint.com/python_pandas/index.htm
- https://www.w3resource.com/pandas/dataframe/dataframe-filter.php