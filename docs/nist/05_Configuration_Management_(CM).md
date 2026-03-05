# Access Control Architecture – NIST AC Controls

-----

## Technologies

|Technology                     |Role                          |
|-------------------------------|------------------------------|
|**Active Directory (AD)**      |Primary IDM / Directory       |
|**ADSys**                      |Linux GPO Enforcement         |
|**SSSD**                       |Linux AD Integration / PAM    |
|**ADFS**                       |Internal SSO / Claims Provider|
|**Microsoft ADCS (Root CA)**   |PKI Root of Trust             |
|**Microsoft ADCS (Issuing CA)**|Windows Certificate Issuance  |
|**Dogtag CA**                  |Linux Certificate Issuance    |
|**YubiKey / FIDO2**            |MFA / Hardware Token          |
|**MidPoint**                   |Identity Governance (IGA)     |
|**SailPoint**                  |IGA (Notable Alternative)     |
|**PacketFence**                |Network Access Control        |
|**Cisco ISE**                  |NAC (Notable Alternative)     |
|**HashiCorp Vault**            |Secrets Management / PAM      |
|**HashiCorp Boundary**         |Privileged Session Brokering  |
|**Netwrix Privilege Secure**   |PAM (Notable Alternative)     |
|**CyberArk**                   |PAM (Notable Alternative)     |
|**Pleasant Password Server**   |Enterprise Password Manager   |

-----

## Analysis

### Controller
**Active Directory** - The authoritative identity store for the environment. All user accounts, computer objects, and security groups are managed here. RBAC is enforced exclusively through security group membership, making AD the enforcement backbone for access control across both Windows and Linux systems.

**ADSys** - Extends AD Group Policy to Ubuntu systems joined to the domain. It allows Linux machines to receive and apply GPO-based configuration and policy without requiring a separate Linux directory infrastructure, keeping policy management centralized in AD.

**SSSD** - Handles the low-level AD integration on Ubuntu systems — providing Kerberos authentication, group membership resolution, and sudo rule enforcement sourced from AD. It works alongside ADSys, with ADSys managing policy application and SSSD handling identity and PAM-level authentication.

**ADFS** - Provides internal SSO for applications and services that support claims-based authentication. As the environment is airgapped, ADFS operates entirely on-premises with no cloud dependency, issuing tokens based on AD identity and group membership for federated access within the network.

### PKI
**Microsoft ADCS (Root CA)** - The trust anchor for the entire PKI infrastructure. The Root CA is kept offline except for issuing subordinate CA certificates, with private keys protected by HSM as noted in the CP architecture. It issues to both the Windows and Linux issuing CAs, maintaining a single root of trust across platforms.

**Microsoft ADCS (Issuing CA)** - The online subordinate CA responsible for issuing certificates to Windows systems, users, and services. It integrates natively with AD for auto-enrollment, supporting machine certificates, user smart card certificates, and service authentication certificates.

**Dogtag CA** - A standalone Linux-based issuing CA, subordinate to the same Microsoft Root CA, dedicated to issuing certificates for Linux systems and services. Running Dogtag without FreeIPA keeps the footprint minimal and avoids introducing a competing directory infrastructure for Ubuntu domain members.

**YubiKey / FIDO2** - Hardware tokens provide MFA for the airgapped environment without any cloud dependency. They support FIDO2/WebAuthn for modern authentication flows as well as PIV/smart card mode for certificate-based authentication against AD and ADFS, making them versatile across both Windows and Linux contexts.

### IGP
**MidPoint** - The selected Identity Governance and Administration (IGA) platform, responsible for identity lifecycle management — provisioning, deprovisioning, role management, and access certification. It connects to AD as its authoritative target, ensuring group memberships and RBAC assignments are governed through a controlled workflow rather than ad hoc changes.

**SailPoint (Alternate)** - A mature enterprise alternative to MidPoint for IGA, offering similar lifecycle and governance capabilities with broader vendor support.

### NAC
**PacketFence** - Provides Network Access Control, enforcing that only authorized and compliant devices can connect to the network. It integrates with AD for identity-aware policy decisions and can enforce VLAN segmentation based on device posture and group membership.

**Cisco ISE (Alternate)** - An enterprise alternative to PacketFence, offering deeper integration with Cisco switching and wireless infrastructure and broader posture assessment capabilities.


### PAM
**HashiCorp Vault** - Manages secrets and privileged credentials across the environment. It provides dynamic secrets, credential rotation, and PKI engine capabilities, ensuring privileged account credentials are never static or directly accessible to end users. It serves as the PAM secrets backend for the environment.

**HashiCorp Boundary** - Provides privileged session brokering, controlling and auditing access to sensitive targets such as servers, databases, and infrastructure components. Users access targets through Boundary without ever receiving direct credentials, with sessions logged for audit purposes.

**Netwrix Privilege Secure (Alternate)** - A PAM platform combining privileged account discovery, session recording, and just-in-time access capabilities.

**CyberArk (Alternate)** - An enterprise-grade PAM, offering the most comprehensive privileged access management feature set including full session recording, threat analytics, and broad platform coverage.


### Password Management
**Pleasant Password Server** - Provides enterprise password management for team-shared credentials and service account passwords that fall outside the Vault/Boundary PAM scope. It integrates with AD for authentication and supports RBAC-based access to credential folders.

-----

## NIST AC Controls

|NIST Control                                        |Fulfilling Technologies                                        |
|----------------------------------------------------|---------------------------------------------------------------|
|**AC-1** – Access Control Policy                    |Document                                                       |
|**AC-2** – Account Management                       |Active Directory, MidPoint                                     |
|**AC-3** – Access Enforcement                       |Active Directory, SSSD, PacketFence                            |
|**AC-4** – Information Flow Enforcement             |PacketFence                                                    |
|**AC-5** – Separation of Duties                     |Active Directory, MidPoint                                     |
|**AC-6** – Least Privilege                          |Active Directory, HashiCorp Vault, HashiCorp Boundary, MidPoint|
|**AC-7** – Unsuccessful Logon Attempts              |Active Directory, ADFS                                         |
|**AC-8** – System Use Notification                  |Active Directory (GPO), ADSys                                  |
|**AC-9** – Previous Logon Notification              |Active Directory                                               |
|**AC-11** – Device Lock                             |Active Directory (GPO), ADSys                                  |
|**AC-12** – Session Termination                     |ADFS, HashiCorp Boundary                                       |
|**AC-14** – Permitted Actions Without Identification|Document                                                       |
|**AC-17** – Remote Access                           |HashiCorp Boundary                                             |
|**AC-18** – Wireless Access                         |PacketFence                                                    |
|**AC-19** – Access Control for Mobile Devices       |PacketFence                                                    |
|**AC-20** – Use of External Systems                 |Document                                                       |
|**AC-21** – Information Sharing                     |Document                                                       |
|**AC-22** – Publicly Accessible Content             |Document                                                       |
|**AC-24** – Access Control Decisions                |Active Directory, MidPoint, ADFS                               |




