<properties
    pageTitle="Paljundada Hyper-V virtuaalmasinates VMM pilved Azure'i saidi taastamise abil Azure portaali | Microsoft Azure'i"
    description="Kirjeldab, kuidas juurutada Azure saidi taastamist korraldab dispersioonanalüüs, Tõrkesiirde ja Azure portaalis Azure Hyper-V vms VMM pilved taastamine"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-azure-site-recovery-with-the-azure-portal--microsoft-azure"></a>Paljundada Hyper-V virtuaalmasinates VMM pilved Azure'i Azure portaali Azure saidi taastamise abil | Microsoft Azure'i

> [AZURE.SELECTOR]
- [Azure'i portaal](site-recovery-vmm-to-azure.md)
- [Azure'i klassikaline](site-recovery-vmm-to-azure-classic.md)
- [PowerShelli ressursihaldur](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [PowerShelli klassikaline](site-recovery-deploy-with-powershell.md)

Tere tulemast Azure'i saidi taastamine! Selles artiklis, kui soovite ise kohapealse Hyper-V virtuaalmasinates hallatava System Center virtuaalse masina Manager (VMM) pilved Azure'i Azure saidi taastamise kasutamine Azure portaali kasutamine.

> [AZURE.NOTE]Azure'i on kaks eri [juurutamise mudelite](../resource-manager-deployment-model
> ) loomise ja ressursside töötamine: Azure'i ressursihaldur ja klassikaline. Azure'i on kaks portaalide – Azure klassikaline portaali, mis toetab klassikaline juurutamise mudeli ja Azure portaali nii juurutamise mudelid tugi.


Azure'i saidi taastamine Azure'i portaalis pakub mitmesuguseid uusi funktsioone.

- Azure'i portaalis kombineeritakse Azure varukoopia ja Azure saidi taastamise teenuseid ühe taastamise teenused võlvikus nii, et saate häälestada ja hallata järjepidevuse ja avariitaastet (BCDR) ühest kohast. Ühendatud armatuurlaua võimaldab teil jälgida ja hallata oma kohapealse saitide ja Azure avaliku pilve toimingud.
- Azure'i tellimused ette valmistatud programmiga pilve lahenduse pakkuja (CSP) kasutajad saavad nüüd hallata saidi taastamise toimingute Azure'i portaalis.
- Saidi taastamine Azure'i portaalis saab paljundada masinad Azure'i ressursihaldur salvestusruumi kontod. Veebisaidil Tõrkesiirde, loob saidi taastamine Azure'i ressursihaldur-põhise VMs.
- Klassikaline salvestusruumi kontode kopeerimine toetab jätkuvalt saidi taastamine. Veebisaidil Tõrkesiirde, loob saidi taastamine VMs klassikaline abil.


Pärast selle artikli lugemist postitada allosas Disqus kommentaaride kommentaarid. Tehniline küsimuste [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

## <a name="overview"></a>Ülevaade

Peavad organisatsioonid BCDR strateegia, mis määrab, kuidas rakendused, töökoormus ja andmete püsida töötab ja kättesaadav ajal plaanitud ja plaanimata tööseisakute ja taastamiseks tavaline tingimuste nii kiiresti kui võimalik. BCDR strateegia kavandamine peaks äriandmete turvalised ja saab hoida ja tagama töökoormus pidevalt katastroofi ilmnemisel.

Saidi taastamine on Azure'i teenus, mis aitab teie BCDR strateegia orkesteroinnin dispersioonanalüüs füüsilise kohapealsed serverid ja virtuaalmasinates pilveteenusesse (Azure) või teisene andmekeskusesse. Kui katkestuste esineda teie peamine asukoht, teil ei õnnestu üle teisene asukohta hoida rakendused ja töökoormus saadaval. Te ei tagasi oma peamine asukoht kui tagastab see toiminguid. Lisateavet leiate [mis on Azure saidi taastamise?](site-recovery-overview.md)

Selles artiklis antakse kogu teavet, mida soovite korrata kohapealse VMM pilved Azure Hyper-V VMs. See sisaldab ülevaate arhitektuuri, teavet ja juurutamise juhised konfigureerida Azure, kohapealsed serverid, dispersioonanalüüs sätted ja võimsuse planeerimine kavandamine. Kui olete häälestanud taristu, saate lubada dispersioonanalüüs arvutites, mida soovite kaitsta ja kontrollida selle Tõrkesiirde töötab.


## <a name="business-advantages"></a>Eelised ettevõtetele

- Saidi taastamine pakub väljapoole kaitse business töökoormus ja Hyper-V VMs töötavad rakendused.
- Taastamise teenused portaalis häälestada, hallata ja jälgida dispersioonanalüüs, Tõrkesiirde ja taastamine ühes kohas.
- Saate hõlpsalt käivitada failovers oma kohapealse taristu Azure ja migreerite (Taasta) Azure Hyper-V hosti serverid kohapealse saidi.
- Saate konfigureerida taastamise mitme nii, et astmeline rakenduse töökoormus ei õnnestu üle koos.


## <a name="scenario-architecture"></a>Stsenaarium arhitektuur


Need on stsenaarium komponendid.

- **VMM server**: ettevõtte kohapealse VMM serveris, mis ühe või mitme õhupalli.
- **Hyper-V hosti või kobar**: Hyper-V hosti serverite või kogumite hallatava VMM pilved.
- **Azure taastamise pakkujalt ja taastamise teenused agendi**: juurutamisel saate installida Azure saidi taastamise pakkuja VMM server ja Microsoft Azure taastamise teenused agendi Hyper-V hosti serverites. Pakkuja serveris VMM suhtleb saidi taastamine HTTPS 443 korraldamise kopeerida. Agent Hyper-V hosti server tiražeerib andmete Azure storage üle HTTPS 443 vaikimisi.
- **Azure'i**: peate tellimuse Azure, Azure storage konto poe kopeeritud andmete ja on Azure virtuaalse võrgu nii, et Azure VMs on pärast Tõrkesiirde võrku ühendatud.

![E2A topoloogia](./media/site-recovery-vmm-to-azure/architecture.png)


## <a name="azure-prerequisites"></a>Azure'i eeltingimused

Siin on, mida on vaja Azure juurutamine seda stsenaariumi.

**Nõutav** | **Üksikasjad**
--- | ---
**Azure'i konto**| Teil on vaja [Microsoft Azure'i](http://azure.microsoft.com/) kontosse. Saate alustada [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). [Lisateavet leiate teemast](https://azure.microsoft.com/pricing/details/site-recovery/) saidi taastamine hindade kohta.
**Azure'i salvestusruum** | Peate standard Azure storage konto kopeeritud andmete talletamiseks. Saate kasutada LRS või GRS salvestusruumi konto. Soovitame GRS nii, et andmed on olles piirkondliku katkestuste ilmnemisel või kui esmane piirkond ei saa taastada. [Lisateavet leiate teemast](../storage/storage-redundancy.md). Konto peab olema sama piirkonna vault taastamise teenused.<br/><br/>Premium mälu ei toetata.<br/><br/> Kopeeritud andmed on talletatud Azure salvestusruumi ja Azure VMs luuakse Tõrkesiirde korral. <br/><br/> [Lugege](../storage/storage-introduction.md) Azure'i salvestusruumi.
**Azure'i võrgu** | Teil on vaja Azure virtuaalse võrku ühendatud Azure VMs Tõrkesiirde korral. Võrgu peab olema sama piirkonna vault taastamise teenused.

## <a name="on-premises-prerequisites"></a>Kohapealse eeltingimused

Siin on vajalik kohapealse

**Nõutav** | **Üksikasjad**
--- | ---
**VMM**| Ühe või mitme VMM serverites, kus töötab System Center 2012 R2. Iga VMM server peaks olema üks või mitu pilved konfigureeritud. Pilv peaks sisaldama:<br/><br/> Ühe või mitme VMM hosti rühmad.<br/><br/> Ühe või mitme Hyper-V hosti serverite või kogumite iga host jaotises.<br/><br/>[Lisateavet leiate teemast](http://www.server-log.com/blog/2011/8/26/vmm-2012-and-the-clouds.html) VMM pilved häälestamise kohta.
**Hyper-V** | Hyper-V hosti serverid peab olema vähemalt opsüsteemi **Windows Server 2012 R2** Hyper-V roll või **Microsoft Hyper-V Server 2012 R2** ja olete installinud uusimaid värskendusi.<br/><br/> Hyper-V server peaks sisaldama vähemalt ühte VMs.<br/><br/> Hyper-V hosti server või kobar, mis sisaldab VMs, mida soovite korrata tuleb hallata VMM pilveteenusesse.<br/><br/>Hyper-V serverite peaks Internetiga otse või puhverserveri kaudu.<br/><br/>Hyper-V serverite peaks olema mainitud artiklis [2961977](https://support.microsoft.com/kb/2961977) installitud parandused.<br/><br/>Hyper-V hosti serverid on vaja andmete kopeerimise Azure Interneti-ühendus.
**Pakkuja ja agent** | Azure saidi taastamise juurutamisel saate installida Azure saidi taastamise pakkuja VMM serveris ja taastamise teenused agendi Hyper-V hosts. Pakkuja ja agent vaja luua ühendus Azure'i otse internetis või puhverserveri kaudu. Pange tähele, et mõne HTTPS-põhiste puhverserveri ei toetata. Puhverserveri VMM server ja Hyper-V hosts peaks juurdepääsu lubamiseks tehke järgmist. <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blob.core.windows.net <br/><br/> *. store.core.windows.net<br/><br/>Kui teil on IP-aadress-põhiste tulemüüri reeglid VMM-i server, veenduge, et reegleid andnud Azure'i teatis. Peate esmalt lubama [Azure'i andmekeskuse IP-vahemikke](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja pordi HTTPS (443).<br/><br/>Luba IP-aadresside vahemikud tellimuse Azure piirkond ja Lääne US.<br/><br/>Lisaks. puhverserveri VMM Server vajab juurdepääsu https://www.msftncsi.com/ncsi.txt


## <a name="protected-machine-prerequisites"></a>Kaitstud arvuti eeltingimused


**Nõutav** | **Üksikasjad**
--- | ---
**Kaitstud VMs** | Enne, kui teil ei õnnestu üle VM, veenduge, et nimi, mis on määratud Azure VM vastab [Azure eeltingimused](site-recovery-best-practices.md#azure-virtual-machine-requirements). Nime saate muuta, kui olete lubanud dispersioonanalüüs VM. <br/><br/> Üksikute ketta maht kaitstud masinad ei tohiks üle 1023 GB. VM võib olla kuni 16 ketast (seega 16 TB).<br/><br/> Ühiskasutuses ketta Külastajate kogumite ei toetata.<br/><br/> Ühendatud laiendatav püsivara Interface (UEFI) / laiendatav püsivara Interface(EFI) buutimine ei toetata.<br/><br/> Kui allika VM on NIC lubatud teisendatakse see pärast Tõrkesiirde Azure ühe NIC.<br/><br/>Kaitse VMs töötab Linux staatiline IP-aadress ei toetata.

## <a name="prepare-for-deployment"></a>Juurutamise ettevalmistamine

Ettevalmistamiseks juurutamiseks peate:

1. [Azure'i võrgu määramine](#set-up-an-azure-network) , kus Azure VMs paigutatakse pärast Tõrkesiirde.
2. Kopeeritud andmed [Azure storage konto häälestamine](#set-up-an-azure-storage-account) .
4. Saidi taastamine juurutamiseks [ettevalmistamine VMM server](#prepare-the-vmm-server) .
5. [Võrgu kaardistamine ettevalmistamine](#prepare-for-network-mapping). Häälestage võrgu nii, et saate konfigureerida võrgu kaardistamine saidi taastamine juurutamisel.

### <a name="set-up-an-azure-network"></a>Azure'i võrgu

Peate mõne Azure võrgu nii, et Azure VMs, mis on loodud pärast Tõrkesiirde ühenduse seda.

- Võrgu peaks olema sama piirkonna taastamise teenused vault juurutada üks.
- Mudelist ressursi soovite kasutada üle Azure VMs nurjus, saate häälestada Azure võrgu [ressursihaldur režiimi](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) või [klassikaline režiim](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Soovitame häälestada võrku enne alustamist. Kui te pole, peate saidi taastamine juurutamisel seda teha.

> [AZURE.NOTE] [Võrgu migreerimine](../resource-group-move-resources.md) üle ühe tellimuse ressursi rühma või tellimustes võrke kasutatakse juurutamine saidi taastamine ei toetata.


### <a name="set-up-an-azure-storage-account"></a>Azure'i salvestusruumi konto häälestamine

- Peate standard Azure storage konto Azure kopeeritud andmete mahutamiseks. Konto peab olema sama piirkonna vault taastamise teenused.
- Mudelist ressursi soovite kasutada üle Azure VMs nurjus, saate häälestada [Ressursihaldur](../storage/storage-create-storage-account.md) [klassikalise režiimi](../storage/storage-create-storage-account-classic-portal.md)või konto.
- Soovitame häälestada konto enne alustamist. Kui te pole, peate saidi taastamine juurutamisel seda teha.

> [AZURE.NOTE] [Migreerimise salvestusruumi kontode](../resource-group-move-resources.md) üle ühe tellimuse ressursi rühma või tellimustes salvestusruumi kontod kasutatakse juurutamine saidi taastamine ei toetata.

### <a name="prepare-the-vmm-server"></a>Ettevalmistused VMM server

- Veenduge, et VMM server vastab [eeltingimused](#on-premises-prerequisites).
- Saidi taastamine juurutamisel, saate määrata, et tuleb kõik parkimise VMM server Azure'i portaalis. Kui soovite üksnes teatud pilved portaalis kuvada, saate lubada seda sätet konsooli VMM haldus pilve.


### <a name="prepare-for-network-mapping"></a>Võrgu kaardistamine ettevalmistamine

Peate saate seadistada võrgu saidi taastamine juurutamisel. Võrgu kaardistamine kaartide vahel allika VMM VM võrkude ja suunata Azure võrkude lubamiseks järgmist:

- Masinad, mis ei õnnestu üle samasse võrku ühendamiseks, isegi juhul, kui need on nurjus üle samal viisil või sama lepingu taastamine.
- Kui sihtkohas Azure võrk on häälestatud võrgu lüüsi, Azure'i virtuaalmasinates saate luua ühenduse kohapealse virtuaalmasinates.
- Võrgu häälestamine vastendamise siin on vajalik ettevalmistamine.

    - Veenduge, et VMs allikas Hyper-V hosti server on ühendatud võrku VMM VM. Selle võrgu peaks olema seotud loogilise võrgu, mis on seotud pilve.
    - Muu Azure kaudu, nagu on kirjeldatud [ülal](#set-up-an-azure-network)

- [Lisateavet leiate teemast](site-recovery-network-mapping.md) võrgu kaardistamine tööpõhimõtete kohta.


## <a name="create-a-recovery-services-vault"></a>Taastamise teenused vault loomine

1. [Azure'i portaali](https://portal.azure.com)sisse logida.
2. Klõpsake nuppu **Uus** > **halduse** > **taastamise teenused**. Teise võimalusena võite klõpsata **sirvida** > **Taastamise teenused** võlvid > **Lisa**.

    ![Uue vault](./media/site-recovery-vmm-to-azure/new-vault3.png)

3. **Nimi**, määrake sõbralik nimi, mis tähistavad vault. Kui teil on mitu tellimust, valige üks neist.
4. [Ressursi rühma loomine](../resource-group-template-deploy-portal.md), või valige olemasoleva. Määrake Azure piirkond. Masinad on kopeeritud selle alale. Märkige ruut toetatud regioonide leiate [Azure'i saidi taastamise hinnad üksikasjad](https://azure.microsoft.com/pricing/details/site-recovery/) geograafilised saadavus
4. Kui soovite kiiresti juurde vault armatuurlaual, klõpsake käsku **Kinnita armatuurlaua** > **loomine vault**.

    ![Uue vault](./media/site-recovery-vmm-to-azure/new-vault-settings.png)

Uue vault kuvatakse **armatuurlaua** > **kõik ressursid**, ja peamine **taastamise teenused võlvid** tera.

## <a name="getting-started"></a>Alustamine

Saidi taastamine pakub on alustamine aitab teil juurutada nii kiiresti kui võimalik. Alustamine eeltingimused kontrollib ja juhendab teid saidi taastamine juurutamise etappide õiges järjestuses.

Alustamine valige tüüp masinad, mida soovite kopeerida, ja kuhu soovite korrata. Saate häälestada kohapealsed serverid, Azure salvestusruumi kontod ja võrgu. Dispersioonanalüüs poliitikate loomine ja teha võimsuse planeerimine. Kui olete häälestanud oma taristu lubate dispersioonanalüüs vms. Saate käivitada failovers teatud masinad või luua taastamine ei õnnestu üle mitme masinad.

Alustage alustamine, valides, kuidas soovite juurutada saidi taastamine. Alustamine voogu muudab veidi vastavalt oma vajadustele kopeerimine.



## <a name="step-1-choose-your-protection-goals"></a>Samm 1: Valige kaitse eesmärgid.

Valige mida soovite korrata ja kuhu soovite korrata.

1. Valige oma vault **taastamise teenused võlvid** tera, ja klõpsake nuppu **sätted**.
2. **Alustamine** nuppu **Saidi taastamine** > **Samm 1: ettevalmistamine taristu** > **kaitse eesmärk**.

    ![Valige eesmärgid](./media/site-recovery-vmm-to-azure/choose-goals.png)

3. Valige **Abil Azure'i** **kaitse eesmärk** ja valige **Jah, mille Hyper-V**. Valige **Jah** kinnitada, et kasutate VMM Hyper-V hosts ja taastamine saidi haldamiseks. Klõpsake nuppu **OK**.

    ![Valige eesmärgid](./media/site-recovery-vmm-to-azure/choose-goals2.png)



## <a name="step-2-set-up-the-source-environment"></a>Samm 2: Allika keskkonna häälestamine

Installige Azure'i saidi taastamise pakkuja VMM server ja registreerida serveri vault. Installige Azure'i taastamise teenused agendi Hyper-V hosts.

1. Klõpsake **Samm 2: ettevalmistamine taristu** > **Allikas**.

    ![Allika häälestamine](./media/site-recovery-vmm-to-azure/set-source1.png)

2. Klõpsake **andmeallika ettevalmistamine** **+ VMM** VMM server lisada.

    ![Allika häälestamine](./media/site-recovery-vmm-to-azure/set-source2.png)

3. **Lisada serveri** blade kontrolli **System Center VMM server** **serveri tüüp** ja mis kuvatavas VMM server vastab [eeltingimused ja URL-i nõuded](#on-premises-prerequisites).
4. Azure taastamise pakkujalt installi faili alla laadida.
5. Laadige alla registreerimise võti. Peate seda, kui käivitate häälestus. Võti kehtib 5 päeva pärast selle loomist.

    ![Allika häälestamine](./media/site-recovery-vmm-to-azure/set-source3.png)

6. Installige Azure'i saidi taastamise pakkuja VMM server.


### <a name="set-up-the-azure-site-recovery-provider"></a>Azure saidi taastamise pakkuja häälestamine

1.  Käivitage pakkuja installifail.
2. **Microsoft** Update'i saate valite värskendusi, et pakkuja värskendused on installitud vastavalt teie Microsoft Update poliitika.
3. **Installi**, nõustuge või pakkuja installimise vaikeasukoha muutmiseks ja klõpsake nuppu **Installi**.

    ![Installi asukoht](./media/site-recovery-vmm-to-azure/provider2.png)

4. Kui installi lõpuleviimist klõpsake **registreerida** registreerida VMM server autoriloomingut.
5. Lehe **Vault sätted** nuppu **Sirvi** vault võtme faili valimiseks. Määrake Azure saidi taastamise tellimus ja vault nimi.

    ![Server registreerimine](./media/site-recovery-vmm-to-azure/provider10.PNG)

6. **Interneti-ühendus**, määrata, kuidas serveris VMM pakkuja loob ühenduse saidi taastamine Interneti kaudu.

    - Valige soovitud otse ühenduse pakkuja **ühenduse otse Azure saidi taastamise ilma proxy**.
    - Kui teie olemasoleva puhverserver nõuab autentimist või soovite kasutada kohandatud puhverserveri valik **Azure saidi taastamise ühenduse abil puhverserverit**.
    - Kui kasutate kohandatud puhverserveri, määrake aadress, portide ja mandaadi.
    - Kui kasutate teil peaks juba lubatud URL-ide kirjeldatud [eeltingimused](#on-premises-prerequisites)proxy.
    - Kui kasutate kohandatud puhverserveri VMM RunAs konto (DRAProxyAccount) luuakse automaatselt määratud puhverserveri mandaadi abil. Puhverserveri konfigureerida nii, et selle konto saate edukalt autentimiseks. VMM RunAs konto sätteid saate muuta VMM konsooli. **Sätted**, laiendage **turvalisuse** > **Käivitada kontode**, ja seejärel muuta parooli DRAProxyAccount. Peate nii, et see säte jõustub VMM teenust uuesti.

    ![Internet](./media/site-recovery-vmm-to-azure/provider13.PNG)

7. Aktsepteerige või SSL-sert, mis luuakse automaatselt andmete krüptimiseks asukoha muutmine. Seda kasutatakse kui lubate Cloud, mis on kaitstud Azure'i Azure saidi taastamise portaalis andmete krüptimine. Selle serdi turvamiseks. Kui käivitate Azure Tõrkesiirde peate see lahti, kui andmete krüptimine on lubatud.


8. **Serveri nimi**Määrake tuvastamiseks VMM server autoriloomingut sõbralik nimi. Määrake kobar konfiguratsiooni, VMM kobar rolli nimi.
9. Luba **Sünkroonimine pilveteenuse metaandmed** , kui soovite sünkroonida kõik parkimise VMM server metaandmete vault. Seda toimingut tuleb ainult juhtub üks kord igale serveris. Kui te ei soovi sünkroonida kõik pilved, saate jätke see säte on märkimata ja sünkroonida iga pilv eraldi cloud atribuutide VMM konsooli. Klõpsake **registreerida** lõpule viia.

    ![Server registreerimine](./media/site-recovery-vmm-to-azure/provider16.PNG)

10. Algab registreerimine. Kui registreerimine on lõpule jõudnud, kuvatakse serveri **sätted** > **serverid** blade autoriloomingut.


#### <a name="command-line-installation-for-the-azure-site-recovery-provider"></a>Käsurea installi Azure saidi taastamise pakkuja

Käsurea kaudu saab installida Azure saidi taastamise pakkuja. See meetod saab installida Server Core Windows Server 2012 R2 pakkuja.

1. Laadige pakkuja installimise faili ja registreerimise võti kausta. Näiteks C:\ASR.
2. Alates käsuviipa, käivitage need eraldada pakkuja Installeri käsud:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Käivitage see käsk komponentide installimine.

            C:\ASR> setupdr.exe /i

4. Käivitage registreerida autoriloomingut serveri need käsud:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Kui:

- **/Credentials**: kohustuslik parameeter, mis määrab, kus registreerimise võtme fail asub.  
- **/FriendlyName**: kohustuslik parameeter Hyper-V hosti server Azure'i saidi taastamine portaalis kuvatavat nime.
- - **/EncryptionEnabled**: valikuline parameeter, kui te olete imitatsiooniga Hyper-V VMs VMM pilved Azure. Määrake, kas soovite krüptida Azure'i virtuaalmasinates (rest krüptimise) juures. Tagada, et faili nimi on **pfx** laiendamine. Krüptimise on vaikimisi välja lülitatud.
- **/proxyAddress**: valikuline parameeter, mis määrab puhverserveri aadressi.
- **/proxyport**: valikuline parameeter, mis määrab puhverserveri pordi.
- **/proxyUsername**: valikuline parameeter, mis määrab puhverserveri kasutajanimi (kui puhverserver nõuab autentimist).
- **/proxyPassword**: valikuline parameeter, mis määrab parooli kinnitamiseks puhverserver (kui puhverserver nõuab autentimist).


### <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Installige Azure'i taastamise teenused agent Hyper-V hosts

1. Kui olete häälestanud pakkuja, peate installi faili allalaadimiseks Azure taastamise teenused agendi. Installiprogramm iga Hyper-V server VMM pilveteenuses.

    ![Hyper-V saidid](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)

2. **Eeltingimused märkige** lehel klõpsake nuppu **edasi**. Vajaliku puudu installitakse automaatselt.

    ![Eeltingimused taastamine Services Agent](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)

3. Aktsepteerida või installi asukoht ja vahemälu asukoha muutmine lehel **Installimise sätted** . Saate konfigureerida vahemälu kettale, mis on vähemalt 5 GB salvestusruumi, kuid soovitame vahemälu ketas 600 GB või rohkem vaba ruumi. Klõpsake nuppu **Installi**.
4. Kui installimine on lõpule jõudnud, klõpsake nuppu **Sule** .

    ![Registri MARS Agent](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

#### <a name="command-line-installation-for-azure-site-recovery-services-agent"></a>Azure'i saidi taastamise teenused agendi käsurea installimine

Saate installida Microsoft Azure taastamise Services Agent käsureal järgmine käsk:

     marsagentinstaller.exe /q /nu

#### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Saidi taastamine Hyper-V hosts puhverserveri Interneti-ühenduse häälestamine

Töötab Hyper-V hosts taastamise teenused agent peab Interneti-ühendus Azure VM kopeerimise. Kui pääsete Interneti kaudu proxy, selle häälestada järgmiselt:

1. Avage Microsoft Azure'i varundus MMC lisandmooduli Hyper-V hosti. Vaikimisi on saadaval töölaual või C:\Program Files\Microsoft Azure taastamise teenused Agent\bin\wabadmin otsetee Microsoft Azure varukoopia.
2. Lisandmooduli nuppu **Muuda atribuute**.
3. Määrake vahekaardil **Puhverserveri konfigureerimine** puhverserveri info.

    ![Registri MARS Agent](./media/site-recovery-vmm-to-azure/mars-proxy.png)

4. Veenduge, et agent jõuavad kirjeldatud [eeltingimused](#on-premises-prerequisites)URL-id.


## <a name="step-3-set-up-the-target-environment"></a>Samm 3: Target keskkonna häälestamine

Määrake Azure storage konto, mida soovite kasutada kopeerimise ja Azure võrk, millele Azure VMs pärast Tõrkesiirde ühendada.

1.  Klõpsake **ettevalmistamine taristu** > **Target** ja valige soovitud Azure'i tellimus, mida soovite kasutada.
2.  Määrata, mida soovite kasutada vms pärast Tõrkesiirde juurutamise mudeli.
3.  Saidi taastamine kontrollib, kas teil on ühe või mitme ühilduvad Azure salvestusruumi kontod ja võrgu.

    ![Salvestusruumi](./media/site-recovery-vmm-to-azure/compatible-storage.png)

4.  Kui te pole loonud salvestusruumi konto ja te soovite luua abil ressursihaldur, klõpsake nuppu **+ salvestusruumi konto** teha seda teksti sees.  Määrake **Loo salvestusruumi konto** enne konto nimi, tüüp, tellimus ja asukoht. Konto peaks olema taastamise teenused vault samasse asukohta.

    ![Salvestusruumi](./media/site-recovery-vmm-to-azure/gs-createstorage.png)

    Pange tähele järgmist.

    - Kui soovite luua salvestusruumi konto abil klassikaline, saate teha Azure'i portaalis. [Lisateave](../storage/storage-create-storage-account-classic-portal.md)
    - Kui kasutate premium salvestusruumi konto kopeeritud andmed, peate häälestada konto salvestusruumi standard dispersioonanalüüs logid, mis jäädvustada poolelioleva muudatused – kohapealsete andmete talletamiseks.

4.  Kui te pole loonud mõne Azure võrgu ja te soovite luua kasutamise ressursihaldur nuppu **+ võrgu** teha seda teksti sees. **Loo virtuaalse võrgu** enne määrata võrgu nimi, aadress vahemikus, alamvõrgu üksikasjad, tellimus ja asukoht. Võrgu peaks olema taastamise teenused vault samasse asukohta.

    ![Võrgu](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

    Kui soovite luua klassikaline kasutades saate seda teha Azure'i portaalis. [Lisateavet leiate teemast](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Võrgu kaardistamine konfigureerimine

- [Lugege](#prepare-for-network-mapping) kiire ülevaade sellest, millised võrgu kaardistamine ei. [Lugege seda artiklit](site-recovery-network-mapping.md) põhjalikum selgitus.
- Veenduge, et virtuaalmasinates VMM Server on ühendatud VM võrk ja, et olete loonud vähemalt üks Azure virtuaalse võrgu. Mitme VM võrgu saab vastendada ühe Azure võrgus.

Konfigureerige vastendamise järgmiselt:

1. **Sätted** > **Saidi taastamise taristu** > **võrgu vastendused** > **Võrgu vastendamise**, klõpsake ikooni **+ võrgu vastendamise** .

    ![Võrgu kaardistamine](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. **Vastenduse lisamine võrgu** valige allikas VMM server ja **Azure** mis Sihtvaluuta.
3. Kontrollige tellimuse ja juurutamise mudeli pärast Tõrkesiirde.
4. Valige **Allikas võrgu**soovite vastendada loendis seotud VMM server kohapealse andmeallika VM võrgu.
5. **Target võrgus**, valige Azure võrk, mis koopia Azure VMs paigutatakse nende olete loomisel. Klõpsake nuppu **OK**.

    ![Võrgu kaardistamine](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Mis juhtub, kui võrgu kaardistamine algab siit.

- Olemasoleva andmeallika VM võrgu VMs on target võrku ühendatud vastenduse alustamisel. Uue andmeallika VM võrku ühendatud VMs on vastendatud Azure võrku ühendatud dispersioonanalüüs ilmnemisel.
- Kui muudate mõne olemasoleva võrgu kaardistamine, ühendatakse koopia virtuaalmasinates uute sätete abil.
- Kui target võrgus on mitu alamvõrku ja üks nende alamvõrku on alamvõrgu allika virtuaalse masina asub sama nimi, siis koopia virtuaalse masina ühendatakse selle target alamvõrgu Tõrkesiirde pärast.
- Kui pole target alamvõrgu kattuvad nimega, virtuaalse masina ühendatud esimese alamvõrgu võrgus.



## <a name="step-4-set-up-replication-settings"></a>Samm 4: Häälestamine dispersioonanalüüs sätted


1. Kopeerimine uue poliitika loomiseks klõpsake nuppu **ettevalmistamine taristu** > **Dispersioonanalüüs sätted** > **+ loomine ja seostada**.

    ![Võrgu](./media/site-recovery-vmm-to-azure/gs-replication.png)

2. **Loomine ja sidusettevõtte poliitika**, määrake poliitika nimi.
3. **Kopeerige sagedus**, määrake kui sageli soovite korrata delta andmete pärast algse dispersioonanalüüs (iga 30 sekundi järel, 5 või 15 minutit).
4. **Taastamine osutage säilitamine**, määrake mõne tunni pärast kaua säilituspoliitika akna saab iga taastamine punkti. Kaitstud masinad taastada mis tahes punkti aken.
6. **Rakenduse ühtsete hetktõmmise sagedus**, saate määrata, kui sageli (1 – 12 tundi) taastamise punkte, mis sisaldavad rakenduse ühtsete hetktõmmiseid saavad luua. Hyper-V kasutab kahte tüüpi hetktõmmiseid – standard pildi, mis pakub on kogu virtuaalse masina suureneva hetktõmmise ja rakenduse ühtsete hetktõmmise, mis viib rakenduse andmete virtual seadmes hetktõmmise punkti õigel ajal. Rakenduse ühtsete hetktõmmiseid Kasuta helitugevuse vari Service (VSS) on rakenduste ühtsete olekus kui hetktõmmis. Märkus Kui lubate rakenduse ühtsete pilte, mõjutavad jõudlus töötavad allikast virtuaalmasinates rakendused. Veenduge, et seate väärtus on väiksem kui arvu täiendavad taaste suunab teid konfigureerimine.
3. **Algne dispersioonanalüüs algusaeg**, näidates Millal alustada algse kopeerimine. Funktsiooni dispersioonanalüüs ilmneb üle Interneti-läbilaskevõime nii, et võiksite plaanida selle väljaspool teie hõivatud tundi.
5. **Krüpti salvestatud Azure'i andmed**, saate määrata, kas krüptimiseks veebisaidil Azure storage ülejäänud andmed. Klõpsake nuppu **OK**.

    ![Poliitika dispersioonanalüüs](./media/site-recovery-vmm-to-azure/gs-replication2.png)

6. Kui loote uue poliitika on automaatselt seotud VMM pilvega. Klõpsake nuppu **OK**. Täiendavad VMM pilved (ja nende VM) saate seostada selle dispersioonanalüüs rühmapoliitika **sätted** > **Dispersioonanalüüs** > poliitika nimi > **Seostada VMM pilve**.

    ![Poliitika dispersioonanalüüs](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="step-5-capacity-planning"></a>Juhis 5: Võimsuse planeerimine

Nüüd, kui teil on oma basic taristu saate häälestada saate mõelda võimsuse planeerimine ja aru saada, kas teil on vaja täiendavaid ressursse.

Saidi taastamine pakub võimsus Plaanur aitavad eraldada õige ressursside allika keskkonna, saidi taastamise komponendid, võrgunduse ja salvestusruumi. Saate käivitada soovitud Plaanur kiirrežiimis valmisväärtusi põhinevad keskmine arv VMs, ketast ja salvestusruumi või üksikasjalik režiimis, kus saate sisestada töökoormus tasandi andmed. Enne alustamist peate:

- Koguge teavet dispersioonanalüüs keskkonna, sh VMs kohta VMs ketast ja ketta salvestusruumi.
- Hindamiseks igapäevane muutmine (piimapütt) peate kopeeritud andmed. [Võimsus Plaanur Hyper-V koopia jaoks](https://www.microsoft.com/download/details.aspx?id=39057) abil saate seda teha.

1.  Klõpsake tööriista alla laadida ja seejärel käivitage see **alla laadida** . [Lugege artiklit](site-recovery-capacity-planner.md) , mis kaasneb tööriist.
2.  Kui olete lõpetanud valige **Jah** **on käivitate võimsus Plaanur**?

    ![Võimsuse planeerimine](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Võrgu läbilaskevõime kasutuse

Võimsus Plaanur tööriista abil saate arvutada vajalikku läbilaskevõimet kopeerimise (algse kopeerimise ja seejärel delta). Kontrollida läbilaskevõime kasutuse kopeerimise summa on teil mitu võimalust.

- **Throttle läbilaskevõime**: Hyper-V liiklus tiražeerib saidi kontrollimisel teatud Hyper-V server. Saate throttle läbilaskevõime hosti server.
- **Kohandada efektisuvandite läbilaskevõime**: saate mõjutada kopeerimise abil registrivõtmete paar kasutatavat ribalaiust.

#### <a name="throttle-bandwidth"></a>Ahendamise läbilaskevõime

1. Avage Microsoft Azure'i varundus MMC lisandmooduli Hyper-V hosti server. Vaikimisi on saadaval töölaual või C:\Program Files\Microsoft Azure taastamise teenused Agent\bin\wabadmin otsetee Microsoft Azure varukoopia.
2. Lisandmooduli nuppu **Muuda atribuute**.
3. Vahekaardil **Throttling** valige **Luba Interneti läbilaskevõime kasutuse varukoopia toimingute pidurdamise**, ja töö piirangud ja mitte-töö tundi. Lubatud vahemikud on 512 Kbps 102 Mbps sekundis.

    ![Ahendamise läbilaskevõime](./media/site-recovery-vmm-to-azure/throttle2.png)

Cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) abil pidurdamise määramine. Siin on näide:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting-NoThrottle** näitab, et pole pidurdamise on nõutav.


#### <a name="influence-network-bandwidth"></a>Võrgu läbilaskevõime mõju

**UploadThreadsPerVM** registriväärtuse juhtelemendid teemad, mida kasutatakse andmeedastus (esmane või delta dispersioonanalüüs) ketta arv. Suurem väärtus suurendab võrgu läbilaskevõime, kasutatakse kopeerimine. **DownloadThreadsPerVM** Registrisätte väärtuseks saate määrata kasutada andmete edastamiseks ajal failback arvu.

1. Liikuge registris **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure'i Backup\Replication**.

    - Väärtuse **UploadThreadsPerVM** muutmine (või luua võti, kui seda pole) juhtelemendi Teemad kasutada kettapuhastusriista kopeerimise abil.
    - Väärtuse **DownloadThreadsPerVM** muutmine (või luua võti, kui seda pole) juhtelemendi Teemad kasutatakse failback liiklus Azure'i abil.
2. Vaikeväärtus on 4. "Overprovisioned" võrgus registrivõtmete muuta oma vaikeväärtused. Maksimumväärtus on 32. Jälgida liikluse optimeerida väärtus.

## <a name="step-6-enable-replication"></a>Samm 6: Luba dispersioonanalüüs

Nüüd lubada kopeerimine järgmiselt:

1. Klõpsake **Samm 2: rakenduse ise** > **Allikas**. Kui olete lubanud dispersioonanalüüs esimest korda klõpsate **+ ise** võlvkelder lubamiseks dispersioonanalüüs arvutite jaoks.

    ![Lubada kopeerimine](./media/site-recovery-vmm-to-azure/enable-replication1.png)

2. **Andmeallika** tera > valige VMM server ja asuvad Hyper-V hosts pilve. Klõpsake nuppu **OK**.

    ![Lubada kopeerimine](./media/site-recovery-vmm-to-azure/enable-replication-source.png)

3. **Target (sihtkoht)** valige tellimus, postituse-Tõrkesiirde juurutamise mudeli ja kasutate kopeeritud andmed salvestusruumi konto.

    ![Lubada kopeerimine](./media/site-recovery-vmm-to-azure/enable-replication-target.png)

4. Valige salvestusruumi konto, mida soovite kasutada. Kui soovite kasutada erinevaid salvestusruumi konto, kui teil on te saate [luua](#set-up-an-azure-storage-account). Ressursihaldur näidise konto storage loomiseks klõpsake nuppu **Loo uus**. Kui soovite luua salvestusruumi konto abil klassikaline, tehke selle [Azure'i portaalis](../storage/storage-create-storage-account-classic-portal.md). Klõpsake nuppu **OK**.
5. Valige Azure võrgu- ja millele loob ühenduse Azure VMs, kui need on kedratud pärast Tõrkesiirde alamvõrgu. Valige võrgu sätte rakendamiseks kõik masinad valite kaitse **nüüd jaoks valitud masinad konfigureerimine** . Valige **Konfigureeri hiljem** Azure võrgu masina kohta. Kui soovite kasutada erinevate võrgu mandaadist peate te saate [luua](#set-up-an-azure-network). Ressursihaldur mudeli võrgu loomiseks klõpsake nuppu **Loo uus**. Kui soovite luua võrgu klassikaline teen selle [Azure'i portaalis](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Valige soovitud alamvõrgu võimalusel. Klõpsake nuppu **OK**.
6. **Virtuaalmasinates** > **Valige virtuaalmasinates** klõpsake ja valige igas arvutis, mida soovite korrata. Saate valida ainult masinad dispersioonanalüüs saate lubatud. Klõpsake nuppu **OK**.

    ![Lubada kopeerimine](./media/site-recovery-vmm-to-azure/enable-replication5.png)

5. **Atribuutide** > **atribuutide konfigureerimine**, valige operatsioonisüsteem valitud VMs ja OS ketas. Klõpsake nuppu **OK**. Täiendavad atribuudid saate määrata hiljem.

    ![Lubada kopeerimine](./media/site-recovery-vmm-to-azure/enable-replication6.png)


12. **Dispersioonanalüüs**sätted > **sätete konfigureerimine dispersioonanalüüs**, valige dispersioonanalüüs poliitika, millele soovite rakendada kaitstud vms. Klõpsake nuppu **OK**. Saate muuta dispersioonanalüüs rühmapoliitika **sätted** > **Dispersioonanalüüs poliitikad** > poliitika nimi > **Muuda sätteid**. Mis on juba imitatsiooniga, ja uus kasutatakse muudatuste rakendamist.

    ![Lubada kopeerimine](./media/site-recovery-vmm-to-azure/enable-replication7.png)

Saate jälgida edenemist **Lubamine kaitse** töö **sätted** > **töö** > **saidi taastamise tööd**. Pärast **Viimistlemine kaitse** töö töötab seade on valmis Tõrkesiirde.

### <a name="view-and-manage-vm-properties"></a>Saate vaadata ja hallata VM atribuudid

Me soovitame, et veenduda masina andmeallika atribuudid. Pidage meeles, et Azure'i VM nimi peab vastama [Azure virtuaalse masina](site-recovery-best-practices.md#azure-virtual-machine-requirements)nõuetele.

1. Klõpsake nuppu **sätted** > **Kaitstud üksuste** > **Kopeeritud üksused** > ja valige seadme üksikasjade kuvamiseks.

    ![Lubada kopeerimine](./media/site-recovery-vmm-to-azure/vm-essentials.png)

2. **Atribuudid** kuvatakse kopeerimise ja Tõrkesiirde teabe VM.

    ![Lubada kopeerimine](./media/site-recovery-vmm-to-azure/test-failover2.png)

3. **Arvuta**ja võrgu > **arvutada atribuudid** saate määrata Azure VM nimi ja sihtkoht suurus. [Azure'i nõuete](site-recovery-best-practices.md#azure-virtual-machine-requirements) täitmiseks, kui teil on vaja nime muuta. Saate vaadata ja muuta teavet target võrgus, alamvõrgu ja IP-aadress on määratud Azure VM. Võtke arvesse järgmist.

    - Saate seada target IP-aadress. Kui te ei esita aadressi nurjunud üle arvuti kasutab DHCP. Kui seate aadressi, mis pole saadaval Tõrkesiirde, on tõrkesiire nurjub. Sama target IP-aadressi saab kasutada test Tõrkesiirde kui aadress on test Tõrkesiirde võrgus saadaval.
    - Võrguadapteri arv on tingitud suurus saate määrata target virtuaalse masina, järgmiselt:

        - Kui allika arvutisse võrguadapteri arv on väiksem või võrdne arv adapterit masina sihtformaat lubatud, siis sihtkohas on samal hulgal adapterit allikana.
        - Kui adapterit allika virtual masina arv ületab lubatud target suurus ja seejärel soovitud suuruse maksimaalne arv kasutatakse.
        - Näiteks kui allika arvuti on kaks võrguadapteri ja seadme sihtformaat toetab neli, sihtarvutis on kaks adapterit. Kui allika arvuti on kaks adapterit, kuid toetatud sihtformaat toetab ainult üks siis sihtarvutis on ainult üks adapterit.     
        - Kui VM on mitu võrguadapteri need kõik ühendada samasse võrku.

    ![Lubada kopeerimine](./media/site-recovery-vmm-to-azure/test-failover4.png)

5.  **Ketast** näete operatsioonisüsteem ja andmete ketast VM, mis kuvatakse kopeeritud.



## <a name="step-7-test-your-deployment"></a>Juhis 7: Testige oma juurutamine

Juurutamise käivitate test Tõrkesiirde ühe virtuaalse masina või taastamise kava, mis sisaldab ühe või mitme virtuaalmasinates.


### <a name="prepare-for-failover"></a>Tõrkesiirde ettevalmistamine

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

- Lisage avaliku lõpp-punkti RDP protokolli (port 3389) ja määrake sisselogimise mandaat.
- Veenduge, et teil pole mõnda domeeni poliitika, mis takistada ühenduse virtuaalse masina avalik aadressidel.
- Proovige ühendust luua. Kui te ei saa ühendust luua, veenduge, et VM töötab. Veel tõrkeotsingunäpunäiteid lugege selle [artikli](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Kui soovite juurdepääsu Azure VM, mis töötab Linux pärast Tõrkesiirde Secure Shell kliendi (ssh), tehke järgmist.

**Kohapealse arvutisse enne Tõrkesiirde**:

- Veenduge, et automaatselt käivituma süsteemi käivitamine on määratud Secure Shell teenuse Azure VM.
- Kontrollige, et lubada tulemüüri reeglid selle SSH ühendus.

**Klõpsake the Azure VM pärast Tõrkesiirde**:

- Klõpsake nurjunud VM ja Azure alamvõrgu, millega on ühendatud üle võrgu rühma reeglid tuleb lubada SSH pordi sissetulevaid ühendusi.
- Avaliku lõpp-punkti luua, et lubada sissetulevaid ühendusi SSH Port (TCP port 22 vaikimisi).
- Kui VM pääseb VPN-ühenduse kaudu (teekonna või saidilt VPN) siis kliendi saab kasutada otse ühenduse loomiseks VM üle SSH.


### <a name="run-a-test-failover"></a>Käivitage test Tõrkesiirde

Käivitamiseks test Tõrkesiirde tehke järgmist:

1. Nurjumise üle ühe VM **sätted** > **Kopeeritud üksused**, klõpsake VM > **+ Test Tõrkesiirde**.
2. Kasutada taastamise kava **sätted** > **Taastamine lepingud**, paremklõpsake leping > **Test Tõrkesiirde**. Luua enam kavandamine, [järgige neid juhiseid](site-recovery-create-recovery-plans.md).

3. Valige **Test Tõrkesiirde** Azure võrgu Azure VMs ühendada pärast Tõrkesiirde.
4. Klõpsake nuppu **OK** , et alustada selle Tõrkesiirde. Saate jälgimiseks, klõpsates VM selle atribuudid avamiseks või **Test Tõrkesiirde** töö **sätted** > **saidi taastamise tööd**.
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


## <a name="monitor-your-deployment"></a>Juurutamise jälgimine

Siin on, kuidas saate jälgida konfiguratsioonisätted, olek ja seisund oma saidi taastamine juurutamiseks.

1. Klõpsake vault nime juurdepääsu **Essentialsi** armatuurlaud. Armatuurlaua saate saidi taastamise tööde haldamine, kopeerimise olek, taastamise, serveri seisundi ja sündmusi.  Saate kohandada Essentialsi paanid ja paigutusi, mis on kõige kasulikumad teile, sh saidi varundus ja taaste võlvid oleku kuvamiseks.

    ![Essentialsi](./media/site-recovery-vmm-to-azure/essentials.png)

2. Paani **seisundit** saate jälgida saidi serverid (VMM või konfiguratsiooni servers), mis on tekkinud probleem ja sündmuste tõstatatud saidi taastamine viimase 24 tunni jooksul.
3. Saate hallata ja jälgida **Kopeeritud üksused**, **Taastamine lepingud**, kopeerimise ja **Saidi taastamise töö** paanid. Saate minna tööle **sätted** -> **töö** -> **Saidi taastamise tööd**.


## <a name="next-steps"></a>Järgmised sammud

Pärast juurutamise on häälestatud ja töötab, [Lugege lisateavet](site-recovery-failover.md) eri tüüpi Tõrkesiirde kohta.
