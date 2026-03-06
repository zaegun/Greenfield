# Logging Architecture – NIST AU Controls

-----

## Technologies

|Technology                        |Role                        |
|----------------------------------|----------------------------|
|**Rsyslog**                       |Syslog Forwarder            |
|**Windows Event Forwarding (WEF)**|WinEvent Collector Transport|
|**Sysmon (Enhanced)**             |WinEvent Enrichment         |
|**Windows Event Collector (WEC)** |WinEvent Aggregator         |
|**Graylog Sidecar**               |Agent / Collector           |
|**Graylog**                       |Log Indexer / SIEM          |
|**SolarWinds SEM**                |Alert Analyzer              |

-----

## Analysis

**Rsyslog** - Acts as the first leg of the syslog pipeline. Collects sylogs and then forwards them directly to Graylog over a structured transport (UDP/TCP syslog).

**Sysmon (Enhanced)** - Augments native Windows event logging by generating detailed telemetry on process creation, network connections, and file changes.

**Windows Event Forwarding (WEF)** - Leverages the native Windows subscription mechanism to push those enriched Sysmon events and standard Windows security events from endpoints to a central collection point without requiring a third-party agent on every machine.

**Windows Event Collector (WEC)** - The receiving server in the WEF pipeline. It collects all subscribed WinEvent data from across the environment into a single point.

**Graylog Sidecar** - Deployed on or alongside the WEC server and acts as the bridge between the Windows collector and Graylog. It ships the collected WinEvents into Graylog.

**Graylog** - Serves as the central indexer and correlation engine for both pipelines. All syslog data (via Rsyslog) and all WinEvent data (via Sidecar from WEC) land here. Graylog normalizes, stores, and correlates logs across both sources. Designated servers are forwarded to the SEM.

**SolarWinds SEM** - Sits downstream of Graylog and receives only critical, high-priority alerts forwarded by Graylog. SEM provides dedicated security event analysis, alerting, and compliance reporting for escalated events.

-----

## NIST AU Controls

|NIST Control                                         |Fulfilling Technologies                   |
|-----------------------------------------------------|------------------------------------------|
|**AU-1** – Audit & Accountability Policy             |Document                                  |
|**AU-2** – Event Logging                             |Sysmon, WEF, Rsyslog, Graylog             |
|**AU-3** – Content of Audit Records                  |Sysmon, Rsyslog, Graylog                  |
|**AU-4** – Audit Log Storage Capacity                |Graylog                                   |
|**AU-5** – Response to Audit Logging Process Failures|Graylog, SolarWinds SEM                   |
|**AU-6** – Audit Record Review, Analysis & Reporting |Graylog, SolarWinds SEM                   |
|**AU-7** – Audit Record Reduction & Report Generation|Graylog, SolarWinds SEM                   |
|**AU-8** – Time Stamps                               |Sysmon, Rsyslog, WEF, Graylog             |
|**AU-9** – Protection of Audit Information           |Graylog                                   |
|**AU-10** – Non-repudiation                          |                                          |
|**AU-11** – Audit Record Retention                   |Graylog                                   |
|**AU-12** – Audit Record Generation                  |Sysmon, Rsyslog, WEF, WEC, Graylog Sidecar|

