![Ignite/img/Header.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/Ignite/img/Header.png)
---
https://tryhackme.com/room/ignite
---
## Questions:
1. User.txt
2. Root.txt

---
### Question 1: User.txt
---

We are back at it with Ignite. To start, we need to enumerate ports. Nmap FTW!

*Nmap Image*

Only port 80 open. Nmap does give some information about what is running on port 80, Fuel CMS.

*Fuel image*

Looking through Exploit-DB, I found the following CVE. Let's give it a shot and see what happens.

*CVE image*

Running the python script gives us a prompt. To test it I ran simple commands like "ls" and noticed it was returning information. I wonder if we can run a reverse shell?

I set up a listener and got a simple revshell script from revshells.com

*revshell image*
*initial image*

Just like that we have the initial access as www-data. The user flag is in www-data's folder so we can cat that out really quick then move onto trying to obtain root.

*userflag image*

---
### Question 2: Root.txt
---

To find the root flag, I ran Linpeas to see if it could find anything. Looking through the output I noticed if found a password a file named database.php. 

*possible image*

Looking at that file, it has what looks to be the root password hard coded into it. Let's test shall we?

*root image*

Success! Now to find the root flag.

*rootflag image*

There it is!
