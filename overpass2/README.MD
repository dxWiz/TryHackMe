***************

Author : dxWiz
Date : 12 Mar 2022 11:39 PM


**************


It is recommended I do are rewrite on this incomplete research


Task 1 Forensics - Analyse the PCAP 

Just open up the .pcap file do the necessary analysis to get the answer

carcking the the hashes is relatively easy with john + fasttrack wordlist (/usr/share/wordlists/fasttrack.txt)
******

Task 2 Research - Analyse the code 

Study the source code from https://github.com/NinjaJc01/ssh-backdoor



To crack the salted hash with john and rockyou.txt
john --format=dynamic_82 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt 


to list dynamic format : john --list=subformats
Format = dynamic_82  type = dynamic_82: sha512($p.$s)


hash.txt content
----------------
6d05358f090eea56a238af02e47d44ee5489d234810ef6240280857ec69712a3e5e370b8a41899d0196ade16c0d54327c5654019292cbfe0b5e98ad1fec71bed$1c362db832f3f864c8c2fe05f2002a05

***note : salt is separated by $ reference from this url : https://miloserdov.org/?p=5960#66
---------------------------------------------------------

---------------------------------------------------------
To crack the salted hash with hashcat and rockyou.txt
./hashcat 6d05358f090eea56a238af02e47d44ee5489d234810ef6240280857ec69712a3e5e370b8a41899d0196ade16c0d54327c5654019292cbfe0b5e98ad1fec71bed:1c362db832f3f864c8c2fe05f2002a05 rockyou.txt -m 1710

***note : salt is separated by colon
---------------------------------------------------------

Additional Studies
At the end of the article at miloserdov.org, it is mentioned that when the salt has special characters in it, converting salt to hexadecimal salt is to be done with a program called raw2dyna.

eg: raw2dyna -2h='STRING'


raw2dyna is a program that convert ASCII char into equivalent hexadecimal value. Please refer to the ASCII to HEX chart for actual value verification. 


Task 3 Attack - Get back in! 

To be continued....
