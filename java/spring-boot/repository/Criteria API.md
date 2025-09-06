---
tags:
  - Java
  - Spring-boot
  - jpql
---

This is other way to create  custom dynamic queries, without using a database approach [[Procedures Spring boot]] or using a fixed method [[Derived queries]].
A difference between [[Custom Queries]] and this approach is: **criteria api** give us dynamic queries.

1. Define the repository
	We need to define a repository for our method that has the criteria api.
```java
interface EntityCriteriaRepository{
	List<Entity> findAllByCriteria(String name, BigDecimal price);
}
```
2. Extends the repository
	Extends the new interface in your entity repository
```java
interface EntityRepository extends CrudRepository<Entity, Integer>, EntityCriteriaRepository {}	
```
3. Implement the custom repository
	We need to implement the new method for this we create a new class with the same name of the custom repository `EntityCriteriaRepository` with an `Impl` suffix `EntityCriteriaRepositoryImpl`
```java
@Repository
class EntityCriteriaRepositoryImpl implements EntityCriteriaRepository {
	@Override
	List<Entity> findAllByCriteria(String name, BigDecimal price){
		// implementation
	}
}
```
# Criteria
To implement our dynamic query we need to ensure that our query is correctly setup.
## CriteriaBuilder
Give us methods to create expressions and queries.
```java
CriteriaBuilder cb = entityManager.getCriterBuilder();
```
### Create CriteriaQuery
We create the body of our query using the method createQuery and we passed the **returned** value
```java
CriteriaQuery<Entity> cq = cb.createQuery(Entity.class); // this query will return entities objects
```
### Create Root
When we create the CriteriaQuery we need to specify **from** where we will get the data, so we use the .from method.
```java
Root<Entity> root = cq.from(Entity.class); // FROM Entity
```
Now at this point our query knows the returned type `Entity` and the origin `root` .
So the next step is creating some predicates
We can get a joined entity using the root value [[Join CriteriaBuilder]]
## Predicates
The predicates are conditions used in queries. CriteriaBuilder gives us methods to create these predicates
To define one we use this
```java
Predicate p = cb.like(root.get("name"), "value"); // Entity.name like "value"
```
### Adding dynamic predicates
```java
List<Predicate> predicates = new ArrayList<>();

if (name != null){
	predicates.add(cb.like(root.get("name", "my name"))); // Entity.name like "my name" 
}

if (price != null){
	predicates.add(cb.greaterThan(root.get("price", 100))); // Entity.price > 100 
}

```
# Specifying the type of the query
Now we define the returned type, the root and the predicates. But spring doesn't still know if the query is a select, update, delete, etc. statement, so we need to specify it.
```java
cq.select(root); // SELECT e FROM Entity;

cq.select(root.get("name")); // SELECT e.name FROM Entity;
// no auto mapping for this directly returns string type
```
## Adding the where
We need to attach the predicates
```java
cq.where(predicates.toArray(new Predicates[predicates.size()]));
// SELECT e FROM Entity WHERE e.name like "my name" AND e.price > 100;
```
# Create the query in the persistence context
```java
entityManage.createQuery(cq).getResultList(); // the returned type depends of the cq type specified.
```

# Complete example
```java
@Override
List<Entity> findByCriteria(String name, BigDecimal price){
	CriteriaBuilder cb = entityManager.getCriteriaBuilder();
	CriteriaQuery<Entity> cq = cb.createQuery(Entity.class);
	Root<Entity> root = cq.from(Entity.class);
	
	List<Predicate> predicates = new ArrayList<>();
	if(name != null){
		predicates.add(cb.like(root.get("name", name)));
	}
	
	if(price != null){
		pridicates.add(cb.greaterThan(root.get("price", price));
	}
	
	cq.select(root);
	cq.where(predicates.toArray(new Predicate[predicates.size()]));
	
	return entityManager.createQuery(cq).getResultList();
}
```
Using Dto's
```java
@AllArgsConstructor
@Getter
@Setter
class PublicProduct{
	Integer id;
	String name;	
}

@Override
public List<PublicProduct> findTestByCriteria() {
	CriteriaBuilder cb = entityManager.getCriteriaBuilder();
	CriteriaQuery<PublicProduct> cq = cb.createQuery(PublicProduct.class);
	Root<Product> root = cq.from(Product.class);
	// we use cb.construct to map the PublicProduct to the root fields
	cq.select(cb.construct(PublicProduct.class, root.get("id"), root.get("name")));

	return entityManager.createQuery(cq).getResultList();
}
```
A simple way to do this is using the [[Specification API]].