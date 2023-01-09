With injection attacks you have [[Inputs For Websites]] that go to the process. 

Let's say you have an input, and you want to give mix it with SQL. You can remove bad characters using input validation but then maybe those chars are valid sometimes like `'S-hertogenbos`.  So just replace them using [[Input validation|Input sanitisations]] but this has the same, those characters where maybe needed, and then you need to desanitze later. All languages need different types of escaping. 

![[Pasted image 20230106142846.png]]

## Prepared statements

So for every language you need to be sure what you are doing before. Like SQL prepared statements. Like `sql.run("select * from User where username = ?*",id)`.  For every parser thing where features may be abused you need something like this. Also, this only works for simple things. HTML doesn't work. Also, really configurable queries it gets kind of big. 


## Tainting 

A type system that knows if data is tainted. You can stop tainted data going into [[Weakness|weak]] APIs. Then you can have functions that untainted the data like parsers. Also, if you concatenate tainted data and non-tainted data it should become tainted.  

While perhaps being obvious they are not simple and minimal. 

### Multiple types of taint

If you untainted data then this is not black and white. Maybe you untainted it for SQL but not for html? Who knows? This becomes much more complex.

You also get false negatives. 

## Safe builders

To solve the tainting issues you switch from a negative to a plosive model. You say what data you can trust and everything else you can't trust. So like a URL type that when it parses you know is URL. But you also know it's not SQL.  So we track untainted/safe data. You can safe that compile time constants are untrusted. 

So if you have `executeDynamicSQL(String s)`  you could instead have `executeDymanicSQLQuery(SQLString s)` the only way to get SQL strings is with safe builders. 

The good thing is that this has the different levels of taint. So you can have `SafeOsCommand`, `SafeFilename`. Usually with safe create, concat and append operations that have parsing build in. 

What makes HTML so hard is that there are different languages mixed together. It is a context sensitive grammar. At every place you have to have the correct safe builder. 

![[Pasted image 20230106163723.png]]

--- 

The root cause of the injection attacks is (browser) APIs taking <span style='font-family:bloody;color:red;'>strings</span> as inputs. The devs then just input it directly but not really knowing what kind of string it is. This is what the safe builder approach solves. You classify the strings, and then you make that required in the type system.

So for instance document.write() takes a `TrustedHTML` type as input instead of <span style='font-family:bloody;color:red;'>string</span>.  Because the string could actually run code like Eval and `TrustedHTML` can't. Then google replaced all the browser APIs with this in mind. You can even say explicitly which html you allow. Like only `<br>` or `<p>`.


