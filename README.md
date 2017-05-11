# 14-740-Final-Study-Guide


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
1) Mission
2) Frame Fields
3) Transmission Mechanism
4) Caching
5) Security
6) Gratuitous Use

### Describte repeater, hub, switch, and bridge

### 

