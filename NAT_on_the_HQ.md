# Configuration NAT on HQ

### Configure
Configure port security on access ports:
```shell
vrf definition CORP
 rd 100:100
 !
 address-family ipv4
 exit-address-family
```
DHCP snooping & DAI
```shell
track 10 interface GigabitEthernet0/1 line-protocol

ip route vrf CORP 0.0.0.0 0.0.0.0 100.64.10.5 global track 10
ip route vrf CORP 0.0.0.0 0.0.0.0 100.64.10.1 global 10
```
```shell
ip access-list extended NAT-CORP
 deny   ip host 10.10.0.1 any
 deny   ip host 10.10.0.2 any
 permit ip 10.10.10.0 0.0.0.255 any
 permit ip 10.10.20.0 0.0.0.255 any
 permit ip 10.10.30.0 0.0.0.255 any
```
Configure trust ports between another network devices
```shell
route-map NAT-WAN1 permit 10
 match ip address NAT-CORP
 match interface Gi0/0

route-map NAT-WAN2 permit 10
 match ip address NAT-CORP
 match interface Gi0/1
```
Configure limit to PC ports
```shell
ip nat inside source route-map NAT-PE1 interface GigabitEthernet0/0 vrf CORP overload
ip nat inside source route-map NAT-PE2 interface GigabitEthernet0/1 vrf CORP overload
```
## Config is done
