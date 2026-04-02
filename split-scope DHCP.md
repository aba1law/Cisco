# Configuration split-scope DHCP

## Preparation

- Configure full L2 network structure and ip addressing

### Configure
DSW1 config:
```shell
conf t
ip dhcp excluded-address 10.10.10.1 10.10.10.49
ip dhcp excluded-address 10.10.10.150 10.10.10.254

ip dhcp pool VLAN10
 network 10.10.10.0 255.255.255.0
 default-router 10.10.10.254
 dns-server 8.8.8.8 1.1.1.1
end
wr
```
DSW2 config:
```shell
	conf t
ip dhcp excluded-address 10.10.10.1 10.10.10.149
ip dhcp excluded-address 10.10.10.250 10.10.10.254

ip dhcp pool VLAN10
 network 10.10.10.0 255.255.255.0
 default-router 10.10.10.254
 dns-server 8.8.8.8 1.1.1.1
end
wr
```
Check
```shell
show ip dhcp binding
```
## Config is done
