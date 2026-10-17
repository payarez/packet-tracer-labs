# Day 08: IPv4 Addressing & Interface Configuration

## 🎯 Overview

This lab covers fundamental IPv4 addressing, subnetting concepts across Class A, Class B, and Class C networks, interface description best practices, and gateway configurations to establish cross-network communication using a Cisco router.

## 📐 Network Topology

### Subnet Details

| Network / Color | Subnet / CIDR | Router Interface | Router IP (Gateway) | Connected Host IP |
| :--- | :--- | :--- | :--- | :--- |
| **Blue LAN** | `15.0.0.0/8` | `GigabitEthernet0/0` | `15.255.255.254` | PC1 (`15.0.0.1`) |
| **Green LAN** | `182.98.0.0/16` | `GigabitEthernet0/1` | `182.98.255.254` | PC2 (`182.98.0.1`) |
| **Yellow LAN** | `201.191.20.0/24` | `GigabitEthernet0/2` | `201.191.20.254` | PC3 (`201.191.20.1`) |

## 🛠️ Tasks & Step-by-Step Configuration

### 1. Assign Router Hostname

```
Router> enable
Router# configure terminal
Router(config)# hostname R1
```

### 2. Configure Interface IP Addresses & Descriptions

Configured appropriate IPv4 addresses, subnet masks, descriptive interface labels, and enabled each interface using `no shutdown`:

```
R1(config)# interface gigabitethernet 0/0
R1(config-if)# description Link-to-SW1-Blue-LAN
R1(config-if)# ip address 15.255.255.254 255.0.0.0
R1(config-if)# no shutdown
R1(config-if)# exit

R1(config)# interface gigabitethernet 0/1
R1(config-if)# description Link-to-SW2-Green-LAN
R1(config-if)# ip address 182.98.255.254 255.255.0.0
R1(config-if)# no shutdown
R1(config-if)# exit

R1(config)# interface gigabitethernet 0/2
R1(config-if)# description Link-to-SW3-Yellow-LAN
R1(config-if)# ip address 201.191.20.254 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit
```

### 3. Save Active Configuration

```
R1# copy running-config startup-config
```

### 4. End Host IP Configuration

Configured IP address settings and default gateways on each PC:

* **PC1:** IP `15.0.0.1` | Subnet Mask `255.0.0.0` | Gateway `15.255.255.254`
* **PC2:** IP `182.98.0.1` | Subnet Mask `255.255.0.0` | Gateway `182.98.255.254`
* **PC3:** IP `201.191.20.1` | Subnet Mask `255.255.255.0` | Gateway `201.191.20.254`

## 🔍 Verification & Testing

### Interface Status Verification

Executing `show ip interface brief` verifies that all configured interfaces are administratively `up` and operational (`up` status, `up` protocol):

```text
R1# show ip interface brief
Interface              IP-Address      OK? Method Status                  Protocol
GigabitEthernet0/0     15.255.255.254  YES manual up                      up      
GigabitEthernet0/1     182.98.255.254 YES manual up                      up      
GigabitEthernet0/2     201.191.20.254  YES manual up                      up      
```

### Connectivity Verification (Ping Test)

Tested inter-subnet routing by pinging PC2 and PC3 from PC1:

```text
C:\> ping 182.98.0.1
Pinging 182.98.0.1 with 32 bytes of data:
Reply from 182.98.0.1: bytes=32 time=1ms TTL=127
Reply from 182.98.0.1: bytes=32 time=1ms TTL=127

C:\> ping 201.191.20.1
Pinging 201.191.20.1 with 32 bytes of data:
Reply from 201.191.20.1: bytes=32 time=1ms TTL=127
Reply from 201.191.20.1: bytes=32 time=1ms TTL=127
```

## 📝 Key Takeaways

1. **Default Gateways:** End-user devices require a Default Gateway configured to route traffic outside of their local subnet.
2. **Interface Shutdown Default:** Router interfaces are disabled by default (`administratively down`) and require the `no shutdown` command to activate.
3. **Interface Descriptions:** Documentation within device configurations (`description <text>`) is crucial for administrative troubleshooting and organization in enterprise networks.