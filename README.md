# Azure Cloud Identity: End-to-End AD DS Deployment & Troubleshooting

## Project Overview
This project involved the deployment of a scalable Enterprise Identity Management system within Microsoft Azure(Already created and configured Ressource Groupes, Virtual Networks and VM's). I established a secure Virtual Network (VNet) environment, promoted a Windows Server 2022 instance to a Domain Controller, and successfully integrated a Windows 11 client, And added users, reated and configured GPO's. 
The core focus of this lab was navigating real-world cloud networking hurdles, specifically focusing on DNS resolution and Network Security Group (NSG) rule optimization.

---

## Technical Skills Demonstrated
* **Infrastructure:** Azure Virtual Machines, VNet Peering, Private IP Management.
* **Security:** NSG Inbound/Outbound Rule Configuration, Least Privilege Access.
* **Identity:** Active Directory Domain Services (AD DS), User/Group Provisioning.
* **Diagnostics:** ICMP Troubleshooting, PowerShell (Test-NetConnection), DNS SRV Record verification.

---

## Deployment & Implementation Journey

### 1. The Architectural Foundation
I initiated the lab by deploying two Virtual Machines within the same Azure VNet. To ensure stable identity services, I assigned a static private IP to the Domain Controller(DC-05) and updated the Client VM’s(NewVM) DNS settings to point directly to it.

![DNS Server Configuration](assets/02-dns-server-configuration.png)

### 2. The Connectivity Hurdle (Troubleshooting Phase)
A critical issue arose where the Client VM could not communicate with the Domain Controller despite being in the same subnet. 

* **The Symptom:** `ping` requests timed out, and the Domain Join failed with "Network Path Not Found."
* **The Diagnosis:** I identified that the default Azure NSG was dropping traffic necessary for Active Directory communication.

![Connectivity Troubleshooting](assets/03-connectivity-troubleshooting-ping-fail.png)

### 3. NSG Optimization & Resolution
I manually configured the Inbound Security Rules for the DC's NSG, by opening specific ports: **DNS (53), Kerberos (88), LDAP (389), and SMB (445)**. Then I established the necessary "handshake" between the machines.

![NSG Verification](assets/04-connectivity-nsg-verification-ping-success.png)

### 4. Domain Integration & User Provisioning
With connectivity confirmed via PowerShell, I performed the formal Domain Join. I authenticated the Windows 10 machine using the Domain Administrator credentials and verified its placement in the Active Directory Administrative Center (ADAC).

![Domain Join Success](assets/06-domain-join-success-confirmation.png)

### 5. Final Verification: User Logon
To conclude the lab, I created a test user account (`Caw.Covi`) on the DC-05. I successfully logged into the Windows 11 workstation using these domain credentials, confirming that the authentication flow was fully functional.

![Final User Login](assets/10-final-domain-user-login-success.png)

---

## Conclusion
This lab reinforces the importance of DNS and firewall management in cloud environments. Successfully joining a machine to a domain isn't just about clicking "Join"; it's about understanding the underlying protocols (LDAP, Kerberos, DNS) that allow enterprise-grade identity management to function.
*Developed by [Taki] as part of an Advanced Azure Systems aand Active Directory Administration Portfolio.*
