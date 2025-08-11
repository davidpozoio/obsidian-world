---
tags:
  - Java
  - Spring-boot
---

The entity manager is an interface that gives us methods to interact with the [[Persistence Context]].

## Methods
### persist
The persist method is used when we can `persist` or add an entity to the persistence context.
```java
User user = new User();
user.setName("new name");

em.persist(user);
```
The persist method only can be used when the entity has the transient state or in other words when entity was not still in the persistence context. If you try to execute persist method when the user is already in the persistence context, this is gonna give you an error.
### find
Returns the specified entity and persist it if the entity is not already there.
```java
User user = em.find(User.class, 1);
// the second time, it will use the cache entity
User newUser = em.find(User.class, 1);
```
## merge
The same as persist, but now you can use it when an entity is already in the persistence context.
 ```java
 User user = em.find(User.class, 1); // user has detached state
 user.setName("new name");
 em.merge(user); // we set the new changes
 ```
### detach
Detach the entity from the persistence context manually
```java
em.detach(user);
```
### flush
The flush method execute all the sql queries to the database before the commit.

```java
User user = new User();
em.persist(user);
user.setName("new name"); // this query should be executed to the final of the function
em.flush(); // but with flush() we can execute the sql queries to the database before;
User currentUser = em.find(User.class, user.getId());
currentUser.getName(); // it should show the new name
```