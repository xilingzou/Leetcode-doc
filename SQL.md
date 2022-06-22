## DELETE
DELETE FROM [table_Name]
WHERE [condition]

```
DELETE p1 FROM Person p1, Person p2
WHERE p1.email = p2.email AND p1.id>p2.id
```
## DATEDIFF
DATEDIFF(date1, date2): date1 - date2
