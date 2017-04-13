<properties
    pageTitle="Kohapealse Azure Active Directory seadme registreerimine kaudu juurdepääsu häälestamine | Microsoft Azure'i"
    description="Samm-sammult juhendi kohapealse rakenduste Active Directory Federation teenust (AD FS) kasutamine Windows Server 2012 R2 tingimusvormingu juurdepääsu lubamiseks."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>


# <a name="setting-up-on-premises-conditional-access-using-azure-active-directory-device-registration"></a>Kuidas häälestada kohapealse juurdepääsu abil Azure Active Directory seadme registreerimine

Isiklikult kuuluv seadmete kasutajad võib tähistada tuntud teie organisatsioonile, et töökohal Liitu kasutajad saavad seadmed Azure Active Directory seadme registreerimine teenusega. Allpool on samm-sammult juhendi tingimusvormingu juurdepääsu kohapealse rakenduste kasutamine Windows Server 2012 R2 Active Directory Federation teenust (AD FS).

> [AZURE.NOTE]
> Office 365 litsentsi või Azure AD Premium litsentsi on seadmete registreeritud Azure Active Directory seadme registreerimine teenuse tingimusvormingu juurdepääsupoliitikaid nõutav. See hõlmab poliitikast ressurssidele kohapealse Active Directory Federation Services (AD FS).

Kohapealse juurdepääsu stsenaariumid kohta leiate lisateavet teemast [töökoha SSO-d ja sujuvalt teise tegur autentimist üle ettevõtte rakenduste mis tahes seadme kaudu liituda](https://technet.microsoft.com/library/dn280945.aspx).

Need funktsioonid on saadaval klientidele, kes ostavad on Azure Active Directory Premium litsents.

<a name="supported-devices"></a>Toetatud seadmed
-------------------------------------------------------------------------
* Windows 7 domeeni ühendatud seadmete.
* Windows 8.1 isiklik ja domeeni liidetud seadmed.
* iOS 6 ja uuemad versioonid, Safari brauseri jaoks
* Android 4.0 või uuem versioon, Samsung GS3 või kohal Samsung Lisa2 telefonides või tahvelarvutites kohal.


<a name="scenario-prerequisites"></a>Stsenaarium eeltingimused
------------------------------------------------------------------------
* Teenusekomplekti Office 365 tellimuse või Azure Active Directory Premium
* Azure Active Directory rentnikku
* Windows Serveri Active Directory (Windows Server 2008 või uuem)
* Windows Server 2012 R2 värskendatud skeemi
* Azure Active Directory Premium litsentsi
* Windows Server 2012 R2 Federation Services, konfigureeritud SSO Azure AD
* Windows Server 2012 R2 Web rakenduse puhverserveri Microsoft Azure Active Directory ühendamine (Azure'i AD-ühenduse). [Laadige alla Azure'i AD-ühenduse siin](http://www.microsoft.com/en-us/download/details.aspx?id=47594).
* Kinnitatud Domeen.

<a name="known-issues-in-this-release"></a>See väljaanne teadaolevad probleemid
-------------------------------------------------------------------------------
* Seadme vastavalt tingimusvormingu juurdepääsupoliitikaid nõuda seadme objekti allahindluse Active Directory Azure Active Directory. Võib kuluda kuni 3 tundi seadme objektide peaks olema kirjutada tagasi Active Directory
* iOS 7-seadmete Küsi alati kasutaja serdi valimiseks ajal kliendi serdi autentimist.
* Mõned versioonid iOS8, enne kui iOS 8.3 ei tööta.

## <a name="scenario-assumptions"></a>Stsenaarium oletused
See stsenaarium eeldab, et teil on mõni Azure AD rentniku ja kohapealse Active Directory hübriidkeskkonna loomiseks. Nende rentnikud ühendatakse Azure'i AD-ühenduse kasutamise ja kinnitatud domeen ja ADFS SSO. Allpool kontroll aitab teil konfigureerida keskkonna eespool kirjeldatud etappi.

<a name="checklist-prerequisites-for-conditional-access-scenario"></a>Kontroll-loend: Eeltingimuste juurdepääsu stsenaarium
--------------------------------------------------------------
Ühendage oma Azure AD rentniku oma kohapealse Active Directory.

## <a name="configure-azure-active-directory-device-registration-service"></a>Azure Active Directory seadme registreerimise teenuse konfigureerimine
Selle juhendi abil saate juurutada ja konfigureerida Azure Active Directory seadme registreerimise teenust oma ettevõtte jaoks.

Sellest juhendist eeldab, et olete konfigureerinud Windows Server Active Directory ja Microsoft Azure Active Directory tellinud. Vt ülaltoodud eeltingimused.

Juurutada Azure Active Directory seadme registreerimise teenust oma Azure Active Directory rentniku, täitke järgmised kontroll, et tööülesanded. Kui viide link viib teid kontseptuaalne teema, naasmiseks järgmise kontroll-loendi, kui vaatate kontseptuaalne teema nii, et saate jätkata järgmise kontroll-loendi Tööülesanded ülejäänud. Mõned tööülesanded sisaldab stsenaarium valideerimine toimingut, mis aitavad teil kinnitada etappi on edukalt lõpule viidud.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>Osa 1: Luba Azure Active Directory seadme registreerimine

Järgige alltoodud lubamiseks ja konfigureerimiseks Azure Active Directory seadme registreerimise teenust kontroll.

| Ülesanne                                                                                                                                                                                                                                                                                                                                                                                             | Viide                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Luba seadme registreerimine oma Azure Active Directory rentniku lubamiseks seadmed töökoha liituma. Vaikimisi on lubatud mitmikautentimise teenuse kohta. Kuid mitmikautentimise on soovitatav, kui seadme registreerimine. Enne mitmikautentimise sisse Vali, veenduge, et et AD FS-i on konfigureeritud mitmikautentimise pakkuja. | [Luba Azure Active Directory seadme registreerimine](active-directory-conditional-access-device-registration-overview.md)               |
| Seadmete avastada oma Azure Active Directory seadme registreerimise teenust otsides tuntud DNS-i kirjed. Konfigureerige oma ettevõtte DNS-i, et seadmed saate tuvastada teie Azure Active Directory seadme registreerimise teenust.                                                                                                                                                   | [Konfigureerida Azure Active Directory seadme registreerimine.](active-directory-conditional-access-device-registration-overview.md) |

##<a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>Osa 2: Juurutada ja konfigureerida Windows Server 2012 R2 Active Directory Federation Services federation seose Azure AD häälestamine

| Ülesanne                                                                                                                                                                                                                                                                                                                                                                                             | Viide                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Active Directory domeeniteenuste domeeni laiendiga Windows Server 2012 R2 skeemi juurutamine. Teil pole vaja mõnda hulka kuuluvad teie domeeni versioonile Windows Server 2012 R2. Skeemi täiendamine on ainus nõue. | [Teie Active Directory domeeni teenuste skeemi täiendamine](#upgrade-your-active-directory-domain-services-schema)               |
| Seadmete avastada oma Azure Active Directory seadme registreerimise teenust otsides tuntud DNS-i kirjed. Konfigureerige oma ettevõtte DNS-i, et seadmed saate tuvastada teie Azure Active Directory seadme registreerimise teenust.                                                                                                                                                   | [Ettevalmistamine Active Directory tugi seadmetes](#prepare-your-active-directory-to-support-devices) |


##<a name="part-3-enable-device-writeback-in-azure-ad"></a>Osa 3: Luba seadme tagasikirjutusega Azure AD

| Ülesanne                                                                                                                                                                                                                                                                                                                                                                                             | Viide                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Täitke osa 2, mis võimaldab seadme tagasikirjutusega rakenduses Azure'i AD-ühenduse. Pärast lõpetamist, tagastab see sellest juhendist. | [Seadme tagasikirjutusega rakenduses Azure'i AD-ühenduse lubamine](#upgrade-your-active-directory-domain-services-schema)               |


##<a name="optional-part-4-enable-multi-factor-authentication"></a>[Valikulised] Osa 4: Luba mitmikautentimise

See on tungivalt soovitatav mitme võimaluse mitmikautentimise jaoks konfigureerida. Kui soovite nõuda MFA, lugege teemat [valimine mitme teguri turvalisus lahendus teie jaoks](../multi-factor-authentication/multi-factor-authentication-get-started.md). See sisaldab iga lahenduse ja lingid, mis aitavad teil konfigureerida lahendus teie valitud kirjeldus.

## <a name="part-5-verification"></a>Osa 5: kontrollimine

Juurutamise on lõppenud. Nüüd võite proovida mõne stsenaariumi. Teenuse katsetada ja selle funktsioonidega tutvuda allolevate linkide kaudu


| Ülesanne                                                                                                                                                                                                                         | Viide                                                                       |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Liituda mõne seadme teie töökohale kasutades Azure Active Directory seadme registreerimine. Saate liituda iOS-i, Windowsi ja Androidi seadmete                                                                                         | [Liitumine seadmed teie töökohale kasutades Azure Active Directory seadme registreerimine](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Saate vaadata ja lubada või keelata registreeritud seadmed portaalis administraator. Selles ülesandes kuvamisel mõne registreeritud seadme portaalis administraator.                                                        | [Azure Active Directory seadme registreerimise ülevaade](active-directory-conditional-access-device-registration-overview.md)                             |
| Veenduge, et seadme objektide kirjutada tagasi Azure Active Directory Windows Server Active Directory.                                                                                                                  | [Veenduge, et registreeritud seadmed on kirjutatud tagasi Active Directory](#verify-registered-devices-are-written-back-to-active-directory)                  |
| Nüüd, kui kasutajad saate oma seadmed registreerida, saate luua rakenduse access poliitikat AD FS-i, mis võimaldavad ainult registreeritud seadmed. Selles ülesandes loote reegli Accessi rakendus ja kohandatud juurdepääs keelatud sõnum | [Rakenduse access poliitika ja kohandatud juurdepääsu keelamise teade loomine](#create-an-application-access-policy-and-custom-access-denied-message)            |



## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Azure Active Directory integreerida kohapealse Active Directory
See aitab teil oma Azure AD rentniku integreerida oma kohapealse active directory, Azure'i AD-ühenduse abil. Kuigi juhised kehtivad Azure klassikaline portaalis, märkige selles jaotises toodud juhiseid.

1.  Logige sisse kontoga, millel on üldadministraator Azure AD Azure klassikaline portaali.
2.  Klõpsake vasakpoolsel paanil nuppu **Active Directory**.
3.  Valige vahekaardil **Directory** kataloogi.
4.  Valige vahekaart **Kataloogi integreerimise** .
5.  Jaotises **juurutada ja hallata** , järgige juhiseid 1 – 3 Azure Active Directory integreerida kohapealse kataloogi.
  1.    Domeene lisada.
  2.    Installimine ja käivitamine Azure'i AD-ühenduse: installige Azure'i AD-ühenduse järgmised juhised, [kohandatud installi Azure'i AD-ühenduse](./connect/active-directory-aadconnect-get-started-custom.md)kaudu.
  3. Kontrollige ja directory sünkroonimise haldamine. Ühekordse sisselogimise juhiseid on saadaval seda toimingut.

  > [AZURE.NOTE]
  > Ühinemise konfigureerimine AD FS, nagu on kirjeldatud ülal lingitud dokument. Teil pole vaja eelvaade funktsioonide konfigureerimine.


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Uuendada oma Active Directory domeeniteenuste skeem

> [AZURE.NOTE]
> Teie Active Directory skeem üleminekut ei saa tühistada. Soovitatav on esmalt teha seda testimiskeskkonnas.

1. Logige sisse oma domeenikontrolleri kontoga, millel on nii ettevõtte administraator ja skeemi administraatori õigused.
2. Kopeerige **[meediumi] \support\adprep** kataloogi ja sub kataloogide ühte hulka kuuluvad teie Active Directory domeeni.
3. Kui [meediumi] on Windows Server 2012 R2 installikandjalt tee.
4. Käsureale, liikuge adprep kataloogi ja käivitada: **adprep.exe forestprep**. Järgige kuvatavaid juhiseid skeemi täiendamiseks.

## <a name="prepare-your-active-directory-to-support-devices"></a>Teie Active Directory seadmed toetavad ettevalmistamine

>[AZURE.NOTE] See on ühekordne toiming, mida tuleb käitada ettevalmistamiseks teie Active Directory kogumis toetamiseks seadmed. Peate olema sisse logitud ettevõtte administraatori õigustega ja teie Active Directory kogumis peab olema Windows Server 2012 R2 skeemi selle toimingu lõpuleviimiseks.


##<a name="prepare-your-active-directory-forest-to-support-devices"></a>Teie Active Directory kogumis toetamiseks seadmete ettevalmistamine

> [AZURE.NOTE]
>See on ühekordne toiming, mida tuleb käitada ettevalmistamiseks teie Active Directory kogumis toetamiseks seadmed. Peate olema sisse logitud ettevõtte administraatori õigustega ja teie Active Directory kogumis peab olema Windows Server 2012 R2 skeemi selle toimingu lõpuleviimiseks.

### <a name="prepare-your-active-directory-forest"></a>Teie Active Directory kogumis ettevalmistamine

1.  Avage Windows PowerShelli käsuaknas ja tippige oma liiduserveri: lähtestamise ADDeviceRegistration
2.  **ServiceAccountName**Küsimisel sisestage valitud teenusekonto AD FS-i teenuse konto nimi. Kui see on assotsiatsioon konto, sisestage konto **domain\accountname$** -vormingus. Kasutage domeeni konto vorming **domain\accountname**.



### <a name="enable-device-authentication-in-ad-fs"></a>Seadme autentimise AD FS-i lubamine

1. Teie liiduserveri, avage AD FS-i halduskonsool ja liikuge **AD FS-i** > **Autentimise poliitika**.
2. Valige **Redigeeri globaalne esmane autentimine...** paanilt **toimingud** .
3. Kontrollige **seadme autentimise** ja seejärel klõpsake**nuppu OK**.
4. Vaikimisi perioodiliselt eemaldab AD FS-i kasutamata seadmete Active Directoryst. Selle ülesande peab keelata, kasutades Azure Active Directory seadme registreerimine, et saab hallata Azure'i seadmetes.


### <a name="disable-unused-device-cleanup"></a>Kasutamata seadme puhastus keelamine
1. Avage Windows PowerShelli käsuaknas ja tippige oma liiduserveri: Set-AdfsDeviceRegistration - MaximumInactiveDays 0

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Ettevalmistamine Azure'i AD-ühenduse seadme tagasikirjutusega

1.  Täitke osa 1: Azure'i AD ettevalmistamine ühendust luua.


## <a name="join-devices-to-your-workplace-using-azure-active-directory-device-registration"></a>Liitumine seadmed teie töökohale kasutades Azure Active Directory seadme registreerimine

### <a name="join-an-ios-device-using-azure-active-directory-device-registration"></a>Liitumine Azure Active Directory seadme registreerimine iOS-seadmes

Azure Active Directory seadme registreerimine kasutab Over-the-Air profiili registreerimise käigus iOS-seadmete jaoks. Selle protsessi algab kasutaja ühenduse profiili registreerimine URL-i Safari veebibrauseri abil. URL-i vorming on järgmine:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

Kus `yourdomainname` on domeeni nimi, mille olete konfigureerinud Azure Active Directory. Näiteks kui teie domeeni nimi on contoso.com, oleks URL:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

On palju erinevaid võimalusi suhelda seda URL-i kasutajatele. Ühe soovitatav viis on avaldada seda URL-i kohandatud rakenduse Access denied sõnumi AD FS-i. See on eelseisvad jaotise alla: [loomine rakenduses access poliitika ja kohandatud juurdepääsu keelamise teade](#create-an-application-access-policy-and-custom-access-denied-message).

###<a name="join-a-windows-81-device-using-azure-active-directory-device-registration"></a>Liitumine Windows 8.1 seadmes, kasutades Azure Active Directory seadme registreerimine

1. Windows 8.1 seadmes, liikuge **Arvutisätete** > **võrgu** > **töökoha**.
2. Sisestage oma kasutajanimi UPN-vormingus. Näiteks dan@contoso.com..
3. Valige **Liitu**.
4. Kui palutakse, logige sisse oma kasutajanimi ja parool. Kasutatav seade on nüüd liitunud.

###<a name="join-a-windows-7-device-using-azure-active-directory-device-registration"></a>Liitumine Windows 7 seadme abil Azure Active Directory seadme registreerimine
Windows 7 domeeni registreerimiseks liitunud seadme registreerimine tarkvarapaketi juurutamiseks seadmed. Tarkvarapaketi nimetatakse töökoha liitumine Windows 7 ja on saadaval [veebisaidil Microsoft Connect](https://connect.microsoft.com/site1164)alla. Paketi kasutamise kohta on saadaval veebisaidil [konfigureerimine automaatse seadme registreerimine Windows 7 domeeni ühendatud seadmete](active-directory-conditional-access-automatic-device-registration-windows7.md)juhiseid.

### <a name="join-an-android-device-using-azure-active-directory-device-registration"></a>Liitumine Androidi seadme abil Azure Active Directory seadme registreerimine

[Azure'i Autentija Androidi teema](active-directory-conditional-access-azure-authenticator-app.md) on juhiseid, kuidas installida Azure autentimisrakenduse oma Androidi seadmes ja töö konto lisamine. Kui töö konto on edukalt loodud Androidi seadmes, selle seade on ühendatud organisatsiooni töökoha.

## <a name="verify-registered-devices-are-written-back-to-active-directory"></a>Veenduge, et registreeritud seadmed on kirjutatud tagasi Active Directory
Saate vaadata ja veenduge, et teie seadme objektid on kirjutanud tagasi oma Active Directory abil LDP.exe või ADSI redigeerimine. Mõlemad on saadaval Active Directory administraator tööriistu.

Vaikimisi paigutatakse seadme objekte, mis on kirjutatud tagasi Azure Active Directory kaudu oma AD FS-i serveripargi sama Domeen.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Rakenduse access poliitika ja kohandatud juurdepääsu keelamise teade loomine
Võtke arvesse järgmist stsenaariumi: saate luua rakenduse Relying poole usaldus AD FS-i ja konfigureerida emissiooni autoriseerimine reegel, mis võimaldab ainult registreeritud seadmed. Nüüd on lubatud ainult seadmetes, mis on registreeritud, rakenduse avamiseks. Teha lihtne pääseda rakenduse kasutajate jaoks, saate konfigureerida kohandatud juurdepääs, mis on keelatud, mis sisaldab juhiseid, kuidas liituda oma seade. Kasutajatel on nüüd sujuvalt viis rakenduse juurdepääsuks oma seadmed registreerida.

Järgmised toimingud näitab teile, kuidas rakendada selle stsenaariumi.

>[AZURE.NOTE]
Selles jaotises eeldab juba konfigureerinud on tuginedes poole usaldada rakenduse AD FS-i.

1. Avage AD FS-i MMC tööriista ja liikuge AD FS-i > usalda seosed > tuginedes tootja loodab.
2. Otsige üles rakendus, mille rakendatakse see Accessi uus reegel. Paremklõpsake rakendus ja valige Redigeeri taotluste reeglid...
3. Valige vahekaart **Emissiooni autoriseerimine reeglid** ja seejärel valige **Lisa reegel**
4. **Taotluse reegli** malli ripploendit, valige **Luba või Keela kasutajate sissetuleva nõude kohta**. Valige **edasi**.
5. Taotluse reegli nimi: välja tüüp: **ühenduse jaoks registreeritud seadmetes**
6. Sissetulevad kaudu taotlemine tüüp: ripploendis, valige **On registreeritud kasutaja**.
7. Sissetulevad nõuda väärtus: välja, tippige: **true**
8. Klõpsake raadionuppu **Luba juurdepääs kasutajad, kellel on see sissetulevate taotluste** .
9. Valige **valmis** ja seejärel valige **Rakenda**.
10. Reeglid, mis on rohkem kui äsja loodud reegel lubav eemaldada. Näiteks vaikimisi **Kõigi kasutajate juurdepääsu lubada** reeglit eemaldada.

Teie rakendus on nüüd konfigureeritud Luba juurdepääs ainult siis, kui kasutaja on pärit seadme, mille nad registreerimis- ja töökohal liitunud. Täpsemate Accessi poliitika kohta leiate [Mitme teguri juurdepääsu reguleerimine seotud ohtude haldamine](https://technet.microsoft.com/library/dn280949.aspx).

Järgmiseks tuleb konfigureerida kohandatud veateate rakenduse. Kuvatakse tõrketeade kuvatakse anda kasutajatele teada, et nad peate liituma oma seadme töökohas enne, kui neil on lubatud juurdepääs rakenduse. Saate luua kohandatud rakenduse access, keelamise teade kohandatud HTML-i ja Windows PowerShelli abil.

Teie liiduserveri, avage Windows PowerShelli käsuviiba aken ja tippige järgmine käsk. Üksused, mis on seotud teie süsteemi käsk osade asendamiseks tehke järgmist.

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Registreerige oma seade selle rakenduse juurdepääsuks.

**Kui kasutate iOS-seadmes, valige see link liitumiseks seadme**.

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

IOS-seadmes liita oma töökoha.


**Kui kasutate opsüsteemiga Windows 8.1 seadmes**, saate liituda seadme **Arvuti **sätted >  **võrgu** > **töökoha**.


Kus on "**tuginedes tootja usalda nimi**" oma rakenduste tuginedes poole usaldus objekti AD FS-i nimi.
Kus asub **teiedomeen.com** on konfigureeritud Azure Active Directory domeeni nimi. Nt contoso.com.
Kindlasti eemaldamine HTML-sisu, mida te kaotate cmdlet **Set-AdfsRelyingPartyWebContent** mis tahes reapiirid (vajaduse korral).


Nüüd, kui kasutajad rakenduse seadmes, mis on registreeritud Azure Active Directory seadme registreerimise teenust, nad saavad lehe, mis sarnaneb ekraanipilt allpool.

![Vea, kui kasutajad pole registreeritud oma seadme Azure AD Screeshot](./media/active-directory-conditional-access/error-azureDRS-device-not-registered.gif)

##<a name="related-articles"></a>Seotud artiklid

- [Artikli Index Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
