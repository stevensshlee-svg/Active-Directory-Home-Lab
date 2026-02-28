# Objective
Before deploying Active Directory, I wanted to build a stable properly configured network. Since Active Directory relies heavily on DNS and consistent IP addressing, my goal was to ensure the environment was configured correctly at the infrastucture level before introducing domain services. This section documents how I configured the server, deployed Active Directory Domain Services, and validated internal DNS functionality.  

## Server Configurations
I began by creating a Windows Server 2019 and a Windows 10 client vm on Virtualbox, then I assigned static IP addresses to the Windows Server (192.168.1.55) and Windows 10 client (192.168.1.34). The reason I decided to assign a static IP address is because dynamic IP addresses can cause DNS inconsistencies, authentication failures, and issues locating domain services. It is critical for the Domain Controller to always be reachable by clients.  

I also manually assigned the servers DNS IP address to be its own after promoting the server as a Domain Controller because the Domain Controller would also serve as the primary DNS server for the domain. This reinforced the principle that domain-joined systems should rely on an internal DNS rather than external DNS provider. 

## Active Directory Domain Services Deployment
After confirming network connectivity, I installed the AD DS role and promoted the server to a Domain Controller / DNS server. During the process, I decided to created a new forest, TechSolutionsINC, so I could fully control the structure and configuration decisions. Key-takeaways I got from this part was the Domain Controller also acts as a directory database, DNS integration is automatic during promotion, and the promotion process configures important services such as Kerberos and LDAP in the background. 

## Validation Steps
Upon promoting the server, I validated DNS functionality. I confirmed via nslookup on the domain itself to ensure hostname resolution. After confirming resolution, I joined the Windows 10 client machine to the domain logging in with a domain account. 
<img width="977" height="508" alt="nslookup" src="https://github.com/user-attachments/assets/a4a542f1-eb01-49d0-b069-4d6b797b7de1" />
<img width="1019" height="853" alt="tupac shakur" src="https://github.com/user-attachments/assets/aef7d072-e978-41dd-9c7b-6f69cfa01b83" />

## Mistakes and Lessons learned
One mistake I learned early on was the importance of an internal DNS server. I originally created two Domain Controllers to offer redundancy. My mistake was setting the primary Domain Controller's primary DNS servers IP address to the secondary server and vice versa. I forgot to turn on the secondary server which broke DNS functionality within my domain.
