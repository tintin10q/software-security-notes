
Typing is great to make certain bugs less likely because you know what things are in the [[Information Flow]]. 

- Visibility restrictions (private)
- Sand boxing (code based access control)
- Sand boxing
- Immutable objects
- Keeping track of different string types (url, trusted url, sql)


Type system can be overly restrictive. We had all those flows.

```js
if (hi) {lo = ...} // not allowed because you can learn about hi
```

Sometimes you have to make exceptions like `//@ts-ignore` but honestly then just leave it in to keep showing the error to the reader.  

For this to work you have to keep track of the level of these, and you can only branch on something lower or same. 

Actually this already is dangerous.

```ts
if (hi) {...} // not allowed because of timing
```

 There is pretty bad types syntax where you put @SomeType in any place where you can write a type (since java 8) but honestly just use the Checker is a framework for this for Java. You can add more semantic information. It is defined by functions that add this information. Kind of like safe builder.
 
```java
void send(@NonNul String msg) {...} //has to take a nunnull string
void sendE(@Encrypted String msg) {...} //has to take a nunnull string

// or 
@Encrypted String encrypt(String s, Key K) {...}

String msg = "Plaintext"; 
@Encrypted String msg1 = "fGpOQEA="
String c = encrypt(m)
send(msg) // ok
sendE(msg) // NOT ok
sendE(msg1) // ok
sendE(c) // ok
```

These types things are cool for play stores to check apps. 

This is useful because without this kind of marking something like `stmt.executeSQLs(s)` could be SQL injection, but you don't know and Java also doesn't know. You need these prefast like typing systems for it.  

## Prevent
- Steal user information and credentials
- Make premium calls and SMS or spam with SMSs
- Improve rankings
- Destruction
- Ransomware on phones (he says this will be a big thing)

---

## Type system 

People from a paper wanted to fix this by checking with Java annotations on the information flow and human control + description of what app does. Then you can cross-match this description with the  app. You can have annotations in the code and then the sotre can run it.

> You still need a human to  check if what the app asks for in the description is valid, like a flashlight app can't ask for location 

Example of description:

```java
READ_SMS -> DISPLAY
USER_INPUT -> CALL_PHONE
CAMERA   -> DISPLAY, DATA
LOCATION -> INTERNET(maps.google.com)
```

Now they thought about cool exploits where with this

```java
CAMERA   -> FILESYSTEM 
FILESYSTEM -> INTERNET
```

You must also include `CAMERA -> INTERNET` Because it is implicit. 

Now you can then take this to the type system with:
```java
@Source(Camera) // data sourced from camera 
@Source(Location) // data sourced from location
@Sink(Internet) // data send over internet
@Source(Internet, Camera) // data from internet or camera
```

So you just got `@Source` and `@Sink`. 
Sink is a destination like display or internet or filesystem. 
Source is source like camera, the filesystem or location information. 
Then the type system could check the information flow.

> For this to work the whole java android api has to be marked

So like this 
```java
public static @Source(LOCATION) String readGPS();
public static void sendToServer(@Sink(INTERNET) String s);
```

You have to mark variables as to where it is giong to go:

```java
@Sink(INTERNET)@Source(LOCATION) String loc = readGPS();
sendToServer(loc)
```

If you don't mark this as `Sink(Internet)` the type system can complain. But the system complains if you did not have `LOCATION -> INTERNET` in your policy or desciption file. 

```java
@Source(LOCATION) String loc = readGPS();
sendToServer(loc) // type checker contains 
```

This is also ok:

```java
@Source(SMS) @Sink(SMS,Internet) String s1 = ...;
// you just say ok this could have come from internet but it didn't but doesn't really matter if you expand on that. The sink is ok because you said that sink sms was in there. 
@Source(SMS, INTERNET) @Sink(SMS) String s1 = msg;  
```

![[Pasted image 20221214234720.png]]

All typecheckers are often too restrictive. 

```
@SupressWarnings("Flow") // Now a human reviewer has to check
```

## Implicit flows

```java
@Source(USER_INPUT) long pin = getPINCode();
long i=0;
while (true) {
	if (i == pin) {
		sendToInternet(i);
		i++;	
	}
}
```

So here we can not do this because pin is used in the if guard. They do not check for the internet but this hack. If you do this you have to put `USER_INPUT -> CONDITIONAL`. 

>Ok... I mean when do you not do this. 

The type checker always warns these and these have to be checked. 
So what is checked
1. The android OS, incl the java virtual machine
2. The type checker for annotations
3. The java compiler & byte code verifier
4. The annotaions provided for the APIs
5. The annotations provided by the app developer. 
6. The human verifier

Only 5 is can not be trusted by the app store.

## Generics

You can have generics to make a function convert a source or sink. That looks like:

```java
@PolySource char[] stringToCharArray(@PolySource String s)
```

Thsi was all mostly on confidientially. 

**This can not check for bitcoin mining**

You also have remember trusted types. SafeScript, SafeURL, SafeURL. 

