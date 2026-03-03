# Overview
To simulate a real user environment, I configured a GPO to map network drives instead of using a traditional logon script. Each department share was mapped based on security group membership using item level targeting. This allowed access to be controlled dynamically without modifying scripts for individual users.

## Why I used GPP instead of logon scripts
When mapping network drives, I had two options: use a logon script or configure it through Group Policy Preferences. I chose GPP because it is more scalable and easier to manage in a multi user environment.  

Logon scripts work, but they can become harder to maintain over time. If targeting different departments, you either need multiple scripts or more advanced scripting logic. Troubleshooting can also be less straightforward if something fails silently.  

With Group Policy Preferences, I was able to use item level targeting to apply drive mappings based on security group membership. This keeps everything centralized within Group Policy and removes the need for additional scripting. It also aligns well with the AGDLP structure I already implemented.  

## Validation
After creating the GPO, I configured separate drive letters for each department share. I then used item level targeting so that only users in the appropriate security group would receive the mapped drive. 
These are the LA and NY HR and Finance department mappings in the GPP:
<img width="1003" height="723" alt="mapping network ny hr" src="https://github.com/user-attachments/assets/e7f0f6be-38fd-44d4-8444-7458bb768248" />
<img width="994" height="713" alt="mapping network ny finance" src="https://github.com/user-attachments/assets/ddf07928-f321-4261-92e0-5b3a97ab180b" />
<img width="1005" height="722" alt="mapping network la hr" src="https://github.com/user-attachments/assets/cd88d568-19a4-4201-bee4-5271eff86ac8" />
<img width="1012" height="725" alt="mapping network la finance" src="https://github.com/user-attachments/assets/434ccffb-4fbc-403c-9802-04980fa52a3d" />


When a user logs in, the GPO checks their group membership. If they are part of the security group, the drive is mapped automatically. If they aren't, nothing is applied. Testing with different user accounts confirmed that the mapping behaved as expected.  
<img width="991" height="652" alt="testing mapped drive as eminem" src="https://github.com/user-attachments/assets/d8e6ce6a-aaf0-4fab-a03a-86a3a073de91" />  
Accessing the IT Share as user Eminem (member of the IT department)
