
# Access control 

Access control consists out of two steps:
- Authentication - Checking **who** are
- Authorization - Enforcing what **you** are **allowed to do**

Sometimes when people are talking about access control they are just talking about the authorization. These are very very different problems. Often authentication is outsourced to third parties. 

## Authentication Solutions 

You can authenticate user to system, system to user or system to system or user to user. 

Human to system:
- Pin code
- Password or Passphrase
- Email with a code 
- Biometrics

System to human:
- Cryptography 
- Challenge response protocols
- API keys
- SMB pass the hash

Users to human **(Often forgotten)**:
- Certificates (that no one checks)
- URLs

Phishing attacks often make use of the fact that humans don't authenticate the system. You can also make fake pop-ups.

### How authentication?

Basically a security factor: 
- Something you are
- Something you have
- Something you know
- Something you do (a signature or context)

## Authorization Solutions 

Authorization is often configuring a policy. From this policy, code is generated. The policy can be hard to write. 


### Problems

- Authentication can be weak or broken. Like you can steal credentials. 
- Authorization can be misconfigured.
- Policies can be <span style='font-family:bloody;color:red'>complex</span>
- Authentication is just missing or circumventable
- Not repeated often enough. 

# Information Leakage

Side channel attacks or information leakage is when you leak information in a system. This can be used to recover passwords and secrets. Error messages can leak a lot of information. 

# Race conditions 

![[Race Conditions]]
