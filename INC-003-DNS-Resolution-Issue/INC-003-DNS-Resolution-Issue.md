# INC-003 — DNS Resolution Issue

## Ticket Details

| Field | Value |
|---|---|
| Portfolio ID | INC-003 |
| GLPI Ticket ID | #3 |
| Ticket type | Incident |
| Category | Network and Connectivity > DNS Resolution Issue |
| Status | Closed |
| Urgency | Medium |
| Impact | Medium |
| Priority | Medium |
| Support level | Level 1 |
| Requester | Employee User |
| Assigned technician | Service Desk Technician |
| Affected workstation | CLIENT01 |
| DNS server | SRV01 |
| Ticketing server | GLPI01 |

---

## Issue Type

This was a workstation DNS configuration issue.

`CLIENT01` could still access the internet, but it could not resolve the internal server name `srv01.homelab.local`.

The issue affected one workstation and prevented the employee from accessing an internal resource using its hostname.

---

## User Report

> Hi, I’m unable to access the internal server using its usual name. I tried again a few times, but Windows says it cannot find the server. My internet connection still appears to be working. Can someone please check this? Thanks.

![Ticket submitted](images/INC-003-03-ticket-submitted.png)

---

## Systems Involved

| System | Purpose |
|---|---|
| `CLIENT01` | Employee workstation experiencing the issue |
| `SRV01` | Active Directory and internal DNS server |
| `GLPI01` | GLPI ticketing server |

---

## Initial Working State

Before reproducing the incident, `CLIENT01` was configured to use the internal DNS server:

```text
192.168.241.10
```

The following commands confirmed that the internal hostname was resolving correctly:

```powershell
Resolve-DnsName srv01.homelab.local
```

```powershell
ping srv01.homelab.local
```

The hostname resolved to the correct internal IP address, and connectivity was successful.

![Working DNS baseline](images/INC-003-01-dns-baseline-working.png)

---

## Incident Reproduction

The DNS server configured on `CLIENT01` was temporarily changed from the internal DNS server to Google Public DNS:

```powershell
Set-DnsClientServerAddress `
  -InterfaceAlias "Ethernet0" `
  -ServerAddresses "8.8.8.8"
```

The local DNS cache was then cleared:

```powershell
Clear-DnsClientCache
```

The workstation was now configured to use:

```text
8.8.8.8
```

Because this public DNS server did not contain the private `homelab.local` DNS zone, the workstation could no longer resolve the internal server name.

The issue was confirmed using:

```powershell
Resolve-DnsName srv01.homelab.local
```

```powershell
ping srv01.homelab.local
```

Both tests failed to locate the internal server by hostname, while general network connectivity remained available.

![DNS resolution failed](images/INC-003-02-dns-resolution-failed.png)

---

## Technician Acknowledgement

The ticket was assigned to the Service Desk Technician and moved to `Processing`.

> Hi, I’ve received your ticket. Since your internet connection is still working but the internal server name cannot be found, I’ll check the workstation’s DNS configuration and test the connection to the internal DNS server. I’ll update you once I identify the cause.

![Ticket assigned and acknowledged](images/INC-003-04-ticket-assigned-and-acknowledged.png)

---

## Investigation

The DNS configuration on `CLIENT01` was reviewed using:

```powershell
Get-DnsClientServerAddress `
  -InterfaceAlias "Ethernet0" `
  -AddressFamily IPv4 |
Select-Object InterfaceAlias, ServerAddresses
```

The result showed that the workstation was using:

```text
8.8.8.8
```

The expected internal DNS server was:

```text
192.168.241.10
```

The internal DNS service was tested using:

```powershell
Test-NetConnection 192.168.241.10 -Port 53
```

The test returned:

```text
TcpTestSucceeded : True
```

This confirmed that the internal DNS server was reachable on DNS port `53`.

The internal DNS record was then tested directly:

```powershell
Resolve-DnsName `
  srv01.homelab.local `
  -Server 192.168.241.10
```

The internal DNS server successfully resolved the hostname.

![DNS configuration investigated](images/INC-003-05-dns-configuration-investigated.png)

---

## Root Cause

`CLIENT01` was configured to use a public DNS server instead of the internal Active Directory DNS server.

The public DNS server could resolve internet addresses but could not resolve the private `homelab.local` domain.

```text
Incorrect DNS server configured
          ↓
Internet connectivity remains available
          ↓
Private domain cannot be resolved
          ↓
Internal server unavailable by hostname
```

---

## Remediation

The correct internal DNS server was restored on `CLIENT01`:

```powershell
Set-DnsClientServerAddress `
  -InterfaceAlias "Ethernet0" `
  -ServerAddresses "192.168.241.10"
```

The local DNS cache was cleared:

```powershell
Clear-DnsClientCache
```

The corrected DNS configuration was confirmed using:

```powershell
Get-DnsClientServerAddress `
  -InterfaceAlias "Ethernet0" `
  -AddressFamily IPv4 |
Select-Object InterfaceAlias, ServerAddresses
```

The workstation was again using:

```text
192.168.241.10
```

Internal name resolution was tested using:

```powershell
Resolve-DnsName srv01.homelab.local
```

Connectivity by hostname was tested using:

```powershell
ping srv01.homelab.local
```

Both tests completed successfully.

![DNS configuration restored](images/INC-003-06-dns-configuration-restored.png)

---

## Technician Update

> Hi, I checked the workstation and found that it was using a public DNS server instead of the internal company DNS server. I restored the correct DNS address, cleared the local DNS cache, and confirmed that the internal server name is resolving again.

---

## Official Solution

> The DNS settings on CLIENT01 were corrected from the public DNS server to the internal DNS server. The DNS cache was cleared, and successful name resolution and connectivity to the internal server were verified. The user can now access the server by name.

The ticket status was changed to `Solved`.

![Ticket solved](images/INC-003-07-ticket-solved.png)

---

## User Confirmation

The employee confirmed that the internal server was accessible again.

> I can access the internal server again using its normal name. Everything is working now. Thank you.

The solution was accepted, and the ticket status changed to `Closed`.

![User confirmation](images/INC-003-08-user-confirmation.png)

---

## Validation

| Validation check | Result |
|---|---|
| Internet connectivity remained available | Passed |
| Incorrect DNS server identified | Passed |
| Internal DNS server reachable on port 53 | Passed |
| Internal DNS record resolved directly | Passed |
| Correct DNS server restored | Passed |
| DNS cache cleared | Passed |
| Internal hostname resolved successfully | Passed |
| Connectivity by hostname succeeded | Passed |
| Employee confirmed restored access | Passed |
| Ticket closed | Passed |

---

## Ticket Timeline

| Step | Action | Result |
|---:|---|---|
| 1 | Confirmed the original DNS configuration | Working baseline recorded |
| 2 | Changed the workstation DNS server to `8.8.8.8` | Issue reproduced |
| 3 | Tested the internal hostname | Name resolution failed |
| 4 | Employee submitted Ticket #3 | Incident recorded |
| 5 | Technician accepted and acknowledged the ticket | Ticket moved to Processing |
| 6 | Reviewed the workstation DNS configuration | Incorrect DNS server identified |
| 7 | Tested the internal DNS server | Port 53 reachable |
| 8 | Queried the internal DNS record directly | DNS record resolved successfully |
| 9 | Restored the internal DNS server address | DNS configuration corrected |
| 10 | Cleared the local DNS cache | Old cached results removed |
| 11 | Retested hostname resolution and connectivity | Access restored |
| 12 | Recorded the solution | Ticket moved to Solved |
| 13 | Employee confirmed the fix | Ticket closed |

---

## Closure

| Closure check | Result |
|---|---|
| Incident investigated | Yes |
| Root cause identified | Yes |
| Corrective action completed | Yes |
| Internal name resolution restored | Yes |
| User validation completed | Yes |
| Technician update recorded | Yes |
| Official solution recorded | Yes |
| Ticket closed | Yes |

---

## How the Issue Was Resolved

The workstation could still access the internet, so I first checked whether the problem was related to internal name resolution rather than a complete network outage.

I reviewed the DNS configuration on `CLIENT01` and found that it was using the public DNS server `8.8.8.8` instead of the internal DNS server `192.168.241.10`.

I tested the internal DNS server and confirmed that it was reachable on port `53`. I also queried the server directly and confirmed that the DNS record for `srv01.homelab.local` was working.

I restored the correct DNS server address on the workstation, cleared the local DNS cache, and tested the internal hostname again. The hostname resolved successfully, connectivity was restored, and the employee confirmed that the server was accessible.

---

## Technician Insight

I did not immediately treat the issue as a complete network failure because the employee could still access the internet. That helped me narrow the investigation to DNS and internal name resolution.

Before changing the configuration, I confirmed that the internal DNS server was reachable and that the required DNS record existed. This showed that the server itself was working and that the problem was limited to the workstation configuration.

After correcting the DNS address, I cleared the cache and performed the same tests again to make sure the issue was fully resolved before updating and closing the ticket.

---

## Evidence Files

```text
images/INC-003-01-dns-baseline-working.png
images/INC-003-02-dns-resolution-failed.png
images/INC-003-03-ticket-submitted.png
images/INC-003-04-ticket-assigned-and-acknowledged.png
images/INC-003-05-dns-configuration-investigated.png
images/INC-003-06-dns-configuration-restored.png
images/INC-003-07-ticket-solved.png
images/INC-003-08-user-confirmation.png
```

---

## Final Status

```text
Portfolio ID        : INC-003
GLPI Ticket ID      : #3
Ticket type         : Incident
Issue               : DNS Resolution Issue
Affected system     : CLIENT01
Root cause          : Incorrect DNS server configuration
Correct DNS server  : 192.168.241.10
Configuration fixed : Yes
Access restored     : Yes
User confirmed      : Yes
Final status        : CLOSED
```
