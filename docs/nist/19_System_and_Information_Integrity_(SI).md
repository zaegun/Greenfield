# System & Information Integrity Architecture – NIST SI Controls

-----

## Technologies

|Technology                                 |Role                                            |
|-------------------------------------------|------------------------------------------------|
|**Bitdefender XDR**                        |EDR / XDR                                       |
|**SentinelOne**                            |EDR / XDR (Alternative)                         |
|**SolarWinds Observability**               |Vulnerability Scanning / FIM / Health Monitoring|
|**SolarWinds SEM**                         |Security Event Correlation / Alerting           |
|**Graylog**                                |Log Aggregation / Integrity Monitoring          |
|**AWX / Ansible**                          |Patch Orchestration                             |
|**WSUS**                                   |Windows Patch Source                            |
|**APT Repository (TBD)**                   |Linux Patch Source                              |
|**Ubuntu Landscape**                       |Linux Patch Manager                             |
|**Postfix SMTP Server**                    |Email Server Notification                     |
|**HashiCorp Vault**                        |Secrets Integrity                               |

-----

## Analysis

**Bitdefender XDR** - XDR platform for threat detection, automated response, and protection of endpoints across the network. 

**SentinelOne (Alternative)** -  It provides endpoint threat detection, behavioral AI, automated response, and XDR correlation across the environment.

**SolarWinds Observability** - Provides vulnerability scanning across the environment, identifying unpatched systems, misconfigurations, and software flaws. It also provides file integrity monitoring to detect unauthorized changes to critical system files and configurations, and monitors overall system health for anomalies that may indicate integrity issues.

**SolarWinds SEM** - Correlates security events from across the environment, including EDR alerts, FIM events, IDS/IPS detections, and log anomalies. It provides alerting and reporting for integrity-related events and serves as the central security operations console for SI-related monitoring and response.

**Graylog** - Aggregates logs from all sources and provides the indexing and search layer for security event analysis. In the SI context, Graylog ingests EDR telemetry, IDS/IPS alerts, and system integrity events, enabling correlation and investigation of potential integrity violations.

**AWX / Ansible** - The orchestration engine for patch deployment across the environment. All flaw remediation through patching is initiated and tracked through AWX, ensuring patches are applied in a controlled, auditable manner with change records in GLPI. Phased deployment rings prevent simultaneous patching of all systems.

**WSUS** - Serves as the approved and cached source for Windows updates within the airgapped network. It enforces an approval workflow ensuring only tested and authorized patches are made available for deployment by AWX.

**APT Repository (TBD)** - All Ubuntu systems pull patches from this internal repository rather than reaching external sources directly, maintaining the airgap while ensuring timely flaw remediation. 

**Ubuntu Landscape** - Works with AWX for distribution of Ubuntu updates. Used for tracking patch compliance.

**Postfix SMTP Server** - Provides outbound alert notification for system-generated alerts from monitoring, backup, and security platforms. 

**HashiCorp Vault** - Maintains the integrity of secrets and credentials across the environment through dynamic secrets, rotation policies, and access controls. It ensures that credential compromise does not persist due to static or unrotated authenticators.

-----

## NIST SI Controls

|NIST Control                                    |Fulfilling Technologies                                                              |
|------------------------------------------------|-------------------------------------------------------------------------------------|
|**SI-1** – System & Information Integrity Policy|Document                                                                             |
|**SI-2** – Flaw Remediation                     |AWX, Ansible, WSUS, APT Repository, Landscape, SolarWinds Observability                   |
|**SI-3** – Malicious Code Protection            |EDR / XDR,                |
|**SI-4** – System Monitoring                    |SolarWinds Observability, SolarWinds SEM, Graylog|
|**SI-5** – Security Alerts & Advisories         |SolarWinds SEM, SMTP Server|
|**SI-6** – Security Function Verification       |SolarWinds Observability                                                      |
|**SI-7** – Software & Firmware Integrity        |SolarWinds Observability (FIM), AWX / Ansible                                 |
|**SI-8** – Spam Protection                      |                                                                                     |
|**SI-10** – Information Input Validation        |                                                                                     |
|**SI-12** – Information Management & Retention  |Graylog, SolarWinds SEM                                                              |
|**SI-16** – Memory Protection                   |EDR / XDR, Active Directory GPO                      |
