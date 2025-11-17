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
