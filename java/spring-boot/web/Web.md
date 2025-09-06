---
tags:
  - Java
  - Spring-boot
---

## Installing
You need to add the spring-boot-starter-web library, this library includes the tomcat servlet.
```xml
<dependencies>
<!--The other dependencies-->
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
	</dependency>
</dependencies>
```

## Executing
You need to execute `./mvnw spring-boot:run`
```bash
2025-09-04T20:10:59.537-05:00  INFO 35443 --- [course] [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port 8080 (http) with context path '/'
2025-09-04T20:10:59.545-05:00  INFO 35443 --- [course] [           main] com.example.course.CourseApplication     : Started CourseApplication in 1.38 seconds (process running for 1.662)
```
## [[Auto restart]]
## [[Controller]]