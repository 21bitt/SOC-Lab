# SOC-Lab

# Active Directory Home Lab Project (VirtualBox)

A hands-on Active Directory lab environment built using Oracle VM VirtualBox. This project demonstrates core enterprise networking and identity management concepts, including Active Directory Domain Services (AD DS), DNS, DHCP configuration, user provisioning, and virtual network segmentation.

---

## 🖥️ Lab Environment & Topology

### Domain Controller (DC) Specifications
- **Virtualization Software:** Oracle VM VirtualBox
- **ISO / OS Version:** Windows Server 2025 ISO (Operating System: Windows Server 2022)
- **Hostname:** `CORP`
- **Domain Name:** `lab-x-dc`
- **CPU:** 2 Cores
- **RAM:** 4 GB
- **Storage:** 50 GB
- **IP Address:** `10.0.0.5`
- **Default Gateway:** `10.0.0.1`
- **Subnet Mask:** `255.255.255.0` (`/24`)
- **Active Server Roles:** Active Directory Domain Services (AD DS), DNS, DHCP

---

## 🌐 Network Configuration

### VirtualBox Custom NAT Network
- **Network Name:** `lab-x-network`
- **IPv4 Prefix:** `10.0.0.0/24` (usable range: `10.0.0.1` – `10.0.0.254`, broadcast: `10.0.0.255`)
- **Gateway IP:** `10.0.0.1`

### DHCP Scope Settings
- **DHCP Pool Range:** `10.0.0.100` – `10.0.0.200`
- **Subnet:** `10.0.0.0/24`
- **DNS Server:** `10.0.0.5` (DC: `CORP`)
- **Router / Gateway:** `10.0.0.1`

---

## 👥 Accounts & Provisioning

| Account Name | Username | Role / Description |
| :--- | :--- | :--- |
| **Administrator** | `Administrator` | Built-in Domain Administrator for AD / DC management |
| **John Doe** | `johnd` | Standard domain user client account |
| **Jane Doe** | `janed` | Standard domain user client account |

---

## 🔌 VirtualBox Network Adapter Modes Overview

VirtualBox provides several networking modes depending on how you want virtual machines to communicate with each other, the host machine, and the outside internet:

- **NAT (Network Address Translation):**
  The default mode. The VM gets outbound internet access through the host's IP address, but external devices and other VMs cannot directly connect to it.

- **NAT Network:**
  Similar to NAT, but creates an internal virtual router allowing multiple VMs assigned to the same NAT Network to communicate with one another while still having outbound internet access.

- **Bridged Adapter:**
  Connects the VM directly to the physical network of the host. The VM receives its own dedicated IP address from the physical network's router (e.g., home Wi-Fi router) and appears as a separate physical device on the local network.

- **Internal Network:**
  Creates an isolated private network strictly between VMs on the same host. VMs can only talk to each other; they cannot access the host machine or the internet.

- **Host-Only Adapter:**
  Creates a private network between the host machine and the VMs. VMs can talk to each other and the host, but have no access to the outside internet unless routed specifically.

---

## 🛡️ Active Directory Core Concepts

### The 3 Main Components of Active Directory

1. **Authentication:**
   Verifies identity ("Who are you?"). Checks user credentials via protocols like Kerberos and NTLM when logging into a domain machine or service.

2. **Authorization:**
   Determines permissions ("What are you allowed to do?"). Checks whether an authenticated user has rights to access specific folders, files, or administrative tools based on group memberships and access control lists (ACLs).

3. **Management:**
   Centralizes control of network objects (users, computers, printers, and organizational units) across the entire enterprise from a single administrative interface.

---

### Key Strengths of Active Directory

- **Centralized Management:** Administrators manage users, passwords, devices, and permissions across thousands of machines from one place rather than configuring each machine individually.
- **Scalability:** Easily grows from a small lab environment to large enterprise networks containing millions of objects across multiple sites and domains.
- **Group Policy (GPO):** Enforces security baselines, software installations, password policies, and desktop settings automatically across all domain-joined computers.
- **Seamless Service Integration:** Integrates natively with critical infrastructure services like DNS, DHCP, file shares, Certificate Services, and cloud platforms (Microsoft Entra ID / Office 365).

---

### Why Active Directory is a Prime Target for Attackers

- **"Keys to the Kingdom":** Gaining Domain Admin privileges gives attackers complete control over every domain-joined computer, server, and user identity across the entire organization.
- **High Privilege Consolidation:** AD stores sensitive credential hashes, Kerberos tickets, and privilege maps, making it a primary target for credential harvesting, lateral movement, and privilege escalation (e.g., Pass-the-Hash, Kerberoasting, Golden Ticket attacks).
- **Misconfiguration Exposure:** Because large Active Directory environments are complex and often accumulate legacy settings, weak permissions, and outdated protocols, attackers frequently find paths to escalate privileges.
