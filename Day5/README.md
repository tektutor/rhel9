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

sudo chsh -s /bin/bash hacluster
getent passwd hacluster


sudo pcs host auth rhelvm1.tektutor.org rhelvm2.tektutor.org rhelvm3.tektutor.org -u hacluster

pcs cluster setup --name mycluster rhelvm1 rhelvm2 rhelvm3
pcs cluster start --all
pcs cluster enable --all
pcs resource create vip ocf:heartbeat:IPaddr2 ip=192.168.122.250 cidr_netmask=24 op monitor interval=30s
pcs resource create nginx systemd:nginx op monitor interval=30s
pcs resource group add web-group vip nginx
pcs status

# Test failover
pcs resource disable web-group
pcs status
pcs resource enable web-group
pcs status
pcs resource meta web-group resource-stickiness=100
pcs resource meta web-group migration-threshold=3
pcs property list
pcs stonith show
curl http://192.168.122.250

```
