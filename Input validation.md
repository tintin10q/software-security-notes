Input validation is validating if input should be accepted. This is OK, but really you should just parse the input and then return it as an object in the language like a struct. This is what you want to do anyways. Otherwise, you are doing double work. 

Have a look [[Input Protections]] for more information about this.


Really validation should just be about checking the crypto.