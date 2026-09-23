
# Windows Server Directory Services
Implementation and administration of Active Directory, DNS, DHCP and Group Policy (GPO) using Windows Server 2025.

## Lab Environment

| Machine | OS | IP | Role |
|---|---|---|---|
| DC01-LAB-WS | Windows Server 2025 | 192.168.78.10 | AD DS / DNS / DHCP / GPO |
| WIN10-01 | Windows 10 | 192.168.78.20 | Domain Client |

**Network:** 192.168.78.0/24  
**Virtualization:** VMware  
**Virtual Network:** VMnet1 Host-only

## Objectives

- Deploy Active Directory Domain Services
- Configure DNS
- Configure DHCP
- Create users and organizational units
- Join Windows 10 to the domain
- Implement Group Policy
- Validate the environment

## Implementation

### 1. Network Configuration

Configured static IP addresses for the Windows Server and Windows 10 client.

### 2. Active Directory

AD DS will be installed and the server promoted to Domain Controller.

### 3. DNS

DNS will be configured for the Active Directory domain.

### 4. DHCP

A DHCP scope will be configured for the lab network.

### 5. Group Policy

GPOs will be created and applied to domain users and computers.

## Validation

Connectivity, DNS, DHCP, domain membership and GPO application will be tested.

## Evidence

Screenshots and configuration evidence are available in the `screenshots/` directory.

