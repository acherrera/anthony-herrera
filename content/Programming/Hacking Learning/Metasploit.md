---
tags:
  - programming/hacking/tools
  - programming/linux
---
# Metasploit

Awesome awesome tool, but only allowed to be used on one exploit on OSCP exam


First time running, may want to set up database
```bash
    msfdb init
```

Start up metasploit with `msfconsole`

Search with `search`. Find exploit you want to use and use `use` with the number or full path (number is way easier)

Can use `connect` to check that host is up. Not super useful as there is "netcat" that we could use instead

Can set variables for the module using `set [option] [value]` which tells the module what to use. Use `show options` to
show all the values that can be set. Can also set global variables using `setg [option] [value]` which is useful when
switching modules



# Metasploit commands

After you are in the console, use these commands

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

# Using the built in database

Can use the built in database to keep track of tests. Also have built in nmap that saves data to the database

```
    db_nmap -sV IPADDR
```

Once we do that, we can run a bunch of commands and see what comes back

```
    hosts
    services
    vulns
```

# Busting in

In this case we want to use icecast to bust the box. `use icecast` will load it. Or search for icecast and load from the
number

Windows shell commands
```
    getuid      # More user info
    sysinfo     # More system info 
    getprivs    # user privlidges
    ipconfig    # Get ip config data
```

More metasploit commands
```
    upload      # Move files to target
    run         # run program
    load        # Load program to target
    shell       # get a windows shell to use
```

Useful commands

```
    # Check if virtual machine
    run post/windows/gather/checkvm

    # Check for exploits - can try them after found
    run post/multi/recon/local_exploit_suggester

    # Get a  remote desktop
    run post/windows/manage/enable_rdp
```
