---
tags:
  - Java
---
## Get input
To get the input of a user we can use the next code.
```java
InputStreamReader input = new InputStreamReader(System.in); // we specify where we get from the input data. Read one character at the time.
BufferedReader reader = new BufferedReader(input); // we will read from the input, and give you a convenient readLine method
for(;;){
	String line = reader.readLine();
	if(line == null) break;
}
```
