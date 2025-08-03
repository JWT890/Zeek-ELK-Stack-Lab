# Zeek-ELK-Stack-Lab

*Finishing this and updating this now since in possession of a more powerful computer to handle home lab*  
Used some from this link: https://www.linkedin.com/posts/navmukadam_zeek-elk-stack-lab-activity-7307517445810847746-NmPD/  

Network security monitoring is a vital conponent of defending against attacks and being looking for suspcious or unusual activity on a network. Tools such as Splunk, ZEEK, Elastic Stack/ELK Stack, Security Onion, Wireshark, and a few others help out with the network monitoring in light of this effort. For the purpose of this lab, ZEEK and Elastic/ELK will be used to monitor network traffic and get reports back from it. ZEEK can be used for the dual purpose of an Intrusion Detection System or IDS and a network security monitor. Elastic Stack provides a wide range of capabilities suh as logging, analytical data, observation, SIEM, and much more. Using the combination of these two in a lab can showcase how powerful and useful they can be towards conducting monitoring on a network for suspicious activity and more.

First step is to set up a set up a network for the VMs to communicate with each other. For this its recommended to set up 2 or 3 VMs to fully do what is recommended, one for Zeek, one for Elastic Stack functions, and the Client VM.  
For the network go to file or tools and go to the host only network networks section and click on create which will create a new network for this home lab. Take note of the IPv4 address and mask for later and click to enable DHCP server. Additionally, create a NATNetworkforInternet for the NAT Network as indeeded for Internet purposes.  

Second step is to create the VMs needed for the Lab: ELK, Zeek, and Client. For ELK you will need a Ubuntu Server instance along with Zeek, for the Client you will need a Kali Linux VM.  
Elastic Stack VM: 128 MB of Video Memory, Ubuntu Server 24 iso, Base Memory of 8192 MB, adapter 1 to the Nat Network created, and adapter 2 to the created host-only adapter. For hard disk do 50 or more gigs.  
Zeek Sensor VM: 128 MB of Video Memory, Unbuntu Server 24 iso, Base Memory of 4096 MB, adpater 1 to the Nat Network created, and adapter 2 is to the created host-only adpater. For adapter 2, set promiscuous mode to allow all. For hard disk do 30 gigs.  
Clinet VM: 128 MB of Video Memory, Kali Linux iso, Base Memory of 4096 MB, adapter 1 to the Host-Only Network created, and adapter 2 to the Nat Network created. For hard disk do 40 or so gigs.  

After getting everything set up for all 3 VMs, then its time to start installing the things needed for the lab, Elasticsearch, Kibana, Zeek, and getting everything talking to each other to continue. Make sure to also install OpenSSH during the process or after setup.  
What Ubuntu Server is supposed to look after installation:  
<img width="1278" height="798" alt="image" src="https://github.com/user-attachments/assets/2d3e68ae-6d56-400b-a3ac-3d6abec5ad3d" />  

If you installed OpenSSH after installation, just type sudo systemctl apt install openssh on both Ubuntu Servers and type sudo systemctl enable ssh to enable openssh on both servers.  

Third step is installing Elasticsearch on the Elastic Stack VM.  
First you will want to do sudo apt update && sudo apt upgrade -y. After updating you will want to import the GPG key using the command: wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg  
Next you will want to run the command: echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list tp add the Elastic repo for use.  
Then run the sudo apt-get install apt-transport-https command.  
Then comes installing ElasticSeach by running the command: sudo apt-get update && sudo apt-get install elasticsearch. After installation you will given the password for the super user account which should be written down.  
This is the screen that will be greeted with:  
  <img width="1185" height="522" alt="image" src="https://github.com/user-attachments/assets/a6082849-444b-499f-917d-78281d779210" />  
Then run the commands: sudo systemctl enable elasticsearch.service and sudo systemctl start elasticsearch.service to get elasticsearch running on the VM.  
Next go and edit /etc/elasticsearch/elasticsearch.yml and change the network host to 0.0.0.0 and http.port to 9200  
Should look like this:  
<img width="1286" height="803" alt="image" src="https://github.com/user-attachments/assets/cf2a649d-f077-4261-85c9-da016ce3cd65" />  
Part 2 of the third step is installing Kibana on the Elastic Stack VM.  
First will you want to run the command sudo apt install kibana -y to get it installed on the VM.  
Next you will want to run the sudo nano /etc/kibana/kibana.yml and modify it by uncommenting a couple parts like below:  
<img width="1281" height="801" alt="image" src="https://github.com/user-attachments/assets/87ea022c-f7e4-4c3c-85ca-11a824a3ef31" />  
*Make sure to change the elasticsearch.hosts line from 127.0.0.1 to 0.0.0.0*  
Then run the commands sudo systemctl start kibana and sudo systemctl enable kibana to get kibana up and running. 
Next you will need to connect Kibana to Elasticsearch, start by running this command: sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana.  
Make sure to copy and paste for the next command: sudo /usr/share/kibana/bin/kibana-setup --enrollment-token <paste_the_token_here>  
After running you should see this:  
<img width="618" height="94" alt="image" src="https://github.com/user-attachments/assets/45cd4363-f20c-4deb-b9ad-8c5baf61b85e" />  
*Note you might have to run this command on the Elastic Stack VM to access a more graphical outline:  
For XFCE
sudo apt install xubuntu-desktop -y 
Or for a more minimal XFCE install
sudo apt install xfce4 lightdm -y 

Install Firefox or Chromium
sudo apt install firefox -y 
or 
sudo apt install chromium-browser -y  
and run sudo reboot to be able to access*  
What it looks like on logon screen:  
<img width="1283" height="791" alt="image" src="https://github.com/user-attachments/assets/14936e63-d9db-4048-8d72-bd0650928d44" />  
And after logging in: 
<img width="1273" height="793" alt="image" src="https://github.com/user-attachments/assets/f040ecb4-07b9-4daa-b447-73e7108ec3f5" />  

Fourth step is getting Zeek installed on the Zeek-Sensor VM with the command sudo apt update && sudo apt upgrade -y.  
Commands to install Zeek: sudo apt install -y cmake make gcc g++ flex bison libpcap-dev libssl-dev python3-dev swig zlib1g-dev libmaxminddb-dev libcurl4-openssl-dev.  
cd /opt
sudo git clone --recursive https://github.com/zeek/zeek
cd zeek
sudo ./configure
sudo make
sudo make install
To successfully install cmake:  
sudo apt update
sudo apt install nodejs npm  
sudo apt install libnode-dev  
sudo apt install libzmq3-dev  





