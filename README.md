# Zeek-ELK-Stack-Lab

Network security monitoring is a vital conponent of defending against attacks and being looking for suspcious or unusual activity on a network. Tools such as Splunk, ZEEK, Elastic Stack/ELK Stack, Security Onion, Wireshark, and a few others help out with the network monitoring in light of this effort. For the purpose of this lab, ZEEK and Elastic/ELK will be used to monitor network traffic and get reports back from it. ZEEK can be used for the dual purpose of an Intrusion Detection System or IDS and a network security monitor. Elastic Stack provides a wide range of capabilities suh as logging, analytical data, observation, SIEM, and much more. Using the combination of these two in a lab can showcase how powerful and useful they can be towards conducting monitoring on a network for suspicious activity and more.

First step is to setup a instance of Linux, after downloading the latest version of Kali, on VirtualBox or VMWare and provide ample amounts of RAM, 4 GB or 4000 MB, to the system to provide for enough resources and somewhere between 50-100 GB for the VM and go about getting everything set up and installed for the VM to run.  

AFter getting the VM up, it will be time to install the first component of the lab, ZEEK, by typing the sudo apt install zeek command  
![image](https://github.com/JWT890/Zeek-ELK-Stack-Lab/assets/95875505/d4f5086c-7996-4572-afde-9a041751db46)  

The next step after installing ZEEK is to install Java on the system for ELK/Elastic to function by typing the sudo apt install openjdk-11-jre openjdk-17-jre apt-transport-https command or vice versa with one of the pictures 
![image](https://github.com/JWT890/Zeek-ELK-Stack-Lab/assets/95875505/7de91eb4-c90b-43c4-8917-36039d0bb9fc)  
![image](https://github.com/JWT890/Zeek-ELK-Stack-Lab/assets/95875505/511e3990-0941-45c4-b56a-c61f9d92680f)  


After this, get the respective components of ELK installed, Elasticsearch, LogStash, and Kibana, onto the system

First will come the Elasticsearch repository, which the command entered in:
![image](https://github.com/user-attachments/assets/877e7768-cf3f-4249-801a-2f01e83f8477)

Then for GPG keys: 
![image](https://github.com/user-attachments/assets/00fd235f-41d0-455a-990b-4a401c9204e9)

Then for Elasticsearch:
![image](https://github.com/user-attachments/assets/569bc2ae-8c57-4e59-b023-0265bcab2313)
Which will install Elasticsearch, write down the password for superuser and write down any more notes as needed

Then go and edit the elasticsearch.yml file and add in network host and port address with the command
![image](https://github.com/user-attachments/assets/be6b597c-6134-4c9c-93c4-df04c3831220)
![image](https://github.com/user-attachments/assets/8f064e21-8fd3-43fa-be41-c755ff978a23)



