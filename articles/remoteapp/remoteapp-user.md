<properties
    pageTitle="Kasutaja lisamine oma Azure RemoteApp saidikogumi | Microsoft Azure'i"
    description="Saate teada, kuidas oma Azure RemoteApp saidikogumi kasutajate lisamine"
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

# <a name="how-to-add-a-user-to-your-azure-remoteapp-collection"></a>Kuidas lisada kasutaja oma saidikogumi Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Enne teie kasutajad näevad ja kasutada rakendusi Azure RemoteApp, on teil seda anda juurdepääsu oma saidikogumi. See on lihtne: sisestage menüü **Kasutajajuurdepääsu** kontoteabe kasutaja ja klõpsake märkeruutu.

Kontoteave vajate? See sõltub saidikogumi tüüpi loodud (pilve või hübriid) ja kas te kasutate teenusekomplekti Office 365 ProPlus selle saidikogumi.

## <a name="supported-user-identities"></a>Toetatud kasutaja identiteet

Saidikogumi eri tüüpi (pilve vs hübriid) toetavad erinevate kasutajate identiteete kasutamise rakendustele juurdepääsemiseks.  

Hübriidjuurutuse saidikogumi RemoteApp, peate häälestada infrastruktuur Active Directory domeeni ja Azure Active Directory rentnikku kataloogi integreerimise tööruumides (ja soovi korral ühekordse sisselogimise). Lisaks peate looma mõned Active Directory objekti kohapealse kataloogi.  

Pilveteenuse kogumiseks RemoteApp iga kasutaja kohta, mis on Azure Active Directory toetavad identiteedid saate anda kasutajajuurdepääsu RemoteApp Microsofti Accounts kaasata.  Vt alltoodud tabelit.

Office 365 kasutajad on Azure Active Directory kasutajad. Kui nad on Azure Active Directory hübriid, Directory sünkroonitud kontode need saab anda RemoteApp hübriidjuurutuse kasutajate juurdepääsu.   

Saate kasutada selle tabeli jaoks, mis toetavad identiteedi oma saidikogumi ja milline Active Directory nõuded on kiirülevaade.

|Kasutajakontode |Pilveteenuse   |Hübriid|
|--------------|--------|------|
|Microsofti konto|     Jah|    Ei|
|Azure Active Directory (Azure AD)| | |
|Azure'i AD pilveteenuses    |Jah    |Ei |
|ADsync parooli sünkroonimise abil  |Jah    |Jah    |
|ADsync ilma parooli sünkroonimise|  Jah |Ei |
|ADsync AD FS-i abil  |Jah    |Jah    |
|[3-osaline Azure toetatud identiteedipakkujad](https://msdn.microsoft.com/library/azure/jj679342.aspx)  (nt Ping) |Jah    |Jah|
|Mitmikautentimise    |Jah    |Jah    |

Vaadake [Lisateavet](remoteapp-ad.md) Active Directory jaoks RemoteApp konfigureerimise kohta.


> [AZURE.NOTE] Azure Active Directory kasutajad peavad olema rentnikust, mis on teie tellimusega seotud. (Saate vaadata ja muuta oma tellimuse portaalis vahekaardil **sätted** . Lisateavet [muutmine Azure Active Directory rentniku kasutatavaid RemoteApp](remoteapp-changetenant.md) .)

## <a name="office-365-proplus-user-account-information"></a>Office 365 ProPlusi kasutaja teabe
Kui kasutate teenusekomplekti Office 365 ProPlus malli pilti oma saidikogumi *või* kui olete loonud kohandatud pilt, mis kasutab teenusekomplekti Office 365, on lubatud ainult lisada Azure Active Directory kasutajatele, kellel on Office 365 tellimused oma tellimuse jaoks vaikedomeeni. Lisateavet leiate [Azure'i RemoteAppi abil Office 365](remoteapp-o365.md) .
