***

Author : dxWiz
Date : 29 Apr 2022

***


# Task 1 Deploy the machine
# Task 2 Reconnaissance

Do an nmap scan to get the number of open ports and the version of apache server with this command nmap -sC -sV $IP.

Do a gobuster scan to get the hidden directory.

# Task 3 Getting a shell

Upload a PHP reverse shell file to the server however it will block the .php extension, change it to php5 then setup a netcat session listening to a port of your desire. Execute the file to get a reverse shell.

Get the user.txt flag

# Task 4 Privilege escalation 

Search for SUID files with this command `find / -type f -perm /4000 2>/dev/null

Refer to GTFobin and follow the instruction to get the root flag.
