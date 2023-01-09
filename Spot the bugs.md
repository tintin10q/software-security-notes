

![[Pasted image 20230104130404.png]]




-----








![[Pasted image 20230104130414.png]]

Also race conditions. 

You got logic errors, lack of input validation or interaction with underlying platform. The integer thing is also because the abstractions.





-----

![[Pasted image 20230104153129.png]]

I would say use after fee on buf. free befofre malloc? But could have happened in between. Not checking if we actually got the memory or not. buf1 is not free.  

![[Pasted image 20230104153555.png]]



![[Pasted image 20230104162043.png]]
It should be sizeof(buf) - 7 because we already filled 7 with the prefix. 


![[Pasted image 20230104162354.png]]

Here you do `strcpy` first with 9 but then the src did not have space for the null terminator so oops the string is not null terminated anymore and now strcpy just keeps copyng. 

![[Pasted image 20230104162531.png]]


So don't replace `strcpy(dest, src)` with `strncpy(dest, src, sizeof(dest))` but with:
```c
strncpy(dest, src, sizeof(dest)-1); 
dst[sizeof(dest)-1] = '\0';
```

![[Pasted image 20230104162922.png]]

Here you say max(len, 1024) so that means you want minimum of 1024.But we should overwrite len with the max we choose otherwise you still read len. **Also malloc can fail.** Also what happens if len is negative? Apperently the third parameter is read as unsigned so negative len is interperted as a big positve one. THIS ONE YOU DON:T HAVE TO REMber 

![[Pasted image 20230104163412.png]]


![[Pasted image 20230104164309.png]]

~~If the MAX_BUFF is exactly the same as strlen and you copy there will be no null ternminator in the buffer anymore.~~

![[Pasted image 20230104164449.png]]

len is a short so you can get an overflow with strlen(input). Then you get a potential buffer overflow. 

![[Pasted image 20230104164746.png]]

sheesh.



![[Pasted image 20230104165053.png]]

So here they wanted to detect the overflow by looking if start + 100 wrapped around. But this does not always happens is it is undefined. 

![[Pasted image 20230104165310.png]]

The compiler will actually rip line 5 out because because of 4 it is already not null and if it is is null we get undefiend behavior and the compiler can rip out 5. Then oops it doesn't check if its null.  

![[Pasted image 20230104165658.png]]k

Basically you can't print based on the size because the str_len and size are not the same. If the buffer is 20, then for unicode this is actually not one byte per position so sizeof is much bigger. strlen can be much less then sizeof because one unicode is bigger than a byte. 

So `sizeof(buf)` is the size in bytes but this parameter should be the number of characters. So you can't make the arbitrary assumption that the number of bytes will be the same as the number of elements.

![[Pasted image 20230104170248.png]]

---- 

![[Pasted image 20230104191646.png]]

Semiclon at the end of the if statement oeps.  

![[Pasted image 20230104192332.png]]

Two goto statments. It looks like both belong to the if statement but only the first one does. So if the if does not work then it also goes to fail. 

Examples of tools is:

![[Pasted image 20230104194244.png]]


They covered [[Tool Prefast]] especially.

![[Pasted image 20230105231312.png]]

Here the bytes inputs at m2 is mutable, so you could mutate the bytes after `enablePermission` on another thread. 


![[Pasted image 20230105232701.png]]

Here the String is not mutable but the String array is. Oops. So this is great, you can just call `getSingers` which returns the pointer to the signers array, and then you can just append to it!!!!! lol. So just return a copy. This shows that private is pretty misleading because you can just return the pointer. You can say don't expose your private parts. There are type systems that do this but they are not flexible enough. 

![[Pasted image 20230106011704.png]]

Integer underflow of tries left. You still have to be secure in the enclave. If you can get malware in there you can't detect this.  


![[Pasted image 20230106133042.png]]

There might not be a /. Infinite loop Not enough parsing. Why not just parse the url directly into a URL struct. This is just lazy. The partial parsing repeats work done by isValid. We have here shotgun parsing. Just parse it in one go.

![[Pasted image 20230106165435.png]]


![[Pasted image 20230106180910.png]]

We should not return false in the for loop but set a bool to false and then return that. If you put in an empty guess it doesn't check anything! So you need pwd.check.

![[Pasted image 20230106183213.png]]

sql injection of course but also It's updating a global variable. 

![[Pasted image 20230106183524.png]]