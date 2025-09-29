---
tags:
  - sql
---

```mermaid
flowchart LR
user-->|optional 1:1|address
```

There are different ways to create a 1:1 relationship. The key is where should we place the foreign key.
## Foreign key in the owner
This is the most simple way but with some pitfalls.
- First we must delete the dependent table manually
- If the dependent table is optional we must set the foreign key to null
```sql
CREATE TABLE address(
	id INT PRIMARY KEY,
	name VARCHAR(255)
);
CREATE TABLE user(
	id INT PRIMARY KEY,
	name VARCHAR(255),
	address_id INT NOT NULL UNIQUE,
	FOREIGN KEY (address_id) REFERENCES address(id)
);
```
## Foreign key in the dependent
Simple and with some benefits.
- Delete automatically if we specify on delete cascade
- We avoid to set null the foreign key if it's optional
- Keep logical order when we create the tables
```sql
CREATE TABLE user(
	id INT PRIMARY KEY,
	name VARCHAR(255)
);
CREATE TABLE address(
	id INT PRIMARY KEY,
	name VARCHAR(255),
	user_id INT NOT NULL UNIQUE,
	FOREIGN KEY (user_id) REFERENCES user(id) ON DELETE CASCADE
);
```
## Foreign key and primary at the same time in the dependent
Slightly less clear, but useful if we don't want to repeat code and we sure that the dependent table is not going to be independent in future.
```sql
CREATE TABLE user(
	id INT PRIMARY KEY,
	name VARCHAR(255)
);
CREATE TABLE address(
	id INT PRIMARY KEY,
	name VARCHAR(255),
	FOREIGN KEY (id) REFERENCES user(id) ON DELETE CASCADE
);
```