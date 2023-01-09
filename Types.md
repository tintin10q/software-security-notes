One layer up from [[safe Programming Languages|memory safety]]. Types assert properties of program elements like this variable is always an integer. This function will always return a string if you give it an integer. This array will never store more then 10 items. Basically type systems tell the compiler how to interpert a blob of data. 

Type systems vary a lot. 

**Type checking** verifies these assertions. **Type soundness** (aka type safety or strong typeing) is a language where the assertions are guarenteed to hold at runtime eithe statically or dynamically.

```ts
fuction hi(yo: string) : int {
	return yo.length
}
```

So languages can be memory safe, typed and type sound.

In c++ you can actually cast a to a bytesarray and then you can just edit private fields. Because of this you have to review the entire code base to see if this doesn't happen.  

C is typed but not type safe or memory safe. Python is typed but not type safe but it is memory safe. Haskell is type safe and memory safe. 

You can check types at **compile time** or **run time** or both. For instance there are times when you don't know whats what. You have to do both. Exceptions are just runtime tracks.  

### Breaking type safety

Type safety is fragile. One flaw in the type system brings it all down. If you can create **type confusion** by having two references to the same memory but with different types its over. Then the type system is not sound. 

This called type confusion attacks. You compile B inherited from A but then what if you compile with another binary that has already A. If B has x and A.x is object but A from the binary is integer you can do pointer aritmatic because the object is just a pointer. This causes problems. In java you need binary compatability for classes to load. 

This goes wrong because java then doesn't check the types at the runtime.  You only do the runtime checks for things you don't know at runtime like is something null or api response. You can have execution engines that keep checking everything at runtime. 

## Type sound?

How can you prove that type system is sound? 

### Representation independence

It should not matter how types are represented in the abstraction. Booleans could be 0x00 for false and 0xFF for 1 or the other way around but it should not matter for the program flow. The type is the level we care about. If that is true you can't open the abstraction.  If you can break it for one type you can prove it for everything.

You do this with theorem prover. 

## Actual type system

Many ways to get fancy type system. The most basic thing is non-null & possibly null 

`public @NonNull string hello = "hello"`
Now you said that this has to be not null. It will yell if it might be. 

`public @Nullable string hello = "hello"`
Now you said that this could be not null. It forces you to check. 

This syntax is [[Typing in java in android]].







