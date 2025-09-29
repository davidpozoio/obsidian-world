---
tags:
  - compilers
---
This step is done in the frontend or middle end section ([[Fronted, middle end, backend]]). It's in charge to rewrite our code in a more efficient way, for example we have the concept **constant folding**, this is about pre-calculate an static result a put it directly in the code.
```java
var a = 2 / 2; // our original code
// after constant folding optimization
int a = 1; // put directly the 
```
Now we have more techniques to optimize our code.
After doing all the possible optimizations we can generate our code [[Code generation]].