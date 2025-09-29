---
tags:
  - sql
---

## Mysql
You need the next command in mysql to show the constraints, or more specifically the names.
```sql
SHOW CREATE TABLE table_name;
```
Result profiles table example:
```sql
CREATE TABLE `profiles` (
	`id` int NOT NULL,
	`bio` varchar(255) DEFAULT NULL,
	`phone_number` varchar(255) DEFAULT NULL,
	`date_of_birth` date DEFAULT NULL,
	`loyality_points` int DEFAULT NULL,
	PRIMARY KEY (`id`),
	CONSTRAINT `profiles_ibfk_1` FOREIGN KEY (`id`) REFERENCES `users` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
```