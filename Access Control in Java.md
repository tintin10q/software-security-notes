
The policy is written as:

```java
grant 
	codebase "http://www.cs.ru.nl/ds", signedBy "Radboud",
	{ permission
		java.io.FilePermission "/home/ds/erik", "read", "write";
	};

grant 
	codebase "http://www.untrustedtrust.us", signedBy "Nort Korea",
	{ permission
		java.io.FilePermission "/home/ds/erik", "read";
	};
```

So if you don't have write permissions this code can not write. 

So if you have code like this:

```java
package trusted;
class Trusted {
	void m1() { System.delete file}
}

package trusted;
class NOTTrusted {
	Trusted t;
	void f1() { t.m1() } // can this delete? hard problem
}
```

The components interact. You can restrict it with good interfaces But the interfaces will still be accessible. So should the file be deleted? This is called the **confused deputy problem**. It depends. If the filename is a compile time constants its probably OK but if it's an input its not. 

One way to do this is to check the call stack. You could allow the action if **the top stack frame has the access**, or the paranoid approach where **every stack frame has to have the right.**

The top frame could be too dangerous because it could just call the bad code, and then it can call the other code... A class might expose dangerous functionality. Every frame is very restrictive, and you might as well grant everyone permissions. 

## Stack walking 

The solution for this is that if you have the permission you can say that you become the top stack frame. 
This is called **stack walking**. You explicitly allow an untrusted component to call a function. You can also take away permissions from the function. This allows principle of least privilege. But will people do that? 

So you can do that with 

```java
function deleteFile(String[] file) {
	// lots of checks
	...
	Enable_permission(P); // Enable the permission for the callers, 
	// I am taking resposibility 
	Enable_permission(P2); // Take P2 away  I don't need it
}
```

Then you can call `DemandPermission` to check if the stack has the required permission. So you can at any moment call `DemandPermission` to assert that the callers either had the permission or said that they did the checks and `EnabledPermission`. You can stop permissions trickeling down by doing `DisablePermission`.

### Demand Permission Algorithm 

```python
def DemandPermission(P):
	for caller in stack: # This goes from where we are now to the top of stack
		if not caller.hasPermission(P) raise "Not permitted"	
		if not caller.hasDisabledPermission(P) raise "Disabled"	
		if caller.hasEnabledPermission(P) return True;	
	else:
		return True # If we hit the end of the stack without error its allowed.
	
```

![[Pasted image 20230105224930.png]]

This won't work because `PD1` does not have P1. Also, if you make threads you have to actually get back to the thread creation stack because otherwise you can just make a thread with an empty stack to call the function. 

However, this does work. Because going through the stack stops at `EnablePermission`.

![[Pasted image 20230105225042.png]]

This one does not work because the `demandPermission` hits the `DisablePermission`. You could do this if `PD2` has a lot of <span style='font-family: "Bloody"; color:red'>user input</span>.

![[Pasted image 20230105225328.png]]

## Example

```java
class Trusted{
	public void unsafeMethod(File f) delete f;
	public void safeMethod(File f) {
		... // lots of checks
		enablePermission(FileDelete)	
		delete f;
		disablePermission(FileDelete)	
	}
	public void antoherSafeMethod(File f) {
		enablePermission(FileDelete)	
		delete "/tmp/hardcodedfile";
		disablePermission(FileDelete)	
	}
}
```

At the `safeMethod` and `anotherSaveMethod` you can become the top of the stack or stop the stack walking because you did the checks. The `unsafeMethod` is only OK if the whole stack frame or up till another method that enabled the permission has the delete permission. **Here the delete keyword calls the `DemandPermission(FileDelete)`** implicitly. 

The data has to be immutable for this to make sense. Otherwise you can change the thing after the checks. You 