<properties
    pageTitle="Azure'i AD-ühenduse: Pordid | Microsoft Azure'i"
    description="Sellel lehel on portide, mida on vaja avatud Azure'i AD-ühenduse tehniline teave leht"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-required-ports-and-protocols"></a>Hübriid identiteedi nõutav pordid ja protokollid
Järgmine dokument on vajalikud pordid ja protokollid, mis on vajalikud hübriid identiteedi lahenduse kohta teavet tehnilise viide. Kasutage järgmisel joonisel ja vastava tabeli viidata.

![Mis on Azure AD-ühenduse](./media/active-directory-aadconnect-ports/required1.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Tabel 1 - Azure AD ühenduse loomiseks ja kohapealse AD
Selles tabelis kirjeldatakse pordid ja protokollid, mis on vajalikud Azure'i AD-ühenduse server suhtlemine ja kohapealse AD.

Protocol (protokoll) | Pordid | Kirjeldus
--------- | --------- |---------
DNS-I|53 (TCP/UDP)| DNS-i otsingud sihtkoha mets kohta.
Kerberose|88 (TCP/UDP)| Kerberos-autentimine AD mets.
MS-RPC |135 (TCP/UDP)| Kui see seob AD mets esialgne konfiguratsioon Azure'i AD-ühenduse viisardi ajal kasutada.
LDAP|389 (TCP/UDP)| Andmete importimine AD kasutada. Andmed on krüptitud Kerberos Logi ja Seal.
LDAP SSL-I|636 (TCP/UDP)| Andmete importimine AD kasutada. Andmeedastus on allkirjastatud ja krüptitud. Kasutada ainult siis, kui kasutate SSL-i.
RPC |49152 – 65535 (juhuslik kõrge RPC Port)(TCP/UDP)| Kui see seob AD metsadele algse Azure'i AD-ühenduse konfiguratsiooni ajal kasutada. Lisateabe saamiseks vaadake [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017)ja [KB224196](https://support.microsoft.com/kb/224196) .

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Tabel 2: Azure'i AD-ühenduse ja Azure AD
Selles tabelis kirjeldatakse pordid ja protokollid suhtlemine Azure'i AD-ühenduse server ja Azure AD jaoks vajalike.

Protocol (protokoll) |Pordid |Kirjeldus
--------- | --------- |---------
HTTP KAUDU|80 (TCP/UDP)| Laadige alla CRL-ID (serdi tühistamise loendeid) kinnitamaks, et SSL-sertide kasutada.
HTTPS|443(TCP/UDP)| Azure AD sünkroonimiseks kasutada.

Loendi URL-id ja IP-aadressid, peate avama tulemüüri, lugege teemat [Office 365 URL-id ja IP-aadresside vahemikud](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-federation-serverswap"></a>Tabel 3 – Azure AD ühenduse ja Federation serverid/WAP
Selles tabelis kirjeldatakse pordid ja protokollid suhtlemine Azure'i AD-ühenduse server ja Federation/WAP serverite jaoks vajalike.  

Protocol (protokoll) |Pordid |Kirjeldus
--------- | --------- |---------
HTTP KAUDU|80 (TCP/UDP)| Laadige alla CRL-ID (serdi Tühistusloendid) kinnitamaks, et SSL-sertide kasutada.
HTTPS|443(TCP/UDP)| Azure AD sünkroonimiseks kasutada.
WinRM|5985| WinRM kuulajale

## <a name="table-4---wap-and-federation-servers"></a>Tabel 4 - WAP ja Liiduserverite
Selles tabelis kirjeldatakse pordid ja protokollid vajalike liiduserverite ja WAP serverid vahel.

Protocol (protokoll) |Pordid |Kirjeldus
--------- | --------- |---------
HTTPS|443(TCP/UDP)| Kasutada autentimiseks.

## <a name="table-5---wap-and-users"></a>Tabel 5 - WAP ja kasutajad
Selles tabelis kirjeldatakse pordid ja protokollid, side kasutajate ja WAP serverid jaoks vajalike.

Protocol (protokoll) |Pordid |Kirjeldus
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| Kasutada seadme autentimiseks.
TCP|49443 (TCP)| Kasutada serdi autentimist.

## <a name="table-6a--6b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabeli 6a ja 6b - Azure'i AD-ühenduse seisund agent jaoks (AD FS-i/Sync) ja Azure AD
Järgmises tabelis on kirjeldatud lõpp-punktid, pordid ja protokollid suhtlemine Azure'i AD-ühenduse seisund agentide ja Azure AD jaoks vajalike

### <a name="table-6a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tabeli 6a - pordid ja protokollid Azure'i AD-ühenduse seisund agent jaoks (AD FS-i/Sync) ja Azure AD
Selles tabelis kirjeldatakse väljaminev järgmised pordid ja protokollid suhtlemine Azure'i AD-ühenduse seisund agentide ja Azure AD jaoks vajalike.  

Protocol (protokoll) |Pordid  |Kirjeldus
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| Väljamineva meili
Azure'i teenus siini|5671 (TCP/UDP)| Väljamineva meili

### <a name="6b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>6b - lõpp-punktide jaoks Azure'i AD-ühenduse seisund agent jaoks (AD FS-i/Sync) ja Azure AD
Lõpp-punktid loendi leiate [Azure'i AD-ühenduse tervise agent](active-directory-aadconnect-health-agent-install.md#requirements)jaotisest nõuded.
