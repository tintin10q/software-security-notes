Data racing when two threads are both having the same data, and it goes wrong. See [[Safe Programming Languages#Thread Safety]]. 

```python
x = 0
with ThreadPool(2):
	x = x + 1
print(x) # 1 or 2?
```

The root cause of this problem is that x+1 is not atomic and this leads to undefined behaviour. It may be interleaved in unexpected ways. 

**This was on the exam last year!!**

These types of attack are called **TOCTOU** Time of check, time of use.

## TOCTOU (time of check, time of use)

Time of check, time of use attacks is that you can not trust a value in a multithreaded setting because after you checked something the data can have change when you use it. You can only safely use a value after you checked it if you are sure no one else can change the value. So really all variables should just have one mutable reference around. <span style=font-size:smaller>This is what rust realized and made the borrow checker.</span>

Also called **Non-atomic check and use**. Again from the slides *some precondition required for an action is invalidated between the time it is checked and the time the action is performed*. Typically, this check has to do with access control and involves concurrency. Say it as TOKTO

**Filenames** deserve a special mention here. When you check a file exist, you could delete the file quickly after to still cause a read error or create a symbolic link.