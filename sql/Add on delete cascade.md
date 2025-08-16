---
tags:
  - sql
---

If we create a table and we forget to add the on delete cascade statement for a foreign key, we can add it using the next steps.
First we need to remove the constraint `foreign key`.
```sql
ALTER TABLE table_name DROP FOREIGN KEY constraint_name;
```
To catch the constraint name we can use this [[Show constraints]].
Second we need to add the foreign key constraint again but adding our changes.
```sql
ALTER TABLE table_name ADD CONSTRAINT constraint_name
FOREIGN KEY (foreign_key_id) REFERENCES related_table(id) ON DELETE CASCADE;
```
