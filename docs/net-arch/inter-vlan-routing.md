# inter-VLAN routing
from page 178 to 249

## why
Connection between devices on separate networks

## Default gateway
When a packet has a destination IP outside of the
network of the source IP, it is sent to the default gateway
- Source address is replaced with the IP of the default gateway on the other side
- Layer 3 device (can be a layer 3 switch)

## Inter-vlan routing
### Before
Before switches had layer 3 functionality, a router had to be used to connect VLANs
- Each vlan connected to a different NIC
- Packet arrive on the on the router through one interface get routed and leave through another
- Router had IP on each VLAN
- Very inefficient use of resources, not scalable

Alternatively, a trunk connection can be made between switch and router,
this is called a "router-on-a-stick"
- One of the router’s physical interfaces is configured as a 802.1Q trunk port
- One logical subinterface per VLAN each with their own IP
  - These are set as default gateway for devices on that VLAN
- Only one port of the router is used

### Layer 3 Switches
Layer 3 switches can route between VLANs **if** is routing is enabled,
Each VLAN existent in the switch is an SVI.
An SVI (Switch Virtual Interface) are seen as layer 3 interfaces. This way the switch
can route between between those interfaces within the switch.

All Catalyst switches support two types of Layer 3 interfaces:
1. Routed port
   Just like a router, the port has an IP address/mask that makes it a member of that subnet
   - Physical port
   - No layer 2 functionality
   - Doesn't belong to any VLAN
   - Typically used when only 1 host is connected to the network on the port
   - faster
   ```ios
   S1(config)# interface GigabitEthernet 0/2
   S1(config-if)# no switchport
   S1(config-if)# ip address 10.10.50.1 255.255.255.0
   ```
2. SVI: Switch Virtual Interface
   The switch is a member of that IP subnet/VLAN. All switch port that are
   a member of that VLAN can communicate with the switch
   ```ios
   S1(config)# interface vlan 10
   S1(config-if)# ip address 10.10.50.1 255.255.255.0
   S1(config-if)# exit
   S1(config)# interface GigabitEthernet 0/2
   S1(config-if)# switchport access vlan 10
   ```

### Routing Table
Used to store information about which network is reachable via which network interface on the routing device

### Switched Network Design
- As network technologies evolved, __routing became faster and cheaper__
- Today, _routing can be performed at hardware speed_
- Once consequence of this evolution is that __routing can be brought
  down to the core and the distribution layers without impacting network performance__
- Because many users are in separate VLANs, and each VLAN is usually a separate subnet, it is __logical
  to configure the distribution switches as Layer 3 gateways for the users of each access switch VLAN__
- This implies that __each distribution switch must have IP addresses matches each access switch VLAN__
