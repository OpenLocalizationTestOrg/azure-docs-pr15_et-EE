<properties
    pageTitle="Sätted ja andmete rändluse FAQ | Microsoft Azure'i"
    description="Alt leiate vastused küsimustele IT-administraatorid võib olla sätted ja rakenduse andmete sünkroonimine."
    services="active-directory"
    keywords="ettevõtte maakond rändluse sätted windows pilves, korduma kippuvad küsimused enterprise olekus rändlusteenuse"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="settings-and-data-roaming-faq"></a>Sätted ja andmete rändluse FAQ

Sellest teemast leiate vastused küsimustele IT-administraatorid võib olla sätted ja rakenduse andmete sünkroonimine.

## <a name="what-data-roams"></a>Milliseid andmeid roams?
**Windowsi sätete**: Windowsi operatsioonisüsteemi sisse ehitatud Arvutisätted. Üldiselt need sätted, mida arvuti isikupärastamiseks ja need sisaldavad järgmist ülesandekategooriat.

- *Kujundus*, mis sisaldab funktsioone, näiteks töölaua teema ja tegumiriba sätted.
- *Internet Exploreri sätted*, sh viimati avatud vahekaartide ja lemmikud.
- *Edge'i brauseri sätteid*, näiteks lemmikud ja lugemine nimekirja.
- *Paroolide*, sh Interneti paroole, Wi-Fi profiilid ja teistele.
- *Keele-eelistused*, mis sisaldab klahvipaigutust, süsteemi keel, kuupäeva ja kellaaja ja sätted.
- *Accessi funktsioonid hõlpsamaks*, nt kõrge kontrastsusega kujunduse, Jutustaja ja Ekraanisuurendi.
- *Muud Windowsi sätted*, nt Käsuviip sätted ja rakenduste loendis.


**Rakenduse andmete**: Universal Windows rakendusi saate kirjutada sätted andmete rändluse kausta ja kõik andmed, kirjutada selle kausta sünkroonitakse automaatselt. See on kuni üksikute rakenduse arendaja kujundada rakenduse ära seda võimalust. Lisateavet selle kohta, kuidas töötada Universal Windows rakendus, mis kasutab rändluse teemast [appdata salvestusruumi API](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) ja [Windows 8 appdata Rändluskulud arendaja ajaveebi](http://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx).

## <a name="what-account-is-used-for-settings-sync"></a>Millist kontot kasutatakse sätete sünkroonimine?
Windows 8 ja Windows 8.1, sätete sünkroonimine alati kasutatakse tarbija Microsofti kontod. Ettevõtte kasutajad Microsofti kontoga ühenduse loomiseks oma Active Directory domeeni konto, et saada juurdepääsu sätete sünkroonimine võimalus. Opsüsteemis Windows 10 see ühendatud Microsofti konto funktsioonid on asendatud esmane/teisene konto raames.

Esmane konto on määratletud konto, mida kasutatakse Windows sisse logida. See võib olla Microsofti konto, konto Azure Active Directory (Azure AD), kohapealse Active Directory kontot või kohalik konto. Lisaks esmase konto Windows 10 kasutajad saavad lisada ühe või mitme teisene cloud kontod oma seadmesse. Teisene konto on üldiselt Microsofti konto, konto Azure AD või mõne muu nt Gmaili või Facebooki konto. Teisene kontod hõlmavad täiendavaid teenuseid nagu ühekordse sisselogimise ja Windowsi poest, kuid need ei võimalda anda sätete sünkroonimine.

Opsüsteemis Windows 10 saab kasutada ainult esmase konto seadme sätete sünkroonimine (vt [Kuidas uuendada Microsofti konto sätete sünkroonimine opsüsteemis Windows 8: Azure'i AD sätete sünkroonimine Windows 10?](active-directory-windows-enterprise-state-roaming-faqs.md#How-do-I-upgrade-from-Microsoft-account-settings-sync-in-Windows-8-to-Azure-AD-settings-sync-in Windows-10?)).

Andmed on segatud kunagi seadme erinevate kasutajakontodele. On kaks reeglid sätete sünkroonimine.

- Windowsi sätted kuvatakse alati ringi esmane kontoga.
- Rakenduse andmed kuvatakse kodeeritud konto, mida kasutatakse rakenduse hankimine. Sünkroonitakse ainult rakenduste sildistatud esmane kontoga. Rakenduse omandiõiguse sildistamise määratakse, kui rakendus on pool laaditud Windowsi poest või mobiilsideseadmete haldus (MDM-i).

Kui rakenduse omanik ei saa tuvastada, kas see ringi esmane kontoga. Kui seade on Windows 8 või Windows 8.1 versioonile Windows 10, tuleb kõik rakendused sildistada nagu omandanud Microsofti kontoga. Seda sellepärast, et enamik kasutajaid hankida rakendusi Windowsi poe kaudu ja esines Windowsi poe tugi Azure AD kontod enne opsüsteemi Windows 10. Kui rakendus on installitud on ühenduseta litsents kaudu, kuvatakse rakenduse sildistatud seadme esmane kontot kasutades.

>[AZURE.NOTE]  
> Windows 10 seadmetes, mis on enterprise omanik ja Azure AD ühendatud saate oma Microsofti kontoga ühenduse enam domeeni kontoga. Microsofti kontoga ühenduse domeeni konto ja on kõik kasutaja andmete sünkroonimine Microsofti kontoga (st Microsofti konto kaudu ühendatud Microsofti kontot ja Active Directory funktsionaalsuse rändlusteenuse) eemaldatakse Windows 10 seadmetes, mis on ühendatud Active Directory või Azure AD keskkond on ühendatud.

## <a name="how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10"></a>Kuidas uuendada Microsofti konto sätete sünkroonimine opsüsteemis Windows 8: Azure'i AD sätete sünkroonimine Windows 10?
Kui teil on ühendatud Active Directory domeeni operatsioonisüsteemi Windows 8 või Windows 8.1 ühendatud Microsofti kontoga, sünkroonitakse teie Microsofti konto kaudu sätted. Pärast üleminekut versioonile Windows 10, saate jätkuvalt sünkroonimine kasutajasätete Microsofti konto kaudu, kui olete domeeni ühendatud kasutaja ja Active Directory domeeni ei saa ühendust Azure AD.

Kui kohapealse Active Directory domeeni kasutajaga Azure AD, proovib seadme kaudu ühendatud sätete sünkroonimine Azure AD konto. Kui Azure AD administraator ei luba ettevõtte olekus rändlusteenuse, teie ühendatud konto Azure AD katkestab sünkroonimise sätted. Kui teil on Windows 10 kasutaja ja olete sisse logitud identiteedi Azure AD, mida hakatakse sünkroonima Windowsi sätted, niipea, kui teie administraator lubab sätete sünkroonimine kaudu Azure AD.

Kui ettevõtte seadmesse salvestatud isikuandmeid, arvestage, et Windows OS- ja rakenduse hakkab Azure AD sünkroonimine. See on järgmised tagajärjed.

- Isiklik Microsofti konto sätted kuvatakse triivi peale sätted oma töö või koolikontod Azure AD. See on, kuna Microsofti konto ja Azure AD sätete sünkroonimine on nüüd kasutades eraldi kontod.
- Isikuandmeid, näiteks Wi-Fi paroole, web identimisteabe ja Internet Exploreri lemmikud, mis varem ühendatud Microsofti konto kaudu sünkroonitud sünkroonitakse Azure AD kaudu.


## <a name="how-do-microsoft-account-and-azure-ad-enterprise-state-roaming-interoperability-work"></a>Kuidas Microsofti konto ja Azure AD Enterprise olekus rändlusteenus interoperability töö?
Windows 10 November 2015 või uuem versioon versioonides Enterprise olekus rändlusteenuse toetatakse ainult ühe konto korraga. Kui te Windowsi sisse logida, kasutades töö- või koolikontoga Azure AD, sünkroonitakse kõik andmed Azure AD kaudu. Kui logite Windowsi isikliku Microsofti kontoga, sünkroonitakse kõik andmed Microsofti konto kaudu. Universaalne kaust appdata on ringi ainult esmane sisselogimise kontot kasutades seadme ja see on ringi ainult siis, kui rakenduse litsentsi kuulub esmane konto. Universaalne appdata kuuluv teisene kontodest apps edaspidi ei sünkroonita.

## <a name="do-settings-sync-for-azure-ad-accounts-from-multiple-tenants"></a>Tehke sätete sünkroonimine Azure AD kontode kaudu mitme rentniku jaoks?
Kui mitme Azure AD kontod erinevate Azure AD rentnikud on samasse seadmesse, peate värskendama seadme registri suhelda Azure Rights Management (Azure RMS) iga Azure AD rentniku jaoks.  

1. Iga Azure AD rentniku GUID otsimine. Avage Azure'i klassikaline portaal ja valige mõni Azure AD rentniku. Rentniku GUID on URL brauseri aadressiribale. Näiteks: `https://manage.windowsazure.com/YourAccount.onmicrosoft.com#Workspaces/ActiveDirectoryExtension/Directory/Tenant GUID/directoryQuickStart`
2. Kui olete GUID, peate registrivõtme lisamine **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\SettingSync\WinMSIPC\<rentniku ID GUID >**.
**Rentniku ID GUID** Key, looge uus stringist väärtus (REG-mitme-SZ) nimega **AllowedRMSServerUrls**. Oma andmete jaoks määrata hulgilitsentsimise jaotuse punkti URL-id on Azure rentnikud, mis kasutab seade.
3. Hulgilitsentsimise jaotuse punkti URL-ide leiate käivitades cmdleti **Get-AadrmConfiguration** . Kui **LicensingIntranetDistributionPointUrl** ja **LicensingExtranetDistributionPointUrl** väärtused on erinevad, määrake mõlemad väärtused. Kui väärtused on samad, määrake väärtus ainult üks kord.


## <a name="what-are-the-roaming-settings-options-for-existing-windows-desktop-applications"></a>Mis on rändluse sätted suvandite olemasoleva Windowsi töölaua rakendusi?
Rändlusteenuse töötab ainult ühes kohas Windowsi rakendused. Rändluse olemasoleva Windowsi töölaua rakendus, mis võimaldab saadaval on kaks võimalust:

- [Töölaua Bridge](http://aka.ms/desktopbridge) abil saate oma olemasoleva Windowsi töölauarakendused tuua ühes kohas Windowsi platvormi. Siit minimaalsete kood muudatused on vaja Azure AD Rakenduse andmete rändluse ära. Töölaua silla pakub teie rakendused rakendus identiteediga, mis on vaja rakenduse andmete rändluse olemasolevate töölauarakenduste jaoks.
- [Kasutaja kogemus Virtualization (UE-V)](https://technet.microsoft.com/library/dn458947.aspx) abil saate olemasoleva Windowsi töölaua rakenduste kohandatud sätteid malli loomine ja lubamine rändlusteenuse Win32 rakendused. See suvand ei nõua koodi rakenduse muutmiseks minirakenduse arendaja. Piiratud kohapealse Active Directory rändlusteenuse klientidele, kes ostsid Microsoft Desktop optimeerimine pakett UE-V.

Saavad administraatorid konfigureerida UE-V ringi liikuda Windowsi töölauarakenduse andmed, muutes rändluse Windows OS sätted ja universaalne rakenduse andmed läbi [UE-V rühma poliitikad](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2), sh:

- Ringi liikuda Windowsi rühmapoliitika sätted
- Sünkroonimine Windowsi rakendused rühmapoliitika
- Internet Exploreri rändlusteenuse jaotises rakendused

Microsoft võib tulevikus uurida, kuidas muuta UE-V sügavalt integreeritud Windowsi ja laiendamine UE-V sätted Azure AD pilve kaudu ringi liikuda.


## <a name="can-i-store-synced-settings-and-data-on-premises"></a>Saate salvestada sünkroonitud sätteid ja andmete kohapealne?
Ettevõtte olekus rändlusteenuse talletab kõik sünkroonitud andmed Azure pilveteenusesse. UE-V pakub asutusesisese rändlusteenuse lahendus.

## <a name="who-owns-the-data-thats-being-roamed"></a>Kellele on roamed andmeid?
Oma ettevõtte andmete roamed Enterprise olekus rändlusteenuse kaudu. Andmed salvestatakse on Azure andmekeskuse. Kõik kasutajate andmed on krüptitud nii ajal kui ka ülejäänud pilveteenuses Azure RMS-i abil. See on Microsofti konto põhinev sätted sync, mis krüptib ainult teatud tundliku loomuga andmeid, nt kasutajatunnust seadme lahkumisel võrreldes parandamine.

Microsoft soovib kliendiandmete kaitsmine. Ettevõtte kasutaja sätted andmed on automaatselt krüptitud Azure RMS-i lahkumisel Windows 10 seadmesse, nii et teised kasutajad saavad lugeda andmed. Kui teie ettevõte kasutab tasuliseks tellimuseks Azure RMS-i, saate kasutada Azure RMS-i funktsioone, näiteks jälitamine ja dokumente tühistada, automaatselt e-kirju, mis sisaldavad tundliku teabe kaitse, ja hallata oma klahvid ("tuua oma klahv" lahenduse nimetatakse ka BYOK). Nende funktsioonide ja Azure RMS-i toimimise kohta leiate lisateavet teemast [mis on Azure Rights Management](https://technet.microsoft.com/jj585026.aspx).

## <a name="can-i-manage-sync-for-a-specific-app-or-setting"></a>Mõne kindla rakenduse või säte sünkroonimise saate hallata?
Opsüsteemis Windows 10 on pole MDM-i või rühmapoliitika sätte keelamiseks rändlusteenuse üksikute rakenduse. Rentnikuadministraatorid saavad keelata appdata Sünkrooni kõik rakendused hallatavate seadmes, kuid ei kontrolli on iga rakenduse või rakenduse sees tasemel.

## <a name="how-can-i-enable-or-disable-roaming"></a>Kuidas lubada või keelata rändlusteenuse?
Rakenduse **sätted** , liikuge **kontod** > **sätete sünkroonimine**. Sellel lehel saate vaadata, millist kontot kasutatakse ringi liikuda sätted ja saate lubada või keelata sätteid nii, et olla roamed üksikud rühmad.

## <a name="what-is-microsofts-recommendation-for-enabling-roaming-in-windows-10"></a>Mis on Microsofti soovitus rändluse Windows 10, mis võimaldab?
Microsoft on mõned sätted rändlusteenuse lahendusi, mis on saadaval, sh rändluse kasutajaprofiilid, UE-V ja ettevõtte olekus Rändluskulud.  Microsoft on võtnud endale kohustuse luua investeeringu Enterprise maakond rändlusteenuse tulevikus Windowsi versiooni. Kui teie asutus ei ole valmis või mugav andmete teisaldamisega pilveteenusesse, siis soovitame kasutada UE-V kui teie peamine rändluse tehnoloogia. Kui teie asutus nõuab rändlusteenuse olemasoleva Windowsi töölauarakenduste tugi, kuid soovib teisaldada pilve, soovitame kasutada Enterprise olekus rändluse ja UE-V. Kuigi UE-V ja ettevõtte olekus rändlusteenuse on väga sarnased tehnoloogiad, ei ole üksteist välistavad. Need täiendavad üksteist tagada, et teie ettevõte pakub rändluse teenused, mida teie kasutajatel on vaja.  

Ettevõtte olekus rändlusteenuse ja UE-V kasutamisel kehtivad järgmised reeglid.

- Ettevõtte olekus rändlusteenuse on esmane rändluse agent seadmes. UE-V kasutab täiendada "Win32 vahe."
- UE-V Windowsi sätted ja kaasaegse UWP rakenduse andmete rändluse peaks välja, kui poliitikat UE-V rühm, kasutades. Need on juba hõlmatud Enterprise olekus rändlusteenuse.

## <a name="how-does-enterprise-state-roaming-support-virtual-desktop-infrastructure-vdi"></a>Kuidas ettevõtte olekus rändlusteenuse ei toeta töölaua virtuaaltöölaua infrastruktuuri (VDI)?
Ettevõtte olekus rändlusteenuse on toetatud Windows 10 kliendi SKU-de jaoks, kuid mitte serveri SKU-de jaoks. Kui mõnda muud klienti VM majutab hypervisor arvutisse ja eemalt sisselogimist virtuaalse masina, teie andmed on ringi liikuda. Mitme kasutaja ühiskasutusse sama OS ja kasutajate kaugühenduse teel sisse logida serveri täielik töölauafunktsioonid, rändlusteenuse ei pruugi töötada. Viimane seansi stsenaarium ei toeta.


## <a name="what-happens-when-my-organization-purchases-azure-rms-after-using-roaming"></a>Mis juhtub minu ettevõtte ostmisel Azure RMS-i abil rändlusteenuse pärast?
Kui teie ettevõttes on kasutusel juba rändluse Windows 10 Azure RMS-i piiratud tasuta tellimus, makstud Azure RMS-i tellimuse pole on mis tahes mõju Rändluskulud funktsiooni ja IT-administraator on vajalik konfiguratsiooni muudatusi.

## <a name="known-issues"></a>Teadaolevad probleemid

- Kui proovite sisse logida kiipkaardi või virtuaalne kiipkaardi kasutamine Windowsi seadmes, sätete sünkroonimine ei tööta enam. Probleemi lahendamiseks tulevikus värskendusi versioonile Windows 10.
- Internet Exploreri lemmikute sünkroonimine töötamiseks on vaja opsüsteemi Windows 10 (järk 10586.494 või uuem versioon) juuli koondvärskenduse.
- Andmed, mis on kaitstud Windowsi teabekaitse ei sünkroonita Enterprise olekus rändlusteenuse kaudu. Lisaks pole kogemus masinad, mis on lubatud Windows teabekaitse kujunduse sünkroonimine. 
- Teatud tingimustel Enterprise olekus rändlusteenuse võib nurjuda sünkroonida andmed, kui Azure'i Mitmikautentimise on konfigureeritud.
    - Kui teie seade on konfigureeritud nõudma [Mitmikautentimise](multi-factor-authentication.md) Azure Active Directory portaalis, ei pruugi te sünkroonimise ajal Windows 10 seadmesse parooliga sisselogimine sätted. Seda tüüpi Mitmikautentimise konfiguratsioon on mõeldud kaitsmiseks Azure administraatorikontoga. Administraator kasutajate võimalik logides sisse oma Windows 10 seadmetes [Microsoft pass töö](active-directory-azureadjoin-passport.md) PIN-koodi või muud Azure teenused, nt Office 365 juurdepääsul Mitmikautentimise täites sünkroonida.
    - Sünkroonimine võib nurjuda, kui selle administraator konfigureerib juurdepääsu Active Directory Federation Services Mitmikautentimise poliitika ja seadme juurdepääsu luba on aegunud.  Veenduge, et logige sisse ja välja logida, kasutades [Microsoft pass töö](active-directory-azureadjoin-passport.md) PIN-koodi või täitke muud Azure teenused, nt Office 365 juurdepääsul Mitmikautentimise.

- Kui masina domeeni ühendatud automaatse registreerimise ja Azure Active Directory seadmed, see võib tekkida sünkroonimine fail kui seade on väljaspool pikema perioodi ja domeeni autentimine, ei saa lõpule viia. Selle probleemi lahendamiseks ühendage seade ettevõtte võrku nii, et sünkroonimise jätkamiseks.


## <a name="related-topics"></a>Seotud teemad
- [Ettevõtte olekus rändluse ülevaade](active-directory-windows-enterprise-state-roaming-overview.md)
- [Luba ettevõtte olekus rändlusteenuse Azure Active Directory 's](active-directory-windows-enterprise-state-roaming-enable.md)
- [Rühmitamine poliitika ja MDM-i sätete sätete sünkroonimine](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Windows 10 Rändluskulud sätted viide](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
