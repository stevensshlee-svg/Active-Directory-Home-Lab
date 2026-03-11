# Ticket #1: User cannot create a file

## Scenario
User Tupac Shakur (Sales Department) reported that he was unable to create a new file within the Sales shared folder. The user confirmed that he could successfully access and browse the folder contents but received an access denied message when attempting to create a new file.  

Key Details:
* User name: Tupac Shakur
* Department: Sales
* Issue: Unable to create files on Sales share  
* Error: "you need permission to perform this action"

## Environment / Context
Because the issue involved file creation, the investigation focused on permission related configurations.
- Windows Server 2019 Domain Controller
- Shared folder hosted on the Domain Controller
- Share-level permissions configured
- NTFS permissions assigned via security groups
- Security group: LA-Sales-RW

## Investigation
1. Confirmed the user could access and browse the folder (TechSolutionsINC).
2. Attempted to create a file as the user. Received "Access denied" error with "you need permission to perform this action" message.
![salescannotcreatefile](images/sales%20cannot%20create%20file.png)

3. Checked the scope to determine whether the issue was relative to a single user or multiple.
4. Confirmed the issue also affected users in other departments as well.
![itcannotcreatefile](images/IT%20cannot%20create%20file%20as%20well.png)

5. Verified the user was in the correct security group (LA-Sales-Share-RW).
![validatinggroupmembership](images/validating%20group%20membership.png)
![validatinggroupmembershipdomain](images/validating%20group%20membership%20domain%20side.png)  

6. Reviewed NTFS permissions and confirmed the group had Modify access.  
![salesfolderntfs](images/sales%20folder%20ntfs%20permissions.png)

7. Reviewed Share permissions and found Authenticated Users had "Read only" access.  
![techsolutionsharepermissions](images/techsolutions%20share%20permissions.png)

8. Identified that hare level permissions were restricting write access despite NTFS allowing it.

## Root Cause Analysis (RCA)
Share permissions were configured as "Read only", which prevented write operations over the network. Although NTFS permissions granted Modify access, effective permissions over SMB are determined by the most restrictive combination of Share and NTFS permissions.

The "Read only" share setting overrode the NTFS write permissions, preventing file creation.

## Remediation / Verification
- Updated the Share permissions to grant Authenticated Users "Change" access  
![changepermission](images/enable%20change%20permission%20on%20parent%20folder.png)
- User was then able to successfully create a file
![docucreated](images/document%20created.png)
![filecreatedIT](images/file%20created%20IT%20department.png)

- No additional permission issue was observed

## Lessons Learned
- Effective Network permissions are determined by both Share and NTFS permissions.
- Share permissions should be set broadly, while NTFS permission are more fine tuned.
- It is important to verify both layers when troubleshooting access issues. 
