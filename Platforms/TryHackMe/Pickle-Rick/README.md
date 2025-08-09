# Pickle Rick TryHackMe Challenge
## Difficulty: Easy<br>Link to: [THM Room](https://tryhackme.com/room/picklerick)
*This was my very first ever challenge room completed in my life<br>I am very happy about it! :D*<br><br>
![Rick And Morty Leaving a Portal - Image](/Platforms/TryHackMe/Pickle-Rick/Imgs/47d2d3ade1795f81a155d0aca6e4da96.jpeg)<br>


## First Step: Reconnaissance
As a first step, I have used nmap to search for open ports and check if there was any vulnerable port<br>
The only open ports were:
```
$ nmap -sV 10.201.119.83

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH
80/tcp open  http    Apache httpd

```
(I took out uneed information from the output)

This information was not really helpful, since connecting to ssh was not permitted (it needed a public key).

After that I realized that the room description asks to search for vulnerabilities on the webpage, and so I began to search for it

## Exploring the website

I took a brief time to read the challenge a little bit more, and noticed the main page asks for Morty logon to his computer and find the 3 secret ingredients. The three main ingredients being the tree flags 

There were two things I have done almost immediately:
- [Inspected the website](#inspecting-the-main-website)
- [Tried to search for /login paths](#searching-for-login-paths)
- [Scanning the website] ()

### Inspecting the main website
By simply pressing F12, I noticed that Rick left a note for himself on its body
```
<!--Note to self, remember username! Username:R1ckRul3s-->
```
### Searching for /login paths
By seeing the username I thought to myself that a login form was most likely real, so I brute-forced "/login" and "/login.php", which worked. I was also going to try "/login.html" if it didn't work.

### Scanning the website
I have quickly scanned the website with burp suite in order to map it and quickly got the website mapped.
I noticed a path called "robots.txt", and so I opened it:
<u><b>10.201.119.83/robots.txt</b></u>

## Logging in
### Getting the credentials
When opening "robots.txt", there was only a word:<br>Wubbalubbadubdub<br>
I quickly tried to use it as a password along with R1ckRul3s as the username, and it worked! I was now at <u><b>10.201.119.83/portal.php</b></u>

### Main page
The main page was just a command execution pannel<br>
![Command Execution Pannel. The headers show the options "Commands", "Potions", "Creatures", "Potions" and "Beth Clone Notes"](/Platforms/TryHackMe/Pickle-Rick/Imgs/cmdpannel.png)<br>
Clicking on other items such as "Potions" or "Creatures" always redirects the user to denied.php.

## Getting the flags
### Locating myself
The first thing I did was execute the 'ls' command, which gave me the output:
```
.
Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php
robots.txt
```
<br>
The first thing I did was trying to use 'cat Sup3rS3cretPickl3Ingred.txt', but it did not work, as 'Rick' wanted to add more challenge.

![Message: Command disabled to make it hard for future Pickle Rick.](/Platforms/TryHackMe/Pickle-Rick/Imgs/carfail.png)

### Locating other files
After exploring the file system a little bit more, I managed to also find the Second Ingredient on Rick's homepage.
```
$ls /home/rick

second ingredients
```
But trying to open this document with the command 'cat' led to the same error.

After exploring even further, I managed to find the third flag on /root. However, since this was the root home directory, I had to use sudo.
```
sudo ls /root

3rd.txt
```

## Reading flag's content
I do not know if it was intentional, but I managed to find that we can simply open the flags with 'less' command.
```
less Sup3rS3cretPickl3Ingred.txt && less /home/rick/second\ ingredients && sudo less /root/3rd.txt

mr. meeseek hair
1 jerry tear
3rd ingredients: fleeb juice
```
---
It was really fun doing this room, and I am happy I could do it alone!

I am looking forward to keep going and doing more challenges!