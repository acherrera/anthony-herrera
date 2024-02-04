---
tags:
  - programming/hacking/tools
  - programming/linux
---
# How to use burpsuite

Should already be install on Kali, just start, use temporary project and load defaults. 

## Configuration to use the web

Need to install FoxyProxy for Firefox and route traffic through 127.0.0.1:8080

- Install foxyProxy
- add 127.0.0.1:8080 as the proxy to use

Now need to go in and get a certification so burpsuite can run correctly

- go to 127.0.0.1:8080
- click "CA certificate" at top right
- go to "settings" in Firefox and search for "Certificates"
- "View Certificates" > "Authorities" > "import"
- find certificate

Now, when you make a request, burpsuite will capture it. Turn off interception to use the internet as god intended

## Running attack

Should have taken more notes. Basica idea is that you forward the traffic through burpsuite so you can see the requests
being made and make more requests if needed.


foxyproxy => 127.0.0.1:8080 => burpsuit => web

and it goes back the same way it came in. Don't foreget to switch to the correct tab when making a request or it will
look like it is just frozen
