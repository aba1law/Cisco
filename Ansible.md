# Configuration Ansible for SNMP Zabbix

## Preparation

- Edit ssh configs for Cisco Devices
- Install ansible 
- Configure files

### Configure
- For first open ~/.ssh/config and configure ssh
```shell
sudo nano ~/.ssh/config
```
```shell
	Host 10.*.*.*
		KexAlgorithms +diffie-hellman-group14-sha1,diffie-hellman-group-exchange-sha1
		HostKeyAlgorithms +ssh-rsa
```
- Second install all packages
```shell
sudo apt install -y python3-pip
sudo apt install python3-venv -y
python3 -m venv venv
source venv/bin/activate
python3 -m venv ansible
source ansible/bin/activate
python3 -m pip install ansible
python3 -m pip install paramiko
python3 -m pip install ansible-pylibssh
```
- Create directory 
```shell
mkdir ansible-testing
```
- Files configuration
```shell
nano /etc/ansible.cfg
	[defaults]
	inventory = hosts.yml
	host_key_checking = false
	retry_files_enable = false
	interpreter=python = /usr/bin/python3

nano /etc/inventory.yml
	[routers]
	router1 router1_host=10.1.1.1
	
nano /ansible-testing/group_vars/all.yml
	ansible_user: "rtladmin"
	ansible_password: "$p3Ctr@Ld1sP3r$10N258%592"
	ansible_connection: network_cli
	ansible_network_os: ios
	ansible_network: ios
```
- Playbook file config
```shell
nano /ansible-testing/get_routers_snmp.yml
	---
	- name: Config Zabbix Server
	hosts: all
	gather_facts: no

	tasks:
		- name: Configure zabbix
		  ios_config:
			lines:
				- permit 10.10.23.51
			parents: ip access-list standard acl_SNMP_RO

		- name: Configure SNMP base settings
		  ios_config:
          lines:
			- snmp-server community eR5pohP3vi RO acl_SNMP_RO
			- snmp-server community public RO acl_SNMP_RO
			- snmp-server enable traps snmp authentication linkdown linkup coldstart warmstart
			- snmp-server host 10.10.23.51 version 2c eR5pohP3vi
```
- Check playbook
```shell
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i /hosts.yml get_routers_snmp.yml
```
- If you got some "ok: [azsZ7]" congrats your script works!
## Config is done
