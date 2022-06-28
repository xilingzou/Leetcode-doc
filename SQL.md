## General
- You can't use the name after AS in your SELECT statement, use t1.col  

## DELETE
DELETE FROM [table_Name]
WHERE [condition]

```
DELETE p1 FROM Person p1, Person p2
WHERE p1.email = p2.email AND p1.id>p2.id
```
## DATEDIFF
DATEDIFF(date1, date2): date1 - date2

## LIMIT n1, n2
start from row n1+1 to count n2 rows 
![](https://user-images.githubusercontent.com/102558337/175049520-e668ab34-d329-4e50-bc5d-73cccf5dec6d.png)

## DENSE_RANK() (PARTITION BY [] ORDER BY [] ASC/DESC) rename
![](https://user-images.githubusercontent.com/102558337/175118510-c09668ee-2bad-417e-9549-fdc9d7d9ba52.png)
![](https://user-images.githubusercontent.com/102558337/175118600-42616ee9-8108-40aa-99de-97f156c94787.png)

 - rank starts from 1

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
