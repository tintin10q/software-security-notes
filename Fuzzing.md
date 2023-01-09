![[Pasted image 20230104212415.png]]

Fuzzing is trying a bunch of inputs to a program to find inputs that crash the program. This looks for memory bugs. Fuzzers include **Radamsa, zuff** and also better ones like **afl(++)** and **HugFuzz**. 

You can also use instrumentation like (ASan, MSan or valgrind) to find additional checks on memory safety.

The idea is to send really long inputs or with sql you can try weird sql so yeah or %p. The fuzzer will generate weird inputs for you. 

1. Basic fuzzing is with long random inputs.
2. Dumb mutation fuzzing - OCPP
3. Generational fuzzing - grammar based fuzzing
4. Code coverage guided fuzzing with afl 
	- grey box fuzzing or smart mutational fuzzing. 
 5. Whitebox fuzzing with sage using symbolic execution

## Basic fuzzing 

- Long inputs 
- High or low numbers 
- nulls 
- newlines 
- end of line charactsers
- %s %x %n 
- halt 
- root

## Pros of fuzzing
-  Easy and little effort
- Can quickly give good indication of robustness of the code
- There are really bugs and not false positives like with [[Tool Prefast]]

## Cons of fuzzing
- Only finds shallow bugs not deeper bugs. 
- Does not exploit buisness logic of sut. Especially if the program is statefull. 
- Smarter fuzzing is needed for complex bugs
- Crashers are hard to analyse 

## Mutation based fuzzing

Mutation based fuzzing starts with a valid input and then mutates it slowly to try to get past the inital checks. So mutation based fuzzing applies random mutations to valid inputs. You can observe network traffic and then replay with some modifications. This is more likely to produce random inputs. 

- FrankenCert makes really complex certificate chains. 
- This takes more work to contruct this fuzzer or grammer for a fuzzer

## Evolutionary greybox fuzzing

With greybox fuzzing you observe execution to try to learn which mutations are intresting. This is what **afl** does. You make random mutations to the input. Then you check if you hit new branches in the code and then thats a good input and you keep going with that. Does not use symbolic execution. 

This is called greybox fuzzing, evolutionary fuzzing or coverage guided fuzzing.

### AFL
AFL works by making a 64k bitmap. Each control flow is mapped to change the bit map. Different executions could result in the same bitmap but the chance is small. Then after a mutation they look if the branches changed. You can either instrument the code by compiling it with afl or by running it in an emulator.  You also have **mutation stratagies** like maxint, or minint or bit flips or 0 or -1 or user specified dictionary. The fuzzer also forks the sut to speed up fuzzing. It collects hangs and crashes. It can learn jpg by starigin from hello world.

Its called grey boxing because we are not really looking at the code just a little.

## Generation/grammar/model-based fuzzing

Generate inputs that are malformed or hit corner cases based on knowlege of input format protocol. You can use regular expression, context free grammar or some other descripion. Once you have the grammar you try to create inputs that are just outside the grammer to test weak for parsers. 

> Examples of generation fuzzing are:
> Snooze, SPIKE, Peach, Sulley, antiparser, Netzob

These tools are either made for one grammar or you can upload a grammer. 

## Whitebox fuzzing 

Whitebox approaches analyse source code to construct inputs. If we know the code then by analyzing the code we can find intresting input values to try.  Sage from microsoft research uses symbolic execution of x86 binaries to generate test cases.

### Symbolic execution

Basically you do algebra on the function inputs and values. This is possible because of the source code.

![[Pasted image 20230105003120.png]]

By doing symbolic execution you get path conditions. Then you give these conditions to set solvers (SMT solver) like Yikes or Z3 to make values that satisfy the constraints that make if statements true. This gives you values that hit all the branches. This is really great. Test data with good coverage. However at some stage it becomes too complex with recursion.   


This is also how [[Tool Prefast]] works behind the scenes. 

Also you can't really deal with apis. 

#### Sage 

![[Pasted image 20230105121916.png]]

From these paths it makes a tree of inputs that mutate the original input to hit the branches. This tree is given to the solver and 

![[Pasted image 20230105122143.png]]

This then gives you optimal test coverage. Sage is quite successfull. The bugs are often parsing errors.

## Black box fuzzing

With black box fuzzing you don't have the source code.



Ideally checks for both spatial bugs ([[memory bugs|buffer overruns]]) and temporal bugs [[Memory Bugs|Malloc and free]] bugs

## Exploiting

You have tools like angr that try to find security flaw from afl. You also have automatic patch creators.

## Conclusions

- Fuzzing is a great technique to find (a class of) security flaws.
- If you ever write C(++) code you should fuzz it
- The challenge is to get good code coverage without too much effort, you use
	- **White box fuzzing** based on symbolic execution with sage
	- **Evolutionary fuzzing/coverage guided greybox fuzzing** with **afl**.


