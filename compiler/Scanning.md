Scanning also known as lexing or lexical analysis, it's the step of read a chunk of character a group in "words", these words are called `tokens`.
Some tokens are single characters `( , [` and other are composed by a sequence of characters like numbers, strings, identifiers and keywords (1234) and ("hi") and identifiers (min). Other tokens doesn't have any meaning like white space.
```lox
// code
var average = (min + max) / 2;
```
**![[Pasted image 20250829002150.png]]**
The next step is [[Parsing]].