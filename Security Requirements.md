
The crucial first step in any security discussion! To talk about [[Security Lifecycle]] you have to do [[Threat modeling]]. 

## What are your security requirements?
What does it mean for your systems to be secure.

- Someome like me could not change things in ways that we don't want. 
- Look at the models in the database and set up the rules
- The server under the stairs should not be vulnerable

## What is your attacker model?

Against what does the system have to be secure.  

## Who
- People like the ctf team 
- Goverments
- Malicious insider

## What
This is the attack surface. Things you want to protect. 

### Motivations of an attacker. 
- Money 
- Fame

## Trust assumptions
You typically have to make some assumptions. Like, we can trust the programmers on your team to do x. 


# Ways of making security requirements

You can say this app can not be hacked this is  a generic default  requirements but this is quite vauge.

## More  specific security requirements 

It is good to think of this as confidentiality, integrity and availability. 

- How do we prevent people spamming our API - Availability 
- What data of the user table should be secret or accicible to who
- How do we ensure integrity of users tokens.

It is better think of this in terms of authentication and authorization. How do you authenticate people and then what are they authorized to do. How do we know someone is you and what can you do when we said that you are you. 

The thing is, to do the jwt in a secure way you have to check the blacklist of tokens anyways. So you might as well just store the tokens that are active instead in something like redis or something. 


## Prevention vs Detection & Reaction

Often overlooked is the detection and reaction of fuckery. Even 













 