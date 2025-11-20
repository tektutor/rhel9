# Day 4

## Info - Subnet
<pre>
- logical subdivision of your actual flat network
- helps in applying different security policy 
- 192.168.1.0/24 (IPV4)
- 32 bits ( 4 byte )
- 4 Octets
- 24 (CIDR)
- Starting IP - 192.168.1.0
- Ending IP - 192.168.1.255
- the above is a range of IP Address
- another subnet example 192.168.0.0/16
- how many IP addresses are there in the above  ( 256 x 256 = 65535 IP addresses in the above subnet )
- Starting IP 
</pre>

## Lab - Manually modify the network configuration in vm1 ( assign static IP manually )
From the RHEL9 base machine run the below
```
sudo su -
virsh net-list --all
virsh net-dumpxml default
ls -l /etc/libvirt/qemu/networks
```

Ensure you have already logged in to your vm1 using virsh
```
virsh console vm1
```

Or you can login using ssh
```
ssh root@192.168.122.62
```

See all network interfaces
```
nmcli connection show
nmcli con show
```

See active connections
```
nmcli connection show --active
```

Show all network interfaces
```
nmcli device
```

Configure static IP by modify the existing network interface
```
nmcli con mod enp1s0 ipv4.addresses "192.168.122.50/24"
nmcli con mod enp1s0 ipv4.gateway "192.168.122.1"
nmcli con mod enp1s0 ipv4.method manual
nmcli con mod enp1s0 ipv4.dns "8.8.8.8 4.4.4.4 1.1.1.1"

ifconfig

nmcli con down enp1s0
nmcli con up enp1s0

nmcli device show
```

## Lab - Rename network interface name
note
<pre>
- nmcli doesn't support renaming interface name, hence we need to use a different approach  
- create a directory name network if it is missing under /etc/systemd
- create a file name /etc/systemd/network/10-rename.link
  [Match] 
  MACAddress=52:54:00:61:82:a7 
  [Link] Name=eth0
- restart - systemctl restart systemd-udevd
- restart machine - reboot
</pre>

You may follow the below instructions
```
sudo su -
cd /etc/systemd
mkdir network
touch 10-rename.link
```

Edit 10-rename.link with vim editor and add the below lines
<pre>
[Match] 
MACAddress=52:54:00:61:82:a7 
[Link] 
Name=eth0
</pre>

## Lab - Deleting KVM vm1 and vm2 and recreating it
```
virsh destroy vm1
virsh destroy vm2
virsh undefine vm1 --remove-all-storage
virsh undefine vm2 --remove-all-storage
```

Create disks for vm1 and vm2 using kvm
```
sudo qemu-img create -f qcow2 /var/lib/libvirt/images/rhel1.qcow2 50G
sudo qemu-img create -f qcow2 /var/lib/libvirt/images/rhel2.qcow2 50G
```

Let's create the vm1, when it prompts text mode or vnc mode while install select vnc
```
sudo virt-install   --name vm1   --ram 8192   --vcpus 2   --cpu host-model   --disk path=/var/lib/libvirt/images/rhel1.qcow2,format=qcow2   --location /var/lib/libvirt/images/rhel-9.0-x86_64-dvd.iso   --os-variant rhel9.0   --graphics none   --extra-args "console=ttyS0,115200n8"
```

You can then open remina and paste the 192.168.122.110:1 or whatever the installer shows in the terminal to proceed with gui mode of rhel9 installation

Let's create the vm2, when it prompts text mode or vnc mode while install select vnc
```
sudo virt-install   --name vm2   --ram 8192   --vcpus 2   --cpu host-model   --disk path=/var/lib/libvirt/images/rhel2.qcow2,format=qcow2   --location /var/lib/libvirt/images/rhel-9.0-x86_64-dvd.iso   --os-variant rhel9.0   --graphics none   --extra-args "console=ttyS0,115200n8"
```

You can then open remina and paste the 192.168.122.110:1 or whatever the installer shows in the terminal to proceed with gui mode of rhel9 installation


## Lab - Changing hostname of vm1 and vm2
Login to vm1
```
sudo su -
hostnamectl set-hostname vm1
hostname
```

Login to vm2
```
nmcli general hotname vm2
hostname
```

## Lab - Login to vm1 and add a new network connection
```
sudo su -
nmcli con add type ethernet ifname eth0 con-name eth0 ipv4.method manual \
ipv4.addresses 192.168.122.2/24 \
ipv4.gateway 192.168.122.1 \
ipv4.dns "8.8.8.8 4.4.4.4"

ifconfig
```
