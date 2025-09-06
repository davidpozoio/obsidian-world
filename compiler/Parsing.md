In this step we define some "rules" for our language how sentences (tokens) are structured
For example if we have the next code:
```lox
var name = "Jo";
```
We want to define that it's not possible to put the keyword `var` after the name identifier.
 ```lox
 //incorrect sintax
 name var = "Jo";
 ```
 A good approach to define this is using syntax trees. A syntax tree expresses the functionality of the code and the "order".
 ![[Pasted image 20250829003856.png]]
 ```lox
 //code version
 var average = (min + max) / 2;
 ```
 In this step we can report the classic syntax errors.