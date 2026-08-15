# Active-Directory-Enterprise-Lab-Windows-11-Domain-Join-DHCP-GPO-File-Services
A Windows Server 2019 homelab project building on an existing Active Directory infrastructure – adding Windows 11 Pro domain integration, automated IP assignment, centralized file storage, and Group Policy enforcement for a complete corporate environment. YOU WILL DO SOME FORM OF THIS AT WORK

Complete Lab Structure



Step 0	Active Directory Foundation	✅ Already completed – DC, OUs, users, security groups(Check my last Lab this step should be completed prior to moving forward!) GOTO LINE 16

Step 1	Windows 11 Domain Join	Join Windows 11 Pro to your domain with proper DNS configuration

Step 2	DHCP & NAT	Install/configure DHCP for automatic IP assignment + NAT for internet access

Step 3	File Server	Create departmental shares with NTFS/Share permissions based on your existing security groups

Step 4	Group Policy	Enforce drive mappings, folder redirection, wallpaper, and software deployment on Windows 11


Make sure you have before Step 1

□ Windows 11 Pro VM installed (not Home edition – Home can't join domains)



□ VM network adapter set to the same virtual network as your DC (ex. NAT or Internal network in Hyper-V/VMware)



□ DC is powered on and reachable



□ DC's IP address written down (ex 192.168.1.10)



□ Domain name written down (ex. yourdomain.local)



□ Domain admin credentials (ex. YOURDOMAIN\Administrator)



□ A test domain user credentials to verify login after join

Step 1: Join Windows 11 Pro to the Domain
 1.1: Configure DNS on Windows 11
<img width="1918" height="1022" alt="win11todcdns 1" src="https://github.com/user-attachments/assets/1622fe62-d7e2-4415-a6d3-800dc9d0486d" />

 1.2 Test DNS Resolution
 <img width="1918" height="1020" alt="dnstestresults 2" src="https://github.com/user-attachments/assets/361974be-0dad-47be-8d56-2fdf728649c1" />

1.3 Join the Domain





1.4 Log in as a Domain User




1.5 Verify Domain Join




1.6 Verify Domain Join








