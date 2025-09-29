Now we finally are in the backend section ([[Fronted, middle end, backend]]), here we need to translate our language to code (assembly-cpu instructions) for a specific architecture (x86, ARM).
Now we can create this backend for x86 architecture but now our language can only be executed in x86 devices and not ARM, so we need to write other backend for ARM.
![[Pasted image 20250912185618.png]]
To resolve this issue some hackers Martin Richards and Niklaus Wirth invent a middle step, first in place we need to generate the code (assembly-cpu) instructions we create a p-code (pre-code), this p-code or commonly called **bytecode** is created to run in a fictional architecture, so in this way we don't depend on the architecture of the computers to run our code.
![[Pasted image 20250912185640.png]]
## Parse bytecode to code
Now we can generate bytecode but how can we run our application, so a solution would be to create a mini-compiler to translate the bytecode to code (assembly-cpu) instructions. This part is where backend comes in, we create a backend that in place to translate our language to code, take a bytecode and translate it to code.
## Run bytecode
There are two approaches to run bytecode, the first is parse the bytecode to code (very fast). The other is creating  a virtual machine that emulates that **fictional architecture** (a little bit slower).
![[Pasted image 20250912185657.png]]
