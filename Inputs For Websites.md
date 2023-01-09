

Most (all?) attacks on most software is bad checking of user <span style="font-family: 'Bloody'; color:red; font-size:1.5em;">Input</span> which is mostly a problem with the <span style="font-family: 'Bloody'; color:red; font-size:1.7em;">Parsing</span> of some complex file formats. Bad parsing or uninteded parsing. The processing of the input causes the software to go of the rails. 

Fun inputs include:
- Big files
- Big text
- %p%p%p%p%p%p
- Zip bomb (Dos)
- SQL injection
- Request query parameters
- Operating commands `ls`, `bash`
-  Files `.` `../` `./../../`
- HTML encoded software
- Pointers 
- Memory adreses
- xml json
- Malicious PDF files
- Convincing word document with Excel with macros 
- Javascript code? Executed on some other machine and an image link to the js code. 



## Supply chain attack

Join the open-source project and inject (input) bad code. Kind of still an input.

To prevent this you have to include a software bill of materials like package.json or pyproject.toml

## List of these things

- OWASP Top 10 etc
	- Not a good new list very specific and non-specific. 

New list and reconfiguring things. But there is just either a bug or a misused feature. 

- CVE - common vulnerability 
- CWE - Common weakness enumeration 
- CRE - NEW, common requirement enumeration

The big ones are memory corruption, access control (authorization or authentication), injection attacks. 

![[Pasted image 20230106015304.png]]

The lists are sort of useful but also not because too many. You have to know the most important ones. Also, it's very messy. The lists are very incomplete. Some are misleading, like lack of input validation. [[Input validation]] is not what you want but input parsing?


**BASICALLY,** either **Implication flaws** or **design problems**. 

![[Pasted image 20230106111654.png]]

Spam is a feature of emails. 

![[Pasted image 20230106111745.png]]


Nearly all attacks are input problems and the process goes of the rails. From denail of service  to remote code execution. Its either a design flaw or an implementation flaw. In the whole stack this can go wrong.

![[Pasted image 20230106111924.png]]

![[Pasted image 20230106111946.png]]

You get some tainted (untrusted) input, and it flows to the process, and it tricks the program into doing something unwanted. Either because an implementation flaw or incorrect design. 


## BUGS or FEATURES. 

There are two types of attacks. Unintended functionality (bugs) and intended functionality. 

Bugs are just things that can happen that are not intended or features. Cross site scripting is meant to happen. It is not a bug it's a feature. Sql injection is abusing a feature, the fact that the sql DB executes SQL is a feature. You just were dum to concatenate strings.

![[Pasted image 20230106112101.png]]

Buggy parsing or ambiguous parsing or parsing something as a language when you don't want to, or it shouldn't be parsed. The root cause here is the <span style="font-family:'Bloody'; color:red">complexity</span> of the underlying data formats. They are much more <span style="font-family:'bloody'; color:red">expressive</span> than you think. Lots of legacy features that can be abused. 

Social engineering is an injection attack you abuse the features that a human has.

Like `www.paypal.com\0.mafia.com` some certificate auths say this is mafia.com the `\0` could be a newline.
## Parsing

- Buggy parsing - Buffer overflow or crash
- Incorrect parsing - Difficult parsing of ambiguous languages 
- Unintended parsing - SQL injection attack

### Solution

Just write down the grammar and give it to a code generator. Zod does this. 

## Defence

### Prevention 
- Secure input handling [[Input Protections]]
- **secure output handling!** Good tip!

### Mitigate impact
- Reduce the expressive power of inputs
- Reduce privileges or isolate sandbox compartmentalize  (not running web server as root)

### Detect and react
- Monitor to see if things have gone wrong
- Keep logs





