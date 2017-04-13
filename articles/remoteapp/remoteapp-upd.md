
<properties 
    pageTitle="Kuidas salvestada Azure RemoteApp kasutaja andmed ja sätted? | Microsoft Azure'i"
    description="Siit saate teada, kuidas Azure RemoteApp salvestab kasutaja profiili ketta kasutamine kasutaja andmeid."
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

# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Kuidas salvestada Azure RemoteApp kasutaja andmed ja sätted?

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp salvestab kasutaja identiteedi ja kohandused seadmete ja seansid. Kasutaja andmed on salvestatud kasutaja kohta saidikogumi kettal, nimetatakse kasutaja profiili kettal (UPD). Ketas järgneb kasutaja ja tagab, et kasutajal on ühtsete kogemusi, olenemata sellest, kus need sisse logida. salvestab 

Kasutaja profiili ketast on täiesti läbipaistev kasutajale – kasutajad salvestada dokumendid oma kausta dokumendid (mis näib kohalikule kettale) ja muuta oma rakenduse sätteid nagu tavaliselt. Samal ajal kõiki isiklikke sätteid püsida Azure RemoteApp ühendamisel mis tahes seadme kaudu. Kõik kasutaja näeb on oma andmeid samas kohas.

Iga UPD on 50GB salvestusruumi püsivate ja see sisaldab nii kasutaja andmeid ja sätted. 

Kasutaja profiili andmete põhjal täpsemalt lugeda.

>[AZURE.NOTE] Teil on selle UPD keelata? Saate teha selle nüüd - väljamöllimise Pavithra's ajaveebipostitusest [Keelamine kasutaja profiili ketta (UPDs) jaotise Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/), üksikasju.


## <a name="how-can-an-admin-get-to-the-data"></a>Kuidas saan andmetega administraator?

Kui vajate mõne (avariitaastet või kui kasutaja lahkub ettevõtte) kasutajatele andmetele juurdepääsuks, Azure tugiteenuste ja tellimuse teabe kogumine ja kasutaja identiteet. Azure RemoteApp meeskonnatöö annab teile selle VHD URL-i. Laadige alla selle VHD ja tuua kõik dokumendid või failid. Pöörake tähelepanu sellele, on VHD 50GB, et kulub natuke alla laadida.


## <a name="is-the-data-backed-up"></a>Andmeid varundada?

Jah, saame salvestamine varukoopia andmete kasutaja geograafilise asukoha kohta. Andmed on kirjutuskaitstud ja pääseb samamoodi nagu tavalise andmeid oleks (Azure RemoteApp saamiseks võtke ühendust), kui esmane andmekeskuse ei tööta. Andmed kopeeritakse reaalajas varundusasukohta ja me ei säilita koopiad erinevaid versioone. Klõpsake andmete rikkumist, me ei saa taastada head eelnevalt teada versiooni, kuid kui esmane andmekeskuse ei tööta, siis saab kasutaja andmete toomine teise asukohta.

## <a name="how-do-users-see-the-upd-on-the-server-side"></a>Kuidas kasutajad näen selle UPD serveripoolse?

Iga kasutaja on oma kataloogi serveris, mis nende UPD kaardid: c:\Users\username.

## <a name="whats-the-best-way-to-use-outlook-and-upd"></a>Mis on parim viis kasutada Outlooki ja UPD?

Azure RemoteApp salvestab Outlooki olek (postkastid PSTs) seansside vahel. Selle läheb vaja talletamise kasutaja profiili andmed PST (c:\users\<kasutajanimi >). Vaikeasukoht andmete, seega niikaua, kui muudate asukohta, andmete kestab seansside vahel.

Soovitame kasutada Outlooki "vahemälurežiim" ja "server/online" režiimi otsimiseks kasutada.

Lugege [artiklit](remoteapp-outlook.md) Outlooki ja Azure RemoteApp kasutamise kohta lisateabe saamiseks.

## <a name="what-about-redirection"></a>Kuidas on lood ümbersuunamise?
Saate konfigureerida Azure RemoteApp lasta kasutajatel juurdepääs kohalikud seadmed [ümber suunamine](remoteapp-redirection.md)häälestades. Kohalikud seadmed saab seejärel klõpsake soovitud UPD andmetele juurdepääsuks.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Saate kasutada minu UPD nagu võrgukettal?
Ei. UPDs ei saa kasutada võrgu osa. Mõne UPD on kasutajale saadaval ainult siis, kui kasutaja on ühendatud aktiivselt Azure RemoteApp.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Kui kasutaja kustutamine kollektsioonist, nende UPD kustutada?

Ei, kui kasutaja kustutamine, me automaatselt kustutada selle UPD - selle asemel salvestada andmed, kuni kustutamiseni kogumist. 90 päeva pärast seda, kui kustutate selle saidikogumi me kustutada kõik UPDs. 

Kui teil on vaja mõnda UPD kollektsioonist kustutamine, võtke ühendust Azure RemoteApp - saame kustutada UPD meie poolelt.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Pääseb kasutajate UPDs (praeguse- või kustutatud kasutajad)?

Jah, kui [Azure RemoteApp](mailto:remoteappforum@microsoft.com)poole me saate häälestada saate URL-andmetele juurdepääsuks. Saate alla laadida või kõik failid on UPD enne juurdepääs lõppemist tuleb teil umbes 10 tundi.

## <a name="are-upds-available-offline"></a>On UPDs võrguühenduseta režiimis saadaval?

Praegu me ei anna võrguühenduseta juurdepääsu UPDs, et lisaks 10 tund Accessi akna eespool kirjeldatud. See tähendab, et meil on praegu võimalus teile juurdepääsu kauaks piisavalt keerulisem toimingute tegemiseks, nt töötab viirusetõrjetarkvara on UPDs või auditi andmetele juurde pääseda.

## <a name="do-registry-key-settings-persist"></a>Ei jää püsima võtme registrisätete?
Jah, midagi kirjutatud HKEY_Current_User on osa selle UPD.

## <a name="can-i-disable-upds-for-a-collection"></a>Kas saab keelata UPDs kogumi jaoks?

Jah, saate esitada Azure RemoteApp keelamiseks UPDs tellimuse jaoks, kuid ei saa te seda ise teha. See tähendab, et UPDs keelatakse tellimuse kõigi saidikogumite jaoks.

Võite soovida keelamine UPDs mis tahes järgmistes olukordades: 

- Juurdepääs ja haldamine kasutajaandmete lõpuleviimist peate (jaoks audit ja vaadake üle näiteks rahandus õppeasutuste).
- Teil on 3-osaline Kasutaja halduse lahenduste kohapealse profiil ja soovite neid oma domeeni ühendatud Azure RemoteApp juurutuse kasutada. See nõuab profiili agent laaditakse kuldmedali pilti. 
- Te ei pea iga kohalike andmete talletamine või teil on kõik andmed pilve või faili ühiskasutusse andmine ja soovite juhtelemendi andmete kohalikult kasutades Azure RemoteApp salvestamine.

Lisateabe saamiseks vaadake [Keelamine kasutaja profiili ketta (UPDs) jaotise Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) .

## <a name="can-i-restrict-users-from-saving-data-to-the-system-drive"></a>Kaudu andmete salvestamine draivi, kuhu saate piirata kasutajatel?

Jah, kuid peate häälestamiseks mis malli pilti enne selle saidikogumi luua. Järgmiste juhiste abil saate blokeerida juurdepääsu süsteemi juhtida:

1. Käivitage **gpedit.msc** malli pilti.
2. Liikuge **Kasutaja konfiguratsioon > mallide haldus > Windowsi komponentide > Exploreri**.
3. Valige järgmised suvandid:
    - **Need määratud draivid minu arvuti peitmine**
    - **Juurdepääsu vältimiseks draivid minu arvutist**

## <a name="can-i-seed-upds-i-want-to-put-some-data-in-the-upd-thats-available-the-first-time-the-user-signs-in"></a>Saate ma seeme UPDs? Soovin luua mõned andmed UPD, mis on saadaval kasutaja esimest korda.

Jah, kui olete loonud malli pilti, saate lisada teavet vaikeprofiili. See teave lisatakse selle UPD.

## <a name="can-i-change-the-size-of-the-upd-depending-on-how-much-data-i-want-to-store"></a>UPD sõltuvalt sellest, kui palju andmeid, mida soovin salvestada suurust saab muuta?

Ei, kõik UPDs on 50 GB salvestusruumi. Kui soovite talletada erinevate andmehulkade, proovige järgmist.

1. Keelake UPDs kogumi.
2. Failide ühiskasutamine kasutajatele juurdepääsu häälestamine.
3. Faili ühiskasutus laadimine käivitamisel skripti abil. Käivitus-skriptid Azure RemoteApp üksikasjad allpool.
4. Otse kasutajate kõik andmed salvestada faili ühiskasutusse anda.


## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Kuidas ma saan startup skripti Azure RemoteApp?

Kui soovite käivitada startup skripti, Alustuseks ajastatud loomine malli pilti kavatsete selle saidikogumi jaoks kasutada. (Tehke seda *enne* käivitate sysprep.) 

![Süsteemi ülesande loomine](./media/remoteapp-upd/upd1.png)

![Mis käivitub, kui kasutaja logib süsteemi tööülesande loomine](./media/remoteapp-upd/upd2.png)

Klõpsake vahekaardi **üldist** olla kindel, et muuta **Kasutajakonto** jaotises Turve, et "BUILTIN\Users."

![Muuta kasutajakonto lisamine rühma](./media/remoteapp-upd/upd4.png)

Ajastatud ülesanne käivitab käivitus skripti, kasutades kasutaja mandaat. Plaanimine toimingu käivitada iga kord, kui kasutaja sisse logib.

![Määratud tööülesande päästik nagu "Logi"](./media/remoteapp-upd/upd3.png)

Saate kasutada ka [rühmapoliitika alusel käivitus-skriptid](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx). 

## <a name="what-about-placing-a-startup-script-in-the-start-menu-would-that-work"></a>Kuidas on lood paigutamine startup skripti menüü Start? Mis toimiks?

Teisisõnu, saate luua .bat fail, mis töötab config akna skripti ja salvestage see c:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp kausta ja seejärel on iga kord, kui kasutaja alustab RemoteApp seansi käivitamine skripti?

Ei, mis ei toeta Azure RemoteApp, mis kasutab RDSH, mis ei toeta ka käivitus-skriptid menüüs Start.

## <a name="can-i-use-mstscexe-the-remote-desktop-program-to-configure-logon-scripts"></a>Kasutada mstsc.exe (kaugtöölaua programmi) sisselogimise skriptide konfigureerimiseks?

Ei, ei toeta Azure RemoteApp.

## <a name="can-i-store-data-on-the-vm-locally"></a>Saate salvestada VM kohalikult andmeid?

EI, kui VM on UPD suvalist talletatud andmed lähevad kaotsi. On suur võimalus kasutaja ei saa sama VM järgmise aega, mida nad logige Azure RemoteApp. Me ei säilita kasutaja-VM püsimine, nii, et kasutajal on pole sama VM sisse logida ja andmed lähevad kaotsi. Lisaks, kui selle saidikogumi värskendamiseks olemasoleva VMs on asendatud VMs - uus komplekt, mis tähendab, et ise VM andmed lähevad kaotsi. Soovitus on UPD, nt failiserverisse sees on VNET või pilveteenuse salvestusruumi süsteemi nagu Dropboxi abil pilve Azure failid ühiskasutusega salvestusruumi andmete talletamiseks.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Kuidas seinale on Azure failikettalt VM, PowerShelli kaudu?

Saate ühendada ketas, järgmiselt Net-PSDrive cmdlet-käsk:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


Lisaks saate salvestada oma kasutajanimi ja parool, käitades järgmine.

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Mis saab vahele jätta – mandaati parameeter cmdlet-käsu New-PSDrive.
