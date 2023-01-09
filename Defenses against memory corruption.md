Binairy competability inserted in the compiler. The compiler adds some extra checks:
One downside is that you can not link with a binary that is not compilied with this.

## Stack canaries, 
Add null bytes on the stack and check if they are overwritten to detect buffer overflows before returning. 

![[Pasted image 20230104171447.png]]

used to be fixed value then became include null value which is hard to insert or you could do xor the return adress and canary value. Simple signature.

![[Pasted image 20230104171720.png]]

## Non executable memory 
This is called nx w xor e. In the hardware. Write or execute. So bascially you say what is data and what is code. This  prevents code injection. If the instruction pointer goes into data we say hooo stop. 

Does not work with JIT because the output of the compile step is data. So you can't inject own code but you can jump to other code. 

Then you could just jump to system cals like exec or system. You could statically check if these are not used and then rip them out. Or ring alarms. This caused return oriented programming using  gagets (small pieces of code). This is return orient programming. Most libsare turning complete. 

You also have data only attacks. We can overwrite the `isAdmin` bool. Or only reading data is also possible still.

## Adres space layout randomization

Make sure that the binary always chooses a different address space. This way things with specific memory addresses don't work. The operating system can also do this. Security by obscurity. 

But all these defences are all defeated by offsets detection (aslr) or writing null bytes back somehow or changing non executable memory bits saying what's what. 

## Save buffers at the top of memory, so they can't overwrite other parts of the memory. 

But if you can overwrite a pointer somewhere. 

## Shadow stack

Do more check with storing the return address in the heap and checking if it's the same. More overhead but you can also corrupt the heap.

> **Probabilistic methods:**  Like **ASLR** add randomness to make attacks harder. Also, pointer encryption.

> **Memory safety**: Do additional bookkeeping & add runtime checks to prevent illegal memory access - **canaries**

> **Control flow hijack defences** - Do additional bookkeeping & add runtime check to prevent strange control flow 

## Probabilistic methods

### Pointer encryption

> This is a probabilistic method

Store the encrypted version in memory and when you make changes to it decrypt it and encrypted it again. This way the pointer can not be overwritten by attackers in buffer overflows because you have to write the right value encrypted. This uses encryption normally used for integrity as confidentiality. 

You also encrypt all data, actually. Some and chips support this called SME secure memory encryption that uses AES. 

## Pointer authentication code

> This is a probabilistic method

This is part of the hardware. You can add a mac to pointers that are short. Uses a cipher called QARMA. These ciphers have to be super lightweight. 

## Memory safety

## Fat pointers 

> This is a bookkeeping for memory safety 

Record size information for all pointers, adds runtime checks for pointer arithmetic & array indexing. This is what every good language already so not c. This is not great because now it's not compatible with binaries that are not compiled with the compiler with fat pointers. You can do this at the value or at a central location. 

Tools like valgrind and memcheck or asan keep track of which memory is allocated, and then they check if you made mistakes. 

![[Pasted image 20230104174937.png]]

## Guard pages

![[Pasted image 20230104175021.png]]

Make the pages in the page tables in such a way that you have memory and non writable pages alternating. This way you can catch a buffer overflow of already one byte. This increases memory usage a lot. But you can just do it for debugging. 


## Control flow hijack defences

Here you do extra bookkeeping with data gained at compile time to check if certain control flow is happening. Because if you know that function a never calls b you can quit if it does. Or a return from b to a. This can prevent ROP (return oriented programming) or return to libc. Also with the vtables thing you don't now what actually is going to happen. 

![[Pasted image 20230104180119.png]]

You can also make multiple g for both calls but that doesn't work for recursive functions. 

Downside of this is that you have to do a whole control flow analysis but this really hinders rop.

# State

Most devices have Stack canaries, ASLR and NX (the write execute thing) are standard except on very cheap devices such as IoT. The s in IoT stands for secure.

Some fancier things are slowly becoming used like pointer encryption on IOS and hardware enforeced stack protection in windows 10. 


## Limits of static analyses 

```c
if (i < 5) {c = 5}
if ((i < 0 || (i*i > 20)) {c = 5}
```

Like this it is very hard to say when c will exist. They can either try a bunch of values or say

- Dont know
- False positive
- false negative 

Ok so [[Tool Prefast]] and sal.