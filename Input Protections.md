
## Canonicalisation 

Normalize inputs to canonical form. Like if you have dates just store them as numbers. **Like Removing spaces from the end of inputs.**

**You should do canonicalisation first!** Before the other validation or security decisions. But the canonicalisation itself can be abused. For instance with zip bomb. So you should stop after some time. 

## Validation 

Reject 'invalid' inputs. If inputting a date, maybe don't accept things from the past or negative inputs? 

If you want to be more precise you can write down a more precise grammar. Either regular expressions for a context free grammar. But most languages are too complex for this. 

### Validation Techniques 
- Indirect selection = Let user choose from list of good inputs. But has to be checked on backend.
- Deny-listing = List of invalid patterns you reject. Reject if invalid. 
- Allow-listing = List valid patterns and only accept if it matches, more secure

## Sanitization  

Try to <span style="color:red">fix</span> dangerous inputs also called neutralizing. So you could convert `<script>` `&lt;script&gt;`. Validation and sanitization are often confused. Because you can say oh a negative number, lets times it by -1.......

- Replacing \" to \\\". 
- Replacing < with &lt etc
- lowercasing things or uppercasing things
- removing dangerous words, allow or deny list (allow is better)

After sanitization, you should probably validate again.

Removing javascript ..


# Parsing 

If input requires parsing than parse it and don't just validate

Instead of:

`isValidURL(s: string): boolean` you get should have `createURL(s:string): URL` function that returns something immutable. This is much better because:
- with the first method you have to keep validating. 
- With isValidURL your duplicating code because later you will make a URL out of it anyways. So just do it immediately and let it throw an exception. 
- Better type safety because you have to parse it. !!! You should have in other functions except URL and not string. So you can not forget validation and subsequent parsing.
- 

It is more work though because defining parsing is hard but better in the long run.

## Where to do input checking

At choke points like rest api endpoints or public interfaces. But be carefull to not trust what is behind the choke point. Like you should not trust things in your db really. Like img should have integrity with the link so you know it doesn't change. 