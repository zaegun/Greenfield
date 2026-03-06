# Configuration Management Architecture – NIST CM Controls

-----

## Technologies

|Technology                        |Role                               |
|----------------------------------|-----------------------------------|
|**Active Directory GPO**          |Windows Policy Enforcement         |
|**ADSys**                         |Linux Policy Enforcement           |
|**AWX**                           |Change Orchestration               |
|**Gitea**                         |Code / Playbook / Config Repository|
|**WSUS**                          |Windows Update Approval & Cache    |
|**APT Repository (TBD)**          |Linux Update Approval & Cache      |
|**Ubuntu USG**.                   |Linux STIG Compliance Scanning     |
|**SolarWinds Observability (FIM)**|Drift / File Integrity Monitoring  |
|**SolarWinds SEM**                |FIM Alerting & Correlation         |
|**GLPI ITSM**                     |Change Ticketing / ITAM            |
|**SolarWinds IPAM**               |IP Address Management              |

-----

## Analysis

**Active Directory GPO** - Enforces baseline configuration on all Windows systems. Security settings, software restrictions, Windows Update behavior — including GPO-enforced blocking of autonomous patching — and STIG-based hardening controls are all applied and maintained through Group Policy. GPO is the primary mechanism ensuring Windows systems do not drift from their approved baseline between change windows.

**ADSys** - Extends GPO-based policy enforcement to Ubuntu systems joined to the domain, ensuring Linux hosts receive and apply the same centrally managed configuration controls without requiring a separate Linux policy infrastructure.

**AWX** - The orchestration layer for all configuration change operations across both Windows and Linux systems. All patching, configuration deployments, and automation jobs are initiated through AWX, providing a controlled and auditable change pipeline. AWX integrates with GLPI to tie job execution to approved change tickets, and pulls all playbooks and scripts from Gitea.

**Gitea** - Serves as the central version control repository for all playbooks, scripts, configuration templates, baseline definitions, and supporting documents. Every change to automation or configuration is committed through Gitea, providing a full audit trail of what was changed, by whom, and when.

**WSUS** - Serves as the approved and cached source for Windows updates within the airgapped network. It enforces an approval workflow ensuring only tested and authorized patches are made available for deployment by AWX.

**APT Repository (TBD)** - All Ubuntu systems pull patches from this internal repository rather than reaching external sources directly, maintaining the airgap while ensuring timely flaw remediation.

**Ubuntu USG** - Performs STIG compliance scanning on Ubuntu systems, validating configuration against the approved security baseline. 

**SolarWinds Observability (FIM)** - Provides continuous file integrity and configuration drift monitoring across both Windows and Linux systems. It detects unauthorized or unexpected changes to critical system files, configurations, and registry keys, alerting on deviations from the approved baseline between scheduled SCAP/GPO compliance cycles.

**SolarWinds SEM** - Receives FIM events and alerts from SolarWinds Observability, correlating file change activity with other security events to distinguish authorized changes from potential unauthorized modifications or indicators of compromise.

**GLPI ITSM** - The change management and IT asset management platform. All configuration changes must have an associated GLPI change ticket before AWX job execution, enforcing a documented approval workflow for every change. GLPI also maintains the ITAM inventory, providing an authoritative record of all hardware and software assets in the environment.

**SolarWinds IPAM** - Manages IP address allocation and DNS/DHCP records across the network, ensuring IP assignments are tracked, authorized, and consistent with the overall configuration baseline.

-----

## NIST CM Controls

|NIST Control                              |Fulfilling Technologies                                            |
|------------------------------------------|-------------------------------------------------------------------|
|**CM-1** – Configuration Management Policy|Document                                                           |
|**CM-2** – Baseline Configuration         |Active Directory GPO, ADSys, AWX, Gitea, Ubuntu USG                |
|**CM-3** – Configuration Change Control   |AWX, GLPI ITSM, Gitea, WSUS                                        |
|**CM-4** – Impact Analyses                |GLPI ITSM                                                          |
|**CM-5** – Access Restrictions for Change |Active Directory GPO, AWX, Gitea                                   |
|**CM-6** – Configuration Settings         |Active Directory GPO, ADSys, Ubuntu USG                            |
|**CM-7** – Least Functionality            |Active Directory GPO, ADSys, AWX                                   |
|**CM-8** – System Component Inventory     |GLPI ITSM, SolarWinds IPAM                                         |
|**CM-9** – Configuration Management Plan  |Document                                                           |
|**CM-10** – Software Usage Restrictions   |GLPI ITSM, Active Directory GPO                                    |
|**CM-11** – User-Installed Software       |Active Directory GPO, ADSys                                        |
|**CM-12** – Information Location          |GLPI ITSM, SolarWinds IPAM                                         |
|**CM-14** – Signed Components             |WSUS, APT Repository (TBD)                                         |