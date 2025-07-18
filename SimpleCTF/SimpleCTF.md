![SimpleCTF/img/Header.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/Header.png)

---

https://tryhackme.com/room/easyctf

---

## Questions:
1. How many services are running under port 1000?
2. What is running on the higher port?
3. What's the CVE you're using against the application?
4. To what kind of vulnerability is the application vulnerable?
5. What's the password?
6. Where can you login with the details obtained?
7. What's the user flag?
8. Is there any other user in the home directory? What's its name?
9. What can you leverage to spawn a privileged shell?
10. What's the root flag?

---
### Question 1: How many services are running under port 1000?
---

Let's start off running an Nmap scan to see what ports are open (HINT: Specify which ports to scan, not just the default)

![SimpleCTF/img/Nmap.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/Nmap.png)

We see that Port 21 (FTP) and Port 80 (HTTP) are open.

---
### Question 2: What is running on the higher port?
---

I ran Nmap a second time since the first scan only scanned ports 1-1000. We see SSH running on Port 2222.

![SimpleCTF/img/Nmap2.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/Nmap2.png)

---
### Question 3: What's the CVE you're using against the application?
and
### Question 4: To what kind of vulnerability is the application vulnerable?
---

Let's do some digging to see what we can exploit.

We see that there is an FTP server running, and you can log in as Anonymous. I did that and came across a file named ForMitch.txt. By the looks of it, Mitch is not very good at creating unique and complex passwords.

![SimpleCTF/img/ForMitch.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/ForMitch.png)

We also see that there is a robots.txt on the website, but that doesn't have much information. I thought that maybe we could use CUPS as the point of exploitation but that proved false.

![SimpleCTF/img/robots.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/robots.png)

I tried to navigate to the /openemr-5_0_1_3 directory but that didn't have anything either.

![SimpleCTF/img/openemr.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/openemr.png)

I guess I should break out the shovel again and keep digging. Life's a garden right?

Anywho, a Gobuster scan produces some results. Pretty "simple" huh?

![SimpleCTF/img/gobuster.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/gobuster.png)

What is on that directory?

![SimpleCTF/img/cms.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/cms.png)

Some Google-Fu and I found this CVE. It uses SQL Injection (sqli) to uncover login information.

![SimpleCTF/img/CVE.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/CVE.png)

---
### Question 5: What's the password?
---

Now here is where I ran into some issues using the provided Python script. It was written for pre-Python3 so I had to make some tweaks. It's not pretty but it works, and I included it here.

https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/Python/cve.py

![SimpleCTF/img/cracked%20password.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/cracked%20password.png)

BOOM! We have a username and password and we know Mitch has a habit at reusing passwords.

---
### Question 6: Where can you login with the details obtained?
and
### Question 7: What's the user flag?
---

How much do you wanna bet that Mitch's credentials work on SSH?

![SimpleCTF/img/user%20flag.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/user%20flag.png)

We can also get the user flag here.

---
### Question 8: Is there any other user in the home directory? What's its name?
---

Quick directory change and we can see if there are any other users in the home directory.

![SimpleCTF/img/other%20user.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/other%20user.png)

---
### Question 9: What can you leverage to spawn a privileged shell?
---

Here is where I took awhile to find the information. I ran Linpeas and couldn't find anything and did a few other things that didn't pan out. Sometimes the quick answer is in fact the answer. I ran sudo -l and found that Mitch can run vim as root!

![SimpleCTF/img/vim.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/vim.png)

GTFOBins to the rescue!

![SimpleCTF/img/sudovim.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/sudovim.png)

![SimpleCTF/img/root.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/root.png)

And now we are ROOT!

---
### Quesion 10: What's the root flag?
---

Root flag is now within reach!

![SimpleCTF/img/Header.png](https://github.com/h3r37ix/THM-Walkthroughs/blob/main/SimpleCTF/img/roottxt.png)
