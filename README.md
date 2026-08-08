<div align="center">

# IT Service Management and Asset Administration with GLPI

**A hands-on IT service desk homelab covering ticket management, Windows troubleshooting, identity support, infrastructure administration, and technical documentation.**

<br>

<img src="https://img.shields.io/badge/GLPI-11-005C9C?style=for-the-badge&logo=glpi&logoColor=white" alt="GLPI 11">
<img src="https://img.shields.io/badge/Ubuntu%20Server-24.04%20LTS-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="Ubuntu Server 24.04 LTS">
<img src="https://img.shields.io/badge/Windows%20Server-Active%20Directory-0078D4?style=for-the-badge&logo=windows&logoColor=white" alt="Windows Server">
<img src="https://img.shields.io/badge/Tickets%20Completed-10%20of%2012-2EA44F?style=for-the-badge&logo=checkmarx&logoColor=white" alt="10 of 12 tickets completed">

</div>

---

## Project Overview

This project demonstrates the deployment and administration of an internal IT service desk using **GLPI 11**.

The environment was built in VMware Workstation and integrates GLPI with a Windows enterprise homelab containing Active Directory, Group Policy, DNS, file services, printer services, Windows Server Update Services, and a domain-joined workstation.

The project focuses on realistic entry-level responsibilities associated with:

- IT Support
- Service Desk
- Help Desk
- Desktop Support
- Junior Systems Administration
- Identity and Access Management
- IT Service Management

Each support ticket follows a complete service desk lifecycle:

```text
Employee reports an issue
        ↓
Ticket is created in GLPI
        ↓
Technician acknowledges and investigates
        ↓
Root cause is identified
        ↓
Remediation is performed
        ↓
Employee validates the result
        ↓
Solution is documented
        ↓
Ticket is closed
```

---

## Project Objectives

The main objectives of this project are to demonstrate the ability to:

- Deploy and administer a GLPI service desk
- Configure technician and employee access
- Manage incidents and service requests
- Categorize, prioritize, assign, and resolve tickets
- Troubleshoot Windows workstation issues
- Support Active Directory user accounts
- Investigate Group Policy application
- Resolve DNS and network-connectivity problems
- Support shared folders and mapped drives
- Troubleshoot network printers
- Investigate Windows Update and WSUS failures
- Communicate professionally with end users
- Document root causes, remediation steps, and validation results
- Maintain clear technical evidence for every support case

---

## Lab Architecture

```text
                         VMware Workstation
                                │
        ┌───────────────────────┼────────────────────────┐
        │                       │                        │
        ▼                       ▼                        ▼
     GLPI01                   SRV01                   CLIENT01
 Ubuntu Server 24.04     Windows Server          Windows Workstation
        │                       │                        │
        │                       ├── Active Directory     ├── Domain Joined
        │                       ├── DNS                  ├── Group Policy
        │                       ├── Group Policy         ├── HR Drive Mapping
        │                       ├── File Services        ├── Network Printer
        │                       ├── Printer Services     └── Windows Update
        │                       └── WSUS
        │
        ├── Apache
        ├── PHP
        ├── MariaDB
        └── GLPI 11
                │
                ▼
       Internal Service Desk Portal
                │
        ┌───────┴────────┐
        │                │
        ▼                ▼
 Employee User      Service Desk Technician
 Submit Tickets     Investigate and Resolve
```

---

## Lab Environment

| Component | Purpose |
|---|---|
| VMware Workstation | Hosts the virtualized lab environment |
| GLPI01 | Ubuntu server hosting the GLPI platform |
| Ubuntu Server 24.04 LTS | Operating system used by the GLPI server |
| Apache | Web server used to publish the GLPI portal |
| PHP | Application runtime required by GLPI |
| MariaDB | Database platform storing GLPI information |
| GLPI 11 | ITSM, ticketing, and asset-administration platform |
| SRV01 | Windows Server providing enterprise infrastructure services |
| Active Directory Domain Services | Manages domain users, groups, computers, and authentication |
| DNS | Provides internal hostname and domain-name resolution |
| Group Policy | Centrally manages workstation and user settings |
| File Services | Hosts shared departmental folders |
| Printer Services | Publishes shared network printers |
| WSUS | Provides centralized Windows Update management |
| CLIENT01 | Domain-joined employee workstation used for incident simulations |
| Windows host computer | Administrative workstation and browser client |
| PowerShell | Windows administration and troubleshooting |
| SSH | Remote administration of the Ubuntu server |

---

## GLPI Configuration

The GLPI service desk includes separate accounts and access levels for:

| Role | Purpose |
|---|---|
| GLPI Administrator | Manages the GLPI platform and configuration |
| Service Desk Technician | Receives, investigates, updates, and resolves tickets |
| Employee User | Submits incidents and service requests |
| Requester | Reports the affected service or workstation issue |
| Assigned Technician | Owns the investigation and resolution process |

Default GLPI accounts were disabled after the required administrative accounts were created.

---

## Ticket Workflow

Each documented ticket includes:

1. Initial working state
2. Employee-reported issue
3. GLPI ticket submission
4. Technician acknowledgement
5. Technical investigation
6. Investigation results
7. Root-cause analysis
8. Remediation
9. Employee testing
10. User confirmation
11. Official solution
12. Ticket closure
13. Validation checklist
14. Evidence screenshots
15. Technician insight

---

## Incident and Service Request Portfolio

| ID | Ticket | Type | Category | Status |
|---|---|---|---|---|
| INC-001 | Active Directory Account Lockout | Incident | Accounts and Access | Completed |
| INC-002 | Password Reset Request | Service Request | Accounts and Access | Completed |
| INC-003 | DNS Resolution Issue | Incident | Network and Connectivity | Completed |
| INC-004 | Shared Folder Access Denied | Incident | Files and Permissions | Completed |
| INC-005 | Mapped HR Drive Missing | Incident | Files and Permissions | Completed |
| INC-006 | Network Printer Missing | Incident | Hardware and Peripherals | Completed |
| INC-007 | Slow Workstation Performance | Incident | Hardware and Peripherals | Completed |
| INC-008 | Business Application Will Not Open | Incident | Software and Applications | Completed |
| INC-009 | Workstation Has No Network Access | Incident | Network and Connectivity | Completed |
| INC-010 | Windows Update Connection Failure | Incident | Software and Applications | Completed |
| SR-011 | New Employee Onboarding Request | Service Request | Employee Lifecycle | Planned |
| SR-012 | Employee Offboarding Request | Service Request | Employee Lifecycle | Planned |

Current progress:

```text
Completed tickets : 10
Planned tickets   : 2
Total tickets     : 12
Completion        : 83%
```

---

## Completed Technical Scenarios

### Accounts and Access

The account-support scenarios demonstrate:

- Identifying locked Active Directory accounts
- Reviewing account status
- Unlocking employee accounts
- Resetting passwords securely
- Requiring password changes at the next sign-in
- Validating successful employee authentication

### Network and Connectivity

The network scenarios demonstrate:

- Reviewing workstation IP configuration
- Testing internal and external connectivity
- Diagnosing DNS resolution failures
- Identifying incorrect DNS settings
- Identifying incorrect IP addresses and default gateways
- Restoring DHCP configuration
- Validating network access after remediation

### Files and Permissions

The file-access scenarios demonstrate:

- Reviewing Active Directory group membership
- Investigating shared-folder permissions
- Restoring access through security-group membership
- Troubleshooting missing mapped drives
- Reviewing Group Policy links and targeting
- Refreshing Group Policy
- Validating restored departmental drive mappings

### Hardware and Peripherals

The workstation-support scenarios demonstrate:

- Troubleshooting missing network printers
- Reviewing printer deployment through Group Policy
- Applying group-based printer targeting
- Investigating slow workstation performance
- Reviewing processor, memory, disk, and process utilization
- Terminating an unresponsive or resource-intensive process
- Confirming restored workstation performance

### Software and Applications

The software-support scenarios demonstrate:

- Troubleshooting invalid application shortcuts
- Reviewing application paths
- Restoring valid shortcut targets
- Investigating Windows Update failures
- Reviewing WSUS registry policy
- Testing update-server connectivity
- Refreshing Group Policy
- Restarting Windows Update services
- Validating communication with the internal WSUS server

---

## Repository Structure

```text
IT-Service-Management-and-Asset-Administration-GLPI/
│
├── README.md
│
├── 01-Accounts-and-Access/
│   ├── INC-001-Active-Directory-Account-Lockout/
│   └── INC-002-Password-Reset-Request/
│
├── 04-Network-and-Connectivity/
│   ├── INC-003-DNS-Resolution-Issue/
│   └── INC-009-Workstation-No-Network-Access/
│
├── 08-Files-and-Permissions/
│   ├── INC-004-Shared-Folder-Access-Denied/
│   └── INC-005-Mapped-HR-Drive-Missing/
│
├── 09-Hardware-and-Peripherals/
│   ├── INC-006-Network-Printer-Missing/
│   └── INC-007-Slow-Workstation-Performance/
│
└── 10-Software-and-Applications/
    ├── INC-008-Business-Application-Will-Not-Open/
    └── INC-010-Windows-Update-Connection-Failure/
```

Each ticket directory contains:

```text
Ticket documentation
        │
        ├── Ticket details
        ├── User communication
        ├── Investigation commands
        ├── Investigation results
        ├── Root cause
        ├── Remediation
        ├── Validation
        ├── Technician insight
        └── Evidence screenshots
```

---

## Example Support Technologies

### Windows Administration

```text
Active Directory Users and Computers
Group Policy Management
Windows Services
Windows Event Viewer
Windows Settings
File Explorer
Printer Management
Windows Server Update Services
```

### PowerShell and Command-Line Tools

```powershell
Get-ADUser
Search-ADAccount
Unlock-ADAccount
Set-ADAccountPassword
Get-ADGroupMember
Add-ADGroupMember
Get-GPInheritance
gpupdate /force
ipconfig /all
Resolve-DnsName
Test-NetConnection
Get-Service
Get-Process
Get-Counter
Get-WinEvent
```

### Linux Administration

```text
Ubuntu Server administration
Apache configuration
PHP configuration
MariaDB administration
SSH remote access
Linux file permissions
Service management
Web-application deployment
```

---

## Documentation Standard

Every completed ticket is written as a structured technical case study.

The documentation separates:

- The employee’s original report
- Technician communication
- Technical investigation
- Root-cause analysis
- Corrective action
- User validation
- Final ticket closure
- Personal technician reflection

The documentation focuses on what a technician would reasonably observe during a real support case. Internal lab-preparation activities are not presented as part of the employee-facing incident record.

---

## Evidence and Validation

Screenshots are captured throughout each support workflow to demonstrate:

- Initial issue state
- Submitted GLPI ticket
- Technician assignment
- Investigation results
- Configuration changes
- Successful remediation
- Employee confirmation
- Solved ticket state
- Closed ticket state

Commands and screenshots are reviewed before publication to avoid exposing:

- Passwords
- Authentication tokens
- Personal information
- Private tenant identifiers
- Sensitive infrastructure details
- Unnecessary internal configuration data

---

## Skills Demonstrated

### IT Service Management

- Incident management
- Service-request management
- Ticket categorization
- Ticket prioritization
- Technician assignment
- Status management
- User communication
- Root-cause documentation
- Solution recording
- Ticket closure

### Technical Support

- Account lockout resolution
- Password reset support
- DNS troubleshooting
- TCP port testing
- DHCP restoration
- Shared-folder access troubleshooting
- NTFS and group-based access review
- Group Policy troubleshooting
- Printer deployment troubleshooting
- Workstation performance analysis
- Application-path troubleshooting
- Windows Update and WSUS troubleshooting

### Systems Administration

- Active Directory administration
- Security-group management
- Group Policy administration
- DNS administration
- File and printer services
- Windows service management
- Windows event-log analysis
- Linux server administration
- Web-server administration
- Database-backed application deployment

### Professional Skills

- Clear employee communication
- Technical documentation
- Evidence collection
- Incident ownership
- User validation
- Structured troubleshooting
- Escalation awareness
- Privacy-conscious documentation

---

## Project Status

```text
GLPI deployment                    : Completed
Administrative accounts           : Completed
Technician account                 : Completed
Employee self-service account      : Completed
Ticket categories                  : Implemented as required
Incident documentation             : 10 completed
Service-request documentation      : 1 completed
Employee onboarding workflow       : Planned
Employee offboarding workflow      : Planned
Repository-wide validation         : Pending
Root README refinement             : In progress
Final portfolio review             : Pending
```

---

## Remaining Roadmap

The remaining work for the first phase includes:

1. Review all completed ticket folders
2. Validate screenshot filenames and Markdown links
3. Confirm consistent ticket formatting
4. Complete the new-employee onboarding request
5. Complete the employee offboarding request
6. Create a final ticket index
7. Review the repository for sensitive information
8. Complete the final repository-wide validation
9. Publish the completed portfolio project

---

## Lab Limitations

This project was created in a controlled virtual homelab environment.

The environment does not represent a production organization and does not contain real employee, customer, or business information.

Some enterprise capabilities are simulated on a smaller scale because of:

- Limited virtual-machine resources
- A single Active Directory domain
- A limited number of user accounts and workstations
- No production network infrastructure
- No production email integration
- No externally hosted customer portal
- No commercial support or licensing dependencies

These limitations do not affect the project’s primary objective: demonstrating a structured and realistic IT support workflow.

---

## Key Takeaway

This project demonstrates more than the installation of a ticketing application.

It shows the complete process of receiving a user issue, communicating professionally, investigating the affected system, identifying the technical root cause, applying an appropriate solution, validating the result with the employee, and documenting the entire incident for future reference.

The repository is designed to provide practical evidence of entry-level IT support, service desk, desktop support, and junior systems-administration skills.

---

<div align="center">

**Built as a practical IT Support and Service Desk portfolio project**

</div>
