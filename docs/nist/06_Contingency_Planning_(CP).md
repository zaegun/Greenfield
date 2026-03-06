# Continuity Architecture – NIST CP Controls

-----

## Technologies

|Technology                       |Role                              |
|---------------------------------|----------------------------------|
|**Proxmox VE**                   |Virtualization Platform           |
|**Proxmox Backup Server (PBS)**  |Backup Indexer / Manager          |
|**Hardened Backup Repository VM**|Multi-Protocol Backup Target      |
|**Secondary Servers**            |Recovery Priority Systems         |
|**Root CA**                      |PKI Continuity                    |
|**HSM (Primary)**                |Key Protection                    |
|**HSM (Backup)**                 |Key Redundancy                    |
|**SolarWinds Observability**     |Infrastructure / Backup Monitoring|
|**SolarWinds SEM**               |Security Event Alerting           |
|**Offsite Backup**               |Geographic Redundancy             |

-----

## Analysis

**Proxmox VE** - The hypervisor platform underpinning the environment. The primary cluster is setup with Ceph, but the secondary servers for disaster recovery are setup with ZRAID for direct access.

**Proxmox Backup Server (PBS)** - It handles scheduled VM snapshot backups across both Proxmox environments. This is the primary backup service.

**Veeam (Alternative)** - If PBS is deemed unsuitable, Veeam works directly with Proxmox hosts to backup virtual machines.

**Backup Repository VM** - Serves as the ingestion point for systems that cannot use PBS natively. It is configured to support various protocols based on what internal services need for backups. The VM is hardened to protect backup data in transit and at rest, and is itself backed up by PBS.

**Secondary Servers** - VMs are hosted on the DAS Proxmox node as recovery-priority systems. In the event of a primary cluster failure, these VMs provide continued identity and name resolution services. They are backed up via PBS VM-level snapshots.

**Root CA** - Private keys are backed up to the HSM rather than to file-based storage, ensuring PKI continuity without exposing key material to the general backup pipeline.

**HSM (Primary)** - Protects Root CA key material in hardware, preventing exposure during backup and recovery operations. The specific HSM platform is TBD.

**HSM (Backup)** - A recommended secondary HSM that mirrors the key material from the primary. Without a backup HSM, loss or failure of the primary HSM results in permanent loss of the Root CA private key and the ability to issue or revoke certificates. This is strongly recommended before production deployment.

**SolarWinds Observability** - The primary monitoring platform for infrastructure and backup health. It receives backup job success and failure status from PBS, monitors the availability and performance of the Proxmox hosts, and provides dashboarding for the overall continuity infrastructure. Integration with PBS needs to be further researched.

**SolarWinds SEM** - Sits alongside Observability but focuses on security event correlation. In the CP context, it would handle alerting on anomalous backup patterns, repeated failures, or access events against the backup infrastructure that may indicate a security incident rather than a simple operational fault.

**Offsite Backup** - Provides geographic separation for backups in accordance with 3-2-1 backup principles. 

-----

## NIST CP Controls

|NIST Control                                |Fulfilling Technologies                                                                 |
|--------------------------------------------|----------------------------------------------------------------------------------------|
|**CP-1** – Contingency Planning Policy      |Document                                                                                |
|**CP-2** – Contingency Plan                 |Document                                                                                |
|**CP-3** – Contingency Training             |Document                                                                                |
|**CP-4** – Contingency Plan Testing         |Document                                                                                |
|**CP-6** – Alternate Storage Site           |Offsite Backup Target                                                                   |
|**CP-7** – Alternate Processing Site        |Proxmox VE (DAS Node), Secondary Servers                                          |
|**CP-8** – Telecommunications Services      |                                                                                        |
|**CP-9** – System Backup                    |PBS, Backup Repository VM, Offsite Backup, SolarWinds Observability|
|**CP-10** – System Recovery & Reconstitution|PBS, Proxmox VE, HSM (Primary), HSM (Backup)                                            |


