# Configuration iBGP & eBGP

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
On ISP PE2:
```shell
ip route 10.10.0.1 255.255.255.255 100.64.10.6
ip route 10.10.0.2 255.255.255.255 100.64.20.2

router bgp 65000
 bgp log-neighbor-changes
 neighbor 10.255.0.1 remote-as 65000
 neighbor 10.255.0.1 password BGPmd5
 neighbor 10.255.0.1 update-source Loopback0

 neighbor 10.10.0.2 remote-as 65100
 neighbor 10.10.0.2 ebgp-multihop 2
 neighbor 10.10.0.2 password EBGPmd5
 neighbor 10.10.0.2 update-source Loopback0
 
 address-family ipv4
  neighbor 10.10.0.2 activate
  neighbor 10.10.0.2 default-originate
  neighbor 10.10.0.2 as-override
  neighbor 10.10.0.2 route-map ONLY-DEFAULT out
  neighbor 10.255.0.1 activate
  neighbor 10.255.0.1 next-hop-self
 exit-address-family
```
On ISP-PE1:
```shell
ip route 10.10.0.1 255.255.255.255 100.64.10.2

router bgp 65000
 bgp log-neighbor-changes
 neighbor 10.255.0.1 remote-as 65000
 neighbor 10.255.0.1 password BGPmd5
 neighbor 10.255.0.1 update-source Loopback0

 neighbor 10.10.0.1 remote-as 65100
 neighbor 10.10.0.1 ebgp-multihop 2
 neighbor 10.10.0.1 password EBGPmd5
 neighbor 10.10.0.1 update-source Loopback0
 
 address-family ipv4
  neighbor 10.10.0.1 activate
  neighbor 10.10.0.1 default-originate
  neighbor 10.10.0.1 as-override
  neighbor 10.10.0.1 route-map HQ-ONLY-DEFAULT out
  neighbor 10.255.0.1 activate
  neighbor 10.255.0.1 next-hop-self
 exit-address-family
```
On HQ-EDGE (eBGP Client)
```shell
ip route 10.255.0.11 255.255.255.255 100.64.10.1

router bgp 65100
 bgp log-neighbor-changes
 neighbor 10.255.0.11 remote-as 65000
 neighbor 10.255.0.11 ebgp-multihop 2
 neighbor 10.255.0.11 password EBGPmd5
 neighbor 10.255.0.11 update-source Loopback0
 
 address-family ipv4
  network 10.10.0.1 mask 255.255.255.255
  network 100.64.10.0 mask 255.255.255.252
  network 100.64.10.4 mask 255.255.255.252
  neighbor 10.255.0.11 activate
 exit-address-family
```
On BR-EDGE (eBGP Client)
```shell
ip route 10.255.0.12 255.255.255.255 100.64.20.1

router bgp 65100
 bgp log-neighbor-changes
 neighbor 10.255.0.12 remote-as 65000
 neighbor 10.255.0.12 ebgp-multihop 2
 neighbor 10.255.0.12 password EBGPmd5
 neighbor 10.255.0.12 update-source Loopback0

 address-family ipv4
  network 10.10.0.2 mask 255.255.255.255
  network 100.64.20.0 mask 255.255.255.252
  neighbor 10.255.0.12 activate
 exit-address-family
```
Check
```shell
show ip bgp summ
show ip route
clear ip bgp *
```
## Config is done

