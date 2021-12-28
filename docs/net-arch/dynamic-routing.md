# Dynamic Routing
from page 309 to 418

## Purpose of Dynamic Routing Protocols
- Used to facilitate the exchange of routing information between routers
- Discovery of remote networks
- Maintaining up-to-date routing information
- Choosing the best path to destination networks
- Ability to find the best path if the current path is down

Main components of dynamic routing protocols include:
- Data structures
  Tables or databases kept in RAM
- Routing protocol messages
  Various types of messages to discover neighboring routers, exchange routing information,
  and other tasks to learn and maintain accurate information about the network
- Algorithm
  Facilitating routing information for best path determination
  
### Advantages
- Automatically share information about remote networks
- Determine the best path to each network and add this information to their routing tables
- Compared to static routing, dynamic routing protocols require less administrative overhead
- Help the network administrator manage the time- consuming process of configuring and maintaining static routes

### disadvantages
- Dedicate part of a routers resources for protocol operation, including CPU time and network link bandwidth


## How
In general, the operations of a dynamic routing protocol can be described as follows:
1. The router sends and receives routing messages on its interfaces
2. The router shares routing messages and routing information with other routers that are
   using the same routing protocol
3. Routers exchange routing information to learn about remote networks
4. When a router detects a topology change the routing protocol can advertise this change to other routers

## Routing protocols

[comment]: <> (TODO: Page 312 to 335)

### Convergence A network has converged when all routers have complete and accurate information about the entire network

- Convergence time
  The time it takes routers to share information, calculate best paths, and update their routing tables
- A network is not completely operable until the network has converged

Convergence properties include the speed of propagation of routing information and
the calculation of optimal paths. The speed of propagation refers to the amount of time it takes
for routers within the network to forward routing information
- Older protocols converge slower
- Modern faster

### Link-state Protocols
Each router:
- learns about each of its own directly connect networks
- is responsible for "saying hello" to its direct neighbors
- builds a Link State Packet (LSP) containing that stage of each directly connect link
- floods the LSP to all neighbors wo sore all the LSP's received in a DB
  NOTE: All or just using OSPF?
- uses the database to construct a complete topology map for the best path to each network

[comment]: <> (TODO: Page 351)

Advantages:
- Each router has its own map
- Fast convergence
- Only updates when something changes
- Can be implemented using multiple zones/areas

Disadvantages:
- Memory
- Processing
- Bandwidth

**Why use areas**
To minimize impacts when something goes wrong

### OSPF
#### Packet types
| Type | Packet Name                | Description                                               |
|------|----------------------------|-----------------------------------------------------------|
| 1    | Hello                      | Discovers neighbors and builds adjacencies between then   |
| 2    | Database Description (DBD) | Check for database synchronization between routers        |
| 3    | Link-State Request (LSR)   | Requests specific link-state record from router to router |
| 4    | Link-State Update (LSU)    | Sends specifically requested-link state records           |
| 5    | Link-Sate Acknowledgment   | the other packet types                                    |


## Link-Local addresses
- FF02::5 address is the all OSPF router address
- FF02::6 is the DR/BDR multicast address

- Automatically created when an IPv6 global unicast address is assigned to the interface (required)
- Global unicast address are not required
- Cisco routers create the link-local address using FE80::/10 prefix and the
  EUI-64 process unless the router is configured manually
  - EUI-64 involves using the 48-bit Ethernet MAC address, inserting FFFE in
    the middle and flipping the seventh bit. For serial interfaces, Cisco uses
    the MAC address of an Ethernet interface.
    
## The routing table
### Terms
- Ultimate route

   -Routing table entry that contains next-hop IP address or an exit

  - Directly connect, dynamical learned and link local routes are ultimate routes
- Level 1 route
  - route with mask <= classful mask
  - Directly connect, static or dynamic routing protocol
- Level 1 parent route

    automatically added to the routing table whenever a subnet (child route) with a
    mask > classful mask is added to the routing table

- Level 2 child route
  - route with subnet mask > classful mask
  - Directly connect, static or dynamic routing protocol
  
### Lookup process
Best Route = Longest match
1. look in level 1 routes for best match
    - If it is a parent route: go to step 2
2. look in child routes for best match
    - If match found: FORWARD PACKET
    - If no match found in child routes: go to step 3
3. is router classful or classless?
    - If it is classful: DROP PACKET
    - If it is a classless: keep looking in level 1 routes for match
4. If lesser match found in level 1 routes
   - FORWARD PACKET
   - DROP PACKET

[comment]: <> (TODO: classful vs classless)

## Comparison
Distance vector
  - Slow convergence
  - Keeps sending update at regular intervals
    - Even if nothing changes

Link-state
  - Faster convergence
  - Only sends updates in case of network topology changes
  - Might give heavy network traffic due to flooding of update packets

|                    | Distance Vector | -      | -      | -       | Link State | -       |
|--------------------|-----------------|--------|--------|---------|------------|---------|
| PROTOCOL           | RIPv1           | RIPv2  | IGRP   | EIGRP   | OSPF       | IS-IS   |
| Speed convergence  | Slow            | Slow   | Slow   | Fast    | Fast       | Fast    |
| Scalability        | Small           | Small  | Small  | Large   | Large      | Large   |
| Use of VLSM        | No              | Yes    | No     | Yes     | Yes        | Yes     |
| Resource Usage     | Low             | Low    | Low    | Medium  | High       | High    |
| impl & maintenance | Simple          | Simple | Simple | Complex | Complex    | Complex |
