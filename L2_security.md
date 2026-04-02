# Configuration Port-security, dhcp snooping, dai.

## Preparation

- Configure L2 network infrastructure firstly

### Configure
Configure port security on access ports:
```shell
interface gi0/x
 switchport mode access
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
 spanning-tree portfast
exit
```
DHCP snooping & DAI
```shell
conf t
ip dhcp snooping
ip dhcp snooping vlan 10
```
```shell
ip arp inspection vlan 10
```
Configure trust ports between another network devices
```shell
interface range gi0/1 - 3
 ip dhcp snooping trust
 ip arp inspection trust
```
Configure limit to PC ports
```shell
interface gi0/3
 ip dhcp snooping limit rate 10
 ip arp inspection limit rate 15
```
Check
```shell
show ip dhcp snooping
show ip dhcp snooping binding
show ip arp inspection
show ip arp inspection vlan 10
```
## Config is done
