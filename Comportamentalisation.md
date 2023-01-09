You want to create borders around something. Like a sandbox. You don't want the sand to come out. Or in a ship you don't want a leaks to go from one room to another room. You want to put things in compartments with strong walls so that they can't affect eachother. No communication between cells because no holes. Running the user database and the password database on different machines. 

![[Pasted image 20230105164900.png]]

You can not just have this in software but also in organisations. Where parts of the organisations can not talk to other parts. In an it system different machines for differnet parts. On a single computer, processes are the main example of this. Different processes for different tasks. Different accounts for different users. Users can't touch files of other users. 


You can even compartimentialize inside a single process/ program / app / application. This is an example of visibilty. You do this with different modules that all have different tasks. Each module can have seperate privaleges. 

You want the modulase to be **Isolated**. You can't **confidentialty** and **integrity** between data and code of different modules. 

![[Pasted image 20230105165413.png]]

One flow can not touch another flow in any way. 

## Use cases 

There are two main use cases for compartementalization. But they boil down to isolating different trust levels. 

1. To contain an untrusted process from attacking others, this is sandboxing 
2. To protect a trusted process from outside attacks. 
	- You can also have recurseive sandboxing
	- ![[Pasted image 20230105211458.png]]

With sim cards its actually number 2. With pdf viewers its 1. With a vm its sort of both but mostly 1. With compartimalization you make the attack surface smaller and also the impact. 

## Trusted computing base (TCB)

In the picture the compartementalization is a brick wall but when you do compartementalization its made of software so it can still have buffer overflows..... You have to trust the compatementalization software. Sometimes you can also control it with **policies** allowing certain **parties** certain **access rights** checked with **runtime monitoring**. This is nice but also dangerous because you can mess it up. Also there are always **input** and **output channels** so it is never a complete brick wall otherwise it is pretty useless. The interfaces have to be simple with minimal parsing requirements!

But compartmentalisation is great for security. You should do it everywhere with simple interfaces between modules.


## Problems with OS access control 

- The trusted computing base with an OS is huge.
- Too much complexity 
- Not expressive enough. The OS access control can be not expressive enough. But you can go further in process based access control. 
	- Basically, if an OS grants a process a right any line in the process can do this. Either you split the process in separate process or you use tools of the programming language to split permissions. Every browser for tab is a separate process with separate processes for rendering and the js. 

So keeping it simple is good.  CubeOS? 


## Access control at the language level

To get over the OS granting a whole process you can use language features to do access control at language level. This makes it possible to have **security guarantees in the presence of untrusted code**. Without memory safety this is impossible because you can just break the abstraction and change the access.

![[Pasted image 20230105214605.png]]

Without [[Types]] safety this is hard because it is hard to manage the interfaces correctly.  So that is what you get with js when not using ts. Although the whole js actually gets the permission. 

The read more about this in Java go to [[Access Control in Java]].

## Hardware sandboxing

Unsafe languages cannot provide sandboxing on the language level because you can corrupt the memory. We can use hardware sandboxing for this. 

With intel, you have enclaves. The hardware makes sure that you can not access the blue area from anywhere except from the green code! Pretty cool. The green code has to be secure though. Also, it is not safe against return oriented programming. Less flexible then [[Access Control in Java|Stack walking]]. Even the OS is outside the trusted computing base. 

![[Pasted image 20230106010706.png]]

This is for private keys storing in cloud computing pretty cool. Also, DRM without having to expose keys. But how do you know that you are talking to intel ships? Intel has to have signatures with certificates. Side channel attacks? Does intel have to sign the code???

These things are called trusted execution engine. Typically, it's a separate chip, so the private key never leaves the chip! 

You can do access control based on the program counter. Only read the tcb if the program counter is a certain state. But then you have to trust the OS so no. 

## Main advantages

- Reduces TCB
- Allows checking only small piece of code
- Run code from many sources on the same vm
- Also safety for unsafe programming languages

But.

- Less expressive and less flexible 
- Hardware is hard to replace 
- Will become big in the coming years 

