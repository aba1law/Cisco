# Configuration NAT on HQ

### Configure
Create VRF
```shell
vrf definition CORP
 rd 100:100
 !
 address-family ipv4
 exit-address-family
```
Create router on VRF
```shell
track 10 interface GigabitEthernet0/0 line-protocol

ip route vrf CORP 0.0.0.0 0.0.0.0 100.64.10.1 global track 10
ip route vrf CORP 0.0.0.0 0.0.0.0 100.64.10.5 global 10
```
Access-list
```shell
ip access-list extended NAT-CORP
 deny   ip host 10.10.0.1 any
 permit ip 10.10.10.0 0.0.0.255 any
 permit ip 10.10.20.0 0.0.0.255 any
 permit ip 10.10.30.0 0.0.0.255 any
```
Configure route-maps
```shell
route-map NAT-WAN1 permit 10
 match ip address NAT-CORP
 match interface Gi0/0

route-map NAT-WAN2 permit 10
 match ip address NAT-CORP
 match interface Gi0/1
```
NAT
```shell
ip nat inside source route-map NAT-WAN1 interface GigabitEthernet0/0 vrf CORP overload
ip nat inside source route-map NAT-WAN2 interface GigabitEthernet0/1 vrf CORP overload
```
## Config is done
