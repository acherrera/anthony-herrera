---
tags:
  - programming/hacking/tools
  - programming/linux
---
# John The Ripper

AKA the program that I wasted like half a day trying to get working and then just used `hashcat` and it worked
perfectly. I am hoping that we can get this working correctly and that perhaps my previous attempts were using the wrong
command. Regardless, I DID get John working with the GPU, it just didn't ever actually crack the password. I suspect
this is because it wasn't using rockyou.txt and instead of was using the default wordlist. Rockyou.txt had an UTF-8
encoding issue. Which is annoying.

## Wordlsts

Get you some wordlists! https://github.com/danielmiessler/SecLists

Rockyou is great. Never use a password from that list. Moving on

## Cracking Basic Hashes

There is a file associated with this - download it and extract the contents


Simple method - let the program guess the format and crack it
`john --wordlist=/usr/share/wordlists/rockyou.txt ./first_task_hashes/hash1.txt`

If John is not playing nicely with a hash, manually tell it which has to use. You can find the hash by either googling
an unline has finding website, or downloading this fun tool  
`wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py`

Great, now you can paste the hash into the program (or add it as a command arg) and then see what the hasing type is

`john --format=[format] --wordlist=[path to wordlist] [path to file]`

Now the tricky part is knowing that format to use because `md5` just won't cut it. In order to get the potential options
use: `john --list=formats | grep -iF "md5"` which will show you all the potential opions that match 'md5'

Hash one: type-md5
`john --format=raw-md5 --wordlist=./rockyou.txt ./hash1.txt`

Hash two crack: type - SHA-1

Could use: 
 - raw-sha1
 - mysql-sha1-opencl

`john --format=raw-sha1 --wordlist=./rockyou.txt ./hash2.txt`

Hash Three: type - sha256

This command failed, but the output let me to the one that worked:
`john --format=sha256crypt-opencl --wordlist=./rockyou.txt ./hash3.txt`


This one actually worked.
`john --format=raw-SHA256-opencl --wordlist=../rockyou.txt ./hash3.txt`

I suspect that a lot of the issue I have been having are because I'm putting the format in incorrectly.

**NOTE** - search for `SHA256` instead of `sha256` when grepping the formats. Thiat will help a lot. Took a few tried
of removing the format and reading the output before I noticed the pattern. Duh

Hash Four: type - whirlpool

## Cracking Windows Authentication Hashes

This is something that you might see out in the real world - you get into a computer and get some hashes, but then what? 

Windows stores hashes at NTHash or NTLM (LM being the old version - NTLM just means the newer and the older version.
NTHash stands for "New Technology Hash" which was a designation Microsoft things that were different than the MS-DOS of
old. Now it lives on in weird corners like this.

You get the hashes by dumping the SAM using something like Mimikatz or the AD istelf. You might be able to just pass the
hash directly, but give it a shot to crack it and see what happens.

In this case we will need to set the hashing method to "NT" which I found by grepping for a few variation of "NTLM"
including "NT".


## Cracking Passwords from /etc/shadow

`/etc/shadow` is the location of the password file for Linux machines. In order to get to it you must have root, but
once you get it, there may be passwords that can be cracked pretty easily. In order to do this, you must "unshadow" the
file. John has a built in "unshadower" (?) that can do this. The command will look something like this: `unshadow
local_passwd local_shadow > unshadowed.txt`

Where
- local_passwd: /etc/passwd
- local_shadow: /etc/shadow
- unshadowed.txt: output file

## Single Crack Mode

So far we have worked out passwords based on a given passwords file. But what if the user is attempting to be clever (as
they should be?). For example, instead of "Mark", but if they use "Mark!". Well currently we would not be able to crack
that. Say hell to single crack mode! John will create its own password dictionary based off the info in the username
instead of using a bit wordlist.


John is compatible with GECOS system as well. What's this? It's just a file format where fields are separated by ":" -
see the `/etc/shadow` and `/etc/passwd` file for an example. This way it can feed a bit more information into the single
crack word mangling program.

The password file needs to contains the username at the beginning. For example, if the hash was `1234` and the username
was `mike`:

1234 => mike:1234

Other than that, just pass in the `--single` flag and the format and you should be good to go. 

`john --single --format=[format] [path to file]`

## Custom Rules

We can define our own custom rules if we have an idea what the person might do. For example, it is common to use a word,
but change the first letter and then add some characters at the end. For example `asdf=>Asdf1!`. This is done because it
is much easier to remember than a completely random password. This is often done to make password requirements happy
while also missing the point of have requirements.

We can create custom rules by modifying the file at `/etc/john/john.conf`. If you would like to see a full list of rules
and what they do, check it out here: https://www.openwall.com/john/doc/RULES.shtml

### Creating Custom Rules

First off, open up the file found at `/etc/john/john.conf`. There is a huge level of control, so we will just go over a
little modifiers that we can use

Use regex style pattern matching (there it is again) to define where the word will be modified.

`[List.Rules:THMRules]` will create a rule called "THMRules". Modifiers we will use:

`Az` - Takes word and appends characters

`A0` - Take word and prepends

`c` - Capatilizes character positionally

Finally, will tell it what we actually want to append or prepend. Examples:

`[0-9]` - Will include numbers 0-9

`[0]` - Will include only the number 0

`[A-z]` - Will include both upper and lowercase

`[A-Z]` - Will include only uppercase letters

`[a-z]` - Will include only lowercase letters

`[a]` - Will include only a

`[!£$%@]` - Will include the symbols !£$%@

Finally, let's make a rule!

```
[List.Rules:PoloPassword]
cAz"[0-9] [!£$%@]"
```

Breaking this down:

`c` - capitalize the first letter
`Az` - append to the end of the word
`[0-9]` - a number in range 0-9
`[!£$%@]` - then add one of these

Next!

## Cracking Password Protected Zip Files

YEP - we can do this as well with John - `Zip2John`. The idea is that we are going to convert the zip into a hash and
then crack that hash. Usages goes:
`zip2john [options] [zip file] > [output file]`

Or

`zip2john zipfile.zip > zip_hash.txt`

This should look pretty familiar - this is similar to what was done with the unshadow program. After that we can feed it
into John and get the output.

Doing this was pretty easy - do the steps about and then run 

```
john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt
unzip secure.zip 
# enter password from step one
# done
```

## Cracking Password Protected RAR Archives

Given what we just did, it should be easy to figure this one out. 

```
rar2john [rar file] > [output file]
```

Then, crack the output file, get the password and open the file

## Cracking SSH Keys

This comes up semi-frequently in CTF challenges. Using John to crack SSH private key passwords of id_rsa files. Usually
you authenticate over SSH using a password. However you can also use a key-based system which uses a private key, id_rsa
to authenticate over SSH. Sometimes you will also need a password - that is, just getting the SSH key not enough since
there is yet another layer of protection. This is where John comes in.

We can use `SSH2John` to convert the SSH key into something that John can break open. If this is not on the system, there
is `/usr/share/john/ssh2john.py` that can be used instead. 

Format is the exact same as above: `python3 /usr/share/john/ssh2john [id_rsa private key file] > [output file]`

