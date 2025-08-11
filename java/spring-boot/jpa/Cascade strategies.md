---
tags:
  - Java
  - Spring-boot
---
When we use relationships we define relations between our entities, and these have different ways to create, delete, find, etc. So we can define how these entities are gonna act.
```java
Class User{
	@OneToOne(cascade=CascadeType.ALL, mappedBy="user")
	Address address;
}
```
## Persist
This cascade strategy is used for create the nested entities, by default when we have a relation between two entities and we creates a new entity this is gonna create an entity, but no their nested entities even if we set the entities in our current state. To fix this we can specify in the nested entities what we want to do, using persist we can create a nested entity automatically.
```java
@Entity
Class User{
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	Integer id;

	String name;

	@OneToOne(mappedBy="user", cascade=CascadeType.PERSIST) // we specify that nested entities are going to be created
	Address address;
}
@Entity
class Address{
	@Id
	Integer id;

	@OneToOne
	@JoinColumn(name="id")
	@MapsId
	User user;
}

/// create an user
@Transactional
void create(){
	User user = new User();
	user.setName("new name");

	Address address = new Address();
	address.setUser(user); // it's important to specify the user because the address id is dependent of user id

	user.setAddress(address); // we set the nested entity

	entityPersistence.persist(user);

}

```
This works for `@OneToMany` relationships too.
## Remove

Delete nested tables. It's specially used for nested one to one entities because for one to many it's used the cascading value of the database, but if not we need to specify manually this value using the remove cascade type.
```java
@Entity
Class User{
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	Integer id;

	String name;

	@OneToMany(mappedBy="user", cascade=CascadeType.PERSIST) // we specify that nested entities are going to be created
	Set<Address> address;
}
@Entity
class Address{
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	Integer id;

	@OneToMany
	@JoinColumn(name="user_id")
	User user;
}
```
In this case we assume that address table has the on delete cascade value set.
```java
/// delete an user
@Transactional
void create(){
	User user = em.find(User.class, 1);

	em.remove(user); // delete the user, if the addresses table have the on cascade delete it's not gonna give us an error.

}
```
This is not going to give us an error if address table  has the on delete cascade value, but if address table doesn't have this value we need to specify in the entity structure manually.
```java
class User{
	// other attributes
	
	@OneToMany(mappedBy="user", cascade={CascadeType.PERSIST, CascadeType.REMOVE}) // we specify remove type
	Set<Address> address;
}

```
With this attribute enabled when we delete the user hibernate is going to do a delete query for addresses by first and later it will delete the user row.
### Delete child entities of parent entity
When we want to delete a child entity of other entity we need to do the same.
```java
class User{
	// other attributes
	
	@OneToMany(mappedBy="user", cascade={CascadeType.PERSIST, CascadeType.REMOVE}) // we specify remove type
	Set<Address> address;
}
```
But now the problem is when we execute the delete statement
```java
@Transactional
void remove(){
	User user = em.find(User.class, 1);
	Address address = user.getAddresses().getFirst(); // we get the first address of this user
	user.getAddresses().delete(address); // we delete the address in the array
	address.setUser(null); // we detach the user from this address

	em.merge(user); // we merge the changes of user
}
```
This should work if the foreign key of address `user_id` can be optional or it can have the null statement but if not we can't set the user to null, so how can we fix this?
Luckily jpa provides us a property to create orphan entities in memory but these entities are gonna be deleted after.
```java
class User{
	// other attributes
	
	@OneToMany(mappedBy="user", cascade={CascadeType.PERSIST, CascadeType.REMOVE}, orphanRemoval=true) // we specify orphanRemoval
	Set<Address> address;
}
```
Now we can delete a child entity without problems.
```java
@Transactional
void remove(){
	User user = em.find(User.class, 1);
	Address address = user.getAddresses().getFirst(); // we get the first address of this user
	user.getAddresses().delete(address); // we delete the address in the array
	address.setUser(null); // we detach the user from this address

	em.merge(user); // we merge the changes of user
}
```
