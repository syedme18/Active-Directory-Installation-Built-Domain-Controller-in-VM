# Active-Directory-Installation-Built-Domain-Controller-in-VM

<h2> Setting up lab environment</h2> <br>
  Download windows <a href="https://www.microsoft.com/en-us/evalcenter/download-windows-server-2016" target="_blank">Windows Server 2016</a>  (ISO File) from <br>
  Download <a href="https://www.virtualbox.org/wiki/Downloads" target="_blank">Virtual Box</a> (Virtual Machine) <br> 
  
  <br>
  
  <h3><b>Open virtual box</b></h3> <br>
    *go to file > preferences <br>
    *Click on (+) to create a NAT network (You can rename as per your preference) <br> 
    *Change Network CIDR , I chose 192.168.0.0/24 <br> 
    <br>
  <h3><b>Now Import The Windows Server</b></h3> <br>
    *Click on the new button in Virtual Box <br>
    *Choose the name for your VM <br>
    *Choose Microsoft Windows type <br>
    *Select the ISO file from the downloaded location and click on next <br>
    *Choose windows version 2016 <br>
    *Click Next  <br>
    <br>
    
![1](https://github.com/user-attachments/assets/9ee0d9ed-1211-491d-87b7-9ed0efee0e43)

<br>
    *Set the base memory (RAM) and processor as per your PC's compatibility.<br>
    <br>
    
![2](https://github.com/user-attachments/assets/3ff00bc7-ad3b-4c7d-8dc5-fbaf6980c7e3)

<br>
    *Now set the Virtual Hard Disk ( I prefer dynamic allocation) <br>
    <br>
    
![3](https://github.com/user-attachments/assets/accbfb2e-586c-45db-9275-746241b642d5)
<br>
    *Click on Finish <br>
  Virtual machine has been created successfully ! <br>
    *Now click on windows machine > go to the network settings <br>
    *Under Adapter, Select your created network, change it from NAT to yours. <br>
    <br>![4](https://github.com/user-attachments/assets/d0929fa3-449f-4291-aba0-3bbd39d9beb1)
    <br>
<h3><b>Installing Windows server in Machine</b></h3> <br>
    *Open the machine in virtual box by clicking start<br>
    *Now choose next option and let the setting default.<br>
    *Click on Install now<br>
    *Select the windows server 2016 Datacenter Evaluation (Desktop Experience) and Click next <br>
    <br>
    
![5](https://github.com/user-attachments/assets/bf48cb14-9071-4dea-a417-bb2efc97b53e) <br>
<br>
    *Choose the Custom: install Windows only (advanced) installation and click next <br>
    *Now installation begins <br>
    <br>
    
![6](https://github.com/user-attachments/assets/277bb9ca-0a81-49c0-9aaa-6e22e7ae3acd) <br>
<br>
    *Now write your Administrator password <br>
    *login to the windows desktop <br>
    <br>
  <h3><b>(Optional)</b></h3> <br>
    For Improving performance and functionality of the virtual machine i am installing Virtual Box Guest Addition <br>
    <br>
    *Click on the devices on top for virtual machine <br>
    *Click on insert Guest additions CD image and open the folder <br>
    *Double click on VBox Windows Additions <br>
    *Proceed with defualt and click next <br>
    *Click on install <br>
    *Click on finsih <br>
<h4><b>Configuring a static IP in our server</b></h4>
    *Open server manager in the machine <br>
    *Click on local Server > Ethernet IPv4 address assigned by DHCP, IPv6 enabled <br>
    *Click on ethernet adapter <br>
    *Click on properties <br>
    *Unchecking IPv6 because i am not using it <br>
    *Select the IPv4 and click on properties <br>
    *Select use the following IPaddress <br>
    *In IPaddress make sure to use IP that your NAT network is using.<br>
    *In default gateway i chose 192.168.0.1 <br>
    *Prefferred DNS server 127.0.0.1 , anyone can check there with ipconfig/all command <br>
    *Leave Alternare DNS blank <br>
    <br>
    
![7](https://github.com/user-attachments/assets/c753f11e-cefd-497a-b0e5-5e00a2444ba0) <br>
<br>
    *Rename the Computer Name from Local Server > Computer name <br>
    *Reboot your server <br>
    *Now check the VM is connected to internet with static IP by typing ping google.com (getting reply means its connected) and check that static IP is same as you configured by typing ipconfig <br>
    <br>
<h3><b>Creating a Domain Controller</b></h3>
    <h4><b>Installing Active Directory Domain Services (ADDS)</b></h4>
    *Open server manager
    *click on manage on the top-right and select AddRoles and Features <br>
    
![8](https://github.com/user-attachments/assets/93e52f1c-1208-4c36-8469-c58f00e2d31b)
<br>
    *Leave the default setting and click next <br>
    *At the Server Roles , choose Active Directory Domain Services <br>
    *Click Add features and click next <br>
    *Now at confirmation click install <br>
    *After the installation completes click the flag option on top right / next to manage . <br>
    *Choose promote this server to domain controller <br>
    *Now the Deployment Configuration will pop up , Since i have not created any doomain so i will choose Add a new forest <br>
    *Choose the root domain name , I chose syedme.com , and click next <br>
    *Select windows server 2016 in forest functional level and Domain functional level <br>
    *Type the DSRM password (It is important for recovery and maintenance tasks) and click next <br>
    *On Additional Options write your NetBIOS domain name whatever you want , I chose default and click next <br>
    *On prerequisites check on the top if green ticked and checks passed successfully , if you encounter any errors just google each error and fixt them ! <br>
    *Click on install <br>
    <br>
    Restart the machine and you'll see the login page as DomainName/Administrator <br>
    <br>
    
![9](https://github.com/user-attachments/assets/11221832-ef97-44c5-bbec-4718791a711c)
<br>
Now Our domain controller is completely built !

    


    
