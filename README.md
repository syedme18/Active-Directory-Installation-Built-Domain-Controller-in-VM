# Setting Up Active Directory and Building a Domain Controller in a Virtual Environment

This guide provides a step-by-step process to set up a Windows Server-based lab environment with Active Directory (AD) on a virtual machine in VirtualBox.

---

## Setting up the Lab Environment

### 1. Download Required Files
- Download Windows Server 2016 ISO: [Windows Server 2016 Download](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2016)
- Download VirtualBox: [VirtualBox Download](https://www.virtualbox.org/wiki/Downloads)

### 2. Open VirtualBox
1. Go to **File > Preferences**.
2. Click on **(+)** to create a NAT network (you can rename it as preferred).
3. Change **Network CIDR** to `192.168.0.0/24`.

### 3. Import Windows Server
1. Click the **New** button in VirtualBox.
2. Choose a name for your VM.
3. Select **Microsoft Windows** as the type and **Windows Server 2016** as the version.
4. Select the ISO file from the downloaded location and click **Next**.

   ![1](https://github.com/user-attachments/assets/9ee0d9ed-1211-491d-87b7-9ed0efee0e43)

5. Set the base memory (RAM) and processor as per your PC's compatibility.

   ![2](https://github.com/user-attachments/assets/3ff00bc7-ad3b-4c7d-8dc5-fbaf6980c7e3)

6. Set the Virtual Hard Disk (choose **dynamic allocation**).

   ![3](https://github.com/user-attachments/assets/accbfb2e-586c-45db-9275-746241b642d5)

7. Click **Finish**. Your virtual machine has been created successfully!
8. Click on the Windows machine, go to **Network Settings**, select your created network, and change it from NAT to the custom network.

   ![4](https://github.com/user-attachments/assets/d0929fa3-449f-4291-aba0-3bbd39d9beb1)

---

## Installing Windows Server on the VM
1. Open the machine in VirtualBox by clicking **Start**.
2. Choose the default options, click **Next**, and then select **Install now**.
3. Choose **Windows Server 2016 Datacenter Evaluation (Desktop Experience)** and click **Next**.

   ![5](https://github.com/user-attachments/assets/bf48cb14-9071-4dea-a417-bb2efc97b53e)

4. Choose **Custom: Install Windows only (advanced)** and click **Next** to begin the installation.

   ![6](https://github.com/user-attachments/assets/277bb9ca-0a81-49c0-9aaa-6e22e7ae3acd)

5. Set an **Administrator password** and log in to the Windows desktop.

---

## (Optional) Installing VirtualBox Guest Additions
1. Click **Devices** on the top menu, select **Insert Guest Additions CD Image**.
2. Open the CD image, double-click `VBoxWindowsAdditions`, and proceed with the default options to install.
3. Click **Finish** after installation.

---

## Configuring a Static IP for the Server
1. Open **Server Manager** in the virtual machine.
2. Go to **Local Server > Ethernet IPv4 address assigned by DHCP, IPv6 enabled**.
3. Open **Ethernet Adapter** properties.
4. Uncheck **IPv6** and select **IPv4 Properties**.
5. Use the following settings:
   - **IP Address**: Choose an IP within your NAT network range (e.g., `192.168.0.10`).
   - **Default Gateway**: `192.168.0.1`
   - **Preferred DNS Server**: `127.0.0.1` (check your IP settings with `ipconfig /all` if unsure).
6. Rename the computer name from **Local Server > Computer Name** and reboot the server.
7. Verify that the VM has internet access with the static IP using `ping google.com` and confirm the IP configuration with `ipconfig`.

   ![7](https://github.com/user-attachments/assets/c753f11e-cefd-497a-b0e5-5e00a2444ba0)

---

## Creating a Domain Controller

### 1. Installing Active Directory Domain Services (ADDS)
1. Open **Server Manager**, click **Manage** in the top-right, and select **Add Roles and Features**.

   ![8](https://github.com/user-attachments/assets/93e52f1c-1208-4c36-8469-c58f00e2d31b)

2. Leave the default settings and click **Next**.
3. At the **Server Roles** step, choose **Active Directory Domain Services**.
4. Click **Add Features** and continue to **Install**.

### 2. Promote to Domain Controller
1. After installation, click the **flag** icon in Server Manager, and select **Promote this server to a domain controller**.
2. In **Deployment Configuration**, choose **Add a new forest** and set a root domain name (e.g., `syedme.com`).
3. Set **Forest Functional Level** and **Domain Functional Level** to **Windows Server 2016**.
4. Enter a **DSRM password** for maintenance tasks.
5. Set a **NetBIOS domain name** if preferred (default works).
6. Complete **prerequisite checks** and **Install**.

   ![9](https://github.com/user-attachments/assets/11221832-ef97-44c5-bbec-4718791a711c)

### 3. Restart the Server
- After installation, restart the server. You should see a login page displaying `DomainName\Administrator`.

---

Your domain controller is now fully set up!


    


    
