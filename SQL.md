## General
- Always specify the source table when selecting columns (a good habit)
- Can't use the renamed name after AS in your SELECT statement, use the original t1.col  
- count (exp) & count(distinct exp)
![](https://user-images.githubusercontent.com/102558337/176285412-5509257b-5163-4f51-abe6-c197dd3eb8ec.png)
- NOT IN (subquery)-> JOIN subquery_table WHERE IS NULL/NOT NULL  
IN/NOT IN，不能直接接table，要用到subquery （SELECT FROM table)，可以转化为JOIN，并用WHERE IS NULL/NOT NULL  
- 遇到JOIN: 主动clarify用什么JOIN, what key to JOIN ON, and why (练习时写下comment)



## DELETE
DELETE FROM [table_Name]
WHERE [condition]

```
DELETE p1 FROM Person p1, Person p2
WHERE p1.email = p2.email AND p1.id>p2.id
```
## DATEDIFF (between two date values)
DATEDIFF(date1, date2): date1 - date2  
date1 > date2

## DATE_ADD()
DATE_ADD(date, INTERVAL value addunit) 
![](https://user-images.githubusercontent.com/102558337/177012721-313bdb36-b182-4636-b4d9-75f3860be30c.png)  
![](https://user-images.githubusercontent.com/102558337/177012739-7f990201-9161-4b27-80cc-0a4a0b472282.png)



## LIMIT n1, n2
start from row n1+1 to count n2 rows 
![](https://user-images.githubusercontent.com/102558337/175049520-e668ab34-d329-4e50-bc5d-73cccf5dec6d.png)

## DENSE_RANK() (PARTITION BY [] ORDER BY [] ASC/DESC) rename
![](https://user-images.githubusercontent.com/102558337/175118510-c09668ee-2bad-417e-9549-fdc9d7d9ba52.png)
![](https://user-images.githubusercontent.com/102558337/175118600-42616ee9-8108-40aa-99de-97f156c94787.png)

 - rank starts from 1
 - assign the same rank to tie, and then continue with the next consecutive number  
 - **comparison among RANK(), DENSE_RANK(), ROW_NUMBER()**  
 ![](https://user-images.githubusercontent.com/102558337/176566759-cddce8ce-b07d-44de-81dd-31e9582feafb.png)


## IF
IF(condition, val_if_true, val_if_false)  
![](https://user-images.githubusercontent.com/102558337/175315130-6fa2a074-63e9-4d11-9d13-f21d95a0d148.png)

## IFNULL
IFNULL(expression, alter_val): returns the alter_val if expression is null

## CASE 
![](https://user-images.githubusercontent.com/102558337/175699558-a2fdb9c5-bb6b-461a-a9fa-982a19e558f2.png)

## UNION
![](https://user-images.githubusercontent.com/102558337/175783553-efea073f-f131-4e34-bb40-927c149b3f58.png)
- Two tables have the same number of columns
- same data types in the same order

## POWER
POWER(x, y): x^y  

## COALESCE
COALESCE(val1, val2, ..., valn, ...): returns the first value that is not null in the list  

## UPDATE
![](https://user-images.githubusercontent.com/102558337/176744166-981aa979-40a6-4690-b1ca-c39a4e9e719e.png)

![](https://user-images.githubusercontent.com/102558337/176744029-4986b9d0-4cf3-4355-9e84-080bb97d70e9.png)
