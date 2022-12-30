  
  #  Overthewire Bandit Write-up: Sai Rithvik Nama  


## Level 0   
Had to use ```ssh``` command. Initially tried ```ssh -D[bandit.labs.overthewire.org:]2220``` but that failed.   
After looking at examples online, used the right command: ```ssh bandit0@bandit.labs.overthewire.org -p 2220``` and cleared the level.  
  
## Level 0-1    
Used ```ls``` to list all files in directory, followed by cat readme to get the password.  

## Level 1-2  
Tried to redo what I did in previous level, but it failed.   
Googling dashed filenames showed me I had to use ```./``` or ```<``` with the filename to get the filename to be read and not treated as input.  
Used ```cat < -``` to display the password then proceeded to the next level.  

## Level 2-3    
  After googling, used ```cat "spaces in the filename"```  
  
## Level 3-4
Step 1: 
```cd inhere```
Next:
```ls``` didn't work,  but ```ls -a``` followed by   ```cat .hidden``` worked.  
## Level 4-5  
```
cd inhere
ls -a
man find
man du
man file
```

```
find -readable
```

showed multiple files Tried

```
file
```

next learnt from googling that

```
*
```
is a wildcard which can be used with file to check all filenames It gave an error 

[This superuser post](https://superuser.com/questions/163515/bash-how-to-pass-command-line-arguments-containing-special-characters "https://superuser.com/questions/163515/bash-how-to-pass-command-line-arguments-containing-special-characters")  made me try ```\``` but that did not work.  
Further googling led to using

``` ./*```  
```
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data'
```  
```
Password: lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR
```

## Level 5-6  
  After reading the man page for file and following previous level I tried ```find -type c -size 1033c -!executable``` which did not work.  
  After a little bit of trial and error and going through the man pages I found the ```-readable``` argument and used ```! -executable``` in conjunction to get the file.  
  Then I used ```cat``` to find the password and move to the next level.  
  
```Password: P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU```
 
## Level 6-7  
```  
ls -a 
ls .   
ls ..  
```    
```
bandit0   bandit11  bandit14  bandit17  bandit2   bandit22  bandit25  bandit27-git  bandit29      bandit30      bandit31-git  bandit4  bandit7  krypton1  krypton4  krypton7
bandit1   bandit12  bandit15  bandit18  bandit20  bandit23  bandit26  bandit28      bandit29-git  bandit30-git  bandit32      bandit5  bandit8  krypton2  krypton5  ubuntu
bandit10  bandit13  bandit16  bandit19  bandit21  bandit24  bandit27  bandit28-git  bandit3       bandit31      bandit33      bandit6  bandit9  krypton3  krypton6
```
  Went through man pages for ```find``` again and found:  
   ```-user uname: File is owned by user uname (numeric user ID allowed).``` and ```-group gname: File belongs to group gname (numeric group ID allowed).```.      
  
    
   Used ```find -size 33c -user bandit7 -group bandit6``` and got the following:
  
```find: ‘./bandit28-git’: Permission denied```  
```find: ‘./bandit29-git’: Permission denied```  
```find: ‘./bandit27-git’: Permission denied```  
```find: ‘./bandit31-git’: Permission denied```  
```find: ‘./bandit30-git’: Permission denied```  
```find: ‘./bandit5/inhere’: Permission denied```  
```find: ‘./ubuntu’: Permission denied```  

Following this I used ``cd ..`` and tried ```-type f```.
A larger series of error messages were printed. In between those there was:  
```./var/lib/dpkg/info/bandit7.password```
Using ```cat``` with this path gave the password:  
```
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S
```

## Level 7-8      
Simply used ```grep millionth data.txt``` after reading man page to get password. 
```NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL```

## Level 8-9
Went through the man page for
```
grep
 ``` 
Read about
```sort```  
and  
```uniq```

[This post](https://askubuntu.com/questions/1130731/how-to-combine-4-commands-into-1-in-terminal-while-they-are-connected-logically "https://askubuntu.com/questions/1130731/how-to-combine-4-commands-into-1-in-terminal-while-they-are-connected-logically") taught me about the piping character ```|``` which helps use multiple commands in conjunction.   
Tried
```
sort data.txt
```
and
```
uniq data.txt
```

Still returned a bunch of lines

```
sort |uniq data.txt
```
also gave a bunch of lines.   
Looked through
```uniq``` examples and found
```uniq -u```
```sort |uniq -u data.txt```didn't work but
```sort data.txt |uniq -u``` worked
```
EN632PlfYiZbn3PhVK3XOGSlNInNE00t
```  
## Level 9-10   
```
grep -F = data.txt
```
gave:
```grep: data.txt: binary file matches``` 
 Read up on the
```strings```
command
Usings ```strings``` with data.txt showed this:
```G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s```![Image](https://media.discordapp.net/attachments/1058181455706062900/1058234413537820742/image.png?width=1140&height=611)
  
## Level 10-11 
Straightforward.  
Used ```base64 -d ``` to decode the text file.
![Image](https://media.discordapp.net/attachments/1058181455706062900/1058235110777958483/image.png)  
## Level 11-12  
Looked up the 			tr			 command:

> The tr command is a UNIX command-line utility for translating or deleting characters. It supports a range of transformations including uppercase to lowercase, squeezing repeating characters, deleting specific characters, and basic find and replace. It can be used with UNIX pipes to support more complex translation. tr stands for translate.  
  
 After going through stackexchange posts and tr guides, I used the following command :  
 ```cat data.txt | tr '[a-z]' '[n-za-m]' | tr '[A-Z]' '[N-ZA-M]'```    
 ![Image](https://media.discordapp.net/attachments/1058181455706062900/1058239113314185296/image.png)  
 ```JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv```  
## Level 12-13  
![Image](https://media.discordapp.net/attachments/1058181455706062900/1058253989772283984/image.png)    
Next, I used the  ```xxd``` command in reverse:

> xxd creates a hex dump of a given file or standard input. It can also convert a hex dump back to its original binary form    
  
 After repeatedly renaming the file using the mv command (mv [file] [destination file]) to a .gz or a .bz2 or a .tar and repeatedly decompressing using 
```tar -xf``` or ```gzip -d``` or ```bzip2 -d```  
I finally got ```data8.bin``` as an ASCII text file which contained the password:``` wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw```
  
  ## Level 13-14  
  After googling, I followed [This stackexchange post](https://unix.stackexchange.com/questions/23291/how-to-ssh-to-remote-server-using-a-private-key)
  I used ```ssh -i```
  So final command:   
  ```ssh bandit14@bandit.labs.overthewire.org -p 2220 -i sshkey.private```

 Then using ```cat /etc/bandit_pass/bandit14 ``` gave me :
 ```fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq```
    
## Level 14-15  
I had to connect to the given port  
After reading about ```telnet``` and ```netcat```,
```netcat``` seemed to be the way to go:
```nc localhost 30000```
Entered the password:
bandit14@bandit:~$ nc localhost 30000 fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq Correct! jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt

Password for next level : jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
  
## Level 15-16  
![Image](https://media.discordapp.net/attachments/1058181455706062900/1058427589280149554/image.png?width=1137&height=611)  
Description talks about using SSL, so I read about openSSL from [FCC](https://www.freecodecamp.org/news/openssl-command-cheatsheet-b441be1e8c4a/#4d47)   
  
```
bandit15@bandit:~$ openssl s_client localhost -port 30001
s_client: Use -help for summary. 
bandit15@bandit:~$ openssl s_client -connect localhost -port 30001  
CONNECTED(00000003)
```
I then entered the password and got the next password:
```JQttfApK4SeyHwDlI9SXGR50qclOAil1``  
 

## Level 16-17  
 After reading about port scanning I followed [this guide](https://linuxhint.com/port_scan_linux/ "https://linuxhint.com/port_scan_linux/") to figure out how to do it.  
 then from man nmap

```
bandit16@bandit:~$ nmap -p31000-32000 
Nmap scan report for bandit.labs.overthewire.org (127.0.0.1)
Host is up (0.0012s latency). 
rDNS record for 127.0.0.1: localhost 
Not shown: 996 closed ports 
PORT STATE SERVICE 
31046/tcp open unknown 
31518/tcp open unknown 
31691/tcp open unknown 
31790/tcp open unknown 
31960/tcp open unknown 
Nmap done: 1 IP address (1 host up) scanned in 0.08 seconds
```
  
 This did not show info about SSL so I tried again after going through nmap guides:  
 ```
 bandit16@bandit:~$ nmap -sV -p31000-32000  bandit.labs.overthewire.org  
 ```  
 ```
 Nmap scan report for localhost (127.0.0.1) 
 Host is up (0.00010s latency). 
 Not shown: 996 closed ports
  PORT STATE SERVICE VERSION 
  31046/tcp open echo 
  31518/tcp open ssl/echo 
  31691/tcp open echo 
  31790/tcp open ssl/unknown 
  31960/tcp open echo```  

Tried 31518, which led to me realising the ```echo``` next to it means it just functions as the ```echo``` command.
Tried 31790, and got a private RSA key:  
read R BLOCK   
JQttfApK4SeyHwDlI9SXGR50qclOAil1   
Correct!
  ```

>  -----BEGIN RSA PRIVATE KEY-----   
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama +TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT 8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM 77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3 vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----    

Next, I tried to create a file to save the private key like in one of the previous levels. I was denied permission while trying to create the file with touch till I tried in tmp.  
```
cd ../../tmp
then touch sshkey.private1
vim sshkey.private1  
```  
Pasted the RSA key.
While trying to use the private key with ssh command-like in level 14-I got a message stating the private key will be ignored.  
[Following this guide](https://ubccr.freshdesk.com/support/solutions/articles/13000072165-changing-permission-on-your-ssh-private-key) I made the key inaccessible to others using ```chmod 600``` then I was able to login to bandit17.  

## Level 17-18
Directly using the ```diff``` command given I got:  
```  
bandit17@bandit:~$ diff passwords.new passwords.old   
42c42   
< hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg ---   
>U79zsNCl1urwJ5rU6pg7ZSCi7ifWOWpT
```

Trying both passwords, I found that the right password is: ```hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg```

## Level 18-19  
[Going off this superuser post](https://superuser.com/questions/1277825/ssh-automatically-logging-out "https://superuser.com/questions/1277825/ssh-automatically-logging-out") I tried ```ssh -T```  which didn't seem to work.  

So I tried something different:  
I used ``cd`` to switch to bandit18
```
cd ../bandit18 
ls readme 
cat readme
```
This did not work.  
After further reading I discovered we can run a command along with ssh simultaneously   
```ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme```   
Entering the password gave me the next pass: ```awhqfNnAbc1naukrpqDYcF95h7HoMTrC ``` 

## Level 19-20  
``` 
bandit19@bandit:~$ls   
bandit20-do   
bandit19@bandit:~$  ./bandit20-do   
Run a command as another user. Example: ./bandit20-do id   
bandit19@bandit:~$ . /bandit20-do id  
uid=11019(bandit19) gid=11019(bandit19) euid=11020(bandit20)  groups=11019(bandit19)   
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20   
VxCazJaVykI6W36BkBU0mJTCM8rR95XT 
```  

## Level 20-21  
Fun level.  
Had to open a new cygwin window   
I used ```nc -nlvp 2004```
 Then in other window ```./suconnect 2004```
 Then in the first window, I entered the previous password.  
 Output was the next password:
```NvEJF7oVjkddltPSrdKEFOllh9V1IBcq```    

## Level 21-22  
Tried ```ls /etc/cron.d/```.
Since it's level 21-22, decided to look into ```cronjob_bandit22```

``` 
bandit21@bandit:~$ ls /etc/cron.d/      
cronjob_bandit15_root cronjob_bandit17_root cronjob_bandit22 cronjob_bandit23 cronjob_bandit24 cronjob_bandit25_root e2scrub_all otw-tmp-dir sysstat   
bandit21@bandit:~$ cat cronjob_bandit22   
cat: cronjob_bandit22: No such file or directory   
bandit21@bandit:~$ cat /etc/cron.d/cronjob_bandit22   
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null  
 \**** bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null   
bandit21@bandit:~$
 ```

Googled '.sh' and learnt that ```/usr/bin/cronjob_bandit22.sh``` is a shell script
Tried ```cat``` with it
    
 ``` 
 bandit21@bandit:~$ cat /usr/bin/cronjob_bandit22.sh   
 #!/bin/bash   
 chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv   
 cat /etc/bandit_pass/bandit22> /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv   
 bandit21@bandit:~$
   ```
    
Since \> is for redirection (from one of the previous levels with piping and redirection) so I checked the tmp file
  ```bandit21@bandit:~$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff```
    
```password:WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff```  

## Level 22-23  

It seems similar to prev. level.
```bandit22@bandit:~$ cat /usr/bin/cronjob\_bandit23.sh   
#!/bin/bash   
myname=$(whoami)     
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)   
echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"   
 cat /etc/bandit_pass/\$myname > /tmp/$mytarget  
 ```

```whoami ```when run in the shell will return bandit23 here.
So it's going to go ```echo I am user bandit23 | md5sum |``` etc.  
Based off what I read on [here](https://www.geeksforgeeks.org/md5sum-linux-command/ "https://www.geeksforgeeks.org/md5sum-linux-command/")  I tried the following:
```  
bandit22@bandit:~$ md5sum | cut -d ' ' -f 1  
^Z   
[1]+ Stopped md5sum | cut -d ' ' -f 1  
bandit22@bandit:~$ md5sum bandit23 | cut -d ' ' -f 1   
md5sum: bandit23: No such file or directory  
bandit22@bandit:~$ echo I am user $bandit23 | md5sum | cut -d ' ' -f 1  
8ca319486bfbbc3663ea0fbe81326349
``` 
```8ca319486bfbbc3663ea0fbe81326349``` must be the filename in tmp  
Using that:
Password:```QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G```  

## Level 23-24  
Following same steps as earlier:  
```
bandit23@bandit:~$ ls /etc/cron.d/
cronjob_bandit15_root  cronjob_bandit17_root  cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  cronjob_bandit25_root  e2scrub_all  otw-tmp-dir  sysstat
bandit23@bandit:~$ cat /etc/cron.d/cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit23@bandit:~$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```  
Reading this script and based off the description on overthewire, it looked like I needed to create a script in ```/var/spool/bandit24/foo``  

Further, from the if condition it looks like only if the owner is bandit23 then the file will be executed. However it'll also get deleted (rm)  .  
I need to write a script which will keep password from getting deleted, so I need to redirect output to another spot.    
Since it's been mentioned earlier that all passwords will be in    
```/etc /bandit_pass/..```   
 I need to take that into consideration as well.  
 So, bandit24's password file needs to be redirected to a spot where bandit23 can read it And bandit24 script should be able to get into the folder created by bandit23.    
This involved creating a directory for the password then a file to store the password, then modifying that file's permissions to allow bandit24's password to be able to be stored in it.   
> (Unfortunately I didn't copy-paste those things from the terminal and didn't notice it until after I'd closed everything)
    
    #!bin/bash   
    cat /etc/bandit_pass/bandit24 > /tmp/kenobipass/kenobiPASS.txt

Using the ```date``` command, I kept checking till the seconds were at :59 or so

When it hit 00 I can ls to get the redirected password, which took me a few tries but then I got the password:

![Image](https://media.discordapp.net/attachments/1058181455706062900/1058315320969941032/image.png)  
Password: ```VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar```   


## Level 24-25  
## Level 25-26  
## Level 26-27  
## Level 27-28  
## Level 28-29  
## Level 29-30  
## Level 30-31  
## Level 31-32  
## Level 32-33  
## Level 33-34







