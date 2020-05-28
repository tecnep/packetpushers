---
title: "OSPF Adjacency State "
date: 2020-04-22T20:39:58+05:45
draft: true
author: "Sushant"
---

OSPF has always been one of the most difficult dynamic routing protocols personally for me to understand if we try to deep dive into the protocol itself. In this blog let me briefly explain the different adjacency state that 2 routers go through before they exchange the route.

1. Down
   - No hello has been received from the neighbors.

2. Attempt
    - Unicast hello has been sent to the neighbor router but no hello has been received back yet

3. Init
    - Received hello packet from the neighbor but the other router has not acknowledged my hello.

4. 2-WAY
    - Hello packet has been exchanged and also the acknowledgment has been done.

5. Exstart
    - Master and Slave has been formed where master will always have the highest Router-ID
    -Master chooses the starting sequence number for the exchange of database descriptor packets that are used for actual LSA exchange.

6. Exchange
    - Link state database is sent with the help of Database packets and describes the content of link state database.

7. Loading
     - Based on the information provided by the DBD packets routers send link state request and gets back the requested link state information in link state update packets.

8. Full
    - Neighbors are fully adjacent and databases are synchronized.

  ![Cisco Image](/Cisco/OSPF.jpg)
