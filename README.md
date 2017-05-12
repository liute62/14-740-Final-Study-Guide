# 14-740-Final-Study-Guide

## L17 PlugNPlayNetworking



## L18 Network Measurement

### purposes of network measurement
1. short-term
   * Detect hot-spots in network (congested linkes, overload routers)
   * Detecting attacks (DoS, worm propagation)
2. long-term
   * Traffic engineering (If many bits from Yahoo are observed, maybe negotiate a peering relationship)
   * Re-routing traffic (Change IGP metrics)
   * Upgrade links (e.g. from OC-48 to OC-192)
3. Others
   * Accounting
   * Research
### uses of basic measurement tools
+ packet traces
   * Raw data: header + data payload
+ SNMP counters
   * Simple Network Management Protocol (SNMP)
   * Simplistic view: asking router for info
   * Router has counters on link traffic: 1) Byte counts 2) Packet counts
   * Usages: 1) billing 2) graphing
   * Limitations: can't figure out 1) Types of traffic (Web, P2P, TCP) 2) Source of the traffic 3) Destination
+ ping
   * 
+ traceroute
   * 

### netflow tools vs measurement tools
+ flow definition

+ flow record

+ sampling methods

+ flow aggregation

### pick the most appropriate measurement tool



## L19 Congestion Control The Router’s View

### packet scheduling algorithms vs drop policies
+ Packet scheduling: ordering of transmission
   * FIFO
   * Priority Queuing: Routers use one queue for each priority (multiple queues in total)
   * Round Robin Queuing: Separate queue for each flow, if a flow send too fast, its own queue will fill up
   * Fair Queuing: Desired Bit-by-Bit round robin, but can't send a bit at a time : calculate transmission finishing time for each packet
+ Drop policy: which packet gets discarded
   * FIFO: drop tail
   * Priority Queuing: drop lowest priority first
   * Round Robin Queuing: Separate drop policy could be implemented
### algorithm and issues
+ FIFO
   * Doesn't discriminate
   * Relies on end-hosts to implement congestion control
+ Priority Queueing
   * High priority will starve low priority
+ Round Robin Queueing
   * A flower with bigger packets will get more bandwidth in a packet-by-packet round-robin router
+ Fair Queuing
   * **Work conserving** Link is never left idle
   * **max-min fairness** Maximizes the min data rate of any flow, no starvation of any flow
+ Weighted Fair Queueing
   * Assign a weight(priority) to each queue
   
### opportunities for a router to do congestion control
+ TCP needs to create loss to discover available bandwidth
+ Rounter knows congestion state and could signal end-hosts:
  * Randomly Early Drop(RED) gateways
  
### goals, details and advantages of the RED Gateway algorithm
#### Detail
+ Router-centric congestion control
  * still leverages TCP end-hosts
  * Active Queue Management (AQM) scheme
+ Decide which connection to nofity of congestion
  * **Randomly** chooses a packet ...
  * ... before buffer overflow, thus **early**
  * Chosen packet is **dropped** to inform sender
#### Goals
 + Reduce persistent queuing delay
 + Reduce unnecessary packet drops in some cases
#### Design
 + Avoid **global synchronization**
     * don't want all connections to back off and increase at the same time
 + Avoid bias against **bursty traffic** which otherwise uses low bandwidth
     * don't want to drop packets mostly from busty connections
#### Advantages
RED keep the router from Drop Tail regime
Problems about Drop Tail:
+ Biased against bursty connection
+ Global synchronization
#### issues
+ Probability of particular flow's packets being dropped is roughly proportional to the share of bandwidth
+ Can identify bad flows, but additional mechanisms on top of RED needed to deal with them (UDP)

#### Algorithm
1) avg < MINth add packet
2) MAXth <= avg drop the arriving packet
3) MINth <= avg < MAXth
   calculate probability Pa
   rand() < Pa ? if so: mark the arriving packet
Queue Length: 
* avg <- (1-B) * avg + B * q_length
Calculate Probability:
* pb <- MAXp * (Avg - MINth) / (MAXth - MINth) (nornamlly MAXp = 2%)
* pa <- pb / (1 - count * pb) count is the number of packets not dropped while Avg has been medium
* pa is the actual drop probability

## L20 Routing Instability

## L21 Link Layer; Ethernet

### Mission, scope, addressing mechanism, data types and responsibilities/services of the Data Link Layer
+ Mission: The **Data Link Layer** transfers frames from one node, over a link, to an adjacent node
    * service provided to network layer
+ Data type: frame
+ Services: Each data link protocol provides different services
    * Framing: encapsulate datagram into frame, identify source, destination with addresses
    * Link access: use medium access control (MAC) protocol (PoP or broadcast)
    * Error Detection: errors caused by signal attenuation, noise. retransmission or drops frame
    * Error Correction: receiver identifies and corrects bit error(s) without retransmission
    * Reliable Delivery: critical for wireless links -> high error rates

### Differences between broadcast and point-to-point links
**Links**: Communication channel that connects adjacent nodes (host or router)

+ Point-to-Point (sender to receiver)
  * point-to-point link between Ethernet switch and host
  * Point-to-Point Protocol used to negotiate, establish, authenticate, etc
+ Broadcast
  * traditional Ethernet
  * 802.11 wireless LAN

Router has multiple link layers -- Each link is a subnet

### Three different general types of media access protocols
1) Multiple access protocol: a distributed alrotihm that determines how nodes share channel
   * **collision** if node receives two or more signals at the same time
   * communication about channel sharing must use channel itself
+ Channel Partitioning
  * Divide the channel into pieces
  * Let each node use a piece of the channel
  * Problem, channel pre-setup, waste due to bursty network
+ Taking Turns
  * Like a time slot partition scheme, but nodes with more to send can take longer turns
  * Polling Protocol: Master node asks each node in turn to send
  * Token-passing: special-purpose frame is passed in fiexd order from node to node
+ Random Access
  * Deal with Collision when multiple nodes can send simultaneously
  * Collisions can be avioded or detected, for detected: CSMA/CD Protocols
  * CSMA/CD: Collision Detection: Listen as you talk, if you hear someone else, be quiet

### CSMA/CD protocol description and Ethernet's implementation
1) before transmitting, listen
2) if channel is sensed idle, send the frame
3) if collision is detected, abort transmission

Collision Detection
1) Easy in wired LANs
   * Measure signal strengths, compare the transmitted and received signals
2) Difficult in wireless LANs
   * Receiver is shut off during transmission
   
### Space-time diagrams to describe or solve problems relating to media access and Ethernet's implementation
1) See figure of slides page 23
2) Min transmission time must be long enough for collisions to propagate

### Ethernet frame format
1) Preamble: 8 bytes, at the begining, used to synchronize receiver, sender clock rates
2) Dest Addr: 6 bytes, 3 bytes indicate adapter manufacturer, 3 bytes generated uniquely
3) Source Addr: 6 bytes, ...
4) Type: 2 bytes: Mostly IP
5) Data: 46 - 1500 bytes, short datagrams padded to 46 bytes
6) CRC: 4 bytes: Error Checking

### solving interaction of several Ethernet senders and receivers, collisions, propagation times

## L22 Link Layer Devices

### Describe the Address Resolution Protocol
1) Mission: Translate a network-layer address (IP) to a link-layer address (MAC)
2) Frame Fields: Sender Ethernet and IP address | Target Ethernet and IP address
3) Transmission Mechanism: Broadcast or direcly response, and how about address of another subnet? 
4) Caching: 
5) Security: ARP frames are not authenticated.
6) Gratuitous Use: ARP can be an "announcement" protocol, can send an ARP frame just to update other node's ARP tables

### Describte repeater, hub, switch, and bridge
1) Repeater: echoes signal from one wire to another
2) Hub: echoes signal from one wire to many, at the same rate for all links
   * problem: Signals are heard everywhere, collisions happened
3) Switch/Bridge:  LAN bridge (standards speak) == LAN switch(industrial speak)
   * Stores and forwards frames
   * Selectively forwards the frame based on destination address

### Analyze and describe the self-learning capabilities of a switch for forwarding scenarios
1) A switch **filters** packets
   * same link frames not usually forwarded onto other links
2) links become separate **collision domains**

How does a switch determine which interface / link to forward a frame?

+ Self-Learning

Learns source ethernet address
1) if is in the forwarding table:
   * Lout = link of destination
   * if Lin == Lout then drop f
   * else forward f on Lout
2) else: flood //forward on all links except Lin

Avoiding Loops
+ Solution: create a spanning tree

### Describe the spanning tree protocol (IEEE 802.1D) & distributed aspects
+ Defined in IEEE 802.1D
+ Uses an edge weight based on interface data rate
  * 10 Mbps is cost 100
  * 1 Gbps is cost 4
+ Distributed aspects:
  * All root switch (candidates) think they are the root switch, then send a config message every 2 seconds, finally reach a stable stage

### Analyze a multi-switch/LAN scenario using the spanning tree protocol
?
1) Select switch to be the root of the tree
   * by lowest priority value
2) Determine **root port** for each switch
   * root port is one with the least-cost path to root switch
3) Select **desinated port** for each link
4) All switches disable all ports that are not designated or root ports

## L23 Virtual Link Layer

### describe the use of virtual LANs (VLAN) to allow multiple subnets to be connected with a single port-based switch.
Static VLAN: VLAN=Group of Ports

1) broadcast domain separation
   *  
2) flexibility for re-assigning hosts within the VLAN
   * 
3) connection mechanisms for when the same VLAN is connected across switches.
   * Trunked connection: port belongs to all VLANs -> all frames at that port are forwarded to all VLANs
   
### how link virtualization allows links to be more than just a simple "channel connecting adjacent nodes"

### diagram the encapsulation of messages inside segments inside packets inside frames
?
1) for ICMP
   
2) for ARP
   
### describe MPLS
MPLS Forwarding process don't examine the iP header, except at entry to MPLS network
1) advantages
   * IP routing is slowed by the variable length address searching in the forwarding table
   * MPLS replaces IP routing within a network by using a fixed length label
2) labeled frame formats
   * Ethernet Header | MPLS Header | IP Header
3) router operations
   * Inside the network: on receipt of packet, lookup label
   * Edge of the network: Incoming packet: convert IP to label. Outgoing packet: pop MPLS header.
4) describe what an MPLS forwarding table might look like
   

## L24 Wireless Networks	

### explain the challenges of a wireless subnet
1) range limits
2) mobility
   * Routing and addressing, how can a TCP flow sustain while user moves to a different subnet?
3) receiver shutdown
   * Receiver must be sensible
4) noise
5) multi-path
   * Receiver "sees" signal that is the sum of the line-of-sigh signal plus those from all other paths
6) hidden terminal
   * Two hosts may be prevented from hearing each other's transmissions
7) exposed terminal problems
   * A -> A', B -> B', one of them will shutup due to hear other guys talking

### Describe the CSMA/CA algorithm and how it helps overcomes some of the wireless challenges
Make collision happened as less
+ Listen for specific time
   * if medium is not free: Exponential Backoff
   * if medium is free: 1) Transmit **entire** frame 2) Await ACK frame 3) If no ACK, then Exponential Backoff
+ Reason:
   * Sender doesn't know if a collision happened with a hidden terminal

### Use of channel reservations to avoid collisions
+ A node can request access to the channel to ensure no collisions from hidden terminals
+ AP grants permission with a Clear to Send (CTS) frame
+ ALL nodes can hear the AP, even the hidden terminal situation.

### Some features of the 802.11 standard
1) operating modes
   * Ad-hoc: Nodes create network, but unattached to internet
   * Infrastructure: Access Point(AP) connect wireless subnet to wired network
   * Point-to-point: Use directional antennas to extend range
2) security
3) frame format
4) power management
   * Node can tell AP that it will go to sleep for short time period
   * AP will buffer frames until it awakes
5) rate adaptation
   * transmission rate can be tuned to the environment, as frames are lost, reduce rate

## L25 Software Defined Networking

### Describe the structure of a software-defined network
1) flow-tables(and actions therein)
2) controllers (and actions therein)
3) domains
