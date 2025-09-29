## Single-pass compilers
This type of compilers doesn't have an intermediate representation or a syntax tree. They read the enough information of some piece of code and compile it directly. In simple words they don't group all the provided code in a context but compile it by parts.
## Transpilers
A transpiler is a compiler that compiles the code to another language normally some high language level. This used to avoid to write your own backend, this is because create a backend can be very difficult, so reusing the backend of other language can be a good approach. Now it's useful too to execute code in other platforms like browser (javascript).
## Just-in-time compilers
just-in-time compilers or JIT compilers are a compiler that executes the code normally using an interpreter and later do the optimizations, finally compile all to machine code. It's not necessary to implement an interpreter but one of their main features is optimize the code in [[Runtime]], because when you only see the code you can deduce some optimizations but not all, so this is a complicated way to implement a compiler but generates the most fastest executables.
![[Pasted image 20250913200404.png]]
