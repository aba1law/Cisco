# Configuration MSTP

## Preparation

- Create vlans, add trunk and access ports

### Configure
On both switches:
```shell
conf t
spanning-tree mode mst

spanning-tree mst configuration
 revision 1
 instance 1 vlan 10,30
 instance 2 vlan 20
 exit
```
On DSW1:
```shell
conf t
spanning-tree mst 0 root primary
spanning-tree mst 1 root primary
spanning-tree mst 2 root secondary
end
```
On DSW2:
```shell
conf t
spanning-tree mst 0 root secondary
spanning-tree mst 1 root secondary
spanning-tree mst 2 root primary
end
```
Check
```shell
show spanning-tree mst
show spanning-tree mst 1
show spanning-tree mst 2
show spanning-tree mst configuration
```
## Config is done
