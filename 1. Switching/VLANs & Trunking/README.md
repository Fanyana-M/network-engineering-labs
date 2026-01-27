# 1.1 VLAN Creation & Trunking

## Purpose
The purpose of this lab is to go through the fundamentals of VLAN creation, assigning access
and trunk ports, as well as verifying VLAN propagation.

## Topology
![Topology](topology.png)

## Requirements
- Creating VLANs (10 - Users, 20 - Servers, 30 - Management)
- Configure trunk  links
- Configure access ports
- Configure native VLAN

## Tasks

### SW1
~~~plaintext
enable
config terminal
hostname SW1
vlan 10
name Users
vlan 20
name Servers
vlan 30
name Management
exit
interface range fa0/1-2
switchport mode trunk
switchport trunk allowed vlan 10,20,30
switchport trunk native vlan 30
~~~

### SW2
~~~plaintext
enable
config t
hostname SW2
vlan 10
name Users
vlan 20
name Servers
vlan 30
name Management
exit
interface range fa0/1,fa0/3
switchport mode trunk
switchport trunk allowed vlan 10,20,30
switchport trunk native vlan 30
exit
interface fa0/5
switchport mode access
switchport access vlan 10
end
~~~

### SW3
~~~plaintext
enable
config t
hostname SW3
vlan 10
name Users
vlan 20
name Servers
vlan 30
name Management
exit
interface range fa0/2-3
switchport mode trunk
switchport trunk allowed vlan 10,20,30
switchport trunk native vlan 30
exit
interface fa0/5
switchport mode access
switchport access vlan 20
end
~~~

📄 Full write-up: [1.1-VLAN-Creation-and-Trunking.pdf](1.1-VLAN-Creation-and-Trunking.pdf)
