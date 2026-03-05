# Overview
I created a shared folder structure that mirrored the company layout. Company share (TechSolutionsINC) > location (Los Angeles & New York) > department folders (HR, Sales, IT, etc). Rather than sharing everything openly, I configured both Share and NTFS permissions to provide layered secure access control.

## Permission Configurations
* At the Share level, I kept it simple and allowed broader access. I allowed Read and Change access to Authenticated Users.
* At the NTFS level, I removed inheritance where needed and applied permissions using department-based security groups.
<img width="781" height="590" alt="techsolutions company data share" src="https://github.com/user-attachments/assets/26a9bf60-6dd1-4a3a-b7ee-c56547f83030" />
<img width="360" height="444" alt="techsolutions parent folder share permissions" src="https://github.com/user-attachments/assets/6c85f685-4612-42f1-b67c-f2fafe3cfd03" />
<img width="357" height="479" alt="la-finance-share-ntfs" src="https://github.com/user-attachments/assets/13fd4a1d-457e-4fae-b6d0-dc6ef94e0199" />  

This allowed me to control exactly who could read or modify content.  
Trying to access New York Share and LA Finance Share as user Tupac (LA-Sales)
<img width="1018" height="850" alt="denied permission to ny as tupac" src="https://github.com/user-attachments/assets/7910a186-abb4-438d-83a0-4abb7a701c8e" />
<img width="1019" height="849" alt="denied permission to finance as tupac" src="https://github.com/user-attachments/assets/9bc3a64f-db2c-4b99-8e23-321e704b8e3b" />  
Accessing La-Sales Share as Tupac
<img width="1012" height="846" alt="sales folder as tupac" src="https://github.com/user-attachments/assets/000fe10f-9f5c-4e17-ab81-4345d48ff243" />


## Lessons Learned
During testing, I noticed users couldn’t create files in their department folders even though they had Modify permissions at the NTFS level. Initially I assumed it was an NTFS misconfiguration, but after reviewing both layers, I realized the issue was at the Share level. The Share permissions were more restrictive than I intended. Since the most restrictive permission between Share and NTFS takes precedence, users were effectively limited despite having proper NTFS rights. This helped reinforce how Share and NTFS permissions work together. Even if NTFS is configured correctly, overly restrictive Share permissions will still block access. After correcting the Share permissions, file creation worked as expected.
