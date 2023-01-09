![[Pasted image 20230104180750.png]]

Static analysis is an umbrella term you do at compile time. 

Basic things

- Syntactic checks;  `grep gets` or `ctrl-F`
- type checking, check if Bool is not assigned to INT
- Taking semantics into amount. 
- Dead code detection 
- Checking conventions to enforce them - style checking
- unused variables 
- missing initialization
- Using toosl like [[Tool Prefast]]

The tools give false positives and false negatives. The false positives are worse because the users of the tools don't use them if they give to many false positives. False negatives are also bad, but positives are worse. 

Sound - it only finds real bugs ie no false positives and completeness it finds all bugs -> no false negatives.. . 

To do the analysis you have to do **control flow analysis**

```js
if (b) {c = 5} else {c = 6}; // initializes c
if (b) {c = 5} else {d = 6}; // initializes d and not c
```

or you have to do **[[Information Flow|data flow]] analysis**

```js
d = 5; c = d; // initializes c
c = 5; d = 5; // does not
```
