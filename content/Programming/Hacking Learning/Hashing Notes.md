---
tags:
  - programming/hacking
---
# Finding a database file

Run `sqllite3 webapp.db` to open up the file and check it out. Run `.table` to show tables. Oh, look we found a users
table. Let's look into that using `PRAGMA table_info(Users);` and we get:

```
0|userID|TEXT|1||1
1|username|TEXT|1||0
2|password|TEXT|1||0
3|admin|INT|1||0
````

# Raw values

The raw values from the database are:
```
    4413096d9c933359b898b6202288a650|admin|6eea9b7ef19179a06954edd0f6c05ceb|1
    23023b67a32488588db1e28579ced7ec|Bob|ad0234829205b9033196ba818f7a872b|1
    4e8423b514eef575394ff78caed3254d|Alice|268b38ca7b84f44fa0a6cdc86e6301e0|0
```

Putting the two together we can see the format is `userID | username | passwrod | admin. Cleaning up the above we get a
nice username - password  of 

```
    admin       6eea9b7ef19179a06954edd0f6c05ceb
    Bob         ad0234829205b9033196ba818f7a872b
    Alice       268b38ca7b84f44fa0a6cdc86e6301e0
```


Now, we are feeling lazy, so let's just load those hashes into crackstation.net and we get 
```
    admin       6eea9b7ef19179a06954edd0f6c05ceb => qwertyuiop
    Bob         ad0234829205b9033196ba818f7a872b => test2
    Alice       268b38ca7b84f44fa0a6cdc86e6301e0 => *NOT FOUND*
```
