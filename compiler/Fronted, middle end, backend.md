We have three important parts to divide a compiler
## Frontend
This part contains [[Scanning]], [[Parsing]] and [[Static analysis]] steps, this part represents our **language** itself.
## Backend
This part is in charge of parsing our language to some architecture (x86, ARM, etc.).
## Middle end
This part is in charge of adding a new layer of abstraction between the frontend and backend, this is important to avoid to write many implementations, for example if we want to create the implementation of **javascript** and **C#**, or in other words we need to write 2 frontends and 4 backends, yes we need to write four backend because we want to be able to run our code in the two architectures (x82, ARM), so here is a problem.
A way to resolve this is create a layer of abstraction called **IR** (intermediate representation), now our backend only must know how to the translate the IR to the respective architecture.
Doing this we only need to write two backends because our two frontends translate directly to the same IR without caring about the language implementation.
![[Pasted image 20250912000649.png]]
