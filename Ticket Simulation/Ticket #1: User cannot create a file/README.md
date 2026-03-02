# Ticket #1: User cannot create a file

## Scenario
User Tupac Shakur (Sales Department) reported that he was unable to create a new file within the Sales shared folder. The user confirmed that he could successfully access and browse the folder contents but received an access denied message when attempting to create a new file.  

Important information:
* User name: Tupac Shakur
* Department: Sales
* Reported Issue: User cannot create files on the Sales Share  
User claims to receive a "you need permission to perform this action" error notification.

## Environment / Context
Because the user claimed there was an issue with creating a new file, we should factor in all things related to permissions.
- Windows 2019 Domain Controller
- Shared folder on the Domain Controller
- Share permissions
- NTFS permissions assigned
- Security groups

## Investigation
1. Confirmed the user could access and browse the folder (TechSolutionsINC).
2. Attempted to create a file as the user. Received "Access denied" error with "you need permission to perform this action" message.
3. Checked the scope to determine whether the issue was relative to a single user or multiple.
4. Confirmed the issue affected users in other departments as well.
5. Verified the user was in the correct security group (LA-Sales-RW), and verified the NTFS permissions of the security group.
6. Reviewed the share permissions: Authenticated Users had Read-only access.
7. Concluded that share-level permissions were taking precedence over NTFS.

## Root Cause Analysis (RCA)
Share permissions were set to Read-only, which restricted write operations despite NTFS permissions allowing modify access. When it comes to share, effective permissions are determined by the most restrictive between Share and NTFS permissions. 

## Remediation / Verification
- Updated the Share permissions to grant Authenticated Users "Change" access
- User was then able to successfully create a file
- No additional permission issue was observed

## Lessons Learned
- Effective NEtwork permissions are determined by both Share and NTFS permissions.
- Share permissions should be set broadly, while NTFS permission are more fine-tuned.
- It is important to verify both layers when troubleshooting access issues. 
