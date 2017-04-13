<properties
    pageTitle="Looge taastamise | Microsoft Azure'i" 
    description="Luua taastamine Azure'i saidi taastamine ei õnnestu üle ja virtuaalmasinates ja füüsilise serveri taastada." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="create-recovery-plans"></a>Taastamise loomine

Azure'i saidi taastamise teenuse aitab teie ettevõtte järjepidevus ja avariijärgne taastamine (BCDR) strateegia orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates ja füüsilise serveri. Masinad saab paljundada Azure'i või sekundaarne asutusesisese andmekeskuse. Kiire ülevaate saamiseks lugege [mis on Azure saidi taastamise?](site-recovery-overview.md).


## <a name="overview"></a>Ülevaade

Sellest artiklist leiate teavet loomise ja kohandamise taastamise kohta. 

Taastamise koosnevad järjestatud rühmad, mis sisaldavad virtuaalmasinates või füüsilise serveri, mida soovite kasutada koos. Taastamise tehke järgmist.

- Määratle rühmad, mis ei õnnestu üle ja seejärel käivitada koos.
- Mudeli sõltuvusi masinad koondamine nende taastamise leping rühma. Jaoks näiteks, kui soovite ei õnnestu üle ja avab konkreetse rakenduse oleks rühmitamiseks virtuaalmasinates taastamise leping samasse rakenduse.
- Automatiseerimine ja laiendamine Tõrkesiirde. Käivitada test, kavandatud, või planeerimata Tõrkesiirde taastamis. Saate kohandada taastamine lepingute skriptide, Azure automatiseerimine ja käsitsi toimingud.

Taastamise kuvatakse **Taastamise** saidi taastamine portaalis.


Postitada kommentaare või küsimusi allosas käesoleva artikli või [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Enne alustamist

Võtke arvesse järgmist.

- Taastamis ei tohi segada ühe või mitme võrguadapteri VMs. Seda sellepärast, et need VMs segamine pole lubatud Azure pilveteenuses.
- Saate laiendada taastamise plaanid skripte ja käsitsi toimingud. Võtke arvesse järgmist.
    - Kirjutage skriptide Windows PowerShelli kaudu.
    - Veenduge, et skriptide kasutamine proovige kaaspüük plokid, et erandid lahendatakse nõtkelt. Kui skripti erandiks on see lakkab töötamast ja tööülesanne kuvatakse nurjus.  Tõrke ilmnemisel skript ülejäänud osa ei tööta. Kui see juhtub, kui teil on planeerimata Tõrkesiirde, jäävad taastamise kava. Kui teil on kavandatud Tõrkesiirde sellisel juhul katkestab taastamise kava. Sellisel juhul muuta skript, veenduge, et see töötab ootuspäraselt ja seejärel käivitage uuesti taastamise kava.
    - Käsu täitmine-Host ei tööta taastamise leping skripti ja skripti ei õnnestu. Kui soovite luua väljundi, luua puhverserveri skripti, mis omakorda töötab teie peamine skripti ja veenduge, et kõik väljund on veetorustiku, kasutades funktsiooni >> käsk.
    - Skripti ajalõpp, kui see ei tagasta 600 sekunditega.
    - Kui midagi on kirjutatud STDERR, skripti saab liigitada nurjus. See teave kuvatakse skripti täitmise üksikasjad.
    - Kui kasutate juurutamise VMM Pange tähele järgmist.

        - Skriptide taastamis käivitage VMM teenusekonto kontekstis. Veenduge, et see konto on Loe õigusi skripti asub remote võrgukettal ja testida skripti käivitada VMM teenuse konto õiguste tase.
        - Windows PowerShelli moodul edastatakse VMM cmdlet-käsud. VMM Windows PowerShelli moodul on installitud, kui installite VMM konsooli. VMM mooduli saab laadida skripti abil skript järgmine käsk: impordi-moodul-virtualmachinemanager nimi. [Rohkem üksikasju](hhttps://technet.microsoft.com/library/hh875013.aspx).
        - Veenduge, et teil on vähemalt ühte teeki server juurutamise VMM. Vaikimisi asub kausta nimi MSCVMMLibrary serveris VMM kohalikult teegi ühiskasutus tee VMM Server.
        - Kui teie teegis ühiskasutus tee on remote (või kohaliku, kuid mitte ühiskasutusse MSCVMMLibrary, konfigureerida ühiskasutus järgmiselt (abil \\libserver2.contoso.com\share\ näide):
            - Avage registriredaktor ja liikuge HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure saidi Recovery\Registration.
            -  ScriptLibraryPath väärtus Redigeeri ja paigutage väärtuse, mis \\libserver2.contoso.com\share\. määrata täielik FQDN. Sisestage õiguste ühiskasutuse kohta.
            -  Veenduge, et testida skript kasutaja kontoga, mis on samad juurdepääsuõigused VMM teenusekonto, veenduge, et eraldiseisev testita skriptide käivitamine on taastamise samal viisil. Serveris VMM seada täitmise poliitika mööda hiilida järgmiselt:
                -  Avage 64-bitine Windows PowerShelli konsooli abil administraatoriõigusi.
                -  Tüüp: **Set-executionpolicy mööduda**. [Rohkem üksikasju](https://technet.microsoft.com/library/ee176961.aspx).

## <a name="create-a-recovery-plan"></a>Taastamis loomine

Saate luua taastamis viis sõltub juurutuse saidi taastamine.

- **Imitatsiooniga VMware VMs ja füüsilise serveri**– saate luua ja lisada dispersioonanalüüs rühmad, mis sisaldavad virtuaalmasinates ja füüsiliste serverite taastamis.
- **Imitatsiooniga Hyper-V VMs (VMM pilved)**– saate plaani loomine ja lisamine VMM cloud kaitstud Hyper-V virtuaalmasinates taastamis.
- **Imitatsiooniga Hyper-V VMs (mitte VMM pilved)**– plaani loomine ja lisamine kaitstud Hyper-V virtuaalmasinates taastamis kaitse rühmast.
- **SAN dispersioonanalüüs**– plaani loomine ja lisamine dispersioonanalüüs rühm, mis sisaldab virtuaalmasinates taastamine lepingule. Valite rühma kopeerimise asemel teatud virtuaalmasinates, kuna kõik virtuaalmasinates dispersioonanalüüs rühma peab ei õnnestu üle koos (Tõrkesiirde kihis salvestusruumi esmalt).


Luua taastamis järgmiselt:

1. Klõpsake vahekaarti **Taastamine lepingud** > **Loo taastamise kava**.
Määrake taastamine lepingut, ja lähte- ja sihtsaitide jaoks nimi. Andmeallika server peab olema virtuaalmasinates Tõrkesiirde ja taastamise jaoks lubatud.

    - Kui te olete imitatsiooniga VMM VMM valige **Andmeallika tüüp** > **VMM**ja lähte- ja VMM serverid. Klõpsake **Hyper-V** pilved, mis on konfigureeritud kasutama Hyper-V koopia kuvamiseks. 
    - Kui te olete imitatsiooniga VMM VMM SAN valige **Andmeallika tüüp** > **VMM**ja lähte- ja VMM serverid. Klõpsake **SAN** pilved, mis on konfigureeritud SAN kopeerimise kuvamiseks.
    - Kui te olete imitatsiooniga VMM Azure valige **Andmeallika tüüp** > **VMM**.  Valige allikas VMM server ja **Azure** sihtkohas.
    - Kui te olete imitatsiooniga saidilt Hyper-V **Andmeallika tüübi**valimine > **Hyper-V saidi**. Valige saidi lähte- ja **Azure **mis Sihtvaluuta.
    - Kui olete imitatsiooniga VMware või füüsilise kohapealses serveris Azure, valige serveri konfiguratsioon lähte- ja **Azure** mis Sihtvaluuta

2. **Valige virtuaalmasinates** valige virtuaalmasinates (või kopeerimine rühma) lisamiseks vaikerühma (rühma 1) taastamise kava soovitud.

## <a name="customize-recovery-plans"></a>Taastamise kohandamine

Kui olete lisanud virtuaalmasinates või dispersioonanalüüs rühmade taastamise leping vaikerühma ja loomise leping, saate seda kohandada:

- **Lisa uute rühmade**– saate lisada täiendavad taaste leping rühmad. Rühmad on nummerdatud selles järjestuses, milles te neid lisada. Saate lisada kuni seitse rühmad. Uute rühmade saate lisada rohkem masinad või dispersioonanalüüs rühmad. Pange tähele, et virtuaalse masina või kopeerimine rühma saab ainult lisada ühe taastamise leping rühma.
- **Skripti lisamine **– saate lisada skriptide, et enne või pärast plaani rühma. Skripti lisamisel see lisatakse uus komplekt rühma toimingud. Näiteks luuakse eelse juhised rühma 1 kogumi nimi: rühma 1: eelse juhiseid. Kõigi eelse toimingute tegemiseks on loetletud selleks väärtuseks sees. Pange tähele, saate lisada ainult skripti esmane kohapeal, kui teil on VMM serveris juurutatud.
- **Käsitsi toimingu lisamine**– saate lisada käsitsi toiminguid, mida käivitada enne või pärast rühma taastamine leping. Taastamise kava käitamisel lakkab kohas, kus te lisatud käsitsi toimingu ja dialoogiboks, mis palub teil määrata, et käsitsi toiming on lõpule.

## <a name="extend-recovery-plans-with-scripts"></a>Taastamine lepingute skripte laiendamine

Saate lisada oma taastamise kava skripti:

- Kui teil on VMM lähtesaidi, saate luua skripti VMM Server ja lisada selle oma taastamise kava.
- Kui te olete imitatsiooniga Azure'i Azure'i automaatika tegevusraamatud saate integreerida oma taastamise kava

### <a name="create-a-vmm-script"></a>Luua skripti VMM


Luua skripti järgmiselt:

1. Uue kausta loomine teegi ühiskasutus, näiteks \<VMMServerName > \MSSCVMMLibrary\RPScripts. Selle paigutamine allikas ja suunata VMM serverites.
2. Luua skripti (nt RPScript) ja kontrollige, kas see töötab ootuspäraselt.
3. Skripti soovitud asukohta paigutada \<VMMServerName > \MSSCVMMLibrary lähte- ja VMM serverites.

### <a name="create-an-azure-automation-runbook"></a>Saate luua ka Azure automatiseerimine käitusjuhendi

Saate pikendada oma taastamise kava käivitades on Azure automatiseerimine käitusjuhendi raames leping. [Lisateavet vt](site-recovery-runbook-automation.md).


### <a name="add-custom-settings-to-a-recovery-plan"></a>Lisada kohandatud sätteid taastamis

1. Avage taastamise leping, millele soovite kohandada.
2. Klõpsake soovitud virtuaalmasinates lisamiseks või uue rühma.
3. Käsitsi või skripti lisamiseks toimingu **samm** loendis mõnda üksust ja klõpsake **skripti** või **Käsitsi toimingut**. Määrake, kas soovite lisada skripti või enne või pärast valitud üksusega toimingu. Nupud **Nihuta üles** ja **Nihuta alla** käsu abil saate skripti nihutamine üles või alla.
4. Kui lisate VMM skripti, valige **Tõrkesiirde VMM skripti**ja **Skripti** Path tippige suhteline tee ühiskasutus. Jah, siis Käesolevas näites, kus asub ühiskasutus veebilehel \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, Määrake tee: \RPScripts\RPScript.PS1.
5. Kui lisate mõne Azure automatiseerimine Käivita vihik, määrata **Azure automatiseerimine konto** , kus asub käitusjuhendi ja valige sobiv **Azure'i Käitusjuhendi skripti**.
5. Tehke Tõrkesiirde taastamise kava tagamaks, et skript töötab ootuspäraselt.


## <a name="next-steps"></a>Järgmised sammud

Saate kasutada erinevat tüüpi failovers taastamise kava, sh test Tõrkesiirde keskkonna ja kavandatud või planeerimata failovers kontrollida. [Lisateavet leiate teemast](site-recovery-failover.md).


 
