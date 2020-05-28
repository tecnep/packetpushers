---
title: "SNAT and Auto Map"
date: 2020-04-22T20:39:58+05:45
draft: false
---

This is going to be my first blog in the technology that I am starting  to love pretty much. Yes, as my topic suggests we are going to talk about an technology which all the time used to come in my mind as a loadbalancer but finally I started stating it as a Application Delivery Controller which it really is . Big IP F5 has become one of the hot topics over the years that provides intelligent traffic management, high availability of your critical applications, security, optimization and many more things. I will be talking more about Big IP F5 features in my next blog.

Let me now be more specific and talk bit about what SNAT and Automap really does in Big IP F5. SNAT and Automap is all about changing the original source ip address before F5 sends the traffic towards the application server. This article will be more focused on the incoming traffic technically specifying  what we call it as a Destination NAT.

![F5 image](/F5/f5.png)

Let's assume a client with an IP of 10.10.254.10 wants to access our web application (www.sushant.com.np) which is being hosted in 3 different back end servers and resides behind the Big IP F5.  The client will firstly be initiating a traffic towards the Big IP F5 Virtual Server IP which being 192.168.1.155 in our scenario.  Inside the Virtual Server there will be pool members (172.16.1.2, 172.16.1.3, 172.16.1.4) where we have installed application for sushant.com.np . Big IP F5 will be then performing the selected load balancing algorithm and distribute the traffic accordingly to the multiple application servers . So, in order to reply the traffic back to the client the application server  will first check its local routing table and if it does not find the exact match in its routing table it will be sending the traffic towards the GW (172.16.1.1) which in our case will be the Router . This will result in Asymmetric routing and the client will simply not accept the traffic or even the stateful firewall in the client's end can simply drop it   as the Destination IP when the client initiated was 192.168.1.155  and the reply the client will be receiving is from different IP which being 172.16.1.2, 172.16.1.3 or can also be 172.16.1.4 (according to the load balancing algorithm).

F5 have created a work around for this kind of issue . The first solution would simply be using the F5 as a gateway and forwarding the traffic accordingly to the routers.  The other solution to this problem is SNAT and Automap which simply changes the Source IP address and avoids asymmetric routing to happen. I will only be talking about SNAT and automap in this article so I will be writing another blog for F5 being used as a gateway.

So how does SNAT work?
For every incoming request from the client towards  F5 Virtual IP, SNAT simply changes the original source IP (10.10.254.10) of the client to Internal Self IP (172.16.1.155)  and forwards the packet to the application server. The application server will then simply reply back to the Internal Self IP and there won't be any  issues. Automap is similar to SNAT and simply selects the internal Self IP automatically. So it may come in your mind what is the difference between Automap and SNAT ?  SNAT can use a pool of address whereas automap simply takes the Internal Self IP which can lead to port exhaustion as one IP can only allow around 65,000 connection at a single time.

Let me simply put down all the request and response IP , port in a table which will make my above statement more clearer and easier to understand.





This is more of a use when the application server  wants to make use of internet for downloading some patch or some application then in this scenario they will be directly forwarding the packets towards the router and then the router to the internet . The traffic will simply not go through the F5 as the BIG IP F5 is mainly used for incoming traffic processing for our mission critical applications residing behind it. The use of SNAT, Automap and F5 being the gateway all depends upon the scenario and requirements of the client. This blog is getting too long I will be writing more about F5 in the coming days. Please, do comment if you have any queries as well as suggestion will be well appreciated.

Thanks !
