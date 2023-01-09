
Prefast is a [[DAST, SAST|static]] analysis tool that analyses source code:


Prefast assignment code:


```c
#include "stdio.h"
#undef __analysis_assume
#include <CodeAalysis\SourceAnnotations.h>

int* wrap(int*, int);
void zero(int*, int);

void work() {
	 char name[4];
	 name[4] = 'X';
	 int elements[200];
	 wrap(elements, 200)
}

int* wrap(int*, buf, int len) {
	int* buf2 = buf;
	int len2 = len;
	zero (buf2, len2);
	return buf;
}

void zero( int* buf, int len) {
	int i;
	for (i = 0; i<=len; i++) buf[i] = 0;
}

```

So the first bugs here is line  2 of `work`. It does an out of bounds. The second bug is that it has to be `i-1`. or `len-1`  or <= instead of = in the for loop of zero.

Now prefast is able to pick up the first bug with the out of bounds but not the last bug with zero. Why? Because to not cause a false positive, the tool needs you to anotate the function to make clear that len is the length of buf. Like this:

```c
void zero( _Out_cap_(len) int* buf, int len) {
	int i;
	for (i = 0; i<=len; i++) buf[i] = 0;
}
```
So here you say that the capacity of `buf` is `len`. The \_Out\_  says that the array does not have to be initilized but when it is returned it should be intilized. And now it can detect it. 

So you can fix it like this:

```c
void zero( _Out_cap_(len) int* buf, int len) {
	int i;
	for (i = 0; i < len; i++) buf[i] = 0;
}
```

```c
void zero( _Out_cap_(len+1) int* buf, int len) {
	int i;
	for (i = 0; i <= len; i++) buf[i] = 0;
}
```

These annotations are SAL (Standart Annotation Language) which is a lanague for annotating c(++) code and libraries. 

Prefast features:
- checks on deprecated functions  like gets
- correct use of functions 
- coding errors like using = instead of == in an if statment 
- memory errors like assuming that malloc returns non-zero or going out of array bounds. 

- SAL annotations improve the results of PREfast 
	- more checks 
	- more precise checks 


Some underling thing is 

`_Check_return_ void *malloc(size_t s);` 

The `_Check_return_` means that caller must check the return value of malloc. 

We start an annotation with a suffix:

- `_In_` The function reads from the buffer. The caller provides the buffer and initializes it
- `_Inout_` The function both reads from and writes to the buffer. The caller provides the buffer and initializes it
- `_Out_` The function **only** writes to the buffer. The caller must provide the buffer, the function will initialize it. 
- `_Ret_` This you use to specify return values for functions.


In the second part of the annotation you can specify other things like this:

- `cap_(size)` the writable size in elements 
- `bytecap_(size)` the writable size in bytes

- `count_(size)` the readable size in elements.
- `bytecount_(size)` the readable size in elements.


Count and bytecount should only be used for inputs, so `_In_`. `cap` and `bytecap` is for `_Out_`

Be default the tool assumes a tool is not null. If you want to have a null parameter you have to say `opt_`. Those parameters may be null. Without this a pointer can not be null. 

The tool can do some basic alritmetic. Behind the scenes all information is fed to a thereom prover. 

Example for `_Ret_`. Basically you say you return an int buf with size len. 

```c
_Ret_cap(len) int *wrap(_Out_cap_(len) int *buf int len) {
	zero(buf, len)
	return buf;
}
```

You can also make pre and post conditions, `_Check_return_` is a shorter version of something more complicated. 

Like:

- `[Sa_Post (MustCheck=SA_Yes)]` Longer version of return
- `[SA_Pre(Tainted=SA_Yes)]` This argument is tainted and conned be trusted without validation
- `[SA_Pre(Tainted=SA_No)]` This argument is trusted
- `[SA_Post(Tainted=SA_No)]` The result should not be tainted

The syntax is horrible though. Typescript pls. But typescript doesn't do as much as this though.

intra-procedural  = with annotations individual methods (local) analsys 
inter-procedural  = without annotations (global) analysis, you have to expand or give up

The downside of these annotations is that you have to write them. But you can have an incremental approuch like ts. Also `SALinfer` can infer annotatios. But just for microsoft. 

You could also use and check these contracts at runtime. 

## Pros & Cons Dynamic
- -overhead
- -only exceptions at runtime
- -code coverage is limited to test case
- +/- you get running code faster
- + all warnings are real problems
- + less need for annotations
- - the runtime error does not happen, only when the rocket launches

## Pros & Cons Static
+ +feedback sooner
+ -complain about deadcode
+ +annotations
- +You can run this before the code runs






