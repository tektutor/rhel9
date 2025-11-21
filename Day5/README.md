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

#In rhelvm1 terminal
echo "Nginx works in rhelvm1" > /usr/share/nginx/html/index.html

#In rhelvm2 terminal
echo "Nginx works in rhelvm2" > /usr/share/nginx/html/index.html

#In rhelvm3 terminal
echo "Nginx works in rhelvm3" > /usr/share/nginx/html/index.html

pcs host auth rhelvm1.tektutor.org rhelvm2.tektutor.org rhelvm3.tektutor.org -u hacluster

#sudo pcs cluster stop --all
#sudo systemctl stop pacemaker
#sudo systemctl stop corosync

pcs cluster setup --force mycluster rhelvm1.tektutor.org rhelvm2.tektutor.org rhelvm3.tektutor.org

pcs cluster start --all
pcs cluster enable --all
pcs status

#For lab purpose
pcs property set stonith-enabled=false
pcs property set no-quorum-policy=ignore
# Step 1: Disable STONITH and quorum rules
pcs property set stonith-enabled=false
pcs property set no-quorum-policy=ignore

# Step 2: Create VIP and Nginx resources
pcs resource create vip ocf:heartbeat:IPaddr2 ip=192.168.122.250 cidr_netmask=24 op monitor interval=30s
pcs resource create nginx systemd:nginx op monitor interval=30s
pcs resource group add web-group vip nginx

# Step 3: Check cluster status
pcs status
curl http://192.168.122.250

# Step 4: Test failover
pcs cluster stop rhelvm1.tektutor.org

pcs status
curl http://192.168.122.250
pcs cluster start rhelvm1.tektutor.org
pcs status
curl http://192.168.122.250

#Production-grade
#pcs stonith create fence-rhelvm1 fence_virsh pcmk_host_list="rhelvm1.tektutor.org" ipaddr=192.168.122.214 login=root passwd='RedHatRootPassword'
#pcs stonith create fence-rhelvm2 fence_virsh pcmk_host_list="rhelvm2.tektutor.org" ipaddr=192.168.122.174 login=root passwd='RedHatRootPassword'
#pcs stonith create fence-rhelvm3 fence_virsh pcmk_host_list="rhelvm3.tektutor.org" ipaddr=192.168.122.180 login=root passwd='RedHatRootPassword'
#pcs property set stonith-enabled=true
#pcs property set no-quorum-policy=freeze
