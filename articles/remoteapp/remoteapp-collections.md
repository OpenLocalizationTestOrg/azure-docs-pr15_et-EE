<properties 
    pageTitle="Millist liiki saidikogumi jaoks Azure RemoteApp vajate? | Microsoft Azure'i" 
    description="Saidikogumite saadaval Azure RemoteAppi tüüpi teavet." 
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



# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Millist liiki saidikogumi jaoks Azure RemoteApp vajate?

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp võimaldab teil rakendused ja ressursside ühiskasutusse kasutajatele, mis tahes seadmes. Oleme selleks luua saidikogumid korraldada need rakendused ja ressursid ja seejärel ühiskasutus nende saidikogumite kasutajatega. On 2 eri saidikogumi võimalust, erinevate võrgu ja autentimise suvandid – mis on õige?

Vaatame erinevate asjaoluga ja Valikud, mida peate tegema, et kõige paremini teie Azure RemoteApp saidikogumi. 


## <a name="quick-differences-between-the-collection-types"></a>Kiirülevaate erinevused saidikogumi tüübid

|           | Pilveteenuse | Hübriid |
|-----------|-------|--------|
|Mõne olemasoleva VNET kasutamine| Jah| Jah|
|Nõuab AD ühendatud kontod (DirSync)| Ei| Jah|
|Nõuab Domeeniga liitumine| Ei| Jah|
|Nõuab domeenikontrolleri kättesaadavaks VNET| Ei| Jah|

## <a name="cloud-collections"></a>Pilveteenuse saidikogumid
- Kiire loomine – selle saidikogumi kiiresti ettevalmistatud, s.t rakenduste kasutajatele kiiremini kätte.
- Tuua oma rakendused ja meie ühiskasutusse anda. Saate kohandatud pildi (ehitatud on Azure VM) või üks teie tellimuses sisalduv piltide.
- Te ei pea oma saidikogumi ja kohapealse domeeni vahelise ühenduse konfigureerimine.
- Kuid soovi korral saate oma Azure'i VNET juurdepääsu andmiseks kohapealse keskkonna andmete ühiskasutuse sisse või -Windowsi autentimist kasutades ressursse nagu SQL serveri (andmebaasi autentimist kasutades).


OK, kuidas luua üks?

- Üksnes pilveteenuses? Suvandiga **Kiiresti luua** portaali loomine.
- Pilveteenuse + VNET? Luua, **koos VNET loomine** funktsiooni abil, kuid ei vali mõne domeeniga.

## <a name="hybrid-collections"></a>Hübriidjuurutuse saidikogumid
- Sisestage kohapealse võrgu + Azure VNET täielik juurdepääs.
- Domeeni Liitu Accessi rakenduste ja andmete sisaldab. Serveri rakendused saate esitada oma kohapealse Active Directory autentimine – need pääseb teie domeeni ressursid.
- Täiustatud jälgimise ja halduse olemasoleva System Center lahendusi ja Windowsi rühmapoliitikat (kaudu Windows Server 2012 R2 sisseehitatud kohandatud pildi) lubamine
- Toeta [ExpressRoute](https://azure.microsoft.com/services/expressroute/) oma Azure'i VNET ühenduse loomiseks oma kohaliku VNET.

Käsk **Loo VNET abil** luua ja valida mõne domeeniga.

## <a name="authentication-options"></a>Autentimise suvandid
Azure RemoteApp toetab nii Microsofti kontod ja Azure Active Directory kontod, kuid mitte kõigi saidikogumite toetavad kõik meetodid. 

| Konto tüüp                      |                                                             | Pilveteenuse | Pilveteenuse + VNET | Hübriid |
|-----------------------------------|-------------------------------------------------------------|-------|--------------|--------|
| Microsofti konto                 |                                                             | Jah   | Jah          | Ei     |
| Azure Active Directory (Azure AD) |                                                             |       |              |        |
|                                   | Ainult Azure AD                                               | Jah   | Jah          | Ei     |
|                                   | AD-ühendus parooli sünkroonimise                               | Jah   | Jah          | Jah    |
|                                   | Parooli sünkroonimise ilma AD-ühendus                            | Jah   | Jah          | Ei     |
|                                   | AD-ühenduse AD FS-i abil                                       | Jah   | Jah          | Jah    |
|                                   | 3-osaline Azure toetatud identiteedipakkujad (nt Ping) | Jah   | Jah          | Jah    |
| Mitmikautentimise       |                                                             | Jah   | Jah          | Jah    |



### <a name="cloud-and-cloud--vnet"></a>Pilveteenuse ja pilveteenuse + VNET 
Pilveteenuse saidikogumid, saate Microsofti kontod, Azure AD kontod või kahe kombinatsioon. Kasutage kõige paremini kasutajate kontod.

On teatud nõuded kasutades Microsofti kontot. 

Kui soovite kasutada Azure AD kontod, peate veenduge, et teie Azure AD rentniku vastab üks teie tellimusega seostatud. Kui olete loonud tellimuse Azure RemoteApp, Azure AD rentniku, mida kasutasite oli automaatselt teie tellimusega seostatud. Mis tahes juurdepääsuõiguse anda Azure AD kasutaja peab olema selle samal rentnikukontol. Vajadusel saate [muuta Azure AD rentniku](remoteapp-changetenant.md) teie tellimusega seostatud.
 
### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hübriid (või pilveteenuse + Azure AD + AD)

Kasutades Azure AD + kohapealse Active Directory on nõutav hübriid saidikogumi. Peate kasutama ühendada kaks kataloogide AD-ühendus. Kuid on mõned valik, kui tegemist on konfigureerimine AD-ühendus. 

2 AD on ühendus stsenaariumid - parooli sünkroonimise kasutamine või AD federation abil. Vaadake [AD-ühendus teabe](../active-directory/active-directory-aadconnect.md) aru saada, mis neid töötab teie jaoks sobivaim.

Saate kasutada ka Azure AD + AD kogumik pilve. Veenduge, et järgite samad juhised häälestamine.

Tutvuge [Azure AD + Azure RemoteApp Active Directory nõuded](remoteapp-ad.md) jaoks konfigureerida Azure AD etapid ja Active Directory.

## <a name="go-create-your-collection"></a>Avage oma saidikogumi loomine!
OK, ma arvates me olete otsustanud seda kohe, seega ei vasakule teha-oma esimese Azure RemoteApp saidikogumi loomine ainult ühe asja.

[Pilveteenuse saidikogumi loomine](remoteapp-create-cloud-deployment.md) või [luua hübriid](remoteapp-create-hybrid-deployment.md) - lihtsalt saada luua.
