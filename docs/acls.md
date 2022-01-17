# Access Control Lists
Packet filtering, sometimes called static packet filtering, controls access to a network by
analyzing the incoming and outgoing packets and passing or dropping them based on given criteria,
such as the source IP address, destination IP addresses, and the protocol carried within the packet.

- A router acts as a packet filter when it forwards or denies packets according to filtering rules
- An ACL is a sequential list of permit or deny statements, known as access control entries (ACEs)

- _**The last statement of an ACL is always implicit deny**_

## Types of ACLs
- IPv4
  - Standard
    - Numbered
    - Named
  - Extended
    - Numbered
    - Named
- IPv6
  - Named  
    similar IN functionally to IPv4 Extended

## Standard vs Extended
Standard:
- Filters IP packets based on the source address only
- [1-99] & [1300-1999]: Numbered ACL

Extended:
- Filters on:
  - Source en destination IP
  - Source and destination TCP/UDP ports
  - Protocol type/number (IP, ICMP, UPD, TCP)
- [100-199] & [2000-2699]: Numbered ACL

## Numbered vs Named ACLs
### Numbered
- [1-99] & [1300-1999]: Standard IP ACL
- [100-199] & [2000-2699]: Extended IP ACL

### Named ACL
- Names can contain alphanumeric characters but no spaces
- It's suggested that the name be written in CAPITAL_LETTERS
- You can add or delete entries within the ACL


## Wildcard masking
A wildcard mask is often referred to as an inverse mask.

## General rules
- Use ACLs
  - in __firewall routers__ positioned between your internal network and an external network
  - on a router positioned __between two parts of your network__ to control traffic entering or exiting a specific part of your internal network

- Configure ACLs
  - on __border routers__, routers at the edge of your networks
  - for each network protocol configured on the border router interfaces
  
### The 3 Ps
One ACL per
- protocol  
  To control traffic flow on an interface, an ACL must be defined for
  each protocol enabled on the interface

- direction  
  ACLs control traffic in one direction at a time on an interface. Two
  separate ACLs must be created to control inbound and outbound traffic

- interface  
  ACLs control traffic for an interface
  
### Where
- Extended ACLs  
  __As close as possible to the source__ of the traffic to be filtered
- Standard ACLs  
  standard ACLs do not specify destination addresses: __as close to the destination as possible__
  
Placement of the ACL and therefore the type of ACL used may also depend on: the extent of the
network administrator’s control, bandwidth of the networks involved, and ease of configuration.

## Standard ACL sequence numbers

[comment]: <> (TODO: Don't understand Page 451 to 452)

## Configuring ACls

[comment]: <> (TODO: Page 452 to 464)

[comment]: <> (TODO: Highlighted text on page 452)

## Inbound logic
- packet matches an ACL statement with a permit: it is routed
- packet matches an ACL statement with a deny: it is dropped
- packet does not match anything: implicit deny -> dropped

## Outbound logic
- interface no ACL: sent
- matches permit: sent
- matches deny: dropped
- does not match anything: implicit deny -> dropped

## Logic Operation
[comment]: <> (TODO: page 468-471)

## Extended ACL descision process
check:
1. source address
2. port and protocol of source
3. destination address
4. port and protocol destination
5. makes a final permit or deny decision?

## ACLs on VLANs
- Inbound  
  Will be matched against all packets between VLAN xx and all devices __connected to__ VLAN xx

- Outbound  
  Will be matched against all packets between VLAN xx and all devices outside of VLAN xx
  
## IPv6 VS IPv4 ACLs
There are three significant differences between them
1. applying ACL
   - IPv6 uses `ipv6 traffic-filter`
2. No Wildcard masks
   - Prefix-length is used to indicate how much of an IPv6 source or destination address should be matched
3. additional default statements
   - nd-na (neighbor discovery acknowledgment)
   - nd-ns (neighbor discovery solicitation)
