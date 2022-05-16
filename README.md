# Windows-auditing-strategy
Provides an advanced baseline to implement a secure Windows auditing strategy on Windows OS.

## Disabled event log
Event log	| Default status | 	New status   |	 Host scope	|   Comment |
|:----------------------------- |:------------------ |:------------------|:----------|:---------------|
Microsoft-Windows-CAPI2/Operational | Disabled | Enabled | All | 
Microsoft-Windows-LSA/Operational | Disabled | Enabled | All | 
Microsoft-Windows-PrintService/Admin | Disabled | Enabled | All | 
Authentication/ProtectedUser-Client | Disabled | Enabled | All | If protected group feature in use
Authentication/AuthenticationPolicyFailures-DomainController | Disabled | Enabled | DCs | If protected group feature in use
Authentication/ProtectedUserFailures-DomainController | Disabled | Enabled | DCs | If protected group feature in use
Authentication/ProtectedUserSuccesses-DomainController | Disabled | Enabled | DCs | If protected group feature in use
Microsoft-Windows-Crypto-NCrypt/Operational | Disabled | Disabled (manul activation) | All | 
Microsoft-Windows-Base-Filtering-Engine-Connections/Operational | Disabled | Disabled (manul activation) | VPN/RRAS/NPS/AOVPN | 
Microsoft-Windows-Base-Filtering-Engine-Resource-Flows/Operational | Disabled | Disabled (manul activation) | VPN/RRAS/NPS/AOVPN | 
Microsoft-Windows-WinNat/Oper | Disabled | Disabled (manul activation) | VPN/RRAS/NPS/AOVPN | 
Microsoft-Windows-Iphlpsvc | Disabled | Disabled (manul activation) | VPN/RRAS/NPS/AOVPN | 
Microsoft-IIS-Configuration/Administrative | Disabled | Disabled (manul activation) | IIS | 
Microsoft-IIS-Configuration/Operational | Disabled | Disabled (manul activation) | IIS | 
Microsoft-IIS-Logging/Logs | Disabled | Disabled (manul activation) | IIS | 
Microsoft-Windows-DNS-Client/Operational | Disabled | Disabled (manul activation) | Client | Only if lacking of DNS visibility
