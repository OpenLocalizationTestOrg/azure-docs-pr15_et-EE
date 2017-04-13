<properties
    pageTitle="Azure'i Mitmikautentimise ja 3 pool VPN täpsemalt stsenaariumid"
    description="Sellelt lehelt leiate Azure'i MFA jaoks juhendatud häälestamise konfigureerimise abil 3 tootja KOONDTABEL."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban" 
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-3rd-party-vpn"></a>Azure'i Mitmikautentimise ja 3 pool VPN täpsemalt stsenaariumid
Azure'i Mitmikautentimise saab kasutada mitmesuguseid 3 tootja VPN lahenduste sujuvalt ühendada.  See hõlmab Cisco® ASA VPN seadme, Citrix NetScaler SSL VPN seadme ja seadme kadakas võrkude Secure Access/Pulse'i Secure Connect Secure SSL VPN.

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Cisco ASA VPN seade ja Azure Mitmikautentimise
Azure'i Mitmikautentimise integreerub sujuvalt seadme Cisco® ASA VPN Cisco AnyConnect® VPN sisselogimise ja portaali Accessi täiendavate turvalisuse tagamiseks.  Seda saab teha LDAP või RADIUS protokolli abil.  Valige üks järgmistest üksikasjalikud juhised konfiguratsiooni juhendite allalaadimine.

Seadistamine  | Kirjeldus
------------- | ------------- |
[Cisco ASA LDAP Anyconnect VPN-i ja Azure MFA konfiguratsioon](http://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Sujuv integreerimine Azure MFA kasutamine Lightweight Cisco ASA VPN-seadme|
[Cisco ASA RADIUS Anyconnect VPN-i ja Azure MFA konfiguratsioon](http://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Seadme Cisco ASA VPN sujuvalt integreerimine Azure MFA RADIUS abil

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Citrix NetScaler SSL VPN ja Azure Mitmikautentimise
Azure'i Mitmikautentimise integreerub sujuvalt seadme Citrix NetScaler SSL VPN Citrix NetScaler SSL VPN sisselogimise ja portaali Accessi täiendavate turvalisuse tagamiseks.  Seda saab teha LDAP või RADIUS protokolli abil.  Valige üks järgmistest üksikasjalikud juhised konfiguratsiooni juhendite allalaadimine.

Seadistamine  | Kirjeldus
------------- | ------------- |
[LDAP Citrix NetScaler SSL VPN ja Azure MFA konfigureerimine](http://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Sujuv integreerimine Azure MFA seadme kasutamine Lightweight oma Citrix NetScaler SSL VPN|
[RADIUS Citrix NetScaler SSL VPN ja Azure MFA konfigureerimine](http://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Seadme Citrix NetScaler SSL VPN sujuv integreerimine Azure MFA RADIUS abil

##<a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Kadakas/Pulse'i Secure SSL VPN-seade ja Azure Mitmikautentimise
Azure'i Mitmikautentimise integreerub sujuvalt seadme kadakas/Pulse'i Secure SSL VPN kadakas/Pulse'i Secure SSL VPN sisselogimise ja portaali Accessi täiendavate turvalisuse tagamiseks.  Seda saab teha LDAP või RADIUS protokolli abil.  Valige üks järgmistest üksikasjalikud juhised konfiguratsiooni juhendite allalaadimine.

Seadistamine  | Kirjeldus
------------- | ------------- |
[LDAP kadakas/Pulse'i turvalist SSL VPN ja Azure MFA konfigureerimine](http://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx)| Sujuv integreerimine Azure MFA seadme kasutamine Lightweight oma kadakas/Pulse'i Secure SSL VPN|
[RADIUS kadakas/Pulse'i turvalist SSL VPN ja Azure MFA konfigureerimine](http://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Seadme kadakas/Pulse'i Secure SSL VPN sujuvalt integreerimine Azure MFA RADIUS abil
