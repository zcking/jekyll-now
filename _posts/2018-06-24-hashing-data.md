---
layout: post
title: "Hashing: Data Privacy and Authentication"
categories: programming cybersecurity
---

Today's post is going to be dedicated to "*hashing*", a term used pretty
loosely in the computer science field, yet one that is an absolute
must-know if you're ever going to be a serious developer.  

If you haven't heard of hashing or have no experience with the topic,
consider this read my treat. I will describe the function of hashing, what it is
used for and why, and demonstrate how to use it with examples in Python and Java.

---

## What is Hashing?
A hash function, or algorithm, is a **one-way** function that transforms
an input data *of any size* into a *fixed-size* output, called the "hash."
It's very important to realize that a hash function is a **one-way** function;
that means you *cannot* get the original input back after you've hashed it.
The reason for this, is the hash function has the potential for collision.
Collision is when two or more inputs yield the same output, as the following
diagram from the [wikipedia page](https://en.wikipedia.org/wiki/Hash_function):  
![hash function diagram](https://upload.wikimedia.org/wikipedia/commons/thumb/5/58/Hash_table_4_1_1_0_0_1_0_LL.svg/240px-Hash_table_4_1_1_0_0_1_0_LL.svg.png)

Different hash algorithms will have different probability for collision, but
it's important to acknowledge they all exhibit the quality. If you consider the
definition of the hash function as I just described, this unavoidable risk of
collision is more understandable--the range of inputs (any size data)
outweighs the range of outputs (a fixed size).

## Why Use Hashing?
Before answering this question, allow me to show what the actual data might
look like when hashing. The input might be just a string such as `"foobar"`
and the hash output might be `8843d7f92416211de9ebb963ff4ce28125932878`.

Hashing is used for three primary purposes (this list is *not* exhaustive):  
1. Obfuscating private data (e.g. [Personally Identifiable Information (PII)](https://en.wikipedia.org/wiki/Personally_identifiable_information))
2. Authenticating, or verifying, submitted data (e.g. secure password management)
3. Building caches and data structures for large data sets

For example, a typical password management approach, while not 100% unbreachable,
is to generate a *salt* (a random sequence of bytes) and hash the salted password,
store *that* hash, and when a login request comes in, you can verify by salting
the inputted password and comparing its hash with your stored hash:  
{% highlight javascript %}
// Create account
salt = generateSalt()
passHash = hash(salt + password)
store(username, salt, passHash)

// Authenticate login
salt = getSalt(username)
expectedHash = getHash(username)
formedHash = hash(salt + passwordInput)
return (expectedHash == formedHash)
{% endhighlight %}

In that scenario, the storage is arbitrary so `store(...)` might store
the data in a relational database or whatever other storage entity you're using.
The same goes for fetching the data with `getSalt(...)` and `getHash(...)`.

## Common Hash Functions
Now that we've covered the basics of hashing, what hash functions, or algorithms,
are available to us and which one is right for you? It all depends on the context.
Below is a table of a few of the most commonly used hash algorithms, their
attributes, and what they're commonly used for:  

| Hash Algorithm | Attributes   | Uses  |
| ------------- |:-------------:| :-----|
| [SHA1](https://en.wikipedia.org/wiki/SHA-1)    | 160-bit (20 bytes); medium speed; minimally secure  | minimal security of data rest; data verification |
| [SHA-256](https://en.wikipedia.org/wiki/SHA-2) | 256-bit (32 bytes); not-the-fastest; more secure than SHA1 | good security of data at rest; good authentication; data verification |
| [SHA-512](https://en.wikipedia.org/wiki/SHA-2) | 512-bit (64 bytes); slow; very secure | very strong data security for data at rest; strong authentication; data verification |
| [MD5](https://en.wikipedia.org/wiki/MD5) | 128-bit (16 bytes); very fast; *not secure* | data verification (checksums)
| [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) | variable length; used in conjunction with SHA; iterative hashing | excellent for password storage |
| [Bcrypt](https://en.wikipedia.org/wiki/PBKDF2) | variable length; used in conjunction with other hash algorithms | excellent for password storage |

&nbsp;

---

## Examples
Most languages have built-in support for hashing, and as you'll see in the examples
below it is very straight-forward.

### Python (3.x)
```python
#!/usr/bin/env python3
import hashlib

# SHA-1
sha1 = hashlib.sha1()
sha1.update('foobar'.encode('UTF-8'))
print(sha1.hexdigest()) # 8843d7f92416211de9ebb963ff4ce28125932878

# SHA-256
sha256 = hashlib.sha256()
sha256.update('foobar'.encode('UTF-8'))
print(sha256.hexdigest()) # c3ab8ff13720e8ad9047dd39466b3c8974e592c2fa383d4a3960714caef0c4f2

# SHA-512
sha512 = hashlib.sha512()
sha512.update('foobar'.encode('UTF-8'))
print(sha512.hexdigest()) # 0a50261ebd1a390fed2bf326f2673c145582a6342d523204973d0219337f81616a8069b012587cf5635f6925f1b56c360230c19b273500ee013e030601bf2425

# MD5
md5 = hashlib.md5()
md5.update('foobar'.encode('UTF-8'))
print(md5.hexdigest()) # 3858f62230ac3c915f300c664312c63f
```

### Java
```java
// SHA-256
MessageDigest sha256 = MessageDigest.getInstance("SHA-256");
String text = "foobar";

sha256.update(text.getBytes(StandardCharsets.UTF_8));
byte[] sha256Digest = sha256.digest();

String hex = String.format("%064x", new BigInteger(1, sha256Digest));
System.out.println(hex); // c3ab8ff13720e8ad9047dd39466b3c8974e592c2fa383d4a3960714caef0c4f2

// MD5
MessageDigest md5 = MessageDigest.getInstance("MD5");
md5.update(text.getBytes(StandardCharsets.UTF_8));
byte[] md5Digest = md5.digest();

hex = String.format("%032x", new BigInteger(1, md5Digest));
System.out.println(hex); // 3858f62230ac3c915f300c664312c63f
```

Hopefully now you have a good understanding of hashing, and are prepared to
use it where necessary. For more hashing-related topics check out the links below:  
* [HMAC: Hashed Message Authentication Code](https://en.wikipedia.org/wiki/HMAC)
* [Password Storage Cheet Sheet](https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet)
* [Hash Functions (Wiki)](https://en.wikipedia.org/wiki/Hash_function)
