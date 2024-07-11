#  ***TryHackMe Walkthrough : Pickle Rick***

![Pickle Rick](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/1.png?raw=true "TryHAckMe Pickle Rick Link")

Hello Guys, Today we'll Capture Flags from Pickle Rick CTF Machine by @tryhackme and @ar33zy available for free on TryHackMe **[here](https://tryhackme.com/room/picklerick)** .

So, Let's Go!

"This Rick and Morty-themed challenge requires you to exploit a web server and find three ingredients to help Rick make his potion and transform himself back into a human from a pickle."

Click on the start Machine Button in the task, and give it 3-5 min to fully load.

Now, Start Attakbox or connect using Open-VPN.

> Note: Check the connecting using ping command: ping <Machine IP\>.



## ***Enumeration***

Our first step will be pretty simple, to run an nmap scan to find out open ports on the machine, Run the following command in the Terminal:

`nmap -Pn -O -sV -sC -T5 -vv -p- <Machine IP\>`

> nmap : tool for network scanning

> -Pn : Scan even if the machine seems down

> -O : OS scan

> -sV : Version Scan for services

> -sC : To enable script run

> -T5 : 5th level of speed

> -vv : Verbose verbose

> -p- : To scan all ports from 1 to 65535




***Output:***

![Nmap Scan](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/2.png?raw=true)

From the enumeration process we found out that their are two services running on the machine:
1. SSH
2. Http


So, lets check for the http service running on port 80 on the machine, search for the ip address on browser.

![HTTP](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/3.png?raw=true)


Ok, now we got a web service running on machine IP, Let's check for its Source Code first:

![Browser](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/4.png?raw=true)


From the website's source code, we can see a comment saying:
> Note to self, remember username!    Username: R1ckRul3s


We got a username of something (no idea about it), note it down somewhere else.

Then, run a command to find subdomain and sub directories related to the website, using gobuster (install it using : apt install gobuster):
`gobuster dir -w /usr/share/wordlists/dirb/common.txt -x .php,.txt,.html -u http://<Machine IP>/`

> gobuster : Subdomain finding tool

> dir : to search for directories

> -w <path> : wordlist with its path

> -x <extensions> : search wordlist with extensions also

> -u : URL of webserver


***Output:***
![Gobuster](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/5.png?raw=true)


Let's go for each subdomain one-by-one: http://<Machine IP\>/<Subdomain>

  /.php                 (Status: 403) [Size: 291]
  /.html                (Status: 403) [Size: 292]
  /.hta.php             (Status: 403) [Size: 295]
  /.hta                 (Status: 403) [Size: 291]
  /.hta.txt             (Status: 403) [Size: 295]
  /.htaccess            (Status: 403) [Size: 296]
  /.htaccess.txt        (Status: 403) [Size: 300]
  /.hta.html            (Status: 403) [Size: 296]
  /.htaccess.html       (Status: 403) [Size: 301]
  /.htpasswd            (Status: 403) [Size: 296]
  /.htaccess.php        (Status: 403) [Size: 300]
  /.htpasswd.php        (Status: 403) [Size: 300]
  /.htpasswd.txt        (Status: 403) [Size: 300]
  /.htpasswd.html       (Status: 403) [Size: 300]


Code/Status 403 mean we dont have permission to check these pages.
  /assets: 
  ![Assets](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/6.png?raw=true)

Didn't find anything on /assets, lets move on to the next.
* /index.html is the home/default page.
* /robots.txt:
      ![robots.txt](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/8.png?raw=true)

* /denied.php, /login.php & /portal.php are all redirecting to /login.php:
      ![login.php](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/7.png?raw=true)


Here, we got a login page.
ok, previously we got a username  : R1ckRul3s
and we also got a word string from robots.txt : Wubbalubbadubdub
So, lets try them as username and password.


After login we got a Command panel:
![Command Pannel](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/9.png?raw=true)


Lets try to run simple linux command like: ls, whoami , date, cat ,etc.

 

OK, so its a Linux Server. Now lets see results of ls:
![Cat](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/10.png?raw=true)


Got a list of files on the server. lets try to read a file using "cat <filename>".

OOhhooo  some commands are disabled.

lets try to access Sup3rS3cretPickl3Ingred.txt using url method:

**http://\<Machine IP\>/Sup3rS3cretPickl3Ingred.txt**

***Output:***

![code](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/11.png?raw=true)


Got the first ingredient.

![ans1](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/12.png?raw=true)


So, now lets check other files too: clue.txt

`Output: "Look around the file system for the other ingredient."`

Use ls command to look around the `/home` dir and other directory.

Use sudo command also to check `/root` directory.


Second Ingredient: `/home/rick/second ingredient` : to read this file use less command or tac command with ' \ '  or  ' "" ' \(to avoid mistakes due to \[space\] \) in command panel :
        ex.: `tac /home/rick/second\ ingredients`
              `less /home/rick/"second ingredients"`

***Output: 1 jerry tear***

![ans2](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/13.png?raw=true)


Third Ingredient: `/root`

![Command](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/14.png?raw=true)


Use sudo command to list files in /root directory and read the file using `tac` or `less` command with sudo :

***Output: fleeb juice***

![Ans3](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle-Rick/IMG/15.png?raw=true)


***Finally!! we have successfully solved all challenges!
Thank YOU!***
