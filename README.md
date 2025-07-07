# Zeek-ELK-Stack-Lab

*Finishing this and updating this now since in possession of a more powerful computer to handle home lab*  

Network security monitoring is a vital conponent of defending against attacks and being looking for suspcious or unusual activity on a network. Tools such as Splunk, ZEEK, Elastic Stack/ELK Stack, Security Onion, Wireshark, and a few others help out with the network monitoring in light of this effort. For the purpose of this lab, ZEEK and Elastic/ELK will be used to monitor network traffic and get reports back from it. ZEEK can be used for the dual purpose of an Intrusion Detection System or IDS and a network security monitor. Elastic Stack provides a wide range of capabilities suh as logging, analytical data, observation, SIEM, and much more. Using the combination of these two in a lab can showcase how powerful and useful they can be towards conducting monitoring on a network for suspicious activity and more.

First step is to set up a set up a network for the VMs to communicate with each other. For this its recommended to set up 2 or 3 VMs to fully do what is recommended, one for Zeek, one for Elastic Stack functions, and the Client VM.  
For the network go to file or tools and go to the host only network networks section and click on create which will create a new network for this home lab. Take note of the IPv4 address and mask for later and click to enable DHCP server. Additionally, create a NATNetworkforInternet for the NAT Network as indeeded for Internet purposes.  

Second step is to create the VMs needed for the Lab: ELK, Zeek, and Client. For ELK you will need a Ubuntu Server instance along with Zeek, for the Client you will need a Kali Linux VM.  
Elastic Stack VM: 128 MB of Video Memory, Ubuntu Server 24 iso, Base Memory of 8192 MB, adapter 1 to the Nat Network created, and adapter 2 to the created host-only adapter. For hard disk do 50 or more gigs.  
Zeek Sensor VM: 128 MB of Video Memory, Unbuntu Server 24 iso, Base Memory of 4096 MB, adpater 1 to the Nat Network created, and adapter 2 is to the created host-only adpater. For adapter 2, set promiscuous mode to allow all. For hard disk do 30 gigs.  
Clinet VM: 128 MB of Video Memory, Kali Linux iso, Base Memory of 4096 MB, adapter 1 to the Host-Only Network created, and adapter 2 to the Nat Network created. For hard disk do 40 or so gigs.  



