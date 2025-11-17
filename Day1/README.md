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

In the above output, the first column indicates the permission each user has on the file/directory.

<pre>
d - the first letter 'd' indicates directory  
l - the first letter 'l' indicates link file(shortcut)
r - read ( 4 )
w - write ( 2 )
x - execute ( 1 )
r - read
w - write
x - execute
r - read
w - write
x - execute
</pre>

Let's understand the permission pattern
"drwxrwxrwx"

d - directory
rwx - the currently logged in user has read,write and execute permission for that folder
the second set of rwx - indicates the access the other users in the same user group has for that folder
the third set of rwx - indicates the access other users in other user group has for that folder

## Lab - Let's create an user as an administrator
Replace 'jegan' with your short name. When prompted for password, type 'palmeto' without quotes.
```
sudo su -
useradd -m jegan
ls -l /home
```

Let's find the IP address of the lab machine
```
ifconfig
```

Attempt ssh as 'jegan' user ( Replace 'jegan' with your user name )
```
ssh jegan@192.168.8.36
```

Expected output
<pre>
[root@palmeto.org ~]# ifconfig
enp1s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.8.36  netmask 255.255.252.0  broadcast 192.168.11.255
        inet6 fe80::82e8:2cff:fee3:fecc  prefixlen 64  scopeid 0x20<link>
        ether 80:e8:2c:e3:fe:cc  txqueuelen 1000  (Ethernet)
        RX packets 96572  bytes 56322249 (53.7 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 43294  bytes 76077710 (72.5 MiB)
        TX errors 0  dropped 690 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 531  bytes 52200 (50.9 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 531  bytes 52200 (50.9 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

[palmeto@palmeto.org /]$ ssh jegan@192.168.8.36
The authenticity of host '192.168.8.36 (192.168.8.36)' can't be established.
ED25519 key fingerprint is SHA256:zjeN8DELqfkl5a3J9RLVwe9SXRYUvEQ0L+EhhM6EajY.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.8.36' (ED25519) to the list of known hosts.
jegan@192.168.8.36's password: 
Permission denied, please try again.
jegan@192.168.8.36's password: 
Permission denied, please try again.
jegan@192.168.8.36's password: 
jegan@192.168.8.36: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).
</pre>

The reason the login failed is because we haven't configured the password after creating the user. 
Let's configure the password for user 'jegan' as admin
```
sudo su -
passwd jegan
```

Expected output
<pre>
[root@palmeto.org ~]# passwd jegan
Changing password for user jegan.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.
[root@palmeto.org ~]#   
</pre>

Now switch the second tab, to login as 'jegan' user
```
ssh jegan@192.168.8.36
```

Expected output
<pre>
[palmeto@palmeto.org /]$ ssh jegan@192.168.8.36
jegan@192.168.8.36's password: 
Register this system with Red Hat Insights: rhc connect

Example:
# rhc connect --activation-key <key> --organization <org>

The rhc client and Red Hat Insights will enable analytics and additional
management capabilities on your system.
View your connected systems at https://console.redhat.com/insights

You can learn more about how to register your system 
using rhc at https://red.ht/registration
Last login: Mon Nov 17 12:37:16 2025 from 192.168.8.36  
</pre>

## Lab - Creating a folder under your home directory
```
cd ~
pwd
mkdir vim-demo
cd vim-demo
ls -l
```

Let's create an empty zero byte file
```
touch mobiles.txt
ls -l
```

## Lab - Editing a text file using vim text editor
```
cd ~/vim-demo
vim mobiles.txt
ls -l

# See the content of the file mobiles.txt
cat mobiles.txt
```

Set line number. ( you need to press esc button and Shift :set number )

In order to type anything using vim editor on the file mobiles.txt, you need to press esc i

Saving the changes in the file mobiles.txt ( Press Esc Shift :w )

Exiting vim editor ( Press Esc :q )

Expected output
<pre>
[jegan@palmeto.org vim-demo]$ touch mobiles.txt
[jegan@palmeto.org vim-demo]$ ls -l
total 0
-rw-r--r-- 1 jegan jegan 0 Nov 17 12:46 mobiles.txt
[jegan@palmeto.org vim-demo]$ vim mobiles.txt 
[jegan@palmeto.org vim-demo]$ ls
mobiles.txt
[jegan@palmeto.org vim-demo]$ ls -l
total 4
-rw-r--r-- 1 jegan jegan 23 Nov 17 12:53 mobiles.txt
[jegan@palmeto.org vim-demo]$ cat mobiles.txt 
Nothing 3Pro
Iphone 16  
</pre>

Appending a mobile at the end of the file without overwriting
```
ls -l
echo "Motorola" >> mobiles.txt
cat mobiles.txt
```

Expected output
<pre>
[jegan@palmeto.org vim-demo]$ echo "Motorola" >> mobiles.txt 
[jegan@palmeto.org vim-demo]$ cat mobiles.txt 
Nothing 3Pro
Iphone 16
Motorola  
</pre>

Overwriting the file
```
echo "Motorola" > mobiles.txt
cat mobiles.txt
```

Expected output
<pre>
[jegan@palmeto.org vim-demo]$ cat mobiles.txt 
Nothing 3Pro
Iphone 16
Motorola
[jegan@palmeto.org vim-demo]$ echo "Motorola" > mobiles.txt 
[jegan@palmeto.org vim-demo]$ cat mobiles.txt 
Motorola  
</pre>
