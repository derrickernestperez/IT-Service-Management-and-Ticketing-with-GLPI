# INC-002 — Password Reset Request

## Ticket Details

| Field | Value |
|---|---|
| Portfolio ID | INC-002 |
| GLPI Ticket ID | #2 |
| Ticket type | Service Request |
| Category | Accounts and Access > Password Reset |
| Status | Closed |
| Urgency | Medium |
| Impact | Medium |
| Priority | Medium |
| Support level | Level 1 |
| Requester | Employee User |
| Assigned technician | Service Desk Technician |
| Affected account | `john.smith` |
| Affected workstation | CLIENT01 |
| Active Directory server | SRV01 |
| Ticketing server | GLPI01 |

---

## Issue Type

This was an Active Directory password reset service request.

The employee could not sign in because the current password had been forgotten.

The request affected one employee account and required a secure temporary password followed by a mandatory password change at the next sign-in.

---

## User Report

> Hi, I forgot my password and I can’t sign in to my workstation. I tried the passwords I usually use, but none of them worked. Can you please help me reset it? Thanks.

![Request submitted](images/INC-002-01-request-submitted.png)

---

## Systems Involved

| System | Purpose |
|---|---|
| `CLIENT01` | Domain-joined workstation used by the employee |
| `SRV01` | Active Directory Domain Services server |
| `GLPI01` | GLPI ticketing server |

---

## Technician Acknowledgement

The request was assigned to the Service Desk Technician and moved to `Processing`.

> Hi, I’ve received your password reset request. I’ll verify the account status in Active Directory before resetting the password. Once completed, you’ll be required to create a new password when you next sign in.

![Ticket assigned and acknowledged](images/INC-002-02-ticket-assigned-and-acknowledged.png)

---

## Account Verification

Before resetting the password, the employee account was checked on `SRV01`:

```powershell
Get-ADUser -Identity "john.smith" `
  -Properties Enabled, LockedOut, PasswordExpired, PasswordLastSet |
Select-Object Name, SamAccountName, Enabled, LockedOut, PasswordExpired, PasswordLastSet
```

The account state showed:

```text
Account enabled : True
Account locked  : False
```

This confirmed that the account was active and that the issue was not caused by an account lockout or disabled account.

![Account state verified](images/INC-002-03-account-state-verified.png)

---

## Root Cause

The employee had forgotten the current domain password.

The account was active and not locked, but the employee could not provide the correct password needed for authentication.

```text
Current password forgotten
            ↓
Authentication unsuccessful
            ↓
Employee unable to sign in
            ↓
Secure password reset required
```

---

## Remediation

A secure temporary password was entered using PowerShell without displaying it in plain text:

```powershell
$newPassword = Read-Host "Enter temporary password" -AsSecureString
```

The Active Directory password was reset:

```powershell
Set-ADAccountPassword `
  -Identity "john.smith" `
  -Reset `
  -NewPassword $newPassword
```

The account was configured to require a password change at the next sign-in:

```powershell
Set-ADUser `
  -Identity "john.smith" `
  -ChangePasswordAtLogon $true
```

The temporary password was not included in GLPI, GitHub, screenshots, or documentation.

![Password reset completed](images/INC-002-04-password-reset-completed.png)

---

## Password Change Requirement

The employee attempted to sign in to `CLIENT01` using the temporary password.

Windows required the employee to create a new private password before access was granted.

![Password change required](images/INC-002-05-password-change-required.png)

The employee entered:

1. The temporary password.
2. A new private password.
3. The new private password again for confirmation.

The final employee-selected password was not recorded by the technician.

---

## Validation

After changing the password, the employee signed in to `CLIENT01` using the new password.

The sign-in completed successfully, confirming that:

- The password reset was successful.
- The mandatory password change worked.
- The employee could access the workstation.
- The temporary password was no longer required.

![New password sign-in validated](images/INC-002-06-new-password-sign-in-validated.png)

---

## Technician Update

> Hi, your password has been reset successfully. Please sign in using the temporary password provided through the approved secure method. Windows will ask you to create a new private password before opening the desktop.

---

## Official Solution

> The Active Directory password for the employee account was reset securely. The account was configured to require a password change at the next sign-in. The employee created a new private password and successfully accessed the workstation.

The ticket status was changed to `Solved`.

![Ticket solved](images/INC-002-07-ticket-solved.png)

---

## User Confirmation

The employee confirmed that the password change and workstation sign-in were successful.

> I changed the temporary password and was able to sign in using my new password. Everything is working now. Thank you.

![User confirmation](images/INC-002-08-user-confirmation.png)

The solution was accepted, and the ticket status changed to `Closed`.

![Ticket closed](images/INC-002-09-ticket-closed.png)

---

## Validation Results

| Validation check | Result |
|---|---|
| Employee account identified | Passed |
| Account enabled state confirmed | Passed |
| Account lockout state checked | Passed |
| Temporary password created securely | Passed |
| Active Directory password reset | Passed |
| Password change at next sign-in enabled | Passed |
| Employee created a new private password | Passed |
| Workstation sign-in completed | Passed |
| Sensitive password values excluded from documentation | Passed |
| Employee confirmed restored access | Passed |
| Ticket closed | Passed |

---

## Ticket Timeline

| Step | Action | Result |
|---:|---|---|
| 1 | Employee forgot the current password | Workstation sign-in unavailable |
| 2 | Employee submitted Ticket #2 | Service request recorded |
| 3 | Technician accepted and acknowledged the request | Ticket moved to Processing |
| 4 | Employee account status checked | Account active and not locked |
| 5 | Temporary password created securely | Password value protected |
| 6 | Active Directory password reset | Temporary credentials assigned |
| 7 | Password change at next sign-in enabled | Security requirement applied |
| 8 | Employee signed in using the temporary password | Password change prompt displayed |
| 9 | Employee created a new private password | Temporary password replaced |
| 10 | Employee signed in successfully | Workstation access restored |
| 11 | Technician recorded the solution | Ticket moved to Solved |
| 12 | Employee confirmed the fix | Ticket closed |

---

## Closure

| Closure check | Result |
|---|---|
| Request verified | Yes |
| Account status reviewed | Yes |
| Password reset securely | Yes |
| Mandatory password change applied | Yes |
| Temporary password protected | Yes |
| Employee created a private password | Yes |
| Workstation access restored | Yes |
| User validation completed | Yes |
| Official solution recorded | Yes |
| Ticket closed | Yes |

---

## How the Issue Was Resolved

The employee reported being unable to sign in because the current password had been forgotten.

I checked the account `john.smith` in Active Directory and confirmed that it was enabled and not locked. This showed that the issue required a password reset rather than an account unlock.

I created a secure temporary password, reset the account password, and enabled the requirement to change the password at the next sign-in.

The employee signed in using the temporary password, created a new private password, and successfully accessed `CLIENT01`. The employee then confirmed that the account was working normally.

---

## Technician Insight

I checked the account status before resetting the password to make sure the sign-in problem was not caused by a disabled or locked account.

The temporary password was entered securely and was never included in the ticket, screenshots, or repository. I also required the employee to change it during the next sign-in so that only the employee knew the final password.

I closed the request only after the employee created a new password and confirmed successful access to the workstation.

---

## Evidence Files

```text
images/INC-002-01-request-submitted.png
images/INC-002-02-ticket-assigned-and-acknowledged.png
images/INC-002-03-account-state-verified.png
images/INC-002-04-password-reset-completed.png
images/INC-002-05-password-change-required.png
images/INC-002-06-new-password-sign-in-validated.png
images/INC-002-07-ticket-solved.png
images/INC-002-08-user-confirmation.png
images/INC-002-09-ticket-closed.png
```

---

## Final Status

```text
Portfolio ID             : INC-002
GLPI Ticket ID           : #2
Ticket type              : Service Request
Request                  : Password Reset
Affected account         : john.smith
Account enabled          : Yes
Account locked           : No
Password reset completed : Yes
Password change required : Yes
Workstation access       : Restored
User confirmed           : Yes
Final status             : CLOSED
```
