---
tags:
  - programming/hacking/tools
  - programming/general
---
# Cryptography 101

Terminology:
- Plaintext: data before encryption or hashing. Usually text, but could be anything
- Encoding: NOT encryption: just the form the data takes (base64, hexidecimal)
- Hash - The output of a hashing function. Can also be a verb "to hash"
- Brute Force - just try everything and hope it works
- Cryptanalyis - analyzing the encryption and finding a weakness

![Attack Vector](./img/hashing_vectors.jpg")

## Hash Functions

Different from encryptions. No key, meaning it should be impossible to go from the output back to input. Hashing is
typically used to store and retrieve passwords. I **think** they hash the input and compare it to the stored hash. In
this way, they never actually deal with the passsword, just the hash. Steal the hash and you got nothing! Right? 

Hash collision is when two inputs give the same output. There are only so many outputs for each hash function, but an
infinite number of inputs. In this way, there will HAVE to be some overlap. MD5 and SHA1 have both been attacked and
made insecure due to collisions. 


## But Why Hashing?

Use for data integrity verification and for password verification.

Storing a password in plaintext would be bad. Databases get broken into and people are dumb and reuse passwords all the
time. The "rockyou.txt" file on Kali was due to a plaintext data breach - over 14 million passwords leaked!

Adobe has passwords leaked because they were not using a good hashing. Linkedin had a hack because they were using SHA1
which is quick to break using GPUs. Storing some kind of key is a bad idea as well since if you lose the key, everything
becomes open.

Hashing comes in now - you stored the hash instead of the actual password so even if they have the hash, they can't
directly get the password. That's great until you realize that the password translates directly a hash. They have
rainbow tables that can be used to look up the password=>hash relationship. A few examples:

zxcvbnm => 02c75fb22c75b23dc963c7eb91a062cc
11111 => b0baee9d279d34fa1dfd71aadb908c3f

This is called a rainbow table. There is Website called Crackstation that has a HUGE rainbow table to return fast
password cracking. 

## How To Determine Hashing Type

Online tools exist: https://pypi.org/project/hashID/ but can be unreliable. If the hash a prefix, the tools can be
pretty reliable. If they are not prefixed, then they don't really help that much. This is where using your brain
comes in. You have to know a bit about hashes though. Unix hashes - very easy since they are prefixed. The format is
`$format$rounds$salt$hash`. So, yeah, look for that.

Windows uses NTLM, a varient of MD4. Looks like same as md5 and md4, so use your brain on that one.

Linux passwords are stored in `/etc/shadow` and normally only readable by root. They used to be storeing in
`/etc/passwd` and were readable by everyone Shocking that went out of style

Windows password hashes are stored on the SAM. You're not supposed to get to them, but mimikatz can help with that.

Hash types and examples can be found here: https://hashcat.net/wiki/doku.php?id=example_hashes


## Password Cracking

Rainbow tables are great, but what if the passwords are salted (making the rainbow table useless)? In this case, we just
bang on the front door and brute force it. hashcat and John the Ripper are usually used for this. Use a dang GPU t0
crack passwords. 


### GPUs

 Use a GPU - it is significantly faster than using the CPU. You can get GPUs running on a virtual machine but it is a
 real pain the butt. Even if it does work, it will likely be much slower than just running on a host machine. 

NOTE - never use `--force` for hashcat. Just don't It's not good.

Alright, let's get cracking

 -  $2a$06$7yoU3Ng8dHTXphAg913cyO6Bjs3K5lBnwq5FJyA6d01pMSrddr1ZG

Okay, we can see this is a Linux hash of some kind, let's just run it through CrackStation. Nothing. Using the John the
Rippers `john ./01_passwordfile` start it going. But it takes up a LOT of power. Need to get the GPU running.

alright, let's narrow down the hashing type. 

```
Prefix	Algorithm
$1$	                            md5crypt, used in Cisco stuff and older Linux/Unix systems
$2$, $2a$, $2b$, $2x$, $2y$	    Bcrypt (Popular for web applications)
$6$	                            sha512crypt (Default for most Linux/Unix systems)
```

So, this would be a Bcrypt hash.

There's a lot of those and they take a while, so let's move on while that processes...

### John The Ripper

See [John The Ripper](John%20The%20Ripper.md) for more

Well, found out a LOT about John the Ripper while I was trying to get everything working. Set up my desktop basement
server, got it running, it would fall asleep on my, got that fixed, got GPU support, removed it, repeat a few times,
installed CUDA, built John from scratch, didn't find CUDA support, didn't even try, found out that OpenCL is used
instead of CUDA since everything support OpenCL now, got it running with OpenCL - didn't work, tried a few times, didn't
work. 

### Hashcat

Had to look up the syntax to get hash cat working. Got the syntax right and it was cracked in ONE SECOND. Oh man. I'm
going to pretend like all the GPU work is what made Hashcat work out of the box so well.

The command was: `hashcat -m 3200 ./01_passwordfile  /usr/share/wordlists/rockyou.txt`

Bonus - monitor the GPU usage: `watch -d -n 2 nvidia-smi`

As a bonus bonus, I attempted to crack the password on my local Kali linxu - 3 core virtualbox machine. And that took 17
seconds. Which, sure, isn't a lot but that is 17 times longer! Image trying to crack multiple passwords at 17 seconds
each vs 1 second. 

Trying on hash number two:
$6$GQXVvW4EuM$ehD6jWiMsfNorxy5SINsgdlxmAEl3.yif0/c3NqzGLa0P.S7KRDYjycw5bnYkF5ZtB8wQy8KnskuWQS3Yr1wQ0

From above, we can see that this is `sha512crypt` hashing. If we go through the hashcast help `hashcat --help` we can
see that this is number is 1800.

DONE!

## Hashing for Data Integity

Because changing one bit in the input will massively change the output of the hashing function, we can make sure that
all the bit received are the same as the ones that were sent - IE: nothing has changed.

