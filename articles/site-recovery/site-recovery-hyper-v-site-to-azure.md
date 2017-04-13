<properties
    pageTitle="Korrata (ilma VMM) Hyper-V virtuaalmasinates Azure'i Azure portaali Azure saidi taastamise abil | Microsoft Azure'i"
    description="Kirjeldab, kuidas juurutada Azure saidi taastamist korraldab dispersioonanalüüs, Tõrkesiirde ja kohapealse Hyper-V VMs pole haldab VMM Azure'i portaalis Azure taastamine"
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
    ms.date="09/19/2016"
    ms.author="raynew"/>


# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure-using-azure-site-recovery-with-the-azure-portal"></a>Korrata (ilma VMM) Hyper-V virtuaalmasinates Azure'i Azure portaali Azure saidi taastamise abil

> [AZURE.SELECTOR]
- [Azure'i portaal](site-recovery-hyper-v-site-to-azure.md)
- [Azure'i klassikaline](site-recovery-hyper-v-site-to-azure-classic.md)
- [PowerShelli - ressursihaldur](site-recovery-deploy-with-powershell-resource-manager.md)



Tere tulemast Azure'i saidi taastamine! Selles artiklis, kui soovite ise kohapealse Hyper-V virtuaalse masinad selle **pole** hallatava System Center Virtuaalmasinates Manager (VMM) pilved Azure kasutamine. Selles artiklis kirjeldatakse, kuidas häälestada kopeerimise abil Azure saidi taastamine Azure'i portaalis.

> [AZURE.NOTE] Azure'i on kaks eri [juurutamise mudelite](../resource-manager-deployment-model.md) loomise ja ressursside töötamine: Azure'i ressursihaldur ja klassikaline. Azure'i on kaks portaalide – Azure klassikaline portaali, mis toetab klassikaline juurutamise mudeli ja Azure portaali nii juurutamise mudelid tugi.

> Azure'i saidi taastamine toetab taastamine ja Azure Hyper-V virtuaalmasinates migreerimine. Selles artiklis kirjeldatud juhised kehtivad ühtemoodi konfigureerimisel dispersioonanalüüs Azure avariitaastet või migreerimine VMs Azure'i

Azure'i saidi taastamine Azure'i portaalis pakub mitmesuguseid uusi funktsioone:

- Funktsiooni Azure portaali, Azure varukoopia ja Azure saidi taastamise teenuste kombineeritakse ühe taastamise teenused vault nii, et saate häälestada ja hallata järjepidevuse ja avariitaastet (BCDR) ühest kohast. Ühendatud armatuurlaua võimaldab teil jälgimine ja haldamine toiminguid kõigis oma kohapealse saitide ja Azure avaliku pilve.
- Azure'i tellimused ette valmistatud programmiga pilve lahenduse pakkuja (CSP) kasutajad saavad nüüd hallata saidi taastamise toimingute Azure'i portaalis.
- Saidi taastamine Azure'i portaalis saab paljundada masinad ressursihaldur salvestusruumi kontod. Veebisaidil Tõrkesiirde, loob saidi taastamine Azure'i ressursihaldur-põhise VMs.
- Klassikaline salvestusruumi kontod ja Tõrkesiirde VMs klassikaline juurutamise näidise dispersioonanalüüs toetab jätkuvalt saidi taastamine.


Pärast lugemist selle artikli postituse allosas Disqus märkuste tagasiside. Tehniline küsimuste [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)


## <a name="overview"></a>Ülevaade


Peavad organisatsioonid BCDR strateegia, mis määrab, kuidas rakendused, töökoormus ja andmete püsida töötab ja kättesaadav ajal plaanitud ja plaanimata tööseisakute ja taastamiseks tavaline tingimuste nii kiiresti kui võimalik. BCDR strateegia kavandamine äriandmete turvalised ja saab hoida ja tagada töökoormus pidevalt katastroofi ilmnemisel.

Saidi taastamine on Azure'i teenus, mis aitab BCDR strateegia kavandamine, orkesteroinnin dispersioonanalüüs füüsilise kohapealsed serverid ja virtuaalmasinates pilveteenusesse (Azure) või teisene andmekeskusesse. Kui teie peamine asukoht esineda katkestuste, võib nurjuda üle teisene asukohta hoida rakendused ja töökoormus saadaval. Tagasi, et teie peamine asukoht võib nurjuda, kui see annab toiminguid. Lisateavet leiate [mis on Azure saidi taastamise?](site-recovery-overview.md)

Selles artiklis antakse kogu teavet, mida soovite korrata kohapealse Hyper-V virtuaalse masinad selle **pole** hallatava System Center Virtuaalmasinates Manager (VMM) pilved Azure. See sisaldab ülevaate arhitektuuri, teavet ja juurutamise kohapealsed serverid, Azure, dispersioonanalüüs poliitika ja võimsuse planeerimine konfigureerimise juhised kavandamine. Kui olete häälestanud infrastruktuuri lubada kopeerimine arvutites, mida soovite kaitsta ja teha test Tõrkesiirde valideerimiseks ülesseadmine. Saate ka migreerida oma VMs Azure'i esimese täitmisel kavandatud Tõrkesiirde ja seejärel täitke migreerimise.

## <a name="business-advantages"></a>Eelised ettevõtetele

- Annab väljapoole (Azure) Tõrkesiirde business töökoormus ja Hyper-V virtuaalmasinates töötavad rakendused.
- Lihtne häälestamise ja haldamise dispersioonanalüüs, Tõrkesiirde ja taastamise protsesside pakub ühe taastamise teenused konsooli.
- Võimaldab teil hõlpsalt käivitada failovers oma kohapealse taristu Azure, ja fail tagasi (taastamine), Azure asutusesisese saidile.
- Saate konfigureerida taastamise koos mitme masinad, nii, et astmeline rakenduse töökoormus ei õnnestu üle koos.

## <a name="scenario-architecture"></a>Stsenaarium arhitektuur

Need on stsenaarium komponendid.

- **Hyper-V hosti või kobar**: kohapealse Hyper-V hosti serverite või kogumite. Saidi taastamine juurutamisel kogutakse loogiline Hyper-V saitide Hyper-V hosts töötab VMs, mida soovite kaitsta.
- **Azure taastamise pakkujalt ja taastamise teenused agendi**: juurutamisel installimist Azure saidi taastamise pakkuja ja Microsoft Azure taastamise teenused agendi Hyper-V hosti serverites. Pakkuja suhtleb Azure saidi taastamine HTTPS 443 korraldamise kopeerida. Agent Hyper-V hosti server tiražeerib andmete Azure storage üle HTTPS 443 vaikimisi.
- **Azure'i**: peate tellimuse Azure, Azure storage konto poe kopeeritud andmete ja on Azure virtuaalse võrgu nii, et Azure VMs on pärast Tõrkesiirde võrku ühendatud.

![Hyper-V saidi arhitektuur](./media/site-recovery-hyper-v-site-to-azure/architecture.png)



## <a name="azure-prerequisites"></a>Azure'i eeltingimused

Siin on, mida peate Azure juurutamine seda stsenaariumi.

**Nõutav** | **Üksikasjad**
--- | ---
**Azure'i konto**| Peate [Microsoft Azure'i](http://azure.microsoft.com/) kontosse. Saate alustada [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). [Lisateavet leiate teemast](https://azure.microsoft.com/pricing/details/site-recovery/) saidi taastamine hindade kohta.
**Azure'i salvestusruum** | Peate kindlad konto. Saate kasutada LRS või GRS salvestusruumi konto. Soovitame GRS nii, et andmed on olles piirkondliku katkestuste ilmnemisel või kui esmane piirkond ei saa taastada. [Lisateavet leiate teemast](../storage/storage-redundancy.md). Konto peab olema sama piirkonna vault taastamise teenused.<br/><br/> Premium mälu ei toetata.<br/><br/> Kopeeritud andmed on talletatud Azure salvestusruumi ja Azure VMs luuakse Tõrkesiirde korral.<br/><br/> [Lugege](../storage/storage-introduction.md) Azure'i salvestusruumi.
**Azure'i võrgu** | Peate Azure virtuaalse võrgu, mis Azure VMs loob ühenduse Tõrkesiirde korral. Azure virtuaalse võrgu peab olema sama piirkonna vault taastamise teenused.

## <a name="on-premises-prerequisites"></a>Kohapealse eeltingimused

Siin on teil vaja kohapealse.

**Nõutav** | **Üksikasjad**
--- | ---
**Hyper-V** | Ühe või mitme kohapealse serverites, kus töötab **Windows Server 2012 R2** uusimaid värskendusi ja Hyper-V roll lubatud või **Microsoft Hyper-V Server 2012 R2**.<br/><br/>Hyper-V server peaks sisaldama vähemalt ühte virtuaalmasinates.<br/><br/>Hyper-V serverite peaks Internetiga otse või puhverserveri kaudu.<br/><br/>Hyper-V serverite peaks olema mainitud [KB2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977") installitud parandused.
**Pakkuja ja agent** | Azure'i saidi taastamine juurutamisel installimist Azure saidi taastamise pakkuja. Pakkuja installimise installib Azure taastamise Services Agent iga Hyper-V serveris virtuaalmasinates, mida soovite kaitsta. Kõik Hyper-V serverid saidi taastamise hoidla peaks olema sama versioon pakkuja ja agent.<br/><br/>Pakkuja tuleb ühenduse Azure saidi taastamise Interneti kaudu. Liikluse saab saata otse või puhverserveri kaudu. Pange tähele, et HTTPS-i põhiste puhverserveri ei toetata. Puhverserveri peaks juurdepääsu lubamiseks tehke järgmist. <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blog.core.windows.net <br/><br/> *Store.Core.Windows.net <br/><br/> https://www.msftncsi.com/NCSI.txt<br/><br/>Kui teil on serveri IP-aadress-põhiste tulemüüri reeglid, veenduge, et reegleid andnud Azure'i teatis. Peate lubama [Azure'i andmekeskuse IP-vahemikke](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja pordi HTTPS (443).<br/><br/>Luba IP-aadresside vahemikud tellimuse Azure piirkond ja Lääne US.

## <a name="protected-machine-prerequisites"></a>Kaitstud arvuti eeltingimused


**Nõutav** | **Üksikasjad**
--- | ---
**Kaitstud VMs** | Enne, kui teil ei õnnestu üle VM peate veenduge, et nimi, mis määratakse Azure VM vastab [Azure eeltingimused](site-recovery-best-practices.md#azure-virtual-machine-requirements). Nime saate muuta, kui olete lubanud dispersioonanalüüs VM.<br/><br/> Üksikute ketta maht kaitstud masinad ei tohiks üle 1023 GB. VM võib olla kuni 16 ketast (seega 16 TB).<br/><br/> Ühiskasutuses ketta Külastajate kogumite ei toetata.<br/><br/> Kui allika VM on NIC lubatud teisendatakse see pärast Tõrkesiirde Azure ühe NIC.<br/><br/>Kaitse VMs töötab Linux staatiline IP-aadress ei toetata.

## <a name="prepare-for-deployment"></a>Juurutamise ettevalmistamine

Ettevalmistamiseks juurutamiseks peate:

1. [Azure'i võrgu määramine](#set-up-an-azure-network) , kus Azure VMs paigutatakse kui need on loodud pärast Tõrkesiirde.
2. Kopeeritud andmed [Azure storage konto häälestamine](#set-up-an-azure-storage-account) .
3. [Ettevalmistamine Hyper-V hosts](#prepare-the-hyper-v-hosts) tagamaks, et nad pääsevad vajalik URL-id.

### <a name="set-up-an-azure-network"></a>Azure'i võrgu

Määrake Azure võrgu. Peate seda, et Azure VMs, mis on loodud pärast Tõrkesiirde on võrku ühendatud.

- Võrgu peaks olema sama piirkonna, kus kuvatakse juurutamist taastamise teenused vault üks.
- Mudelist ressursi soovite kasutada üle Azure VMs nurjus, saate häälestada Azure võrgu [ressursihaldur režiimi](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) või [klassikaline režiim](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Soovitame häälestada võrku enne alustamist. Kui te pole teil vaja seda teha saidi taastamine juurutamisel.

> [AZURE.NOTE] [Võrgu migreerimine](../resource-group-move-resources.md) üle ühe tellimuse ressursi rühma või tellimustes võrke kasutatakse juurutamine saidi taastamine ei toetata.

### <a name="set-up-an-azure-storage-account"></a>Azure'i salvestusruumi konto häälestamine

- Peate standard Azure storage konto Azure kopeeritud andmete mahutamiseks.
- Mudelist ressursi soovite kasutada üle Azure VMs nurjus, kuvatakse konto sätestatud [Ressursihaldur](../storage/storage-create-storage-account.md) [klassikalise režiimi](../storage/storage-create-storage-account-classic-portal.md)või.
- Soovitame häälestamine salvestusruumi konto enne alustamist. Kui te pole teil vaja seda teha saidi taastamine juurutamisel. Kontode peavad olema sama piirkonna vault taastamise teenused.

> [AZURE.NOTE] [Migreerimise salvestusruumi kontode](../resource-group-move-resources.md) üle ühe tellimuse ressursi rühma või tellimustes salvestusruumi kontod kasutatakse juurutamine saidi taastamine ei toetata.

### <a name="prepare-the-hyper-v-hosts"></a>Hyper-V hosts ettevalmistamine

- Veenduge, et Hyper-V hosts vastavad [eeltingimused](#on-premises-prerequisites).

### <a name="create-a-recovery-services-vault"></a>Taastamise teenused vault loomine

1. [Azure'i portaali](https://portal.azure.com)sisse logida.
2. Klõpsake nuppu **Uus** > **halduse** > **varundamine ja taastamine saidi (OMS)**. Teise võimalusena võite klõpsata **sirvida** > **Taastamise teenused** võlvid > **Lisa**.

    ![Uue vault](./media/site-recovery-hyper-v-site-to-azure/new-vault3.png)

3. Määrake **nimi** sõbralik nimi, mis tähistavad vault. Kui teil on mitu tellimust, valige üks neist.
4. [Uue ressursirühma Loo](../resource-group-template-deploy-portal.md) või valige olemasoleva ja määrake Azure piirkond. Masinad on kopeeritud selle alale. Märkige ruut toetatud regioonide leiate [Azure'i saidi taastamise hinnad üksikasjad](https://azure.microsoft.com/pricing/details/site-recovery/) geograafilised saadavus
4. Kui soovite kiiresti juurde vault armatuurlaualt klõpsake käsku **Kinnita armatuurlaua** ja klõpsake **Loo vault**.

    ![Uue vault](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

Kuvatakse uus vault **armatuurlaua** > **kõik ressursid**, ja peamine **taastamise teenused võlvid** tera.

## <a name="getting-started"></a>Alustamine

Saidi taastamine pakub on alustamine aitab teil juurutada nii kiiresti kui võimalik. Alustamine eeltingimused kontrollib ja juhendab teid saidi taastamine juurutamise etappide õiges järjestuses.

Alustamine valige tüüp masinad, mida soovite kopeerida, ja kuhu soovite korrata. Saate häälestada kohapealsed serverid, Azure salvestusruumi kontod ja võrgu. Dispersioonanalüüs poliitikate loomine ja teha võimsuse planeerimine. Kui olete häälestanud oma taristu lubate dispersioonanalüüs vms. Saate käivitada failovers teatud masinad või luua taastamine ei õnnestu üle mitme masinad.

Alustage alustamine, valides, kuidas soovite juurutada saidi taastamine. Alustamine voogu muudab veidi vastavalt oma vajadustele kopeerimine.



## <a name="step-1-choose-your-protection-goals"></a>Samm 1: Valige kaitse eesmärgid.

Valige mida soovite korrata ja kuhu soovite korrata.

1. Valige oma vault **taastamise teenused võlvid** tera ja klõpsake nuppu **sätted**.
2. **Sätted** > **Alustamine** nuppu **Saidi taastamine** > **Samm 1: ettevalmistamine taristu** > **kaitse eesmärk**.

    ![Valige eesmärgid](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)

3. Valige **Abil Azure'i** **kaitse eesmärk** ja valige **Jah, mille Hyper-V**. Valige **ei** kinnitada, et te ei kasuta VMM. Klõpsake nuppu **OK**.

    ![Valige eesmärgid](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Samm 2: Allika keskkonna häälestamine

Hyper-V saidi häälestamine, installida Azure saidi taastamise pakkuja ja Azure taastamise teenused agendi Hyper-V hosts ja autoriloomingut hosts registreerida.


1. Klõpsake **Samm 2: ettevalmistamine taristu** > **Allikas**. Lisada uue saidi Hyper-V ümbris Hyper-V hosts või kogumite, klõpsake nuppu **+ Hyper-V saidi**.

    ![Allika häälestamine](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)

2. Määrake **saidi loomine Hyper-V** tera saidi nimi. Klõpsake nuppu **OK**. Valige äsja loodud sait.

    ![Allika häälestamine](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. Klõpsake nuppu **+ Hyper-V Server** server saidile lisada.
4. **Lisage**serveri > **serveri tüüp** , märkige ruut, et **Hyper-V server** kuvatakse. Veenduge, et Hyper-V server, mille soovite lisada vastab [eeltingimused](#on-premises-prerequisites) ja pääseb juurde määratud URL-id.
4. Azure taastamise pakkujalt installi faili alla laadida. Te käivitate selle faili installida nii pakkuja ja taastamise teenused agendi iga Hyper-V hosti.
5. Laadige alla registreerimise võti. Peate selle käivitamisel häälestus. Võti kehtib 5 päeva pärast selle loomist.

    ![Allika häälestamine](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)

6. Käivitage pakkuja installifail iga lisatud Hyper-V saidi hosti. Kui installite Hyper-V klaster, käivitage installiprogramm iga kobar sõlme. Installimine ja registreerimine iga Hyper-V klaster sõlmed tagab, et virtuaalmasinates jäävad kaitstud isegi juhul, kui nad migreerimine sõlmed üle.

### <a name="install-the-provider-and-agent"></a>Pakkuja ja agent installimine

1. Käivitage pakkuja installifail.
2. **Microsoft** Update'i saate valite värskendusi, et pakkuja värskendused on installitud vastavalt teie Microsoft Update poliitika.
3. **Installi** aktsepteerida või pakkuja installimise vaikeasukoha muutmiseks ja klõpsake nuppu **Installi**.
4. Klõpsake lehe **Vault sätted** , **sirvige** soovitud vault võtme faili alla laadida. Määrake Azure saidi taastamise tellimuse, vault nime ja Hyper-V sait, mis Hyper-V server kuulub.

    ![Server registreerimine](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5. **Puhvrisätete** määrata, kuidas pakkuja, mis installitakse serveris loob ühenduse Azure saidi taastamise Interneti kaudu.

- Kui soovite ühendada otse pakkuja valige **ühendus otse ilma proxy**.
- Kui soovite luua ühenduse puhverserveri, mis on seadistatud server valige **Loo ühendus olemasoleva puhverserveri sätted**.
- Kui teie olemasoleva puhverserver nõuab autentimist või soovite kasutada kohandatud puhverserveri pakkuja ühenduse valimine **kasutajaga kohandatud puhverserveri sätted**.
- Kui kasutate kohandatud puhverserveri aadressi, portide ja mandaadi määramiseks peate
- Kui kasutate puhverserveri tegemine kindel kirjeldatud [eeltingimused](#on-premises-prerequisites) URL-id on lubatud läbi.

    ![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6. Kui installimine on lõpule viidud klõpsake **registreerida** autoriloomingut serveri registreerimiseks.

![Installi asukoht](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7 kui registreerimine on lõpule viidud metaandmete Hyper-v server tuuakse Azure saidi taastamine ja serveri **sätted**kuvatakse > **Saidi taastamise taristu** > **Hyper-V Hosts** tera.


### <a name="command-line-installation"></a>Käsurea installimine

Azure taastamise pakkujalt ja agent saab installida ka järgmised käsurea kaudu. See meetod saab installida pakkuja lisamine Server Core jaoks Windows Server 2012 R2.

1. Laadige pakkuja installimise faili ja registreerimise võti kausta. Näiteks C:\ASR.
2. Alates käsuviipa, käivitage need eraldada pakkuja Installeri käsud:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Käivitage see käsk komponentide installimine.

            C:\ASR> setupdr.exe /i

4. Käivitage registreerida autoriloomingut serveri need käsud: CD C:\Program Files\Microsoft Azure saidi taastamine Provider\
            C:\Program Files\Microsoft Azure taastamise pakkujalt\> DRConfigurator.exe /r /Friendlyname <friendly name of the server> /mandaatide<path of the credentials file>

<br/>
Kui:

- **/Credentials** : kohustuslik parameeter, mis määrab, kus asub registreerimise võtme faili asukoht  
- **/FriendlyName** : kohustuslik parameeter Hyper-V hosti server Azure'i saidi taastamine portaalis kuvatavat nime.
- **/proxyAddress** : valikuline parameeter, mis määrab puhverserveri aadressi.
- **/proxyport** : valikuline parameeter, mis määrab puhverserveri pordi.
- **/proxyUsername** : valikuline parameeter, mis määrab puhverserveri kasutajanimi (kui puhverserver nõuab autentimist).
- **/proxyPassword** : valikuline parameeter, mis määrab parooli koos puhverserveri autentimine (kui puhverserver nõuab autentimist).


## <a name="step-3-set-up-the-target-environment"></a>Samm 3: Target keskkonna häälestamine

Määrake Azure storage konto, mida soovite kasutada kopeerimise ja Azure võrk, millele Azure VMs pärast Tõrkesiirde ühendada.

1.  Klõpsake **ettevalmistamine taristu** > **Target** ja valige soovitud Azure'i tellimus, mida soovite kasutada.
2.  Määrata, mida soovite kasutada vms pärast Tõrkesiirde juurutamise mudeli.
3.  Saidi taastamine kontrollib, kas teil on ühe või mitme ühilduvad Azure salvestusruumi kontod ja võrgu.

    ![Salvestusruumi](./media/site-recovery-hyper-v-site-to-azure/select-target.png)

4.  Kui te pole loonud salvestusruumi konto ja te soovite luua ressursihaldur abil klõpsake nuppu **+ salvestusruumi** konto teha seda teksti sees. Määrake **Loo salvestusruumi konto** enne konto nimi, tüüp, tellimus ja asukoht. Konto peaks olema taastamise teenused vault samasse asukohta.

    ![Salvestusruumi](./media/site-recovery-hyper-v-site-to-azure/gs-createstorage.png)

    Kui soovite luua salvestusruumi konto abil klassikaline, mida saate teha selle [Azure'i portaalis](../storage/storage-create-storage-account-classic-portal.md).

5.  Kui te pole loonud mõne Azure võrgu ja te soovite luua kasutamise ressursihaldur nuppu **+ võrgu** teha seda teksti sees. **Loo virtuaalse võrgu** enne määrata võrgu nimi, aadress vahemikus, alamvõrgu üksikasjad, tellimus ja asukoht. Võrgu peaks olema taastamise teenused vault samasse asukohta.

    ![Võrgu](./media/site-recovery-hyper-v-site-to-azure/gs-createnetwork.png)

    Kui soovite luua võrgu klassikaline teen selle [Azure'i portaalis](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).


## <a name="step-4-set-up-replication-settings"></a>Samm 4: Häälestamine dispersioonanalüüs sätted

1. Uue koopia loomiseks klõpsake poliitika **ettevalmistamine infrastruktuur** > **Dispersioonanalüüs sätted** > **+ loomine ja seostada**.

    ![Võrgu](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)

2. **Loomine ja sidusettevõtte poliitika** määrata poliitika nimi.
3. **Kopeerige sagedus** Määrake kui sageli soovite korrata delta andmete pärast algse dispersioonanalüüs (iga 30 sekundi järel, 5 või 15 minutit).
4. **Taastamine osutage säilitamine**, määrake mõne tunni pärast kaua säilituspoliitika akna saab iga taastamine punkti. Kaitstud masinad taastada mis tahes punkti aken.
6. Määrake **rakenduse ühtsete hetktõmmise sagedus** tihti (1 – 12 tundi) taastamise punkte, mis sisaldavad rakenduse ühtsete hetktõmmiseid saab luua. Hyper-V kasutab kahte tüüpi hetktõmmiseid – standard pildi, mis pakub on kogu virtuaalse masina suureneva hetktõmmise ja rakenduse ühtsete hetktõmmise, mis viib rakenduse andmete virtual seadmes hetktõmmise punkti õigel ajal. Rakenduse ühtsete hetktõmmiseid Kasuta helitugevuse vari Service (VSS) on rakenduste ühtsete olekus kui hetktõmmis. Märkus Kui lubate rakenduse ühtsete pilte, mõjutavad jõudlus töötavad allikast virtuaalmasinates rakendused. Veenduge, et seate väärtus on väiksem kui arvu täiendavad taaste suunab teid konfigureerimine.
3. Määrake **Algne dispersioonanalüüs alguskellaaeg** Millal algse dispersioonanalüüs alustada. Funktsiooni dispersioonanalüüs ilmneb üle Interneti-läbilaskevõime nii, et võiksite plaanida selle väljaspool teie hõivatud tundi. Klõpsake nuppu **OK**.

    ![Poliitika dispersioonanalüüs](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Kui loote uue poliitika on automaatselt seotud Hyper-V saidiga. Klõpsake nuppu **OK**. Saidi Hyper-V (ja selle VM) saate seostada mitme dispersioonanalüüs poliitikate **sätted** > **Dispersioonanalüüs** > poliitika nimi > **Hyper-V saidi seostada**.

## <a name="step-5-capacity-planning"></a>Juhis 5: Võimsuse planeerimine

Nüüd, kui teil on oma basic taristu saate häälestada saate mõelda võimsuse planeerimine ja aru saada, kas teil on vaja täiendavaid ressursse.

Saidi taastamine pakub võimsus Plaanur aitavad eraldada õige ressursside allika keskkonna, saidi taastamise komponendid, võrgunduse ja salvestusruumi. Saate käivitada soovitud Plaanur kiirrežiimis valmisväärtusi põhinevad keskmine arv VMs, ketast ja salvestusruumi või üksikasjalik režiimis, kus saate sisestada töökoormus tasandi andmed. Enne alustamist peate:

- Koguge teavet dispersioonanalüüs keskkonna, sh VMs kohta VMs ketast ja ketta salvestusruumi.
- Hindamiseks igapäevane muutmine (piimapütt) peate kopeeritud andmed. [Võimsus Plaanur Hyper-V koopia jaoks](https://www.microsoft.com/download/details.aspx?id=39057) abil saate seda teha.

1.  Klõpsake tööriista alla laadida ja seejärel käivitage see **alla laadida** . [Lugege artiklit](site-recovery-capacity-planner.md) , mis kaasneb tööriist.
2.  Kui olete lõpetanud valige **Jah** **on käivitate võimsus Plaanur**?

    ![Võimsuse planeerimine](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Võrgu läbilaskevõime kasutuse

Võimsus Plaanur tööriista abil saate arvutada vajalikku läbilaskevõimet kopeerimise (algse kopeerimise ja seejärel delta). Kontrollida läbilaskevõime kasutuse kopeerimise summa on teil mitu võimalust.

- **Throttle läbilaskevõime**: Azure'i tiražeerib Hyper-V liiklus läheb läbi kindlate Hyper-V server. Saate throttle läbilaskevõime hosti server.
- **Kohandada efektisuvandite läbilaskevõime**: saate mõjutada kopeerimise abil registrivõtmete paar kasutatavat ribalaiust.

#### <a name="throttle-bandwidth"></a>Ahendamise läbilaskevõime

1. Avage Microsoft Azure'i varundus MMC lisandmooduli Hyper-V hosti server. Vaikimisi on saadaval töölaual või C:\Program Files\Microsoft Azure taastamise teenused Agent\bin\wabadmin otsetee Microsoft Azure varukoopia.
2. Lisandmooduli nuppu **Muuda atribuute**.
3. Vahekaardil **Throttling** valige **Luba Interneti läbilaskevõime kasutuse varukoopia toimingute pidurdamise**, ja töö piirangud ja mitte-töö tundi. Lubatud vahemikud on 512 Kbps 102 Mbps sekundis.

    ![Ahendamise läbilaskevõime](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

Cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) abil pidurdamise määramine. Siin on näide:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting-NoThrottle** näitab, et pole pidurdamise on nõutav.


#### <a name="influence-network-bandwidth"></a>Võrgu läbilaskevõime mõju

1. Liikuge registris **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure'i Backup\Replication**.
    - Mõjutada läbilaskevõime liikluse imitatsiooniga kettal, väärtus **UploadThreadsPerVM**muuta, või luua võti, kui seda pole.
    - Mõjutada failback liikluse Azure läbilaskevõime, muutke väärtus **DownloadThreadsPerVM**.
2. Vaikeväärtus on 4. "Overprovisioned" võrgus registrivõtmete muuta oma vaikeväärtused. Maksimumväärtus on 32. Jälgida liikluse optimeerida väärtus.

## <a name="step-6-enable-replication"></a>Samm 6: Luba dispersioonanalüüs

Nüüd lubada kopeerimine järgmiselt:

1. Klõpsake **Samm 2: rakenduse ise** > **Allikas**. Kui olete lubanud dispersioonanalüüs esimest korda võite nuppu **+ ise** võlvkelder lubamiseks dispersioonanalüüs arvutite jaoks.

    ![Lubada kopeerimine](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)

2. **Andmeallika** tera > valige Hyper-V sait. Klõpsake nuppu **OK**.
3. Valige **Target** vault tellimus ja Tõrkesiirde mudelit, mida soovite kasutada pärast Tõrkesiirde Azure (klassikaline või ressursside haldamine).
4. Valige salvestusruumi konto, mida soovite kasutada. Kui soovite kasutada erinevaid salvestusruumi konto, kui teil on te saate [luua](#set-up-an-azure-storage-account). Ressursihaldur näidise konto storage loomiseks klõpsake nuppu **Loo uus**. Kui soovite luua salvestusruumi konto abil klassikaline, mida saate teha selle [Azure'i portaalis](../storage/storage-create-storage-account-classic-portal.md). Klõpsake nuppu **OK**.
5.  Valige Azure võrgu- ja millele loob ühenduse Azure VMs, kui need on kedratud pärast Tõrkesiirde alamvõrgu. Valige võrgu sätte rakendamiseks kõik masinad valite kaitse **nüüd jaoks valitud masinad konfigureerimine** . Valige **Konfigureeri hiljem** Azure võrgu masina kohta. Kui soovite kasutada erinevate võrgu mandaadist peate te saate [luua](#set-up-an-azure-network). Ressursihaldur mudeli võrgu loomiseks klõpsake nuppu **Loo uus**. Kui soovite luua võrgu klassikaline teen selle [Azure'i portaalis](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Valige soovitud alamvõrgu võimalusel. Klõpsake nuppu **OK**.

    ![Lubada kopeerimine](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. **Virtuaalmasinates** > **Valige virtuaalmasinates** klõpsake ja valige igas arvutis, mida soovite korrata. Saate valida ainult masinad dispersioonanalüüs saate lubatud. Klõpsake nuppu **OK**.

    ![Lubada kopeerimine](./media/site-recovery-hyper-v-site-to-azure/enable-replication5.png)

11. **Atribuutide** > **atribuutide konfigureerimine**, valige operatsioonisüsteem valitud VMs ja OS ketas. Veenduge, et Azure'i VM nimi (sihtrakenduse nimi) vastab [Azure virtuaalse masina nõuetele](site-recovery-best-practices.md#azure-virtual-machine-requirements) ja muuta seda, kui teil on vaja. Klõpsake nuppu **OK**. Täiendavad atribuudid saate määrata hiljem.

    ![Lubada kopeerimine](./media/site-recovery-hyper-v-site-to-azure/enable-replication6.png)

12. **Dispersioonanalüüs**sätted > **sätete konfigureerimine dispersioonanalüüs**, valige dispersioonanalüüs poliitika, millele soovite rakendada kaitstud vms. Klõpsake nuppu **OK**. Saate muuta dispersioonanalüüs rühmapoliitika **sätted** > **Dispersioonanalüüs poliitikad** > poliitika nimi > **Muuda sätteid**. Mis on juba imitatsiooniga, ja uus kasutatakse muudatuste rakendamist.

    ![Lubada kopeerimine](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

Saate jälgida edenemist **Lubamine kaitse** töö **sätted** > **töö** > **saidi taastamise tööd**. Pärast **Viimistlemine kaitse** töö töötab seade on valmis Tõrkesiirde.

### <a name="view-and-manage-vm-properties"></a>Saate vaadata ja hallata VM atribuudid

Me soovitame, et veenduda masina andmeallika atribuudid.

1. Klõpsake nuppu **sätted** > **Kaitstud üksuste** > **Kopeeritud üksused** > ja valige seade.

    ![Lubada kopeerimine](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)

2. **Atribuudid** kuvatakse kopeerimise ja Tõrkesiirde teabe VM.

    ![Lubada kopeerimine](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)

3. **Arvuta**ja võrgu > **arvutada atribuudid** saate määrata Azure VM nimi ja sihtkoht suurus. Azure'i nõuete täitmiseks, kui teil on vaja nime muuta. Saate vaadata ja muuta teavet target võrgus, alamvõrgu ja IP-aadress, mis määratakse Azure VM. Võtke arvesse järgmist.

    - Saate seada target IP-aadress. Kui te ei esita aadressi nurjunud üle arvuti kasutab DHCP. Kui seate aadressi, mis pole saadaval Tõrkesiirde, on tõrkesiire nurjub. Sama target IP-aadressi saab kasutada test Tõrkesiirde kui aadress on test Tõrkesiirde võrgus saadaval.
    - Võrguadapteri arv on tingitud suurus saate määrata target virtuaalse masina, järgmiselt:

        - Kui allika arvutisse võrguadapteri arv on väiksem või võrdne arv adapterit masina sihtformaat lubatud, siis sihtkohas on samal hulgal adapterit allikana.
        - Kui adapterit allika virtual masina arv ületab lubatud target suurus ja seejärel soovitud suurus maksimum kasutada number.
        - Näiteks kui allika arvuti on kaks võrguadapteri ja seadme sihtformaat toetab neli, sihtarvutis on kaks adapterit. Kui allika arvuti on kaks adapterit, kuid toetatud sihtformaat toetab ainult üks siis sihtarvutis on ainult üks adapterit.     
        - Kui VM on mitu võrguadapteri need kõik ühendada samasse võrku.

    ![Lubada kopeerimine](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

5.  **Ketast** näete operatsioonisüsteem ja andmete ketast VM, mis kuvatakse kopeeritud.


## <a name="step-7-test-the-deployment"></a>Juhis 7: Testige juurutamise

Juurutamise testimiseks käivitage test Tõrkesiirde ühe virtuaalse masina või taastamise kava, mis sisaldab ühe või mitme virtuaalmasinates.


### <a name="prepare-for-test-failover"></a>Test Tõrkesiirde ettevalmistamine

- Käivitage test Tõrkesiirde soovitame luua uue Azure võrk, mis on eraldatud Azure tootmise võrgu (see on vaikekäitumine, kui loote uue võrgu Azure). [Lisateavet leiate teemast](site-recovery-failover.md#run-a-test-failover) kohta, kuidas testi failovers.
- Saada parimaid tulemusi, kui teil ei õnnestu üle Azure, installige Azure'i Agent kaitstud arvutisse. See muudab käivitamist kiiremini ja aitab tõrkeotsing. Installige [Linux](https://github.com/Azure/WALinuxAgent) või [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) agent.
- Juurutamise täielikult kontrollimiseks peate infrastruktuur tiražeeritud masina õigesti töötada. Kui soovite testida Active Directory ja DNS-i saate luua virtuaalse masina nimega domeenikontrolleri DNS- ja kopeerida see Azure'i Azure saidi taastamise abil. Lugege rohkem tekstivormingus [test Tõrkesiirde kaalutluste kohta Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Kui soovite käivitada planeerimata Tõrkesiirde asemel test Tõrkesiirde võtke arvesse järgmist.

    - Võimaluse korral tuleks sulgeda esmane masinad enne käivitamist planeerimata Tõrkesiirde. See tagab, et teil pole nii lähte-kui ka koopia masinad töötab samal ajal.
    - Kui käivitate planeerimata Tõrkesiirde lõpetab andmete kopeerimine esmane masinad nii, et kõik andmed delta ei üle pärast planeerimata Tõrkesiirde hakkab. Lisaks kui käivitate planeerimata Tõrkesiirde taastamis see töötab seni, kuni olete lõpetanud, isegi kui ilmneb tõrge.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Pärast Tõrkesiirde Azure VMs ühenduse ettevalmistamine

Kui soovite ühendada Azure VMs abil RDP pärast Tõrkesiirde, veenduge, et saate teha järgmist:

**Kohapealse arvutisse enne Tõrkesiirde**:

- Interneti kaudu juurdepääsu lubamine RDP, veenduge, et TCP- ja UDP reeglid lisatakse ka **avalik**ja veenduge, et **Windowsi tulemüür**on lubatud RDP -> **lubatud rakendused ja funktsioonid** kõigi profiilide.
- Saitide ühenduse kaudu juurdepääsu lubamine RDP arvutis ja veenduge, et **Windowsi tulemüür**on lubatud RDP -> **lubatud rakendused ja funktsioonid** **domeeni** ja **Privaatne** võrgustikke.
- Kohapealse arvutisse installida [Azure VM agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) .
- Veenduge, et operatsioonisüsteemi SAN poliitika on seatud väärtusele OnlineAll. [Lisateave]( https://support.microsoft.com/kb/3031135)
- Enne, kui käivitate selle Tõrkesiirde IPSec teenuse välja lülitada.

**Klõpsake the Azure VM pärast Tõrkesiirde**:

- Lisada Azure VM lubada RDP-ga seostatud NIC avaliku IP-aadressi.
- Veenduge, et teil pole mõnda domeeni poliitika, mis takistada ühenduse virtuaalse masina avalik aadressidel.
- Proovige ühendust luua. Kui te ei saa ühendust luua, veenduge, et VM töötab. Veel tõrkeotsingunäpunäiteid lugege selle [artikli](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Kui soovite juurdepääsu Azure VM, mis töötab Linux pärast Tõrkesiirde Secure Shell kliendi (ssh), tehke järgmist.

**Kohapealse arvutisse enne Tõrkesiirde**:

- Veenduge, et automaatselt käivituma süsteemi käivitamine on määratud Secure Shell teenuse Azure VM.
- Kontrollige, et lubada tulemüüri reeglid selle SSH ühendus.

**Klõpsake the Azure VM pärast Tõrkesiirde**:

- Klõpsake nurjunud VM ja Azure alamvõrgu, millega on ühendatud üle võrgu rühma reeglid tuleb lubada SSH pordi sissetulevaid ühendusi.
- Avaliku lõpp-punkti luua, et lubada sissetulevaid ühendusi SSH Port (TCP port 22 vaikimisi).
- Kui VM pääseb VPN-ühenduse kaudu (teekonna või -saidilt VPN) siis kliendi saab kasutada otse ühenduse loomiseks VM üle SSH.

### <a name="run-a-test-failover"></a>Käivitage test Tõrkesiirde

Käivitamiseks test Tõrkesiirde tehke järgmist:

1. Nurjumise üle ühe VM **sätted** > **Kopeeritud üksused**, klõpsake VM > **+ Test Tõrkesiirde**.

    ![Test Tõrkesiirde](./media/site-recovery-hyper-v-site-to-azure/run-failover1.png)

2. Kasutada taastamise kava **sätted** > **Taastamine lepingud**, paremklõpsake leping > **Test Tõrkesiirde**. Luua enam kavandamine, [järgige neid juhiseid](site-recovery-create-recovery-plans.md).

3. Valige **Test Tõrkesiirde** Azure võrgu, millega on ühendatud Azure VMs, kui Tõrkesiirde.

    ![Test Tõrkesiirde](./media/site-recovery-hyper-v-site-to-azure/run-failover2.png)

4. Klõpsake nuppu **OK** , et alustada selle Tõrkesiirde. Saate jälgimiseks, klõpsates avada oma properies VM või **Test Tõrkesiirde** töö **sätted** > **saidi taastamise tööd**.
5. **Täielik testimine** etapp jõudmisel on tõrkesiire tehke järgmist.
    1. Saate vaadata koopia virtuaalse masina Azure'i portaalis. Veenduge, et virtuaalse masina õnnestub.
    2. Kui te pole Accessi virtuaalmasinates kuni määramine kohapealse võrgu kaudu saate algatada virtuaalse masina kaugtöölaua ühendus.
    3. Klõpsake nuppu **täielik test** seda lõpetada.
    4. Klõpsake nuppu **märkmed** salvestada ja salvestada märkusi, mis on seotud test Tõrkesiirde.
    5. Klõpsake nuppu **testi Tõrkesiirde on lõpule viidud**. Puhasta testimiskeskkonnas, automaatselt väljalülitamise ja kustutada testi virtuaalse masina.
    6. Selles etapis mis tahes elemente või saidi taastamine test Tõrkesiirde ajal automaatselt loodud VMs kustutatakse. Teie loodud test Tõrkesiirde täiendavad andmed ei kustutata.

    > [AZURE.NOTE] Kui test Tõrkesiirde kestab kauem kui kaks nädalat lõpuleviimist jõuga.

6. Pärast selle Tõrkesiirde lõpulejõudmist saate ka peaks olema võimalus leiate Azure'i koopia arvutisse kuvatakse Azure'i portaalis > **Virtuaalmasinates**. Veenduge, et VM on sobiv suurus, mis see on sobiv võrku ühendatud ja see töötab.
7. Kui te [pärast Tõrkesiirde ühenduste valmis](#prepare-to-connect-to-Azure-VMs-after-failover) teiega ühendust luua Azure VM.


## <a name="failover"></a>Tõrkesiirde

Kui algset kopeerimine on lõpule jõudnud oma masinad, saab käivitada failovers vastavalt vajadusele. Saidi taastamine toetab erinevat tüüpi failovers - Test Tõrkesiirde, kavandatud Tõrkesiirde ja planeerimata Tõrkesiirde.
[Lisateavet leiate teemast](site-recovery-failover.md) kohta erinevat tüüpi failovers ja millal ja kuidas teha iga üksikasjalik kirjeldus.

> [AZURE.NOTE] Kui teie eesmärk on migreerida Azure'i virtuaalmasinates, soovitame kasutada Azure'i virtuaalmasinates soovitud migreerimiseks on [plaanitud Tõrkesiirde toiming](site-recovery-failover.md#run-a-planned-failover-primary-to-secondary) . Kui migreeritud rakendus on kinnitatud Azure test Tõrkesiirde abil, siis järgige nimetatud [Täieliku migreerimise](#Complete-migration-of-your-virtual-machines-to-Azure) oma virtuaalmasinates migreerimise lõpule viimiseks. Pole vaja teha kinnitamine või Kustuta. Täieliku migreerimise migreerimine on lõpule jõudnud, eemaldab virtuaalse masina kaitse ja lõpetab masina Azure saidi taastamise arveldus.


###<a name="run-an-unplanned-failover"></a>Planeerimata Tõrkesiirde käivitamine

See tuleb valida, kui esmane saidi muutub kättesaamatuks ootamatu juhtum, nt power katkestuste või viiruse rünnak. See toiming kirjeldab käivitamise 'planeerimata Tõrkesiirde' taastamis jaoks. Teise võimalusena võite käivitada Tõrkesiirde ühe virtuaalse masina jaoks Virtuaalmasinates menüü. Enne alustamist veenduge, et soovite kasutada virtuaalmasinates lõpetanud algse dispersioonanalüüs.

1. Valige **taastamine lepingud > recoveryplan_name**.
2. Tagastamise leping tera, klõpsake nuppu **Planeerimata Tõrkesiirde**.
3. Valige lehel **planeerimata Tõrkesiirde** lähte- ja asukohad.
4. Valige **virtuaalmasinates sulgeda ja nende sünkroonimine uusimaid andmeid** määramiseks saidi taastamine peaks proovite sulgeda kaitstud virtuaalmasinates ja sünkroonida, et andmed uusim versioon on nurjunud üle.
5. Pärast Tõrkesiirde, on selle virtuaalmasinates on Kinnita ootele.  Klõpsake nuppu **Kinnita** on tõrkesiire kinnitamiseks.

[Lisateave](site-recovery-failover.md#run-an-unplanned-failover)

## <a name="complete-migration-of-your-virtual-machines-to-azure"></a>Täielik migratsioon oma Azure'i virtuaalmasinates


>[AZURE.NOTE] Järgmised juhised kehtivad ainult siis, kui migreerite Azure'i virtuaalmasinates

1. Tegutsemine kavandatud Tõrkesiirde mainitud [siin](site-recovery-failover.md)
2. Klõpsake **Sätted > kopeeritud üksuste**, paremklõpsake virtuaalse masina ja valige **Täielik migratsioon**

    ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

2. Klõpsake nuppu **OK** migreerimise lõpule viimiseks. Klõpsates VM selle atribuudid avamiseks või rakenduses töö valmis migreerimise abil saate jälgida edenemist **Sätted > saidi taastamise töö**.


## <a name="monitor-your-deployment"></a>Juurutamise jälgimine

Siin on, kuidas saate jälgida konfiguratsioonisätted, olek ja seisund oma saidi taastamine juurutamiseks.

1. Klõpsake vault nime juurdepääsu **Essentialsi** armatuurlaud. Armatuurlaua saate saidi taastamise tööde haldamine, kopeerimise olek, taastamise, serveri seisundi ja sündmusi.  Saate kohandada Essentialsi paanid ja paigutusi, mis on kõige kasulikumad teile, sh saidi varundus ja taaste võlvid oleku kuvamiseks.

    ![Essentialsi](./media/site-recovery-hyper-v-site-to-azure/essentials.png)

2. Paani **seisundit** saate jälgida saidi serverid, mis on tekkinud probleem ja sündmuste tõstatatud saidi taastamine viimase 24 tunni jooksul.
3. Saate hallata ja jälgida **Kopeeritud üksused**, **Taastamine lepingud**, kopeerimise ja **Saidi taastamise töö** paanid. Saate minna tööle **sätted** -> **töö** -> **Saidi taastamise tööd**.
