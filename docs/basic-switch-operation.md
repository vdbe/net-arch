# Basic Switch Operation
page 10 to 99

## boot
1. Post
2. Run  boot loader software
3. Boot loader does low-level CPU initialization
4. Boot loader initializes the flash filesystem
5. Boor loader locates and loads a default IOS operating system software image into memory and hands control of the switch over to IOS
### Locating IOS image
This is done trough the `BOOT` environment var if not set first executable found on flash (searched top to bottom).
`BOOT` var can be set with the `boot system` and inspected with `show bootvar`.


## Duplex vs Half-Duplex
- Duplex: Send **AND** receive, at the same time
- Half-Duplex Send **OR** receive

### commands
```ios
S1(config-if)# duplex full
S1(config-if)# duplex auto
S1(config-if)# duplex half # I think
```

## MDIX auto
MDIX: **M**ediom-**D**ependent **I**nterfece **C**rossover
- Detects if cable is straight-through or crossover
- needs `speed auto` and `duplex auto` to work
### Enable
```ios
S1(config-if)# duplex auto
S1(config-if)# speed auto
S1(config-if)# mdix auto
```

## MAC vs IP
**MAC**
- Does not change
- Known as physical address assigned to the host NIC (Network Interface Card)

**IP**
- Based on where the host is located, can change
- Known as logical address because assigned logically

Both are required for routing

## ARP
ARP: Address Resolution Protocol

[comment]: <> (TODO: Explanation of ARP)

## CAM table
CAM: Content Addressable Memory

- Cached MAC addresses on each Port
  - Speed up
  - Stored inside the switch
- Floods all ports (except source) with the frame if no match is found

## Layer 2 vs 3
### 2
- Can only communicate within subnet
- Plug and play

### 3
- Can route to other subnets

## Line VTY 0 4
- Each remote user uses a 'Virtual TTY' line
- `Line vty 0 4` allows for lines [0-4] to be used: 5 users at the same time

## Security Threads
### MAC address flooding
Evil actor fills CAM table with fake MAC address, this means all frames will be flooded to all ports.
Attacker now receives frames meant for other devices.

[comment]: <> (TODO: mitigation?)

### DHCP Spoofing
Two types
1. DHCP spoofing
A fake DHCP server is placed on the network to confuse devices
2. DHCP starvation
Used before spoofing to deny valid service to the  valid DHCP server

### CDP abuse
CDP (Cisco Discovery Protocol) is a layer 2 Cisco proprietary protocol used to 1 other
Cisco devices that are connected and auto-configure their connection.

An attach can listen to CDP message to gather device models, versions and so on.
Disable CDP when its not needed anymore.

### Telnet vulnerabilities
- Telnet is insecure **USE SSH!!!**
- Password brute force
- Denial of service
- Passwords can be captured

## Security
### Secure unused ports
Disable them
```ios
S1(config)# interface range fastEthernet0/1-24,gigabitEthernet0/1-2
S1(config-if-range)# switchport mode access
S1(config-if-range)# switchport access vlan 999
S1(config-if-range)# shutdown
```

### DHCP snooping setting
Specify what ports can respond to DHCP requests or limit the amount of request it responds to
```ios
S1(config)# ip dhcp snooping
S1(config)# ip dhcp snooping vlan 10,20
S1(config)# interface fastethernet 0/1
S1(config-if)# ip dhcp snooping trust
S1(config)# interface fastethernet 0/2
S1(config-if)# ip dhcp limit rate 5
```

### Used Port security

[comment]: <> (TODO: Add commands for all actions below)
#### MAC Address
   - Static: manually configured list
   - Dynamic: Limit amount of diffrent MAC addresses (cleared on boot)
   - Sticky: Dynamic but not cleared on boor
   - MAC move: MAC address goes to another port on the same VLAN -> security violation

### Violation mode
IOS considers a security violation when either of these situations occurs:
1. The maximum number of secure MAC addresses for that interface have been added to the CAM,
    and a station whose MAC address is not in the address table attempts to access the interface.
2. An address learned or configured on one secure interface is seen on another secure interface in the
    same VLAN.
    
There are 3 possible action to be taken when a violation is detected:
1. Protect
   Drops packets with unknown source address until you remove a sufficient number of secure MAC addresses.
2. Restrict
   Same as Protects but also causes a _SecurityViolation_ counter increment
3. Shutdown
   Put interface in the error-discable state and sends an
   SNMP (Simple Network Management Protocol) trap notification

## Monitoring/logging
For efficient monitoring and logging of infrastructure make sure all date/time settings are correct.
USE **NTP** for this
### NTP
NTP: Network Time Protocol
- Used to sync clocks across the network
- Gets correct time from inter/external source (So it just gets it from somewhere)
- Sources can be
  - Local master clock
  - Master clock on the Internet
  - GPS or atomic clock
- A network device can be an NTP server or an NTP client
## 10 Best Security Practices
0. Develop a written security policy for the organization
1. Shut down unused services and ports
2. Use strong passwords and change them often
3. Control physical access to devices
4. Use HTTPS instead of HTTP
5. Perform backups operations on a regular basis.
6. Educate employees about social engineering attacks
7. Encrypt and password-protect sensitive data
8. Implement firewalls.
9. Keep software up-to-date

