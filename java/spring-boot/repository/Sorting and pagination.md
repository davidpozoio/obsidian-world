---
tags:
  - Java
  - Spring-boot
---
# Setup
We need to extend the `JpaRepository`  to have the sorting and pagination properties enabled for findAll methods.
```java
interface EntityRepository extends JpaRepository<Entity, Integer>{}
```
# Sorting
We need to create a sorting object.
```java
Sort sort = Sort.by("name");
```
By default the order is ascending but we can change it
```java
Sort sort = Sort.by("name").descending();
```
Also we can specify multiple attributes
```java
Sort sort = Sort.by("name", "prime"); // order by name, prime;
```
And also we can compose sorts
```java
Sort sort = Sort.by("name").and(Sort.by("price").descending()); // order by name, price descending;
```
Calling with sort
```java
Sort sort = Sort.by("name", "prime");
var entities = entityRepository.findAll(sort);
```
# Pagination
We need to create a ` PageRequest` object.
```java
PageRequest pageRequest = PageRequest.of(pageNumber, limit);
// pageNumber starts from zero
```
## Using pagination
To paginate a list of entities we only passed the pageRequest object, but now the method won't return a plain entity list, but a `Page<T>` object. We use this to retrieve other values like totalPages and the total value of entities.
```java
Page<Entity> page = entityRepository.findAll(pageRequest);
var entities = page.getContent();
var totalPages = page.getTotalPages();
var totalElements = page.getTotalElements();
```
Hibernate detects when it needs to do the count query to get the totalElements.
## Specifying sort in pagination
```java
Sort sort = Sort.by("name");
PageRequest pageRequest = PageRequest.of(pageNumber, limit, sort); // add sort to the end
```