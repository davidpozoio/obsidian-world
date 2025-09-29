We use transaction a temporary state where we can set changes in the database and until we **commit** the changes we don't spread the changes globally.
```sql
BEGIN;
	-- do any sql query
COMMIT;
```