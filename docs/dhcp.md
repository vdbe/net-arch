# DHCP

##  DHCPV4
3 different address allocation methods:
1. **_Manual_**  
   Administrator assing __pre-alloacted__ address to the client, DHCP communicates only the address to the device
2. **_Automatic Allocation_**  
   Automatically assings a static IPv4 address __permanently__ to a device, selectin it from a pool of
   avaible devices. __No lease__
3. **_Dynamic allocation_**  
   __dynamically assigns, or leases__ an address from a pool of addresses for a limited period of Wed Dec 29 14:03:09 2021chose by the server, or until the client no longer needs the address. (Most used method)
   
### Discovery
1. DHCP client sends a direct IP broadcast with a DHCPDISCOVER packet
2. Lots of stuff
3. Server Responds

### Relay
- Centralized DHCP server
- Destioniation IP address is changed for the discover packet


## DHCPv6
3 methods
1. SLAAC only
   - Device uses prefix, prefix-length adnd default gateway from RA
   - No info from DHCPv6 server
     - Interfeace ID wil be auto-generated
2. SLAAC and DHCPv6 (stateless DHCPv6)
   - Device uses prefix, prefix-length adnd default gateway from RA
   - info such as DNS server from DHCPv6 server
   - Known as **stateless DHCPv6** because DHCPv6 does not need to track clients/addresses
   - Interfeace ID wil be auto-generated
3. DHCPv6 only
   - Devices **does not** use info from RA message (for its addressing info)
   - The device uses the normal process of discovering and querying a DHCPv6 server
   - Get all info from DHCPv6 server: global unicast, prefix length, default gateway, DNS server
   - Server keeps track of IPv6 addresses
     
     
### Server types
- Stateful DHCP  
  - Keeps track ow which clients have been assigned which IPv6 addresses (state information)
- Stateless DHCP  
  - Server **does not** lease IPv6 addresses to clients
  - Server provides other useful information such as DNS server but does not keep track of clients

### SLAAC
SLAAC: Stateless Address Auto Configuration

- Feature of IPv6 in which a host or router can be assigned an IPv6 unicast address without the need for 
  a stateful DHCP server
- Obtains prefix, prefix length and default gateway from an IPv6 router without the use of a DHCPv6 server
  - relies on the local __router’s ICMPv6 Router Advertisement (RA)__ messages to obtain the necessary information
  
#### SLAAC RA
RA: Router Advertisement

IPv6 routers periodically send out ICMPv6 Router Advertisement (RA) messages to all
IPv6-enabled devices on the network

A client can also request the information by sending a ****RS (Router Solicitation) message** to all IPv6 routers
using multicast address FF02::2 (All routers on subnet) to which the routers respond with an RA message

#### Interface ID
If no interface ID is provided by a DHCP server, 2 methods are used to generate one:
1. Randomly generated
2. EUI-64  
   Host divides its own MAC address into two 24-bits halves. Then 16-bit Hex value 0xFFFE is sandwiched
   into those two halves of MAC address, resulting in 64-bit Interface ID
   MAC: `00 11 22 AB CD EF` -> Interface ID: `00 11 22 FF FE AB CD EF`
   
- Global unicast address  
  When using SLAAC, the device receives its prefix from
  the ICMPv6 RA and **combines it** with the **Interface ID**
- Link-local address  
  - begins with prefix FE80::/10
  - followed by Interfeace ID (This is not always the case)
  
### Relay
- Router Solicitation (RS) is sent to multicast address
- Router translates it to unicast address
