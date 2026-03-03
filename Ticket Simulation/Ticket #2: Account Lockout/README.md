# Ticket #2: Account Lockout

## Scenario
User Kendrick Lamar (Sales Department) reported he was unable to log in to his workstation. The user stated he was certain he was entering the correct password but continued receiving an “Account locked out” message.

Key Details:
* User: Kendrick Lamar
* Department: Sales
* Issue: Unable to log in
* Error Message: "The referenced account is currently locked out"

## Environment / Context
* Windows Server 2019 Domain Controller
* Active Directory Domain Services User and Computers
* Account Properties

## Investigation
1. Attempted to replicate the issue at the user's workstation
2. Confirmed an error message indicating an account lockout
<img width="1016" height="849" alt="kendrick locked out" src="https://github.com/user-attachments/assets/a7b549ac-a20a-4f1b-99c5-b23bc9878a55" />

3. Checked Domain Controller Security Logs via Event Viewer
4. Located Event 4740 (account lockout) and event origin
<img width="816" height="585" alt="event 4740" src="https://github.com/user-attachments/assets/a3ef9f26-992e-4d60-b4a8-db649c60a82d" />

5. Checked Active Directory Users and Computers (ADUC) for Kendrick Lamar's account properties
6. Verified the user account was locked
<img width="930" height="671" alt="kendrick account properties" src="https://github.com/user-attachments/assets/9b1a1124-fd54-406d-a4b5-c4e6309ba71e" />


## Root Cause Analysis
The user account was locked due to multiple failed login attempts, triggering the domains account lockout policy threshold.

## Remediation / Validation
* Unlocked user Kendrick Lamar's account on the Domain Controller via account properties tab
* Configured a temporary password and enabled "User must change password at next logon" to allow the user to configure a new password
<img width="406" height="537" alt="remediation" src="https://github.com/user-attachments/assets/b5ae1b84-1f2d-4590-a19a-6ca3585cc872" />

* Verified user connectivity by successfully logging on
<img width="1020" height="789" alt="account unlocked and logged in" src="https://github.com/user-attachments/assets/20cb77e6-a0c0-479c-8797-c9ac76b37c47" />


## Lessons Learned
Account lockouts can occur for various reasons, such as mistyped passwords, failure to update a password, or cached old credentials on a system. It is always best practice to review the event logs to identify the source of the lockout. Repeatedly unlocking an account without performing root cause analysis can lead to recurring issues; therefore, always determine the origin of the lockout before unlocking the account.
