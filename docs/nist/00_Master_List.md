# Master List
All Nist controls have a table at the beginning. This is the aggregate of all those tables.

| Technology                         | Role                                                                 |
|------------------------------------|----------------------------------------------------------------------|
| **Active Directory (AD)**          | Primary Identity Store / Primary IDM / Directory                     |
| **ADFS**                           | Internal SSO / Claims-Based Auth / Adaptive Auth                     |
| **SSSD**                           | Linux AD Integration / Linux Authentication / PAM                    |
| **ADSys**                          | Linux GPO / Policy Enforcement                                       |
| **Active Directory GPO**           | Windows Policy Enforcement / Baseline Compliance / Session Policy    |
| **Active Directory DNS**           | Internal Name Resolution                                             |
| **YubiKey / FIDO2**                | MFA / Hardware Token                                                 |
| **MidPoint**                       | Identity Governance / Identity Lifecycle / Identity Proofing         |
| **SailPoint**                      | IGA (Alternative)                                                    |
| **PacketFence**                    | Network Access Control / Device Authentication / Segmentation        |
| **Cisco ISE**                      | NAC (Alternative)                                                    |
| **HashiCorp Vault**                | Secrets Management / Certificate Mgmt / Authenticator Management     |
| **HashiCorp Boundary**             | Privileged Session Brokering / Privileged Session Protection         |
| **Netwrix Privilege Secure**       | PAM (Alternative)                                                    |
| **CyberArk**                       | PAM (Alternative)                                                    |
| **Pleasant Password Server**       | Enterprise Credential / Password Management                          |
| **Microsoft ADCS (Root CA)**       | PKI Root of Trust                                                    |
| **Microsoft ADCS (Issuing CA)**    | Windows Certificate Issuance                                         |
| **Dogtag CA**                      | Linux Certificate Issuance                                           |
| **HSM (Primary)**                  | Cryptographic Module / Key Protection                                |
| **HSM (Backup)**                   | Cryptographic Module Redundancy                                      |
| **Rsyslog**                        | Syslog Forwarder                                                     |
| **Windows Event Forwarding (WEF)** | Windows Event Transport                                              |
| **Windows Event Collector (WEC)**  | Windows Event Aggregator                                             |
| **Sysmon (Enhanced)**              | Windows Event Enrichment                                             |
| **Graylog Sidecar**                | Log Agent / Collector                                                |
| **Graylog**                        | Log Aggregation / SIEM / Integrity Monitoring                        |
| **SolarWinds SEM**                 | Security Event Correlation / Alerting / FIM Alerting                 |
| **SolarWinds Observability**       | Infra Monitoring / Vulnerability Scanning / FIM / Network Monitoring |
| **SolarWinds IPAM**                | IP Address Management                                                |
| **Proxmox VE**                     | Virtualization / Network Virtualization                              |
| **Proxmox Backup Server (PBS)**    | Backup Indexer / Manager                                             |
| **Hardened Backup Repository VM**  | Multi-Protocol Backup Target                                         |
| **Secondary Servers**              | Recovery Priority Systems                                            |
| **Root CA**                        | PKI Continuity                                                       |
| **Offsite Backup**                 | Geographic Backup Redundancy                                         |
| **GLPI ITSM**                      | Change Ticketing / ITAM / Risk Register / Finding Tracking           |
| **AWX / Ansible**                  | Change & Patch Orchestration                                         |
| **Gitea**                          | Code / Playbook / Config Repository                                  |
| **WSUS**                           | Windows Update Approval / Patch Source                               |
| **APT Repository (TBD)**           | Linux Update Approval / Patch Source                                 |
| **Ubuntu Landscape**               | Linux Patch Management                                               |
| **Ubuntu USG**                     | Linux STIG Compliance Scanning                                       |
| **SolarWinds Observability (FIM)** | Drift / File Integrity Monitoring                                    |
| **Bitdefender XDR**                | Endpoint Detection & Response                                        |
| **SentinelOne**                    | EDR / XDR (Alternative)                                              |
| **Nessus / Tenable**               | Deep Credentialed Vulnerability Scanning                             |
| **Nmap**                           | Network Discovery / Service Enumeration                              |
| **DMZ Jump Boxes (Windows/Linux)** | Controlled Remote Access Gateway                                     |
| **Postfix SMTP Server**            | Email Server Notification                                          |