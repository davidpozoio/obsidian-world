---
tags:
  - Java
---

To start any application in debug mode, java uses a jvm debug server, in `vscode` we can specify this using the next configuration in the `launch.json`
```json
{

	"version": "0.2.0",
	"configurations": [
		{
			"type": "java",
			"name": "CourseApplication",
			"request": "launch",
			"mainClass": "com.example.projectname.MainClass",
			"projectName": "projectname",
			"cwd": "${workspaceFolder}"
		}
	]
}
```
Now we can initialize our application in debug mode.
**the first time when the application is stopped by a breakpoint it is delayed for a few seconds**