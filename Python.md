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
