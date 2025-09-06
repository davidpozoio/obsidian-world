When we want to inject a bean in other we write the next code
```java
@Component
class EntityOne{}

@Component
class EntityTwo{
	final EntityOne entityOne;

	public EntityTwo(EntityOne entityOne){
		this.entityOne = entityOne;
	}
}
```
This boilerplate can be repetitive because we need to specify the variable and initialize the value, but we want to automatize this process using `lombok`.
```java
@Component
class EntityOne{}

@AllArgsConstructor
@Component
class EntityTwo{
	final EntityOne entityOne;
}
```
Now using `@AllArgsConstructor` annotation of lombok we don't need to initialize the injected values.
