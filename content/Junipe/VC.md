---
title: "Virtual Chassis "
date: 2020-04-22T20:39:58+05:45
draft: false
author: "Sushant"
---
Juniper has always been an technology that I have always liked right from the beginning of my career when I was working as an Jr Network Administrator. The logical command as well as the hierarchy based Juniper devices has always made me love the device.

Let me today talk about a very useful and an interesting topic which we usually called stack in the world of Cisco and virtual chassis in the world of Juniper, both of them mean the same thing logically make two or more than two switch as a one and configure and manage the device as a single unity. High Availability, managed configuration and maintenance are few of the benefits that a virtual chassis can provide. The configuration for virtual chassis can be found easily in the juniper sites but my objective about writing this is making it more simpler in context to Juniper's document.

There are basically two ways of configuring virtual chassis in Juniper.
 >Nonprovisioned configuration : The master switch assigns a priority value and the role is determined by the mastership priority value.

 >Preprovisioned configuration : The role is determined by the serial number of the corresponding devices.
I will be configuring it in two EX 3400 switches in this particular blog.


### PREPROVISIONED CONFIGURATION
We simply need to power on of the first switch and the first thing we need to decide is whether we will be using the dedicated virtual chassis port or Juniper also offers us the privilege of using the uplink port available. In this particular example I had used the SFP + uplink ports which will require and additional configuration. The uplink port will now be needing to convert it to the virtual chassis port.

~~~
root@lab-ex3400>  request virtual-chassis vc-port set pic-slot 2 port 0
~~~

In my case I was using the pic slot 2 and the port being 0. After using this command we have enabled the port to be used as a virtual chassis port.
~~~
root@lab-ex3400> show chassis hardware
Hardware inventory:
Item             Version  Part number  Serial number     Description
Chassis                                NX0219270070      Virtual Chassis
Routing Engine 0          BUILTIN      BUILTIN           RE-EX3400-48T
Routing Engine 1          BUILTIN      BUILTIN           RE-EX3400-48T
FPC 0            REV 25   650-059881   NX0219270070      EX3400-48T
  CPU                     BUILTIN      BUILTIN           FPC CPU
  PIC 0          REV 25   BUILTIN      BUILTIN           48x10/100/1000 Base-T
  PIC 1          REV 25   650-059881   NX0219270070      2x40G QSFP
  PIC 2          REV 25   650-059881   NX0219270070      4x10G SFP/SFP+
    Xcvr 3                NON-JNPR     CMD9K000087       SFP+-10G-SR
  Power Supply 0 REV 05   640-060603   1EDV9190NFT       JPSU-150W-AC-AFO
  Fan Tray 0                                             Fan Module, Airflow Out (AFO)
  Fan Tray 1                                             Fan Module, Airflow Out (AFO)
  ~~~
  In this particular blog we will be using the preprovisioned mode so we will need to specify it.

   ~~~
   {master:0}[edit]
root@lab-ex3400# set virtual-chassis preprovisioned
{master:0}[edit]

root@lab-ex3400# set virtual-chassis no-split-detection
~~~
Specify all the members that you want included in the Virtual Chassis, listing each switch’s serial number with the desired member ID and role.
~~~~
{master:0}[edit]
root@lab-ex3400# set virtual-chassis member 0 role routing-engine serial-number NX0219270207

{master:0}[edit]
root@lab-ex3400# set virtual-chassis member 0 role routing-engine serial-number NX0219270207
~~~~

### VERIFICATION
~~~
master:0}[edit virtual-chassis]
root@lab-ex3400# show
preprovisioned;
no-split-detection;
member 0 {
    role routing-engine;
    serial-number NX0219270207;
}
member 1 {
    role line-card;
    serial-number NX0219270070;
~~~
We will need to now power on the second device and the first we need to do is convert the uplink port to the virtual chassis port using the command.

~~~
root@lab-ex3400>  request virtual-chassis vc-port set pic-slot 2 port 0
~~~
After the conversion we will now need to plug in the SFP and the cable to the other switch participating in virtual chassis. We can also verify the virtual chassis port.
~~~
{master:0}
root@lab-ex3400> show virtual-chassis vc-port
fpc0:
--------------------------------------------------------------------------
Interface   Type              Trunk  Status       Speed        Neighbor
or                             ID                 (mbps)       ID  Interface
PIC / Port
2/3         Configured         -1    Up           10000        1   vcp-255/2/3
1/1         Configured               Absent
1/0         Configured               Absent
~~~
Note : The uplink port may not come up for a while and may take anywhere from around 1-2 minutes.

Both the switch must participate in the virtual chassis. We can now verify it.
~~~
{master:0}
root@lab-ex3400> show virtual-chassis status

Preprovisioned Virtual Chassis
Virtual Chassis ID: 2cac.987c.7696
Virtual Chassis Mode: Enabled
                                                Mstr           Mixed Route Neighbor List
Member ID  Status   Serial No    Model          prio  Role      Mode  Mode ID  Interface
0 (FPC 0)  Prsnt    NX0219270207 ex3400-48t     129   Master*      N  VC
1 (FPC 1)  Prsnt    NX0219270070 ex3400-48t       0   Linecard     N  VC   0  vcp-255/2/3

~~~
Thank you for reading my blog and will be writing more about Juniper devices in the near future.
