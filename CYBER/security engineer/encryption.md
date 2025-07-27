# ceasar style encryption

base if encryption .
allow us to discuss by changing each letter by another .
it can use ceasar cipher (pass to X letter) or column based cypher.
the site quipqiup can solve this type of problemes .

# Synetric Encryption :

this is the most common encryption .
the user and the server use a key to crypt the info .
it's impossible to read the data without the key .
this encryption use :

-   Key
-   Cipher : the algo
-   plainText : the text no encrypted
-   cipherText : the text encrypted

the key is used to encrypt and decrypt . the ceasar style is an symetric encryption .

-> DES : a 56 bit key not secure anymore (can be broken in ~ 60h), have been replaced by 3ds but no secure anymore too .
-> AES : it's a standard encryption algo . can use 128,192,256 bit key more the key have bit more is secure but the computer will take more time to encrypt / decrypt the data .

in linux we have two command to encrypt or decrypt data :
-> openssl
-> gpg

# asymetric encryption

the asymtric encryption allow to use 2 key . one key to encrypt the message (known as public key) and another to read the message (the private key).
we can't find ine key with another .

the most common algo is RSA .

## RSA

to encrypt the data we use prime number that allow us to generate key

1. Bob chooses two prime numbers: p = 157 and q = 199. He calculates N = 31243.
2. With ϕ(N) = N − p − q + 1 = 31243 − 157 − 199 + 1 = 30888, Bob selects e = 163 and d = 379 where e × d = 163 × 379 = 61777 and 61777 mod 30888 = 1. The public key is (31243,163) and the private key is (31243,379).
3. Let’s say that the value to encrypt is x = 13, then Alice would calculate and send y = xe mod N = 13163 mod 31243 = 16341.
4. Bob will decrypt the received value by calculating x = yd mod N = 16341379 mod 31243 = 13.

from a n we can try to find the rest of the parameter from the website FactorDB > Search
to calculate private key from p and q we can use dcode.fr

a key file is generated with .pem extension .

```bash
#to find info of a private-key we can:
openssl rsa -in private-key.pem -text -noout
#to encrypt a file we use :
openssl pkeyutl -encrypt -in plaintext.txt -out ciphertext -inkey public-key.pem -pubin
#to decrypt a file we use :
openssl pkeyutl -decrypt -in ciphertext -inkey private-key.pem -out decrypted.txt
```

## Helf Difman Key

when communicate over an insecure channel how can we exchange key ?
difman allow to do that. we can exchange some data and keep some other private. only these that created the private value know what is the value and these value can't be found with th public one .

1. Alice and Bob agree on q and g. For this to work, q should be a prime number, and g is a number smaller than q that satisfies certain conditions. (In modular arithmetic, g is a generator.) In this example, we take q = 29 and g = 3.
2. Alice chooses a random number a smaller than q. She calculates A = (ga) mod q. The number a must be kept a secret; however, A is sent to Bob. Let’s say that Alice picks the number a = 13 and calculates A = 313%29 = 19 and sends it to Bob.
3. Bob picks a random number b smaller than q. He calculates B = (gb) mod q. Bob must keep b a secret; however, he sends B to Alice. Let’s consider the case where Bob chooses the number b = 15 and calculates B = 315%29 = 26. He proceeds to send it to Alice.
4. Alice receives B and calculates key = Ba mod q. Numeric example key = 2613 mod 29 = 10.
5. Bob receives A and calculates key = Ab mod q. Numeric example key = 1915 mod 29 = 10.

even with q,g,A,B it's impossible to find a or b ! but the probleme if there is a MIM attack .

```bash
#to generate a p,g we can use :

# to see a key
openssl dhparam -in dhparams.pem -text -noout

```

# hashing

the ashing allow to text an input and give an ouptput of constant size for all input size .
md5 is a non secure hashing, hashing secure SHA224, SHA256 ...
from the hashing value we can't recover the input value .
the hash allow 2 things :

-   check if the file we have is the file we want (compare hash value)
-   save password of user in non plaintext

## HMAC :

a hashing function based on secret key .
a hmac need :

-   secret key
-   inner pad (constant number defined by the RFC)
-   outer pad (same)

1. Append zeroes to the key to make it of length B, i.e., to make its length match that of the ipad.
2. Using bitwise exclusive-OR (XOR), represented by ⊕, calculate key ⊕ ipad.
3. Append the message to the XOR output from step 2.
4. Apply the hash function to the resulting stream of bytes (in step 3).
5. Using XOR, calculate key ⊕ opad.
6. Append the hash function output from step 4 to the XOR output from step 5.
7. Apply the hash function to the resulting stream of bytes (in step 6) to get the HMAC.

there are two command to generate hmac

```bash
hmac256 key message
sha256hmac message --key {key}
cat {file}| openssl sha256 -hmac "{key}"
```

# SSL

I will make a SSL tuto.
the ssl allow to store a public key in a safe place already known by the authority .
to prevent MIM we can send out cert (public key) in a place already coded in the os .

to check a cert we can use

```bash
openssl x509 -in cert.pem -text
```

# SALT

when a server need to store password we hash the password to be sure in case of data breach we don't give password of users .
the problem is rainbow table .
the rainbow table is hashing password table with key value so with a hash we can find the value .
create a rainbow table is slow . these value are precomputed .
to prevent rainbow table attack we can use a salt a new text appended to the password to be sure that there is no rainbow table with the apssword event if the hacker know the value of the salt make a new rainbow table will be impossible .
hash = hash(pass + salt ) or hash = hash(hash(password) + salt)
we can use a key derivation function such as PBKDF2. PBKDF2 takes the password and the salt and submits it through a certain number of iterations, usually hundreds of thousands.
crackstation is a good rainbowtable .
