---
tags:
  - Java
  - Spring-boot
---
We can define derived queries in the repository entity to extend the functionality of the repository [[CrudRepository]], but we must follow some patterns. Spring boot automatically creates the sql for these derived queries.
But this system can be hard to use when we need to write more complex sql queries, so for this type of problems we can write `custom queries` too [[Custom Queries]].
We can combine them with [[Projections]].

```java
public interface SomeEntity extends CrudRepository<Entity, Integer>{
	// we can define derived queries here
}
```
# **Find queries**
We must use the `findBy` keyword to name our functions, and after we can complete the function name with the properties of our entity.
### Basic structure
```java
// returned type   keyword + attribute   params
List<Book>         findByTitle          (String title)
```
### Queries
 ```java
public interface SomeEntity extends CrudRepository<Entity, Integer>{
	List<Book> findByTitle(String title);
	// select * from entities where title = ?*
	List<Book> findByTitleLike(String title)
	// select * from entities where title like ?*
}
 ```
### Queries + operators
  ```java
  public interface SomeEntity extends CrudRepository<Entity, Integer>{
	List<Book> findByTitleAndDescription(String title, String description);
}
 ```
### Greater and less than and equal
```java
  public interface SomeEntity extends CrudRepository<Entity, Integer>{
	List<Book> findByPriceGreaterThan(String title, String description);
	List<Book> findByPriceGreaterThanEqual(String title, String description);
	List<Book> findByPriceLessThan(String title, String description);
	List<Book> findByPriceLessThanEqual(String title, String description);
}
 ```
### Between operator
```java
  public interface SomeEntity extends CrudRepository<Entity, Integer>{
	List<Book> findByPriceBetween(Double min, Double max);
}
 ```
### Order by operator
You only need to include the `OrderByAttribute` to specify the order by default is `asc` but you can change it like this `findByIdOrderByNameDesc`.
Order by operator doesn't need and operator.
```java
public interface SomeEntity extends CrudRepository<Entity, Integer>{
	List<Book> findByPriceBetweenOrderByName(Double min, Double max);
}
 ```

