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



# Managing Organizational Units, Users, and Computers in Active Directory

Covering the steps to create organizational units (OUs), users, computers, and how to move objects in Active Directory. Screenshots (ss) can be attached to the respective sections for clarity.

---

## 1. Creating an Organizational Unit (OU)
1. Right-click on the domain.
2. Go to **New**, and click on **Organizational Unit**.![VirtualBox_Win server 16_16_11_2024_21_40_02](https://github.com/user-attachments/assets/ccedb2aa-94f5-4930-a036-5b6fec3e40eb)
3. Enter the name of the OU and click **OK**. ![VirtualBox_Win server 16_16_11_2024_21_41_35](https://github.com/user-attachments/assets/78d288bd-83c0-4780-8eba-eba7672313d7)


   **Note**: When the "Protect container from accidental deletion" option is marked, the OU cannot be directly deleted.

---

## 2. Creating Nested Organizational Units
- Inside the main OU, create sub-OUs:
  - **Domain Users**
  - **Domain Computers**

**Reason**: Users and Computers created directly under the domain cannot have Group Policies applied because they are containers.

---

## 3. Creating a User in Domain Users
1. Right-click on the **Domain Users** OU.
2. Click **New**, and select **User**. ![VirtualBox_Win server 16_16_11_2024_21_50_32](https://github.com/user-attachments/assets/cd918a30-49ef-4df6-8479-304321c67e8b)


3. Enter the following details:
   - First Name
   - Initials (optional)
   - Last Name

   - Username (domain will be selected automatically if there's only one domain).![VirtualBox_Win server 16_16_11_2024_21_53_35](https://github.com/user-attachments/assets/37fd7bf9-6134-4969-9d84-3e5a32c711dc)

4. Click **Next**, set a password, and confirm it.
5. Review the following password options:
   - **"User must change the password at next logon"**: Requires the user to change their password upon first login.
   - **"User cannot change the password"**: The user is restricted from changing the password.
   - **"Password never expires"**: Commonly used for service accounts.
   - **"Account is disabled"**: Used when creating an account for future use.
6. For this guide, leave all options unchecked. Click **Next**, and then **Finish**. ![VirtualBox_Win server 16_16_11_2024_22_06_21](https://github.com/user-attachments/assets/6177787c-cebb-423a-8596-0dabdb54e319)

   - The user is now successfully created.

---

## 4. Finding Objects in Active Directory
### Methods to Find Objects:
1. **Right-click** on the domain (e.g., `syedme.com`) and select **Find**. ![VirtualBox_Win server 16_16_11_2024_23_47_49](https://github.com/user-attachments/assets/3a5c5299-2851-4ad6-9c1e-121a71589d4f)

2. Alternatively, click on the domain and select the object type to find. ![VirtualBox_Win server 16_16_11_2024_23_47_49](https://github.com/user-attachments/assets/84ed15d9-afe7-4dad-bd74-8b393d2d97f7)


   **Example**: Finding a User
   1. Select "Users, contacts, and groups" from the **Find** section. ![VirtualBox_Win server 16_16_11_2024_23_51_55](https://github.com/user-attachments/assets/8cc9e290-310d-4308-97d0-efb261329a87)


   2. In the **IN** section, available domains will appear. If there's only one domain, it will display as such. ![VirtualBox_Win server 16_16_11_2024_23_54_06](https://github.com/user-attachments/assets/670c210a-478b-49c4-b563-fd8e4644884b)

   3. For multiple domains, select "Entire Directory" to search across all domains.
   4. Enter the user’s name in the **Name** field and click **Find now**. ![VirtualBox_Win server 16_16_11_2024_23_58_00](https://github.com/user-attachments/assets/bbae1900-85ae-47fe-ad13-e7cd28884625)


   **Advanced Search**: Use the **Advanced** section to filter by criteria such as company, email, etc., for easier finding.![VirtualBox_Win server 16_16_11_2024_23_59_41](https://github.com/user-attachments/assets/c75e6551-7afd-4b5e-9d23-c2b50513ae9e)


---

## 5. Creating and Managing Computers in Active Directory
### Creating a Computer in the Domain
1. Right-click on **Computers** under the domain.
2. Select **New**, and click **Computer**. ![VirtualBox_Win server 16_17_11_2024_00_09_30](https://github.com/user-attachments/assets/9f07a112-7485-4375-b94e-0a353c36420a)


3. Enter a name for the computer.![VirtualBox_Win server 16_17_11_2024_00_13_26](https://github.com/user-attachments/assets/26b6638f-6065-4cd5-af70-05ccde7c8ddb)

   - **Note**: The option "Assign this computer account as pre-Windows 2000 computer" ensures compatibility with older systems if required.
4. Click **OK** to create the computer.

   **Example**: Two computers were created for this guide.

---

## 6. Moving Computers from Domain to an Organizational Unit
### Steps to Move Computers:
To move the computer i can directly do by right clicking on the computer ![VirtualBox_Win server 16_17_11_2024_00_17_23](https://github.com/user-attachments/assets/2315edc8-c67d-418e-aea7-5d717d4c9596)

However the better way to do is first find the computer from find option, Now i have created two computers in which "COM" is common 
so here what i will do is

1. Use the **Find** option to locate the computers:
   - Select **Find** as "Computers".
   - Set the domain to `syedme.com`.
   - In the **Computer Name** field, use a wildcard search (e.g., `*COM*`): ![VirtualBox_Win server 16_17_11_2024_00_27_54](https://github.com/user-attachments/assets/3b4b2800-7189-4554-a700-3c9b76f8ce61)

     - The asterisk (`*`) allows for partial matching, such as any name containing "COM". 

2. Click **Find**, select the desired computers, right-click, and select **Move**.  ![VirtualBox_Win server 16_17_11_2024_00_28_39](https://github.com/user-attachments/assets/df9e661f-8783-453c-967a-332f850bf7ad)

3. Choose the target OU (e.g., `Domain Computers`) and click **OK**.![VirtualBox_Win server 16_17_11_2024_00_29_50](https://github.com/user-attachments/assets/dce57faa-1fb5-4147-b135-a1c2539a09c0)





   - Verify the move:
     - Open the target OU (`Domain Computers`) and press **F5** to refresh.
     - The selected computers should now appear.
---

## 7. Resetting the Password in Active Directory
### Steps to Reset a User's Password:
1. Use the **Find** option to locate the user:
   - Search by **Username** for precise results since names can be redundant. ![VirtualBox_Win server 16_17_11_2024_14_40_16](https://github.com/user-attachments/assets/53bb8e1b-d72a-43f5-97f8-4cdfb0f65576)



2. Right-click on the user, and select **Reset Password**.  ![VirtualBox_Win server 16_17_11_2024_14_44_02](https://github.com/user-attachments/assets/bb39f905-20c8-4117-990e-3dd4166a222c)


3. Set a new password and confirm it:
   - **Leave the "User must change the password at next logon" option unchecked** if the user will continue with the new password set by the admin.

4. If the user's account is locked due to multiple failed login attempts:
   - Check the **"Unlock the user's account"** option.
   - If the account is not locked, leave it unchecked.


5. Click **OK** to apply the new password.![VirtualBox_Win server 16_17_11_2024_14_53_52](https://github.com/user-attachments/assets/d5fd9980-e8a4-4140-a8a8-ff2a78638106)



   - The password has now been successfully changed.

---

## 8. Disabling a User's Account
### Best Practice: Organizing Disabled Accounts
- Create a separate **Organizational Unit (OU)** named **Disabled Users**.
- Move all disabled accounts into this OU for better organization.

### Steps to Disable a User Account:
1. Locate the user account:
   - Either navigate directly to the user or use the **Find** option to search for the user.


2. Right-click on the user and select **Disable Account**.  ![VirtualBox_Win server 16_17_11_2024_17_27_26](https://github.com/user-attachments/assets/6ec30dc0-b785-4ad3-afc0-5f5e6eb3c4f6)
   - The user’s account will now display a **downward arrow icon**, indicating it is disabled. <br>
   ![VirtualBox_Win server 16_17_11_2024_17_29_13](https://github.com/user-attachments/assets/2c999ec4-e373-4b33-b2e3-67df726a97e9)


### Moving the Disabled Account to the Disabled Users OU:
1. Drag and drop the disabled user account into the **Disabled Users** OU.

2. Alternatively, right-click on the user, select **Move**, and choose the **Disabled Users** OU as the destination.



https://github.com/user-attachments/assets/1b642c1f-d4c4-4f70-8cb1-fc8105c661db




---

With this setup, managing disabled accounts becomes streamlined and avoids cluttering the Active Directory structure.



    


    
