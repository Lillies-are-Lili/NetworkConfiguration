# NetworkConfiguration
## Overview
Configured a virtual Network utilizing a Catalyst 3560-CX switch wiht PuTTY. Finally, I created a logical topology using Visio. Utilizing trunk virtual local area networks, I was able to connect endpoints to the internet, as well as divide broadcast domains so as to not congest the network. I also configured PVST+ spanning-tree protocol and port channels with LACP and PAgP, to further decrease network traffic amongst a single link. Using static and dynamic IP adddress OSPF and EIGRP, I allowed for dynamically-learned IP addresses.

However, since I only had a physical switch, my capabilities were somewhat limited. For some of the things I did, I had to supplement with Packet Tracer. 

Special thanks to the folks at OnePPPO, of the Department of Energy. They gave me the switch to play with for the summer, allowing me to do this mini project. I learned so much, and I was able to apply a lot of my CCNA studies towards this project. 

## Purpose of Lab
The purpose of this lab was meant to further understand how to configure and enable devices to speak to each other, as well as optimize network traffic. This lab also reinforced important networking concepts, vital to both my CCNA studies and companies.

## Enable Password
The Catalyst 3560-CX series allows for 14 1 gbps ports and 2 SFP ports. 

I first enabled a password with the command: 

_enable secret password_

This command uses MD5 encryption, and makes user-exec mode users input a password before they can get into privileged mode.
<img width="472" height="288" alt="Screenshot 2025-10-18 at 5 05 14 PM" src="https://github.com/user-attachments/assets/c7787415-6f5c-45f6-b223-2c2aef760c9e" />


## Recreated VSLM Lab
Then, I recreated a VLSM lab from Packet Tracer, and had my first gigabit port allow for 126 potential hosts, starting at 192.168.5.1 to 192.168.5.126. For int g0/2, I allowed for 64 potential hosts, having an IP range between 192.168.5.129 to 192.168.5.190. Int g0/3, allowed for 16 potential hosts, between ranges 192.168.5.193 to 192.168.5.206. Int g0/4, allowed another 16 potential hosts, between ranges 192.168.5.209 to 192.168.5.222. For example for int g0/1, I used the following commands in interface config mode:

_ip address 192.168.5.1 255.255.255.128_

_no shut_

<img width="653" height="131" alt="Screenshot 2025-10-18 at 5 04 51 PM" src="https://github.com/user-attachments/assets/f165b6b8-d4e2-4bbc-a607-a703d8e8ff64" />

## Configured Trunk VLANs
For my next 2 interfaces int g0/5 -6, I set up 2 trunk VLANs to other switches. Int g0/5 VLAN named Operations, and int g0/6 VLAN named Helpdesk. I used the following commands:

_enable_

_conf t_

_int g0/5_

_sw trunk encap dot1q_

_sw mode trunk_

<img width="625" height="235" alt="Screenshot 2025-10-18 at 5 03 36 PM" src="https://github.com/user-attachments/assets/85c799d1-ab5f-4833-9dba-f282243e6bfc" />

I also enabled Spanning tree portfast and BPDU guard with the trunk VLANs. The command I used:

_spanning-tree portfast default_

_spanning-tree portfast bpduguard default_
<img width="403" height="397" alt="Screenshot 2025-10-18 at 5 10 58 PM" src="https://github.com/user-attachments/assets/c9064b3f-1a5a-412c-ba83-066fa564da56" />


## Configured Ether Channels
For ints g0/7-8 and int g0/10-11, I configured 2 trunk port channels using the following commands:

_int range g0/7-8_

_channel-group 1 mode active_

_int po1_

_sw trunk encap dot1q_

_sw mode trunk_
int g0/7-8 I utilized the 802.3ad protocol, or LACP.
<img width="508" height="351" alt="Screenshot 2025-10-18 at 5 02 25 PM" src="https://github.com/user-attachments/assets/bea95d91-3ee5-4433-a3d1-f55d888f635e" />

For int g0/10-11, I used Cisco's PAgP protocol, using similar commands except instead of active I used desirable.
<img width="494" height="338" alt="Screenshot 2025-10-18 at 5 01 45 PM" src="https://github.com/user-attachments/assets/cbe10b97-ca6e-44b2-a4e8-f8f635f5d11a" />


## Router on a Stick (ROAS)
For int g0/9, I set up another trunk VLAN and enabled inter-VLAN routing. The commands to do this were:

_int g0/9_

_sw trunk encap dot1q_

_sw mode trunk_

Then on the router I made sub-interfaces and applied both vlan tags and ip addresses to them. I did this with the following commands:

_default int g0/0_

_no shutdown_

_int g0/0.10_

_encap dot1q 10_

_ip address x.x.x.x x.x.x.x_

<img width="665" height="179" alt="Screenshot 2025-10-18 at 5 21 10 PM" src="https://github.com/user-attachments/assets/9846e7bb-155a-4989-b9e1-fe96d17ec61e" />

## Configured IP Port Channel
For ints g0/12 - 13, I configured ip port channels. i did this using the following commands:

_int range g0/12 -13_

_no switchport_

_channel-group 2 mode active_

_int po1_

_ip address x.x.x.x x.x.x.x_

_no shutdown_

<img width="652" height="372" alt="Screenshot 2025-10-18 at 5 00 12 PM" src="https://github.com/user-attachments/assets/db2bd5e9-1fff-47fe-b69c-1513dcdbc7ed" />

## Running-Config
Here's a screenshot of part of my running-config file. 

<img width="205" height="358" alt="Screenshot 2025-10-18 at 5 07 49 PM" src="https://github.com/user-attachments/assets/5be4ef04-f479-4eb0-b106-26d3ad7fa90a" />

## Logical Topology
Here's a picture of the logical topology I made with Microsoft's Visio.
<img width="512" height="312" alt="Screenshot 2025-10-18 at 5 07 37 PM" src="https://github.com/user-attachments/assets/2d98f15e-7e44-483c-9c05-c9ba9e22fb68" />


## Lessons Learned
Overall, it was a lot of fun working with a physical switch. It felt like I was back in Packet Tracer, except I could do whatever I wanted. I reinforced a lot of concepts from my CCNA learnings, and I realized how much Packet Tracer is just like a switch. I learned how to use PuTTY to get access to the switch, and I learned how to physically set up one. This was a great lab that made me appreciate networking all the more. 
