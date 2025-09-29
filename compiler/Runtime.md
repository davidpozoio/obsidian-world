This is the last step in our pipeline [[Compilers]]. If we compile our language to machine code we only need to execute it, if it's compiled to bytecode we use the VM (virtual machine) and execute it there.
Now normally our language need some **services** running in second plane to handle some features of our language. These services are **garbage collector** if our language has one, if the language supports **instance of** we need to track these types.
All services running in this phase are called **runtime**, now the runtime processes are attached if we compile our program to machine code directly in the executable file.
![[Excalidraw/Drawing 2025-09-12 19.19.43.excalidraw]]
If we use a VM the runtime lives there.
![[Excalidraw/Drawing 2025-09-12 19.20.35.excalidraw]]