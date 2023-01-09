
## Security testing is harder than normal functional testing

-  We don't know what we are looking for and the issue can be hard to find and very specific
- Normal users are good testers as they will complain about functional bugs but not security bugs.


## Security testing are easier than normal functional testing

- The properties are really easy and basic like it should not crash 
- [[Fuzzing]] is the great success story in software security in the past decade. 

# Testing

A program that is being tested is called a SUT. System under test. To do testing you need 2 things. 

1. test suite - a collection of tests also called test corpus 
2. test oracle - to decide if response is ok or refeals an error, some way if the SUT behaves how we want. 

For the fuzzing the test oracle is just does the program not crash. But for other programs making the test suite and orcale is a lot of work. 

Code coverage criteria can measure how good a test suite is. You can have **statement coverage** which checks if every line is tested. You also have branch coverage which is if every branch is covered. So an if statement is one statement but 2 branches. Yes we do the if or we don't do the if. 

Statement coverage needs 1 test case, branch coverage needs 2. Code coveage metrics can also be used to guie test case generation. Where to fuzz.  

One problem is that test criteria can discourgage defensive programming because it means you also have to write more tests for them.

The [[Tool Prefast]] sal annotations can be used to generate automatic run time checks. The annotations proivde a test oracle for free!

## Testing vs Security testing 

With testing we want to know if the functionality works. With security testing we want to know if the system can be abused. 

![[Pasted image 20230104214904.png]]


Normally you test normal inputs and a bit weird inputs. But for security you have to also check negative inputs. Or basically negative test cases. These are test cases which are supposed to fail!

# Negative Testing 

Negative test cases are tests that are supposed to fail. So negative tests succeed if the sut gives an error.  Postive tests succeed if the sut does not give an error. 

So you want to check that a function gives an error on a certain input you use a negative test.


## Model based testing

[[State Machine learning]]