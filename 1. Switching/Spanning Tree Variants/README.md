# Spanning Tree Variants

## Purpose
The purpose of this lab is to explore and understand the different variants of the Spanning Tree Protocol (STP), that being STP, RSTP (Rapid Spanning Tree Protocol) & PVST+ (Per-VLAN Spanning Tree Plus). We’ll look into how a switch elects a root bridge, blocks redundant links and how they converge under failure. We’ll also look at the differences across STP, RSTP as well as PVST+ modes.

## Topology
![Topology](topology.png)

## Requirements
### 1. STP (IEEE 802.1D)
- Enable STP on all switches
- Observe root bridge election
- Identify root ports and designated ports
- Identify which link becomes blocked

### 2. RSTP (IEEE 802.1w)
- Enable RSTP on all switches
- Note differences in convergence
- Trigger a topology change
- Observe faster transitions

### 3. PVST+ (Cisco's proprietary per-VLAN STP)
- Enable PVST on all switches
- Use at least 2 VLANSs (10 & 20)
- Make SW1 root for VLAN 10
- Make SW2 root for VLAN 20
- Observe how blocked ports differ per VLAN

## Tasks
### STP
STP is enabled by default on Cisco switches, so we’ll go ahead and run ```show spanning-tree``` on all three switches.

```
>enable
#show spanning-tree
```

### RSTP
For the second part, we’ll enable RSTP on all three switches.
```
#config t
(config)#spanning-tree mode rapid-pvst
```

From there, we’ll shutdown FastEthernet0/2 then undo the shutdown on SW1, and observe the faster transition based on the topology change.

### PVST
For the last part, we’ll go ahead and enable PVST+ and create VLAN 10 and VLAN 20 on all three switches. All the links should be made trunk links as well, and have both VLAN 10 & 20 allowed on them. We will then make SW1 the root for VLAN 10, and SW2 for VLAN 20

```
(config)#spanning-tree mode pvst
(config)#vlan 10
(config-vlan)#vlan 20
```

#### SW1
```
SW1(config)#int range fa0/1-2
SW1(config-if-range)#switchport mode trunk
SW1(config-if-range)#switchport trunk allowed vlan 10,20
SW(config-if-range)#exit
SW1(config)#spanning-tree vlan 10 priority 4096
SW1(config)#end
```

#### SW2
```
SW2(config)#int range fa0/1,fa0/3
SW2(config-if-range)#switchport mode trunk
SW2(config-if-range)#switchport trunk allowed vlan 10,20
SW2(config-if-range)#exit
SW2(config)#spanning-tree vlan 10 priority 4096
SW2(config)#end
```
#### SW3
```
SW3(config)#int range fa0/1-2
SW3(config-if-range)#switchport mode trunk
SW3(config-if-range)#switchport trunk allowed vlan 10,20
SW3(config-if-range)#end
```
