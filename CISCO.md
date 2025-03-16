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
