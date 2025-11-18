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

## Lab - Starting vm1 and vm2 in your cloud lab machine
KVM virtualization server commands
```
sudo virsh list --all
sudo virsh start vm1
sudo virsh start vm1
sudo virsh console vm1
```
When vm1 prompts for user, type root as user and root as password.

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

Check the current default target
```
systemctl get-default
```

Change default boot target ( switches to gui permanently )
```
sudo systemctl set-default graphical.target
```

Switch to command-line
```
sudo systemctl set-default multi-user.target
```

List all processes started by systemd
```
systemctl status $(pidof sshd)
systemctl status process-id 
```

## Lab - Control System services
```
sudo systemctl enable sshd
sudo systemctl start sshd
sudo systemctl status sshd
sudo systemctl stop sshd
sudo systemctl restart sshd
sudo systemctl disable sshd
```
