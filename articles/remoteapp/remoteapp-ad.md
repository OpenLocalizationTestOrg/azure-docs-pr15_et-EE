
<properties 
    pageTitle="Azure AD + Azure RemoteApp Active Directory nõuded | Microsoft Azure'i" 
    description="Saate teada, kuidas häälestada töötama RemoteAppi Azure Active Directory." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure AD + Azure RemoteApp Active Directory nõuded

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .


Azure RemoteApp hübriid kogumiseks või pilveteenuse kogumi, mida soovite liita abil AD-ühendus, peate tehke järgmist.

### <a name="connect-azure-ad-and-active-directory"></a>Ühenduse loomine Azure AD ja Active Directory

Kui soovite luua ühenduse oma Azure AD rentniku ja teie kohapealse Active Directory keskkonnas, kasutage AD-ühendus. Kulub teil ainult [4 klõpsuga](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) kahe kataloogi ühenduse.

Pange tähele - kataloogi sünkroonimine on nõutav hübriid saidikogumid.

### <a name="make-sure-your-domaincom-match"></a>Veenduge, et teie "@domain.com" vastavad
Enne alustamist, veenduge, et oma kohapealse mets UPN-i vastab Azure AD domeeni järelliide. 

Kui olete häälestanud UPN-i domeeni sufiks Azure AD, Azure RemoteApp sisse logida kõik kasutajad saavad Logi sisse, kui “user@ <the suffix you set up>". Veenduge, et kasutajad saavad ka logida sama user@suffix kohapealse domeeni. Teatud juhtudel saate häälestada domeeni selle kasutaja kohta – prem. domeenil järelliite täpsustades Azure AD Sel juhul kasutajaid ei saa te ühendamiseks mis tahes domeeni ühendatud arvutites või Azure RemoteApp abil.

Näiteks kui häälestate oma UPN-i domeeni järelliite AAD nimega contoso.com, kuid mõned ruumide/AD kasutajad on konfigureeritud sisse logida @contoso.uk, siis need kasutajad ei saa sisse logida õigesti ARA saidikogumi. Kasutaja UPN-i AAD ja AD peab olema sama login võimalik"

### <a name="create-objects-for-azure-remoteapp"></a>Objektide jaoks Azure RemoteApp loomine
Samuti peate looma järgmised asutusesisese Active Directory objekti:

- Teenusekonto anda juurdepääsu domeeni ressursse RemoteAppi programmid liitumist kohapealse domeeni RDSH lõpp-punktid.
- Ettevõtte üksus (Organisatsiooniüksusega) RemoteApp seadme objektide sisaldama. Kasuta OU on soovitatav (kuid ei pea) eristamiseks kontode ja kasutate RemoteAppi poliitika.

Peate mõlemad need objektid, kui loote oma RemoteApp kogum, et olla kindel, et tehke esmalt järgmist.