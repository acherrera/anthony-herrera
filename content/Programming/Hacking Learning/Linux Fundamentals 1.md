---
tags:
  - programming/hacking/general
  - programming/linux
---
# Linux Tutorial 1

Lots of basic stuff here. 

## Basic Commands:


    echo        ## Will output file text
    whoami      ## Will output username

## File system

    cd          ## Change directory
    ls          ## list files in direcotry
    cat         ## show contents of the file
    pwd         ## Print Working Directory

For example, change to directory `cd`, then list files `ls` and show the contents of a file `cat`

## Searching for files

Commands to search for files

    find ## search for files
    grep ## text search within files

Example - if we know that name of the file that we are looking for is `passwords.txt`

    find -name passwords.txt

Example - name of file ends with ".txt"


    find -name *.txt

Example - want to find a specific line in a file. Can't use cat here. We are searchfor the IP add ress "81.143.211.90".

    grep "81.143.211.90" access.log

Example - find the prefix "THM" in access.log

    grep "THM" access.log


## Shell operators

"&" sign will run a command in the background so we can go on and do other things.

"&&" has literally nothing to do with "&". "&&" allows us to make a list of commands to run. `command1 && command2`.
However, the second command will only run if the first one was successful.


">" is an output redirect. It will take the output of the command and send it somwhere else. An example would be taking
the output of a command and saving it to a file for later

    echo hey > welcome.txt # output "hey" to "welcome"

Note that Linux doesn't really care about filenames. "welcome" is just as valid as "welcome.txt"

">>" Like ">" except that this will append to the file instead of overwriting the values.

    echo hello > welcome # will add "hello" to "welcome" file
