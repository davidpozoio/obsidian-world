This problem is about how many queries are done to the database.
The most common example is the next
	![[one to many relation.excalidraw|500]]
we have an entity that has many child entities. The problem is the next: 
If we fetch all the users and we fetch all the notes of each user
```java
var users = userRepository.findAll();
for(var user : users){
	var notes = user.getNotes();	
}
```
This is gonna execute the next queries
```sql
select * from users;
select * from notes where user_id = ?;
select * from notes where user_id = ?;
select * from notes where user_id = ?;
..... for each user
```
that's why this is called n + 1 problem.
# Ways to avoid a n + 1 problem
One way is using a custom query and Entity graphs [[Entity Graph]] to retrieve all the necessary data in one sql query.
```java
interface UserRepository extends CrudRepository<User, Integer>{
	@EntityGraph(attributePaths={'notes'})
	@Query(value="select u from User u")
	Iterable<User> findAllWithNotes();
}
```
Now when we use the method `findAllWithNotes` will retrieve the notes at the same time.
```java
var users = userRepository.findAllWithNotes();
for(var user : users){
	var notes = user.getNotes();	
}
```
The done sql queries are these:
```sql
select ...some params from users left join notes; 
```
So here is only one query made.

## Problems using the last solution
The main problem is if the notes table has many records and we need to paginated by user, we can't, so this approach is useful for data that is static or things like admin dashboard, etc.