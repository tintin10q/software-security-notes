
Safe programming languages are programming languages that provide safety garantees in the code. 

- [[Memory Bugs|Memory Safty]], [[Types]] safty (different levels), thread safety. 
- Access control.
- Visibility restrictions with `public` or `private` keywords
- Sandboxing (relies on memory and type safety)
- Information control flow
- Hiding unsafe code behind an unsafe keyword
- Ofering safe good api/libs
- Confienient features like exceptions 
- Automatic memory management using **garbage collector**
- Runtime checks
- Execution engine like java vm

The idea of safety is to prevent classes of bugs or make them harder to happen or reduce the impact. 

Example:

```c
a[i] = (byte)b // can you cast b to a byte? 
``` 

The programmer should check -> **Unsafe**
The language should check -> **Safe**

Tradeof between speed & control vs saftey & security. 

Safe programming language impose some disiplines or relialbe things. Some programmers don't like this. 

1. Trust abstractions from the language
2. Price & well defined semantics, not leaving things undefined 
3. You can understand the behavior in a modular way. You can understand small parts. No interelations. Without safety you can not understand what a program does in a modular way. 

![[Pasted image 20230105151549.png]]

You can do multiple levels of safety:

Like what happens if you go out of bounds
0. Let attacker inject code
1. Possibly crash program
3. Definitly crash program
4. Throw exception that can be handled 
5. Make the compiler stop injection

From level 3 its safe. You should do both runtime and compile time checks. Especially when working with api because who knows what they give back.

## Memory Safety

1. Programs can never access memory they don't own
2. Programs can not access uninitilized memory
3. Automatically zero out freed memory


## Unsafe features

- No array bound checking
	- Just don't use the heap
	 - Using a garbarge collector
	 - Ownership type system
- pointer arithmetic
- null pointers, but only if they cause undefined behavior
- manual memory management. .


[[Types]] increase safety of languages.

## Visibility 

This is private and public fields and [[Comportamentalisation]].

## Immutability 

You can declare variables as constants. This is great because speed reason it was made but also more secure because it can never change. You have to make a new thing based on the previous thing.  This way state can't just get rugged. Basically, if another part of the program want to change state they also have to create a new reference, so they will not mess with other parts of the program that used to old reference. Great. Mutability is fine with a single threaded environment I guess (js). 

Immutability makes you able to look at the program as a state machine.

## Safe arithmetic 

What happens if `i = i+1` overflows? 

0. Unsafest: Undefined behaviour (c())
1. Safer approach: Defined behaviour 
2. Safer still: Exception thrown
3. Safest: have infinite precision integers & reals


## Thread Safety

Two concurrent execution threads both execute the statement `x = x+1`. What should `x` be? This is a data race because `x+1` is not atomic. In Java this is undefined and anything could happen. These errors are terrible. 

![[Pasted image 20230105162742.png]]

The main thing is **data races**. This happens when you have multiple threads and one of them writes. The program is **thread safe** if the behaviour of the program consists of several threads that can be understood as an interleaving of those threads. Most programming language don't promise anything here. 

Rust aims to guarantee the absence of data races, thread safety at the language level. 

### Aliasing 

Data racing is just a dangerous combination of **Aliasing** and **mutations**. Two threads have a reference to the same object. This is why JavaScript doesn't do this.

![[Pasted image 20230105163810.png]]

1. In concurrent programs you get data races. 
	 - Solved by locking or semaphores but expensive and difficult and cause deadlocks
 2. Single threaded context: have dangling pointers, who is responsible for freeing shared A or B?
 3. Single threaded context: Broken assumptions 
	 -  If A changes the shared object x, this may break B's code because it breaks the assumptions from B about x. The compiler can also change things in ways you don't expect like we saw with Java example above. References to mutable data are dangerous. 

```
public void f(char[] x) {
	if (x[0] != 'a') {throw Error("Not a)}
	 // we can NOT assume that it is 
}
```

One clock cycle later after a check it could be changed. 

```
public void f(String x) {
	if (x[0] != 'a') {throw Error("Not a)}
	 // we can assume that it is because String is immutable
}
```

References to immutable data is less bad because it CANT mutate. **Aliasing** is sharing data between threads. 