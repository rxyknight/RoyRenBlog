---
layout: post
title: 'Linux Special Permission'
date: '2016-05-12'
header-img: "img/post-bg-unix.jpg"
tags:
     - linux
author: 'Roy Ren'
---

In Linux, we are familiar with permissions of **rwx** (read, write and execute). In addition, there are special permissions which are **setuid**, **setgid** and **Sticky Bit**. We will also talk about a command, **chattr** as well.

## setuid Permission

When set-user identification (setuid) permission is set on an executable file, a process that runs this file is granted access based on the owner of the file (usually root), rather than the user who is running the executable file. This special permission allows a user to access files and directories that are normally only available to the owner. For example, the setuid permission on the passwd command makes it possible for a user to change passwords, assuming the permissions of the root ID:

	-r-sr-sr-x   3 root     sys       104580 Sep 16 12:02 /usr/bin/passwd

### How to set setuid permission

	chmod u+s file # file MUST have x priviledge
	chmod 4xxx file # file MUST have x priviledge
	
This special permission presents a **security risk**, because some determined users can find a way to maintain the permissions that are granted to them by the setuid process even after the process has finished executing. 

## setgid Permission

The set-group identification (setgid) permission is similar to setuid, except that the process's effective group ID (GID) is changed to the group owner of the file, and a user is granted access based on permissions granted to that group. The /usr/bin/mail command has setgid permissions: 

	-r-x--s--x   1 root     mail       63628 Sep 16 12:01 /usr/bin/mail
	
### How to set setgid permission

	chmod g+s file/dir # if file, MUST have x priviledge
	chmod 2xxx file/dir # if file, MUST have x priviledge

When setgid permission is applied to a directory, files that were created in this directory belong to the group to which the directory belongs, not the group to which the creating process belongs. Any user who has write and execute permissions in the directory can create a file there. However, the file belongs to the group that owns the directory, not to the user's group ownership. 

## Sticky Bit

The sticky bit is a permission bit that protects the files within a directory. If the directory has the sticky bit set, a file can be deleted only by the owner of the file, the owner of the directory, or by root. This special permission prevents a user from deleting other users' files from public directories such as /tmp: 

	drwxrwxrwt 7  root  sys   400 Sep  3 13:37 tmp

### How to set Sticky Bit permission

	chmod o+t dir
	chmod 1xxx dir
	
## chattr Command

chattr (Change Attribute) is a command line Linux utility that is used to set/unset certain attributes to a file in Linux system to secure accidental deletion or modification of important files and folders, even though you are logged in as a root user.

There are many attributes and associated flags, the common flag are **a** and **i**. 

* A file is set with ‘a‘ attribute, can only be open in append mode for writing.
* A file is set with ‘i‘ attribute, cannot be modified (immutable). Means no renaming, no symbolic link creation, no execution, no writable, only superuser can unset the attribute.

		chattr +a file/dir
		chattr +i file/dir

