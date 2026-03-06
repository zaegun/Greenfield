# Identification & Authentication Architecture – NIST IA Controls

-----

## Technologies

|Technology                     |Role                                           |
|-------------------------------|-----------------------------------------------|
|**Active Directory (AD)**      |Primary Identity Store                         |
|**ADFS**                       |SSO / Claims-Based Auth / Adaptive Auth        |
|**SSSD**                       |Linux Authentication / PAM                     |
|**ADSys**                      |Linux Policy Enforcement                       |
|**YubiKey / FIDO2**            |MFA / Hardware Token                           |
|**Microsoft ADCS (Root CA)**   |PKI Root of Trust                              |
|**Microsoft ADCS (Issuing CA)**|Windows Certificate Issuance                   |
|**Dogtag CA**                  |Linux Certificate Issuance                     |
|**HSM (Primary)**              |Cryptographic Module / Key Protection          |
|**HSM (Backup)**               |Cryptographic Module Redundancy                |
|**HashiCorp Vault**            |Secrets & Authenticator Management             |
|**HashiCorp Boundary**         |Privileged Session Auth / Re-authentication    |
|**MidPoint**                   |Identity Lifecycle / Identity Proofing Workflow|
|**PacketFence**                |Device Authentication / NAC                    |
|**Pleasant Password Server**   |Credential Management                          |
|**Active Directory GPO**       |Session Timeout / Re-authentication Policy     |

-----

## Analysis

**Active Directory** - The authoritative identity store for all user, service, and computer accounts. All identifiers are issued, maintained, and deprovisioned through AD, with MidPoint governing the lifecycle workflow. AD enforces account lockout, password policy, and authentication feedback behaviors natively across the Windows environment.

**ADFS** - Provides internal SSO for claims-aware applications and services within the airgapped network, requiring no cloud dependency. Beyond SSO, ADFS serves as the adaptive authentication layer — issuing claims based on AD group membership, device state, and authentication method, allowing access decisions to be conditioned on contextual factors without requiring additional software.

**SSSD** - Handles Linux-side authentication against AD, providing Kerberos ticket acquisition, group membership resolution, and sudo rule enforcement for Ubuntu domain members. It works in conjunction with ADSys to ensure Linux systems are fully integrated into the AD identity and policy framework.

**ADSys** - Enforces GPO-based policy on Ubuntu systems, including authentication-related settings such as session controls and PAM configuration, ensuring Linux hosts conform to the same authentication standards as Windows systems.

**YubiKey / FIDO2** - Hardware tokens provide MFA across the environment without cloud dependency, satisfying the multi-factor authentication requirement for privileged and standard user access. YubiKeys support FIDO2/WebAuthn for modern authentication flows and PIV/smart card mode for certificate-based authentication against AD and ADFS, making them applicable across both Windows and Linux platforms.

**Microsoft ADCS (Root CA)** - The offline root of trust for the entire PKI infrastructure. Its private keys are protected by HSM, and it issues only to subordinate issuing CAs. The Root CA underpins all certificate-based authentication across the environment.

**Microsoft ADCS (Issuing CA)** - Issues certificates to Windows systems, users, and services, supporting machine authentication, user smart card logon, and service identity. It integrates with AD for auto-enrollment, ensuring certificates are provisioned and renewed through a governed process.

**Dogtag CA** - The standalone Linux issuing CA, subordinate to the same Microsoft Root CA, providing certificate issuance for Ubuntu systems and Linux-based services. Running Dogtag without FreeIPA maintains a single root of trust without introducing a competing directory infrastructure.

**HSM (Primary)** - Provides FIPS 140-3 validated cryptographic module protection for Root CA private keys and other sensitive key material. While Ubuntu’s full FIPS 140-3 certification is pending later in the year, the environment is architected to meet this standard as a target compliance posture, with the HSM serving as the hardware anchor for cryptographic operations.

**HSM (Backup)** - Mirrors key material from the primary HSM, ensuring cryptographic continuity in the event of primary HSM failure. This is a strongly recommended component — loss of the primary HSM without a backup results in permanent loss of Root CA key material.

**HashiCorp Vault** - Manages secrets, service account credentials, and dynamic credentials across the environment. Vault enforces authenticator rotation policies and ensures privileged credentials are never static or directly exposed. The Vault SSH secrets engine is a candidate for managing SSH key issuance and rotation for Linux systems, though the specific mechanism for SSH key governance is a pending decision point.

**HashiCorp Boundary** - Brokers privileged sessions, ensuring users authenticate through Boundary to reach sensitive targets rather than using direct credentials. Boundary enforces re-authentication requirements at session initiation and applies session time limits, satisfying IA-11 re-authentication requirements for privileged access scenarios.

**MidPoint** - Governs the identity proofing and provisioning workflow — ensuring that before any identifier is created in AD, a documented approval and verification process has been completed. MidPoint provides the procedural and technical enforcement of identity proofing requirements under IA-12.

**PacketFence** - Authenticates devices at the network layer, ensuring only authorized and identified devices are permitted network access. It integrates with AD for identity-aware policy and supports certificate-based device authentication via the PKI infrastructure.

**Pleasant Password Server** - Manages shared and service account credentials that fall outside the Vault PAM scope, providing RBAC-controlled access to stored credentials with AD-based authentication for access to the vault itself.

**Active Directory GPO** - Enforces session timeout and screen lock policies on Windows systems, triggering re-authentication after defined inactivity periods. Combined with Boundary session limits for privileged access, GPO ensures re-authentication requirements are met at both the workstation and privileged session level.

-----

## NIST IA Controls

|NIST Control                                              |Fulfilling Technologies                                                     |
|----------------------------------------------------------|----------------------------------------------------------------------------|
|**IA-1** – Identification & Authentication Policy         |Document                                                                    |
|**IA-2** – Identification & Authentication (Users)        |Active Directory, ADFS, YubiKey / FIDO2, SSSD                               |
|**IA-3** – Device Identification & Authentication         |Active Directory, Microsoft ADCS (Issuing CA), PacketFence                  |
|**IA-4** – Identifier Management                          |Active Directory, MidPoint                                                  |
|**IA-5** – Authenticator Management                       |Active Directory, HashiCorp Vault, YubiKey / FIDO2, Pleasant Password Server|
|**IA-5 (SSH Keys)** – SSH Authenticator Management        |                                          |
|**IA-6** – Authentication Feedback                        |Active Directory, ADFS                                                      |
|**IA-7** – Cryptographic Module Authentication            |HSM (Primary), HSM (Backup), Microsoft ADCS, Dogtag CA                      |
|**IA-8** – Identification & Authentication (Non-Org Users)|ADFS                                                                        |
|**IA-9** – Service Identification & Authentication        |                                      |
|**IA-10** – Adaptive Authentication                       |ADFS                                                                        |
|**IA-11** – Re-authentication                             |Active Directory GPO, HashiCorp Boundary                                    |
|**IA-12** – Identity Proofing                             |MidPoint                                                                    |


