# Overview
After getting the Domain Controller running, I moved on to designing the structure of the domain. I didn’t want to rely on the default “Users” container, so I created an OU structure that simulated a small company with two locations: Los Angeles and New York.

Within each location, I separated objects into:
* Users
* Computers
* Groups
![domain-overview](images/Domain%20overview.png)  

I wanted the structure to feel realistic and easy to manage if this were a real environment.

## Why I structure it this way
I organized the domain by location first, then by object type. My reasoning was that location based OUs would make it easier to apply site specific Group Policies later if needed.

Separating Users and Computers makes policy targeting cleaner and more predictable. Since user based policies and computer-based policies are processed differently, keeping them in separate OUs reduces confusion and makes troubleshooting easier.

I also placed security groups in their own OU to keep access control organized and easier to review. My goal was to build something that would scale if the company expanded, rather than something that only worked for a small lab.

## Lessons Learned
Designing the OU structure helped me understand that OUs aren’t just folders, they directly impact how Group Policies apply and how administrative control can be delegated. Therefore, this part of the lab made me slow down and plan before building. A clean structure at the beginning prevents unnecessary restructuring later.
