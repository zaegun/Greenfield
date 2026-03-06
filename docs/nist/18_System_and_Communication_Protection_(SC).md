# System & Communications Protection Architecture – NIST SC Controls

-----

## Technologies

|Technology                          |Role                                      |
|------------------------------------|------------------------------------------|
|**DMZ Jump Boxes (Windows & Linux)**|Controlled Remote Access Gateway          |
|**PacketFence**                     |Network Segmentation Enforcement / NAC    |
|**Proxmox VE**                      |Network Virtualization / VLAN Segmentation|
|**Microsoft ADCS (Root CA)**        |PKI Root of Trust                         |
|**Microsoft ADCS (Issuing CA)**     |Windows Certificate Issuance              |
|**Dogtag CA**                       |Linux Certificate Issuance                |
|**HSM (Primary)**                   |Cryptographic Key Protection              |
|**HSM (Backup)**                    |Cryptographic Key Redundancy              |
|**HashiCorp Vault**                 |Secrets / Certificate Management          |
|**Active Directory DNS**            |Internal Name Resolution                  |
|**Active Directory GPO**            |TLS Policy Enforcement                    |
|**ADFS**                            |Secure Token-Based Communications         |
|**HashiCorp Boundary**              |Internal Privileged Session Protection    |
|**SolarWinds Observability**        |Network Traffic Monitoring                |

-----

## Analysis

**DMZ Jump Boxes (Windows & Linux)** - Serve as the controlled gateway between remote users and internal systems. Users authenticate via VPN with YubiKey MFA, then RDP or SSH into the appropriate jump box. From the jump box, they access internal resources — ensuring no direct external connectivity to internal systems exists. Both Windows and Linux jump boxes are provided to support the range of administrative tasks required.

**PacketFence** - Enforces network segmentation at the access layer, ensuring devices are placed into the appropriate VLAN based on identity, device posture, and authorization. It works in conjunction with Proxmox VLAN segmentation and firewall boundary enforcement to implement a layered segmentation model.

**Proxmox VE** - Provides network virtualization and VLAN segmentation for the virtual infrastructure, ensuring VM workloads are isolated into appropriate network segments and that inter-VM traffic is controlled at the virtual switch layer.

**Microsoft ADCS (Root CA)** - The offline root of trust for all PKI operations in the environment. Its private keys are protected by HSM, and it issues only to subordinate issuing CAs, maintaining a clean chain of trust for all certificate-based communications protections.

**Microsoft ADCS (Issuing CA)** - Issues certificates supporting encrypted communications for Windows systems, services, and users — including TLS certificates for internal services, machine authentication certificates, and user certificates for smart card and encrypted communications.

**Dogtag CA** - Issues certificates for Linux systems and services, subordinate to the same root of trust as the Windows PKI, ensuring consistent cryptographic identity across both platforms.

**HSM (Primary)** - Provides FIPS 140-3 targeted cryptographic module protection for CA private keys and other sensitive key material, ensuring key operations occur in tamper-resistant hardware rather than software. This is the hardware anchor for all PKI-based communications protections.

**HSM (Backup)** - Mirrors the primary HSM to ensure cryptographic continuity. Loss of the primary HSM without a backup would compromise the ability to issue or renew certificates, breaking encrypted communications across the environment.

**HashiCorp Vault** - Manages TLS certificates, secrets, and dynamic credentials for services and applications. It provides a centralized, auditable interface for certificate lifecycle management and ensures secrets used in service communications are rotated and access-controlled.

**Active Directory DNS** - Provides internal name resolution for the environment. DNS is a critical communications protection component. DNSSEC and DNS filtering need to be implemented. DNS RPZ (Response Policy Zones) or a DNS filtering solution should be evaluated to prevent DNS-based attacks and provide additional SC-20/SC-21 coverage.

**Active Directory GPO** - Enforces TLS version policy across Windows systems. TLS 1.3 should be targeted.

**ADFS** - Provides secure token-based communications for SSO flows, issuing signed and encrypted SAML/OAuth tokens for claims-aware applications. All ADFS communications are protected by TLS certificates issued from the internal PKI.

**HashiCorp Boundary** - Protects internal privileged sessions by brokering access to sensitive targets without exposing direct credentials or network paths. Once users are on the internal network, Boundary controls and encrypts privileged sessions to infrastructure targets.

**SolarWinds Observability** - Monitors network traffic, performance, and communication flows across the environment, providing visibility into bandwidth utilization, connection patterns, and anomalies that may indicate communications integrity issues or unauthorized traffic flows.

-----

## NIST SC Controls

|NIST Control                                               |Fulfilling Technologies                                                             |
|-----------------------------------------------------------|------------------------------------------------------------------------------------|
|**SC-1** – System & Communications Protection Policy       |Document                                                                            |
|**SC-2** – Separation of System & User Functionality       |Proxmox VE, PacketFence, DMZ Jump Boxes                                             |
|**SC-3** – Security Function Isolation                     |Firewall, PacketFence, Proxmox VE                                             |
|**SC-4** – Information in Shared System Resources          |Active Directory GPO, Proxmox VE                                                    |
|**SC-5** – Denial of Service Protection                    |                                                        |
|**SC-7** – Boundary Protection                             |Firewall, PacketFence, DMZ Jump Boxes                                         |
|**SC-8** – Transmission Confidentiality & Integrity        |Firewall Service, Active Directory GPO, Microsoft ADCS, Dogtag CA, ADFS|
|**SC-10** – Network Disconnect                             |Active Directory GPO, ADFS, HashiCorp Boundary                                      |
|**SC-12** – Cryptographic Key Establishment & Management   |Microsoft ADCS, Dogtag CA, HSM (Primary), HSM (Backup), HashiCorp Vault             |
|**SC-13** – Cryptographic Protection                       |HSM (Primary), Microsoft ADCS, Dogtag CA, YubiKey / FIDO2                           |
|**SC-15** – Collaborative Computing Devices                |                                                                                    |
|**SC-17** – Public Key Infrastructure Certificates         |Microsoft ADCS (Root CA), Microsoft ADCS (Issuing CA), Dogtag CA                    |
|**SC-18** – Mobile Code                                    |Active Directory GPO                                                |
|**SC-20** – Secure Name / Address Resolution (Auth)        |DNS   |
|**SC-21** – Secure Name / Address Resolution (Recursive)   |DNS    |
|**SC-22** – Architecture & Provisioning for Name Resolution|DNS    |
|**SC-23** – Session Authenticity                           |ADFS, HashiCorp Boundary                             |
|**SC-28** – Protection of Information at Rest              |HashiCorp Vault, Active Directory GPO                                               |
|**SC-39** – Process Isolation                              |Proxmox VE, Active Directory GPO                                                    |