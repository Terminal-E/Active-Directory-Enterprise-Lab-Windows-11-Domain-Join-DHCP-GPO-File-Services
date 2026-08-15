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






1.5 Verify Domain Join




1.6 








