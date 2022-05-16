# Windows-auditing-strategy
Provides an advanced baseline to implement a secure Windows auditing strategy on Windows OS.


# Event log	| Default status | 	New status   |	 Host scope	|   Comment
|:-----------------------------|:------------------|:------------------|:----------|:---------------|
Microsoft-Windows-CAPI2/Operational | Disabled | All | 
Microsoft-Windows-LSA/Operational | Disabled | All | 
Microsoft-Windows-PrintService/Admin | Disabled | All | 
Authentication/ProtectedUser-Client | Disabled | All | If protected group feature in use
Authentication/AuthenticationPolicyFailures-DomainController | Disabled | DCs | If protected group feature in use
Authentication/ProtectedUserFailures-DomainController | Disabled | DCs | If protected group feature in use
Authentication/ProtectedUserSuccesses-DomainController | Disabled | DCs | If protected group feature in use
Microsoft-Windows-Base-Filtering-Engine-Connections/Operational | Disabled | VPN/RRAS/NPS/AOVPN | 
Microsoft-Windows-Base-Filtering-Engine-Resource-Flows/Operational | Disabled | VPN/RRAS/NPS/AOVPN | 
Microsoft-Windows-WinNat/Oper | Disabled | VPN/RRAS/NPS/AOVPN | 
Microsoft-Windows-Iphlpsvc | Disabled | VPN/RRAS/NPS/AOVPN | 
Microsoft-IIS-Configuration/Administrative | Disabled | IIS | 
Microsoft-IIS-Configuration/Operational | Disabled | IIS | 
Microsoft-IIS-Logging/Logs | Disabled | IIS | 
Microsoft-Windows-Crypto-NCrypt/Operational | Disabled | IIS | 
Microsoft-Windows-DNS-Client/Operational | Disabled | Client | Only if lacking of DNS visibility
