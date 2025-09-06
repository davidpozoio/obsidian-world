---
tags:
  - Java
  - Spring-boot
---

Give us more abstraction in the [[Criteria API]], useful to create custom filters and queries without much boilerplate.
## Setup
You need to extend the `JpaSpecificationExecutor` to have methods that accept specification value
```java
interface EntityRepository extends JpaRepository<Entity, Integer>, JpaSpecificationExecutor<Entity>{
}
```
After doing this you need to create a class that has the specification methods. An specification method is like a filter or in simple word an specification is like a object that has some "filters" values specified.
```java
class EntitySpec{
	public static Specification<Entity> hasName(String name){
		return (root, query, cb)-> cb.like(root.get("name"), "%" + name + "%");
	}
	
	public static Specification<Entity> hasPriceGreaterThan(BigDecimal price){
		return (root, query, cb)-> cb.greaterThan(root.get("price"), price);
	}
}
```
You specify only a callback because ` Specification<T>` is only an `InterfaceMethod`, here its definition.
```java
interface Specification<T>{
@Nullable
Predicate toPredicate(Root<T> root, @Nullable CriteriaQuery<?> query, CriteriaBuilder criteriaBuilder);
}
```
## Using specification api
To use a specification api we need to create a "base" specification object or an object without any logic still.
```java
void call(){
	Specification<Entity> spec = Specification.unrestricted();
}
```
Now we need to specify more predicates
```java
spec = spec.and(EntitySpec.hasName("name"));
```
We can do it conditionally
```java
if(name != null){
	spec = spec.and(EntitySpec.hasName("name"));
}

if(price != null){
	spec = spec.and(EntitySpec.hasPriceGreaterThan(BigDecimal.valueOf(10)));
}
```
## Apply changes
To apply specification we only need to call the method what we want and pass the spec object.
```java
entityRepository.findAll(spec);
```

## Complete code
```java
//repository
interface EntityRepository extends JpaRepository<Entity, Integer>, JpaSpecificationExecutor<Entity>{
}
//// class spec
class EntitySpec{
	public static Specification<Entity> hasName(String name){
		return (root, query, cb)-> cb.like(root.get("name"), "%" + name + "%");
	}
	
	public static Specification<Entity> hasPriceGreaterThan(BigDecimal price){
		return (root, query, cb)-> cb.greaterThan(root.get("price"), price);
	}
}
// calling the spec


void call(String name){
	Specification<Entity> spec = Specification.unrestricted();
	
	if(name != null){
		spec = spec.and(EntitySpec.hasName("name"));
	}
	
	var entitities = entityRepository.findAll(spec);
}
```
