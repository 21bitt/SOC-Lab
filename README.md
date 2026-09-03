
[5 lines collapsed]

## 🏛️ Lab Architecture & Network Topology
```mermaid
flowchart TD
    subgraph NAT["VirtualBox Custom NAT Network: lab-x-network (10.0.0.0/24)"]
        GW["Default Gateway / Virtual Router<br/><b>10.0.0.1</b>"]
        
        GW --- DC["<b>Domain Controller: CORP</b><br/>IP: 10.0.0.5 (Static)<br/>OS: Windows Server 2022<br/>Roles: AD DS, DNS, DHCP Scope (10.0.0.100-200)<br/>Specs: 2 CPU / 4GB RAM / 50GB Disk"]
        
        GW --- WIN["<b>Windows Client: John Doe</b><br/>IP: 10.0.0.100 (DHCP)<br/>Account: johnd@corp.lab-x-dc.com<br/>Role: Domain Member Workstation<br/>Specs: 3 CPU / 4GB RAM / 80GB Disk"]
        
        GW --- LINUX["<b>Ubuntu Client: Jane Doe</b><br/>IP: 10.0.0.101 (DHCP)<br/>Account: janed@linux-client<br/>Role: Samba Winbind AD Member + Mail Watcher<br/>Specs: 1 CPU / 2GB RAM / 50GB Disk"]
        
        GW --- CORP["<b>Ubuntu Corp Server: lab-x-admin</b><br/>IP: 10.0.0.8 (Static)<br/>Account: lab-x-admin@corp-svr<br/>Role: App Server (Docker + MailHog SMTP/Web)<br/>Specs: 1 CPU / 2GB RAM / 25GB Disk"]
    end
    classDef dcStyle fill:#1e3a8a,stroke:#60a5fa,stroke-width:2px,color:#ffffff;
    classDef winStyle fill:#1e293b,stroke:#38bdf8,stroke-width:1.5px,color:#ffffff;
    classDef linuxStyle fill:#312e81,stroke:#a78bfa,stroke-width:1.5px,color:#ffffff;
    classDef serverStyle fill:#064e3b,stroke:#34d399,stroke-width:1.5px,color:#ffffff;
    classDef gwStyle fill:#334155,stroke:#94a3b8,stroke-width:1.5px,color:#ffffff;
    class DC dcStyle;
    class WIN winStyle;
    class LINUX linuxStyle;
    class CORP serverStyle;
    class GW gwStyle;
```
                                  [ lab-x-network ]
                             NAT Network: 10.0.0.0/24
                                Gateway: 10.0.0.1
                                        │
      ┌──────────────────┬──────────────┴───────────────┬──────────────────┐
      │                  │                              │                  │
┌─────▼──────────┐ ┌─────▼───────────────┐ ┌────────────▼──────┐ ┌─────────▼──────────────┐
│ Domain Cont.   │ │ Windows Client      │ │ Ubuntu Client     │ │ Ubuntu Corp Server     │
│ CORP           │ │ John Doe (johnd)    │ │ Jane Doe (janed)  │ │ lab-x-admin            │
│ 10.0.0.5       │ │ 10.0.0.100          │ │ 10.0.0.101        │ │ 10.0.0.8               │
│ AD DS, DNS,    │ │ Windows OS          │ │ Ubuntu Linux      │ │ Docker + MailHog       │
│ DHCP Server    │ │ Domain Member       │ │ Samba Winbind AD  │ │ (SMTP: 1025 / UI: 8025)│
└────────────────┘ └─────────────────────┘ └───────────────────┘ └────────────────────────┘
```
---
### 1. Domain Controller (DC)
- **Hostname:** `CORP`
- **Domain Name:** `lab-x-dc` (FQDN: `corp.lab-x-dc.com`)

[237 lines collapsed]
