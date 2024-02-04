---
tags:
  - programming/hacking/general
---

# OWASP top 10

Open Web Application Security Project top ten issues found in computers. Let's go!

## 1 - Injections

This is the most common issue. That is, user input is taken as commands and run. Types include SQL injection whee the
attacker can use the database and command injections where the attacker is able to put commands to the target computer.

These two vectors could allows the attacker to modified, insert or delete information from the database or execute
arbitrary commands and escalate privileges. 

These can be defended by using a list of allowed characters or by removing any dangerous characters

OS Command Injections occurs when the client side machine makes a call to the hosting machine. 

## 1 - Practical Command Injection

Blind injection is when the command does not return data. Active command injection is when the attack returns data
back to the attacker

## 2 - Broken Authentication

Issues with authentication include weak passwords, susceptibility to brute force attacks and weak session tokens. To 
counter these, the website should enforce strong password use, lockout a user after a given number of login attempts
and use session tokens that can't be easily guessed and finally using multifactor authentication

One way we can take advantage of broken authentication is to register a user with the same name as an actual user but
with a slight change - " anthony" instead of "anthony". In a broken system this will end up showing the information for
"anthony" when logged in.

## 3 - Sensitive Data Exposure

Sometimes webpages just throw a flat file database in the website. Downnload and get info - check `./01_hasing_notes.md`


## 4 - XML External Entity

Basically abuses the parsing of XML and can allow some interesting things to happen. 

These can be broken down in to in band and out of bound (OOB-XXE). In-band attacks receives immediate response. Out of
bound (blind) XXE means that there is no immediate response

### But what is XML

XML stand for eXensible Markup Language which is a language that is used for both machine readable and human readable
code. Sounds like it might be similar to markdown, which is what this document is written in


Some key point of XML is it is platform independent, data stored can be altered without changing presentation,
validation using DTD and Schema (?) so there are no syntax errors, because platform independent, can share between
systems without and issue. 

#### Syntax

The syntax of HTML goes something like:

```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <mail>
       <to>falcon</to>
       <from>feast</from>
       <subject>About XXE</subject>
       <text>Teach about XXE</text>
    </mail>
```

Basically just HTML but different

#### DTD

The DTD is the syntax validator that defines the structure and allowed elements. DTD stands for Document Type
Definition. Because, you know, it defines the document type. Example of a DTD that could be used: 

```dtd
    <!DOCTYPE note [ <!ELEMENT note (to,from,heading,body)>
    <!ELEMENT to (#PCDATA)> <!ELEMENT from (#PCDATA)> <!ELEMENT heading (#PCDATA)> <!ELEMENT body (#PCDATA)> ]> ```
```

### Example

Let's actually do some testing now

This will sometimes work to file a file:

```xml
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
<root>&read;</root>
```

If instead of reading `/etc/passwd` we can go ahead and read `/home/falcon/.ssh/id_rsa` which will give us the ssh key
for the user "falcon" which we got from the passwd file

## Broken Access Control

This one is pretty simple - if you can view websites that you're not supposed to view then the access control is broken.

IDOR - Insecure Direct Object Reference is a way to exploit the way the user input is handled. IE - a type of access
control vulnerability

## Security Misconfiguration


Default username and password falls under this category. Yep. It happens. A LOT. Also http headers that reveal
information, overly detailed error messages, services that aren't needed and just misconfigured resources (public s3
buckets)

##  XSS

Cross Site Scripting is a vulnerability that allows an attacker to run malicious scripts on a target's machine. The
three types of XSS are Stored, Reflected and DOM-Based. Stored attacks are the most dangerous - these are ones where the
malicious string originates from the website's database. This can can happen when user input is not sanitized. The next
type of attack is a Reflected XSS. This is where the target actually is the one that sends the script to the websites to
run. In this way the script is "reflected" off the website and back to the victim. This involved the user clicking on a
malicious link that sends the malicious code to the website and back. Finally there is DOM-Based XSS which is .....
another kind. Based on how the website runs.


Payloads might in include popups <script>alert(“Hello World”)</script>),  writing HTML where the target is shown
whatever HTML code you want them to see, XSS keylogger where all the user input is recorded, and port scanners that act
as a mini local port scanner. 

Hey here's a fun website: XSS-Payloads.com

OH.... Basically you are just putting HTML in the website some how and it is executing. For example, you might put
`<h1>Hello!</h1> and it would render in a chat box. Or, you could put `<script>alert(“Hello World”)</script>)` and it
would make a popup for everyone in the chat
