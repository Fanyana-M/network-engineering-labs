# IPv6 ADDRESSING & STATIC ROUTING 
## Purpose 
The purpose of this lab is to familiarize ourselves with IPv6 structure and notation. We’re going  to look at how we enable IPv6 on Cisco routers as well as how to configure IPv6 static routes.  Let’s get into it. 

## Topology


## Requirements 
1. Enable IPv6 routing and configure IPv6 addresses on the relevant interfaces on all 
routers 
2. Test connectivity between routers and hosts 
3. Configure IPv6 static routes on all routers 
4. Simulate edge routers as in the previous lab (default route)

## Tasks 
First, we’ll have to enable IPv6 routing and configure our IPv6 addresses on all the routers 
### R1 
```
R1(config)#ipv6 unicast-routing 
R1(config)#int e0/0 
R1(config-if)#ipv6 address 2001:DB8:12::1/64 
R1(config-if)#no shut 
R1(config)#int e0/1 
R1(config-if)#ipv6 address 2001:DB8:1::1/64 
R1(config-if)#no shut
```
### R2 
```
R2(config)#ipv6 unicast-routing 
R2(config)#int e0/0 
R2(config-if)#ipv6 address 2001:DB8:12::2/64 
R2(config-if)#no shut 
R2(config-if)#int e0/1 
R2(config-if)#ipv6 address 2001:DB8:23::1/64 
R2(config-if)#no shut
```
### R3 
```
R3(config)#ipv6 unicast-routing 
R3(config)#int e0/0 
R3(config-if)#ipv6 address 2001:DB8:23::2/64 
R3(config-if)#no shut 
R3(config-if)#int e0/1 
R3(config-if)#ipv6 add 2001:DB8:2::1/64 
R3(config-if)#no shut
```
### VPC4 
```
VPCS> ip 2001:DB8:1::2/64 2001:DB8:1::1 
PC1 : 2001:db8:1::2/64
```
### VPC5 
```
VPCS> ip 2001:DB8:2::2/64 
PC1 : 2001:db8:2::2/64
```
Once all interfaces are configured with an IPv6 address, we can test connectivity by pinging our  different interfaces. On Cisco IOS, we ping an IPv6 address by using the command ```ping ipv6 ipv6_address```. 

Next, we need to configure our static routes between our 2 LANs on all 3 of our routers. 

### R1 
```
R1(config)#ipv6 route 2001:db8:2::/64 2001:db8:12::2
```
### R2 
```
R2(config)#ipv6 route 2001:db8:2::/64 2001:db8:23::2 
R2(config)#ipv6 route 2001:db8:1::/64 2001:db8:12::1
```
### R3 
```
R3(config)#ipv6 route 2001:db8:1::/64 2001:db8:23::1
```
To verify that our routes are correct, we’ll ping VPC5 from VPC4. Once that has been verified,  we’ll go ahead and configure default routes on R1 as well as R3. 
### R1 
```
R1(config)#ipv6 route ::/0 2001:db8:12::2
```
### R3 
```
R3(config)#ipv6 route ::/0 2001:db8:23::1
```
We can then use show ipv6 route to verify the default route. Now, we can add a static  address to VPC5’s LAN using a link-local address. 
### R1 
```
R1(config)#ipv6 route 2001:db8:2::/64 e0/0 fe80::2
```

📄 Full write-up: [IPv6-Addressing_and-Static-Routing.pdf](IPv6-Addressing_and-Static-Routing.pdf)
