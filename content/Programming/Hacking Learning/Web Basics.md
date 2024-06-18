---
tags:
  - programming/website
  - programming/hacking/networking
---

# The basics of how the web works. 

I do this stuff every day - maybe I should know a little about what's going on under the skirt. 

1. Make DNS request
2. DNS converts your website names into an IP address. IPs - (0-255) X.X.X.X called an octet
3. Computer know the IP address, came make a `GET` request to get information. Webpage responds with content.

Most requests are now made with HTTPS which encrypts data in transit. This is good for you, bad for hackers. Only you
and the server can see the data. 

Webserver is just software that serves the web page. Apache, Nginx, MS ISS. HTTP is on port 80, HTTPS is on port 443

Webpage is usually a combination of HTML, CSS, and JavaScript

# HTTP Requests

POST request - send data to server (comment, login, etc)

Parts of the request

```
    GET /main.js HTTP/1.1
    Host: 192.168.170.129:8081
    Connection: keep-alive
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36
    Accept: */*
    Referer: http://192.168.170.129:8081/
    Accept-Encoding: gzip, deflate
    Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
```

Line 1 - type (verb) and path for server
Lines 2-7 Headers. Info about request and cookies
Lines 8-? Body of the request. Usually used for POST. GET can have them, but usually ignored

Headers can show some interesting information

# Responses

The server will come back with a response of some kind. The response will contain a status code. These ranges are:

```
    100-199: Information
    200-299: Successes (200 OK is the "normal" response for a GET)
    300-399: Redirects (the information you want is elsewhere)
    400-499: Client errors (You did something wrong, like asking for something that doesn't exist)
    500-599: Server errors (The server tried, but something went wrong on their side)
```

The header of the response will also contain interesting information. Should check that out as well. Example response: 

```
    HTTP/1.1 200 OK
    Accept-Ranges: bytes
    Content-Length: 28
    Content-Type: application/javascript; charset=utf-8
    Last-Modified: Wed, 12 Feb 2020 12:51:44 GMT
    date: Thu, 27 Feb 2020 21:47:30 GMT

    console.log("Hello, World!")
```

418 status code - I'm a teapot.

That's a new one...

# Cookies

Cookies are used to keep track of information since each HTTP request itself is mean to be stateless - doesn't keep
track of it for you. Cookies have a name, value, expiry date and path. The path determines what requests the cookie will
be used with. Normally only used with requests to the sites that made them. 


But advertisers and marketing (tracking) don't care. They do whatever they want.

Can steal Session Tokens to impersonate someone else - fancy

You can use developer tool to view cookies and modify them if you would like. In Firefox, F12 will bring up dev tools.
Check the storage table to see cookies that are set.

There is also LocalStorage and SessionStorage that some people use, but we don't associate with them


# Using CURL and checking out websites

Curl flags to know:
```
-X      Specify request type
--data  Specify data to sent
```

Unlike a web browser, CURL does not store cookies

That's it - that's basics of how the web works
