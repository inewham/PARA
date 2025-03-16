# Overview
This guide is to instruct the reader through the process of creating a symbolic link (or a shortcut) from the Linux command line (CLI).  

A symbolic link, also referred to as a Symlink, maybe beneficial to quickly navigate to a frequently access folder that is deep in the file structure.

NOTE: There are pro's and con's with this i.e., if you executed a **pwd**, it would output the symlink path, in which case you would add the **-P** switch to display the physical directory path. 
# Create a symbolic link
```
ln -s <TARGET> <SYMLINK-DIRECTORY/FILE>
```
# Reference
[The Ultimate Guide to Creating Linux Symlinks | Linode Docs](https://www.linode.com/docs/guides/linux-symlinks/)
