# IT Service Management and Asset Administration with GLPI

<p align="center">
  <strong>Hands-on IT Service Management, Help Desk Ticketing, Asset Management, Linux Administration, and Support Operations Homelab</strong>
</p>

---

## Project Overview

This project demonstrates the deployment and administration of a complete internal IT service desk using GLPI.

The environment was built on an Ubuntu Server virtual machine running in VMware Workstation.

The project focuses on realistic entry-level IT support and service desk responsibilities, including:

- Installing and securing an Ubuntu Server
- Configuring Apache, PHP, and MariaDB
- Installing and configuring GLPI
- Creating administrator, technician, and employee accounts
- Managing incidents and service requests
- Assigning and resolving support tickets
- Creating IT asset records
- Building knowledge-base articles
- Designing SLA and escalation procedures
- Documenting troubleshooting and support workflows

---

## Lab Environment

| Component | Purpose |
|---|---|
| VMware Workstation | Virtualization platform |
| Ubuntu Server 24.04 LTS | GLPI application server |
| Apache | Web server |
| PHP | GLPI application runtime |
| MariaDB | GLPI database |
| GLPI 11 | ITSM, ticketing, and asset-management platform |
| Windows host computer | Administrative workstation and browser client |
| SSH | Remote Linux administration |

---

## Current Architecture

```text
Windows Host Computer
        │
        ├── Web Browser
        │       │
        │       ▼
        │   GLPI Web Portal
        │
        └── SSH Terminal
                │
                ▼
        VMware Workstation
                │
                ▼
             GLPI01
      Ubuntu Server 24.04 LTS
                │
        ┌───────┼────────┐
        │       │        │
      Apache   PHP     MariaDB
        │       │        │
        └───────┴────────┘
                │
                ▼
              GLPI 11
