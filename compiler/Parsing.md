---
tags:
  - compilers
---

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
## Representing code
We need a simple structure for our interpreter, but how can we define this, for example when our brain evaluate an arithmetic expression like this:
```java
1 + 2 * 3 - 4
```
Well our brain knows that it needs to resolve the multiplication by first and later do the sum, a simple way to represent this relation is using a tree.
![[Pasted image 20250918215727.png]]
In the bottom of the tree are the nodes that we need to **evaluate** by first and we start resolving our nodes one by one until end in the parent node.
## Context-free grammar (CFG)
This is one the heaviest topics in `formal grammars.
### Formal grammar
A formal grammar is system that has a set of **atomic-pieces** that forms an **alphabet**. Now the "strings" are a sequence of "letters" (atomic-pieces) that are in the **grammar** they normally are infinite.
This can be confusing when we pass from **lexical grammars** to **syntactic grammars**, so I will give you an explanation.
- **lexical grammars**
	The alphabet here are the characters itself, the "strings" are the tokens generated and this is implemented in the scanner.
- **syntactic grammars**
	The alphabet here are the tokens and the strings are **expressions** and this is implemented in the parser.
	
| Terminology               |      Lexical      |   Syntactic |
| :------------------------ | :---------------: | ----------: |
| the alphabet is formed by |    characters     |      tokens |
| the strings are           | lexemes or tokens | expressions |
| it's implemented by       |      scanner      |      parser |
The work of a formal grammar is defined what "strings" are valid and what not.
### Rules for grammars
Now how can we list all the infinite set of valid strings of our language?, well we can't, so a way to do it is creating a set of rules that generates our strings, now this is like a game that we play to generate our strings.
The rules produces strings or they formal name **derivations** because they derive from rules and the rules we called them **producers** because they produce derivations.
#### Producers
The producers are conformed by a head and a body, the head is the name of the rule and the body has a description about what generates.
The body is essentially a list of symbols, now these symbols have two types:
- **terminal**: these symbols are the **letters** in our grammar, in a syntactical grammar can be words like `if`, `return`, etc. They are called terminal because they "end" the play.
- **nonterminal**: these symbols reference the name of other rule, it's like say put the body of the referenced rule here a put the generated string.
We can have many rules or producers with the same name, so when we reach a nonterminal, we can take any of the rules declared with the referenced name.
## BNF (Backus Naur form)
Pāṇini’s _Ashtadhyayi_, which codified Sanskrit grammar a mere couple thousand years ago. Not much progress happened until John Backus and company needed a notation for specifying ALGOL 58 and came up with [**Backus-Naur form**](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form) (**BNF**)

Now our flavor is described in the next form:
- the rules are described by a name in lowercase followed by an arrow.
- the body is next and it ends in semicolon `;`
- terminals are described with `""` and nonterminals with the name of the rule in lowercase
```ebnf
breakfast  → protein "with" breakfast "on the side" ;
breakfast  → protein ;
breakfast  → bread ;

protein    → crispiness "crispy" "bacon" ;
protein    → "sausage" ;
protein    → cooked "eggs" ;

crispiness → "really" ;
crispiness → "really" crispiness ;

cooked     → "scrambled" ;
cooked     → "poached" ;
cooked     → "fried" ;

bread      → "toast" ;
bread      → "biscuits" ;
bread      → "English muffin" ;
```
Our game starts by old convention with the first rule.
Now with this we can generate random breakfasts. Take the first rule and start the "game".
```ebnf
protein "with" breakfast;
#resolve the protein rule
"sausage" "with" breakfast;
# select a breakfast rule
"sausage" "with" cooked "eggs";
# now I need to resolve the cooked rule
"sausage" "with" "fried" "eggs";
# so now I have a plain string, this is our final result.
"sausage with fried eggs"
```
If you noticed we can select breakfast in process again and we can select it again and again, so we have recursion in our grammar, this sign that our grammar is a **context-free grammar**.
### Syntax sugar
To avoid to repeat some "logic" of the rules we can group this in a set of simple syntax sugar.
- In place to write a rule the same rule name for each terminal, we group them using this
```ebnf
rule -> "terminal1";
rule -> "terminal2";

// syntax sugar
rule -> "terminal1" | "terminal2";
// this is like you can select terminal1 or terminal2
```
- Group operator
```ebnf
rule -> ("terminal" | "terminal2") "end.";
// select terminal or terminal2 and concat to the end "end."
```
- Zero or more postfix, write zero or more terminals.
```ebnf
rule -> "";
rule -> "terminal";
rule -> "terminal" rule;
// syntax sugar
rule -> "terminal"*;
// this is like writes zero or more the word terminals
```
- One or more postfix, write at least one or more terminals
```ebnf
rule -> "terminal";
rule -> "terminal" rule;
// syntax sugar
rule -> "terminal"+;
// this is like writes at least one time the word terminal or more
```

- Optional postfix, write zero or one time the terminal but not more.
```ebnf
rule -> "" "end";
rule -> "terminal" "end";
// syntax sugar
rule -> ("terminal")? "end";
```

With all these useful tools we can condense our current rules into this:
```ebnf
breakfast  → (protein "with" breakfast "on the side") | bread ;

protein    → 
	("really"+ "crispy" "bacon") 
	| "sausage" 
	| ("scrambled" | "poached" | "fried")  "eggs";

bread      → "toast" | "biscuits" | "English muffing";
```
We capitalize terminals that represents a lexeme that their text content can vary.
```ebnf
expression -> literal;
literal -> NUMBER | STRING | "true" | "false";
```
### First context-free grammar implementation
This is still ambiguous but it's enough for our first implementation.
This only describes how to create arithmetic expressions and using some operators.
```ebnf
expression -> literal | unary | binary | grouping;

literal -> NUMBER | STRING | "true" | "false" | "nil";

unary -> ( "!" | "-" ) expression;
binary -> expression operator expression;

grouping -> "(" expression ")";
```
## Syntax tree implementation
Now using our basic grammar we can structure our data, how the rules are recursive the data will be a tree. This syntax structure is called a **syntax tree**.
![[Excalidraw/Drawing 2025-09-22 21.53.44.excalidraw |600]]
In java context we can use a recursive class structure to define these relations
```java
public abstract class Expr{
	static public class Binary extends Expr{
		public Binary(Expr left, Token operator, Expr right){}
		final Expr left;
		final Token operator;
		final Expr right;
	}
}
```
### Metaprogramming the trees
Now how the tree class structure are only some types without logic we can automate this process and avoid to write like 21 classes by hand.
The common structure for our classes is a **name** and some **typed fields**. So we will implement a class that is in charge of creating the classes of AST (abstract syntax tree).
#### GenerateAst
We will implement this class to generate the classes of the ast.
```java
class GenerateAst{
	static public void main(String[] args){
		if(args.length != 1) throw Error();
		String outputDir = args[0];
		// we get a outputDir from an arg
		defineAst(outputDir, "Expr", List.of(
			"Binary : Expr left, Token operator Expr right",
			"Unary : Token operator, Expr right",
			"Grouping : Expr expression",
			"Literal : Obj value"
		));
	}
}
```
Now we define a function called `defineAst` this function takes the outputDir the name of the main Classname "Expr" and the list of the subclasses of the main class with their params `Clasname : params` , if you pay attention you noticed that the subclasses are related to our formal grammar implementation. Except for Grouping where **the parenthesis** are excluded, this is because these symbols are not necessary to understand the expression in other words they don't have a semantic meaning as (+, \*, etc.) they are only necessary to help to create the syntax tree.

Here is the defineAst implementation:
```java
void defineAst(String outputDir, String basename, List<String> types){
	PrintWriter writer = new PrintWriter(outputDir + "/" + basename + ".java", "UTF-8");
	
	writer.println("package app;");
	writer.println("import app.scanner.Token;"); //import Token class
	writer.println();
	writer.println("abstract class " + basename + " {"); 
	writer.println();
	for(String type : types){
		String className = type.split(":")[0].trim(); // we get the subclass name
		String fields = type.split(":")[1].trim();// we get the fields of this class
		defineType(writer, basename, className, fields);
	}
	writer.println("}")
}
```
Now we need to define the `defineType` function:
```java
void defineType(PrintWriter writer, String basename, String className, String fields){
	writer.println("  static class " + className + " extends " + basename + " {");
	writer.println("    public " + className + "(" + fields + ")" + "{");
	for(String field : fields.split(", "){ // split the fields
		String name = field.split(" ")[1]; // get the field name
		// we set the value of our defined attributes
		writer.println("      this." + name + " = " + name + ";");
	}
	writer.println("    }");
	// we set the attributes of the class
	for(String field : fields.split(","){
		writer.println("  final "+ field + ";");
	}
	writer.println("}");
} 
```

### The expression problem
Now we have the next structure, the rows are the types and the columns are the methods
![[Pasted image 20250928001713.png]]
How can you see, it's easy to add new rows using this structure but some difficult to add columns because we need to implement the new method in each **existing** type one by one.
Now if we are using a functional paradigm the structure should be like this, the methods of the types should be matched in the body function and if we need to add a new type we create a new function with the pattern matching structure.
![[Pasted image 20250928002128.png]]

This gives us an easy structure to add columns but now it's difficult to add rows because we need to add the new matching case in each **existing** function.
So this problem is called by programmers as **the expression problem** because it's related to represent expression trees. Now we have a design pattern that give us the best of both worlds an easy way to add rows and columns without touching the existing code for this we can use the [[Visitor Pattern]]
### Visitor expressions
We define a Generic type for our visitor, because we can assume that any accept method of the types returns the same. We will implement the Visitor interface inside the base class and their accept method using the same approach of writing in our tool to generate our ast.
```java
// we define a new function to add the Visitor interface
defineVisitor(writer, basename, types);

// implementation
static void defineVisitor(PrintWriter writer, String basename, List<String> types){
	writer.println("interface Visitor<R>{");
	for(String type: types){
		String typeName = type.split(":")[0].trim();
		writer.println("R visit"+typeName + basename + "("+typename + " " + basename.toLowerCase() + ");");
	}
	writer.pritln("}");
}
```
We add the abstract `accept`method in our base class
```java
writer.println(" abstract <R> R accept(Visitor<R> visitor);");
```
Now we need to implement the accept method in each class type.
```java
writer.println("@Override");

writer.println("<R> R accept(Visitor<R> visitor){");

writer.println(" return visitor.visit" + className + basename + "(this);");

writer.println("}");
```
Now we have our visitor pattern implemented, but we need to still to define the Visitor interface itself.
### Pretty printer
Now we need a way to debug our syntax tree, we can use the debugger but it can be difficult to follow, so a common way to handle this is "parsing" our syntax tree to a plane string, but this string should conserve all the relations between the original tree, for that we use parenthesized, this approach is called **pretty printer**.
Given the next syntax tree that relates a binary expression with a unary expression and a grouping:
![[Pasted image 20250928215814.png]]
We need to generate this:
```
(* (- 123) (group 45.67))
```
We handle the relations using parenthesis. the first character next to the left parenthesis is like the name of the function and indicate what operation should be executed to the literal.
So to implement this we create the next:
```java
class AstPrinter implements Expr.Visitor<String>{
	String print(Expr expr){
		return expr.accept(this); // we call the correspoding visitor method for the expression passed.
	}
}
```
Now we need to implement each visitor method
```java
String visitBinaryExpr(Expr.Binary binary){
	return parenthesize(binary.operator.lexeme, binary.left, binary.right);
}
String visitUnaryExpr(Expr.Unary unary){
	return parenthesize(unary.operator.lexeme, unary.expression);
}
String visitGroupingExpr(Expr.Grouping grouping){
	return parenthesize("group", grouping.expression);
}
String visitLiteralExpr(Expr.Literal literal){
	if(literal.value == null) return "nil";
	return literal.value.toString();
}
// we use this function to parse our syntax tree into (name literal)
String parenthesize(String name, Expr... expressions){
	StringBuilder builder = new StringBuilder();
	builder.append("(");
	builder.append(name);
	
	for(var expr : expressions){
		builder.append(" ");
		builder.append(expr.accept(this)); // we apply this visitor to the expression, or in simple words convert to string this expression
	}
	
	builder.append(")");
	
	return builder.toString();
}
```

Now with all these methods defined we can parse the syntax tree to our plane string (pretty print).
![[Pasted image 20250928215814.png]]
To this.
```java
// main
Expr expression = new Expr.Binary(
	new Expr.Unary(
		new Token(TokenType.MINUS, "-", null, 1),
		new Expr.Literal(123)
	),
	new Token(TokenType.STAR, "*", null, 1),
	new Expr.Grouping(new Expr.Literal(45.67))
);

System.out.println(new AstPrinter().print(expression)); // (* (- 123) (group 45.67))
``` 
The next step is [[Static analysis]].
 