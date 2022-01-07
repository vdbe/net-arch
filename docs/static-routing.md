# Static Routing
From page 150 to 308

## Routing Journey
Example packet from PC1 (IP 192.168.1.2, MAC 01) to PC2 (IP 192.168.2.2, MAC 04)
with router and gateway (IP 192.168.{1,2}.1, MAX {02,03}
1. PC1 to Router
   - Source IPs: 192.168.1.2 (PC1)
   - Destination IP: 192.168.2.4 (PC2)
   - Source MAC: 01 (PC1)
   - Destination MAC: 02 (Router)
2. Router to PC2
   Router looks in routing table which port is connect to the next hop and decrements TTL
   - Source IPs: 192.168.1.2 (PC1)
   - Destination IP: 192.168.2.4 (PC2)
   - Source MAC: 03 (Router)
   - Destination MAC: 04 (PC2)
   
## Best path
- Routing tables contains info to allow informed decisions
  - Is network directly connected
  - metric: cost of the route
    - Lower better
    - Used to measure distance to a give network
  - trustworthiness of the route
- Routes can depend on the protocol
- No entry for next hop use "gateway of last resort"

Dynamic routing protocols use their own rules and metrics to
build and update routing tables for example:
- RIP: Routing Information Protocol
  - Hop Count
- OSPF: Open Shortest Path first
  - Cost based on cumulative bandwidth from source to destination
- EIGRP: Enhanced Interior Gateway Routing Protocol
  - Bandwidth
  - Delay
  - Load
  - Reliability
  
### Administrative Distance
If multiple paths to a destination are configured on a router, the path installed in
the routing table is the one with the best (lowest) Administrative Distance (AD).

Administrative Distance is the "trustworthiness" of the route.
The Lower the AD the more trustworthy the route.

Fixed values are used per routing protocol (see table, can be modified to manipulate routing decision's)

| Route source        | Administrative distance |
|---------------------|-------------------------|
| Direct connected    | 0                       |
| Static              | 1                       |
| EIGRP summary route | 5                       |
| External BGP        | 20                      |
| Internal EIGRP      | 90                      |
| IGRP                | 100                     |
| OSPF                | 110                     |
| IS-IS               | 115                     |
| External EIGRP      | 170                     |
| Internal BGP        | 200                     |

## Static routes
- Manually configured
- Define which network can be reached via which interface
- Must be manually updated if the topology (network layout) changes
- Improved security and control of resources
- `ip route networkmask {next-hop-ip | exit-intf/next-hop-ip}`

### Why
- Pros
    - Are not advertised over the network, resulting in better security
    - Less bandwidth
    - Less CPU cycles (No routes to calculate)
    - Route is known
- Cons
    - Initial configuration and maintenance is time-consuming
    - High risk of user errors in config (especially in large networks)
    - Administrator intervention required to maintain changing route information
    - Does not scale
    - Requires complete knowledge of the network to implement
    

### When
- Providing ease of routing table maintenance in smaller networks
  that are not expected to grow significantly
- Routing to and from stub networks
  - Stub network: A network accessed by a single route, and the router has no other neighbors
- Single default route to represent a path to any network that does not have a more specific
  match with another route in the routing table
  - Default route/gateway of last resort
- Backup route

## Floating static route
Floating static routes are static routes that are used to
provide a backup path to a primary static or dynamic
route, in the event of a link failure.

- Only used when primary route/link fails
- Manually set high administrative distance

## Next Hop
The next hop can be identified by an IP address, exit interface, or both. How the
destination is specified creates one of the three following route types
1. Next-hop route
   Only the next-hop IP address is specified
2. Directly connected static route
   Only the router exit interface is specified
3. Fully specified static route
   The next-hop IP address and exit interface are specified (1 and 2)
