---
tags:
  - Java
  - Spring-boot
---

When we need to do a match or equal queries can be difficult to implement them if we are using only the `CrudRepository` [[CrudRepository]] implementation, specially when the queries are complex.
So to fix this issue we can extend other interface called `JpaRepository` this gives us an implementation for fetch methods `findAll findBy` with a specific object called **example**.
Here is an example:
```java
interface EntityRepository extends JpaRepository<Entity, Integer>{}

// use case
void exec(){
	var entity = new Entity();// we create an entity, with this we can specify what attributes we need to match.
	entity.setName("j");

	var Example = Example.of(entity); // we need to create an example object
	// by default when we pass a example object the operation is "equal"
	var entities = entityRepository.findAll(example);
	// select * from entities where name = "j";
}
```
## Includes matcher
We can change the properties of the example object using a matcher.
```java
void exec(){
	var entity = new Entity();// we create an entity, with this we can specify what attributes we need to match.
	entity.setName("j");
	var matcher = ExampleMatcher.matching()
					.withStringMatcher(ExampleMatcher.StringMatcher.CONTAINING);
	var Example = Example.of(entity, matcher);
	// now the operation will be "like" because of the matcher
	var entities = entityRepository.findAll(example);
	// select * from entities where name like "j";
}
```
### Properties
Include the null values of the specified entity.
```java
var matcher = ExampleMatcher.matching()
				.withIncludeNullValues();
/// select * from entities where id is NULL ...
```
Ignore specific attributes
```java
var matcher = ExampleMatcher.matching()
				.withIgnorePaths("id", "name")
				.withIncludeNullValues();
/// select * from entities where id is NULL ...
```
## Limitations
- only equal operations for numbers and dates
- specific support for the database implementation
- no nested properties support