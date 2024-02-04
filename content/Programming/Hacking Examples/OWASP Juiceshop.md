---
tags:
  - programming/hacking/practice
---
# Welcome to OWASP Juiceshop 

Let's break some shit


## Get Some Info

See notes down below for information on users

The reviews show the emails of people who have written the review. That's convenient. Even more so, the admin has left a
review. His email is: `admin@juice-sh.op`

When we search for items the query looks a like `http://10.10.23.199/#/search?q=dd`.

Jim is a user with the email `jim@juice-sh.op`


## Logging in via SQL injection

This site vulnerable to an SQL injection that will allow you log in as any user. Run the log in through burpsuite and
find where the username is being passed. Instead of actual username change it to `' or 1=1 --` which will complete
whatever the request was supposed to be via the `'` character, then force a True station and exits. We might consider
that the log in is something like 

```sql
-- YES, I know this isn't valid SQL, just showing the logic --
if  '<username>' in users and password == username.password
```

and we have now changed this to:

```sql
if 'username' or true -- 
```

Now everything is just a comment and the query will return True

We could also just pass a valid username and then comment out the rest of it by setting the username to `<username> '
--`. Here we just remove everything after the username and we're done


## Breaking into the admin account

We have successfully logged in as the admin user, but we don't know the admin password. Let's find it. Here we are going
to brute force the password. Let's run it through burpsuite intruder! Find the log in attempt that we used earlier and
send it to the intruder. Clear the funky-signs and then replace the password with two funky-signs (I don't know what
they are called, pretty sure they are paragraph signs)

Load in the password list and let it rip. This will take a while if you don't have the professional edition. Which I
don't. 

After trying a bunch of passwords we find out the account password is `admin123`. Log in and get the flag


## Breaking Into Jim's Account

Jim is a big Star Trek fan apparently. Let's see if we can use that fact to our advantage. We can reset passwords just
by entering some info. In this case it is "what is your oldest brother's middle name?" I would assume that we already
tried looking it up and getting that info. So, I wonder if anyone in Star Trek has a brother. Yes, James Kirk has an
older brother with a middle name of Sameual. That worked! Password changed to `asdfasdf`

# Exploiting Lack of Encryption

Data should be protected as it is traveling from A to B, though that does not always happen. Let's see what we can do
with that.

If you go to their "about us" page and check out the page you can see that there is an ftp site that is accessible.
Interesting.... This FTP store is where the terms of service information is stored. Cool, go back to the home page and
get your secret flag.


## Log in to McSafeSearch

Watch a video and notice that he mentions his password is Mr.Noodles but has replaced some "vowels into zeros" which,
duh, o=>0. Let's try `Mr.N00dles` for the user mc.safesearch@juice-sh.op


## Poison Null Btyte

This one sounds fun. In this case we will go to the FTP site and try and download the file `package.json.bak` but they
have limited us to only .md and .pdf files. Well, we can slip a "poison null byte" in there and get around that. A
poison null byte is `%00` and we can sip that into the file name. The URL is encoded so we need to encode the PNB as
well. %00 => %2500. The new URL will look something like `10.10.227.190/ftp/package.json.bak%2500.md`. This works
because the validator will see that is end in `.md` but the actual file that is returned will be the string terminated
at %00.Fancy.


## Privilege Escalation

Modern systems allow multiple users to be on a single system and we can use this to our advantage. There are two types
of "Broken Access Control" movement. `Horizontal` is where you move to a user with the same privileges and `Vertical` is
where you move to a user with more privileges. 



## Notes

### Admin 

    - email: admin@juice-sh.op

### Jim

Has a review
 
 - email: jim@juice-sh.op
 - mentions replicator from star trek
