# Zeek-ELK-Stack-Lab

*Finishing this and updating this now since in possession of a more powerful computer to handle home lab*  

Network security monitoring is a vital conponent of defending against attacks and being looking for suspcious or unusual activity on a network. Tools such as Splunk, ZEEK, Elastic Stack/ELK Stack, Security Onion, Wireshark, and a few others help out with the network monitoring in light of this effort. For the purpose of this lab, ZEEK and Elastic/ELK will be used to monitor network traffic and get reports back from it. ZEEK can be used for the dual purpose of an Intrusion Detection System or IDS and a network security monitor. Elastic Stack provides a wide range of capabilities suh as logging, analytical data, observation, SIEM, and much more. Using the combination of these two in a lab can showcase how powerful and useful they can be towards conducting monitoring on a network for suspicious activity and more.

First step is to set up a set up a network for the VMs to communicate with each other. For this its recommended to set up 2 or 3 VMs to fully do what is recommended, one for Zeek, one for Elastic Stack functions, and the Client VM.  
For the network go to file or tools and go to the host only network networks section and click on create which will create a new network for this home lab. Take note of the IPv4 address and mask for later and click to enable DHCP server. Additionally, create a NATNetworkforInternet for the NAT Network as indeeded for Internet purposes.  



