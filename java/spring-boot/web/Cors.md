To resolve cross origin errors, we can add the respective headers using the annotation `@CrossOrigin`.
```java
@RestController
@RequestMapping("/api/entities")
@CrossOrigin(
	origins = {"origin1", "origin2"}, // allowed origins 
	methods = { RequestMethod.GET } // allowed methods
) // this is applied to the controller
class EntityController{}
```

If we only specified the annotation "@CrossOrigin"  these are the default values:
```java
@CrossOrigin(
	origins = {"*"} // match all origins
	methods = {RequestMethod.GET, Request.HEAD, Request.POST} // default methods
)
```
