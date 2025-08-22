---
tags:
  - Java
  - Spring-boot
---
### Installing
We need to install the `spring-boot-starter-data-jpa` and a driver for our database (this is required), we'll use `mysql-connector-java`.
```xml
<!-- pom.xml -->
<dependencies>
<!--The other dependencies-->
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-data-jpa</artifactId>
	</dependency>
	
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>8.0.33</version> <!--this version is relative-->
	</dependency>
</dependencies>
```
To handle migrations we need to install `flyway`[[Flyway]].
### Topics JPA
- [[Persistence Context]]
- Repository
	- [[Custom Queries]]
	- [[Criteria API]]
	- [[Derived queries]]
	- [[Projections]]
	- [[Avoiding n +1 problem]]
	- [[Procedures Spring boot]]
## Settings
Show sql queries
```python
# application.properties
spring.jpa.show-sql=true
```