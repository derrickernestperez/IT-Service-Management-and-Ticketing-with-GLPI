# INC-001 — Active Directory Account Lockout

## Ticket Details

| Field | Value |
|---|---|
| Portfolio ID | INC-001 |
| GLPI Ticket ID | #1 |
| Ticket type | Incident |
| Category | Accounts and Access > Account Lockout |
| Status | Closed |
| Urgency | Medium |
| Impact | Medium |
| Priority | Medium |
| Support level | Level 1 |
| Requester | Employee User |
| Assigned technician | Service Desk Technician |
| Affected account | `test.username` |
| Affected workstation | CLIENT01 |
| Active Directory server | SRV01 |
| Ticketing server | GLPI01 |

---

## Issue Type

This was an Active Directory account lockout incident.

The employee could not sign in to the domain because the account had been locked after several unsuccessful password attempts.

The issue affected one user account and prevented access to the assigned workstation.

---

## User Report

> Hi, I’m unable to sign in to my workstation. Windows keeps telling me that my account is locked. I may have entered the wrong password a few times. Can someone please help me regain access? Thanks.

![Ticket submitted](images/INC-001-01-ticket-submitted.png)

---

## Systems Involved

| System | Purpose |
|---|---|
| `CLIENT01` | Domain-joined workstation where the sign-in issue occurred |
| `SRV01` | Active Directory Domain Services server |
| `GLPI01` | GLPI ticketing server |

---

## Account Lockout Policy

An Active Directory account lockout policy was configured for the lab environment.

| Policy setting | Value |
|---|---:|
| Account lockout threshold | 5 invalid sign-in attempts |
| Account lockout duration | 30 minutes |
| Reset account lockout counter after | 10 minutes |

The policy was used to reproduce a realistic account lockout incident.

---

## Incident Reproduction

The employee account was intentionally locked by entering an incorrect password several times on `CLIENT01`.

After the lockout threshold was reached, Windows prevented further sign-in attempts and displayed an account lockout message.

The employee then submitted the incident through GLPI.

---

## Technician Acknowledgement

The ticket was assigned to the Service Desk Technician and moved to `Processing`.

> Hi, I’ve received your ticket. I’ll check the account status in Active Directory and confirm whether it has been locked. I’ll update you once I’ve restored access.

![Ticket assigned](images/INC-001-02-ticket-assigned.png)

---

## Investigation

The account lockout state was checked on `SRV01` using Active Directory PowerShell:

```powershell
Search-ADAccount -LockedOut -UsersOnly |
Select-Object Name, SamAccountName
```

The result showed that the employee account was listed as locked.

```text
SamAccountName : test.username
Account state  : Locked
```

This confirmed that the sign-in issue was caused by the Active Directory account lockout policy.

![Account lockout confirmed](images/INC-001-03-account-lockout-confirmed.png)

---

## Root Cause

The employee entered an incorrect password several times.

After five failed sign-in attempts, Active Directory applied the configured lockout policy and blocked further authentication attempts.

```text
Repeated incorrect password attempts
              ↓
Lockout threshold reached
              ↓
Active Directory account locked
              ↓
Employee unable to sign in
```

---

## Remediation

The account was unlocked on `SRV01` using:

```powershell
Unlock-ADAccount -Identity "test.username"
```

The account state was checked again to confirm that it was no longer listed as locked.

![Account unlocked](images/INC-001-04-account-unlocked.png)

---

## Validation

The employee returned to `CLIENT01` and signed in using the existing password.

The sign-in completed successfully, confirming that the account was active and accessible again.

![User sign-in validated](images/INC-001-05-user-sign-in-validated.png)

---

## Technician Update

> Hi, I confirmed that your account was locked after several unsuccessful sign-in attempts. I unlocked the account in Active Directory and verified that it is available again. Please try signing in using your current password.

![Technician follow-up](images/INC-001-06-technician-follow-up.png)

---

## Official Solution

> The employee account was confirmed as locked in Active Directory. The account was unlocked, and successful workstation sign-in was verified using the employee’s existing password.

The ticket status was changed to `Solved`.

![Ticket solved](images/INC-001-07-ticket-solved.png)

---

## User Confirmation

The employee confirmed that access to the workstation had been restored.

> I was able to sign in again using my password. Everything is working now. Thank you.

The solution was accepted, and the ticket status changed to `Closed`.

![Ticket closed](images/INC-001-08-ticket-closed.png)

---

## Validation Results

| Validation check | Result |
|---|---|
| Account lockout reproduced | Passed |
| Locked account identified in Active Directory | Passed |
| Correct user account verified | Passed |
| Account unlocked successfully | Passed |
| Existing password remained valid | Passed |
| Workstation sign-in completed | Passed |
| Employee confirmed restored access | Passed |
| Technician update recorded | Passed |
| Official solution recorded | Passed |
| Ticket closed | Passed |

---

## Ticket Timeline

| Step | Action | Result |
|---:|---|---|
| 1 | Employee entered an incorrect password several times | Lockout threshold reached |
| 2 | Active Directory locked the account | Sign-in blocked |
| 3 | Employee submitted Ticket #1 | Incident recorded |
| 4 | Technician accepted and acknowledged the ticket | Ticket moved to Processing |
| 5 | Locked accounts were reviewed on SRV01 | Employee account identified |
| 6 | Account was unlocked | Authentication access restored |
| 7 | Employee tested the existing password | Sign-in succeeded |
| 8 | Technician recorded the solution | Ticket moved to Solved |
| 9 | Employee confirmed restored access | Ticket closed |

---

## Closure

| Closure check | Result |
|---|---|
| Incident investigated | Yes |
| Root cause identified | Yes |
| Account unlocked | Yes |
| Password reset required | No |
| Workstation access restored | Yes |
| User validation completed | Yes |
| Technician update recorded | Yes |
| Official solution recorded | Yes |
| Ticket closed | Yes |

---

## How the Issue Was Resolved

The employee reported that Windows showed an account lockout message and prevented access to the workstation.

I checked Active Directory on `SRV01` and confirmed that the account `test.username` was locked. The lockout occurred after the configured threshold of unsuccessful password attempts was reached.

I unlocked the account using `Unlock-ADAccount` and checked the account state again. The employee then signed in to `CLIENT01` using the existing password.

The sign-in completed successfully, and the employee confirmed that workstation access was restored.

---

## Technician Insight

I first verified the account state in Active Directory instead of immediately resetting the password. The account was locked, but there was no indication that the password itself needed to be changed.

Unlocking the account allowed the employee to continue using the existing password and avoided an unnecessary password reset.

After unlocking the account, I asked the employee to test the sign-in before closing the ticket. This confirmed that the original issue had been fully resolved.

---

## Evidence Files

```text
images/INC-001-01-ticket-submitted.png
images/INC-001-02-ticket-assigned.png
images/INC-001-03-account-lockout-confirmed.png
images/INC-001-04-account-unlocked.png
images/INC-001-05-user-sign-in-validated.png
images/INC-001-06-technician-follow-up.png
images/INC-001-07-ticket-solved.png
images/INC-001-08-ticket-closed.png
```

---

## Final Status

```text
Portfolio ID       : INC-001
GLPI Ticket ID     : #1
Ticket type        : Incident
Issue              : Active Directory Account Lockout
Affected account   : test.username
Root cause         : Lockout threshold reached
Password reset     : Not required
Account unlocked   : Yes
Access restored    : Yes
User confirmed     : Yes
Final status       : CLOSED
```
