# Overview
After building out the OU structure, I started creating users and security groups by department. Each department had its own groups for different access levels (Read Only and Read/Write). I followed a simplified AGDLP approach by putting users into department groups, then nesting those into resource access groups. My goal was to avoid assigning permissions individually to users. 
<img width="749" height="531" alt="LA-groups" src="https://github.com/user-attachments/assets/7a127404-4285-4e23-bbc7-c206d3785b9c" />
<img width="943" height="671" alt="LA-sales-users member example" src="https://github.com/user-attachments/assets/00ca503c-e61e-4b3c-884b-9e306cfe126a" />
<img width="935" height="675" alt="NY-groups" src="https://github.com/user-attachments/assets/34597494-9dba-491b-b5ba-a85c7c75632d" />
<img width="939" height="666" alt="NY-hr-users member example" src="https://github.com/user-attachments/assets/47703d81-7ca6-40e4-8993-3fbc91e68600" />
<img width="940" height="675" alt="security groups" src="https://github.com/user-attachments/assets/5b981c5e-dfc9-4384-94a3-f2b997c099cf" />
- There are three groups per department: LA-HR-Users, LA-HR-Share-RW, LA-HR-Share-RO
## Why I used groups instead of direct permissions
Managing permissions at the user level doesn't scale. If someone leaves or changes departments, it becomes harder to manage. Using groups allowed me to enforce least privilege, remove access cleanly when needed, and avoid permission creep. It also made troubleshooting easier because I could check group membership first when testing access issues. 

## Lessons Learned
This part made me realize that access control isn’t just about giving someone entry to a folder. It’s about keeping the environment manageable over time. A clean group structure makes changes easier and reduces the chances of leftover permissions causing problems later.
