# Configuration NAT for BR-EDGE

## Preparation

- Configure L2 network infrastructure firstly

### Configure
Configuration of interface in VRF & WAN interface. Access-list and route for VRF CORP.
```shell
interface Gi0/1
 ip vrf forwarding CORP
 ip address 10.10.50.254 255.255.255.0
 ip nat inside
 no shut

 interface Gi0/0
 ip nat outside
 no shut

 ip access-list extended NAT
 deny ip host 10.10.0.2 any
 permit ip 10.10.50.0 0.0.0.255 any

 ip route vrf CORP 0.0.0.0 0.0.0.0 100.64.20.1 global

 ip nat inside source list NAT interface Gi0/0 vrf CORP overload
```
## Config is done

