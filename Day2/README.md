# Day 2

## Info - initd vs systemd
<pre>
- initd
  - all legacy unix/linux os uses this boot sequence
  - it makes use of many scripts to help the OS boot
  - all the operations will happen in sequence, this leads longer boot time
  - this is still used in
    - alpine
    - slackware
  - service are managed with below commands
    service ssh start
    service ssh status
    service ssh restart
- systemd
  - all modern linux OS supports systemd boot sequence
  - unlike the initd, systemd supports running many boot steps in parallel, this cuts down the booting time
  - hence, this is preferred pretty much in all modern Linux OS
  - Ubuntu
  - Fedora
  - RHEL
  - Suse
  - services are managed with below commands
    systemctl enable sshd
    systemctl disable sshd
    systemctl start sshd
    systemctl stop sshd
    systemctl restart sshd
    systemctl status sshd
</pre>

## Lab - list all auto-started systemd services

```
systemctl list-units --type=service
```

Show all services that are enabled at the boot time
```
systemctl list-unit-files --type=service | grep enabled
```

Show services currently running
```
systemctl --type=service --state=running
```

Check the systemd targets 
```
systemctl list-dependencies multi-user.target
systemctl list-dependencies basic.target
systemctl list-dependencies graphical.target
systemctl list-dependencies rescue.target
systemctl list-dependencies emergency.target
systemctl list-dependencies shutdown.target
systemctl list-dependencies reboot.target

```

List all processes started by systemd
```
systemctl status $(pidof sshd)
systemctl status process-id 
```
