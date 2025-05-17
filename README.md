# DMVPN Dual Hub Single Cloud Project

This repository contains the configuration files for a DMVPN (Dynamic Multipoint Virtual Private Network) setup with a dual hub, single cloud architecture. The configurations are designed for Cisco devices and include settings for PCs, servers, switches, hubs, spokes, ISPs, and a Google router.

## Project Overview

The DMVPN dual hub single cloud topology provides a scalable and secure network solution using GRE (Generic Routing Encapsulation) tunnels, IPsec encryption, and NHRP (Next Hop Resolution Protocol). The setup includes:

- **Hubs**: Two hub routers (Hub-1 and Hub-2) for redundancy.
- **Spokes**: Seven spoke routers (Spoke-1 to Spoke-7) connecting to the hubs.
- **PCs and Servers**: Configured with static routes and telnet access.
- **Switches**: Basic switch configurations for connectivity.
- **ISPs**: Four ISP routers using RIP (Routing Information Protocol) for external connectivity.
- **Google Router**: Simulates an external network.

## Configuration Files

Below are the configuration details for each device in the network.

### PCs

#### PC-1
```bash
en
conf t
ho PC-1
int e0
ip add 192.168.1.10 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 192.168.1.1
ip route 0.0.0.0 0.0.0.0 192.168.1.2
line vty 0 4
pass telnet
login
do wr
end
```

#### PC-2
```bash
en
conf t
ho PC-2
int e0
ip add 192.168.2.10 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 192.168.2.1
line vty 0 4
pass telnet
login
do wr
end
```

#### PC-3
```bash
en
conf t
ho PC-3
int e0
ip add 192.168.3.10 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 192.168.3.1
line vty 0 4
pass telnet
login
do wr
end
```

#### PC-4
```bash
en
conf t
ho PC-4
int e0
ip add 192.168.4.10 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 192.168.4.1
line vty 0 4
pass telnet
login
do wr
end
```

#### PC-5
```bash
en
conf t
ho PC-5
int e0
ip add 192.168.5.10 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 192.168.5.1
line vty 0 4
pass telnet
login
do wr
end
```

#### PC-6
```bash
en
conf t
ho PC-6
int e0
ip add 192.168.6.10 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 192.168.6.1
line vty 0 4
pass telnet
login
do wr
end
```

#### PC-7
```bash
en
conf t
ho PC-7
int e0
ip add 192.168.7.10 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 192.168.7.1
line vty 0 4
pass telnet
login
do wr
end
```

#### PC-8
```bash
en
conf t
ho PC-8
int e0
ip add 192.168.8.10 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 192.168.8.1
line vty 0 4
pass telnet
login
do wr
end
```

### Servers

#### Server-1
```bash
en
conf t
ho Server-1
int e0
ip add 192.168.1.11 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 192.168.1.1
ip route 0.0.0.0 0.0.0.0 192.168.1.2
line vty 0 4
pass telnet
login
do wr
end
```

#### Server-2
```bash
en
conf t
ho Server-2
int e0
ip add 192.168.2.11 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 192.168.2.1
line vty 0 4
pass telnet
login
do wr
end
```

#### Server-3
```bash
en
conf t
ho Server-3
int e0
ip add 192.168.5.11 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 192.168.5.1
line vty 0 4
pass telnet
login
do wr
end
```

### Switches

#### Switch-1
```bash
en
conf t
ho Switch-1
int e0/0
duplex full
no shut
int e0/1
duplex full
no shut
int e0/2
duplex full
no shut
int e0/3
duplex full
no shut
end
wr
```

#### Switch-2
```bash
en
conf t
ho Switch-2
int e0/0
duplex full
no shut
int e0/1
duplex full
no shut
int e0/2
duplex full
no shut
end
wr
```

#### Switch-3
```bash
en
conf t
ho Switch-3
int e0/0
duplex full
no shut
int e0/1
duplex full
no shut
int e0/2
duplex full
no shut
end
wr
```

### Hubs

#### Hub-1
```bash
en
conf t
ho Hub-1
int fa0/0
ip add 210.165.100.1 255.255.255.252
no shu
duplex full
int fa1/0
ip add 192.168.1.1 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 fa0/0

crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 86400

crypto isakmp key DMVPN_KEY address 0.0.0.0

crypto ipsec transform-set DMVPN-TSET esp-aes 256 esp-sha256-hmac
 mode tunnel

crypto ipsec profile DMVPN-PROFILE
 set transform-set DMVPN-TSET

interface tunnel 100
ip address 10.0.0.1 255.255.255.0
ip nhrp authentication NHRP_KEY
ip nhrp map multicast dynamic
ip nhrp network-id 1
ip nhrp redirect
tunnel source fastEthernet0/0
tunnel mode gre multipoint
tunnel protection ipsec profile DMVPN-PROFILE
ip ospf network point-to-multipoint
ip ospf cost 10
no shutdown

Router ospf 1
router-id 1.1.1.1
Network 192.168.1.0 0.0.0.255 area 0
Network 10.0.0.0 0.0.0.255 area 0
end
wr
```

#### Hub-2
```bash
en
conf t
ho Hub-2
int fa0/0
ip add 210.165.100.5 255.255.255.252
no shu
duplex full
int fa1/0
ip add 192.168.1.2 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 fa0/0

crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 86400

crypto isakmp key DMVPN_KEY address 0.0.0.0

crypto ipsec transform-set DMVPN-TSET esp-aes 256 esp-sha256-hmac
 mode tunnel

crypto ipsec profile DMVPN-PROFILE
 set transform-set DMVPN-TSET

interface tunnel 100
ip address 10.0.0.2 255.255.255.0
ip nhrp authentication NHRP_KEY
ip nhrp map multicast dynamic
ip nhrp network-id 1
ip nhrp redirect
tunnel source fastEthernet0/0
tunnel mode gre multipoint
ip nhrp map 10.0.0.1 210.165.100.1
ip nhrp map multicast 210.165.100.1
ip nhrp nhs 10.0.0.1
tunnel protection ipsec profile DMVPN-PROFILE
ip ospf network point-to-multipoint
ip ospf cost 20
no shutdown

Router ospf 1
router-id 2.2.2.2
Network 192.168.1.0 0.0.0.255 area 0
Network 10.0.0.0 0.0.0.255 area 0
end
wr
```

### Spokes

#### Spoke-1
```bash
en
conf t
ho Spoke-1
int fa0/0
ip add 210.165.100.9 255.255.255.252
no shu
duplex full
int fa1/0
ip add 192.168.2.1 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 fa0/0

crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 86400

crypto isakmp key DMVPN_KEY address 0.0.0.0

crypto ipsec transform-set DMVPN-TSET esp-aes 256 esp-sha256-hmac
 mode tunnel

crypto ipsec profile DMVPN-PROFILE
 set transform-set DMVPN-TSET

interface tunnel 100
ip address 10.0.0.3 255.255.255.0
ip nhrp authentication NHRP_KEY
ip nhrp map 10.0.0.1 210.165.100.1
ip nhrp map 10.0.0.2 210.165.100.5
ip nhrp map multicast 210.165.100.1
ip nhrp map multicast 210.165.100.5
ip nhrp network-id 1
ip nhrp nhs 10.0.0.1 priority 1
ip nhrp nhs 10.0.0.2 priority 5
ip nhrp shortcut
ip nhrp registration timeout 60
ip nhrp holdtime 300 
tunnel source fastEthernet0/0
tunnel mode gre multipoint
tunnel protection ipsec profile DMVPN-PROFILE
ip ospf network point-to-multipoint
no shutdown

Router ospf 1
router-id 3.3.3.3
Network 192.168.2.0 0.0.0.255 area 0
Network 10.0.0.0 0.0.0.255 area 0
end
wr
```

#### Spoke-2
```bash
en
conf t
ho Spoke-2
int fa0/0
ip add 210.165.100.13 255.255.255.252
no shu
duplex full
int fa1/0
ip add 192.168.3.1 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 fa0/0

crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 86400

crypto isakmp key DMVPN_KEY address 0.0.0.0

crypto ipsec transform-set DMVPN-TSET esp-aes 256 esp-sha256-hmac
 mode tunnel

crypto ipsec profile DMVPN-PROFILE
 set transform-set DMVPN-TSET

interface tunnel 100
ip address 10.0.0.4 255.255.255.0
ip nhrp authentication NHRP_KEY
ip nhrp map 10.0.0.1 210.165.100.1
ip nhrp map 10.0.0.2 210.165.100.5
ip nhrp map multicast 210.165.100.1
ip nhrp map multicast 210.165.100.5
ip nhrp network-id 1
ip nhrp nhs 10.0.0.1 priority 1
ip nhrp nhs 10.0.0.2 priority 5
ip nhrp shortcut
ip nhrp registration timeout 60
ip nhrp holdtime 300 
tunnel source fastEthernet0/0
tunnel mode gre multipoint
tunnel protection ipsec profile DMVPN-PROFILE
ip ospf network point-to-multipoint
no shutdown

Router ospf 1
router-id 4.4.4.4
Network 192.168.3.0 0.0.0.255 area 0
Network 10.0.0.0 0.0.0.255 area 0
end
wr
```

#### Spoke-3
```bash
en
conf t
ho Spoke-3
int fa0/0
ip add 210.165.100.17 255.255.255.252
no shu
duplex full
int fa1/0
duplex full
ip add 192.168.4.1 255.255.255.0
no shu
exi
ip route 0.0.0.0 0.0.0.0 fa0/0

crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 86400

crypto isakmp key DMVPN_KEY address 0.0.0.0

crypto ipsec transform-set DMVPN-TSET esp-aes 256 esp-sha256-hmac
 mode tunnel

crypto ipsec profile DMVPN-PROFILE
 set transform-set DMVPN-TSET

Interface tunnel 100
Ip address 10.0.0.5 255.255.255.0
ip nhrp authentication NHRP_KEY
ip nhrp map 10.0.0.1 210.165.100.1
ip nhrp map 10.0.0.2 210.165.100.5
ip nhrp map multicast 210.165.100.1
ip nhrp map multicast 210.165.100.5
ip nhrp network-id 1
ip nhrp nhs 10.0.0.1 priority 1
ip nhrp nhs 10.0.0.2 priority 5
ip nhrp shortcut
ip nhrp registration timeout 60
ip nhrp holdtime 300 
tunnel source fastEthernet0/0
tunnel mode gre multipoint
tunnel protection ipsec profile DMVPN-PROFILE
ip ospf network point-to-multipoint
no shutdown

Router ospf 1
router-id 5 RADIUS Authentication and Accounting
Network 192.168.4.0 0.0.0.255 area 0
Network 10.0.0.0 0.0.0.255 area 0
end
wr
```

#### Spoke-4
```bash
en
conf t
ho Spoke-4
int fa0/0
ip add 210.165.100.21 255.255.255.252
no shu
duplex full
int fa1/0
ip add 192.168.5.1 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 fa0/0

crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 86400

crypto isakmp key DMVPN_KEY address 0.0.0.0

crypto ipsec transform-set DMVPN-TSET esp-aes 256 esp-sha256-hmac
 mode tunnel

crypto ipsec profile DMVPN-PROFILE
 set transform-set DMVPN-TSET

Interface tunnel 100
Ip address 10.0.0.6 255.255.255.0
ip nhrp authentication NHRP_KEY
ip nhrp map 10.0.0.1 210.165.100.1
ip nhrp map 10.0.0.2 210.165.100.5
ip nhrp map multicast 210.165.100.1
ip nhrp map multicast 210.165.100.5
ip nhrp network-id 1
ip nhrp nhs 10.0.0.1 priority 1
ip nhrp nhs 10.0.0.2 priority 5
ip nhrp shortcut
ip nhrp registration timeout 60
ip nhrp holdtime 300 
tunnel source fastEthernet0/0
tunnel mode gre multipoint
tunnel protection ipsec profile DMVPN-PROFILE
ip ospf network point-to-multipoint
no shutdown

Router ospf 1
router-id 6.6.6.6
Network 192.168.5.0 0.0.0.255 area 0
Network 10.0.0.0 0.0.0.255 area 0
end
wr
```

#### Spoke-5
```bash
en
conf t
ho Spoke-5
int fa0/0
ip add 210.165.100.25 255.255.255.252
no shu
duplex full
int fa1/0
ip add 192.168.6.1 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 fa0/0

crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 86400

crypto isakmp key DMVPN_KEY address 0.0.0.0

crypto ipsec transform-set DMVPN-TSET esp-aes 256 esp-sha256-hmac
 mode tunnel

crypto ipsec profile DMVPN-PROFILE
 set transform-set DMVPN-TSET

Interface tunnel 100
Ip address 10.0.0.7 255.255.255.0
ip nhrp authentication NHRP_KEY
ip nhrp map 10.0.0.1 210.165.100.1
ip nhrp map 10.0.0.2 210.165.100.5
ip nhrp map multicast 210.165.100.1
ip nhrp map multicast 210.165.100.5
ip nhrp network-id 1
ip nhrp nhs 10.0.0.1 priority 1
ip nhrp nhs 10.0.0.2 priority 5
ip nhrp shortcut
ip nhrp registration timeout 60
ip nhrp holdtime 300 
tunnel source fastEthernet0/0
tunnel mode gre multipoint
tunnel protection ipsec profile DMVPN-PROFILE
ip ospf network point-to-multipoint
no shutdown

Router ospf 1
router-id 7.7.7.7
Network 192.168.6.0 0.0.0.255 area 0
Network 10.0.0.0 0.0.0.255 area 0
end
wr
```

#### Spoke-6
```bash
en
conf t
ho Spoke-6
int fa0/0
ip add 210.165.100.29 255.255.255.252
no shu
duplex full
int fa1/0
ip add 192.168.7.1 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 fa0/0

crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 86400

crypto isakmp key DMVPN_KEY address 0.0.0.0

crypto ipsec transform-set DMVPN-TSET esp-aes 256 esp-sha256-hmac
 mode tunnel

crypto ipsec profile DMVPN-PROFILE
 set transform-set DMVPN-TSET

Interface tunnel 100
Ip address 10.0.0.8 255.255.255.0
ip nhrp authentication NHRP_KEY
ip nhrp map 10.0.0.1 210.165.100.1
ip nhrp map 10.0.0.2 210.165.100.5
ip nhrp map multicast 210.165.100.1
ip nhrp map multicast 210.165.100.5
ip nhrp network-id 1
ip nhrp nhs 10.0.0.1 priority 1
ip nhrp nhs 10.0.0.2 priority 5
ip nhrp shortcut
ip nhrp registration timeout 60
sip holdtime 300 
tunnel source fastEthernet0/0
tunnel mode gre multipoint
tunnel protection ipsec profile DMVPN-PROFILE
ip ospf network point-to-multipoint
no shutdown

Router ospf 1
router-id 8.8.8.8
Network 192.168.7.0 0.0.0.255 area 0
Network 10.0.0.0 0.0.0.255 area 0
end
wr
```

#### Spoke-7
```bash
en
conf t
ho Spoke-7
int fa0/0
ip add 210.165.100.33 255.255.255.252
no shu
duplex full
int fa1/0
ip add 192.168.8.1 255.255.255.0
no shu
duplex full
exi
ip route 0.0.0.0 0.0.0.0 fa0/0

crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 86400

crypto isakmp key DMVPN_KEY address 0.0.0.0

crypto ipsec transform-set DMVPN-TSET esp-aes 256 esp-sha256-hmac
 mode tunnel

crypto ipsec profile DMVPN-PROFILE
 set transform-set DMVPN-TSET

Interface tunnel 100
Ip address 10.0.0.9 255.255.255.0
ip nhrp authentication NHRP_KEY
ip nhrp map 10.0.0.1 210.165.100.1
ip nhrp map 10.0.0.2 210.165.100.5
ip nhrp map multicast 210.165.100.1
ip nhrp map multicast 210.165.100.5
ip nhrp network-id 1
ip nhrp nhs 10.0.0.1 priority 1
ip nhrp nhs 10.0.0.2 priority 5
ip nhrp shortcut
ip nhrp registration timeout 60
ip nhrp holdtime 300 
tunnel source fastEthernet0/0
tunnel mode gre multipoint
tunnel protection ipsec profile DMVPN-PROFILE
ip ospf network point-to-multipoint
no shutdown

Router ospf 1
router-id 9.9.9.9
Network 192.168.8.0 0.0.0.255 area 0
Network 10.0.0.0 0.0.0.255 area 0
end
wr
```

### ISPs

#### ISP-1
```bash
en
conf t
ho ISP-1
int fa0/0
ip add 210.165.100.2 255.255.255.252
no shu
speed 100
duplex full
int fa0/1
ip add 210.165.100.41 255.255.255.252
no shu
speed 100
duplex full
int fa1/0
ip add 210.165.100.37 255.255.255.252
no shu
speed 100
duplex full
int fa2/0
ip add 8.8.8.1 255.255.255.0
no shu
speed 100
duplex full
exi
no router ospf 1
router rip
ver 2
no au
pass fa0/0
network 210.165.100.0 
network 210.165.100.36 
network 210.165.100.40 
network 8.8.8.0  
do wr
```

#### ISP-2
```bash
en
conf t
ho ISP-2
int fa0/0
ip add 210.165.100.6 255.255.255.252
no shu
speed 100
duplex full
int fa0/1
ip add 210.165.100.49 255.255.255.252
no shu
speed 100
duplex full
int fa1/0
ip add 210.165.100.38 255.255.255.252
no shu
speed 100
duplex full
exi
no router ospf 1
router rip
ver 2
no au
pass fa0/0
network 210.165.100.4 
network 210.165.100.36 
network 210.165.100.48 
do wr
```

#### ISP-3
```bash
en
conf t
ho ISP-3
int fa0/0
ip add 210.165.100.10 255.255.255.252
no shu
duplex full
int fa4/0
ip add 210.165.100.42 255.255.255.252
no shu
duplex full
int fa5/0
ip add 210.165.100.45 255.255.255.252
no shu
duplex full
int fa1/0
ip add 210.165.100.14 255.255.255.252
no shu
duplex full
int fa2/0
ip add 210.165.100.18 255.255.255.252
no shu
duplex full
exi
no router ospf 1
router rip
ver 2
no au
pass fa0/0
network 210.165.100.8 
network 210.165.100.12 
network 210.165.100.16 
network 210.165.100.40 
network 210.165.100.44 
do wr
```

#### ISP-4
```bash
en
conf t
ho ISP-4
int fa0/0
ip add 210.165.100.22 255.255.255.252
no shu
duplex full
int fa4/0
ip add 210.165.100.50 255.255.255.252
no shu
duplex full
int fa5/0
ip add 210.165.100.46 255.255.255.252
no shu
duplex full
int fa1/0
ip add 210.165.100.26 255.255.255.252
no shu
duplex full
int fa2/0
ip add 210.165.100.30 255.255.255.252
no shu
duplex full
int fa3/0
ip add 210.165.100.34 255.255.255.252
no shu
duplex full
exi
no router ospf 1
router rip
ver 2
no au
pass fa0/0
network 210.165.100.20 
network 210.165.100.24 
network 210.165.100.28 
network 210.165.100.32 
network 210.165.100.44 
network 210.165.100.48 
do wr
```

### Google Router
```bash
en
conf t 
ho google
int e0 
ip add 8.8.8.8 255.255.255.0
no shu
duplex full
exit
ip route 0.0.0.0 0.0.0.0 8.8.8.1
end 
wr
```

## Setup Instructions

1. **Configure Devices**: Apply the configurations to the respective devices using a Cisco IOS-compatible emulator or hardware.
2. **Verify Connectivity**: Ensure all interfaces are up and running (`no shutdown`).
3. **Test DMVPN**: Verify tunnel establishment and NHRP mappings using commands like `show dmvpn` and `show ip nhrp`.
4. **Check Routing**: Confirm OSPF and RIP routing tables with `show ip route`.
5. **Test Telnet**: Verify telnet access to PCs and servers using the configured password.

## Notes

- Replace `DMVPN_KEY` and `NHRP_KEY` with secure keys in a production environment.
- Ensure consistent OSPF router IDs and area configurations.
- The configurations assume full-duplex interfaces for optimal performance.

## License

This project is licensed under the MIT License.