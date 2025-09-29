To read a file in java we can use the next code
```java
byte[] bytes = Files.readAllBytes(Paths.get(PATH_FILE));
String text = new String(bytes, Charset.defaultCharset()); // we convert the bytes to string utg-8
```
`Paths.get` convert the provided path to a Path object, by default the `main path` is where the project is saved.