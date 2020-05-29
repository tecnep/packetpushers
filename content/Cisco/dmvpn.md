---
title: "DMVPN with EASE "
date: 2020-04-22T20:39:58+05:45
draft: true
author: "Sushant"
---

DMVPN OVERVIEW

DMVPN is a CISCO IOS Software Solution for building an easy,dynamic and scalable VPN solution with the help of NHRP, multipoint GRE, routing protocols like EIGRP,OSPF. DMVPN provides full meshed connectivity with simple configuration of HUB and SPOKE. DMVPN itself is not a protocol to be noted its just a design. A static GRE tunnel between HUB and SPOKE is always possible where we do have create multiple tunnels for each SPOKE we add . This may be a very useful solution for a small scale, it easily grows unwieldy as spokes multiply in number.

COMPONENTS of DMVPN

NHRP

mGRE

ROUTING PROTOCOLS


DMVPN PHASE I
~~~
HUB#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
HUB(config)#interface Tunnel1
HUB(config-if)# ip address 172.16.1.254 255.255.255.0
HUB(config-if)# no ip redirects
HUB(config-if)# ip mtu 1400
HUB(config-if)# no ip next-hop-self eigrp 123
HUB(config-if)# no ip split-horizon eigrp 123
HUB(config-if)# ip nhrp authentication cisco123
HUB(config-if)# ip nhrp map multicast dynamic
HUB(config-if)# ip nhrp network-id 123
HUB(config-if)# ip tcp adjust-mss 1360
HUB(config-if)# tunnel source FastEthernet4
HUB(config-if)# tunnel mode gre multipoint
HUB(config-if)# tunnel key 1234
HUB(config-if)# tunnel protection ipsec profile dmvpn
HUB(config-if)#end
HUB#
~~~






DMVPN PHASE II



We will now need to only add few commands from the above configured commands  for moving onto the DMVPN PHASE II .



HUB # int tun0

          #tunnel mode gre multipoint    (Mandatory)  (Required in case of DMVPN II and DMVPN III)

          #no ip next-hop self eigrp 123



SPOKE 1 # int tun0

           #tunnel mode gre multipoint    (Mandatory)  (Required in case of DMVPN II and DMVPN III)



SPOKE 2 # int tun0

           #tunnel mode gre multipoint    (Mandatory)  (Required in case of DMVPN II and DMVPN III)





DMVPN PHASE III



No NHRP Resolution request





HUB # ip nhrp redirect


SPOKE 1 # ip nhrp shortcut


SPOKE 2 # ip nhrp shortcut
