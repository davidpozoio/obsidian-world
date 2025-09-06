---
tags:
  - Java
  - Spring-boot
---
Spring boot starter web give us an annotation to mark a class as a controller,
```java
@RestController
class EntityController{}
```
To specify the general endpoint we use `@RequestMapping`
```java
@RestController
@RequestMapping("/api/entities")
class EntityController{}
```

# @GetMapping
the `@GetMapping` annotation is used to mark and endpoint as `GET` method and we can specify the name of the sub endpoint
```java
@RestController
class EntityController{
	@GetMapping("") // name of the endpoint
	public List<Entity> findAll(){
		return entityRepository.findAll();
	}
}
```

## Variables
We can specify a variable in the get mapping annotation using the next syntax
```java
@RestController
class EntityController{
	@GetMapping("/{id}") // name of the endpoint
	public List<Entity> findAll(@PathVariable Integer id){
		return entityRepository.findAll();
	}
}
```
We use `@PathVariable` to map the `/{id}` to the `Integer id` of our function.
# @PostMapping
We can specify a `POST` method and its endpoint using this annotation, we use in conjunction with `@RequestBody` to map the body.
```java
@PostMapping("")
void save(@RequestBody Entity entity){}
```
# @PutMapping
We can specify a `PUT` method and its endpoint using this annotation, we use in conjunction with `@RequestBody` and `@PathVariable`.
```java
@PutMapping("/{id}")
void update(@PathVariable Integer id, @RequestBody Entity entity){}
```
# @DeleteMapping
We can specify a `DELETE` method and its endpoint using this annotation, we use in conjunction with `@RequestBody` and `@PathVariable`.
```java
@DeleteMapping("/{id}")
void update(@PathVariable Integer id){}
```
# Change status code
To change the status code we can use `@ResponseStatus` annotation
```java
@ResponseStatus(HttpStatus.NO_CONTENT)
@PutMapping("/{id}")
void update(@PathVariable Integer id, @RequestBody Entity entity){}
```
# Throw errors
We use `ResponseStatusException` class to throw errors
```java
throw new ReponseStatusException(HttpStatus.NOT_FOUND, "optional message");
```