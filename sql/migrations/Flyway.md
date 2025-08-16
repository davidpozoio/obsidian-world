---
tags:
  - sql
---
# Spring boot
For spring boot projects, to add our migrations we need to create a folder in the `src/main/resources/db/migration/
# Installing
We can install the maven plugin (to execute the flyway command manually) or the flyway library itself to execute automatically the commands when we start the project.

It's important to remember that we need to add the **flyway driver** for the database.
In this case we'll add the mysql driver.
```xml
<dependency>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-mysql</artifactId>
    <version>11.11.0</version> <!--this version is relative-->
</dependency>
```

## Maven plugin
We need to install the `flyway-maven-plugin`.
```xml
<plugins>
	<plugin>
		<groupId>org.flywaydb</groupId>
		<artifactId>flyway-maven-plugin</artifactId>
		<version>11.11.0</version> <!--This version is relative-->
	</plugin>
</plugins>
```
### Configuration
```xml
<plugins>
	<plugin>
		<groupId>org.flywaydb</groupId>
		<artifactId>flyway-maven-plugin</artifactId>
		<version>11.11.0</version> <!--This version is relative-->
		<configuration>
			<configFiles>src/main/resources/application.properties</configFiles>
			<!--We set where is the file with the database credentials-->
		</configuration>
	</plugin>
</plugins>
```
application.properties
```python
spring.application.name=relations

# configuration used flyway maven plugin
flyway.url=database_url
flyway.username=root
flyway.password=1234

# configuration used for jpa spring boot
spring.datasource.url=${spring.flyway.url}
spring.datasource.username=${spring.flyway.username}
spring.datasource.password=${spring.flyway.password}
```
# Commands
## flyway:info
give us info about our migrations.
```bash
 +----------+---------+-------------+------+--------------+-------+----------+
| Category | Version | Description | Type | Installed On | State | Undoable |
+----------+---------+-------------+------+--------------+-------+----------+
| No migrations found                                                       |
+----------+---------+-------------+------+--------------+-------+----------+
```
## flyway:migrate
flush the migrations to the database. The migration standard name is `V[number version]__[name].sql`.
## flyway:repair
repair the migration history if a migration had an error.