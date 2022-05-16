# Windows-auditing-strategy
Provides an advanced baseline to implement a secure Windows auditing strategy on Windows OS.
* Configure security log auditing via the advanced audit policy configuration (+ use modern approach)
* Configure other event event log and enable disable ones
* Configure log sizing for retention

## Security event log


## Others event log


## Disabled event log
The following table resume valuable Windows event logs that are per default disabled. 
Event log	| Default status | 	New status   |	 Host scope	|
|:------------------------ |:------------------|:------------------|:----------|
Microsoft-Windows-Crypto-NCrypt/Operational | Disabled | Disabled (manual activation) | All
Microsoft-Windows-CAPI2/Operational | Disabled | Enabled | All
Microsoft-Windows-LSA/Operational | Disabled | Enabled | All
Microsoft-Windows-PrintService/Admin | Disabled | Enabled | All
Authentication/ProtectedUser-Client | Disabled | Enabled | All
Microsoft-Windows-PrintService/Operational | Disabled | Enabled | All
Microsoft-Windows-DriverFrameworks-UserMode/Operational | Disabled | Enabled | All
Microsoft-Windows-DNS-Client/Operational | Disabled | Disabled (manual activation) | Workstations
Authentication/AuthenticationPolicyFailures-DomainController | Disabled | Enabled | Domain controllers
Authentication/ProtectedUserFailures-DomainController | Disabled | Enabled | Domain controllers
Authentication/ProtectedUserSuccesses-DomainController | Disabled | Enabled | Domain controllers
Microsoft-IIS-Configuration/Administrative | Disabled | Disabled (manual activation) | Severs: web (IIS)
Microsoft-IIS-Configuration/Operational | Disabled | Disabled (manual activation) | Severs: web (IIS)
Microsoft-IIS-Logging/Logs | Disabled | Disabled (manual activation) | Severs: web (IIS)
Microsoft-Windows-Base-Filtering-Engine-Connections/Operational | Disabled | Disabled (manual activation) | Servers: VPN
Microsoft-Windows-Base-Filtering-Engine-Resource-Flows/Operational | Disabled | Disabled (manual activation) | Servers: VPN
Microsoft-Windows-WinNat/Oper | Disabled | Disabled (manual activation) | Servers: VPN
Microsoft-Windows-Iphlpsvc | Disabled | Disabled (manual activation) | Servers: VPN
Microsoft-Windows-Dhcp-Client/Operational | Disabled | Enabled | Workstations
Microsoft-Windows-BitLocker/BitLocker Operational | Disabled | Enabled | Workstations


# Sources
The following sources were used to elaborate the auditing strategy:
* **Notable events**: https://github.com/TonyPhipps/SIEM/blob/master/Notable-Event-IDs.md#microsoft-windows-winrmoperational
* **Event forwarding guidance**: https://github.com/nsacyber/Event-Forwarding-Guidance/blob/master/Events/README.md
* **NSA guidance**: https://apps.nsa.gov/iaarchive/library/ia-guidance/security-configuration/applications/assets/public/upload/Spotting-the-Adversary-with-Windows-Event-Log-Monitoring.pdf
* **Awesome event IDs**: https://github.com/stuhli/awesome-event-ids
* **Forensic goodness**: https://nasbench.medium.com/finding-forensic-goodness-in-obscure-windows-event-logs-60e978ea45a3
* **ANSSI**: https://www.ssi.gouv.fr/guide/recommandations-de-securite-pour-la-journalisation-des-systemes-microsoft-windows-en-environnement-active-directory/
* **Audit policy auditing and events**: https://docs.google.com/spreadsheets/d/e/2PACX-1vSD5-83wlU_GwI5Vz4cXhiwZr3QBqCh6VZSAigq8vHakf0UN4DF5SCpKXQm9YdGwIz_rNFBgYoMEIVl/pubhtml
* **Audit policy best practices**: https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations
* **Logging essentials**: https://github.com/JSCU-NL/logging-essentials/blob/main/WindowsEventLogging.adoc
* **Windows 10 event manifest**: https://github.com/repnz/etw-providers-docs/tree/master/Manifests-Win10-17134
