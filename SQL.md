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
- 可以直接GROUP BY, 在HAVING里出现aggregation function构成筛选条件
```
SELECT
	business_id
FROM Events e
JOIN temp
	ON e.event_type = temp.event_type
GROUP BY business_id
HAVING SUM(CASE
		WHEN temp.avg_activity < e.occurences THEN 1 ELSE 0
	END) > 1
```
- 增加column: select statement; 增加row: UNION
合并两个table：
	- 横向：SELECT * FROM tab1, tab2; JOIN without ON (SELECT * FROM tab1 JOIN tab2)
	- 纵向： UNION
- SELECT distinct rows based on multiple columns: GROUP BY these columns at last
- SUM(CASE WHEN)
- 尽量避免反复JOIN一个table, 考虑简化
- You can use a column in group by that is not selected, but can't select a column but not appear in group by
```
SELECT
	DISTINCT viewer_id AS id
FROM Views 
GROUP BY view_date, viewer_id
HAVING COUNT(DISTINCT article_id)> 1
ORDER BY id
```
- SELECT新创建的column，不便用column name，可以用数字代表 ORDER BY 1, 2
```
SELECT
	CASE
		WHEN continent = 'Asia' THEN name ELSE NUll
	END AS `Asia`
FROM Student
WHERE continent = 'Asia'
ORDER BY 1
```
- ROUND 放在最后一个求值（如AVG）位置
- 注意SUM(CASE WHEN)不能排除duplicates, 要确认是否可以用
- 每个COUNT()注意是否要加DISTINCT （问出来是否有duplicates)
- self join考点 （常考，说出这个term)
- average 除了用aggregation AVG, 还可以直接数量/总数，可能可以免去另写一个table做aggregation
- COUNT(expression) only count the number of non-null values  
![](https://user-images.githubusercontent.com/102558337/179357750-da61439b-2300-4643-ba83-d690a498ff7a.png)
![](https://user-images.githubusercontent.com/102558337/179357771-b4a973c6-b9f8-4bb4-ae75-86f170b78427.png)
- 双向无差别的columns （例如caller and recipient在“通电话”意义上）为了方便可以互换名称合并（union)数据
```
SELECT
	caller_id AS user_id,
	recipient_id
FROM calls
UNION
SELECT
	recipient_id AS user_id,
	caller_id AS recipient_id
FROM calls
```
- Calculate moving average (self join on a range of differences on keys)
```
SELECT
    t1.visited_on,
    SUM(t2.amount) AS amount,
    ROUND(AVG(t2.amount), 2) AS average_amount
FROM temp t1
JOIN temp t2
    ON DATEDIFF(t1.visited_on, t2.visited_on) BETWEEN 0 AND 6
GROUP BY t1.visited_on
HAVING COUNT(t2.visited_on) = 7
```
- 除法int/int结果也是int，转换类型：
1. CAST expr AS float
2. times 1.0
- 同一个属性，多个品类在不同table (如web, mobile)：可以先UNION （建一个新column platform, 其他columns都保持一样）再进行统一select操纵；需要用到CASE WHEN
- 筛选条件多个时可用WHERE+list，WHERE NOT IN/IN ('a','b')
## 面试
1. confirm output
没说明output应该是什么样的情况，和面试官confirm
2. 写题顺序先写  
```
SELECT
FROM table t1
JOIN table t2
```
框架，再写filter conditions (WHERE)，最后补充需要的columns
3. JOIN: address INNER/LEFT JOIN (justification)
4. COUNT时务必想是否有duplicates （思考并发问）： 
- 判断是否有duplicates:  
JOIN ON t1.col = t2.col, 看t2 col里有无duplicates，想是什么情形并confirm是否有
- 不确定就加DISTINCT
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

## DATE_ADD()/DATE_SUB()
DATE_ADD(date, INTERVAL value addunit)  
DATE_SUB(date, INTERVAL value addunit)
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

## ROW_NUMBER/RANK/DENSE_RANK() OVER (PARTITION BY [] ORDER BY [] ASC/DESC) rename
![](https://user-images.githubusercontent.com/102558337/175118510-c09668ee-2bad-417e-9549-fdc9d7d9ba52.png)
![](https://user-images.githubusercontent.com/102558337/175118600-42616ee9-8108-40aa-99de-97f156c94787.png)

 - rank starts from 1
 - assign the same rank to tie, and then continue with the next consecutive number  
 - **comparison among RANK(), DENSE_RANK(), ROW_NUMBER()**  
 ![](https://user-images.githubusercontent.com/102558337/176566759-cddce8ce-b07d-44de-81dd-31e9582feafb.png)
## FIRST_VALUE(expression) OVER (partition by [] ORDER BY[])
## LAG(express, offset, default_val) OVER (PARTITION BY [] ORDER BY [])
- offset: the num of rows back from the current row to get the val
- defualt val: if no preceding rows, return
```
SELECT
	LAG(login_ts, 1) OVER (PARTITION BY cust_id ORDER BY login_ts DESC) AS last_login_ts
FROM t1
```
### SUM(exp) OVER(PARTITION BY __ ORDER BY__)
cumulative sum
```
SELECT
	customer_id,
	order_dt,
	SUM(amount) OVER(PARTITION BY customer_id ORDER BY order_dt) AS cumulative_spend
FROM daily_spent
```
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
可以用作JOIN条件：
```
JOIN t2
	ON COALESCE(t1.c1, t1.c2) = t2.c3
```

## UPDATE
![](https://user-images.githubusercontent.com/102558337/176744166-981aa979-40a6-4690-b1ca-c39a4e9e719e.png)

![](https://user-images.githubusercontent.com/102558337/176744029-4986b9d0-4cf3-4355-9e84-080bb97d70e9.png)

## CONCAT
combine strings or column values together  
![](https://user-images.githubusercontent.com/102558337/177172196-ea6d19a9-9a60-4e13-8738-2c6c2a3e6157.png)

## LEFT JOIN union REIGHT JOIN as a full outer join(no syntax as full outer join)
```
SELECT * FROM t1
LEFT JOIN t2 ON t1.id = t2.id
UNION
SELECT * FROM t1
RIGHT JOIN t2 ON t1.id = t2.id
```
## GREATEST & LEAST (arg1, arg2, arg3...)
return the largest or smallest number among multiple arguments
## test query
```
select concat( 'sanity check on # trip_id: ', 

    case when

    (select count(trip_id) from trip_initiated) = (select count(trip_id) from dispatched_events)

    then 'passed' else 'failed' end);

```
