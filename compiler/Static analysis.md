---
tags:
  - compilers
---
This step is implemented in different ways depending of the language. Here we have already a syntactic structure (this is done using  [[Parsing]]), but we don't have still any logic how what values have expressions like `a + b`.
## Binding or resolution
The first part of this step is doing the binding or in simple words bind the values to the variables declared. Here we define the scope of our variables too.
### Ways to save the binding information
- #### Save attributes (values) in the syntax-tree itself.
![[Excalidraw/Drawing 2025-09-11 23.50.44.excalidraw]]
- #### Symbol table
![[Excalidraw/Drawing 2025-09-11 23.52.59.excalidraw]]
- ### Parse syntax-tree to a data structure that expresses more semantic the code.
## Check types
When we have our variables bind, now we can check if the type of the values is correct or incorrect and throw an error.