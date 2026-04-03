# Configuration iBGP

## Preparation

- Configure underlay ospf, ensure pings on loopback interfaces.

### Configure
On route reflector ISP-CORE1:
```shell
ip route 0.0.0.0 0.0.0.0 203.0.113.8
```
```shell
router bgp 65000
 bgp cluster-id 1
 bgp log-neighbor-changes
 network 0.0.0.0
 neighbor 10.255.0.2 remote-as 65000
 neighbor 10.255.0.2 password BGPmd5
 neighbor 10.255.0.2 update-source Loopback0
 neighbor 10.255.0.2 next-hop-self
 neighbor 10.255.0.11 remote-as 65000
 neighbor 10.255.0.11 password BGPmd5
 neighbor 10.255.0.11 update-source Loopback0
 neighbor 10.255.0.11 route-reflector-client
 neighbor 10.255.0.11 next-hop-self
 neighbor 10.255.0.12 remote-as 65000
 neighbor 10.255.0.12 password BGPmd5
 neighbor 10.255.0.12 update-source Loopback0
 neighbor 10.255.0.12 route-reflector-client
 neighbor 10.255.0.12 next-hop-self
```
ISP-CORE2:
```shell
router bgp 65000
 bgp log-neighbor-changes
 neighbor 10.255.0.1 remote-as 65000
 neighbor 10.255.0.1 password BGPmd5
 neighbor 10.255.0.1 update-source Loopback0
```
On ISP-PE1 & ISP PE2:
```shell
router bgp 65000
 bgp log-neighbor-changes
 neighbor 10.255.0.1 remote-as 65000
 neighbor 10.255.0.1 password BGPmd5
 neighbor 10.255.0.1 update-source Loopback0
```
Check
```shell
show ip bgp summ
show ip route
```
## Config is done

