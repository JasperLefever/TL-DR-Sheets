# CheetSheet Computer Networks 2 - Cisco SRWE

## IPV6 On a Switch

```bash
sdm prefer dual-ipv4-and-ipv6 default

reload
```

## Show Commands

```bash
Switch# show running-config
Switch# show flash
Switch# show mac address-table
Switch# show ip interface brief
Switch# show ipv6 interface brief
Switch# show interfaces
Switch# show interfaces <interface>
Switch# show ipv6 interface brief
Switch# show ip route
Switch# show ip route static
Switch# show ipv6 route
Switch# show vlan
Switch# show vlan brief
Switch# show interfaces trunk
Switch# show interfaces status
Switch# show etherchannel summary
Switch# show etherchannel port-channel
Switch# show interfaces port-channel <number>
Switch# show ip dhcp binding
Switch# show ip dhcp server statistics
Switch# show ipv6 dhcp pool
Switch# show standby brief
Router# show ip protocols | include Router ID # OSPF
Router# show ip ospf neighbor # verify the OSPFv2 adjacencies
Router# show ip ospf interface <interface> # verify the OSPFv2 configuration on the interface
Router# show access-lists # show all ACLs
Router# show spanning-tree summary 
```

## SVI Configuration

```bash
Switch(config)# interface vlan <vlan of management interface>
Switch(config-if)# ip address <ip address> <subnet mask>
Switch(config-if)# no shutdown
Switch(config-if)# description "Management Interface"
Switch(config-if)# end

# Setting default gateway
Switch(config)# ip default-gateway <ip address>
```

## Configure SSH

```bash
Switch# show ip ssh # Check if SSH is supported
Switch# ip domain-name <domain name>
Switch# crypto key generate rsa
Switch# username <username> secret <password>
Switch# line vty 0 15
Switch(config-line)# transport input ssh
Switch(config-line)# login local
Switch(config-line)# exit
Switch(config)# ip ssh version 2
```

## Change Full-Duplex

```bash
Switch(config)# interface <interface>
Switch(config-if)# duplex <full/half>
Switch(config-if)# speed <10/100/1000/10000>
```

## Set password for console

```bash
Switch(config)# line console 0
Switch(config-line)# password <password>
Switch(config-line)# login
```

## Password for exec mode

```bash
Switch(config)# enable secret <password>
```

## Encrypt Passwords

```bash
Switch(config)# service password-encryption
```

## Set hostname

```bash
Switch(config)# hostname <hostname>
Switch(config)# end
```

## Set banner

```bash
Switch(config)# banner motd <banner>
Switch(config)# end
```

## Interface Configuration

```bash
Switch(config)# interface <interface>
Switch(config-if)# ip address <ip address> <subnet mask>
Switch(config-if)# ipv6 address <ipv6 address>/<prefix length>
Switch(config-if)# no shutdown
Switch(config-if)# description <description>
# Possible  VLAN configuration
```

## Loopback Interface

> Loopback interfaces are virtual interfaces that are always up and running. They are used for testing and troubleshooting.

```bash
Switch(config)# interface loopback <number>
Switch(config-if)# ip address <ip address> <subnet mask>
```

## Enable ipv6 routing

```bash
Router(config)# ipv6 unicast-routing
```

## Static Routes

On directly connected networks use the exit interface as the next-hop option.

### ipv4

```bash
Router(config)# ip route <destination network> <subnet mask> <next-hop ip address> | <exit interface>
# Fully specified route
#Router(config)# ip route <destination network> <subnet mask> <interface> <next-hop ip address>
```

### ipv6

```bash
Router# ipv6 unicast-routing
Router(config)# ipv6 route <destination network>/<prefix length> <next-hop ip address> | <exit interface>
```

## Default Static Routes

```bash
Router(config)# ip route 0.0.0.0 0.0.0.0 <next-hop ip address> | <exit interface>
Router(config)# end
Router# copy running-config startup-config
```

```bash
Router(config)# ipv6 route ::/0 <next-hop ip address> | <exit interface>
```

## Floating Static Routes

```bash
Router(config)# ip route <destination network> <subnet mask> <next-hop ip address> <administrative distance mostly 5>
```

## VLAN Config

### Switch

#### Create VLAN

```bash
Switch(config)# vlan <vlan number>
Switch(config-vlan)# name <vlan name>
```

#### Assign VLAN to Interface

```bash
Switch(config)# interface <interface>
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan <vlan number>
```

#### Assign VLAN to Trunk

```bash
Switch(config)# interface <interface>
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan <native vlan number>
Switch(config-if)# switchport trunk allowed vlan <vlan number>
```

#### Delete VLAN

```bash
Switch(config)# no vlan <vlan number>
```

### Router

#### Assign VLAN to subinterface

```bash
Router(config)# interface <interface>.<vlan number>
Router(config-subif)# description <description>
Router(config-subif)# no shutdown
Router(config-subif)# encapsulation dot1Q <vlan number>
Router(config-subif)# ip address <ip address> <subnet mask>
```

### Layer 3 Switch

#### Assign VLAN to SVI

```bash
Switch(config)# interface vlan <vlan number>
Switch(config-if)# description <description>
Switch(config-if)# ip address <ip address> <subnet mask>
Switch(config-if)# no shutdown
```

#### Routing configuration to other Layer 3 Device

```bash
Switch(config)# interface <interface>
Switch(config-if)# no switchport
Switch(config-if)# ip address <ip address> <subnet mask>
Switch(config-if)# end
Switch(config)# ip route <destination network> <subnet mask> <next-hop ip address> | <exit interface>
```

## EtherChannel with LACP

```bash
Switch(config)# interface range <interface1> - <interface2>
Switch(config-if-range)# channel-group <number> mode active
Switch(config-if-range)# exit
Switch(config)# interface port-channel <number>
Switch(config-if)# description <description>
Switch(config-if)# no shutdown
## All other configurations like VLAN, IP, etc.
```

## DHCPv4

```bash
Router(config)# ip dhcp excluded-address <start ip address> <end ip address>
### Create new pool
Router(config)# ip dhcp pool <pool name>
Router(dhcp-config)# network <network> <subnet mask>
Router(dhcp-config)# default-router <default gateway>
Router(dhcp-config)# dns-server <dns server>
##Router(dhcp-config)# domain-name <domain name>
Router(dhcp-config)# lease <days> <hours> <minutes>
##Router(dhcp-config)# netbios-name-server <netbios server>
```

### DHCPv4 Relay

```bash
Router(config)# interface <interface>
Router(config-if)# ip helper-address <ip address of DHCP server>
```

### DHCPv4 Client

```bash
Router(config)# interface <interface>
Router(config-if)# ip address dhcp
```

## DHCPv6

### Stateless DHCPv6 server

```bash
Router(config)# ipv6 unicast-routing
Router(config)# ipv6 dhcp pool <pool name>
Router(config-dhcp)# dns-server <dns server>
Router(config-dhcp)# domain-name <domain name>
Router(config-dhcp)# ipv6 dhcp server <pool name>
Router(config-dhcp)# ipv6 nd other-config-flag
```

### Stateless DHCPv6 client

```bash
Router# ipv6 unicast-routing
Router(config)# interface <interface>
Router(config-if)# ipv6 enable
Router(config-if)# ipv6 address autoconfig
```

### Stateful DHCPv6 server

```bash
Router(config)# ipv6 unicast-routing
Router(config)# ipv6 dhcp pool <pool name>
Router(config-dhcp)# address prefix <prefix> <prefix length>
Router(config-dhcp)# dns-server <dns server>
Router(config-dhcp)# domain-name <domain name>
Router(config-dhcp)# ipv6 dhcp server <pool name>
Router(config-dhcp)# ipv6 nd managed-config-flag
Router(config-dhcp)# ipv6 nd prefix default no-autoconfig
```

### Stateful DHCPv6 client

```bash
Router# ipv6 unicast-routing
Router(config)# interface <interface>
Router(config-if)# ipv6 enable
Router(config-if)# ipv6 address dhcp
```

### DHCPv6 Relay

```bash
Router(config)# interface <interface>
Router(config-if)# ipv6 dhcp relay destination <ip address of DHCP server> <interface>
```

## HSRP Configuration

Active router

```bash
R1(config)# interface g0/0/1
R1(config-if)# standby version 2
R1(config-if)# standby 1 ip 172.16.10.1
R1(config-if)# standby 1 priority 150
## ONLY WHEN HAS TO BE PREEMPTIVE
# R1(config-if)# standby 1 preempt
```

Standby router

```bash
R2(config)# interface g0/0/1
R2(config-if)# standby version 2
R2(config-if)# standby 1 ip 172.16.10.1
```

## OSPF

### OSP Databases

```bash
Router# show ip ospf neighbor # Check OSPF neighbors (adjacency)
Router# show ip ospf database # Check OSPF Link State Database (LSDB)
Router# show ip route ospf # Check routing table
```

### OSPF Router ID Configuration

```bash
Router(config)# router ospf <process id> # OSPF process id should be the same in all routers in the same area, between 1 and 65535
Router(config-router)# router-id <router id> # Optional, but recommended to be set manually
```

### Modify OSPF Router ID

```bash
Router(config)# router ospf <process id>
Router(config-router)# router-id <NEW router id> # Optional, but recommended to be set manually
Router(config-router)# end
Router# clear ip ospf process # Restart OSPF process to apply changes
```

### Point-to-Point OSPF Configuration

> Can also be done using the interface command. `ip ospf <process id> area <area id>`

> This will enable OSPF on all interfaces matching the provided network.
> Wildcard mask is calculated as `255.255.255.255 - subnet mask`.

```bash
Router(config-router)# network <network> <wildcard mask> area <area id>
# OR for a specific interface
# Router(config-router)# network <ip address> 0.0.0.0 area <area id>

```

#### OSPF Passive Interface

```bash
Router(config-router)# passive-interface <interface> # Prevents OSPF from sending hello packets on the interface, This is ussually used for interfaces that are not connected to other OSPF routers, such as loopback interfaces or interfaces connected to end hosts.
```

#### Disable DR/BDR Election

```bash
Router(config-if)# ip ospf network point-to-point # Disables DR/BDR election on the interface
```

### Set OSPF interface priority

```bash
Router(config-if)# ip ospf priority <priority> # Default is 1, range is 0-255
Router# clear ip ospf process # Restart OSPF process to apply changes
```

### Cisco OSPF Cost metric

> Make sure to set the cost on all routers in the same area to avoid routing loops.

```bash
Router(config-if)# ip ospf cost <cost> # Default is 10000/bandwidth in kbps, range is 1-65535
Router# clear ip ospf process # Restart OSPF process to apply changes
```

OR

```bash
Router(config-if)# autocost reference-bandwidth <bandwidth> # Default is 100 Mbps, range is 1-4294967 Mbps, for 10 Gigabit Ethernet set to 10000 Mbps, and for 1 Gigabit Ethernet set to 1000 Mbps
```

### OSPF interval timers

```bash
Router(config-if)# ip ospf hello-interval <seconds> # Default is 10 seconds, range is 1-65535 seconds
Router(config-if)# ip ospf dead-interval <seconds> # Default is 40 seconds, range is 1-65535 seconds
```

### Propagate default route

```bash
# Set a default route on the router with interface to the internet
Router(config)# ip route <destination network> <subnet mask> <next-hop ip address> | <exit interface>
Router(config)# router ospf <process id> # OSPF process id should be the same in all routers in the same area, between 1 and 65535
Router(config-router)# default-information originate # Propagates the default route to OSPF neighbors
```

## ACL Configuration

> Keep in mind the **any** and **all** keywords. The any keyword is used to match any IP address, while the all keyword is used to match all IP addresses in the ACL.

### Standard IPv4 ACL

#### Numbered ACL

```bash
Router(config)# access-list <number> <permit/deny/remark(documentation text)> <source ip address> <wildcard mask> # Numbered ACL range is 1-99 and 1300-1999
# Router(config)# access-list <number> remark <documentation text> # Documentation text
```

#### Named ACL

```bash
Router(config)# ip access-list standard <name> # Named ACL range is 100-199 and 2000-2699
# Router(config-std-nacl)# permit <source ip address> <wildcard mask>
# Router(config-std-nacl)# deny <source ip address> <wildcard mask># Implicit deny all at the end of the ACL
# Router(config-std-nacl)# remark <documentation text> # Documentation text
```

---

### Extended ACL

#### Numbered ACL

```bash
Router(config)# access-list <number> <permit/deny/remark(documentation text)> <protocol> <source ip address> <wildcard mask> <destination ip address> <wildcard mask> [eq | gt | lt | neq | range] <port number> # Numbered ACL range is 100-199 and 2000-2699
```

#### Named ACL

```bash
Router(config)# ip access-list extended <name> # Named ACL range is 100-199 and 2000-2699
Router(config-ext-nacl)# permit <protocol> <source ip address> <wildcard mask> <destination ip address> <wildcard mask> [eq | gt | lt | neq | range] <port number>
```

#### TCP established

> The established keyword enables inside traffic to exit the inside private network and permits the returning reply traffic to enter the inside private network.

```bash
Router(config)# ip access-list extended <name> # Named ACL range is 100-199 and 2000-2699
Router(config-ext-nacl)# permit tcp <source ip address> <wildcard mask> <destination ip address> <wildcard mask> established # Allow established TCP connections
```

---

### Linking ACL to Interface

> The ACL must ALWAYS be applied to the interface; otherwise, it will not work.

```bash
Router(config)# interface <interface>
Router(config-if)# ip access-group <number/name> {in | out} # Inbound or outbound traffic
```

### Modify ACL

1. Copy the ACL
2. Modify the ACL
3. Delete the old ACL (no .....)
   - `Router(config)# no access-list <number>` - For deletting a whole ACL
   - `Router(config-std-nacl)# no <number>` - For deletting a ACE entry in a ACL
4. Paste the new ACL
5. Apply the new ACL to the interface

### Clear ACL statistics

```bash
Router# clear access-list counters <number/name> # Clear the statistics of the ACL
```

### Secure VTY Lines

```bash
Router(config)# username <username> secret <password> # Create a local user account with a password
Router(config)# ip access-list standard <name> # Create a standard ACL to allow access to the VTY lines
Router(config-std-nacl)# remark <documentation text> # Documentation text
Router(config-std-nacl)# permit <source ip address> <wildcard mask> # Allow access to the VTY lines from the specified source IP address
Router(config-std-nacl)# deny any # Deny access to the VTY lines from all other source IP addresses
Router(config-std-nacl)# exit # Exit the ACL configuration mode
Router(config)# line vty 0 4 # Access the VTY lines configuration mode
Router(config-line)# access-class <name> in # Apply the ACL to the VTY lines
Router(config-line)# login local # Use the local user account for authentication
Router(config-line)# transport input ssh # Allow only SSH access to the VTY lines
```

## NAT Configuration

```bash
show ip nat translations (verbose) # Show NAT translations
show ip nat statistics # Show NAT statistics
```

### Static NAT

```bash
Router(config)# ip nat inside source static <inside local ip address> <inside global ip address> # Static NAT configuration

Router(config)# interface <interface> # Inside interface
Router(config-if)# ip address <ip address> <subnet mask> # inside interface IP address configuration
Router(config-if)# ip nat inside # Inside NAT configuration

Router(config-if)# exit # Exit the interface configuration mode
Router(config)# interface <interface> # Outside interface
Router(config-if)# ip address <ip address> <subnet mask> # Outside interface IP address configuration
Router(config-if)# ip nat outside # Outside NAT configuration
```

### Dynamic NAT

```bash
Router(config)# ip nat pool <name> <start ip address> <end ip address> netmask <subnet mask> # Dynamic NAT pool configuration

Router(config)# access-list <number> permit <source ip address> <wildcard mask> # Configure ACL to permit only those adresses that will be translated
Router(config)# ip nat inside source list <number> pool <name> # Bind ACL to the NAT pool

#Inside interfaces
Router(config)# interface <interface> # Inside interface
Router(config-if)# ip address <ip address> <subnet mask> # Inside interface IP address configuration
Router(config-if)# ip nat inside # Inside NAT configuration

Router(config)# interface <interface> # Outside interface
Router(config-if)# ip address <ip address> <subnet mask> # Outside interface IP address configuration
Router(config-if)# ip nat outside # Outside NAT configuration
```

```bash
ip nat translation timeout <timeout-seconds>  # Set the timeout for NAT translations, default is 86400 seconds (24 hours)
clear ip nat translation * # To clear dynamic entries before the timeout has expired
```

### NAT overload (PAT)

#### For overloading single ip

```bash
Router(config)# access-list <number> permit <source ip address> <wildcard mask> # Configure ACL to permit only those adresses that will be translated
Router(config)# ip nat inside source list <number> interface <outgoing interface> overload # Bind ACL to the NAT overload (PAT) configuration

Router(config)# interface <interface> # Inside interface
Router(config-if)# ip address <ip address> <subnet mask> # Inside interface IP address configuration
Router(config-if)# ip nat inside # Inside NAT configuration

Router(config)# interface <interface> # Outside interface
Router(config-if)# ip address <ip address> <subnet mask> # Outside interface IP address configuration
Router(config-if)# ip nat outside # Outside NAT configuration
```

#### Overloading a pool

```bash
Router(config)# ip nat pool <name> <start ip address> <end ip address> netmask <subnet mask> # Dynamic NAT pool configuration
Router(config)# access-list <number> permit <source ip address> <wildcard mask> # Configure ACL to permit only those adresses that will be translated
Router(config)# ip nat inside source list <number> pool <name> overload # Bind ACL to the NAT pool and enable overload (PAT) configuration

interface <interface> # Inside interface
Router(config-if)# ip address <ip address> <subnet mask> # Inside interface IP address configuration
Router(config-if)# ip nat inside # Inside NAT configuration

Router(config)# interface <interface> # Outside interface
Router(config-if)# ip address <ip address> <subnet mask> # Outside interface IP address configuration
Router(config-if)# ip nat outside # Outside NAT configuration
```

## Switch Security

### Disabling unused ports

```bash
Switch(config)# interface range <interface1> - <interface2>
Switch(config-if-range)# shutdown # Disable the unused ports
```

### Port Security

> Limits the number of MAC addresses that can be learned on a port.
> This sets to manual mode, Default is dynamic mode.

```bash
Switch(config)# interface <interface>
Switch(config-if)# switchport mode access # Set the interface to access mode
Switch(config-if)# switchport port-security # Enable port security on the interface
```

```bash
Switch(config-if)# switchport port-security <option>
# Set the port security option
# <option> can be one of the following:
# - maximum <number> # Set the maximum number of MAC addresses that can be learned on the port
# - violation <option> # Set the action to take when a violation occurs; options are protect(drops packets no syslog), restrict (drops packets sends syslog ), or shutdown(default )
# - mac-address <mac address> # Set a specific MAC address to be allowed on the port
# - aging <option> # Set the aging time for the MAC address table
```

```bash
Switch# show port-security <interface > # Show port security configuration on the interface
Switch# show port-security address # Show the MAC address table for port security
```

### VLAN attacks mitigation

> For access ports, disable DTP

```bash
Switch(config)# interface <interface>
Switch(config-if)# switchport mode access # Set the interface to access mode on non-trunking ports
```

> For unused ports

```bash
Switch(config)# interface <interface>
Switch(config-if)# switchport mode access # Set the interface to access mode on non used ports
Switch(config-if)# switchport  access vlan <vlan number> # Set the access PARKING VLAN for the interface
Switch(config-if)# shutdown # Disable the unused ports
```

> For trunk ports

```bash
Switch(config)# interface <interface>
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan <vlan number> # Set the native VLAN for the trunk port to the not default VLAN
Switch(config-if)# switchport nongotiate # Disable DTP (Dynamic Trunking Protocol) on the trunk port
```

### DHCP Snooping

```bash
Switch(config)# ip dhcp snooping # Enable DHCP snooping globally
Switch(config-if)# ip dhcp snooping trust # Enable DHCP snooping on the trusted port (all the network device ports)
Switch(config-if)# exit # Exit the interface configuration mode
Switch(config)# ip dhcp snoopnig limit rate <rate-per-sec> # Set the rate limit for DHCP packets on the untrusted ports (all the user ports)
Switch(config)# ip dhcp snooping vlan <vlan number> # Enable DHCP snooping on the specified VLAN
```

```bash
Switch# show ip dhcp snooping # Show DHCP snooping configuration
Switch# show ip dhcp snooping binding # Show the DHCP snooping binding table
```

### Dynamic ARP Inspection (DAI)

> DHCP Snooping must be enabled before DAI.

```bash
# on trusted ports
Switch(config-if)# ip arp inspection trust # Enable DAI on the trusted port (all the network device ports)
```

> arp inspection on src mac, dest mac, and ip address

```bash
Switch(config)# ip arp inspection validate src-mac # Enable DAI on the source MAC address
Switch(config)# ip arp inspection validate dest-mac # Enable DAI on the destination MAC address
Switch(config)# ip arp inspection validate ip # Enable DAI on the IP address
Switch(config)# ip arp inspection validate src-mac dest-mac ip # Enable DAI on the source MAC address, destination MAC address, and IP address
```

### STP

> Only enable on end user ports.

#### Portfast

```bash
Switch(config)# interface <interface>
Switch(config-if)# spanning-tree portfast # Enable PortFast on the interface
Switch(config-if)# exit # Exit the interface configuration mode

# OR

Switch(config)# spanning-tree portfast default # Enable PortFast on all access ports
```

#### BPDU Guard

> When port received unexpected BPDUs, the port is disabled and put into err-disabled state.

> Interface

```bash
Switch(config)# interface <interface>
Switch(config-if)# spanning-tree bpduguard enable # Enable BPDU guard on the interface
```

> Global
> Enable shutdown port -> errdisable recovery cause psecure_violation

```bash
Switch(config)# spanning-tree portfast bpduguard default # Enable BPDU guard on all access ports
```
