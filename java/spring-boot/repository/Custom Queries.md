Allow us to write sql queries using sql or jpql (java persistence query language).
jpql is a language agnostic of the database implementation but it's not so powerful as a sql language.

[[Projections]]

## @Query
We use this annotation in the method that we want to add our custom query.
SQL example, by default jpa uses jpql so we need to specify in the nativeQuery Attribute as true.
It's important to specify `@Modifyng` annotation, because spring boot will treat this method a select.
```java
interface EntityRepository extends CrudRepository<Entity, Integer>{
	@Modifying // we need to this parameter to specify this is an update statement
	@Query(value = "update entity e set e.title = :title where e.user_id = :userId", nativeQuery=true)
	void updateTitleByUserId(@Param("title") String title,@Param("userId") Integer userId);
}
```
Using jpql
```java
interface EntityRepository extends CrudRepository<Entity, Integer>{
	@Modifying // we need to this parameter to specify this is an update statement
	@Query(value = "update Entity e set e.title = :title where e.userId = :userId")
	void updateTitleByUserId(@Param("title") String title,@Param("userId") Integer userId);
}
```
The sintax is similar to sql with the difference that jpql uses the Entities in place of tables.