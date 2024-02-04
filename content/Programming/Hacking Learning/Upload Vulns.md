---
tags:
  - programming/hacking/general
  - programming/hacking/networking
---
# Upload Vulns

Upload files to mess up a website! Woo hoo. Note - the attack vectors in ./img/ where created with https://app.mindmup.com - a pretty cool tool.

Here is my first attempt at someonething resembling an attack vector graph. This is more like a mindmap as it isn't so
much diferent attack methods as it is just a mapping of things I need to remember. Maybe it's the same thing. I don't
know - I'm an engineer.

![Attack Vector](upload_vectors.jpg)

# Remote Code Execution (RCE)

Upload a file that get a connection back to you

Find where we can upload files using gobuster
```
    gobuster dir  -u http://shell.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

This should give you the direcotry '/resources`. Now, let's get the php file that will give us a webshell. use this
address to get it: https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php

The process goes something like this - download the php file, modify to  point ot your current IP address. Start Netcat
`nc -lvnp 1234`. This will listen to port 1234 for incoming call. Finally,
navigate to the file after you upload and this should run the file and you should get a connection back.

# Filtering

This is all well and good, but what happens when people are trying to prevent you from attacking them. Well, we've got a
solution for that as well. A lot of websites will have "clinet-side" filtering. That is, the filtering of the file will
be done on YOUR machine instead of on the host (webserver) machine. This is great - they can test the file out before it
is even uploaded to the server. Except.... if it is tested on our side, we can bypass this. So, let's look at what kind
of filtering is done. 


#### Extension Validation

This checks that extension is of the right type. Unless you live in Windows land, you will know that this doesn't mean
much. This filter either blacklists extensions or whitelists them (disallow list or allowed list)

#### File type Filtering

Similar to the above except it actually looks at the file contents Two types that we will look at: 

- MIME validation (Multipurpose Internet Mail Extension) identifies files mainly when transferred over email, but is
  also used for HTTP(S). This is attached in the header of the reuest to the 

- Magic Number Validation: String of bytes at the beginning of a file that identifies the contents. Unix uses these to
  identify files instead of the extension. This is a bit more challenging to mess with that file extension

#### File Length Filtering

Prevent huge files from being uploaded. For shells, doesn't really bother us as the scripts are tiny. 

#### File Name Filtering

Filtering to keep out bad file names. For example `/`, `;` would be a bad in a file name. If the file is parsed after
upload, it may take some hunting to find the file.

File filtering almost always uses multiple methods.

## ByPassing Client Side Filtering

Let's do this

 1. Turn off javascript in your browser. As long as the site doesn't require javascript to function, the site will
    bypass filtering. If it requires JS to work, then try something else.
 2. Intercept and modify the page. this can be done with Burpsuite. Intercept the page, strip out the javascript filter
    and then continue
 3. Intercept and modify the file upload. Change the file after it has been filterd, but before it gets sent to the
    server. 
 4. Send the file directly to upload point. Don't even mess with the webpage, just upload directly via `curl`. The
    format for this would loks like this: `curl -X POST -F "submit:<value>" -F "<file-parameter>:@<path-to-file>"
    <site>`

If we look at the webpage itself we might be able to see something like 

```
if (file.type != "image/jpeg")){
    // some kind of error message
}
```

In this way we can figure out what the accepted upload method is. Here, that would be a whitelist for "jpeg" images.

How to solve
Generally - enumerate, find the upload location, figure out what kind of filtering they are doing, 

Get directories
```
gobuster dir -u http://java.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
# Return
# /images
# /assets
```

Check for client side filter in the Javascript. In case case view_page_source > client-side-filter.js we find:
```
if (file.type != "image/png"){
			upload.value = "";
			uploadMsg.style = "display:none;";
			error();
		} else{
			uploadMsg.innerHTML = "Chosen File: " + upload.value.split(/(\\|\/)/g).pop();
			responseMsg.style="display:none;";
			errorMsg.style="display:none;";
			success();
		}
```
 This show us that it will only accept png files. Neato. Try to upload some files - only PNG works. Next step is to fire
 up burpsuite and intercept some messages. Reload the page, right click and select `Do intercept > Response to this
 request`. Now, when we resond to the request we can ALSO intercept that - which is what we want. From here we can modfy
 the payload and change it as needed.

We actually have to think here. The message is slightly different than the one in the tutorial. But, in the response
just comment out the scripts that checks data and call it a day. Now, no more checking. In case you forgot, just remove
this bit from the response: 

```
<script src="assets/js/client-side-filter.js"> </script>
```

Now, upload the php file and navigate to it. In this case it is store in `./images`. Don't forget to fire up netcat
before running the exploit: `nc -lvnp 1234`

And.... that didn't work. Reading again - not that we may need to edit the external JS file and not the main page
itself. To do that, we need to modify the filtering being done. Find this under `proxy > options` and remove the `^js$|`
from the file extension filter under "Intercept Client Requests". Let's see if that helps. 

Okay, need to step through the request / response and watch what files are being requested after the inital request. It
appears that a lot of requests are made and then a lot of responses come back. Watch out for `client-side-filter.js` in
both request (capture the response) and then watch for the response as well. After you have this file, modify it to
accept anything.  IE: change this 

```
if (file.type != "image/png"){
        upload.value = "";
        uploadMsg.style = "display:none;";
        error();
    } else{
        uploadMsg.innerHTML = "Chosen File: " + upload.value.split(/(\\|\/)/g).pop();
        responseMsg.style="display:none;";
        errorMsg.style="display:none;";
        success();
    }
```

to this:

```
uploadMsg.innerHTML = "Chosen File: " + upload.value.split(/(\\|\/)/g).pop();
responseMsg.style="display:none;";
errorMsg.style="display:none;";
success();
```

Okay - looks like the first time would have worked. NetCat command was wrong and causing the issue. Make sure the netcat
command is running correctly: `nc -lvnp 1234`

Could have also returned the expected MIME type instead of skipping the filtering all together. Moving on! 


## ByPassing Server-Side Filtering: File Extensions

Client side is easy enough, let's see what we can with server-side filtering. Pretty much we just test it a bunch to see
what is and is not allowed and then make a payload that will be allowed through. 

Let's say we have a blacklist server-side filter. We wouldn't be able to see the code, but we can make a guyess as to
what it could look like: 


```
    //Get the extension
    $extension = pathinfo($_FILES["fileToUpload"]["name"])["extension"];
    //Check the extension against the blacklist -- .php and .phtml
    switch($extension){
        case "php":
        case "phtml":
        case NULL:
            $uploadFail = True;
            break;
        default:
            $uploadFail = False;
    }
    
```

This parses the extension and if it is "php" or "phtml" it will fail the upload. Okay, cool. In the real world, we would
not be able to see. Thus, the guess and check. BUT..... we can run a bunch of php code with different extensions: 
 - php3
 - php4
 - php5
 - php7
 - phps
 - php-s
 - pht
 - phar

 So..... let's upload those instead. In fact, this is the default behavior for Apache2 servers, which is a pretty big
 vulnerability to rely on people know this kind of detail.

 How would we do this with a "blackbox?" - IE: a server that we can't see the code on. Let's try it out.

 First we want to try out a legitimate upload - maybe a jpg file? Then let's try a php file. Unless something is odd,
 that will almost definitely get rejected. Next we want to try any of the extensions above. Remember, we REALLY want to
 get the reverse shell working. As an alternative, let's try uploading a file with `shell.jpg.php`. Perhaps the code is
 something like (in python):

 ```
 if ".jpg" in filename:
    return True
else:
    return False
 ```

 In which case, the above file name would work as it does contain ".jpg". It also contains more, but that's not what the
 filter is looking for. From here, we just navigate, check it and run it. Woo hoo! 


 Alright, time to bust open a server!

Running GoBuster to find directories where files are stored
 ```
    gobuster dir  -u http://annex.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
    # Returns:
    # /privacy
    # /assets
 ```

BONUS - `filter.php` exists in the `/assets/php/` directory, but we can't see the contents. Darn.
png file works. The uploaded images and put in the `./privacy/` directory

Let's make a list shall we?
 - png: yes
 - php: no
 - php: no
 - php3: no
 - php4: no
 - php5: yes!

Netcat baby! `nc -lvnp 1234`


## ByPassing Server-Side Filtering: Magic Numbers

do dooo da do do dah dooo dooo, do dah do do, doooo dah dooooo

It's.... It's Harry Potter theme song. 

If we upload a php file, it doesn't work. If we upload a jpeg file, it works. So, let's try to add jpeg headers to the
php file. We can find magic number signatures here: https://en.wikipedia.org/wiki/List_of_file_signatures and we can see
the magic numbers for jpeg are `FF D8 FF DB`. That's one of them anyways. It's possible to add the ASCII representation
to the top of the file, but that's messy. Time to go to hex land. Like Harry Potter. Hexes.... whatever.

Time to use the `file` command to check the type. `file shell.php`. This will return `PHP script, ASCII text`. Which is
true. So, we know that the magic number is four bytes long - let's add some random characters to the first line.In this
case, I'll add "AAAA". Now we'll use `hexeditor` and edit the hex file. If we open the file with hexeditor we can see
the first 4 characters are "41 41 41 41". Instead of working with weird ASCII representations we can just add "FF D8 FF
DB" to the top of the file.

Now if we open this in ASCII (like with VIM) it will show `ÿØÿÛ` at the top. GIBBERISH! That's why it's magic. Now, run
the command `file shell.php` and it returns as a jpeg file. Alright, let's DO THIS!

First off, let's find the directry structure: 
```
    gobuster dir  -u http://magic.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
    # Returns:
    # /graphics
    # /assets
```

In this case, we can see that only gif files are allowed. Assuming that we need to edit the magic numbers (because
that's what we are learning, we can see that the magic numbers are `47 49 46 38 37 61`. In order to get this, we are
going to add 6 characters to the top of the file. Next, open the file with hexeditor and change to the above. In this
case "GIF87a" would have sufficed, but let's keep with hex editing since it will work anywhere.

File uploaded okay, but now we can't get it - indexing has been disabled so getting to it via the file name is not
possible.

# General Methodology

Now that we know about filters and how they work - how to get around them? Well, let's go through the ways:

1. Checkout the website. Figure out what the backend looks like - is it Python, php, etc. Wappalyzer can do this, but is
   not always accurate. So, let's go through and check it out with burpsuite. For example - `server` or `x-powered-by`
   can show what it is using. While we are there we will also be looking for an upload page.

2. Check out the client-side code for the upload page and see if there is any filtering going on. That is a good start
   as we can directly change that code.
3. Try a pretty simple file upload. If you think an image will be allowed - upload an image and see if it works. Also
   check out where the file is saved. This is a pretty good use case for gobuster. Also, we get to check out the virtual
   landscape. We can use the -X switch to specify file extensions that we are looking for. `-x php, txt, html` will
   search for .php, .txt and .html files that match the wordlist. Super useful if you are uploading a file and the
   server changed the filename.
4. Now we know how to upload the file and where it got placed. Use client side editing to get around the client filter
   and then try different file types to get by the filter.

Alright, cool, cool, but how do we get around the server filtering? Well, we've been over this:

 - upload file with totally random file extension: `testingimage.asdfasdwerwe`. If this works, it is likely operating on
   a blacklist. If it fails, it is likely the server is using a whitelist (IE - the extension was not in the "allowed"
   list.
- Upload the legitmate file, but change the magic number to be something you would expect to be filtered.
- Upload the legit file, but change the request MIME to be something malicious
- Checking file length filers takes a little more. Create a small file and keep increasing file size until until fails.
  OR, just throw a big file up and it might tell what the file size limit is.



# FINAL BOSS

Now on to the last challenge. FIrst off, checking what WILL work. JPEG images will work. That was easy. Next, checking the directory .

```
gobuster => See above. 
# Returns
# /graphics
# /assets
```

Going to open up burpsuite and check if there is client side filtering going on. Capturing the response, we can see: 
` <input id="fileSelect" type="file" name="fileToUpload" accept="image/jpeg">`

Which I think that means it is using MIME filtering for image/jpeg. Great, let's capture the send request and see if we
can modify this. WHOAH - that was much easier than I thought it would be. Let's change image/jpeg to something else and
see if we get rejected.

```
"name":"jpegsystems-home.jpg"
"type":"image/jpeg",
"file":"data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEASABIAAD/.....
```

Changing `"type":"image/jpg"` to `"type":"image/gif"` results in a rejected file. Woo hoo! Let's see if we can find one
of the files that we uploaded earlier. Nope `./graphics` and `./assests` don't work. Perhaps the image was not changed
when we uploaded it, so let's try getting it directly. Adding `image.jpg` to the above didn't work either. THis might
mean that the server is changing the name 

Attempt to directly upload the php file - this didn't work. It was rejected before the request was even made. You know
what that means??? Client side filtering. Checking the source code again, I found a file `input.js` and then dug into
that file and found:

```
    //Check File Size
    if (event.target.result.length > 50 * 8 * 1024){
        setResponseMsg("File too big", "red");			
        return;
    }
    //Check Magic Number
    if (atob(event.target.result.split(",")[1]).slice(0,3) != "ÿØÿ"){
        setResponseMsg("Invalid file format", "red");
        return;	
    }
    //Check File Extension
    const extension = fileBox.name.split(".")[1].toLowerCase();
    if (extension != "jpg" && extension != "jpeg"){
        setResponseMsg("Invalid file format", "red");
        return;
    }
```

Let's go ahead and just get rid of that entirely. Burpsuite > capture the response > remove the javascrpt

Oh! - `X-Powered-By: Express` We'll keep that for later

Fun issue - the 304 error. This means that the request has not changed, so the server doesn't actually send anything
back. Change this: Proxy > Options > Match and Replace. Select "Require non-cached response".

Try one - remove the "upload.js" and that just broke the site. 

Try two - captured the upload.js file and modified it so stop checking the file. Then, selected the php file, uploaded
it and changed the MIME data. SUCCESS! Now, got to find it. 

Oh yeah, also need to check the backend to make sure it is actually php.

Well, according to `expressjs.com` express is a JavaScript based framework. Whoops, need to get a javascript reverse
shell. I went to this nice little website and found a reverse shell `https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md`
along with shells for just about everything else. Copied the code to file `shell.js` and tested it locally - WORKED!


Next step, upload that bad boy to the server. Alright, got it uploaded, now we just need to find it. 

*NOTE* - this is wrong - see further below for actual directories. I must have scanned the wrong machine.
Found dirs:
 - /graphics
 - /assets

Testing: 
 - /graphics/shell.js - 404
 - assets/shell.js - 404

And.... I got bored. Let gobuster this bad boy. Using the `-x` flag to attempt to get just the javascript file that I
am looking for.

```
# Assets dir first
gobuster dir  -u http://jewel.uploadvulns.thm/assets/ -w ~/Downloads/UploadVulnsWordlist.txt -x .js

# Graphics dir next
gobuster dir  -u http://jewel.uploadvulns.thm/graphics/ -w ~/Downloads/UploadVulnsWordlist.txt -x .js

# Graphics dir next
gobuster dir  -u http://jewel.uploadvulns.thm/graphics/ -w ~/Downloads/UploadVulnsWordlist.txt -x js, jpg, jpeg
```

Okay, after struggling with with for quite a while, I checked the hints and I had apparently not enumerated the
directory structure correctly. Running gobuster again I got:

- /content => not found
- /modules => not found
- /admin => weird page that actives modules from ./modules
- /assets => not found
- /Content => not found 
- /Assets => not found
- /Modules => not found
- /Admin => not found

Okay, lots of directories that are not indexed. Let's see if we can find the uploaded file using goubster under the
`content` directory using the UploadVulnsWordlist wordlist and only checking for jpg,js,jpg files.

- /ABH.jpg => image

~70,000 is far too many. Just searching for `js` reduced that to ~35,0000 which only a bit better. This ended up not
returning anything, which is not good. It makes me wonder if the file was actually rejected by the server but said it
was accepted? Or, perhaps it was assigned an extension. Scanning with just "jpg" now

Solution - upload the shell file with jpg extension - either change the magic numbers or remove client side filtering.
Find file by using gobuster in the contents dir since it will have been assigned a random three character name Then 
nagivation to /admin and runthe file using `../content/ASD.jpg` you should get a reverse shell. 

I spend 4 hours on this. Then it quit loading and I cheated and looked up the flag so I could move on with my life.
