# Day 3
## Info - Provisioning tools
<pre>
- helps in automating OS installtion, provisioning machine locally or on cloud on as VMs
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
dnf install -y ansible
```
