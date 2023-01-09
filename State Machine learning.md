
You got stateless system and stateful systems. 
Stateless systems have no memory and always give the same aswer given the same input. Like a PDF viewer. 
Stateful systems have memory and can give different results on the same inputs. Because they also have memory which is like a hidden input, or you could see it as the programming never stopping. Examples are protocols. 

The stateful systems are interesting because bugs might exist only at certain states. 

For protocol, you can either have malformed messages (parsing bug [[Input validation]]) 

--- 
Protocols are often defined in terms of BEA, Bob Eve and Alice, but this is simplistic as it only pictures the happy flow of the protocol. If anything else happens the protocol must abort. But with not crypto things there are usually many more states. 

So it is appropriate to express these protocols with a state machine:

![[Pasted image 20230107211902.png]]

A deterministic finite automata (DFA). Still an over simplification because it only describes happy flows. IRL if things go wrong you have to go back to the beginning. This is called being **input enabled**. Any implementation of the protocol has to be input enabled. 

What is very important is that the state machine should ignore messages that are not relevant to the current state (arrow from the current state). This often goes wrong.  At any stage we can get any message and the implementation has to do something either ignore or go to beginning or an error state. 

![[State Machine learning ssh key.png]]

Typically, however, the specification does not show state machines. People draw them themselves and then draw different ones. This leads to **fingerprinting** opportunities because implementations might have the same result and no bugs but still different state machines which can be detected. Like TCP on windows and Linux. 

## State machine learning 

You can do state machine learning using the **L\*** algorithm (learnLib). This is a form of stateful fuzzing. You give a lot of inputs and slowly map out the state machine. This is black box fuzzing. Humans actually do this a lot to discover the state machine without reading the manual. Try inputs and observe outputs. **Automated reverse engineering**, learning the protocol without reading spec.

---
To do this you need an alphabet of inputs to know what to try. Like a list of valid messages.

The trick is as follows. 
If you give input A, and then you get result X. 
Then you give input A again, and you get result Y. **Now you know A changed the state**

If the result doesn't change the state probably doesn't change. So the state is where you are in the state machine.  

![[Pasted image 20230107215133.png]]

The state machine is only an under approximation of the real system because it could be that if you send `A` 100 times that then you actually get something different. 


# Example Credit cards

You can do this on credit cards that have a really big spec. Like 700 pages. But apparently it's a simple alphabet and the messages are not encrypted, so you can replay them. Some inputs you don't do like the wrong pin because then it might block the card.

Then you get this:

![[State Machine learning bank card.png]]

Clean this up a bit by merging arrows with the same response:

![[State Machine learning crdit card 2.png]]

Merging arrows with same start and end state for the happy flow:

![[State Machine learning credit cards 3.png]]

This is the state machine for the cards. No bugs but different state machines between cards.

![[State Machine learning rabobank vs volksbank.png]]

The cards get tested a lot but only for competability, if you get there in a different way it doesn't matter. If you have more branches than necessary ts still complient but maybe not secure. 

# Take away

So the state machines should be in the protocol.

![[Pasted image 20230107221014.png]]


So the specifications are text and people write code for that, and they extracted the state machine from that.

![[State Machine learning ideal.png]]