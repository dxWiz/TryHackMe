command to fix git corrupted error

    find .git/objects/ -size 0 -exec rm -f {} \;
    git fetch origin
