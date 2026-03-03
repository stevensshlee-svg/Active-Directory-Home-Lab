# Overview
After building the structure, I began implementing Group Policies.
I created and tested policies for: 
* Password requirements
* Account lockout thresholds
* Desktop wallpaper
* Control Panel restrictions
* Blocking access to removable media drives  

I linked policies to specific OUs to control the scope of the policy

## What I focused on
Rather than creating policies and applying them randomly, I focused on understanding how GPO processing works, the difference between enforced and non-enforced, security filtering, and how to troubleshoot when policies don't apply. 
I used gpupdate /force and gpresult /r to validate policy application  
"gpo link overview image"

## Lessons Learned
While configuring these policies, I started to really understand how GPO processing works. Policies follow LSDOU order which means Local, then Site, then Domain, then OU. They process from the top down in Active Directory, and the policy applied last is the one that wins if there is a conflict. Seeing this play out in my lab made it easier to understand why linking a GPO in the right place matters.
Local policies apply first on the machine itself. After that, site linked GPOs apply, then domain linked GPOs, and finally OU linked GPOs. If there are nested OUs, the parent OU policies apply before the child OU policies. Testing this helped me understand why a setting might not apply if it is linked at the wrong level.  

While configuring these policies, I focused on understanding how security and usability balance each other. Setting password length and account lockout policies showed me how small configuration changes can directly impact user behavior. Increasing complexity improves security, but stricter lockout thresholds can also create support overhead if users mistype their passwords too often.
<img width="818" height="522" alt="password gpo" src="https://github.com/user-attachments/assets/4f1ecc91-c181-418a-949a-4fc35be81cf8" />  

Disabling USB storage and restricting Control Panel access helped me see how GPO can reduce attack surface and limit what standard users can change on a system. At the same time, it made me think about how these restrictions would affect legitimate use cases in a real environment. It reinforced that policies shouldn’t just be secure — they need to be intentional.
<img width="865" height="576" alt="deny usb for finance dept" src="https://github.com/user-attachments/assets/bec82ded-16f6-4f99-8f21-97f490bf497b" />
<img width="819" height="525" alt="deny control panel   screen lockout timer" src="https://github.com/user-attachments/assets/aa73b36b-06c8-48ba-83e0-144ba9261291" />  

The desktop wallpaper policy was simple, but it helped me better understand User Configuration settings and how they apply differently from Computer Configuration policies. Testing different users in different OUs made it clear how scope and linking determine whether a policy actually applies.

<img width="860" height="569" alt="desktop screensaver gpo" src="https://github.com/user-attachments/assets/ed79d02f-cd1f-4397-9497-2c7703dc9a5f" />
<img width="1023" height="851" alt="koala gpo applied" src="https://github.com/user-attachments/assets/967f5764-ef79-44a0-aa16-5f704b947d87" />  

Overall, this section helped me understand that GPOs aren’t just settings you flip on. Where you link them, how they inherit, and who they apply to makes a big difference. Testing each policy after deployment was just as important as configuring it.
