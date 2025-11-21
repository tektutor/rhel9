# Day 5

## Lab - Let's create a 3 node production-grade Pacemaker cluster

Create 3 RHEL 9 VMs namely rhelvm1, rhelvm2 and rhelvm3.
Note
<pre>
- use Virtual Machine Manager (GUI) tool to create the VMs
- allocate 2048 GB RAM, CPU - 1 core and  Storage - 20GB.
- I gave root/root as the login credentials for simplicity
- I clone the first VM (rhelvm1) to created rhelvm2 and rhelvm3.
</pre>

Make sure their hostnames are configured as shown below

On rhelvm1
```
hostnamectl set-hostname rhelvm1.tektutor.org
hostname
```

on rhelvm2
```
hostnamectl set-hostname rhelvm2.tektutor.org
hostname
```

on rhelvm3
```
hostnamectl set-hostname rhelvm3.tektutor.org
hostname
```
<img width="1960" height="624" alt="image" src="https://github.com/user-attachments/assets/e5fb4c8c-1d4a-40bc-b2a6-0a6c48595bdf" />


Note down the IP addresses of all 3 vms, in my case it looks as shown below
<pre>
rhelvm1 192.168.122.213
rhelvm2 192.168.122.174
rhelvm3 192.168.122.127
</pre>

We need to add the above IPs in all vm's /etc/hosts file as shown below
<pre>
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

192.168.122.214 rhelvm1.tektutor.org rhelvm1
192.168.122.174 rhelvm2.tektutor.org rhelvm2
192.168.122.180 rhelvm3.tektutor.org rhelvm3  
</pre>

To split the window, you may use tmux utility on the command line
<pre>
- you can install tmux with command sudo dnf install -y tmux
- you need to type tmux in the terminal, a green bar will appear which is an indication that tmux is in action
- you can split the window horizontally using the Ctrl+B "
- you can split the window vertically using Ctrl+B %
- you can move between windows using Ctrl+B Up, Down, Left and Right arrows
- to synchronize all the windows, within tmux Ctrl+B :setw synchronize-panes on to switch Ctrl+B :setw synchronize-panes off
</pre>
<img width="1960" height="624" alt="image" src="https://github.com/user-attachments/assets/7e1914c1-134d-44f2-b728-17b73cd3ed06" />

Install the below tools on all 3 vms(servers)
```
sudo subscription-manager register --auto-attach
sudo subscription-manager repos --enable=rhel-9-for-x86_64-highavailability-rpms
sudo dnf repolist

sudo dnf install -y pcs pacemaker corosync fence-agents-all nginx
sudo systemctl enable --now pcsd
sudo systemctl enable --now nginx
sudo firewall-cmd --permanent --add-service=high-availability
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
```

#In rhelvm1 terminal
echo "Nginx works in rhelvm1" > /usr/share/nginx/html/index.html
passwd hacluster

### In rhelvm2 terminal
```
echo "Nginx works in rhelvm2" > /usr/share/nginx/html/index.html
passwd hacluster
```

### In rhelvm3 terminal
```
echo "Nginx works in rhelvm3" > /usr/share/nginx/html/index.html
passwd hacluster

pcs host auth rhelvm1.tektutor.org rhelvm2.tektutor.org rhelvm3.tektutor.org -u hacluster

#sudo pcs cluster stop --all
#sudo systemctl stop pacemaker
#sudo systemctl stop corosync

pcs cluster setup --force mycluster rhelvm1.tektutor.org rhelvm2.tektutor.org rhelvm3.tektutor.org

pcs cluster start --all
pcs cluster enable --all
pcs status
```

### For learning purpose
```
pcs property set stonith-enabled=false
pcs property set no-quorum-policy=ignore
```
### Step 1: Disable STONITH and quorum rules
```
pcs property set stonith-enabled=false
pcs property set no-quorum-policy=ignore
```

### Step 2: Create VIP and Nginx resources
```
pcs resource create vip ocf:heartbeat:IPaddr2 ip=192.168.122.250 cidr_netmask=24 op monitor interval=30s
pcs resource create nginx systemd:nginx op monitor interval=30s
pcs resource group add web-group vip nginx
```

### Step 3: Check cluster status
```
pcs status
curl http://192.168.122.250
```

### Step 4: Test failover
```
pcs cluster stop rhelvm1.tektutor.org

pcs status
curl http://192.168.122.250
pcs cluster start rhelvm1.tektutor.org
pcs status
curl http://192.168.122.250
```
### Production-grade
```
#pcs stonith create fence-rhelvm1 fence_virsh pcmk_host_list="rhelvm1.tektutor.org" ipaddr=192.168.122.214 login=root passwd='RedHatRootPassword'
#pcs stonith create fence-rhelvm2 fence_virsh pcmk_host_list="rhelvm2.tektutor.org" ipaddr=192.168.122.174 login=root passwd='RedHatRootPassword'
#pcs stonith create fence-rhelvm3 fence_virsh pcmk_host_list="rhelvm3.tektutor.org" ipaddr=192.168.122.180 login=root passwd='RedHatRootPassword'
#pcs property set stonith-enabled=true
#pcs property set no-quorum-policy=freeze
```

## Lab - Network bonding on a VM with two network interfaces for load-balancing(HA internet)

Notes
<pre>
- On the host, we need to create Linux bridges (br0, br1) to forward traffic from physical NICs to virtual NICs
- These bridges act like a virtual switch for the VM
- VM bonding
  - Inside the VM, we need to have two virtual NICs (connected to br0 and br1).
  - We need to configure a bond in the VM (e.g., bond0) so that the VM itself sees two NICs as one logical interface
- This allows redundancy or aggregated bandwidth inside the VM
- Create a VM using VMM GUI with 2GB RAM, Dual Core and 20GB Disk
- Shutdown the VM
</pre>

Add two virtual NICs
```
virsh attach-interface rhelvm --type network --source default --model virtio --config
virsh attach-interface rhelvm --type network --source default --model virtio --config
```

Start the VM
```
virsh start rhelvm
```

Inside the VM, run this, you are supposed to see two NICs
```
nmcli device status
```

Install bonding tools inside VM
```
sudo dnf install -y iputils iproute bridge-utils
```

Enable bonding kernel module
```
echo "bonding" | sudo tee /etc/modules-load.d/bonding.conf
sudo modprobe bonding
lsmod | grep bonding
```

Create a bond
```
sudo nmcli connection add type bond con-name bond0 ifname bond0 mode active-backup
```

Add NICs as slaves
```
sudo nmcli connection add type bond-slave con-name bond0-slave1 ifname enp1s0 master bond0
sudo nmcli connection add type bond-slave con-name bond0-slave2 ifname enp2s0 master bond0
```

Assign IP address to bond0
```
sudo nmcli con mod bond0 ipv4.addresses "192.168.122.50/24"
sudo nmcli con mod bond0 ipv4.gateway "192.168.122.1"
sudo nmcli con mod bond0 ipv4.dns "8.8.8.8"
sudo nmcli con mod bond0 ipv4.method manual
```

Bring up the bond and slaves
```
sudo nmcli con up bond0
sudo nmcli con up bond0-slave1
sudo nmcli con up bond0-slave2
cat /proc/net/bonding/bond0
#Observe enp1s0 is active now
ip addr show bond0

# Test failover
sudo nmcli device disconnect enp1s0
sudo ip link set enp1s0 down
# Observe enp2s0 will be active now, the bond has failed over to the other NIC
cat /proc/net/bonding/bond0
```

## Lab - Bind DNS Setup and configuration

Note
<pre>
- In case, you already have some vms, delete it from the lab machine
- For example
  virsh destroy vm1
  virsh destroy vm2
  virsh undefine vm1 --remove-all-storage
  virsh undefine vm2 --remove-all-storage
</pre>

Let's create two VMs using kvm named vm1 and vm2.

Let's start with creating disks for vm1 and vm2
```
sudo qemu-img create -f qcow2 /var/lib/libvirt/images/vm1.qcow2 20G
sudo qemu-img create -f qcow2 /var/lib/libvirt/images/vm2.qcow2 20G
```

Let's create the vm1 using kvm
```
sudo virt-install   --name vm1   --ram 8192   --vcpus 2   --cpu host-model   --disk path=/var/lib/libvirt/images/rhel1.qcow2,format=qcow2
--location /var/lib/libvirt/images/rhel-9.0-x86_64-dvd.iso   --os-variant rhel9.0   --graphics none   --extra-args "console=ttyS0,115200n8"
```

Let's create the vm2 using kvm
```
sudo virt-install   --name vm2   --ram 8192   --vcpus 2   --cpu host-model   --disk path=/var/lib/libvirt/images/rhel2.qcow2,format=qcow2
--location /var/lib/libvirt/images/rhel-9.0-x86_64-dvd.iso   --os-variant rhel9.0   --graphics none   --extra-args "console=ttyS0,115200n8"
```

