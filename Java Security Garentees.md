Java safety & security guarantees:

- [[Defenses against memory corruption]] (memory safety)
- Strong [[Typing in java in android]] +  [[Types]]
- Visibility restrictions `private public`
- immutable fields using `final`
- non extendable classes using `final`
- immutable types like `String`,` Boolean`, `URL`
- sandboxing based on [[Access Control in Java|stackwalking]]
	
Allows for security guarantees to be made *even if the code is untrusted or buggy.* 

Similar guarantees made by .NET/C# and Scala ok. 

All this works based on the Java [[Comportamentalisation|TCB]] which consists of Byte code verifier, ,vm, security manager, class loader.  So if anything is wrong here it's all for nothing. And turns it out was too complex and bugs in it.

# Failures 

- Messy updating mechanism - Microsoft makes it as hard as possible to update. 
- Large growing TCB with bugs 
- Possibility to download code from the internet oops log4j

## Deserialisation attacks

We know what serialization is. The inverse is deserializing. But when you do this you can upload malicious code. Even if it is not the class you expect it is thrown away, but some code is run. Funny. It makes an object but then throws it away because it's not the right object. Then it becomes garbage and the garbage collector calls a function on the class letting it know its deleted `__finalize__`. Oops. 

