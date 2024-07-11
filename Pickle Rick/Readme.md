#  ***TryHackMe Walkthrough : Pickle Rick***

![alt text](https://github.com/Hk-Hacker-Harsh/TryHackMe/blob/Root/Pickle%20Rick/IMG/1.png?raw=true "TryHAckMe Pickle Rick Link")

Hello Guys, Today we'll Capture Flags from Pickle Rick CTF Machine by @tryhackme and @ar33zy available for free on TryHackMe **[here](https://tryhackme.com/room/picklerick)** .

So, Let's Go!

"This Rick and Morty-themed challenge requires you to exploit a web server and find three ingredients to help Rick make his potion and transform himself back into a human from a pickle."

Click on the start Machine Button in the task, and give it 3-5 min to fully load.

Now, Start Attakbox or connect using Open-VPN.

> Note: Check the connecting using ping command: ping \<Machine IP\>.



## ***Enumeration***

Our first step will be pretty simple, to run an nmap scan to find out open ports on the machine, Run the following command in the Terminal:

`nmap -Pn -O -sV -sC -T5 -vv -p- \<Machine IP\>`

> nmap : tool for network scanning

> -Pn : Scan even if the machine seems down

> -O : OS scan

> -sV : Version Scan for services

> -sC : To enable script run

> -T5 : 5th level of speed

> -vv : Verbose verbose

> -p- : To scan all ports from 1 to 65535

***Output:***
