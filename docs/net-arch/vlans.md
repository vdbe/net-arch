# VLANS
Page 100 to

## Broadcasting
Send a packet to all devices on the same network.
Used for when the destination is unknown:
- ARP
- DHCP discovery
- Service discovery in general

To many broadcast can flood a network

## VLANS
VLAN: Virtual LAN
- Logical partition of a layer 2 network
- Multiple partitions can be created, allowing for multiple VLANs to co-exist
- Each VLAN is a broadcast domain, usually with its own IP network
- VLANS are __mutually isolated__ and packets can only pass between them through a router
- The hosts grouped within a VLAN are unaware of the VLAN’s existence
- VLANs can be defined in a layer 2 device (switch) or layer 3 device (router)

There are 2 ranges of VLANs
1. Normal range
   - [1-1005]
   - Configurations stored in `vlan.dat` (flash)
   - VTP only uses this range
2. Extended Range
   - [1006-4096]
   - Configurations stored in the running-config `nvram`
   - Does not support VTP

### VLAN types
- Data VLAN
  Data VLAN or user VLAN is for User-generated traffic only
- Default VLAN (VLAN 1)
  - Factory default: all ports are by default a member of this vlan
  - Can not be renamed or deleted
  - Carries all layer 2 control traffic (best practice: limit VLAN 1 to this traffic only)
- Black-hole VLAN
  - Collection of unused ports
- Management VLAN
  - Default: VLAN 1
  - Switch management traffic
  - Change this!!!
- Native VLAN
  - All untagged traffic (see VLAN trunk)
  - For backwards compatibility
- PVLAN
  - Private VLAN
  - No exchange of unicast, broadcast, or multicast traffic between protected ports on the switch
  - A protected port only exchanges traffic with unprotected ports
  - A protected port will not exchange traffic with another protected port
  
### VLAN Trunks
- Carries more than one VLAN
- Usually established between switches so same-VLAN devices can communicate even
  if physically connected to different switches
- Not associated to any VLAN.
- Both sides need to have the same settings

[comment]: <> (TODO: Page 121 some info about standards)

#### Tagging
When different VLANs are combined into a trunk link, each frame needs to be **tagged**. Meaning that
information is added to each from about for/from what VLAN it is

- No needed on non-trunk links
  Here is the switch the only device aware of the vlan (via the port)
- Tags are placed when entering a trunk an removed when entering an non-trunk port
- Diagram of where a tag is on page 126

[comment]: <> (TODO: Add img)

### Commands
[comment]: <> (TODO: list of all commands needed)

### DTP
DTP: Dynamic Trunking Protocol
- Switch ports __negotiate and establish a trunk link__
- Both switches need to support en enable DTP

[comment]: <> (TODO: Trunk modes page 155)

### VTP
VTP: Vlan Trunking Protocol
only for normal range

A protocol to sync vlan creation, deletion, names across a trunk
3 modes:
1. Client
   - A switch using this mode can’t change its VLAN configuration (creation, deletion, name)
   - Receives, forwards and applies VTP updates
2. Server
   - Default mode
   - A switch using this mode can create and delete VLANs
   - Will propagate VLAN changes
3. transparent
   - Just forwards the data

### Attacks
#### VLAN hopping
Attacker acts as a switch and tries to establish a trunk with the switch.
This works because bu default a port is set to `dynamic auto`. TURN OFF TRUNKING ON PORTS THAT DON'T NEED IT

#### Double-Tagging
https://sid-500.com/2017/10/31/cyber-security-vlan-double-tagging-hopping-attacks-explained/
!!!Don't set an access port to the native VLAN
