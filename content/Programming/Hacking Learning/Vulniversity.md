---
tags:
  - programming/hacking/practice
  - programming/hacking/networking
  - programming/linux
---

# Vulniversity

Will keep notes of what is found in this document as well 

## Step 1 - nmap

Get some preliminary information with `nmap`
See [NMAP Basic](NMAP%20Basic.md) and [NMAP Advanced](NMAP%20Advanced.md)

```
    nmap flag	    Description
    -sV	            Attempts to determine the version of the services running
    -p <x> or -p-	Port scan for port <x> or scan all ports
    -Pn	            Disable host discovery and just scan for open ports
    -A	            Enables OS and version detection, executes in-build scripts for further enumeration 
    -sC	            Scan with the default nmap scripts
    -v	            Verbose mode
    -sU	            UDP port scan
    -sS	            TCP SYN port scan
```

`nmap -sV xx.XX.XXX.XX` output is shown in file nmap_sV.log

## Step 2- GoBuster

The URIs on website are not usually all listed in one place, so we want to break open the directory (URI) structure.
This is what GoBuster does. 

Will need to have this installed (sudo apt-get install gobuster on Kali). Will also need a wordlist to search. This can
be found in `/usr/share/wordlists`. Use the dirbuster list for directory names

Example command `gobuster dir -u http://<ip>:<port> -w <word list location>

```
GoBuster    flag Description
-e          Print the full URLs in your console
-u          The target URL
-w          Path to your wordlist
-U and -P   Username and Password for Basic Auth
-p <x>      Proxy to use for requests
-c <http cookies> Specify a cookie for simulating your auth
```

# Step 3 - Burpsuite

I don't know what this is - going to do the room and get back to you on this. See [Burpsuite](Burpsuite.md)

Update - did the room, now going to use burpsuite to bust this site wide open

Want to find which extensions are allowed - going to create our own list and try them out by running a sniper attack

We found that `phtml` files are allowed to be upload. Now we are going to get the php reverse shell script, update the
IP address with our currenty IP address and upload it to the server via the upload page. 

Use netcat to listen for incoming connections `nc -lvnp 1234` from the target server.

