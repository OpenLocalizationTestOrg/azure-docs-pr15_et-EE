<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: installi viisardit teist korda | Microsoft Azure'i"
    description="Selgitab, kuidas installimise viisard töötab teine kord, kui käivitate selle."
    keywords="Azure'i AD-ühenduse installimise viisard võimaldab teil teise käitamisel hooldustööd sätete konfigureerimine"
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Azure'i AD-ühendus sünkroonimine: installi viisardit teist korda
Azure'i AD-ühenduse installimise viisardi esimesel see juhatab teid läbi konfigureerimine installi. Kui käivitate installimise viisard uuesti, see pakub haldamise võimalusi.

Installimise viisard leiate **Azure'i AD-ühenduse**nimega menüüs start.

![Menüü Start](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

Installi viisardi käivitamisel kuvatakse lehe suvanditest.

![Leht koos täiendavate tööülesannete loend](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Kui olete installinud ADFS-i abil Azure'i AD-ühenduse, on teil veelgi rohkem suvandeid. Teil on ADFS-i lisasuvandid on avaldatud [ADFS-i haldus](active-directory-aadconnect-federation-management.md#ad-fs-management).

Valige üks järgmistest tööülesanded ja klõpsake nuppu **edasi** .

> [AZURE.IMPORTANT] Kui olete viisardi installimist avage, sync engine kõik toimingud on peatatud. Veenduge, et installimise viisardi sulgemiseks, kui olete lõpetanud oma konfiguratsioonimuudatuste.

## <a name="view-current-configuration"></a>Praeguse vaate konfiguratsioon
See suvand annab teile praegu konfigureeritud suvandeid kiiresti vaadata.

![Lehe kõik suvandid ja nende loendi abil](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Klõpsake nuppu **Eelmine** tagasi. Kui valite **välju**, installimise viisardi sulgemiseks.

## <a name="customize-synchronization-options"></a>Sünkroonimise suvandite kohandamine
See suvand on kasutatud sünkroonimine konfiguratsiooni muudatuste tegemiseks. Näete suvandid kaudu kohandatud Otsingukonfiguratsiooni Installitee alamhulga. Näete isegi juhul, kui kasutasite algselt kiire installi see suvand.

- Saate [lisada rohkem kataloogid](active-directory-aadconnect-get-started-custom.md#connect-your-directories). Eemaldamist kataloogi, leiate teemast [konnektori kustutamine](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).
- [Muuda domeeni ja OU filtreerimine](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
- Rühma filtrite eemaldamiseks.
- [Muuda valikulised funktsioonid](active-directory-aadconnect-get-started-custom.md#optional-features).

Muud võimalused algse installi ei saa muuta ja pole saadaval. Need suvandid on:

- Muutke atribuut userPrincipalName ja sourceAnchor jaoks.
- Muuta liitumist objektide eri pärineb.
- Rühma alusel filtreerimise lubamine

## <a name="refresh-directory-schema"></a>Directory skeemi värskendamine
See suvand on kasutatud kui olete muutnud skeemiga ühel oma kohapealse AD DS metsade. Näiteks võite installimist Exchange'i või on versioonile Windows Server 2012 skeemi seadme objektidega. Sel juhul peate paluge Azure'i AD-ühenduse lugeda skeemiga uuesti AD DS ja värskendage oma vahemälu. Seda toimingut ka taastab sünkroonimine reegleid. Kui lisate Exchange skeemiga, nagu näiteks, lisatakse konfiguratsiooni Exchange'i sünkroonimise reeglid.

Kui valite selle suvandi, on ära toodud kõik kataloogid konfiguratsioonis. Saate hoida vaikesäte ja Värskenda kõik metsade või valimise mõned neist.

![Lehe kõik kataloogid keskkonnas loendi abil](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Lavastus režiimi konfigureerimine
See suvand võimaldab teil lubamine ja keelamine lavastus režiimi serveris. Lavastus režiimi ja selle kasutamise kohta lisateabe saamiseks leiate [toiminguid](active-directory-aadconnectsync-operations.md#staging-mode).

Suvandi näitab, kas lavastus on praegu lubatud või keelatud:  
![Suvand, mis näitab hetkeseisu lavastus režiim](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

Kui soovite muuta, valige see suvand ja valimine või valimise olev ruut.  
![Suvand, mis näitab hetkeseisu lavastus režiim](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Kasutaja sisselogimine muutmine
See suvand, mis võimaldab teil muuta parooli sünkroonimise liitmist või vastupidi. Ei saa muuta **konfigureerimine**.

Selle suvandi kohta leiate lisateavet teemast [kasutajate sisselogimist](active-directory-aadconnect-user-signin.md#changing-user-sign-in-method).

## <a name="next-steps"></a>Järgmised sammud

- Lugege lisateavet konfiguratsiooni mudelit, mis kasutavad Azure'i AD-ühenduse sünkroonimise [Mõistmine deklaratiivseid ettevalmistamise](active-directory-aadconnectsync-understanding-declarative-provisioning.md)kohta.

**Teemade ülevaade**

- [Azure'i AD-ühendus sünkroonimine: Kuupäevatabelid ja nende sünkroonimine kohandamine](active-directory-aadconnectsync-whatis.md)
- [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
