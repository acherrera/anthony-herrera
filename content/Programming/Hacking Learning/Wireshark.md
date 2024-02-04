---
tags:
  - programming/hacking/tools
  - programming/hacking/networking
---
# Wireshark 

Intercept traffic, see how is talking to who, have a great time. 

Don't get caught. 

## Packet Filter

Packet filtering is a very important part of getting packets. There are many types of filter, that should be pretty
common sense for a programmer: 
 - and: and / &&
 - or: or / ||
 - equals: eq / ==
 - not_queal: ne / !=
 - greater than: gt / >
 - less than: ls / <

Examples of these in action:

```
# Just source
ip.addr == <IP Address>

# Source and destination
ip.src == <SRC IP Address> and ip.dst == <DST IP Address>

# Ports and protocols
tcp.port eq <Port #> or <Protocol Name>

# UDP ports and protocols
udp.port eq <Port #> or <Protocol Name>
```

## Packet Dissection

Great, now what do we do with these packets that we've filtered out? Well, we needs to know the different layers that
might be present:

Application - End User Layer: HTTP, FTP, IRC, SSH, DNS
Presentation - Syntax Layer: SSL, SSH, IMAP, FTP, MPEG, JPG
Session - Synch and send to port: APIs, Sockets, Win Sock
Transport - End-to-end communication: TCP, UDP
Network - Packets: IP, ICMP, IPSec, IGMP
Data Link - Frames: Ethernet, PPP, Switch, Bridge
Physical - Physical Structure: Coax, Fiber, Wireless, Hub, Repeaters, Smoke signals

Got it? Good.

Looking at the above info, we can see that a packet will have 5-7 layers. You can think of the OSI layers like an Onion
with the inner most layer being Application and the outermost being Physical. The Application is wrapped by
Presentation, which is wrapped by Session, which is wrapped by Transport and so on until you get the physical layer.
Which makes sense because the Physical layer phsically wraps the entire message and sends it across. Once the receiver
gets the data, it has to unwrap the onion. Similar to an onion, much crying is involved on those who wish to unwrap it
manually. 


In the context of wireshark, each layeris presented as a separate section. 


# Practical Packet Retrieval

## ARP Packet

Download the example file and filter for ARP packets. Literally just 'arp' in the filter bar.
80:fb:06:f0:45:d7

To filter and find only replys: `arp.opcode == reply`. 

Filter on MAC address: `eth.addr == 80:fb:06:f0:45:d7`

## ICMP Packets

ICMP (Internet Control Message Protocol) is used to analyze nodes on a network. Usually used for ping and traceroute
programs.

Here is the most bland paper you've ever read about this: https://datatracker.ietf.org/doc/html/rfc792

Note that paper was written in 1981 and is the DARPA Internet Program Protocol Specification, which is cool as hell
actually. Just very bland.

ICMP type=8 is a request and type=0 is a reply packet. If these codes are not correct, that can be a sign of suspicious
activity.  Timestamp can be useful and the data field should just be a random string. 

The reply packet should be the same as the request except with a type=0 instead of 8.

## TCP Packets

TCP (Transmission Control Protocol handles delivery of packets along with sequencing and errors. 

If you would like to read the original TCP protocol specification: https://datatracker.ietf.org/doc/html/rfc793

TCP packets are colored basic on the level of danger. The problem is that TCP sends a LOT of data. This is probably why
there is not a file to download and test for this one. Check out RSA NetWitness and NetworkMiner for filtering TCP
filter


## DNS Traffic

DNS (Domain Name Server) resolves names to IP addresses. If you're not familiar with DNS - check this out:
https://www.ietf.org/rfc/rfc1035.txt

Type of packets: Query-Response, DNS-Servers Only, UDP. If any of these are missing, there might be a problem.

## Http Traffic

Want to read the official paper? - https://www.ietf.org/rfc/rfc2616.txt

HTTP is straight forward to analyze. The data is not encrypted and we can see the data call, - easy, peasy.

Useful tools:
```
Statistics > protocol hierarchy # show hierarchy
Statistics > endpoints # Show endpoints used 
```

## HTTPS Traffic

HTTPS is real annoying to understand and can be difficult to analyze. Now, how to do we do that? Well, let's understand
what happens between the client and server. 

1. Client and server agree on protocol version
2. Client and server select a cryptographic algorithm
3. Client and server authenticate each other (optional)
4. Create a secure tunnel with a public key

What this means is that we need to capture packets from the beginning so we can mimic this interaction and get the
packets for ourselves. From the view of Wireshark:

"Server Hello" and "Client Hello" - these include session details and SSL certificate information.

Next, we have "Client Key Exchange" packet which determines the public key to use and will encrypt further message with
this information.  Finally, the key is confirmed and the secure tunnel is created. 

### Pactical HTTPS Packet Analysis

LET'S GO! In order to get start we need the decryption key set and the data. First, load the PCAP data into WireShark.
If you look at the data, you can see that the data is all encrypted. If we look at packet 11, we can see that the data
is encrypted. We can load the RSA key by navigating to `Edit > Preferences > Protocols > TLS > [+]. Older wireshark will
have SSL instead of TLS. Will need to use this data to get it working:

```
IP Address: 127.0.0.1
Port: start_tls
Protocol: http
Keyfile: <RSA key location>
```

## Zerologon PCAP Overview

Example of a capture from a Windows Domain controller with a private IP of `192.168.100.6` and an attacker with an IP of
`192.168.100.128`. Let's go through the data and see what it looks like. 
