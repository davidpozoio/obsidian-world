---
tags:
  - Java
  - Spring-boot
---

## PostConstruct
this annotation execute the specified function after injecting the dependencies
```java
// this code is exectued after injecting the dependencies of the class
class Entity{
	public Entity(...dependencies){}
	
	@PostConstruct
	void init() {
		var date = LocalDateTime.now();
		var content = new Content(1, "Course 1", "lorem ipsum", Status.IDEA,     
		Type.COURSE, date, date);
		
		collection.add(content);
	
	}
}
```