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
