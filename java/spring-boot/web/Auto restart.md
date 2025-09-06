If you want to auto restart the server when you have changes in your code, you need to install the next library
```xml
<dependencies>
<!--The other dependencies-->
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-devtools</artifactId>
		<optional>true</optional>
	</dependency>
</dependencies>
```
And if you're using `vscode` you need the next configuration enabled in your `settings.json`
```json
{
	"java.configuration.updateBuildConfiguration": "automatic"
}
```