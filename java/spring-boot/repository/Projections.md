---
tags:
  - Java
  - Spring-boot
---
Projections are interfaces (they can be a classes too) that defined what fields of an entity will be fetched.
Example of a projection
```java
interface ArticleProjection{
	public int getId();
	public int getName();
}
```
This projection defined that the article fetched will have an id and a name.

## Setting a projection
To set a project we need to add a method in our repository and returned the projection
### Simple example
```java
interface ArticleRepository extends CrudRepository<Article, Integer>{
	Optional<ArticleProjection> findProjectedById(Integer id); // use projected word it's a standart
}
```
Retrieving an article would be:
```java
void run(){
	ArticleProjection article = articleRepository.findProjectedById(1).orThrowElse();
	// the sql query will be select article.id, article.name from articles where id = 1;
}
```
### Custom queries
This is a wrong query projection, spring boot doesn't handle automatically fetch field for custom queries so we need to do it manually [[Custom Queries]]
```java
interface ArticleRepository extends CrudRepository<Article, Integer>{
	@Query("select a from Article where a.title = :title")
	Optional<ArticleProjection> findArticlesByTitle(@param("title") String title); // this retrieves all the articles fields even if we are using a projection.
}
```
Here we have the corrected version, we need to manually handle the fields for custom queries.
```java
interface ArticleRepository extends CrudRepository<Article, Integer>{
	@Query("select a.id as id, a.name as name from Article a where a.title = :title")
	Optional<ArticleProjection> findArticlesByTitle(@param("title") String title);
}
```
It's important to declare the name of the attribute to do the mapping correctly
```java
// projection          //custom query
getId()                  select u.id as id
```
# Dynamic projections
If we have different projections and we need to use them in different cases, we won't create function for each case, the best thing to do is create a generic function to handle our projections.
```java
interface ArticleRepository extends CrudRepository<Article, Integer>{
	<T> Optional<T> findProjectedById(Integer id, Class<T> projection); // we need to create other attribute because java doesn't allow us to use other sintax.
}
```
To use the projection we can do the next.
```java
void run(){
	ArticleProjection article = articleRepository.findProjectedById(1, ArticleProjection.class).orThrowElse();

// to specify other projection

ArticleIdProjection article2 = articleRepository.findProjectedById(1, ArticleIdProjection.class).orThrowElse(); // only selects the article id
}
```

![[Excalidraw/Projections.excalidraw|1000]]