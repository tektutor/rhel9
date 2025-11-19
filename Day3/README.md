# Day 3
## Info - Provisioning tools
<pre>
- helps in automating OS installation, provisioning machine locally or on cloud on as VMs
- examples
  - AWS Cloudformation
  - Terraform
</pre>
  

## Info - Configuration Management Tool
<pre>
- it helps your automate system administrative activities
- the assumption is you already have a machine with some OS pre-installed, on that machine you wish to automate software installation, user management, network management 
- examples
  - Puppet 
    - DSL(the language in which the automation is written) is Puppet Proprietrary language
    - installation is difficult and time consuming
    - learning curve is steep
    - requires installing Puppet agent on the server that must be managed by Puppet
  - Chef
    - DSL used is Ruby
    - learn Chef specific features
    - installation is difficult and time consuming
    - as it has 30+ tools, learning curve is steep and mastering take couple of months of years
    - Chef agent must installed on the servers that must be managed by Chef
  - Salt/Saltstack ( deprecated - not used )
  - Ansible
    - follows simple architecture
    - DSL used is YAML ( easy to learn )
    - easy to install and learn and master
    - agentless
    - machines managed by Ansible are called Ansible Nodes
    - Ansible Nodes can be
      - a Windows Server
      - an Unix Server
      - mac machine
      - Linux machine (Physical/Virtual/Cloud)
      - containers
      - Network switches/routers, etc.,
    - comes in 2 flavours
      - Ansible core ( open source ) - supports only command line
      - AWX - opensource and supports webconsole ( built on top of Ansible Core )
      - Ansible Automation Platform (AAP - aka earlier as Ansible Tower )
        - is a enterprise tool from Red Hat
        - requires license to use
        - built on top of opensource AWX
        - supports Webconsole
        - worldwide support is given by Red Hat ( an IBM company )
    - the machine where Ansible is installed is called Ansible Controller Machine ( ACM )
</pre>

## Lab - Installing Ansible core on your RHEL 9 cloud machine 
```
sudo su -
dnf update -y
dnf install -y ansible
ansible --version
```

## Lab - Cloning TekTutor's Training Repository
```
cd ~
git clone https://github.com/tektutor/rhel9.git
cd rhel9
git pull
```

## Lab - Running ansible ad-hoc command to ping and check if ACM can ping vm1 and vm2

Login to your vm1 with credentials i.e username - root and password - root 
```
virsh console vm1
ifconfig
mkdir /root/.ssh
#Create a file named authorized_keys under the folder /root/.ssh/authorized_keys
#and paste the id_ed25519.pub file content kept at folder /home/palmeto/.ssh/id_ed25519.pub
```

Login to your vm2 with credentials i.e username - root and password - root
```
virsh console vm2 
ifconfig
mkdir /root/.ssh
#Create a file named authorized_keys under the folder /root/.ssh/authorized_keys
#and paste the id_ed25519.pub file content kept at folder /home/palmeto/.ssh/id_ed25519.pub
```

If the public keys of palmeto user is exported in the authorized_keys file on vm1 and vm2, 
then the below ssh connections will allow you to login to vm1 and vm2 without password
```
ssh root@192.168.122.62
exit

ssh root@192.168.122.147
exit
```

Now you can run the ansible ad-hoc command to check if ansible can communicate with vm1 and vm2 via SSH connection
```
cd ~/rhel9/ansible/
ansible -i inventory all -m ping
```

Expected output

<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/d132cb10-b57f-4cad-99e2-d497f424e150" />


## Info - What happens internally when ansible runs the ad-hoc ping command
<pre>
- ansible creates a tmp folder on ACM and ansible nodes
- the ping.py modules is copied to the ACM tmp folder and includes all the dependent code in the ping.py on ACM temp folder
- ansible copy the ping.py from ACM temp to the ansible node (vm1 and vm2) in a tmp folder
- ansible gives execute permisison to ping.py on vm1 and vm2
- ansible runs the ping.py on vm1 and vm2
- captures the output of ping.py execution on vm1 and vm2
- gives a summary of out on the ACM machine command prompt
</pre>
