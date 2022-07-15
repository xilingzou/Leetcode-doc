# Methods
## loc & iloc functions
**loc** needs to specify the name of the row or column (label-based)  
- selecting data according to some conditions
```
df.loc[(df.brand == 'Maruti') & (df.age>25)]
```
- selecting range of rows
```
df.loc[2:5]  (select rows from 2 to 5, including the last element of the range)
```
- update the value of any column of some conditions
```
df.loc[(df.Year < 2015), ['age']] = 22
```
**iloc** pass integer index to select specific row/column
- select 0th, 2th, 4th and 8th index rows
```
df.iloc[[0, 2, 4, 8]] 
```
- select a range of rows and columns
```
df.iloc[1:5, 2:5]
```
## apply
1. **axis**
- axis = 0, apply function to each column
- axis = 1, apply function to each row


## pd.Sereis.str.split (string or regex to split on: like ',', expand = whether to expand the split strings into seperate columns)
**Returns**
- if expand = True: returns DataFrame with expanded columns
- if expand = False: returns Series containing lists of strings 

## pd.DataFrame.merge (right_df, how={'left','right','outer','inner','cross'}, default 'inner', on = column name or list of column names in both dfs, left_on: the column name in the left df to join on, right_on, sort: whether to sort the join keys)
![](https://user-images.githubusercontent.com/102558337/177630276-a5fe56d2-2f24-454b-8f1d-33df0ddc4739.png)

### Get the index of a row whose column matches values
```
df.index[df['column_name']==value].tolist()
```
returns a list of indexes (actual index labels)

### Iterate over rows in a dataframe
1. iterate over the indexes (df.index)
``` 
for ind in df.index:
  print(df['Name'][index])
```

2. using loc[] function
```
for i in range(len(df)):
  print(df.loc[i, "Name"])
```

3. using iloc[] function
```
for i in range(len(df)):
  print(df.iloc[i, 0])
```

4. using apply()
```
df.apply(lambda row: row["Name"] + " " + str(row["Percentage"]), axis=1) 
```
## split()
string_to_be_splitted.split(seperator, maxsplit)
