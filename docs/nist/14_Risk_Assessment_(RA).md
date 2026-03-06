# Risk Assessment Architecture – NIST RA Controls

-----

## Technologies

|Technology                  |Role                                              |
|----------------------------|--------------------------------------------------|
|**SolarWinds Observability**|Credentialed Vulnerability Scanning / CVE Tracking|
|**Nessus / Tenable**        |Deep Credentialed Scanning (Notable Alternative)  |
|**SolarWinds SEM**          |KEV Correlation / Risk Alerting                   |
|**Nmap**                    |Network Discovery / Service Enumeration           |
|**GLPI ITSM**               |Risk Register / Finding Tracking                  |
|**Ubuntu USG**              |Linux STIG Compliance Scanning                    |
|**Active Directory GPO**    |Windows Baseline Compliance Enforcement           |


-----

## Analysis

**SolarWinds Observability** - The primary vulnerability scanning platform. It performs credentialed scans against Windows systems via WMI/WinRM and Linux systems via SSH, producing per-host CVE inventories and missing patch reports based on actual installed software versions. It tracks CISA Known Exploited Vulnerabilities (KEVs) and provides risk scoring per system, giving operators a continuously updated view of vulnerability posture across the environment. Credentialed scan configuration should be explicitly documented for audit purposes under NIST 800-171 RA-5.

**Nessus (Altnerative)** - Vulnerability scanning tool. Nessus offers extensive plugin coverage for STIG and CIS benchmark checks, detailed CVE-to-configuration mapping, and compliance reporting outputs. It would be evaluated if SolarWinds scanning is found insufficient during a NIST 800-171 assessment.

**SolarWinds SEM** - Correlates vulnerability and KEV data with security events, providing alerting when systems with known critical vulnerabilities exhibit suspicious behavior. It serves as the escalation layer for high-priority risk findings that require immediate attention beyond standard patch cycle remediation.

**Nmap** - Provides active network discovery and service enumeration across the environment. It identifies hosts, open ports, and running service versions, supplementing SolarWinds asset inventory with network-perspective visibility. Nmap scans are particularly useful for identifying unauthorized devices, unexpected open ports, and services running outside their approved baseline.

**GLPI ITSM** - Serves as the risk register for the environment. Vulnerability scan findings are recorded as GLPI tickets, capturing the identified risk, affected systems, CVE references, severity, and remediation actions. Ticket status tracks remediation progress, and closure documents completion. This provides the formal finding lifecycle record required under RA-3 and RA-5 without requiring a dedicated GRC platform.

**Ubuntu USG** - Performs STIG compliance scanning on Ubuntu systems, validating configuration against the approved security baseline. 

**Active Directory GPO** - Enforces Windows security baseline configuration, providing a continuous compliance enforcement mechanism between scan cycles. GPO-enforced settings reduce the attack surface and limit the number of configuration-based findings generated during vulnerability assessments.


-----

## NIST RA Controls

|NIST Control                                  |Fulfilling Technologies                                      |
|----------------------------------------------|-------------------------------------------------------------|
|**RA-1** – Risk Assessment Policy             |Document                                                     |
|**RA-2** – Security Categorization            |Document                                                     |
|**RA-3** – Risk Assessment                    |GLPI ITSM, SolarWinds Observability, SolarWinds SEM          |
|**RA-3(1)** – Supply Chain Risk Assessment    |Document                                                     |
|**RA-5** – Vulnerability Monitoring & Scanning|SolarWinds Observability, Nmap, USG|
|**RA-5(2)** – Update Vulnerabilities Scanned  |SolarWinds Observability, SolarWinds SEM                     |
|**RA-5(4)** – Discoverable Information        |Nmap                                                         |
|**RA-5(5)** – Privileged Access               |SolarWinds Observability (credentialed), Active Directory GPO|
|**RA-7** – Risk Response                      |GLPI ITSM, AWX / Ansible                                     |
|**RA-9** – Criticality Analysis               |GLPI ITSM, SolarWinds Observability                          |


