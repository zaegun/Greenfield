# Access Controls (AC)
## Constraints 
Microsft Active Directory is to be the primary identity management system (idm).

## System Evaluation List

| Technology                 | Classification  |
|----------------------------|-----------------|
| Windows Event Collector    | WEC             |
| Graylog Sidecar            | Forwarder       |
| Graylog                    | Syslog          |
| SolarWinds SEM             | SIEM            |
| SolarWinds NPM             | Monitoring      |
| PTP/NTP                    | NTP             |

## Software
### Windows Event Collector
Use WEF to send winevents to WEC server via WinRM. Graylog Sidecar used to send forwarded events to Graylog.

### Graylog
Log Indexer used for filtering logs before sending it to the SIEM.

### SolarWinds SEM
SIEM used to generate alerts based on log data.

### SolarWinds NPM
Monitor log systems to ensure log chain is intact.

### PTP/NTP
Ensures accurate time stamps on logs.

## Setup
* Syslogs are sent directly to Graylog server.
    * https://woshub.com/graylog-centralized-log-collection-analysis/
* WEF is used to send winevents to a Windows Event Controller. 
* Graylog Collector Side car is used to forward the winevents to Graylog server.
* Graylog stores and filters logs that it then sends to SolarWinds SEM for alert analysis.
* SolarWinds SEM analyses the logs and generate alerts where applicable.

## Controls
AU-1 - Document
AU-2 - WEF and Graylog
AU-3 - Graylog


