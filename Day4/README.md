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
</pre>

## Lab - Manually modify the network configure in vm1 ( assign static IP manually )
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
nmcli con mod enp1s0 ipv4.addresses "192.168.1.50/24"
nmcli con mod enp1s0 ipv4.gateway "192.168.1.1"
nmcli con mod enp1s0 ipv4.metho manual
nmcli con mod enp1s0 ipv4.dns "8.8.8.8 4.4.4.4 1.1.1.1"

ifconfig

nmcli con down enp1s0
nmcli con up enp1s0

ifconfig
```
