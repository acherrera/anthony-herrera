---
tags:
  - programming/hacking/tools
  - programming/windows
---

# Let's get to it
# Scan the machine
```
    nmap -sV -sC -script vuln blue.nmap XXX.XXX.XXX.XXX.XXX | tee nmap.log
```

`-sV` shows version `-sC` runs common scripts, `-script` runs the specific script names "vuln". Unsure what `blue.nmap
does and finally give the IP address to scan. The `| tee nmap.log` turns the output into a log file

# Use eternal blue to break in

```
    search ms17_010
    use 3 # or whatever
```
