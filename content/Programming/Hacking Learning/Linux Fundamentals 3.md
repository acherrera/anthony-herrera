---
tags:
  - programming/hacking/general
  - programming/linux
---

# Linux Fundamentals 3

## Text Editors

VIM forever, moving on

## Useful Utilities

Okay, this one is kind of interesting. Starting of with "wget" - download files from online

Moving on the SCP "Secure copy". Example below

    scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt

This might look complicated but it is just 
    
    scp <SOURCE> <DESTINATION>

In this case we have to specify the IP address for the destination as well. The above command assumes that you are
logged into the source computer. If that is not the case and you need to get the file a remote computer to your local,
just use the following

    scp ubuntu@192.168.1.30:/home/ubuntu/document.txt notes.txt

MAGIC!

### Python 3 http server

Used to create a mini server that will server the files in the current directory so that "curl" and "wget" will be able
to reach them. There is also a module called "Updog" but we'll get to that later 

    python3 -m http.server          # Start server
    wget http://127.0.0.1:8000/file # Download "file"

In the real world there are firewalls and what not, but this is a nice start

### Systemctl 

Example

    systemctl start apache2

Options
* start
* stop
* enable
* disable

### Cron Jobs

Used to schedule takss and jobs. "crontab" is started during boot and is responsible for managing cron jobs - jobs the
repeat periodically. Crontabs is just a file that cron recognizes and will execute. Parts of a crontab are

    MIN         Minute to execute
    HOUR        Hour to execute
    DOM         Day Of Month to execute
    MON         MONth of year to execte
    DOW         Day Of Week to execute
    CMD         What command to execute

If you want to see what the cron scheduling will look like, checkout `Crontab Generator` or `Cron Guru`. Can edit
crontab 
