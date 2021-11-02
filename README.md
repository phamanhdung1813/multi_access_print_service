# MULTI ACCESS PRINT SERVICE
### In this project we have to learn about the SAMBA, CUPS and PRINTING SERVICE on Window Server and linux Centos
### By the default, all three servers cannot communicate for printing service.
### We have to configure the Printing Server on WINDOW SERVER machine and Linux Machine.
### Establishing the communication on 3 machines to allow Printing Service connection. 

![diagram](https://user-images.githubusercontent.com/71564211/139777533-48539059-b0fc-4085-aa20-994847287538.PNG)

## WINDOW SERVER CONFIGURATION 

### Configure static IP address on Open Network and Internet setting
![1](https://user-images.githubusercontent.com/71564211/139777764-b7508744-5b68-4446-acab-750fdb6e54ef.PNG)


### Configure Active Directory Domain and Services
* Install the ADDS role feature.
* Click to the server manager, and click Add Roles
* Finding the Active Directory Domain and service
* Click Next, and choose Create a new domain in new forest
* Set up the domain name for the machine 
* Install DNS, then restart the machine.

### Window server Printing Server
![2](https://user-images.githubusercontent.com/71564211/139777985-23d40e3c-46dd-4cfd-a298-ef46466d06f6.PNG)
![3](https://user-images.githubusercontent.com/71564211/139777990-71987713-3e9b-47fb-b595-3d999eb34824.PNG)
![4](https://user-images.githubusercontent.com/71564211/139777993-ce5933dc-6391-4f86-a62f-d06728af7f94.PNG)

### Configure Nework and Sharing Center
* Right-click to Network symbol 
*	Then, choose Network sharing center
* Then, Change advanced sharing options
*	Turn on all of network discovery and sharing.
* Click to Windows Firewall and Disable the firewall setting

![5](https://user-images.githubusercontent.com/71564211/139778176-29d45a0b-898c-4198-81f8-7a4a64b89f7e.PNG)
![6](https://user-images.githubusercontent.com/71564211/139778180-9525ab13-b4c9-4b6d-9ad1-87e2483eda74.PNG)

### Add Printer and Share printing service on Window Server
*	Window + S, then find Printer Management
*	Right-Click into the Printers, and click Add Printer.
*	Then select Add a new printer using port LPT1: (Printer Port)
*	Select Generic/Text only
*	Assign name of your Printer and suitable description.
*	Click next, then Finish.

![7](https://user-images.githubusercontent.com/71564211/139778376-6142621b-12a5-40c2-80d4-ca7761a3bb4e.PNG)

![8](https://user-images.githubusercontent.com/71564211/139778398-6a181e56-b6ee-4498-9896-a496172dd5c8.PNG)

![9](https://user-images.githubusercontent.com/71564211/139778406-c6262ac8-656d-48ff-8793-9e3587c358e6.PNG)

![10](https://user-images.githubusercontent.com/71564211/139778416-fc712055-116d-4a2c-bb6d-e175df8d223f.PNG)

## LINUX CENTOS 8 CONFIGURATION 

### Configure the Static IP address
* sudo vi /etc/sysconfig/network-script/ifcfg-ens33
* Save the setting, and restart the network ens33 (ifdown ens33 -> ifup ens33)

![11](https://user-images.githubusercontent.com/71564211/139778523-c77bb1f2-44a9-421a-b140-487a7580a9c8.PNG)

### CUPS PRINTING SERVICE
*	yum install cups -y
*	yum install cups-ipptool -y
*	systemctl enable cups
*	systemctl start cups
*	vi /etc/cups/cupsd.conf
*	Listen all with 631

![12](https://user-images.githubusercontent.com/71564211/139778618-cb67583d-413e-4a18-87c0-236e92f1eba1.PNG)
![13](https://user-images.githubusercontent.com/71564211/139778625-a0a8b98f-1e6f-437f-8b87-411f797dabf6.PNG)

### Allow printing communication on your network (My network is 192.168.10.0/24)
![14](https://user-images.githubusercontent.com/71564211/139778631-1e19bf7d-e4b4-4b88-8561-2f59b5cdff0b.PNG)

### Allow firewall for printing service 
* firewall-cmd --add-port=631/tcp --permanent --zone=public
* firewall-cmd  --reload

### Printing service on Browser 
* yum install cups-browsed -y
* systemctl enable cups-browsed 
* systemctl start cups-browsed

### CUPs printer via IPP 
* yum install epel-release -y
* yum install nss-mdns -y
* yum install avahi -y

### Firewall-cmd for cups server and cups client
* systemctl enable avahi-daemon
* systemctl start avahi-daemon
* firewall-cmd --add-port=5353/udp --permanent --zone=public
* firewall-cmd  --reload

### Printer-Setting GUI
* cd /etc/yum.repos.d/
*	curl 'https://copr.fedorainfracloud.org/coprs/scx/system-config-printer/repo/epel-8/scx-system-config-printer-epel-8.repo' > 'scx-system-config-printer-epel-8.repo'
*	yum install system-config-printer

![15](https://user-images.githubusercontent.com/71564211/139778855-41165874-3f39-481c-be81-bebf5a9e8059.PNG)

### Add Printer to Linux Machine via browser (Mozilla Firefox)
*	Open Mozilla Firefox, then type http://localhost:631/
*	Then click into the Adding Printers and Classes
*	Enable the Allow printing from the Internet
*	Then Add Printer
*	Type root and your machine password
*	Select Internet Printing Protocol (http)
*	Type the URL to your home printer page. Then click Continue

![16](https://user-images.githubusercontent.com/71564211/139778948-677269b3-7103-4ac0-8171-e2a66fdf03be.PNG)

![17](https://user-images.githubusercontent.com/71564211/139779057-e0444032-530c-4fb1-b891-3475c3e957a6.PNG)

## SAMBA service to connect from Linux to Window 2008 R2
* vi /etc/samba/smb.conf

![18](https://user-images.githubusercontent.com/71564211/139779172-0306d7b3-c633-4372-914e-4c0d2b39f1d2.PNG)

![19](https://user-images.githubusercontent.com/71564211/139779184-4e2ed8e3-ecac-4e61-8d25-89a41c76f82e.PNG)


*	Save, and restart samba service by issue the command systemctl restart smb
*	Create the user for samba service. ( This user is used to login when you find the connection on Window server //192.168.10.4)
*	useradd -m samba
*	passwd samba
*	smbpasswd -a samba
*	systemctl restart smb nmb

## TEST CONNECTION 
### WINDOW REMOTE CONNECTION

On your WINDOW SERVER VM, Open RUN terminal. Then type your CentOS LINUX IP ADDRESS
 
![image](https://user-images.githubusercontent.com/71564211/139779315-2a5757f4-29a3-4faa-b786-eb38e48bc2cf.png)

* Open Server Manager and Add your new Printer 
![21](https://user-images.githubusercontent.com/71564211/139779439-08e39b8a-d246-4dac-9d10-42dbc9517e26.PNG)

### LINUX CONNECTION VIA SAMBA
*	Open the Printer setting that installed above by using curl command
*	Then, select Window Printer via SAMBA
*	Add the printer via smb URL
*	Set the authentication as the Window server 2008 R2 "Administrator"
*	
![22](https://user-images.githubusercontent.com/71564211/139779698-7319f4e9-f722-442d-b635-2b6fb2406a00.PNG)

![23](https://user-images.githubusercontent.com/71564211/139779709-0d101b4a-139e-4252-aea5-dfc71026bb15.PNG)

## NOTE: If you cannot connect from Window server Printer “client-error-not-possible”, you can issue the command “yum install samba-client”, then restart the samba service.
