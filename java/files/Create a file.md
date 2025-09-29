---
tags:
  - Java
---
# Write a file
To write some content in a file in java we can use the next:
```java
PrintWriter writer = new PrintWriter("/somePath/fileName", "UTF-8");

writer.println("Hello world"); // writes Hello world and add \n to the end in the file
writer.println(); // writes \n
writer.close(); // close the file
```
