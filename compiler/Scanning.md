---
tags:
  - compilers
---

Scanning also known as lexing or lexical analysis, it's the step of read a chunk of character a group in "words", these words are called `tokens`.
Some tokens are single characters `( , [` and other are composed by a sequence of characters like numbers, strings, identifiers and keywords (1234) and ("hi") and identifiers (min). Other tokens doesn't have any meaning like white space.
```lox
// code
var average = (min + max) / 2;
```
**![[Pasted image 20250829002150.png]]**
The next step is [[Parsing]].
## Difference between lexeme and token
A lexeme is actually a substring of a source, while a token is the "type" or abstract category that belongs that lexeme.
```java
var a = 2;
```
The lexemes of this string are `var, a, =, 2, ;` and the tokens are
- `"var"` → `KEYWORD(VAR)`
- `"a"` → `IDENTIFIER(name="x")`
- `"="` → `ASSIGN_OP`
- `"2"` → `NUMBER(value=42)`
- `";"` → `SEMICOLON`
## Token structure
The token structure is composed by three important parts: type, lexeme and literal.
- The type categorize what role the lexeme will do.
- The lexeme is only the substring that represent that token
- The literal is the value (if it's present) that can store that token.
## Regular languages and expressions
The rules that are used to group the tokens of a source are called **lexical grammar** a, now the formalism to name this is `regular language`.
When we're saying tokens we refer to recognize the lexemes of a source a give the abstract category (token).
### Lexical grammar
Now the lexical grammar are the rules that we used to group tokens, but these rules are written in regex or EBNF, and we can implement them using code or some automatic parsing.
This is the lexical grammar
```EBNF
Identifier -> Letter (Digit | Letter)+
String -> Letter+
Number -> Digit+
```
This is a implementation
```java
default:
	if(isDigit(c)) // digit logic
	if(isAlpha(c)) // string or identifier logic 
```
## Functionality
An scanner is basically a loop that iterates in a string, reads character by character and when the lexeme is defined, throws a token. So an scanner is a machine that reads characters and sometimes throws tokens.
![[Excalidraw/Drawing 2025-09-14 23.48.56.excalidraw|500]]
We use this variables to keep track of where our scanner is.
```java
// Scanner.java
// default values
int start = 0;
int current = 0;
int line = 0;
```
![[Excalidraw/Drawing 2025-09-15 00.01.20.excalidraw|700]]
### Utils
These functions are basic functions that help us to consume lexemes.
- **isAtEnd**
	returns true if the current cursor is not greater or equal than the source length.
- **advance**
	returns the current character and increment the current cursor to the next position.
- **match**
	it's like a conditional advance, returns true if the current character matches with the expected character, if it is, increment the current cursor too. If the cursor is already at the end returns false
- **peek**
	returns the current character, and if we are at the final we returns '\0' character, it doesn't increment any cursor.
- **string**
	strings start in `"` so we consume all the characters until we reach the other `"`.
- **peekNext**
	do the same as peek but returns the next character relative to the current position.

### Adding a Block comment
When we need to add a block comment we use all the tools that we create, now the main problem is when we want to add embed comments inside the block comment, we can't resolve this problem using the same approach we use in `string` function. But we can do this in place.
```java
case '/':
	if(match('*')){
		for(;;){
			if(isAtEnd()) break;
			// end of the block comment
			if(peek() == "*" && peekNext=="/") break;	
			if(peek() == '\n') line++;

			advance();
		}
		// includes the final */ 
		if(peek() == '*' && peekNext == '/'){
			advance(2); // custom increment
		}else {
			Lox.error(line, "Comment not terminated");
		}
	}
```