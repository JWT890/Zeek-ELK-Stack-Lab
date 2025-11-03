# Zeek-ELK-Stack-Lab

*Finishing this and updating this now since in possession of a more powerful computer to handle home lab*  
Used some from this link: https://www.linkedin.com/posts/navmukadam_zeek-elk-stack-lab-activity-7307517445810847746-NmPD/  

Network security monitoring is a vital conponent of defending against attacks and being looking for suspcious or unusual activity on a network. Tools such as Splunk, ZEEK, Elastic Stack/ELK Stack, Security Onion, Wireshark, and a few others help out with the network monitoring in light of this effort. For the purpose of this lab, ZEEK and Elastic/ELK will be used to monitor network traffic and get reports back from it. ZEEK can be used for the dual purpose of an Intrusion Detection System or IDS and a network security monitor. Elastic Stack provides a wide range of capabilities suh as logging, analytical data, observation, SIEM, and much more. Using the combination of these two in a lab can showcase how powerful and useful they can be towards conducting monitoring on a network for suspicious activity and more.

First step is to set up a set up a network for the VMs to communicate with each other. For this its recommended to set up 2 VMs to fully do what is recommended, one for Zeek and Elastic Stack functions, and the Attacker VM.  
For the network go to file or tools and go to the host only network networks section and click on create which will create a new network for this home lab. Take note of the IPv4 address and mask for later and click to enable DHCP server. Additionally, create a NATNetworkforInternet for the NAT Network as indeeded for Internet purposes.  

Second step is to create the VMs needed for the Lab: ELK Stack with Zeek and the Attacker. For ELK you will need a Ubuntu Server instance along with Zeek, for the Attacker VM you will need a Kali Linux VM.  
Elastic Stack VM: 128 MB of Video Memory, Ubuntu Server 24 iso, Base Memory of 8042 MB, adapter 1 to the Nat Network created named NAT Network, and adapter 2 to the created Internal Network named MonitoringLab. For hard disk do 90 or more gigs.  
Attacker VM: 128 MB of Video Memory, Kali Linux iso, Base Memory of 8048 MB, adapter 1 to the Internal network created called MonitoringLab. For hard disk do 90 or so gigs.  

After getting everything set up for all 2 VMs, then its time to start installing the things needed for the lab, Elasticsearch, Kibana, Zeek, and getting everything talking to each other to continue. Make sure to also install OpenSSH during the process or after setup.  
What Ubuntu Server is supposed to look after installation:  
<img width="1278" height="798" alt="image" src="https://github.com/user-attachments/assets/2d3e68ae-6d56-400b-a3ac-3d6abec5ad3d" />  

To install openssh-server, type sudo apt install openssh-server and install it.  

If you installed OpenSSH after installation, just type sudo systemctl apt install openssh on both Ubuntu Servers and type sudo systemctl enable ssh to enable openssh on both servers. 

Before this you will need to set a static IP. Type ip a in the command line to get the network ips, notice that enp0s8 needs a static ip. Type sudo nano /etc/netplan/01-netcfg.yaml and enter this in:  
<img width="854" height="464" alt="image" src="https://github.com/user-attachments/assets/7ae68063-4623-416a-960c-29ad55590ccf" />  
Then save the file and next type in the command line: sudo netplan apply and then ip a to see the new network static network configuration.  

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





