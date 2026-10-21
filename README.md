Active Directory, Linux Integration, and Wazuh Security Lab (VirtualBox)

A hands-on enterprise lab built in Oracle VM VirtualBox. The environment covers Active Directory Domain Services, DNS, DHCP, Linux domain join with Kerberos and Samba Winbind, internal mail services, centralized security monitoring with Wazuh, and a Kali Linux host used for authorized attack-and-defend exercises on this same NAT network.



Lab Architecture and Network Topology

The diagram follows the same hub-and-spoke layout as the lab map: a central hypervisor, directory services at the top, security and email services on the left, Wazuh security functions on the right, and workstations along the bottom.

flowchart LR
    subgraph LEFT
        ES["Email Server<br/>email-svr<br/>Ubuntu Server 22.04<br/>10.0.0.8"] --- SS1["Security Server<br/>corp-svr<br/>Ubuntu Desktop 22.04<br/>10.0.0.8<br/>MailHog / Docker"]
    end

    subgraph CENTER
        DS["Directory Services Server<br/>lab-x-dc / CORP<br/>Windows Server 2022<br/>10.0.0.5<br/>AD DS, DNS, DHCP"]
        HV["Hypervisor<br/>Oracle VM VirtualBox<br/>NAT Network: lab-x-network<br/>10.0.0.0/24<br/>Gateway: 10.0.0.1"]
        EW1["Enterprise Workstation<br/>lab-x-win-client<br/>johnd@corp.lab-x-dc.com<br/>10.0.0.100"]
        EW2["Enterprise Workstation<br/>lab-x-ubu-client<br/>janed@linux-client<br/>10.0.0.101"]
        SW["Security Workstation<br/>lab-x-sec-mon console<br/>Ubuntu Desktop 22.04<br/>10.0.0.10<br/>Wazuh Dashboard"]
        KALI["Attack Workstation<br/>lab-x-kali<br/>Kali Linux<br/>10.0.0.50<br/>2 CPU / 4 GB RAM / 60 GB"]
    end

    subgraph RIGHT
        SS2["Security Server<br/>lab-x-sec-mon<br/>Ubuntu Desktop 22.04<br/>10.0.0.10<br/>Wazuh Manager"] --- SIEM["SIEM<br/>Wazuh log analytics<br/>and correlation"]
        SS2 --- EDR["EDR / XDR<br/>Wazuh agent telemetry<br/>and FIM"]
        SS2 --- VS["VulnScan<br/>Wazuh Vulnerability<br/>Detection"]
    end

    SS1 --- HV
    HV --- DS
    HV --- SS2
    HV --- EW1
    HV --- EW2
    HV --- SW
    HV --- KALI



Topology Node Map







Diagram box



Lab hostname



IP address



Role





Hypervisor



Host machine (VirtualBox)



Gateway 10.0.0.1



Virtualization host and NAT network router





Directory Services Server



lab-x-dc / CORP



10.0.0.5



Active Directory, DNS, DHCP





Security Server (left)



corp-svr



10.0.0.8



Corporate desktop and MailHog Docker host





Email Server



email-svr



10.0.0.8



Dedicated Ubuntu mail backend





Security Server (right)



lab-x-sec-mon



10.0.0.10



Wazuh Manager, Indexer, and Dashboard





SIEM



Wazuh module on lab-x-sec-mon



10.0.0.10



Log collection, correlation, and alerts





EDR



Wazuh module on lab-x-sec-mon



10.0.0.10



Endpoint telemetry, FIM, and response





VulnScan



Wazuh module on lab-x-sec-mon



10.0.0.10



Vulnerability Detection





Enterprise Workstation



lab-x-win-client



10.0.0.100



Domain-joined Windows client (johnd)





Enterprise Workstation



lab-x-ubu-client



10.0.0.101



Domain-joined Ubuntu client (janed)





Security Workstation



lab-x-sec-mon console



10.0.0.10



Analyst console for the Wazuh Dashboard





Attack Workstation



lab-x-kali



10.0.0.50



Kali Linux host on lab-x-network





Network and Subnet Summary







Parameter



Configuration



Details





VirtualBox network mode



Custom NAT Network



Network name: lab-x-network





Network subnet



10.0.0.0/24



Usable range: 10.0.0.1 to 10.0.0.254





Default gateway



10.0.0.1



VirtualBox NAT gateway





Primary DNS server



10.0.0.5



Domain controller (lab-x-dc / CORP)





DHCP pool



10.0.0.100 to 10.0.0.200



Windows Server DHCP role on CORP





Provisioned Systems and Accounts







Hostname



OS



Hardware specs



IP address



Account



Role





lab-x-dc / CORP



Windows Server 2022



2 CPU / 4096 MB RAM / 50 GB



10.0.0.5



Administrator@corp.lab-x-dc.com



Domain controller, DNS, DHCP





lab-x-win-client



Windows Client



3 CPU / 4096 MB RAM / 80 GB



10.0.0.100



johnd@corp.lab-x-dc.com



Domain-joined Windows workstation





lab-x-ubu-client



Ubuntu Desktop 22.04 LTS



1 CPU / 2048 MB RAM / 50 GB



10.0.0.101



janed@linux-client



Domain-joined Linux client and mail watcher





corp-svr



Ubuntu Desktop 22.04 LTS



1 CPU / 2048 MB RAM / 50 GB



10.0.0.8



lab-x-admin@corp-svr; lab root SSH password set to november on purpose



Corporate desktop and MailHog Docker host





email-svr



Ubuntu Server 22.04 LTS



1 CPU / 2048 MB RAM / 25 GB



10.0.0.8



lab-x-admin@email-svr



Dedicated mail server





lab-x-sec-mon



Ubuntu Desktop 22.04 LTS



Wazuh all-in-one host



10.0.0.10



Local admin on the Wazuh host



Wazuh Server, Indexer, and Dashboard





lab-x-kali



Kali Linux



2 CPU / 4096 MB RAM / 60 GB



10.0.0.50



Local Kali user



Attack workstation on lab-x-network



Hardware and Virtual Machine Breakdown





Directory Services Server (lab-x-dc / CORP)





OS: Windows Server 2022 (Windows Server 2025 ISO)



Hardware: 2 CPU cores, 4096 MB RAM, 50 GB storage



IP: 10.0.0.5 (static)



Functions: AD DS, DNS, DHCP scope 10.0.0.100 to 10.0.0.200



Enterprise Workstation (lab-x-win-client / John Doe)





OS: Windows Client



Hardware: 3 CPU cores, 4096 MB RAM, 80 GB storage



IP: 10.0.0.100 (DHCP)



Account: johnd@corp.lab-x-dc.com



Wazuh agent group: windows



Enterprise Workstation (lab-x-ubu-client / Jane Doe)





OS: Ubuntu Desktop 22.04 LTS



Hardware: 1 CPU core, 2048 MB RAM, 50 GB storage



IP: 10.0.0.101 (DHCP)



Account: janed@linux-client



Functions: Samba Winbind domain member and mail watcher



Wazuh agent group: linux



Security Server, left (corp-svr)





OS: Ubuntu Desktop 22.04 LTS



Hardware: 1 CPU core, 2048 MB RAM, 50 GB storage



IP: 10.0.0.8



Account: lab-x-admin@corp-svr



Function: Hosts Docker and MailHog (SMTP 1025, Web UI and API 8025)



Email Server (email-svr)





OS: Ubuntu Server 22.04 LTS



Hardware: 1 CPU core, 2048 MB RAM, 25 GB storage



IP: 10.0.0.8



Account: lab-x-admin@email-svr



Function: Dedicated corporate mail backend



Security Server and Security Workstation (lab-x-sec-mon)





OS: Ubuntu Desktop 22.04 LTS



IP: 10.0.0.10



Function: Wazuh all-in-one install (Manager, Indexer, Dashboard)



Install command: curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh && sudo bash ./wazuh-install.sh -a -i



Attack Workstation (lab-x-kali)





OS: Kali Linux



Hardware: 2 CPU cores, 4096 MB RAM, 60 GB storage



IP: 10.0.0.50 on NAT network lab-x-network



Function: Offensive testing host used against the lab domain and monitored endpoints





Initial Recon and Internal Phishing Exercise

This section records an authorized attack-and-defend exercise on the lab VMs listed above. The work was done from lab-x-kali (10.0.0.50) against hosts on lab-x-network. It is not a guide for use outside this lab.

The exercise had three goals:





Map live hosts and services on the /24.



Show how a weak SSH password on corp-svr leads to local access.



Show how an internal mail service (MailHog) can be used to deliver a phishing lure to janed, and how a fake login page can capture a submitted password.



1. Service discovery

From Kali, a TCP service scan of ports 1 through 1000 was run across the lab subnet (Nmap 7.99, 21 Oct 2026). Eight addresses responded.







IP



Likely host



Notable open ports



What it showed





10.0.0.1



NAT gateway



135/tcp (MSRPC), 445/tcp



Windows-style RPC and SMB on the virtual router path





10.0.0.2



VirtualBox NIC



none in 1-1000



Host up, no services in the scanned range





10.0.0.5



lab-x-dc / CORP



53, 88, 135, 139, 389, 445, 464, 593, 636



AD DC: DNS (Simple DNS Plus), Kerberos, LDAP for corp.lab-x-dc.com, NetBIOS, RPC. NetBIOS name WIN-23NL888TQBS





10.0.0.8



corp-svr



22/tcp OpenSSH 8.9p1 Ubuntu



SSH only in this port range. MailHog ports were outside 1-1000





10.0.0.10



lab-x-sec-mon



443/tcp HTTPS



TLS site redirects to /app/login. Header osd-name: sec-mon matches the Wazuh dashboard





10.0.0.101



lab-x-ubu-client



22/tcp OpenSSH 8.9p1, 25/tcp Postfix



SSH plus local SMTP. Nmap hostname ubu-client





10.0.0.7



extra Debian/Kali-like host



22/tcp OpenSSH 10.3p1, 80/tcp Apache 2.4.68



Same service pair as the Kali box; treated as a second web/SSH listener on the NAT net





10.0.0.50



lab-x-kali



22/tcp OpenSSH 10.3p1, 80/tcp Apache 2.4.68



The attack host (SSH and the phishing site later hosted here)

Takeaway from recon: the DC advertises a full AD port set, corp-svr exposes SSH, MailHog was not visible in ports 1-1000, and the Wazuh UI is on HTTPS 443.

2. Weak SSH on corp-svr

Port 22 on 10.0.0.8 was reachable. The root account was given the dictionary password november on purpose so this lab could show what a guessable SSH secret looks like. After that password was found, login as root@corp-svr succeeded.

This is a configuration failure, not a software exploit:





Root SSH with a password was allowed.



The password was chosen from a common wordlist (november).



There was no account lockout that stopped repeated guesses in time.

Do not reuse that password outside this lab. Prefer SSH keys, disable password root login, and alert on repeated failed SSH in Wazuh (auth.log is already in the Linux agent group).

3. Local discovery after SSH

On corp-svr, listening sockets were listed with netstat -tuln. Besides SSH (22) and local DNS/CUPS, two extra listeners were bound on all interfaces:







Port



Service in this lab





1025/tcp



MailHog SMTP





8025/tcp



MailHog web UI and API

Those ports were not in the 1-1000 scan, so they only showed up after local access. The MailHog UI was opened in a browser at http://10.0.0.8:8025. Existing lab mail showed janed@ubu-client as a recipient.

The send script on the host (/home/mailhog/test_message.py) posted through localhost:1025 to janed@ubu-client from corpserver@example.com. That confirmed MailHog as an internal delivery path that does not require a real mail gateway.

4. Internal phishing simulation

A fake "Verify Your Password" page was hosted on Kali with Apache. The web service was started with:

sudo service apache2 start

The site was served at http://10.0.0.50 (port 80). The lab files for this mock campaign are:





Page: index.html



Form handler: process.php

The form asked for a username and password and posted them to process.php, which appended the submitted values to creds.log on the Kali host.

A second mail was sent through MailHog SMTP (localhost:1025 from the Kali side of the mail path used in the lab) to janed@linux-client. The message claimed to be HR/security, warned about a lockout, and linked to http://10.0.0.50.

From the janed Ubuntu client, the link was opened on purpose and a test username and password were submitted. The same values then appeared in creds.log on Kali.

This showed the full internal path:





Weak admin SSH on the mail host.



Unauthenticated MailHog UI and SMTP on the LAN.



A known mailbox (janed).



Apache on Kali serving the campaign files above.



A user who trusts an internal-looking message and submits a password.



5. Fixes this exercise supports





Disable root SSH passwords. Use keys and PermitRootLogin prohibit-password or no.



Use long, unique passwords (or no passwords) on every lab VM.



Do not bind MailHog SMTP/UI to 0.0.0.0 on a shared LAN. Keep it on localhost, or put it behind auth.



Treat port 8025/1025 as sensitive. They did not show up in a 1-1000 scan.



User training: unexpected "verify your password" mail with a raw IP link is phishing.



Detection: Wazuh Linux auth.log for SSH brute force; web/proxy or DNS logs for visits to 10.0.0.50; mail logs for spoofed From headers.





Wazuh Overview

Wazuh is an open-source platform that provides extended detection and response (XDR) and Security Information and Event Management (SIEM). It is used to protect cloud, container, and server workloads.

Wazuh includes log data analytics, intrusion and malware detection, file integrity monitoring (FIM), configuration assessment, vulnerability detection, and support for regulatory compliance.

Extended Detection and Response (XDR): XDR pulls data from multiple security layers into one platform. Sources can include workstations, servers, cloud environments, and network traffic. The goal is better detection, investigation, and response by looking at patterns and malicious activity in one place. In this lab, Wazuh covers multi-source data collection, threat detection, intrusion detection, incident investigation, and FIM.

Security Information and Event Management (SIEM): A SIEM combines log management, threat detection, and incident response. Wazuh collects and analyzes security data from multiple hosts, detects threats in near real time, and supports investigation and response.

Wazuh uses an agent-based model. Agents are installed on workstations, servers, containers, and virtual machines. Those agents send data to the Wazuh server for processing, aggregation, and visualization.

The Three Main Wazuh Components





Wazuh Indexer: A scalable full-text search and analytics engine. It indexes logs and stores alerts generated by the Wazuh server.



Wazuh Server: Analyzes data from the agents. It runs decoders and rules and uses threat intelligence to look for known indicators of compromise (IOCs). It also manages agents and can configure or upgrade them remotely.



Wazuh Dashboard: The web UI for visualization and analysis. It includes dashboards for threat hunting, compliance, vulnerable applications, FIM, configuration assessment, and cloud events. It is also used to manage Wazuh configuration and monitor status.

In this lab, Wazuh is the central hub for security logging, analysis, defense, and remediation during attack and defend exercises. It is used to watch what happens during initial access, lateral movement, privilege escalation, persistence, and exfiltration.

This project configures Wazuh SIEM, XDR, and File Integrity Monitoring (FIM). The Vulnerability Detection module already has a default configuration.

Security Implications

A SIEM and XDR stack helps monitor, detect, prevent, and respond to security activity. Many commercial tools work in a similar way. Wazuh was chosen because it is open source and covers a full set of capabilities in one platform.

Threat detection





Event correlation: Wazuh correlates logs from servers, endpoints, and other sources to find brute-force attempts, privilege escalation, and suspicious logins.



Real-time alerts: Wazuh can alert on unauthorized access, malware signs, and other anomalies so response can start quickly.

Proactive defense





Intrusion detection: Wazuh acts as a host-based intrusion detection system (HIDS). It watches file integrity, log integrity, and unauthorized changes.



Endpoint visibility: As part of XDR, it collects endpoint data that can show fileless malware, lateral movement, and ransomware activity.

Incident response and investigation





Automated responses: Wazuh can trigger actions such as blocking an IP or running a script to isolate a host.



Forensics and data collection: Stored logs help retrace attack steps, identify the entry path, and estimate impact.

Centralized security management





Single UI: Data from multiple hosts is viewed in one dashboard.



Integration: Wazuh can work with threat intelligence feeds, vulnerability data, and other security tools.

Threat hunting





Behavioral analysis: Analysts can look for unusual system and network behavior.



Custom rules: Detection rules can be written for this lab's specific hosts and attack paths.



Wazuh Agent Deployment

There are two main ways to manage agent configuration:





Centralized configuration (agent.conf)

Changes are made on the Wazuh manager and applied to agents. This file is used for log collection, shared settings, and active response policy. This is the method used in this lab.



Local configuration (ossec.conf)

Each agent can have its own local file. That is useful for one-off hosts, but it is harder to keep consistent across many agents.

When both files are present, the settings are merged. The last matching setting in agent.conf wins. Shared agent.conf settings overwrite conflicting local settings.

Lab Install and Agent Onboarding

Wazuh was installed on lab-x-sec-mon (10.0.0.10) with:

curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh && sudo bash ./wazuh-install.sh -a -i

Agents were added from the Wazuh GUI:





Open Server Management



Open Endpoint Summary



Select Deploy new agent



Generate and run the install script on each host

Enrolled endpoints:







Host



Account / identity



Agent group





lab-x-dc



Administrator



windows





lab-x-win-client



johnd



windows





lab-x-ubu-client



janed



linux

Two agent groups were created (windows and linux). Each host was assigned to the matching group.

Custom logs were added through the Wazuh GUI by editing the shared agent.conf for each group.

Windows group (windows)

<agent_config>
  <!-- Shared agent configuration here -->
  <localfile>
    <location>Security</location>
    <log_format>eventchannel</log_format>
  </localfile>
  <localfile>
    <location>Application</location>
    <log_format>eventchannel</log_format>
  </localfile>
</agent_config>

Linux group (linux)

<agent_config>
  <localfile>
    <log_format>syslog</log_format>
    <location>/var/log/auth.log</location>
  </localfile>
  <localfile>
    <log_format>syslog</log_format>
    <location>/var/log/secure</location>
  </localfile>
  <localfile>
    <log_format>audit</log_format>
    <location>/var/log/audit/audit.log</location>
  </localfile>
</agent_config>



Why These Logs Are the First Ones to Collect

These logs are a practical starting point because they cover identity, privilege use, and process or service changes without drowning the SIEM in low-value noise.

Windows Security and Application logs





Security: logon and logoff, failed logons, account lockouts, group membership changes, privilege use, and process creation. These are the first places to look for brute force, stolen credentials, and privilege escalation.



Application: service crashes, installer activity, and application errors. These often show up when malware, packers, or broken persistence mechanisms run.

Linux auth.log, secure, and audit.log





auth.log (Debian and Ubuntu) and secure (RHEL-style hosts): SSH logons, sudo use, user creation, and PAM events. These are the main sources for failed SSH, sudo abuse, and new local accounts.



audit.log: kernel audit records for file access, command execution, and policy changes. This is useful when you need more than syslog, such as who changed a binary or who ran a privileged command.

Collecting these first gives coverage of authentication, authorization, and host integrity. Broader channels (Sysmon, PowerShell, firewall, web logs) can be added later once the baseline is stable.





Linux Domain Integration (Samba Winbind and Kerberos)

Ubuntu clients are joined to the Active Directory domain using Kerberos (krb5-user), Samba, and Winbind. This allows domain authentication and automatic home directory creation on Linux.

Step 1: DNS and Kerberos

Point /etc/resolv.conf at the domain controller:

nameserver 10.0.0.5
search corp.lab-x-dc.com

Register the realm in /etc/krb5.conf:

[libdefaults]
    default_realm = CORP.LAB-X-DC.COM
    dns_lookup_realm = false
    dns_lookup_kdc = true



Step 2: Samba and Winbind (/etc/samba/smb.conf)

[global]
   kerberos method = secrets and keytab
   realm = CORP.LAB-X-DC.COM
   workgroup = CORP
   security = ads
   template shell = /bin/bash
   winbind enum groups = Yes
   winbind enum users = Yes
   winbind separator = +
   idmap config * : rangesize = 1000000
   idmap config * : range = 1000000-19999999
   idmap config * : backend = autorid



Step 3: Name Service Switch (/etc/nsswitch.conf)

passwd:         files systemd sss winbind
group:          files systemd sss winbind



Step 4: Home Directories and Domain Join

sudo pam-auth-update --enable mkhomedir
sudo net ads join -U Administrator
sudo systemctl restart smbd nmbd winbind
wbinfo -u
wbinfo -g





Corporate Mail Infrastructure

The mail path uses Docker and MailHog (SMTP on port 1025, Web UI and API on port 8025).

Python SMTP test from corp-svr

import smtplib
from email.message import EmailMessage

msg = EmailMessage()
msg.set_content("This is a test email from Ubuntu VM.")
msg["Subject"] = "Hello World from MailHog!"
msg["From"] = "corpserver@example.com"
msg["To"] = "user@example.com"

with smtplib.SMTP("localhost", 1025) as server:
    server.send_message(msg)



Mail watcher on lab-x-ubu-client (janed)

Jane Doe's host polls MailHog on 10.0.0.8:8025 every 30 seconds, parses JSON with jq, tracks seen message IDs, and prints new mail.

#!/bin/bash

MAILHOG_IP="10.0.0.8"
TO_EMAIL="janed"
POLL_INTERVAL=30  # seconds

echo "📡 Janed's Mail Watcher started... polling every $POLL_INTERVAL seconds"
echo "🔎 Watching for new mail sent to: $TO_EMAIL@"

# Keep track of seen message IDs
SEEN_IDS_FILE="/tmp/mailhog_seen_ids_janed.txt"
touch "$SEEN_IDS_FILE"

while true; do
  # Fetch current message list from MailHog API
  curl -s http://$MAILHOG_IP:8025/api/v2/messages | jq -c '.items[]' | while read -r msg; do
    TO=$(echo "$msg" | jq -r '.To[].Mailbox')
    ID=$(echo "$msg" | jq -r '.ID')

    if [[ "$TO" == "$TO_EMAIL" && ! $(grep -Fx "$ID" "$SEEN_IDS_FILE") ]]; then
      SUBJECT=$(echo "$msg" | jq -r '.Content.Headers.Subject[0]')
      BODY=$(echo "$msg" | jq -r '.Content.Body')

      echo -e "\n📬 New Email Received!"
      echo "Subject: $SUBJECT"
      echo "From: $(echo "$msg" | jq -r '.Content.Headers.From[0]')"
      echo "Date: $(echo "$msg" | jq -r '.Created')"
      echo -e "Message:\n$BODY"
      echo "-----------------------------------"

      echo "$ID" >> "$SEEN_IDS_FILE"
    fi
  done

  sleep "$POLL_INTERVAL"
done





VirtualBox Network Adapter Modes

VirtualBox has several adapter modes. They control how VMs talk to each other, the host, and the internet.





NAT: Default mode. The VM can reach the internet through the host IP. Other VMs and external hosts cannot connect inbound to it easily.



NAT Network: Same outbound internet path as NAT, but VMs on the same NAT Network can talk to each other. This lab uses lab-x-network.



Bridged Adapter: The VM sits on the same physical network as the host and gets an IP from the physical router.



Internal Network: VMs can talk only to other VMs on that internal network. No host and no internet.



Host-Only Adapter: VMs can talk to each other and to the host. No internet unless extra routing is added.





Active Directory Core Concepts



The Three Main Components of Active Directory





Authentication: Confirms identity ("Who are you?"). Credentials are checked with Kerberos or NTLM when a user logs on to a domain machine or service.



Authorization: Decides what the user can do. Access is based on group membership and access control lists (ACLs).



Management: Central control of users, computers, printers, and organizational units from one admin interface.



Strengths of Active Directory





Centralized management: Users, passwords, devices, and permissions are managed in one place.



Scalability: The same model works from a small lab to a large enterprise.



Group Policy (GPO): Password policy, security baselines, software, and desktop settings can be applied to many computers at once.



Service integration: AD works with DNS, DHCP, file shares, Certificate Services, and cloud identity platforms such as Microsoft Entra ID.



Why Active Directory Is Often Targeted





High impact: Domain Admin access can control every domain-joined computer and user.



Credential store: AD holds password hashes, Kerberos tickets, and privilege maps. That makes it a target for credential theft, lateral movement, and privilege escalation (Pass-the-Hash, Kerberoasting, Golden Ticket).



Configuration drift: Large domains collect old settings, weak permissions, and leftover accounts. Attackers look for those paths.





Skills and Technologies





Directory services: Windows Server 2022 AD DS, Kerberos, NTLM, domain users and computers



Linux identity: Samba, Winbind, PAM mkhomedir, NSSwitch



Networking: VirtualBox NAT Network, static IPs, Windows DHCP, DNS



Mail lab: Docker, MailHog, Python SMTP, Bash polling with curl and jq



Security monitoring: Wazuh 4.9 all-in-one, agent groups, centralized agent.conf, Windows EventChannel and Linux syslog/audit collection



Attack host: Kali Linux on lab-x-network (10.0.0.50)



Attack-and-defend exercise: lab subnet service mapping, weak SSH on corp-svr, MailHog abuse, simulated phishing to janed

