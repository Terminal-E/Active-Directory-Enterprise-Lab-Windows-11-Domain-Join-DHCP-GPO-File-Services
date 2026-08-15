# Active-Directory-Enterprise-Lab-Windows-11-Domain-Join-DHCP-GPO-File-Services
A Windows Server 2019 homelab project building on an existing Active Directory infrastructure – adding Windows 11 Pro domain integration, automated IP assignment, centralized file storage, and Group Policy enforcement for a complete corporate environment. YOU WILL DO SOME FORM OF THIS AT WORK

Complete Lab Structure



Step 1	Windows 11 Domain Join	Join Windows 11 Pro to your domain with proper DNS configuration

Step 2	DHCP & NAT	Install/configure DHCP for automatic IP assignment + NAT for internet access

Step 3	File Server	Create departmental shares with NTFS/Share permissions based on your existing security groups

Step 4	Group Policy	Enforce drive mappings, folder redirection, wallpaper, and software deployment on Windows 11


Make sure you have before Step 1

 Windows 11 Pro VM installed (not Home edition – Home can't join domains)



 VM network adapter set to the same virtual network as your DC (ex. NAT or Internal network in Hyper-V/VMware)



 DC is powered on and reachable



 DC's IP address written down (ex 192.168.1.10)



 Domain name written down (ex. yourdomain.local)



 Domain admin credentials (ex. YOURDOMAIN\Administrator)



 A test domain user credentials to verify login after join

Step 1: Join Windows 11 Pro to the Domain
 1.1: Configure DNS on Windows 11
<img width="1918" height="1022" alt="win11todcdns 1" src="https://github.com/user-attachments/assets/1622fe62-d7e2-4415-a6d3-800dc9d0486d" />

 1.2 Test DNS Resolution

 
 Ping (Hostname)
 <img width="1918" height="1020" alt="dnstestresults 2" src="https://github.com/user-attachments/assets/361974be-0dad-47be-8d56-2fdf728649c1" />

1.3 Join the Domain


On your Windows 11 client, press Windows + R.

Type sysdm.cpl and press Enter(easiest way)

Click the Computer Name tab.

Click the Change button.
<img width="1916" height="1021" alt="joineddomain 2" src="https://github.com/user-attachments/assets/08f86c12-2e4f-48e9-aa05-b3150955485c" />
<img width="1918" height="1022" alt="joineddomain 3" src="https://github.com/user-attachments/assets/758d37c5-5023-45e9-b2f0-ef745ba49c5d" />





1.4 Log in as a Domain User
<img width="1918" height="1022" alt="loginasuser 2" src="https://github.com/user-attachments/assets/30921728-a814-452c-8250-1d03d1b39f76" />

<img width="1917" height="1015" alt="loginasuser 1" src="https://github.com/user-attachments/assets/8c392caf-6ed3-4526-bf5a-7714c346677e" />







1.5 Verify Domain Join


<img width="1918" height="1022" alt="verification 1" src="https://github.com/user-attachments/assets/5ade6453-bf39-4842-981d-da4da7d88cb9" />
<img width="1918" height="1025" alt="verification 2" src="https://github.com/user-attachments/assets/b8a45e83-7d0b-48e5-8d44-8b1429b2d495" />







Step 2: Install DHCP on Your Domain Controller (HomeLabDC01)



On your Windows Server 2019 DC, open Server Manager.

Click Add roles and features (top right).

Click Next until you reach the Server Roles section.

Check the box for DHCP Server.

A pop-up will appear asking to add features – click Add Features.

Click Next through the rest of the wizard (accept defaults).

On the Confirmation page, click Install.

Wait for the installation to complete, then click Close.


<img width="1918" height="1018" alt="addDHCPserver 1" src="https://github.com/user-attachments/assets/44193ef6-0021-4d1c-adbc-909000b688a2" />



2.1  Configure DHCP Scope

In Server Manager, go to Tools → DHCP (this opens the DHCP console).

In the left pane, expand your server name → right-click IPv4 → New Scope.

The New Scope Wizard will open:

Name: Enter Corp LAN Scope (or anything descriptive).

Description: DHCP scope for yourdomain.local clients

IP Address Range:

Start IP: 192.168.0.100

End IP: 192.168.0.200

Length: 24 (this matches the /24 subnet)

Subnet mask: 255.255.255.0

Exclusions: Leave blank (for now).

Lease Duration: Keep default (8 days).

Configure DHCP Options: Select Yes, I want to configure these options now.

Router (Default Gateway): Enter your router's IP (e.g., 192.168.0.1 – this is your home router or pfSense if you have one).

Domain Name: domain.local

DNS Servers: 192.168.0.50 (your DC)

Activate Scope: Select Yes, I want to activate this scope now.

Click Finish.

<img width="1918" height="1022" alt="dhcp scope 1" src="https://github.com/user-attachments/assets/fcb090f9-7224-4ac9-8489-d5e4d928af33" />


2.2 Authorize DHCP in Active Directory


DHCP must be authorized in AD to work with domain clients.

In the DHCP console, right-click your server name → Authorize.

Wait a few seconds, then right-click again → Refresh.

You should see a green checkmark icon next to your server name.

<img width="1918" height="1021" alt="dhcpauth 1" src="https://github.com/user-attachments/assets/b82fd5fa-7721-4dd4-81d0-ccfea19c880f" />



2.3: Install NAT on Your Domain Controller


This step allows your Windows 11 client to access the internet through your DC.

In Server Manager, click Add roles and features again.

Click Next until you reach Server Roles.

Check the box for Remote Access.

Click Add Features when prompted.

Click Next until you reach the Role Services page.

Check the box for Routing.

Click Add Features if prompted.

Click Next through the rest and click Install.

Wait for the installation to complete, then click Close.


2.4 Step 5: Configure NAT


In Server Manager, go to Tools → Routing and Remote Access.

Right-click your server name → Configure and Enable Routing and Remote Access.

The Routing and Remote Access Setup Wizard will open:

Click Next.

Select Network address translation (NAT) → Click Next.

Select "Use this public interface to connect to the internet".

Choose the network adapter that has internet access:

Ethernet (the one with the IP 192.168.0.x) – this assumes your home router gives your DC internet access.

Check "Enable security firewall on this interface" (optional).

Click Next → Finish.

<img width="1918" height="1020" alt="configNAT 1" src="https://github.com/user-attachments/assets/d3fbdc6a-509e-415a-be42-823f0965a73b" />



2.5  Set Windows 11 Client to Obtain IP and DNS Automatically


Now that DHCP is set up, we need to switch your client from manual IP to automatic.

On your Windows 11 client, right-click Start → Network Connections.

Click "Advanced network settings" → "More network adapter options".

Right-click your active adapter → Properties.

Select "Internet Protocol Version 4 (TCP/IPv4)" → click Properties.

Select:

☑️ Obtain an IP address automatically

☑️ Obtain DNS server address automatically

Click OK → Close.



2.6 Release and Renew IP Address


Open Command Prompt as Administrator.

Run:

#ipconfig /release

#ipconfig /renew

#ipconfig /all

#ping 8.8.8.8

You should now see:

IPv4 Address: 192.168.0.100 or similar (within your DHCP range)

DHCP Enabled: Yes

DHCP Server: 192.168.0.50

DNS Servers: 192.168.0.50

Connection-specific DNS Suffix: yourdomainip.local


<img width="1915" height="1017" alt="releaseandrenewip 1" src="https://github.com/user-attachments/assets/f9a09d93-0cce-47bc-9136-ad9306b3daa5" />

<img width="1918" height="1021" alt="ipconfig 2" src="https://github.com/user-attachments/assets/ccf6473a-0d27-4963-82ad-bc308c621a54" />



Step 3: File Server Setup

3.1 Install File Server Role on Your DC


On your Windows Server 2019 DC, open Server Manager.

Click Add roles and features.

Click Next until you reach Server Roles.

Check the box for File and Storage Services (it may already be installed by default).

Expand it and make sure File Server is checked (it usually is).

Click Next through the rest and click Install (if needed).

Click Close when done.






3.2 Create Folder Structure


On your DC, open File Explorer.

Navigate to the C: drive.

Create a new folder called Shares.

Inside Shares, create three subfolders:

IT

Sales

HR







