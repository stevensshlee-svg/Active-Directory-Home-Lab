# Ticket #1: User cannot create a file

## Scenario
User Tupac Shakur (Sales Department) reported that he was unable to create a new file within the Sales shared folder. The user confirmed that he could successfully access and browse the folder contents but received an access denied message when attempting to create a new file.  

Key Details:
* User name: Tupac Shakur
* Department: Sales
* Issue: Unable to create files on Sales share  
* Error: "you need permission to perform this action"

## Environment / Context
Because the issue involved file creation, the investigation focused on permission-related configurations.
- Windows Server 2019 Domain Controller
- Shared folder hosted on the Domain Controller
- Share-level permissions configured
- NTFS permissions assigned via security groups
- Security group: LA-Sales-RW

## Investigation
1. Confirmed the user could access and browse the folder (TechSolutionsINC).
2. Attempted to create a file as the user. Received "Access denied" error with "you need permission to perform this action" message.
<img width="909" height="590" alt="sales cannot create file" src="https://github.com/user-attachments/assets/a33e359e-4350-4816-b142-b6d1d8b3ebac" />
3. Checked the scope to determine whether the issue was relative to a single user or multiple.
4. Confirmed the issue also affected users in other departments as well.
<img width="1004" height="826" alt="IT cannot create file as well" src="https://github.com/user-attachments/assets/f8de1480-11b1-48bc-b56d-9498aee422c2" />
5. Verified the user was in the correct security group (LA-Sales-RW).
<img width="970" height="510" alt="validating group membership" src="https://github.com/user-attachments/assets/0648c683-6519-41db-9812-8ee4e3c11718" />
<img width="407" height="537" alt="validating group membership domain side" src="https://github.com/user-attachments/assets/5edc1075-e5aa-4f24-b961-6e94b3214520" />
6. Reviewed NTFS permissions and confirmed the group had Modify access.
<img width="356" height="480" alt="sales folder ntfs permissions" src="https://github.com/user-attachments/assets/5fd25fa9-ebd4-40ad-ad5f-592d6a8f7944" />
7. Reviewed Share permissions and found Authenticated Users had Read-only access.
<img width="358" height="446" alt="techsolutions share permissions" src="https://github.com/user-attachments/assets/8994f24c-f15d-4194-855e-61fe57251fcd" />
8. Identified that share-level permissions were restricting write access despite NTFS allowing it.

## Root Cause Analysis (RCA)
Share permissions were configured as Read-only, which prevented write operations over the network. Although NTFS permissions granted Modify access, effective permissions over SMB are determined by the most restrictive combination of Share and NTFS permissions.

The Read-only share setting overrode the NTFS write permissions, preventing file creation.

## Remediation / Verification
- Updated the Share permissions to grant Authenticated Users "Change" access
<img width="357" height="445" alt="enable change permission on parent folder" src="https://github.com/user-attachments/assets/c432557a-e2eb-46ea-bd3c-c228caffd495" />
- User was then able to successfully create a file
<img width="909" height="593" alt="document created" src="https://github.com/user-attachments/assets/83927b4a-a027-43a7-89c8-78e42ffcaef6" />
- No additional permission issue was observed

## Lessons Learned
- Effective Network permissions are determined by both Share and NTFS permissions.
- Share permissions should be set broadly, while NTFS permission are more fine-tuned.
- It is important to verify both layers when troubleshooting access issues. 
