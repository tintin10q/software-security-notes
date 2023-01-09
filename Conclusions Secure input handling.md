Most security problems are Input problems where input is passed around to trigger **bugs** or **features**.

![[Pasted image 20230106170756.png]]

## Problems are:

1. Insecure parsing - Buffer overflow or crashes
2. Incorrect parsing or parsing differences
3. unintended parsing -> injection attacks. Parsing something that you should not parse. This has side effects like executing code.

Even more so if there are many complex, poorly defined, very expressive languages.

Solutions are [[Input validation]] and sanitization, encoding and escaping but the best thing to do is getting out of strings and to an actual type and then working with that. The [[Input validation|safe builder]] approach does this, and then you can require these parsed types at API interfaces.  

## The Root Problem Is:

<span style="font-family:bloody;color:red">Strings</span> and strings concatenation. Inexperienced programmers just don't know about the whole language story. Normally behind the strings is a whole programming language if you give the right input. Strings are is a big anti pattern. Strings are also just byte arrays `char* []` etc. 

You can parse strings in many ways, but you don't know what is actually in the string. Concatenating strings is unparsing because later someone has to parse it again. **Use types instead of strings**.

## Solutions

[[LangSec]] approach prevents **insecure or incorrect parsing**. Langsec says: have a clear spec and **generate** a parser from it. 
use **Type safe builder approach** to prevent [[Injection Attacks]]. This is using types to systematically validate and **output encoding** of untrusted data given to dangerous APIs. This is also parameterized queries. Also called API hardening. 

With the types you can know what every data is actually. 

![[Pasted image 20230106172314.png]]


