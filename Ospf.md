# Configuration OSPF on CORE1/CORE2/PE1/PE2 Area 0.

## Preparation

- Just configure ip addresses on routers

### Configure
ISP-CORE1 config:
```shell
router ospf 10
 router-id 10.255.0.1
 passive-interface Loopback0
 network 10.255.0.1 0.0.0.0 area 0
 network 10.255.10.0 0.0.0.3 area 0
 network 10.255.10.4 0.0.0.3 area 0
 network 10.255.10.8 0.0.0.3 area 0
```
ISP-CORE2 config:
```shell
router ospf 10
 router-id 10.255.0.2
 passive-interface Loopback0
 network 10.255.0.2 0.0.0.0 area 0
 network 10.255.10.0 0.0.0.3 area 0
 network 10.255.10.12 0.0.0.3 area 0
 network 10.255.10.16 0.0.0.3 area 0
```
ISP-PE1 config:
```shell
router ospf 10
 router-id 10.255.0.11
 passive-interface Loopback0
 network 10.255.0.11 0.0.0.0 area 0
 network 10.255.10.4 0.0.0.3 area 0
 network 10.255.10.12 0.0.0.3 area 0
```
ISP-PE2 config:
```shell
router ospf 10
 router-id 10.255.0.12
 passive-interface Loopback0
 network 10.255.0.12 0.0.0.0 area 0
 network 10.255.10.8 0.0.0.3 area 0
 network 10.255.10.16 0.0.0.3 area 0
```
Test:
```shell
show ip ospf neighbor
show ip route ospf
```
## Config is done
