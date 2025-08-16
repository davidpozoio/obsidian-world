---
tags:
  - Java
  - Spring-boot
---
`CrudRepository` is a jpa interface that provides us methods to do different types of queries.
```java
interface EntityRepository extends CrudRepository<Entity, Integer>{
}
```
We can extend the functionally using [[Derived queries]] and [[Custom Queries]].