# No need for Brutus

## Description
```
A simple message for you to decipher:
squiqhyiiycfbudeduutvehrhkjki
Submit the original plaintext hashed with MD5, wrapped between the usual flag format: flag{}
```

## How to solve the CTF:

This message seems encoded. 

Lets visit https://www.cachesleuth.com/multidecoder/.

Paste the encoded string 

```
Input: squiqhyiiycfbudeduutvehrhkjki

Output: A simple message for you to decipher: caesarissimplenoneedforbrutus
```

Lets take the string and make a md5 checksum of it.

This could be done here: https://www.md5.cz/

Once we have the checksum we can wrap it within flag{}

flag: Flag{redacted md5 sum}


