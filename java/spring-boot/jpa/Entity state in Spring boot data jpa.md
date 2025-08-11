---
tags:
  - Java
  - Spring-boot
---

## **Persistence context**
[[Persistence Context]]

There are three states for entities
```java
@Setter
@Getter
@Entity // An entity should have the `Entity` annotation
Class User{
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	Integer id;

	String name;
}
```
## **Transient**
Here the *entity* is not handled by the persistence context, we must understand the persistence as a hash map where each key and value is associated to a unique Entity.
When we create a new entity the persistence context is not alert of the current existence of our entity, so this is the default state for our entities **transient**.

```java
User user = new User();
user.setName("My name");
// current state of user entity is transient
```
## **Managed**
An entity changes to **managed** when we `persist` the entity, or in simple words when we create a reference of the entity in the persistence context. For this we use persist of [[EntityManager]].
```java
EntityManager em = context.getBean(EntityManger.class);
User user = new User();
user.setName("My name");
// this is gonna check if the entity is already in the persistence context, if not create a new value is created in the context and the state changes to managed
em.persist(user);

```
When the state changes to `managed` we can't make any operations for our entity because now the persistence context is in charge of analyze and make queries to the database.
If the persist method finished the state change to detached.
## **Detached**
Now, `Detached` state is similar to `Transient` state but the difference is that Detached is only applied to entities that have already been persisted.
```java
User user = em.find(User.class, 1);
// the current state of user is detached
```