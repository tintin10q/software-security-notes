The information flow.
What is it?

![[Pasted image 20230106185855.png]]

Let's formalize further. 

Imagine having a phone app to find restaurants. Sensitive information is location and credit card numbers. You only want google to get the location and only the hotel the credit card. These are specific location flows you want to allow and disallow. 

[[Input problems not to do with parsing#Access control|Access Control]] is just about access only. Information flow is about what you do with the data after you have accessed it and where you send it. 

How can you write policies for this. But OS access control does not work because it's only on the process level. You can give the app access to information, but the OS doesn't know what the app does with it. So let's have a type system and then the store can check it.

## Goals

- No **confidential** information should leak over the network
- No untrusted input from the network should leak into the database **(integrity)**

Everything flips if you go from confidentiality to integrity.  

## Example Confidential

```java
string hi; // Secret
string lo; // Public 
```

![[Pasted image 20230107110842.png]]

---
<br>
<br>
<br>
<br>
<br>
<br>

![[Pasted image 20230107110759.png]]

4 depends on if the code is secret. 8 depends on if the input is secret.

```python
lo = some_function(hi) # depends what the function does
lo = encrypt(hi, AESKEY) # yes but how does the type system know?

if (hi > 0) {lo = 99;} # This is leackage because low tells you about hi, but implicit
if (lo > 0) {hi = 66; } # Fine becaus hi doesn't leak
if (hi > 0) {print(lo); } # Still implicit leak 
if (lo > 0) {print(hi); } # Explicit information flow leak 
while (hi > 99) do {...} # While epends value hi, is time leakage implicit leak
a[hi] = 23; """a is public""" # Could throw index error this leaks information about hi
a[lo] = 23; """a is secret""" # If this throws index error it leaks length of a
a[lo] = 23; """a is public""" # not a problem 
```



## Example Integrity
```java
String hi; // high integrity (trusted) data
String lo; // lo integrity (untrusted) data
```

Which programs cause issues if `hi` can not be polluted 

![[Pasted image 20230107111445.png]]

- 1 is bad 
- 2 is fine
- 3 is fine, we don't care
- 4, but should be fine as it is a compile time constant, 
- 5 is fine we don't care about integrity,
- 6 is fine we don't care about integrity,
- 7 is OK, its already polluted
- 8 is bad, how do we trust the input

```python
hi = some_f(lo) # We don't know
hi = uppercase(lo) # problem, hi depends on low and we don't want this information flow
hi = HTMLencode(lo) # maybe
hi = checkAndStripMac(lo) # this is ok
```

Hidden channels  - Branching on secrets can leak information 

## Confidentiality and Integrity are Duals
What you can see from these examples is that confidentiality and integrity are **duals**. Everything is the same but opposite. If you flip every property from encryption you get integrity property.

- Inputs are dangerous for integrity
- outputs are dangerous for confidentiality

It is easy to get confused. Information flow tries to capture the dependencies (interference) between values. 

## Type based information flow for Confidentiality (Policy)

You can check things at dynamical (runtime) or statically (compile time). These systems are often too restrictive. You need some declassify system. 

In the type system you have different confidentiality levels. These can be a line like:
Secret > classified > Public 
or a lattice = Secret T1 > classified, classified > Public,  Secret T2 > Classified..

### Grammar

Security Hi > Lo

- `e` - Expression in programming language
- `t` - Confidentiality level
- $X_t$ - Variable X has confidentiality level t
- $e \cdot t$ - e is of confidentiality level t
- $e : t$ - e is of confidentiality level t

Then  we have typing rules like this:  $$\frac{Premise}{Conclusion}$$
So how do we know the security confidentiality level of $e$ + $e'$ where $e:t$ and $e:t'$? We do: $$\frac{e:t~~~~~e':t'}{e'+e':t\textvisiblespace t'}$$ here $\textvisiblespace$ is the lower upper bound. This means that if you have a lattice like this: ![[Pasted image 20230107121440.png]]

On the left: the lower upper bound of Unclassified and secret is secret. 
On the left: the lower upper bound of Classified and secret is secret. 

On the right: The lower upper bound of secret Syria and secret is secret.
On the right: The lower upper bound of unclassified and secret Libya is secret Libya.  
On the left: The lower upper bound of unclassified and secret Libya is secret Libya.  
On the left: The lower upper bound of secret and secret Libya is secret.  
On the left: The lower upper bound of secret Syria and secret Libya is secret.  
On the left: The lower upper bound of Top secret Syria and secret Libya is Top secret.  
On the left: The lower upper bound of Top secret Syria and Top secret Libya is Top secret.  

When combing confidentiality levels you go to the next one where they both meet first. So in a line that is just the higer one out of the two but in a lattice that can be the higher in the line but if the two secret levels are on the same level then you have to go higher. 

We can also write $\textvisiblespace$ as LUB (lower upper bounds). 

--- 

**You can always go up** (subsumption)$$\frac{e:t~~~t\leq t'}{e: t'}$$ So if t' is higher or equal then t, then e can also have t'. If you keep doing this everything becomes top secret and no one can read it.

---

If you combine two documents to the same security level you get the same confidentiality level: $$\frac{e \cdot t~~~e' \cdot t}{e + e': t}$$
So you can get in get rid of the lub ($\textvisiblespace$) rule. Because you just first make something the same and then add it. But if you can't go to the same you have to go higher. Because you can never go down. 

![[Information Flow rules expresssions.png]]

----

$$\Large\frac{\frac{\frac{e:{\text{Secret Syria}}}{e:\text{Secret}}(sub)}{e:\text{Top Secret}}(sub)~\frac{e':\text{Top Secret Libia}}{e':
\text{Top Secret}}(sub)}{e+e' : \text{Top Secret}}$$



This is the story for data. If you combine things that are of the same data they stay at the same security level. You can also increase security level of things. You can only combine things if they have the same security level. 

---
Now we have statements. Expressions use things from statements:  
$$\frac{e:t}{x_t = e~~~~\text{is ok}_t}$$
You can only assign $e$ to $x$ if $x$ has the same confidentiality level as $e$. Here $x_t = e$ is a statement from the grammar $S$. `e` is an expression. We specify the confidentiality level of a statement S with $\text{Is ok}_t$. I think only really assignments are statements.

---

Grammar of S.
S := S;S | var = e

----
We can concatenate statements. Usually using ;
$$\frac{S~\text{is ok}_t~~~~~S'~\text{is ok}_{t'}}{S~;~S'~~~\text{is ok}_{GLB(t,t')}}$$
With two statements S and you use them next to each other you get that **the result is OK (is confidential) for only the greatest lower bound of S ; S'.** Remember that S;S is just another statement. GLB is going down in the lattice to the closest highest shared point.

**So $S~\text{is ok}_t$ means that all assignments in S are of level t or higher!**

But why? Because statements can be like this `if (hi == 99) then {lo = 3; hi = 69}`. What is behind the `then` here is an S. The level of the S is `lo`. **Because this is the dangerous thing attackers can observe**.

So here $S~\text{is ok}_{lo}$. What this means is that **all assignments in S are of level lo or higher!**
So again if we have $S~\text{is ok}_t$ then all assignments in S are of level t or higher. This means that you can always lower the level of confidentiality of a statement by assigning to lower things. 

----
Now we can do the same thing again sort of as the expressions:

$$\frac{S~\text{is ok}_t~~~ t \geq t'}{S~\text{is ok}_{t'}}$$
If the level of S is t and level t' is below or equal to t then you can always make S that level. (By adding assignments of the lower level).


----
You can do if statements and if then else statements in therms of GLB, but this gets complicated, so you just assume the previous rule exists and can be used to lower things. Or higher things with expressions.
 
**If statements:** $$\frac{e:t~~~~S~\text{is ok}_{t}}{\text{if}~(e)~\text{then}~{S'}  ~~~\text{is ok}_t}$$
**The level of $e$ should not be higher than the lowest variable that we assign to in S.** Which is what this rule says because if $e: t$ and $S~\text{is ok}_t$ then the level of e is not higher than the lowest variable we assign to in s. Because $S~\text{is ok}_t$ means that all variables we assign to in S are t or higher. So the lowest assignment is of a variable in S of level t and e is of level t so the level of e is not higher than the **lowest** variable we assign to in S. 

----

All the other rules can, using the upper ing of e and lowering of S, just all be the same t:

![[Information Flow rules.png]]
![[Information Flow more commands.png]]

So everything is of the door is only at the same level, and you can move up (expressions) or down (statements) as required. If you then do something that leaks information the type system complaints.  

----

So:

$e:t$ means $e$ contains information of level t or lower
$s:~\text{is ok}_t$ means $S$ only writes to level t or higher

---

To do the type checking you first assign levels to the expressions and basic assignments. Then to the conditionals. 

**What we get in the end is that you can only do things if they are the same level. Otherwise, you have to change the level explicitly first** 



----

How do we know that this is correct? Complete and sound. 

It is easy to give examples that are not typeable but do not leak. Like 
- if (false) then { lo = hi; } - never executes but still complains because violates $\frac{e:t}{x_t = e~~~~\text{is ok}_t}$
- lo = hi + 1 - hi;  - Also violates that rule but semantically the + is ok but type system doesn't know
- lo = hi; lo = 12; - Type system says no. But maybe it's ok?
- lo = $AESEncrypt(hi)$ 

So it is not complete but is it sound?

**Yes, you do multiple program runs with low and hi variables. Fix the low variables and change the high variables. If low changes then hi and low are dependent.**

![[Information Flow soundness.png]]

This doesn't hold if the program doesn't terminate. Because that happening is also a leak of information. 

So you also Non interference termination insensitive:

![[Information Flow soundess termination.png]]

So for termination sensitive non-interference to hold, we need that between runs with different hi, the lo results in the same and if one state terminates the other state also has to terminate.

The basic idea is that you do two runs, changing the state of hi and seeing if this changes low and termination. 

You can also do timing insensitive.

> Note that for the termination incentive to hold the while rule $\large\frac{e:t~~S~\text{is ok}_t}{\text{while}~e~\text{do}~S~~\text{is ok}_t}$  doesn't work.
> You need $\large\frac{e:L~~S~\text{is ok}_L}{\text{while}~e~\text{do}~S~~\text{is ok}_L}$ here L says that it is leaked. So you can only do the guard in while with leaked things.

This makes interesting programs hard, but that is what you get with cryptography. 

## Declassification 

You have to be able to lower things down again after moving up. For instance a password check, you have to branch on the password and then print correct or incorrect. The type system won't allow this. So there has to be an escape hatch. 

## Integrity 

With integrity LUB (lower upper bound) turns into GLB, greater lower bounds. You have to flip all the $\geq$ and $\leq$ signs.  Different levels of taint and for the  [[Injection Attacks#Safe builders|Save builder]] approach for instance you need different kind of declassification, maybe like untaint. 

![[Information Flow Summary.png]]