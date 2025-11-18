# Day 1

## Linux Kernel
<pre>
- is an open source project
- developed and maintained by Linux Torvalds and the open source community
- every Linux distribution depends on this Linux kernel project
- different Linux distibutions uses different version of Linux kernel as per their strategy 
</pre>

## Linux Distribution Overview
<pre>
- each Linux distribution bundles their preferred linux kernel version, linux tools, shell and other tools
- most Linux distibution supports either KDE or Gnone Desktop to support GUI
- KDE is built using Qt C++ GUI Framework
- while Gnome is built using GTK C++ GUI Framework
- Apart from these two popular Desktop environments there are many others, 
  - Gtk based
    - XFCE
    - Cinnamon
    - MATE
  - Qt based
    - LxQt
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

## Lab - Copy/paste in vim
To copy one line from the current cursor position
```
Esc yy
```

To copy 3 lines from the current cursor position
```
Esc 3 yy
```

To paste
```
Esc p
```

To paste the line(s) copied 10 times
```
Esc 10p
```

## Lab - Copying a block of words in different lines ( let's say copy third column from line 2 to line 4 )
Assume we have a file with below content 
```
This is First line
This is Second line
This is Third line
This is Fourth line
This is Fifth line
```

<pre>
- Take the cursor to line number 2 and navigate to letter 'S' in the word Second ( Press Ctrl + V )
- Keep moving the cursor using up/down arrows select word Fourth in the fourth line
- To copy press y
- Move the cursor to wherever you wish to paste
- Press p
</pre>

## Lab - Find and replace in vim
<pre>
- Esc /word-your-searching/word-you-wish-to-replace-with Enter key  
</pre>

## Info - SSH Overview
<pre>
- Navigate to your home directory .ssh folder
- command is cd /home/palmeto/.ssh
</pre>

## Lab - Public/Private key based login access

Let's login to the lab machine as palmeto user and generate key pair
```
ssh-keygen -t ed25519 -N "" -f ~/.ssh/id_ed25519
ls -l /home/palmeto/.ssh
```

Expected output
<pre>
[palmeto@palmeto.org ~]$ ssh-keygen -t ed25519 -N "" -f ~/.ssh/id_ed25519
Generating public/private ed25519 key pair.
Your identification has been saved in /home/palmeto/.ssh/id_ed25519
Your public key has been saved in /home/palmeto/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:tJjCJJk6Qm8r6uFF2nAySzY7oaNQ8xUfHL6UnWNQr20 palmeto@palmeto.org
The key's randomart image is:
+--[ED25519 256]--+
|        o..      |
|   o   o = o     |
| .+ . . B = .    |
|...+   B = +     |
|+Oo=o + S . E    |
|=o#o.o     .     |
|+B +.            |
|=.=              |
|+o               |
+----[SHA256]-----+
[palmeto@palmeto.org ~]$ ls -l /home/palmeto/.ssh
total 16
-rw------- 1 palmeto palmeto 411 Nov 17 15:29 id_ed25519
-rw-r--r-- 1 palmeto palmeto 101 Nov 17 15:29 id_ed25519.pub
-rw------- 1 palmeto palmeto 835 Nov 17 15:07 known_hosts  
</pre>

Let's copy the public key we generated as authorized_keys
```
cp ~/.ssh/id_ed25519.pub ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Let's configure the SSH Server to allow key based login authentication
```
sudo su -
vim /etc/ssh/sshd_config
```

We need to search for the below line and update as shown below
<pre>
PubKeyAuthentication yes
AuthorizedKeyFile .ssh/authorized_keys
PasswordAuthentication no
</pre>

To apply config changes, we need to restart the SSH Server
```
sudo systemctl daemon-reload
sudo systemctl restart sshd
sudo systemctl status sshd
```

Let's check if SSH login for palmeto user is working using key ( i.e without password )
```
ssh palmeto@192.168.8.36
```
<pre>
[palmeto@palmeto.org ~]$ ssh palmeto@192.168.8.36
Activate the web console with: systemctl enable --now cockpit.socket

Register this system with Red Hat Insights: rhc connect

Example:
# rhc connect --activation-key <key> --organization <org>

The rhc client and Red Hat Insights will enable analytics and additional
management capabilities on your system.
View your connected systems at https://console.redhat.com/insights

You can learn more about how to register your system 
using rhc at https://red.ht/registration
Last login: Mon Nov 17 15:08:11 2025 from 192.168.8.36
</pre>

## Lab - Providing root access to the user you created in RHEL

You need to login as administrator in one of the terminal ( may elivate palmeto user to root )
```
sudo su -
useradd -m nitesh
cat /etc/sudoers
```

Edit the file and add the below entry in the /etc/sudoers file
<pre>
# Allow root to run any commands anywhere 
root	ALL=(ALL) 	ALL
nitesh  ALL=(ALL) 	ALL  
</pre>

## Lab - Giving root access to an user only to specific commands
As an admin, edit the /etc/sudoers file and update it as shown below
```
## Allow root to run any commands anywhere 
root	ALL=(ALL) 	ALL
nitesh  ALL=(ALL)       NOPASSWD: /bin/systemctl restart sshd	
nitesh  ALL=(ALL)       NOPASSWD: /bin/systemctl start sshd	
nitesh  ALL=(ALL)       NOPASSWD: /bin/systemctl stop sshd	
nitesh  ALL=(ALL)       NOPASSWD: /bin/systemctl status sshd
```

Now, the nitesh user can manage ssh server commands without password
```
sudo systemctl restart sshd
sudo systemctl stop sshd
sudo systemctl start sshd
sudo systemctl status sshd
```

However, the nitesh user is not allowed to
```
sudo su -
```

## Lab - Delete an user including the user's home directory recursively
```
sudo userdel -r nitesh
```

## Lab - Let's create a user group
```
sudo groupadd devops
```

List all groups
```
getent group
cat /etc/group
```

Now, let's create an user and add the user to devops group
```
sudo useradd -m -G devops nitesh
sudo passwd nitesh

# To check in which usergroups a particular user is participating
```
id nitesh
```

# To delete the group, the users that are part of this group will not deleted ( Hence, we need delete users with separte userdel command as they may be part of many other groups )
```
sudo groupdel devops
```

## Info - Linux links
<pre>
- they are like shortcuts to files
- there are 2 types
  - soft ( aka symbolic - shortcuts) and
  - hard links ( additional names for a file )
</pre>

## Lab - Let's create a soft link

whoami
cd ~
ln -s /usr/bin/ls ls
ls -l
```

## Lab - Changing permission of a file/folder

<pre>
cd ~
touch file1.txt
ls -l

#Let's give read/write/execute for everyone
chmod 777 file1.txt
ls -l

#Let's give read/write permission only to the owner of the file
chmod 600 file1.txt
ls -l

#Let's give execute permission only to user
chmod u+x file1.txt
ls -l

#Let's give execute permission for everyone
chmod +x file1.txt
ls -l

#Remove execute permission for everyone
chmod -x file1.txt
ls -l
</pre>

## Lab - Configuring default file and folder permissions on the user level

```
umask 022
```
<pre>
The above command removes no permissions for the owner of the file/folder.
The above command removes write permission for the other users in same group
The above command removes write permission for the other users in other groups
</pre>

Let's verify how it works
```
cd ~

touch file1.txt
# Observe the output of ls command to notice write permission is removed for other users in same and other groups
# while the owner has read and write access
ls -l
```

Try the below 
```
umask 077
touch file5.txt
mkdir dir5.txt
```

## Lab - Listing and killing process in Linux
```
ps
ps aux
ps -ef

# Launch firefox manually in RHEL machine
ps -ef |grep -i firefox
kill -9 3461 
```

## Lab - Finding process that utilizes more more cpu/memory
```
top
```
To quit the top, press 'q'

To find top 5 applications that uses more RAM(memory)
```
ps aux --sort=-%mem | head -n 5
```

Find the 5 applications that consumes least RAM(memory)
```
ps aux --sort=-%mem | tail -n 5
```

Find top 10 applications whose CPU utilization is on the higher side
```
ps aux --sort=-%cpu | head
ps aux --sort=-%cpu | head -n 10
```
## Lab - Find all the child processes of a given parent process id 
```
# Launch firefox
ps -ef | grep -i firefox

# List all child processes 
```
ps --ppid 4139
```

## Lab - Check the currently active performance profile applied on your RHEL machine
```
tuned-adm active
```

Change the profile to high performance
```
tuned-adm profile throughput-performance
tuned-adm active
```

Put your machine in powersave mode
```
tuned-adm profile powersave
tuned-adm active
```

## Lab - Starting a application with highest cpu priority ( Default - 0, -20 is hightest and 19 lowest priority )

Assumption is the application is not running already, so you wish start the applicaiton with highest priority

```
nice -n -20 calculator
```

## Lab - Changing the priority of an already running application

```
renice -5 -p 5342
```

## Lab - Set the cpu affinity ( controlling your application runs on which cpu cores )

```
taskset -c 0 ls
```

Changing the cpu affinity of an already running application
```
taskset -cp 2,3 your-process-id
```

## Lab - Running an application/command every minute

The first command, you need to type in your terminal.  This will open vim editor, type the ** command within vim and save it.
<pre>
crontab -e
* * * * * echo "Timer triggered" >> ~/out.log
</pre>

You can check the output of the echo getting appended every one minute in the out.log at your home directory.
```
cat ~/out.log
```

Once you have completed this exercise, make sure the cronjob is removed
```
# Delete the line you added error when the below command open the vim and save it
```
crontab -e
```

## Lab - Schedule an one time job
```
at 14:10
at> echo "One time job got triggered" > ~/out.log
at> Ctrl + D
```

List all the scheduled jobs
```
atq
```

When the timer expires, check the log
```
cat ~/out.log
```

Expected output
<pre>
jegan@localhost:~$ at 14:06
warning: commands will be executed using /bin/sh
at Tue Nov 18 14:06:00 2025
at> echo "One time task got triggered" > ~/out.log
at> <EOT>
job 1 at Tue Nov 18 14:06:00 2025

jegan@localhost:~$ atq
1	Tue Nov 18 14:06:00 2025 a jegan

jegan@localhost:~$ cat ~/out.log 
One time task got triggered
</pre>


## Lab - Installing Podman container engine in RHEL 9

```
sudo dnf update -y
sudo dnf install -y podman
podman --version
podman images
```

<img width="1280" height="768" alt="image" src="https://github.com/user-attachments/assets/700a90de-f465-43a3-a147-3fc9de9dbc1c" />
<img width="1280" height="768" alt="image" src="https://github.com/user-attachments/assets/388bcef4-9b04-40c3-b99d-d2b23c9b33a0" />
<img width="1280" height="768" alt="image" src="https://github.com/user-attachments/assets/2b0bb566-bdd2-40a7-9344-620ca1719096" />
<img width="1280" height="768" alt="image" src="https://github.com/user-attachments/assets/0587f80f-f2de-45c4-9fef-1bc4736bc95d" />


## Info - Container Runtime Overview
<pre>
- is a low-level software that helps us manage containers and images
- container image is blueprint of a container
- container is an instance of a container image
- with a container image, one can create multiple containers
- each container image is given an unique name and ID
- each container is given a unique name and ID
- container runtime is not user-friendly, hence normally end-users will not use this directly
- examples
  - runC
  - cRun
  - CRI-O
</pre>

## Info - Container Engine Overview
<pre>
- Container engine is a high-level software that is also user-friendly
- helps us manage containers and images
- internally, container engines depends on container runtimes
- examples
  - Docker internally depends on containerd which in turn depends on runC container runtime
  - Podman - internally depends on cRun/CRI-O container runtime
</pre>

## Lab - Download nginx image from Docker Hub
Select the docker.io ( Docker Hub while downloading the image )

```
podman images
podman pull bitnami/nginx:latest
podman images
```

## Lab - Create a container using Podman
```
podman run -d --name nginx --hostname nginx bitnami/nginx:latest
podman ps
```
<img width="1280" height="768" alt="image" src="https://github.com/user-attachments/assets/d94b438e-254f-47a0-b60d-d98d36cf9698" />


## Lab - Creating multiple nginx web server containers
```
podman run -d --name nginx2 --hostname nginx2 bitnami/nginx:latest
podman run -d --name nginx3 --hostname nginx3 bitnami/nginx:latest
```

Let's list all running containers
```
podman ps
```

Let's rename the nginx container to nginx1
```
podman rename nginx nginx1
podman ps
```

<img width="1280" height="768" alt="image" src="https://github.com/user-attachments/assets/3a5ec038-29bb-4f98-81d6-1868a0f35d39" />
