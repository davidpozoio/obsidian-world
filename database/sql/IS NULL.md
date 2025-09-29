---
tags:
  - sql
---

This sql statement allow us to compare NULL value successfully, if we use `=` operator this won't work.
```sql
SELECT * FROM users WHERE role IS NULL;
-- select all users that don't have a role assigned.
```
## Tricks
### optional filter pattern
we can use this property of sql to create `optional` values in queries, for example if we have a condition where we want to filter by month and year but sometimes we don't want to specify the month and only the year we can use this in our procedure. [[Procedures Spring boot]]
```sql
DELIMITER $$

CREATE PROCEDURE findAllUsers(
	month INT, -- this month can be NULL
	year INT -- this year can be NULL
)
BEGIN
	SELECT * FROM users
		WHERE (month IS NULL OR MONTH(created_at) = month)
			AND (year IS NULL OR YEAR(created_at) = year);
END $$

DELIMITER ;

```
Using this structure `(value IS NULL OR statement)` we can create an optional value, first the value is evaluated in the statement but if the value is null this condition will never match any value so we can't fetch any users, but if we specify the second part `or value IS NULL` we are saying if this value is null you can pass or more strictly this condition will return true is the value is NULL so now the query are going to return the users correctly.
This is like "ignoring" the value. It's ignoring the `value` because the value is passed in the procedure this is not going work for the attribute of the table itself.
