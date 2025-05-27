# Windows-auditing-baseline

## Project goal
Defining a security audit baseline is a very challenging project, no matter the size of your organization. Indeed, it requires a very good understanding about event logging, knowledge about the value of each event, TTPs event relation and logging activation impact awareness. In order to address this challenge, *Windows-auditing-baseline* project was created. It provides a complete end-to-end toolset that can be applied to any Active Directory environment in order to enable advanced threat detection capacities with a minimum of effort.

## Activation steps overview
At the following you will find the different steps to configure in your environment. We advised to create 3 group policies (domain controllers, member servers and workstations) for security, granularity and flexibility purposes. In detail, the following steps will be covered:
* **0-Event log mindmap overview**: we advise to have a look on the different Windows [mindmaps](https://github.com/mdecrevoisier/Microsoft-eventlog-mindmap) to ensure correct understanding of the coverage.
* **1-Auditing baseline**: overview of configured auditing settings in the group policy templates. It also provides a 1 to 1 relation between each event ID and the MITRE ATT&CK framework TTPs.
* **2-Group policy templates**: pre-configured group policy objects ready for direct import in your group policy console. 
* **2.1-Disabled event logs** *(already covered in point 2, manual configuration steps at the following)*: enable disabled but valuable event logs to increase visibility.
* **2.2-Log sizing** *(already covered in point 2, manual configuration steps at the following)*: increase log retention to reduce the risk of data being overwritten and not forwarded. At the same time, use the caching feature of your log forwarding agent (if available) to avoid data loss.
* **3-Agent configuration**: provide a configuration file for the Windows Splunk Universal Forwarder which collects the required event IDs from the >70 different event log channels mentionned in the points 0 and 1.

## 1-Auditing baseline
The security auditing baseline is defined in the following [document](/files/Windows_auditing_guidelinesg_GPO.xlsx). It highlights the different subcategories to audit (success and/or failure) together with the related **MITRE TTPs** that it can cover (if applicable). We recommend to evaluate your internal auditing  requirements and to adjust the group policy templates accordingly. We also recommend to apply additional steps from [Palantir](https://github.com/palantir/windows-event-forwarding/tree/master/group-policy-objects) for PowerShell auditing, command line auditing and WinRM client.
![](/pictures/event_id_per_category.png)
![](/pictures/event_id_per_ttp.png)

## 2-Group policy templates
*[WORK IN PROGRESS]*

## 2.1-Disabled event logs
*This step is already performed in the group policy templates*. Windows operating system is provided with several event logs that, despite of being disabled, can provide valuable information. The table at the following resumes these events logs together with the advised action to perform.

### Activation
To enable a disabled event log, edit the following registry key using the Group Policy Preferences (GPP) feature on the concerned Group Policy object (DC, SRV, WS) for the event log mentioned at the following:
* *Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Channels\\<Event log name>*
* *Value "Enabled" = 1*

![](/pictures/disabled_eventlog_gpp_registry_activation.png)

### Event logs list
Event log name	| Category | TTP ID | TTP name
|:------------------------ |:------------------:|:------------------:|:----------:|
Microsoft-Windows-Authentication/AuthenticationPolicyFailures-DomainController | Authentication | T1110 | Brutforce
Microsoft-Windows-Authentication/ProtectedUser-Client | Authentication | T1078 | Valid accounts
Microsoft-Windows-Authentication/ProtectedUserFailures-DomainController | Authentication | T1110 | Brutforce
Microsoft-Windows-Authentication/ProtectedUserSuccesses-DomainController | Authentication | T1558 | Steal or Forge Kerberos Tickets 
Microsoft-Windows-CAPI2/Operational | Crypto | T1552.004 | Unsecured Credentials-Private Keys
Microsoft-Windows-Crypto-NCrypt/Operational | Crypto |  | 
Microsoft-Windows-Dhcp-Client/Operational | DHCP client |  | 
DhcpAdminEvents | DHCP server |  | 
Microsoft-Windows-DhcpNap/Operational | DHCP server |  | 
Microsoft-Windows-DriverFrameworks-UserMode/Operational | Drivers | T1091 | Replication Through Removable Media 
Microsoft-Windows-PrintService/Operational | Printer | T1574.002 | Hijack Execution Flow: DLL Side-Loading 
Microsoft-Windows-Base-Filtering-Engine-Connections/Operational | VPN server |  | 
Microsoft-Windows-Base-Filtering-Engine-Resource-Flows/Operational | VPN server |  | 
Microsoft-Windows-Iphlpsvc | VPN server |  | 
Microsoft-Windows-WinNat/Oper | VPN server |  | 
Microsoft-IIS-Configuration/Administrative | Web server |  | 
Microsoft-IIS-Configuration/Operational | Web server |  | 

## 2.2-Log sizing
*This step is already performed in the group policy templates*. Windows event logs are per default defined with a very limited size (between 15 and 20 MB). Having such limited size introduce the risk of data being overwritten and not collected in the case of, for example, limited connectivity due to network outage, VPN unreachable … Therefore we advise to increase the size for the following event logs:
* **Security event log**: in the group policy settings (see picture below), go to *Computer > Policies > Admin Templates > Windows Components > Event Log Service > Security  > Specific the maximum log file size (KB)* > 2 GB
* **Application event log**: in the group policy settings (see picture below), go to *Computer > Policies > Admin Templates > Windows Components > Event Log Service > Application  > Specific the maximum log file size (KB)* > 256 MB
* **System event log**: in the group policy settings (see picture below), go to *Computer > Policies > Admin Templates > Windows Components > Event Log Service > System  > Specific the maximum log file size (KB)* > 256 MB
* **Others event logs**: follow the steps explained in step 2 by defining the *MaxSize* (KB) to 128 MB for *Microsoft-Windows-PowerShell/Operational* and *Windows PowerShell* event logs.

![](/pictures/event_log_sizing.png)

## 3. Agent configuration
Once the auditing baseline is in place and proper events and/or channels are activated, it may be necessary to configure your agent or agent-less solution to forward logs to a SIEM or a central collector. At the following you will find two configuration templates in order to enable log forwarding to the choosen destination:
* **Agent based using the Splunk UF agent**: refers to my *Splunk input windows baseline* [project](https://github.com/mdecrevoisier/Splunk-input-windows-baseline) which defines the different Event IDs and Channel to read.
* **Agentless based using Windows Event Forwarding (WEF)**: refers to my *WEC server auto deploy* [project](https://github.com/mdecrevoisier/Windows-WEC-server_auto-deploy) in order to configure the WEC server together with preconfigured XML subscriptions (subscriptions contains the Event IDs and Channels to read).


# Sources
The following sources were used to elaborate the auditing baseline:
* **Splunk audit policy guide** : https://www.splunk.com/en_us/blog/security/windows-audit-policy-guide.html
* **Event log mindmap**: https://github.com/mdecrevoisier/Microsoft-eventlog-mindmap
* **Yamato Windows log settings**: https://github.com/Yamato-Security/EnableWindowsLogSettings
* **Palantir WEF/WEC**: https://github.com/palantir/windows-event-forwarding
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
* **Windows event ID mapping**: https://github.com/JSCU-NL/logging-essentials/blob/main/WindowsEventIDMapping.json
* **Windows events auditing per subcategory**: https://girl-germs.com/?p=363
* **Joint Sigint Cyber Unit logging essential**: https://github.com/JSCU-NL/logging-essentials/blob/main/WindowsEventLogging.adoc#account-activity
* **Windows 10 & Windows 11 changes in eventlog**: https://github.com/AndrewRathbun/SANSGoldPaperResearch_FOR500_Rathbun/tree/main/EventLogs 
* **MITRE Sensor mapping**: https://center-for-threat-informed-defense.github.io/sensor-mappings-to-attack/levels/mapping_winevtx/