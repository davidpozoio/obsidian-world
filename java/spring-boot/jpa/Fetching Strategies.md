---
tags:
  - Java
  - Spring-boot
---
There are different fetching strategies, these are related how and when we fetched our related data.

## Eager Loading
The related data is fetched immediately when we fetch the main resource.
Applied to `@OneToOne @ManyToOne` by default [[Entity Relationships]].
This strategy is not going to necessary fetch the data in one query, it's probably that it will use two queries, so we probably we need to use [[Entity Graph]] to fix this.
## Lazy Loading
The related data is fetched when we access to the resource.
Applied to `@OneToMany @ManyToMany` by default [[Entity Relationships]].
## Optimization
We can specify that some foreign keys never get null `optional=false`, so in some cases hibernate knows that is not necessary to do a second query. Useful for lazy queries. 
```java
@Entity
class Address{
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	Integer id;

	@OneToOne(fetch = FetchType.LAZY, optional=false)
	@JoinColumn(name="id") // here the id itself is the primary key and the foreign key to user
	User user;
}
```
In this case hibernate must do a second query because id field never can be null, but the foreign key can be, the problem here is the foreign key and primary key are the same so hibernate do a second check for "secure" but it's not necessary, so we need to specify that id is never got to null.