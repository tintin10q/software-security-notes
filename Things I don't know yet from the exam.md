
What is the error here

```c
char* src = malloc(9); 
char* domain = "www.ru.nl"; 
strncpy(src, domain, 9); // copies 9 bytes from domain to src
```

Awnser: Only copies the actual 9 characters but not the null terminator into the heap.
Should use strcpy instead of strncpy because strncopy does not copy the null terminator if the number of characters copied is less than the lenght of the source string

----

- Can prefast catch that?
- TOCTOU attack?

![[Pasted image 20230104180542.png]]