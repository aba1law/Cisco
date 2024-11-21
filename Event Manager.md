# Configuration event manager applet (for example: for backup)

## Preparation

- Configure network interfaces and basic configuration
- Have connect to tftp server

### Configure
```shell
event manager applet Backup
```
```shell
event cli pattern "write memory" sync yes
```
```shell
action 1.0 cli command "enable" 
```
```shell
action 2.0 cli command "copy running-config tftp://192.168.1.100/$('%Y-%m-%d_%H-%M-%S').cfg"
```
```shell
action 3.0 cli command "exit"
```
```shell
do write
```

## Config is done
