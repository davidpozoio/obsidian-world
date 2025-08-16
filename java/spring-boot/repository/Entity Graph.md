This is a concept related to relationships in jpa [[Entity Relationships]].
In simple words is a way to specify that an specific method should retrieve joined data but only for that method.
```java
interface EntityRepository extends CrudRepository<Entity, Integer>{
	@EntityGraph(attributeGraphs={'child'})
	Iterable<Entity> findTestById(Integer id);
}
```
In the `attributeGraphs` you can specify the related entities, the specified entities are going to be loaded in a joined state.
```java
Entity entity = entityRepository.findTestById(1);
// this is gonna do the select including the child entities specified.
// select * from entity left join child where....
```

