---
tags:
  - Java
---
To create a java project we need to specify a main class, java use classes for all the things
```java
// src/Main.java
public class Main{
	public static void main(String[] args){
		System.out.println("hello");
	}
}
```
This class can have any name (must match with the name file) but the main method must have the `main` name and the args params.
## Compile
To compile our program we need the next command
```bash
javac -d target Main.java
```
`-d` we specify the folder where the artifacts are saved.
To include the other file java classes we use this.
```bash
javac -d target $(find src -name "*.java")
```
This creates artifacts in target folder.
## Execute
To execute a java program we need to specify the `classpath` or where are saved the artifacts and put the name of the class to execute.
```bash
java -cp target Main
```