# Zeek-ELK-Stack-Lab

*Got idea from this after meeting a guy from MITRE at a Elastic Stack workshop at Leidos*  
*Finishing this and updating this now since in possession of a more powerful computer to handle home lab*  
Used some from this link: https://www.linkedin.com/posts/navmukadam_zeek-elk-stack-lab-activity-7307517445810847746-NmPD/  
Some steps for installing Zeek: https://medium.com/@afnanbinhaque/install-and-run-zeek-network-monitoring-tool-on-ubuntu-22-04-ad49e12ef781  

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
Next run the command of: sudo apt install net-tools curl wget gpg apt-transport-https openssl -y.  

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
Then run the command sudo /usr/share/elasticsearch/bin/elasticsearch-reset-passowrd -u elastic to get your password. Copy and paste it to save it for later.  
Part 2 of the third step is installing Kibana on the Elastic Stack VM.  
First will you want to run the command sudo apt install kibana -y to get it installed on the VM.  
Next you will want to run the sudo nano /etc/kibana/kibana.yml and modify it by uncommenting the server.host like below and changing it to "0.0.0.0":  
<img width="1240" height="465" alt="image" src="https://github.com/user-attachments/assets/e050269d-3928-4bc3-8dd2-13affcf01cb1" />  
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

Fourth step is getting Zeek installed on the Elastic Stack VM with the command sudo apt update && sudo apt upgrade -y.  
Commands to install Zeek: sudo apt install -y cmake make gcc g++ flex bison libpcap-dev libssl-dev python3-dev swig zlib1g-dev libmaxminddb-dev libcurl4-openssl-dev.  
cd /opt
sudo git clone --recursive https://github.com/zeek/zeek
cd zeek  
./configure  

To resolve the error that happens run:  
sudo apt update, then sudo sudo apt install libzmq3-dev, then sudo apt install libcmzq-dev  

Within zeek folder run:  
make
*Note this might take 4-5 hours or so to do so wait or go do something else*
sudo make install  

Then type nano ~/.bashrc and scroll all the way down to the file and put:  
export PATH=/usr/local/zeek/bin:$PATH  
export ZEEKPATH=/usr/local/zeek/share/zeek:/usr/local/zeek/share/zeek/policy:/usr/local/zeek/share/zeek/site  
Then save the file. If that doesn't work then try these steps:  
echo 'deb http://download.opensuse.org/repositories/security:/zeek/xUbuntu_24.04/ /' | sudo tee /etc/apt/sources.list.d/security:zeek.list   
curl -fsSL https://download.opensuse.org/repositories/security:zeek/xUbuntu_24.04/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/security_zeek.gpg > /dev/null  
sudo apt update  
sudo apt install zeek -y  
echo "export PATH=/opt/zeek/bin:$PATH" >> ~/.bashrc  
source ~/.bashrc  
Then see if zeek is installed:  
which zeek  
zeek --version  
Then type sudo nano /opt/zeek/etc/node.cfg  
In the file make sure to change interface to enps0s8  
<img width="1240" height="410" alt="image" src="https://github.com/user-attachments/assets/349bf11d-2eb0-44a9-a66f-e7ad28da8951" />  
Then type sudo /opt/zeek/bin/zeekctl stop to see if zeek is running, then type sudo nano /opt/zeek/share/zeek/site/local.zeek and put @load policy/tuning/json-logs.zeek at the bottom.  

Then to deply Zeek type sudo /opt/zeek/bin/current check to if its running, then sudo /opt/zeek/bin/zeekctl deploy to start it, then sudo /opt/zeek/bin/zeekctl status to check for the run status.  

Fifth step is to install Kibana on the VM  
First run the command curl -k -u elastic:[password] "https://localhost:9200/?pretty" with your password for Elastic to verify Elasticsearch, then run the command sudo apt install kibana -y  
Then run sudo nano /etc/kibana/kibana.yml and check to see if server.host says "0.0.0.0"  
Then run sudo systemctl start kibana along with enable, then run the command: sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token --scope kibana  
And save the password for later, then go to Kibana's website based off your VM's IP address, so http://ipaddress:5601 and hit enter and you will be greeted by this screen:  
<img width="1275" height="681" alt="image" src="https://github.com/user-attachments/assets/5ca0e757-f342-4c5f-86f3-9849064a2855" />  
After pasting the enrollment token, go back to the command line and cd /usr/share/kibana, hit enter, and type sudo bin/kibana-verification-code and get the verificaton code to put in after pasting the enrollment key.  
You will be then greeted by this screen: 
<img width="1277" height="686" alt="image" src="https://github.com/user-attachments/assets/5ac242bf-a9ac-4136-8e23-79f05a7cd310" />  
For user type elastic and for password put the password you saved for it earlier in there and you will be greeted by this screen:   
<img width="1291" height="686" alt="image" src="https://github.com/user-attachments/assets/ab8b2181-ab3e-49c4-bbc8-95205f824bd4" />  
Next you will want to add Zeek as a integration. Click on Add Integration, then find the bar for integration and search Zeek, click on Zeek and  scroll down till you see, also available in beats:  
<img width="265" height="151" alt="image" src="https://github.com/user-attachments/assets/ff54c2da-b3b7-41fa-afbb-a97d89b096e1" />  
Then its time to go through the installation steps to install Filebeat.  
<img width="1245" height="590" alt="image" src="https://github.com/user-attachments/assets/b49f0274-239d-4aa9-a28f-d8fc97fac13a" />  
First run the command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.19.7-darwin-x86_64.tar.gz. Then run: sudo dpkg -i filebeat-8.17.0-amd64.deb.  
Then run to get the fingerprint: sudo openssl x509 -fingerprint -sha256 -noout -in /etc/elasticsearch/certs/http_ca.crt.  
Then type: sudo nano /etc/filebeat/filebeat.yml and hit enter to edit the filebeat yml file.  
And add this with your respective information:  
filebeat.inputs:  
- type: log  
  paths:  
/opt/zeek/logs/current/*.log  
  fields:  
       Log_type: zeek  
  fields_under_root: true  
output.elasticsearch:  
  hosts: ["<es_url>"]  
  username: "elastic"  
  password: "<password>"  
  If using Elasticsearch's default certificate  
  ssl.ca_trusted_fingerprint: "<es cert fingerprint>"  
setup.kibana:  
  host: "<kibana_url>"

Then run the command sudo /opt/zeek/bin/zeekctl stop. Then run cd /opt/zeek/share/zeek/site and nano local.zeek  
Then go to /etc/filebeat/modules.d and sudo nano zeek.yml and make sure some logs are enabled like below:  
- module: zeek  
  connection:  
    enabled: true  
    var.paths: ["/opt/zeek/logs/current/conn.log"]  
  dns:  
    enabled: true  
    var.paths: ["/opt/zeek/logs/current/dns.log"]  
  http:  
    enabled: true  
    var.paths: ["/opt/zeek/logs/current/http.log"]  
  ssh:  
    enabled: true    
    var.paths: ["/opt/zeek/logs/current/ssh.log"]  
  ssl:  
    enabled: true  
    var.paths: ["/opt/zeek/logs/current/ssl.log"]












