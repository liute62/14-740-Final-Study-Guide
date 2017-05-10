# 14-740-Final-Study-Guide


## Network Measurement

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



## Congestion Control The Router’s View

### packet scheduling algorithms vs drop policies
