# Objective
Before deploying Active Directory, I wanted to build a stable, properly configured network foundation. Since Active Directory relies heavily on DNS and consistent IP addressing, my goal was to ensure the environment was correctly configured at the infrastructure level before introducing domain services.

This section documents how I configured the server, deployed Active Directory Domain Services (AD DS), and validated internal DNS functionality.  

## Server Configurations
I deployed a Windows Server 2019 virtual machine and a Windows 10 client VM using VirtualBox. I assigned static IP addresses to both systems:
* Domain Controller: 192.168.1.55
* Windows 10 Client: 192.168.1.34

I chose static addressing because dynamic IP changes can cause DNS inconsistencies, authentication failures, and difficulty locating domain services. A Domain Controller must remain consistently reachable by clients in order for authentication and service discovery to function properly.

After promoting the server to a Domain Controller, I manually configured its preferred DNS server to point to itself. Since the Domain Controller also serves as the primary DNS server for the domain, this reinforced the principle that domain-joined systems should rely on internal DNS rather than external providers.

## Active Directory Domain Services Deployment
After confirming basic network connectivity, I installed the AD DS role and promoted the server to a Domain Controller and DNS server.  
During promotion, I created a new forest named TechSolutionsINC, which allowed me to fully control the domain structure and configuration decisions.  
Key takeaways from this phase included:
* The Domain Controller functions as both an authentication service and a directory database.
* DNS integration is automatically configured during promotion.
* Critical services such as Kerberos and LDAP are established in the background to support authentication and directory queries.

Understanding what the promotion process configured behind the scenes helped reinforce how tightly integrated DNS and authentication services are within Active Directory. 

## Validation Steps
After promotion, I validated DNS functionality using nslookup to confirm hostname resolution within the domain. I also verified that the forward lookup zone was created and functioning properly.  
Once DNS resolution was confirmed, I successfully joined the Windows 10 client machine to the domain and logged in using a domain account. This verified both authentication services and proper DNS configuration.  
Rather than assuming functionality, I intentionally tested domain join and login workflows to simulate real-world validation practices.
<img width="977" height="508" alt="nslookup" src="https://github.com/user-attachments/assets/a4a542f1-eb01-49d0-b069-4d6b797b7de1" />
<img width="1019" height="853" alt="tupac shakur" src="https://github.com/user-attachments/assets/aef7d072-e978-41dd-9c7b-6f69cfa01b83" />

## Mistakes and Lessons learned
While building the lab, I initially deployed two Domain Controllers to simulate redundancy. However, I misconfigured their DNS settings by pointing each server’s preferred DNS address to the other Domain Controller.    
Because DNS services were not available on the secondary server (I forgot to power on the secondary server), the primary Domain Controller was unable to resolve properly, which disrupted DNS functionality across the domain.  
This mistake reinforced several important lessons:
* Domain Controllers must always be reachable if referenced as DNS servers.
* Service availability directly impacts authentication.
* Redundancy requires proper configuration and monitoring to be effective.
Troubleshooting this issue strengthened my understanding of DNS dependency within an Active Directory environment.
