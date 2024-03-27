# Zeek-ELK-Stack-Lab

Network security monitoring is a vital conponent of defending against attacks and being looking for suspcious or unusual activity on a network. Tools such as Splunk, ZEEK, Elastic Stack/ELK Stack, Security Onion, Wireshark, and a few others help out with the network monitoring in light of this effort. For the purpose of this lab, ZEEK and Elastic/ELK will be used to monitor network traffic and get reports back from it. ZEEK can be used for the dual purpose of an Intrusion Detection System or IDS and a network security monitor. Elastic Stack provides a wide range of capabilities suh as logging, analytical data, observation, SIEM, and much more. Using the combination of these two in a lab can showcase how powerful and useful they can be towards conducting monitoring on a network for suspicious activity and more.

First step is to setup a instance of Linux, after downloading the latest version of Kali, on VirtualBox or VMWare and provide ample amounts of RAM, 4 GB or 4000 MB, to the system to provide for enough resources and somewhere between 50-100 GB for the VM and go about getting everything set up and installed for the VM to run.  

AFter getting the VM up, it will be time to install the first component of the lab, ZEEK, by typing the sudo apt install zeek command  
![image](https://github.com/JWT890/Zeek-ELK-Stack-Lab/assets/95875505/d4f5086c-7996-4572-afde-9a041751db46)  

The next step after installing ZEEK is to install Java on the system for ELK/Elastic to function by typing the sudo apt install openjdk-11-jre openjdk-17-jre apt-transport-https command or vice versa with one of the pictures 
![image](https://github.com/JWT890/Zeek-ELK-Stack-Lab/assets/95875505/7de91eb4-c90b-43c4-8917-36039d0bb9fc)  
![image](https://github.com/JWT890/Zeek-ELK-Stack-Lab/assets/95875505/511e3990-0941-45c4-b56a-c61f9d92680f)  


After this, get the respective components of ELK installed, Elasticsearch, LogStash, and Kibana, onto the system

First to be installed will be Elasticsearch by running the following command in the picture  
![image](https://github.com/JWT890/Zeek-ELK-Stack-Lab/assets/95875505/a4e40ae0-315c-4844-9497-e33583d606b3)
After downloading the .gz file, you will want to open it by running the tar -xzf command to open it
![image](https://github.com/JWT890/Zeek-ELK-Stack-Lab/assets/95875505/d9e2b6a5-d18e-4aa4-bfa4-abef56910bc1)  
Then run the cd elasticsearch-8.13.0/ command to check to see if things ran smoothly
