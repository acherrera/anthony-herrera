---
tags:
  - programming_moc
  - programming/linux
  - programming/hacking/networking
---

# This is from the full walkthrough of nmap 

## TCP

Nmap sends  SYN message and if it receives a SYN/ACK, it will respond with ACK to complete the handshake. Filtered ports
*usually* do not respond and close ports respond with RST.

When using `-sS` flag NMAP will do a "stealth" scan where the ACK is not sent back. This can be useful as most systems
do not register a half-complete handshake.

## UDP 

UDP protocol does not have a return when the message is received so the program just has to guess. If no response it
will try again and if no response still, it will be considered open. These scan take significantly longer to run.

# Ping-sweeps

This iterates over all possible IP addresses and checks to see if they respond. Give a list of values to iterate through
or use CIDR notation. CIDR notation?

```
    nmap -sn 192.168.0.1-254
    nmap -sn 192.168.0.0/24
```

`-sn` means to ports are scanned and only ping status is returned
