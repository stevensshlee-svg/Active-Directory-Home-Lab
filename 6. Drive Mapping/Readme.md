# Overview
To simulate a real user environment, I configured a GPO to map network drives instead of using a traditional logon script. Each department share was mapped based on security group membership using item level targeting. This allowed access to be controlled dynamically without modifying scripts for individual users.

## Why I used GPP instead of logon scripts
When mapping network drives, I had two options: use a logon script or configure it through Group Policy Preferences. I chose GPP because it is more scalable and easier to manage in a multi user environment.  

Logon scripts work, but they can become harder to maintain over time. If targeting different departments, you either need multiple scripts or more advanced scripting logic. Troubleshooting can also be less straightforward if something fails silently.  

With Group Policy Preferences, I was able to use item level targeting to apply drive mappings based on security group membership. This keeps everything centralized within Group Policy and removes the need for additional scripting. It also aligns well with the AGDLP structure I already implemented.  

## Validation
After creating the GPO, I configured separate drive letters for each department share. I then used item level targeting so that only users in the appropriate security group would receive the mapped drive. 
These are the LA and NY HR and Finance department mappings in the GPP:
![mappinglahr](images/mapping%20network%20la%20hr.png)
![lafinance](images/mapping%20network%20la%20finance.png)
![mappingnyhr](images/mapping%20network%20ny%20hr.png)
![nyfinance](images/mapping%20network%20ny%20finance.png)


When a user logs in, the GPO checks their group membership. If they are part of the security group, the drive is mapped automatically. If they aren't, nothing is applied. Testing with different user accounts confirmed that the mapping behaved as expected.  
![testing](images/testing%20mapped%20drive%20as%20eminem.png)
Accessing the IT Share as user Eminem (member of the IT department)
