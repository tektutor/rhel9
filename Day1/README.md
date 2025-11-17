# Day 1

## Linux Kernel
<pre>
- is an open source project
- developed and maintained by Linux Torvalds and the open source community
</pre>

## Linux Distribution Overview
<pre>

- examples
  - Ubuntu
    - ideal for linux beginners
    - it works well on a machine with normal system configuration
  - RHEL ( Red Hat Enterprise Linux )
  - CentOS Stream
  - Rocky Linux
  - Manjaro
  - Kali 
  - Suse
</pre>

## Linux Family
<pre>
- There are many Linux family
- each Linux Family supports a particular type of Package Manager
  - Red Hat Family supports
    - yum, dnf, rpm package managers
  - Debian Family supports
    - apt, apt-get
- For instance, below distributions are part of Red Hat Linux Family
  - RHEL 
  - CentOS Stream
  - CentOS ( Reached end of life - not supported anymore CentOS v7.9 was the last release )
  - Fedora is an upstream of RHEL
  
- For instance, the below distributions are part of Debian
  - Ubuntu
  - Kali
</pre>

## Info - Package Manager Overview
<pre>
- every linux distribution comes with one or more package managers
- package manager helps us
  - installing software
  - uninstalling software
  - upgrading software
- package managers depends on the repository server to download the software installer packages
- in case the reposiotry url for a particular software is not available on your system, it is possible it won't be able to install that software
</pre>

## Lab - Finding the Linux distribution and its version in RHEL
```
cat /etc/redhat-release
```

Expected output
<pre>
[palmeto@palmeto.org ~]$ cat /etc/redhat-release 
Red Hat Enterprise Linux release 9.7 (Plow)  
</pre>

## Lab - Finding linux kernel version
```
uname -r
```

Finding full details
```
uname -a
```

## Lab - Finding present working directory
Find the currently logged in user
```
whoami
```

Switch to user home directory
```
cd ~
```

Now find out the present working directory
```
pwd
```

Expected output
<pre>
[palmeto@palmeto.org ~]$ cd ~
[palmeto@palmeto.org ~]$ 
[palmeto@palmeto.org ~]$ pwd
/home/palmeto
[palmeto@palmeto.org ~]$ whoami
palmeto
</pre>

## Lab - Listing files and folders
```
pwd
cd /
ls -l
```

Expected output
<pre>
[palmeto@palmeto.org ~]$ pwd
/home/palmeto
[palmeto@palmeto.org ~]$ cd /
[palmeto@palmeto.org /]$ ls -l
total 28
dr-xr-xr-x.   2 root root    6 Jun 25  2024 afs
lrwxrwxrwx.   1 root root    7 Jun 25  2024 bin -> usr/bin
dr-xr-xr-x.   5 root root 4096 Nov 13 13:27 boot
drwxr-xr-x   20 root root 3480 Nov 17  2025 dev
drwxr-xr-x. 145 root root 8192 Nov 17  2025 etc
drwxr-xr-x.   3 root root   21 Jun 25  2024 home
lrwxrwxrwx.   1 root root    7 Jun 25  2024 lib -> usr/lib
lrwxrwxrwx.   1 root root    9 Jun 25  2024 lib64 -> usr/lib64
drwxr-xr-x.   2 root root    6 Jun 25  2024 media
drwxr-xr-x.   2 root root    6 Jun 25  2024 mnt
drwxr-xr-x.   2 root root    6 Jun 25  2024 opt
dr-xr-xr-x  421 root root    0 Nov 17  2025 proc
dr-xr-x---.   4 root root 4096 Nov 14 19:16 root
drwxr-xr-x   47 root root 1320 Nov 17  2025 run
lrwxrwxrwx.   1 root root    8 Jun 25  2024 sbin -> usr/sbin
drwxr-xr-x.   2 root root    6 Jun 25  2024 srv
dr-xr-xr-x   13 root root    0 Nov 17  2025 sys
drwxrwxrwt.  20 root root 4096 Nov 17 12:11 tmp
drwxr-xr-x.  12 root root  144 Nov 13 12:47 usr
drwxr-xr-x.  20 root root 4096 Nov 13 13:27 var  
</pre>
