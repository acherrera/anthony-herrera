---
tags:
  - programming/hacking/tools
  - programming/linux
---
# Hydra

Installed on Kali by default. Used to bruteforce usernames / password by iterating through a list.  


# Formatting calls

Format depends on the type of service we are hitting. For example, ssh looks like this: 

```bash
    hydra -l user -P passlist.txt ftp://MACHINE_IP
```


## SSH

SSH call `hydra -l <username> -P <full path to pass> 10.10.127.253 -t 4 ssh`

```bash
   -l   # For username 
   -P   # password list
   -t   # Number of threads to use
   -V   # Verbose output
   ssh  # Using ssh
```

Making an actual call:

```
hydra -l molly -P /usr/share/wordlists/rockyou.txt $ipadd -t 4 ssh
```

## Post Web Form

```bash
hydra -l <username> -P <wordlist> 10.10.127.253 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

Breaking that down becomes

```
    -l  # single username 
    -P  # Password list
    http-post-form  # Type of form (post)
    /login url  # The login page URL
    :username   # Form field for username
    ^USER^      # Tell it to use username here
    password    # form field for password
    ^PASS^      # Use password list supplied earlier
    Login       # indicates the Login failed message
    Login failed    # login failure message returned 
    F=incorrect # If message appears, login failed
    -V verbose output
```
