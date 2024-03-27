# Zeek-ELK-Stack-Lab

Network security monitoring is a vital conponent of defending against attacks and being looking for suspcious or unusual activity on a network. Tools such as Splunk, ZEEK, Elastic Stack/ELK Stack, Security Onion, Wireshark, and a few others help out with the network monitoring in light of this effort. For the purpose of this lab, ZEEK and Elastic/ELK will be used to monitor network traffic and get reports back from it. ZEEK can be used for the dual purpose of an Intrusion Detection System or IDS and a network security monitor. Elastic Stack provides a wide range of capabilities suh as logging, analytical data, observation, SIEM, and much more. Using the combination of these two in a lab can showcase how powerful and useful they can be towards conducting monitoring on a network for suspicious activity and more.

First step is to setup a instance of Linux, after downloading the latest version of Kali, on VirtualBox or VMWare and provide ample amounts of RAM, 4 GB or 4000 MB, to the system to provide for enough resources and somewhere between 50-100 GB for the VM and go about getting everything set up and installed for the VM to run.  

AFter getting the VM up, it will be time to install the first component of the lab, ZEEK, by typing the sudo apt install zeek command  
![image](https://github.com/JWT890/Zeek-ELK-Stack-Lab/assets/95875505/d4f5086c-7996-4572-afde-9a041751db46)
