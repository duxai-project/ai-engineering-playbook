# Pandas – Quick Memory (Command + Output)

Note: Some outputs may vary (platform-dependent dtypes).

Pandas is built for working with tabular data (rows and columns). Use it for data cleaning, analysis, joins, time series, and preparing datasets for modeling or visualization.

Related: For plotting with Pandas, see [`02_pandas_plots.md`](02_pandas_plots.md).

## Creating Series

Build 1D labeled arrays and basic access patterns.

```python
>>> s = pd.Series([10, 20, 30, 40], index=['a', 'b', 'c', 'd'])
>>> s
a    10
b    20
c    30
d    40
dtype: int64
```

```python
>>> s['b']  # Access by label
20
```

---

## Creating a DataFrame

Create tabular data from dicts, lists, and custom indexes.

```python
>>> df = pd.DataFrame({
...     "Name": ["Alice", "Bob", "Carol"],
...     "Age": [25, 30, 35],
...     "City": ["NYC", "LA", "Chicago"]
... })
>>> df
    Name  Age     City
0  Alice   25      NYC
1    Bob   30       LA
2  Carol   35  Chicago
```

```python
>>> # From a dictionary with custom index
>>> df = pd.DataFrame({
...     "A": [1, 2, 3],
...     "B": [4, 5, 6]
... }, index=['x', 'y', 'z'])
>>> df
   A  B
x  1  4
y  2  5
z  3  6
```

```python
>>> # From lists
>>> pd.DataFrame([["Alice", 25], ["Bob", 30]], columns=["Name", "Age"])
    Name  Age
0  Alice   25
1    Bob   30
```

---

## DataFrame Attributes

Inspect shape, columns, indexes, types, and summary info.

```python
>>> df.shape  # (rows, columns)
(3, 3)
```

```python
>>> df.columns
Index(['Name', 'Age', 'City'], dtype='object')
```

```python
>>> df.index
RangeIndex(start=0, stop=3, step=1)
```

```python
>>> df.dtypes  # Data types of each column
Name    object
Age      int64
City    object
dtype: object
```

```python
>>> df.info()  # Summary information
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3 entries, 0 to 2
Data columns (total 3 columns):
>>>  #   Column  Non-Null Count  Dtype 
---  ------  --------------  ----- 
 0   Name    3 non-null      object
 1   Age     3 non-null      int64 
 2   City    3 non-null      object
dtypes: int64(1), object(2)
```

---

## Viewing Data

Preview rows and get quick descriptive stats.

```python
>>> df.head(2)  # First 2 rows
    Name  Age City
0  Alice   25  NYC
1    Bob   30   LA
```

```python
>>> df.tail(2)  # Last 2 rows
    Name  Age     City
1    Bob   30       LA
2  Carol   35  Chicago
```

```python
>>> df.describe()  # Statistical summary
             Age
count   3.000000
mean   30.000000
std     5.000000
min    25.000000
25%    27.500000
50%    30.000000
75%    32.500000
max    35.000000
```

---

## Column Selection

Select one or multiple columns and understand return types.

```python
>>> df["Age"]  # Returns Series
0    25
1    30
2    35
Name: Age, dtype: int64
```

```python
>>> df[["Name", "Age"]]  # Returns DataFrame
    Name  Age
0  Alice   25
1    Bob   30
2  Carol   35
```

```python
>>> df.Age  # Attribute-style access
0    25
1    30
2    35
Name: Age, dtype: int64
```

---

## Row Selection

Select rows by label or position with loc/iloc.

```python
>>> df.loc[0]  # Select by label
Name     Alice
Age         25
City       NYC
Name: 0, dtype: object
```

```python
>>> df.loc[0:1]  # Slice by label (inclusive)
    Name  Age City
0  Alice   25  NYC
1    Bob   30   LA
```

```python
>>> df.iloc[0]  # Select by position
Name     Alice
Age         25
City       NYC
Name: 0, dtype: object
```

```python
>>> df.iloc[0:2]  # Slice by position (exclusive)
    Name  Age City
0  Alice   25  NYC
1    Bob   30   LA
```

```python
>>> df.loc[0, "Name"]  # Specific cell by label
'Alice'
```

```python
>>> df.iloc[0, 1]  # Specific cell by position
25
```

---

## Filtering Rows

Filter rows with boolean masks and string conditions.

```python
>>> df[df["Age"] > 28]
    Name  Age     City
1    Bob   30       LA
2  Carol   35  Chicago
```

```python
>>> df[(df["Age"] > 25) & (df["City"] == "LA")]  # Multiple conditions
  Name  Age City
1  Bob   30   LA
```

```python
>>> df[df["Name"].isin(["Alice", "Carol"])]  # Filter by list
    Name  Age     City
0  Alice   25      NYC
2  Carol   35  Chicago
```

```python
>>> df[df["Name"].str.contains("li")]  # String contains
    Name  Age City
0  Alice   25  NYC
```

---

## Adding/Modifying Columns

Create new columns or transform existing ones.

```python
>>> df["Salary"] = [50000, 60000, 70000]
>>> df
    Name  Age     City  Salary
0  Alice   25      NYC   50000
1    Bob   30       LA   60000
2  Carol   35  Chicago   70000
```

```python
>>> df["Age_Plus_10"] = df["Age"] + 10
>>> df
    Name  Age     City  Salary  Age_Plus_10
0  Alice   25      NYC   50000           35
1    Bob   30       LA   60000           40
2  Carol   35  Chicago   70000           45
```

```python
>>> df["Senior"] = df["Age"] > 30
>>> df
    Name  Age     City  Salary  Age_Plus_10  Senior
0  Alice   25      NYC   50000           35   False
1    Bob   30       LA   60000           40   False
2  Carol   35  Chicago   70000           45    True
```

---

## Dropping Columns/Rows

Remove columns or rows by label or position.

```python
>>> df.drop("Age_Plus_10", axis=1)  # Drop column (axis=1)
    Name  Age     City  Salary  Senior
0  Alice   25      NYC   50000   False
1    Bob   30       LA   60000   False
2  Carol   35  Chicago   70000    True
```

```python
>>> df.drop([0, 2], axis=0)  # Drop rows (axis=0)
  Name  Age City  Salary  Age_Plus_10  Senior
1  Bob   30   LA   60000           40   False
```

```python
>>> df.drop(columns=["Senior", "Salary"])  # Alternative syntax
    Name  Age     City  Age_Plus_10
0  Alice   25      NYC           35
1    Bob   30       LA           40
2  Carol   35  Chicago           45
```

---

## Sorting

Sort by values across one or multiple columns.

```python
>>> df.sort_values("Age")  # Ascending
    Name  Age     City
0  Alice   25      NYC
1    Bob   30       LA
2  Carol   35  Chicago
```

```python
>>> df.sort_values("Age", ascending=False)  # Descending
    Name  Age     City
2  Carol   35  Chicago
1    Bob   30       LA
0  Alice   25      NYC
```

```python
>>> df.sort_values(["City", "Age"])  # Multiple columns
    Name  Age     City
2  Carol   35  Chicago
1    Bob   30       LA
0  Alice   25      NYC
```

---

## Handling Missing Data

Detect, drop, and fill missing values.

```python
>>> df = pd.DataFrame({
...     "A": [1, 2, None, 4],
...     "B": [5, None, None, 8],
...     "C": [9, 10, 11, 12]
... })
>>> df
     A    B   C
0  1.0  5.0   9
1  2.0  NaN  10
2  NaN  NaN  11
3  4.0  8.0  12
```

```python
>>> df.isnull()  # Check for missing values
       A      B      C
0  False  False  False
1  False   True  False
2   True   True  False
3  False  False  False
```

```python
>>> df.dropna()  # Drop rows with any NaN
     A    B   C
0  1.0  5.0   9
3  4.0  8.0  12
```

```python
>>> df.dropna(axis=1)  # Drop columns with any NaN
    C
0   9
1  10
2  11
3  12
```

```python
>>> df.fillna(0)  # Fill NaN with value
     A    B   C
0  1.0  5.0   9
1  2.0  0.0  10
2  0.0  0.0  11
3  4.0  8.0  12
```

```python
>>> df["A"].fillna(df["A"].mean())  # Fill with mean
0    1.000000
1    2.000000
2    2.333333
3    4.000000
Name: A, dtype: float64
```

---

## GroupBy Operations

Aggregate data by categories for summaries.

```python
>>> df = pd.DataFrame({
...     "Team": ["A", "A", "B", "B", "B"],
...     "Points": [10, 15, 10, 20, 25],
...     "Assists": [5, 7, 8, 10, 12]
... })
>>> df.groupby("Team").sum()
      Points  Assists
Team                 
A         25       12
B         55       30
```

```python
>>> df.groupby("Team").mean()
      Points  Assists
Team                 
A       12.5      6.0
B       18.3     10.0
```

```python
>>> df.groupby("Team")["Points"].sum()  # Specific column
Team
A    25
B    55
Name: Points, dtype: int64
```

```python
>>> df.groupby("Team").agg({"Points": "sum", "Assists": "mean"})
      Points  Assists
Team                 
A         25      6.0
B         55     10.0
```

---

## Merging DataFrames

Join tables with inner/outer/left merges.

```python
>>> df1 = pd.DataFrame({"Key": ["A", "B", "C"], "Val1": [1, 2, 3]})
>>> df2 = pd.DataFrame({"Key": ["A", "B", "D"], "Val2": [4, 5, 6]})
>>> pd.merge(df1, df2, on="Key", how="inner")  # Inner join
  Key  Val1  Val2
0   A     1     4
1   B     2     5
```

```python
>>> pd.merge(df1, df2, on="Key", how="outer")  # Outer join
  Key  Val1  Val2
0   A   1.0   4.0
1   B   2.0   5.0
2   C   3.0   NaN
3   D   NaN   6.0
```

```python
>>> pd.merge(df1, df2, on="Key", how="left")  # Left join
  Key  Val1  Val2
0   A     1   4.0
1   B     2   5.0
2   C     3   NaN
```

---

## Concatenating DataFrames

Stack or align tables by rows or columns.

```python
>>> df1 = pd.DataFrame({"A": [1, 2], "B": [3, 4]})
>>> df2 = pd.DataFrame({"A": [5, 6], "B": [7, 8]})
>>> pd.concat([df1, df2])  # Vertical concatenation
   A  B
0  1  3
1  2  4
0  5  7
1  6  8
```

```python
>>> pd.concat([df1, df2], ignore_index=True)  # Reset index
   A  B
0  1  3
1  2  4
2  5  7
3  6  8
```

```python
>>> pd.concat([df1, df2], axis=1)  # Horizontal concatenation
   A  B  A  B
0  1  3  5  7
1  2  4  6  8
```

---

## Apply Functions

Apply custom logic to columns, rows, or cells.

```python
>>> df = pd.DataFrame({"A": [1, 2, 3], "B": [4, 5, 6]})
>>> df["A"].apply(lambda x: x * 2)
0    2
1    4
2    6
Name: A, dtype: int64
```

```python
>>> df.apply(lambda x: x.sum())  # Apply to each column
A     6
B    15
dtype: int64
```

```python
>>> df.apply(lambda x: x.sum(), axis=1)  # Apply to each row
0    5
1    7
2    9
dtype: int64
```

```python
>>> df.applymap(lambda x: x ** 2)  # Apply to each element (use .map() in newer versions)
   A   B
0  1  16
1  4  25
2  9  36
```

---

## Pivot Tables

Summarize data with pivoted aggregates.

```python
>>> df = pd.DataFrame({
...     "Date": ["2024-01", "2024-01", "2024-02", "2024-02"],
...     "City": ["NYC", "LA", "NYC", "LA"],
...     "Sales": [100, 150, 200, 250]
... })
>>> df.pivot_table(values="Sales", index="Date", columns="City")
City       LA  NYC
Date              
2024-01  150  100
2024-02  250  200
```

```python
>>> df.pivot_table(values="Sales", index="Date", columns="City", aggfunc="sum")
City       LA  NYC
Date              
2024-01  150  100
2024-02  250  200
```

---

## Reading/Writing Files

Load and export common file formats.

```python
>>> # Read CSV
>>> df = pd.read_csv("file.csv")

>>> # Read with specific options
>>> df = pd.read_csv("file.csv", sep=";", encoding="utf-8", index_col=0)

>>> # Write CSV
>>> df.to_csv("output.csv", index=False)

>>> # Read Excel
>>> df = pd.read_excel("file.xlsx", sheet_name="Sheet1")

>>> # Write Excel
>>> df.to_excel("output.xlsx", index=False)

>>> # Read JSON
>>> df = pd.read_json("file.json")

>>> # Read HTML tables
>>> tables = pd.read_html("https://example.com")
>>> df = tables[0]  # First table
```

---

## Useful String Methods

Vectorized string operations on text columns.

```python
>>> df = pd.DataFrame({"Name": ["alice", "bob", "CAROL"]})
>>> df["Name"].str.upper()
0    ALICE
1      BOB
2    CAROL
Name: Name, dtype: object
```

```python
>>> df["Name"].str.lower()
0    alice
1      bob
2    carol
Name: Name, dtype: object
```

```python
>>> df["Name"].str.capitalize()
0    Alice
1      Bob
2    Carol
Name: Name, dtype: object
```

```python
>>> df["Name"].str.len()
0    5
1    3
2    5
Name: Name, dtype: int64
```

---

## DateTime Operations

Convert and extract parts from datetime columns.

```python
>>> df = pd.DataFrame({"Date": ["2024-01-01", "2024-02-15", "2024-03-20"]})
>>> df["Date"] = pd.to_datetime(df["Date"])
>>> df
        Date
0 2024-01-01
1 2024-02-15
2 2024-03-20
```

```python
>>> df["Date"].dt.year
0    2024
1    2024
2    2024
Name: Date, dtype: int64
```

```python
>>> df["Date"].dt.month
0    1
1    2
2    3
Name: Date, dtype: int64
```

```python
>>> df["Date"].dt.day_name()
0       Monday
1     Thursday
2    Wednesday
Name: Date, dtype: object
```

---

## Resetting and Setting Index

Move between column-based and index-based labels.

```python
>>> df = pd.DataFrame({"A": [1, 2, 3]}, index=["x", "y", "z"])
>>> df.reset_index()
  index  A
0     x  1
1     y  2
2     z  3
```

```python
>>> df.reset_index(drop=True)  # Don't keep old index
   A
0  1
1  2
2  3
```

```python
>>> df = pd.DataFrame({"Name": ["Alice", "Bob"], "Age": [25, 30]})
>>> df.set_index("Name")
       Age
Name      
Alice   25
Bob     30
```

---

## Value Counts

Count unique values and proportions.

```python
>>> df = pd.DataFrame({"Color": ["Red", "Blue", "Red", "Green", "Blue", "Blue"]})
>>> df["Color"].value_counts()
Blue     3
Red      2
Green    1
Name: Color, dtype: int64
```

```python
>>> df["Color"].value_counts(normalize=True)  # Proportions
Blue     0.50
Red      0.33
Green    0.17
Name: Color, dtype: float64
```

---

## Replace Values

Replace values with scalars or mappings.

```python
>>> df = pd.DataFrame({"A": [1, 2, 3, 2, 1]})
>>> df["A"].replace(1, 100)
0    100
1      2
2      3
3      2
4    100
Name: A, dtype: int64
```

```python
>>> df["A"].replace({1: 100, 2: 200})
0    100
1    200
2      3
3    200
4    100
Name: A, dtype: int64
```

---

## Duplicates

Identify and remove duplicate rows.

```python
>>> df = pd.DataFrame({"A": [1, 2, 2, 3], "B": [4, 5, 5, 6]})
>>> df.duplicated()
0    False
1    False
2     True
3    False
dtype: bool
```

```python
>>> df.drop_duplicates()
   A  B
0  1  4
1  2  5
3  3  6
```

```python
>>> df.drop_duplicates(subset=["A"])  # Based on specific column
   A  B
0  1  4
1  2  5
3  3  6
```

---

## Key Ideas

Core takeaways to remember when working with Pandas.
- **Pandas = think in columns.** Most operations work on entire columns at once.
- **Filters look like NumPy masks** using boolean indexing.
- **loc vs iloc:** `loc` uses labels, `iloc` uses integer positions.
- **axis=0 for rows, axis=1 for columns.**
- **Chaining:** Many operations can be chained: `df.groupby().sum().sort_values()`
- **Inplace:** Most methods return a new DataFrame; use `inplace=True` to modify in place.








