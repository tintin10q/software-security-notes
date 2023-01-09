Memory bugs are by far the most common bugs. They have been making memory bugs harder to exploit but it stays constant. 

![[Pasted image 20230104152206.png]]

We got **use after free** bugs. This is using malloced memory after it was freed. With `malloc` and `free`. Also when not freeing memory you get memory leaks and then that is also a problem. 

The essence of the problem is that with allocated blocks of memory we start counting from 0 and that `c` doesn't check the length of the arrays if you access it.

```c
char buffer[4];
buffer[4] = 'a';
```

Here the fourth place is set to `a` but that is not in the buffer and now anything can happen because what should happen is undefined. But probably it will overwrite something else. The solution is to do length checking but c did not do that. 


So the main things are 
- **access outside of array bounds**, 
	- no length checks
	- strings being null terminated
- **pointer issues**
	- Buggy pointer arithmetic
	- dereferencing a null pointer
	- using a dangling pointer or stale pointer (use after free) or double free
	- forgetting to check for failure in allocation 
	- forgetting to deallocate this is an availability error 


## Undefined behaviour

Many things in c(++) are left undefined. This means that when you do something undefined anything can happen. Like when dereferencing a null pointer anything can happen. Most often it will just give a segmentation fault but yeah anything.  

# Buffer overflow

They store the memory address on the stack above the assigned space for the buffers and things in the function. Thus, if you write outside of the buffer you overwrite the return address. Then you get returned oriented programming **Code reuse attack**. This is overwriting the return address of the function on the stack to go somewhere else.

You can also point to the array that you filled up with special bytes that can be executed as code to do something like open a shell. This is **code injection attack**.

Doing these is very hard because you have to get it just right.

So like this 

```c
void f(int x, void(*error_handler) (int), bool b) {
	int diskquota = 200;
	bool is_super_user = false;
	char* filename = "/tmp/scratchpad";
	char[8] username;
	int j = 12;
} 
// if the attacker can overflow username then they can overwrite filename and is_super_user and the diskquota and the return addresss and the inputs like x and b and the erorr function pointer! but not j because it goes upwards.  
```

If you have heap data you can also corrupt things:

```c
struct BankAccount {
	int number;
	char username[20];
	int balance;
} // if you can overflow username you might be able to change number or balance. Depends on the order decided by the compiler
```

You can also overwrite the vtable in c++ to change methods. 

**We are breaking the abstraction here.**

## Strings

Strings in c are arrays of chars. Chars are 1 byte long.

```c
char* str = "hoi"; // a string str
strlen(str); // this is 3 but there is actually one more char the null terminator.
```
This will crash if there is no null terminator. 

![[Pasted image 20230104155639.png]]

So it is always confusing if the null terminator is counted or not on things.

```c
char dest[20];
strcpy(dest, src); // copies string src to dest. 
```

But this doesn't check the lenght of `dest`. If `src` is longer than 20 it will go wrong. 

> `strcpy` assumes that dest is long enough to hold `src` nad that `src` is null terminated.

We should use `strncpy` instead. This has `strncpy(dest, src, size)`. But size with or without the null pointer?

`strlen(dest)` returns the number of chars up to the first null byte. So actually you can make it shorter by adding null bytes. `sizeof(dest)` returns the size of the array. But without the null terminator. Without the null terminator it could also be longer. 

So:

- `strcpy(dest, src)` - copies src to dest without checking length of src. 
- `strncpy(dest, src, length)` - Copies at most `length` bytes from src to dest. If there is no null terminator at the end of src within `length` there will be no null terminator in `dest` and there is empty memory. 
- `strncat(dest, src, n)` - Appends `n` bytes from `src` to `dest`
- `malloc` - Allocate a piece of memory, always check if it succeded. Doesn't actually touch allocated memory so it could still have old data in there like passwords.
- `calloc` - Allocates piece of memory and also initilizes it to zero. 
- `free` - Free a piece of memory. Don't use memory after you freed it and don't free twice.  
- `printf` - Don't put variable strings, as format strings will collect data from the stack if not everything is given as function input. "%p" is good.  With `%s` you can print characters printed so far and also memory at any locastion with carfully crafted input string. Easy to prevent but `printf('%s-%p', a)`. Is dangerous because there is only one input given.

`strncat` example 

```c
char src[50], dest[50];
strcpy(src,  "This is source");
strcpy(dest, "This is destination");
strncat(dest, src, 15);
printf("Final destination string : |%s|", dest);
// Final destination string : |This is destinationThis is source|
```

One problem with `strncat` is that if you should not use `sizeof` with `strncat` but really `strlen` because as you can see above if there is a null character it stops appending. The null character should always be before the `sizeof`. **So `n` is the number of chars to copy not the buffer size**. 

Also if it is too long like copy 50 in there then there will be no null terminator. 

So don't replace `strcpy(dest, src)` with `strncpy(dest, src, sizeof(dest))` but with:
```c
strncpy(dest, src, sizeof(dest)-1); 
dst[sizeof(dest)-1] = '\0';
```
So the null terminator has to be in the buffer. So when you do `char* buf[10];` you get 9 chars and the terminator. 

