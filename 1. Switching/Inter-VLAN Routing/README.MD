
# Inter-VLAN Routing

## Purpose
The purpose of this lab is to practice configuring inter-VLAN routing using the router-on-a-stick method using a layer-2 switch and a router, as well as SVI-based routing using a layer-3 switch.

## Topology
![Topology](topology.png)

## Requirements
### Part A - Router-on-a-stick
- Create VLAN 10 & VLAN 20 on SW1  
- Configure the link between SW1 and R1 as a trunk 
- Create subinterfaces on R1
- Assign access ports on SW1
- Ping accross VLANs

### Part B - Layer 3 Switch Routing (SVIs)
- Create VLAN 10 & VLAN 20 on L3-SW1  
- Create SVIs 
- Assign access ports
- Enable routing
- Ping across VLANs

## Tasks
### Part A - RoaS
#### SW1:
```plaintext
SW1>enable
SW1#configure terminal
SW1(config)#vlan 10
SW1(config-vlan)#name Sales
SW1(config-vlan)#vlan 20
SW1(config-vlan)#name HR
SW1(config-vlan)exit
SW1(config)#interface gig0/1
SW1(config-if)#switchport mode trunk
SW1(config-if)#switchport trunk allowed vlan 10,20
SW1(config-if)#interface fa0/1
SW1(config-if)#switchport mode access
SW1(config-if)#switchport access vlan 10
SW1(config-if)#interface fa0/2
SW1(config-if)#switchport mode access
SW1(config-if)#switchport access vlan 20
SW1(config-if)#end
```

#### R1:
```plaintext
R1>enable
R1#config t
R1(config)#int gig0/0/0.10
R1(config-subif)#encapsulation dot1Q 10
R1(config-subif)#ip address 192.168.10.1 255.255.255.0
R1(config-subif)#int gig0/0/0.20
R1(config-subif)#encapsulation dot1Q 20
R1(config-subif)#ip address 192.168.20.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int gig0/0/0
R1(config-if)#no shutdown
R1(config-if)#end
```

### Part B - L3 Switch
#### L3-SW1:
```plaintext
L3-SW1>en
L3-SW1#config t
L3-SW1(config)#vlan 10
L3-SW1(config-vlan)#name Sales
L3-SW1(config-vlan)#vlan 20
L3-SW1(config-vlan)#name HR
L3-SW1(config-vlan)#exit
L3-SW1(config)#int vlan 10
L3-SW1(config-if)#ip address 192.168.10.1 255.255.255.0
L3-SW1(config-if)#int vlan 20
L3-SW1(config-if)#ip address 192.168.20.1 255.255.255.0
L3-SW1(config-if)#int fa0/1
L3-SW1(config-if)#switchport mode access
L3-SW1(config-if)#switchport access vlan 10
L3-SW1(config-if)#int fa0/2
L3-SW1(config-if)#switchport mode access
L3-SW1(config-if)#switchport access vlan 20
L3-SW1(config-if)#exit
L3-SW1(config)#ip routing
L3-SW1(config)#end
```

## Conclusion
In this lab, we went through the configurations required for inter-VLAN routing. The first part consisted of a router-on-a-stick configuration, and the second consisted of routing through SVIs on a layer 3 switch. Subinterfaces were configured on R1 with their respective dot1Q encapsulation as well as their respective IP addresses. The link between R1 and SW1 was configured as a trunk, allowing both VLAN 10 and 20 and the respective ports facing the PCs were configured as access ports allowing their respective VLANs.

For Part B, VLAN 10 & VLAN 20 were created on the L3 switch. The SVIs were then created and they were given their respective IP addresses. The ports facing the PCs were configured as access ports, allowing their respective VLANs. IP routing was also enabled on the switch to allow the PCs to communicate with one another.

📄 Full write-up: [Inter-VLAN-Routing.pdf](Inter-VLAN-Routing.pdf)

