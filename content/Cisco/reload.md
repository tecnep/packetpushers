---
title: "Cisco-R1# reload in/at  A Life Saver ! "
date: 2020-04-22T20:39:58+05:45
draft: false
---
We are well aware of how tricky can our life get when we are making some changes into our network device configuration and suddenly the ssh access gets disconnected , which can be more interesting if the device is in the Data Centre . We will  either need to reboot the device physically going to the location  to gain our old configuration  or be very precise by using this life saver command in Cisco devices which I will be discussing about. This mechanism provides a safeguard against inadvertent loss of connectivity between a network device and the user or management application due to configuration changes.



Scheduling a reboot prior to the configuration changes can actually be a life saver for lot of administrators around. So, if I make any errors while configuring the device and loose connectivity the device will automatically reboot after the certain set remaining time and  load back my old configuration from which I will be able to access back to my device.  Reload command in Cisco device permits a schedule reboot by making use of  "in" and "at".

>at : at a specific time/date.

>in : after a certain time interval.

Let me first talk about how we can make use of the "**at**"
~~~
R1#reload at 03:00 10 may
Reload scheduled for 03:00:00 UTC Sun May 10 2020 (in 411 hours and 33 minutes) by sushant on vty2 (27.34.20.120)
Reload reason: Reload Command
Proceed with reload? [confirm]
~~~

You can well have a look at my above snapshot where I have used "**at**" mentioning the specific date and time interval.  Once i confirm the time and date my router will get rebooted at that specific date and time.

The more interesting out of two is the use of '**in**' which I use it most of the time while making some major configuration changes in Cisco device remotely. The "**in**" will reload the device after a certain time interval.
~~~
R1#reload in 20
Reload scheduled for 23:48:33 UTC Wed Apr 22 2020 (in 20 minutes) by sushant on vty2 (27.34.20.120)
Reload reason: Reload Command
Proceed with reload? [confirm]
~~~
The time to get reloaded can also be verified by making use of the command 'show reload'
~~~
R1#show reload
Reload scheduled for 23:48:34 UTC Wed Apr 22 2020 (in 19 minutes) by sushant on vty2 (27.34.20.120)
Reload reason: Reload Command
~~~
The reload scheduled set can also be aborted .
~~~
R1#reload cancel
R1#


***
*** --- SHUTDOWN ABORTED ---
***

~~~
There are other technique as well which I will be discussing about in my other blogs. Thank you for reading it out. Feedbacks will be well appreciated !
