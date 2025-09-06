---
tags:
  - Java
  - Spring-boot
---
To get a nested entity in a entity we can use the root. This can be applied to [[Specification API]] and [[Criteria API]]
```java
import jakarta.persistence.criteria.Join;

Join<Entity, RelatedEntity> relatedEntityJoin = root.join("relatedEntity"); // we use the name of the attribute inside Entity
```
Now we can use `relatedEntityJoin` to get the nested attributes and make complex queries
```java
Predicate predicate = cb.equal(relatedEntityJoin.get("id"), id);
```
