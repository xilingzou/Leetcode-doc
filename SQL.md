## General
- Always specify the source table when selecting columns (a good habit)
- Can't use the renamed name after AS in your SELECT statement, use the original t1.col  
- count (exp) & count(distinct exp)
![](https://user-images.githubusercontent.com/102558337/176285412-5509257b-5163-4f51-abe6-c197dd3eb8ec.png)
- NOT IN (subquery)-> JOIN subquery_table WHERE IS NULL/NOT NULL  
IN/NOT IN，不能直接接table，要用到subquery （SELECT FROM table)，可以转化为JOIN，并用WHERE IS NULL/NOT NULL  
- 遇到JOIN: 主动clarify用什么JOIN, what key to JOIN ON, and why (练习时写下comment)
- 条件多的时候，可以在JOIN前filter out vaild rows
- 要求返回top 3 (if less than 3 records then top 2 or top 1), use window functions like ROW_NUMBER, RANK()
- 只能用一个WITH clause, 多个temp table
- GROUP BY will perform on all columns (including those without aggregation functions), and only return one record for each groupby key value
WITH temp1 AS(),
temp2 AS (),
temp3 AS ()
```
WITH available AS
(SELECT *
FROM Books
WHERE available_from <= '2019-05-23'),
available_order AS
(SELECT *
FROM Orders
WHERE dispatch_date <= '2019-06-23'
    AND dispatch_date >= '2018-06-23')

SELECT
    t1.book_id,
    t1.name
FROM available t1
LEFT JOIN available_order t2
    ON  t1.book_id = t2.book_id
GROUP BY t1.book_id
HAVING 
    SUM(quantity) < 10
    OR SUM(quantity) IS NULL
```


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

## YEAR(), MONTH(), DAY()
- YEAR(date)
- MONTH(date)
- DAY(date)

## DATE_FORMAT(date, format)
```
SELECT DATE_FORMAT("2020-11-23", "%M %D %y")
November 23rd 2020

SELECT DATE_FORMAT("2020-11-23", "%M %d %y")
November 23 2020

SELECT DATE_FORMAT("2020/11/23", "%y-%m-%d")
2020-11-23
```
## BETWEEN
expression BETWEEN val1 AND val2
![](https://user-images.githubusercontent.com/102558337/178010507-c71bc2d4-d1c9-4023-bd25-2bd992ce4749.png)  
![](https://user-images.githubusercontent.com/102558337/178010596-5027d572-e97c-4c61-b6dd-4f52a61e2361.png)

## LIMIT n1, n2
start from row n1+1 to count n2 rows 
![](https://user-images.githubusercontent.com/102558337/175049520-e668ab34-d329-4e50-bc5d-73cccf5dec6d.png)
- Also: SELECT * FROM table LIMIT n2 OFFSET n1 (skip the first n1 rows, start from n1+1)

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
- SUM(CASE WHEN condition THEN 1 ELSE 0)
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

## CONCAT
combine strings or column values together  
![](https://user-images.githubusercontent.com/102558337/177172196-ea6d19a9-9a60-4e13-8738-2c6c2a3e6157.png)
