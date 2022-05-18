# Windows-auditing-baseline

## Project purpose
Defining a security audit baseline is a very challenging project, no matter the size of your organization. Indeed, it requires a very good understanding regarding how event logging works, knowledge about the value of hidden or unknown Windows logs and the meaning and impact of each event ID.
In order to address this challenge, *Windows-auditing-baseline* project provides an advanced baseline that can be applied to any Active Directory environment in order to increase visibility on suspicious activities with a minimum of effort.

## Project description
At the following you will find the different auditing steps to configure on your Windows assets. We advised to create 3 group policies (domain controllers, member servers and workstations) for granularity and flexibility purposes. In detail, the following points will be covered:
* **1-Auditing baseline**: configure auditing settings to increase visibility on your assets
* **2-Disabled event logs**: enable disabled but valuable event logs to increase visibility
* **3-Log sizing**: increase log retention to reduce the risk of data being overwritten and not forwarded

## 1-Auditing baseline
The security auditing baseline is defined in the following [document](https://1drv.ms/x/s!Atu5cjCGMw0sk6lz2u_kEgpoFQJZYg?e=KIJti9). It highlights the different subcategories to audit (success and/or failure) together with the related **MITRE TTPs** that it can cover (if applicable). We also recommend to apply additional steps from [Palantir](https://github.com/palantir/windows-event-forwarding/tree/master/group-policy-objects) for PowerShell auditing, command line auditing and WinRM client.

## 2-Disabled event logs
Windows operating system is provided with several event log that, despite of being disabled, can provide valuable information. The table at the following resume these events logs together with the advised action to perform (enable or manual activation).

### Activation
To enable a disabled event log, edit the following registry key using the Group Policy Preferences (GPP) feature on the concerned Group Policy object (DC, SRV, WS):
* *Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Channels\\<Event log name>*
* *Value "Enabled" = 1*

![](/pictures/disabled_eventlog_gpp_registry_activation.png)

### Event logs list
Event log name	| Default status | 	New advised status |	 Host scope	|
|:------------------------ |:------------------:|:------------------:|:----------:|
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
Microsoft-IIS-Configuration/Administrative | Disabled | Disabled (manual activation) | Servers: web (IIS)
Microsoft-IIS-Configuration/Operational | Disabled | Disabled (manual activation) | Servers: web (IIS)
Microsoft-IIS-Logging/Logs | Disabled | Disabled (manual activation) | Servers: web (IIS)
Microsoft-Windows-Base-Filtering-Engine-Connections/Operational | Disabled | Disabled (manual activation) | Servers: VPN
Microsoft-Windows-Base-Filtering-Engine-Resource-Flows/Operational | Disabled | Disabled (manual activation) | Servers: VPN
Microsoft-Windows-WinNat/Oper | Disabled | Disabled (manual activation) | Servers: VPN
Microsoft-Windows-Iphlpsvc | Disabled | Disabled (manual activation) | Servers: VPN
Microsoft-Windows-Dhcp-Client/Operational | Disabled | Enabled | Workstations
Microsoft-Windows-BitLocker/BitLocker Operational | Disabled | Enabled | Workstations

## 3-Log sizing
Windows event logs are per default defined with a very limited size (between 15 et 20 MB). Having such limited size introduce the risk of data being overwritten and not collected in the case of, for example, limited connectivity due to network outage, VPN unreachable … Therefore we advise to increase the size for the following event logs:
* **Security event log**: in the group policy settings (see picture below), go to *Computer > Policies > Admin Templates > Windows Components > Event Log Service > Security  > Specific the maximum log file size (KB)* > 2 GB
* **Application event log**: in the group policy settings (see picture below), go to *Computer > Policies > Admin Templates > Windows Components > Event Log Service > Application  > Specific the maximum log file size (KB)* > 256 MB
* **System event log**: in the group policy settings (see picture below), go to *Computer > Policies > Admin Templates > Windows Components > Event Log Service > System  > Specific the maximum log file size (KB)* > 256 MB
* **Others event logs**: follow the steps explained in step 2 by defining the *MaxSize* (KB) to 128 MB for *Microsoft-Windows-PowerShell/Operational* and *Windows PowerShell* event logs.

![](/pictures/event_log_sizing.png)

# Sources
The following sources were used to elaborate the auditing baseline:
* **Palantir WEF/WEC**: https://github.com/palantir/windows-event-forwarding
* **Event log mindmap**: https://github.com/mdecrevoisier/Microsoft-eventlog-mindmap
* **Notable events**: https://github.com/TonyPhipps/SIEM/blob/master/Notable-Event-IDs.md#microsoft-windows-winrmoperational
* **Event forwarding guidance**: https://github.com/nsacyber/Event-Forwarding-Guidance/blob/master/Events/README.md
* **NSA guidance**: https://apps.nsa.gov/iaarchive/library/ia-guidance/security-configuration/applications/assets/public/upload/Spotting-the-Adversary-with-Windows-Event-Log-Monitoring.pdf
* **Awesome event IDs**: https://github.com/stuhli/awesome-event-ids
* **Forensic goodness**: https://nasbench.medium.com/finding-forensic-goodness-in-obscure-windows-event-logs-60e978ea45a3
* **ANSSI auditing guide**: https://www.ssi.gouv.fr/guide/recommandations-de-securite-pour-la-journalisation-des-systemes-microsoft-windows-en-environnement-active-directory/
* **Audit policy auditing and events**: https://docs.google.com/spreadsheets/d/e/2PACX-1vSD5-83wlU_GwI5Vz4cXhiwZr3QBqCh6VZSAigq8vHakf0UN4DF5SCpKXQm9YdGwIz_rNFBgYoMEIVl/pubhtml
* **Audit policy best practices**: https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations
* **Logging essentials**: https://github.com/JSCU-NL/logging-essentials/blob/main/WindowsEventLogging.adoc
* **Windows 10 event manifest**: https://github.com/repnz/etw-providers-docs/tree/master/Manifests-Win10-17134
