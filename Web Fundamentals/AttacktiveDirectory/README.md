***

Author : dxWiz  
Date : 02 Apr 2022

***

# Task 1 Intro Deploy The Machine 

Start the virtual machine and connect to TryHackMe network via the vpn

# Task 2 Intro Setup

Follow the instruction to install impacket, bloodhound and Neo4j

# Task 3 Enumeration Welcome to Attacktive Directory

I started of with an nmap scan and found out that the IP has a few interesting open ports. First we can look into the webserver at port 80 and found nothing interesting.

    # Nmap 7.92 scan initiated Tue Mar 29 07:30:36 2022 as: nmap -sC -sV -oN initial.nmap $IP
    Nmap scan report for $IP
    Host is up (0.28s latency).
    Not shown: 987 closed tcp ports (conn-refused)
    PORT     STATE SERVICE       VERSION
    53/tcp   open  domain        Simple DNS Plus
    80/tcp   open  http          Microsoft IIS httpd 10.0
    | http-methods: 
    |_  Potentially risky methods: TRACE
    |_http-title: IIS Windows Server
    |_http-server-header: Microsoft-IIS/10.0
    88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2022-03-29 11:31:17Z)
    135/tcp  open  msrpc         Microsoft Windows RPC
    139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
    389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
    445/tcp  open  microsoft-ds?
    464/tcp  open  kpasswd5?
    593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
    636/tcp  open  tcpwrapped
    3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
    3269/tcp open  tcpwrapped
    3389/tcp open  ms-wbt-server Microsoft Terminal Services
    | rdp-ntlm-info: 
    |   Target_Name: THM-AD
    |   NetBIOS_Domain_Name: THM-AD
    |   NetBIOS_Computer_Name: ATTACKTIVEDIREC
    |   DNS_Domain_Name: spookysec.local
    |   DNS_Computer_Name: AttacktiveDirectory.spookysec.local
    |   Product_Version: 10.0.17763
    |_  System_Time: 2022-03-29T11:31:35+00:00
    | ssl-cert: Subject: commonName=AttacktiveDirectory.spookysec.local
    | Not valid before: 2022-03-28T11:29:56
    |_Not valid after:  2022-09-27T11:29:56
    |_ssl-date: 2022-03-29T11:31:44+00:00; +2s from scanner time.
    Service Info: Host: ATTACKTIVEDIREC; OS: Windows; CPE: cpe:/o:microsoft:windows

    Host script results:
    | smb2-security-mode: 
    |   3.1.1: 
    |_    Message signing enabled and required
    |_clock-skew: mean: 1s, deviation: 0s, median: 1s
    | smb2-time: 
    |   date: 2022-03-29T11:31:38
    |_  start_date: N/A

    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    # Nmap done at Tue Mar 29 07:31:50 2022 -- 1 IP address (1 host up) scanned in 74.70 seconds

By looking at the question, it is asking what other tool can be used to enumerate port 139/445 and the answer is `enum4linux`. Let's do that with the following command.  
`enum4linux -a $IP`. -a is to do all the test and below is the results.

    Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sat Apr  2 10:19:33 2022

     =========================================( Target Information )=========================================

    Target ........... 10.10.199.245
    RID Range ........ 500-550,1000-1050
    Username ......... ''
    Password ......... ''
    Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


     ===========================( Enumerating Workgroup/Domain on 10.10.199.245 )===========================


    [E] Can't find workgroup/domain



     ===============================( Nbtstat Information for 10.10.199.245 )===============================

    Looking up status of 10.10.199.245
    No reply from 10.10.199.245

     ===================================( Session Check on 10.10.199.245 )===================================


    [+] Server 10.10.199.245 allows sessions using username '', password ''


     ================================( Getting domain SID for 10.10.199.245 )================================

    Domain Name: THM-AD
    Domain Sid: S-1-5-21-3591857110-2884097990-301047963

    [+] Host is part of a domain (not a workgroup)


     ==================================( OS information on 10.10.199.245 )==================================


    [E] Can't get OS info with smbclient


    [+] Got OS info for 10.10.199.245 from srvinfo: 
    Could not initialise srvsvc. Error was NT_STATUS_ACCESS_DENIED


     =======================================( Users on 10.10.199.245 )=======================================


    [E] Couldn't find users using querydispinfo: NT_STATUS_ACCESS_DENIED



    [E] Couldn't find users using enumdomusers: NT_STATUS_ACCESS_DENIED


     =================================( Share Enumeration on 10.10.199.245 )=================================

    do_connect: Connection to 10.10.199.245 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)

      Sharename       Type      Comment
      ---------       ----      -------
    Reconnecting with SMB1 for workgroup listing.
    Unable to connect with SMB1 -- no workgroup available

    [+] Attempting to map shares on 10.10.199.245


     ===========================( Password Policy Information for 10.10.199.245 )===========================


    [E] Unexpected error from polenum:



    [+] Attaching to 10.10.199.245 using a NULL share

    [+] Trying protocol 139/SMB...

      [!] Protocol failed: Cannot request session (Called Name:10.10.199.245)

    [+] Trying protocol 445/SMB...

      [!] Protocol failed: SAMR SessionError: code: 0xc0000022 - STATUS_ACCESS_DENIED - {Access Denied} A process has requested access to an object but has not been granted those access rights.



    [E] Failed to get password policy with rpcclient



     ======================================( Groups on 10.10.199.245 )======================================


    [+] Getting builtin groups:


    [+]  Getting builtin group memberships:


    [+]  Getting local groups:


    [+]  Getting local group memberships:


    [+]  Getting domain groups:


    [+]  Getting domain group memberships:


     ==================( Users on 10.10.199.245 via RID cycling (RIDS: 500-550,1000-1050) )==================


    [I] Found new SID: 
    S-1-5-21-3591857110-2884097990-301047963

    [I] Found new SID: 
    S-1-5-21-3591857110-2884097990-301047963

    [+] Enumerating users using SID S-1-5-21-3532885019-1334016158-1514108833 and logon username '', password ''

    S-1-5-21-3532885019-1334016158-1514108833-500 ATTACKTIVEDIREC\Administrator (Local User)
    S-1-5-21-3532885019-1334016158-1514108833-501 ATTACKTIVEDIREC\Guest (Local User)
    S-1-5-21-3532885019-1334016158-1514108833-503 ATTACKTIVEDIREC\DefaultAccount (Local User)
    S-1-5-21-3532885019-1334016158-1514108833-504 ATTACKTIVEDIREC\WDAGUtilityAccount (Local User)
    S-1-5-21-3532885019-1334016158-1514108833-513 ATTACKTIVEDIREC\None (Domain Group)

    [+] Enumerating users using SID S-1-5-21-3591857110-2884097990-301047963 and logon username '', password ''

    S-1-5-21-3591857110-2884097990-301047963-500 THM-AD\Administrator (Local User)
    S-1-5-21-3591857110-2884097990-301047963-501 THM-AD\Guest (Local User)
    S-1-5-21-3591857110-2884097990-301047963-502 THM-AD\krbtgt (Local User)
    S-1-5-21-3591857110-2884097990-301047963-512 THM-AD\Domain Admins (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-513 THM-AD\Domain Users (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-514 THM-AD\Domain Guests (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-515 THM-AD\Domain Computers (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-516 THM-AD\Domain Controllers (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-517 THM-AD\Cert Publishers (Local Group)
    S-1-5-21-3591857110-2884097990-301047963-518 THM-AD\Schema Admins (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-519 THM-AD\Enterprise Admins (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-520 THM-AD\Group Policy Creator Owners (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-521 THM-AD\Read-only Domain Controllers (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-522 THM-AD\Cloneable Domain Controllers (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-525 THM-AD\Protected Users (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-526 THM-AD\Key Admins (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-527 THM-AD\Enterprise Key Admins (Domain Group)
    S-1-5-21-3591857110-2884097990-301047963-1000 THM-AD\ATTACKTIVEDIREC$ (Local User)

     ===============================( Getting printer info for 10.10.199.245 )===============================

    Could not initialise spoolss. Error was NT_STATUS_ACCESS_DENIED


    enum4linux complete on Sat Apr  2 10:34:47 2022


On top of that we can do `smbclient -L \\\\$IP` but it does not return anything interesting as well.

# Task 4 Enumeration Enumerating Users via Kerberos

Add `$IP spookysec.local` in /etc/hosts 

Install `kerbrute` to brute force users discovery with the following command with the userlist provided

`kerbrute userenum --dc spookysec.local -d spookysec.local userlist.txt`

    ─$ /opt/kerbrute/kerbrute userenum --dc spookysec.local -d spookysec.local  userlist.txt                                                                                                130 ⨯

        __             __               __     
       / /_____  _____/ /_  _______  __/ /____ 
      / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
     / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
    /_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

    Version: v1.0.3 (9dad6e1) - 04/02/22 - Ronnie Flathers @ropnop

    2022/04/02 10:56:00 >  Using KDC(s):
    2022/04/02 10:56:00 >  	spookysec.local:88

    2022/04/02 10:56:01 >  [+] VALID USERNAME:	 james@spookysec.local
    2022/04/02 10:56:15 >  [+] VALID USERNAME:	 svc-admin@spookysec.local
    2022/04/02 10:56:23 >  [+] VALID USERNAME:	 James@spookysec.local
    2022/04/02 10:56:28 >  [+] VALID USERNAME:	 robin@spookysec.local
    2022/04/02 10:57:06 >  [+] VALID USERNAME:	 darkstar@spookysec.local
    2022/04/02 10:57:27 >  [+] VALID USERNAME:	 administrator@spookysec.local
    2022/04/02 10:58:10 >  [+] VALID USERNAME:	 backup@spookysec.local
    2022/04/02 10:58:31 >  [+] VALID USERNAME:	 paradox@spookysec.local
    2022/04/02 11:00:49 >  [+] VALID USERNAME:	 JAMES@spookysec.local
    2022/04/02 11:01:36 >  [+] VALID USERNAME:	 Robin@spookysec.local
    2022/04/02 11:06:10 >  [+] VALID USERNAME:	 Administrator@spookysec.local
    2022/04/02 11:14:39 >  [+] VALID USERNAME:	 Darkstar@spookysec.local
    2022/04/02 11:17:18 >  [+] VALID USERNAME:	 Paradox@spookysec.local
    2022/04/02 11:26:40 >  [+] VALID USERNAME:	 DARKSTAR@spookysec.local
    2022/04/02 11:29:24 >  [+] VALID USERNAME:	 ori@spookysec.local
    2022/04/02 11:34:25 >  [+] VALID USERNAME:	 ROBIN@spookysec.local
    2022/04/02 11:46:55 >  Done! Tested 73317 usernames (16 valid) in 3054.313 seconds

The two notable accounts found are svc-admin and backup.

# Task 5 Exploitation Abusing Kerberos
# Task 6 Enumeration Back to the Basics
# Task 7 Domain Privilege Escalation Elevating Privileges within the Domain
# Task 8 Flag Submission Flag Submission Panel 
