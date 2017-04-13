<properties
    pageTitle="Paljundada Hyper-V virtuaalmasinates VMM pilved Azure'i portaalis VMM saidi | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse juurutada Azure saidi taastamist korraldab dispersioonanalüüs, Tõrkesiirde ja taastamise Hyper-V vms VMM pilved VMM saidi Azure'i portaalis."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-the-azure-portal"></a>Paljundada Hyper-V virtuaalmasinates VMM pilved VMM saidi Azure portaalis

> [AZURE.SELECTOR]
- [Azure'i portaal](site-recovery-vmm-to-vmm.md)
- [Klassikaline portaal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShelli - ressursihaldur](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Tere tulemast Azure'i saidi taastamine! Selles artiklis, kui soovite ise kohapealse Hyper-V virtuaalmasinates hallatava System Center virtuaalse masina Manager (VMM) pilved saidi kasutamine. Selles artiklis kirjeldatakse, kuidas häälestada kopeerimise abil Azure saidi taastamine Azure'i portaalis.

> [AZURE.NOTE] Azure'i on kaks eri [juurutamise mudelite](../resource-manager-deployment-model.md) loomise ja ressursside töötamine: Azure'i ressursihaldur ja klassikaline. Azure'i on kaks portaalide – Azure klassikaline portaali, mis toetab klassikaline juurutamise mudeli ja Azure portaali nii juurutamise mudelid tugi.


Azure'i saidi taastamine Azure'i portaalis pakub mitmesuguseid uusi funktsioone.

- Funktsiooni Azure portaali, Azure varukoopia ja Azure saidi taastamise teenuste kombineeritakse ühe taastamise teenused võlvikus nii, et saate häälestada ja hallata järjepidevuse ja avariitaastet (BCDR) ühest kohast. Ühendatud armatuurlaua võimaldab teil jälgida ja hallata oma kohapealse saitide ja Azure avaliku pilve toimingud.
- Azure'i tellimused ette valmistatud programmiga pilve lahenduse pakkuja (CSP) kasutajad saavad nüüd hallata saidi taastamise toimingute Azure'i portaalis.


Pärast selle artikli lugemist postitada allosas Disqus kommentaaride kommentaarid. Tehniline küsimuste [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)


## <a name="overview"></a>Ülevaade

Peavad organisatsioonid BCDR strateegia, mis määrab, kuidas rakendused, töökoormus ja andmete püsida töötab ja kättesaadav ajal plaanitud ja plaanimata tööseisakute ja taastamiseks tavaline tingimuste nii kiiresti kui võimalik. BCDR strateegia kavandamine peaks äriandmete turvalised ja saab hoida ja tagama töökoormus pidevalt katastroofi ilmnemisel.

Saidi taastamine on Azure'i teenus, mis aitab BCDR strateegia kavandamine, orkesteroinnin dispersioonanalüüs füüsilise kohapealsed serverid ja virtuaalmasinates pilveteenusesse (Azure) või teisene andmekeskusesse. Kui katkestuste esineda teie peamine asukoht, teil ei õnnestu üle teisene asukohta hoida rakendused ja töökoormus saadaval. Te ei tagasi oma peamine asukoht kui tagastab see toiminguid. Lisateavet leiate [mis on Azure saidi taastamise?](site-recovery-overview.md)

Selles artiklis antakse kogu teavet, mida soovite korrata kohapealse VMM pilved VMM saidi Hyper-V VMs. See sisaldab ülevaate arhitektuuri, teavet ja juurutamise kohapealsed serverid, dispersioonanalüüs sätted ja võimsuse planeerimine konfigureerimise juhised kavandamine. Kui olete häälestanud taristu, saate lubada dispersioonanalüüs arvutites, mida soovite kaitsta ja kontrollida selle Tõrkesiirde töötab.

## <a name="business-advantages"></a>Eelised ettevõtetele

- Saidi taastamine pakub kaitse business töökoormus ja rakenduste Hyper-V VMs, imitatsiooniga need teisene Hyper-V server töötab.
- Taastamise teenused portaalis häälestada, hallata ja jälgida dispersioonanalüüs, Tõrkesiirde ja taastamine ühes kohas.
- Teisene saidi esmane saidi ja failback (Taasta) saate käivitada oma peamine kohapealse infrastruktuuri hõlpsalt Käivita failovers.
- Saate konfigureerida taastamise mitme nii, et astmeline rakenduse töökoormus ei õnnestu üle koos.

## <a name="scenario-architecture"></a>Stsenaarium arhitektuur

Need on stsenaarium komponendid.

- **Esmane saidi**: esmane saidil on üks või mitu Hyper-V host serverites töötab allika VMs, mida soovite korrata. Need esmane hosti serverid asuvad VMM kohaselt.
- **Saidi**: teisene saidil on ühe või mitme Hyper-V hosti serverites töötab target VMs, millele te ise esmane VMs. Need hosti serverid asuvad VMM kohaselt. Pilveteenuses võib olla esmane serveris (kui teil on ainult üks VMM server) või teisene VMM Server.
- **Pakkuja**: saidi taastamine juurutamisel, installimist Azure saidi taastamise pakkuja serverites VMM ja registreerida need serverid võlvkelder taastamise teenused. Pakkuja serveris VMM suhtleb saidi taastamine HTTPS 443 korraldamise kopeerida. Andmete kopeerimine ilmneb esmaseid ja teiseseid Hyper-V host meiliserverite vahel. Kopeeritud andmed ei viida kohapealse saidid ja võrgu ja Azure ei saadeta. Lugege lisateavet [Privaatsus](#privacy-information-for-site-recovery).

![E2e topoloogia](./media/site-recovery-vmm-to-vmm/architecture.png)

### <a name="data-privacy-overview"></a>Andmete privaatsust ülevaade

Selles tabelis on kokku võetud, kuidas andmed talletatakse seda stsenaariumi.
****
Toiming | **Üksikasjad** | **Kogutud andmete** | **Kasutamine** | **Nõutav**
--- | --- | --- | --- | ---
**Registreerimine** | Registreerige taastamise teenused vault VMM server. Kui soovite hiljem unregister server, saate seda teha kustutades serveri teave Azure portaalist. | Pärast VMM server on registreeritud saidi taastamine kogub, protsessid, ja metaandmeid VMM serveri ja saidi taastamine avastada VMM pilvede nimed üle. | Andmeid saab tuvastamine ja soovitud VMM server suhtlemine ja vastav VMM pilved sätete konfigureerimine. | See funktsioon on vajalik. Kui te ei soovi seda teavet saada saidi taastamine ei tohiks saate kasutada saidi taastamise teenust.
**Lubada kopeerimine** | Azure saidi taastamise pakkuja on installitud VMM server ja üksuse saidi taastamise teenuse suhtlemiseks. Pakkuja on dünaamiliselt Lingitav teek (DLL) majutatud VMM käigus. Pärast installimist pakkuja saab "Andmekeskuse taastamise" funktsioon lubatud VMM administreerimiskonsool. Uute ja olemasolevate VMs saate selle funktsiooni lubamiseks kaitse VM. | See atribuut on seatud, saadab pakkuja nimi ja ID VM saidi taastamine.  Kopeerimine on lubatud, Windows Server 2012 või Windows Server 2012 R2 Hyper-V koopia. Virtuaalse masina andmed saab kopeeritud ühe Hyper-V host teise (tavaliselt asub andmekeskuse erinevate "taastamise"). | Saidi taastamise kasutab metaandmete asustamiseks VM teabe Azure'i portaalis. | See funktsioon on oluline osa teenuse ja ei saa välja lülitada. Kui te ei soovi seda teavet saata, ei luba saidi taastamine kaitse vms. Pange tähele, et kõik andmed, mis on saadetud pakkuja saidi taastamine saadetakse https.
**Taastamise kava** | Taastamise aitab luua korraldamise kavandamine taastamine data Centeri kaudu. Saate määratleda, kus tuleb VMs või rühma virtuaalmasinates alustada taastamine saidi järjestuses. Saate määrata, mis tahes automatiseeritud skriptide olema käitamine või mõnda käsitsi toimingu võetakse iga VM taastamise ajal. Tõrkesiirde käivitatakse tavaliselt koordineeritud taastamise tasemel taastamise leping. | Saidi taastamine kogub, töötleb ja edastab metaandmete taastamine lepingule, sh virtuaalse masina metaandmete ja mis tahes automatiseerimise skripte ja märkmete käsitsi toimingu metaandmete. | Metaandmete abil koostada taastamise kava Azure'i portaalis. | See funktsioon on oluline osa teenuse ja ei saa välja lülitada. Kui te ei soovi seda teavet saada saidi taastamise, ärge looge taastamise.
**Võrgu kaardistamine** | Kaartide võrgu teave esmane andmekeskuse andmekeskuse taastamine. Kui VMs on taastatud taastamine saidi, võrgu kaardistamine aitab luua võrguühendus. | Saidi taastamine kogub, töötleb ja edastab metaandmeid, loogilised võrgud iga saidi (primaarne ja andmekeskuse). | Metaandmete kasutatakse asustamiseks võrgu sätteid nii, et saate vastendada võrgu teave. | See funktsioon on oluline osa teenuse ja ei saa välja lülitada. Kui te ei soovi seda teavet saada saidi taastamise, ärge kasutage võrgu kaardistamine.
**Tõrkesiirde (plaanitud planeerimata/katse)** | Tõrkesiirde ei VMs kaudu ühe VMM haldusega andmekeskuse teisele üle. Azure'i portaalis käivitatakse käsitsi Tõrkesiirde toiming. | Pakkuja serveris VMM antakse teada Tõrkesiirde sündmuse saidi taastamine ja töötab Tõrkesiirde toimingu Hyper-V host VMM liideste kaudu. Tegelik Tõrkesiirde VM on üks Hyper-V Host mõnele teisele ja töödeldud Windows Server 2012 või Windows Server 2012 R2 Hyper-V koopia. Kui Tõrkesiirde on lõpule jõudnud, andmekeskuse taastamine serveris VMM pakkuja saadab edu saidi taastamine. | Saidi taastamise kasutab seda teavet, mis on saadetud asustamiseks Tõrkesiirde toimingu teabe Azure'i portaalis olekut. | See funktsioon on oluline osa teenuse ja ei saa välja lülitada. Kui te ei soovi seda teavet saada saidi taastamise, ärge kasutage Tõrkesiirde.


## <a name="azure-prerequisites"></a>Azure'i eeltingimused

Siin on, mida on vaja Azure juurutamine selle stsenaariumi:

**Eeltingimused** | **Üksikasjad**
--- | ---
**Azure'i**| Teil on vaja [Microsoft Azure'i](http://azure.microsoft.com/) kontosse. Saate alustada [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). [Lisateavet leiate teemast](https://azure.microsoft.com/pricing/details/site-recovery/) saidi taastamine hindade kohta.


## <a name="on-premises-prerequisites"></a>Kohapealse eeltingimused

Siin on, mida on vaja esmaseid ja teiseseid kohapealse saitidel juurutada selle stsenaariumi:

**Eeltingimused** | **Üksikasjad**
--- | ---
**VMM** | Soovitame juurutate VMM esmane saidi serveri ja saidi VMM server.<br/><br/> Samuti saate [ühe VMM server parkimise vahel](site-recovery-single-vmm.md). Selle tegemiseks peate olema vähemalt kahe pilve VMM-i server on konfigureeritud.<br/><br/> VMM serverid olema vähemalt opsüsteemi System Center 2012 SP1 uusimaid värskendusi.<br/><br/> Iga VMM server peab olema üks või pilve konfigureeritud ja kõik pilved peab olema Hyper-V võimsuse reeglite seadmine. <br/><br/>Pilved peab sisaldama vähemalt ühte VMM hosti rühmad.<br/><br/>VMM pilved [konfigureerimise VMM cloud struktuuri](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), häälestamise kohta lisateabe saamiseks ja [tutvustust: loomine privaatne pilved System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM serverid vajate Interneti-ühendus.
**Hyper-V** | Hyper-V serverite peab olema vähemalt opsüsteemi Windows Server 2012 Hyper-V rolliga ja olete installinud uusimaid värskendusi.<br/><br/> Hyper-V server peaks sisaldama vähemalt ühte VMs.<br/><br/>  Hyper-V hosti serverid peaks asuma hosti rühmad põhi- ja VMM pilved.<br/><br/> Kui kasutate Hyper-V klaster Windows Server 2012 R2 peaksite installima [2961977 värskendamine](https://support.microsoft.com/kb/2961977)<br/><br/> Kui teil on Hyper-V klaster Windows Server 2012, Pange tähele, et kobar ta ei luuakse automaatselt, kui teil on staatiline IP address-põhiste kobar. Peate kobar ta käsitsi konfigureerimine. [Lisateavet vt](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Pakkuja** | Saidi taastamine juurutamisel, installige Azure'i saidi taastamise pakkuja VMM serverites. Pakkuja suhtleb saidi taastamine HTTPS 443 korraldab kopeerimine. Andmete kopeerimine toimub esmaseid ja teiseseid Hyper-V serverite üle LAN või VPN-ühendus.<br/><br/> Pakkuja serveris VMM vajab juurdepääsu idest: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.Windows.net; *. backup.windowsazure.com; *. blob.Core.Windows.net; *. store.core.windows.net.<br/><br/> Lisaks võimaldada tulemüüri teatist VMM serverid [Azure andmekeskuse IP-aadresside vahemikud](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja luba (443) HTTPS-protokolli.

## <a name="prepare-for-deployment"></a>Juurutamise ettevalmistamine

Ettevalmistamiseks juurutamiseks peate:

1. Saidi taastamine juurutamiseks [ettevalmistamine VMM server](#prepare-the-vmm-server) .
2. [Võrgu kaardistamine ettevalmistamine](#prepare-for-network-mapping). Häälestage võrgu nii, et saate konfigureerida võrgu kaardistamine.

### <a name="prepare-the-vmm-server"></a>Ettevalmistused VMM server

Veenduge, et VMM server vastab [eeltingimused](#on-premises-prerequisites) ja pääsete juurde loetletud URL-id.


### <a name="prepare-for-network-mapping"></a>Võrgu kaardistamine ettevalmistamine

Võrgu vastenduse kaardid vahel VMM VM esmaseid ja teiseseid VMM serverites, et:

- Optimaalselt asetage koopia VMs teisene Hyper-V hosts pärast Tõrkesiirde.
- Ühenduse vastav VM võrkude koopia VMs.
- Kui konfigureerite võrgu vastendamise koopia VMs ei ühendada mis tahes võrk pärast Tõrkesiirde.
- Kui soovite häälestada võrku vastendamise saidi taastamisel juurutamise siin on, mida vajate:

    - Veenduge, et VMs allikas Hyper-V hosti server on ühendatud võrku VMM VM. Selle võrgu peaks olema seotud loogilise võrgu, mis on seotud pilve.
    - Veenduge, et taastamine kasutatav teisene pilveteenuses on vastava VM võrgu konfigureeritud. Et VM võrgu seotud loogika võrk, mis on seotud teisene pilveteenuses.

- [Lisateavet leiate teemast](site-recovery-network-mapping.md) võrgu kaardistamine tööpõhimõtete kohta.

## <a name="prepare-for-deployment-with-a-single-vmm-server"></a>Juurutus, nii et ühe VMM server ettevalmistamine

Kui teil on ainult üks VMM server saaksite korrata VMs Hyper-V hosts pilveteenuses VMM [Azure](site-recovery-vmm-to-azure.md) või sekundaarne VMM pilveteenusesse. Soovitame esimene variant Kuna imitatsiooniga pilved vahel pole sujuvalt, kuid kui teil on vaja teha siin on, mida peate tegema:

1. Saate **häälestada VMM Hyper-V VM**. Kui seda me soovitame teil colocate kasutatavaid VMM sama VM SQL serveri eksemplar. See salvestab aja ainult üks VM on luua. Kui soovite kasutada serveri eksemplar SQL serveri ja mõne katkestuste ilmneb, peate selle eksemplari taastada, enne kui saate taastada VMM.
2. **Kontrollige, kas VMM server on vähemalt kahe pilve konfigureeritud**. Ühe pilve sisaldab VMs soovite paljundada ja muude pilveteenuses toimib teisene asukoht. Pilveteenuse, mis sisaldab VMs, mida soovite kaitsta järgima [eeltingimused](#on-premises-prerequisites).
3. Selles artiklis kirjeldatud, häälestage saidi taastamine. Loomine ja registreerida VMM server autoriloomingut, häälestada dispersioonanalüüs poliitika ja lubada kopeerimine. Määrake peaks selle algse kopeerimine toimub võrgu kaudu.
4. Kui häälestate võrgu kaardistamine vastendate VM võrgu esmane Cloud VM võrgu jaoks teisese pilve.
5. Konsooli Hyper-V halduri lubamine Hyper-V koopia Hyper-V Host, mis sisaldab VMM VM ja lubada kopeerimine VM. Veenduge, et lisate ei VMM virtuaalse masina pilved, mis on kaitstud saidi taastamine tagamaks, et Hyper-V koopia sätted ei kaalu saidi taastamine.
6. Kui loote Tõrkesiirde jaoks kasutada sama VMM server lähte- ja sihtsaitide jaoks.
7. Ei õnnestu üle ja taastada järgmiselt:

    - Käsitsi ei õnnestu üle VMM VM Hyper-V halduri kasutamine kavandatud Tõrkesiirde teisene saidile.
    - Ei õnnestu VMs üle.
    - Pärast esmast VMM VM on taastatud, Azure portaali sisselogimine -> taastamise teenused võlvkelder ja käivitada planeerimata Tõrkesiirde VMs, saidi esmane saidile.
    - Pärast planeerimata Tõrkesiirde on lõpule jõudnud, pääseb kõik ressursid esmane saidi kaudu uuesti.


### <a name="create-a-recovery-services-vault"></a>Taastamise teenused vault loomine

1. [Azure'i portaali](https://portal.azure.com)sisse logida.
2. Klõpsake nuppu **Uus** > **halduse** > **taastamise teenused**. Teise võimalusena võite klõpsata **sirvida** > **Taastamise teenused** võlvid > **Lisa**.

    ![Uue vault](./media/site-recovery-vmm-to-vmm/new-vault3.png)

3. Määrake **nimi** sõbralik nimi, mis tähistavad vault. Kui teil on mitu tellimust, valige üks neist.
4. [Uue ressursirühma Loo](../resource-group-template-deploy-portal.md) või valige olemasoleva ja määrake Azure piirkond. Masinad on kopeeritud selle alale. Märkige ruut toetatud regioonide leiate [Azure'i saidi taastamise hinnad üksikasjad](https://azure.microsoft.com/pricing/details/site-recovery/) geograafilised saadavus
4. Kui soovite kiiresti juurde vault armatuurlaualt klõpsake käsku **Kinnita armatuurlaua** > **loomine vault**.

    ![Uue vault](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

Kuvatakse uus vault **armatuurlaua** > **kõik ressursid**, ja peamine **taastamise teenused võlvid** tera.




## <a name="getting-started"></a>Alustamine

Saidi taastamine pakub on alustamine aitab teil juurutada nii kiiresti kui võimalik. Alustamine eeltingimused kontrollib ja juhendab teid saidi taastamine juurutamise etappide õiges järjestuses.

Alustamine valige tüüp masinad, mida soovite kopeerida, ja kuhu soovite korrata. Häälestamine kohapealsed serverid, dispersioonanalüüs poliitikate loomine ja teha võimsuse planeerimine. Kui olete häälestanud oma taristu lubate dispersioonanalüüs vms. Saate käivitada failovers teatud masinad või luua taastamine ei õnnestu üle mitme masinad.

Alustage alustamine, valides, kuidas soovite juurutada saidi taastamine. Alustamine voogu muudab veidi vastavalt oma vajadustele kopeerimine.

## <a name="step-1-choose-your-protection-goal"></a>Samm 1: Valige kaitse teie eesmärk

Valige mida soovite korrata ja kuhu soovite korrata.

1. Valige oma vault **taastamise teenused võlvid** tera ja klõpsake nuppu **sätted**.
2. **Sätted** > **Alustamine** nuppu **Saidi taastamine** > **Samm 1: ettevalmistamine taristu** > **kaitse eesmärk**.
3. **Kaitse eesmärk** **taastamine saidi**valimine ja valige **Jah, mille Hyper-V**.
4. Valige **Jah** , kasutate VMM Hyper-V hosts hallata ja valige **Jah** , kui teil on sekundaarne VMM server. Kui juurutamist dispersioonanalüüs vahel ühe VMM server parkimise klõpsate nuppu **ei**. Klõpsake nuppu **OK**.

    ![Valige eesmärgid](./media/site-recovery-vmm-to-vmm/choose-goals.png)


## <a name="step-2-set-up-the-source-environment"></a>Samm 2: Allika keskkonna häälestamine

Installige Azure'i saidi taastamise pakkuja VMM serverites ja registreerida serverid autoriloomingut.

1. Klõpsake **Samm 2: ettevalmistamine taristu** > **Allikas**.

    ![Allika häälestamine](./media/site-recovery-vmm-to-vmm/goals-source.png)

2. Klõpsake **andmeallika ettevalmistamine** **+ VMM** VMM server lisada.

    ![Allika häälestamine](./media/site-recovery-vmm-to-vmm/set-source1.png)

2. **Lisada serveri** blade kontrolli **System Center VMM server** **serveri tüüp** ja mis kuvatavas VMM server vastab [eeltingimused ja URL-i nõuded](#on-premises-prerequisites).
4. Azure taastamise pakkujalt installi faili alla laadida.
5. Laadige alla registreerimise võti. Peate seda, kui käivitate häälestus. Võti kehtib 5 päeva pärast selle loomist.

    ![Allika häälestamine](./media/site-recovery-vmm-to-vmm/set-source3.png)

6. Installige Azure'i saidi taastamise pakkuja VMM server.

> [AZURE.NOTE] Ärge peate installima midagi konkreetselt Hyper-V hosti serverites.


### <a name="set-up-the-azure-site-recovery-provider"></a>Azure saidi taastamise pakkuja häälestamine

1. Käivitage pakkuja installifail iga VMM Server. Kui VMM juurutatakse klaster ja installite selle pakkuja esimest korda installimine aktiivse sõlme ja lõpule viimiseks autoriloomingut VMM server registreerimiseks. Installige pakkuja teisi sõlmi. Kobar sõlmed kõik peaks töötama pakkuja sama versioon.
2. Installiprogramm käivitub mõne prerequirement kontrollib ja taotleb õiguste VMM teenuse peatada. Teenuse VMM taaskäivitatakse automaatselt, kui install on lõpule viidud. Kui installite VMM kobar küsitakse peatamiseks kobar roll.

2.  **Microsoft** Update'i saate valite värskendusi, et pakkuja värskendused on installitud vastavalt teie Microsoft Update poliitika.
3. **Installi** aktsepteerida või pakkuja installimise vaikeasukoha muutmiseks ja klõpsake nuppu **Installi**.

    ![Installi asukoht](./media/site-recovery-vmm-to-vmm/provider-location.png)

3. Pärast installi lõpuleviimist klõpsake **registreerida** autoriloomingut serveri registreerimiseks.

    ![Installi asukoht](./media/site-recovery-vmm-to-vmm/provider-register.png)

9. Kontrollige **Vault nimi**name server registreeritakse Vault. Klõpsake nuppu *edasi*.

    ![Server registreerimine](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)

7. Määrake **Interneti-ühenduse** pakkuja serveris VMM Interneti-ühenduse loomise viisi. Valige **Loo ühendus olemasoleva puhverserveri sätted** on vaikimisi Interneti-ühenduse sätteid server on konfigureeritud kasutama.

    ![Interneti-sätted](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

    - Kui soovite kasutada kohandatud puhverserveri peaks häälestamist enne installimist pakkuja. Kohandatud puhverserveri sätete konfigureerimisel käivituvad test puhverserveri ühenduse kontrollimiseks.
    - Kui kasutate kohandatud puhverserveri või teie vaikimisi puhverserver nõuab autentimist, peate sisestama puhverserveri üksikasjad, sh puhverserveri aadressi ja pordi.
    - Järgmiste URL-ide peaks olema VMM Server ja Hyper-v hostide kaudu juurdepääsetavad
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Luba [Azure'i andmekeskuse IP-vahemikke](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja HTTPS (443) protokollis kirjeldatud IP-aadressid. Mida oleks valge nimekiri IP-aadresside vahemikud Azure piirkond, mida soovite kasutada, ja mis Lääne US.
    - Kui kasutate kohandatud puhverserveri VMM RunAs konto (DRAProxyAccount) luuakse automaatselt määratud puhverserveri mandaadi abil. Puhverserveri konfigureerida nii, et selle konto saate edukalt autentimiseks. VMM RunAs konto sätteid saate muuta VMM konsooli. Selleks **sätete** tööruumi avada, laiendage **Turve**, käsku **Käivita kontod**ja seejärel muuta parooli DRAProxyAccount. Peate nii, et see säte jõustub VMM teenust uuesti.


8. Valige **Registreerimise võti**, võti, mida saate alla laadida Azure saidi taastamine ja kopeeritud VMM Server.


10.  Krüptimise säte kasutatakse ainult siis, kui te olete imitatsiooniga Hyper-V VMs VMM pilved Azure. Kui te olete imitatsiooniga saidi seda ei kasutata.

11.  **Serveri nimi**Määrake tuvastamiseks VMM server autoriloomingut sõbralik nimi. Määrake kobar konfiguratsiooni VMM kobar rolli nimi.
12.  **Sünkroonimine pilveteenuse metaandmete** , valige kas soovite sünkroonida kõik parkimise VMM server metaandmete vault. Seda toimingut tuleb ainult juhtub üks kord igale serveris. Kui te ei soovi sünkroonida kõik pilved, saate jätke see säte on märkimata ja sünkroonida iga pilv eraldi cloud atribuutide VMM konsooli.

13.  Klõpsake nuppu **Järgmine** lõpule viia. Pärast registreerimist tuuakse metaandmete serverist VMM Azure saidi taastamine. Server kuvatakse lehel **serverid** autoriloomingut vahekaardil **VMM serverid** .

    ![Server](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

11. Kui server on saadaval saidi taastamine konsooli, **Allikas** > **ettevalmistamine allika** valige VMM server ja valige Hyper-V host asub pilve. Klõpsake nuppu **OK**.

#### <a name="command-line-installation"></a>Käsurea installimine

Käsurea kaudu saab installida Azure saidi taastamise pakkuja. See meetod saab installida Server Core Windows Server 2012 R2 pakkuja.

1. Laadige pakkuja installimise faili ja registreerimise võti kausta. Näiteks C:\ASR.
2. System Center virtuaalse masina halduri teenuse peatamine.
3. Alates käsuviipa, käivitage need eraldada pakkuja Installeri käsud:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Käivitage see käsk Provideri installimine.

        C:\ASR> setupdr.exe /i

5. Käivitage registreerida autoriloomingut serveri need käsud:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Kus asuvad soovitud parameetrid:

 - **/Credentials**: kohustuslik parameeter, mis määrab, kus asub registreerimise võtme faili asukoht  
 - **/FriendlyName**: kohustuslik parameeter Hyper-V hosti server Azure'i saidi taastamine portaalis kuvatavat nime.
 - **/EncryptionEnabled**: valikuline parameeter, mida saate kasutada ainult imitatsiooniga VMM: Azure'i.
 - **/proxyAddress**: valikuline parameeter, mis määrab puhverserveri aadressi.
 - **/proxyport**: valikuline parameeter, mis määrab puhverserveri pordi.
 - **/proxyUsername**: valikuline parameeter, mis määrab puhverserveri kasutajanimi (kui puhverserver nõuab autentimist).
 - **/proxyPassword**: valikuline parameeter, mis määrab parooli koos puhverserveri autentimine (kui puhverserver nõuab autentimist).  

## <a name="step-3-set-up-the-target-environment"></a>Samm 3: Target keskkonna häälestamine

Valige target VMM server ja pilve.

1. Klõpsake **ettevalmistamine infrastruktuur** > **Target** ja märkige ruut target VMM server, mida soovite kasutada.
2.  Kuvatakse saidi taastamine sünkroonitakse serveriga parkimise. Valige target pilve.

    ![Target (sihtkoht)](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="step-4-set-up-replication-settings"></a>Samm 4: Häälestamine dispersioonanalüüs sätted

1. Uue koopia loomiseks klõpsake poliitika **ettevalmistamine infrastruktuur** > **Dispersioonanalüüs sätted** > **+ loomine ja seostada**.

    ![Võrgu](./media/site-recovery-vmm-to-vmm/gs-replication.png)

2. **Loomine ja sidusettevõtte poliitika** määrata poliitika nimi. Lähte- ja tüüp peaks olema **Hyper-V**.
3. Valige **Hyper-V hosti versiooni** opsüsteemi Host töötab.

    > [AZURE.NOTE] VMM pilveteenuses võib sisaldada Hyper-V hosts erineva (toetatud) versiooniga Windows Server, kuid dispersioonanalüüs poliitika on rakendatud hosts sama operatsioonisüsteemi. Kui teil on rohkem kui ühe opsüsteemi versioon töötab hosts siis luua eraldi dispersioonanalüüs poliitika.

4. Tekstivormingus **autentimine on tippige** ja **autentimise pordi** määrata, kuidas liikluse autenditakse esmane ja taastamise Hyper-V host meiliserverite vahel. Valige **sert** , v.a juhul, kui teil on töö Kerberos keskkonnas. Azure'i saidi taastamine automaatselt konfigureerida HTTPS-autentimise serdid. Te ei pea midagi tegema käsitsi. Vaikimisi avatakse 8083 ja 8084 (sert) jaoks pordi Windowsi tulemüür Hyper-V hosti serverites. Kui valite **Kerberos**, kasutatakse Kerberose Piletite hosti serverite vastastikune autentimine. Pange tähele, et see säte on asjakohane Hyper-V hosti serverites, kus töötab Windows Server 2012 R2 ainult.
3. **Kopeerige sagedus** Määrake kui sageli soovite korrata delta andmete pärast algse dispersioonanalüüs (iga 30 sekundi järel, 5 või 15 minutit).
4. **Taastamine osutage säilitamine**, määrake mõne tunni pärast kaua säilituspoliitika akna saab iga taastamine punkti. Kaitstud masinad taastada mis tahes punkti aken.
6. Määrake **rakenduse ühtsete hetktõmmise sagedus** tihti (1 – 12 tundi) taastamise punkte, mis sisaldavad rakenduse ühtsete hetktõmmiseid saab luua. Hyper-V kasutab kahte tüüpi hetktõmmiseid – standard pildi, mis pakub on kogu virtuaalse masina suureneva hetktõmmise ja rakenduse ühtsete hetktõmmise, mis viib rakenduse andmete virtual seadmes hetktõmmise punkti õigel ajal. Rakenduse ühtsete hetktõmmiseid Kasuta helitugevuse vari Service (VSS) on rakenduste ühtsete olekus kui hetktõmmis. Märkus Kui lubate rakenduse ühtsete pilte, mõjutavad jõudlus töötavad allikast virtuaalmasinates rakendused. Veenduge, et seate väärtus on väiksem kui arvu täiendavad taaste suunab teid konfigureerimine.
7. **Andmete edastamiseks tihendamise** määrata, kas kopeeritud andmed, mis on üle võimalik tihendada.
8. Valige **kustutamine VM koopia** määramaks, et koopia virtuaalse masina tuleks kustutada, kui keelate kaitse allika VM. Kui lubate seda sätet, kui keelate kaitse allika VM, eemaldatakse see saidi taastamine konsooli, eemaldatakse saidi taastamine sätted soovitud VMM VMM konsooli ja koopia on kustutatud.
3. **Algne dispersioonanalüüs** meetodi kui te olete imitatsiooniga võrgu kaudu saate määrata, kas alustada algse kopeerimine või määrake selle. Võrgu läbilaskevõime salvestamiseks võite plaanida selle väljaspool teie hõivatud tundi. Klõpsake nuppu **OK**.

    ![Poliitika dispersioonanalüüs](./media/site-recovery-vmm-to-vmm/gs-replication2.png)

6. Kui loote uue poliitika on automaatselt seotud VMM pilvega. **Dispersioonanalüüs poliitika** nuppu **OK**. Täiendavad VMM pilved (ja nende VM) saate seostada selle dispersioonanalüüs rühmapoliitika **sätted** > **Dispersioonanalüüs** > poliitika nimi > **Seostada VMM pilve**.

    ![Poliitika dispersioonanalüüs](./media/site-recovery-vmm-to-vmm/policy-associate.png)

### <a name="prepare-for-offline-initial-replication"></a>Ühenduseta algse kopeerimisel ettevalmistamine

Mida saab teha ühenduseta kopeerimisel algse andmete koopia. Saate koostada see järgmiselt:

- Andmeallika server, määrake andmete eksportimine toimub asukoha tee. Ekspordi tee VMM teenusega NTFS ja ühiskasutus õiguste määramine Täielik kasutusõigus. Sihtrakenduse server, Määrake tee asukohta, kust ilmneb andmete importimine. Seda impordi teed samu õigusi määrata.
- Kui importimine või eksportimine tee on ühiskasutuses, määrata administraator, Power kasutaja, Prindi tehtemärk või serveri tehtemärk rühmakuuluvus VMM teenusekonto, kus ühiskasutuse asub kaugarvutis.
- Kui kasutate mis tahes käivitada kontode lisamiseks hosts joontega importimise ja eksportimise määramine lugemine ja kirjutusõiguste VMM kontodele Käivita kasutajana.
- Importimise ja eksportimise ühtlasi ei tohi asuda kasutada ka Hyper-V hosti serveri arvutitest, kuna loopback konfiguratsiooni ei toeta Hyper-V.
- Active Directory, iga Hyper-v hosti server, mis sisaldab virtuaalmasinates, mida soovite kaitsta, lubamine ja konfigureerimine piiratud delegeerimine usaldusväärsuse kaugarvutite, mille importimise ja eksportimise teed asuvad, järgmiselt:
    1. Selle domeenikontrolleri, avage **Active Directory kasutajad ja arvutid**.
    2. Klõpsake konsoolipuus **DomainName** > **arvutites**.
    3. Paremklõpsake Hyper-V hosti serveri nimi > **Atribuudid**.
    4. Klõpsake menüü **delegeerimine** **usaldada delegeerimine määratud teenustele arvuti**.
    5. Klõpsake **mis tahes autentimise protokoll kasutada**.
    6. Klõpsake nuppu **Lisa** > **kasutajate ja arvutites**.
    7. Tippige nimi selle arvuti, mis hostib tee ekspordi > **OK**. Saadaolevad teenused loendis, hoidke all juhtklahvi (CTRL) ja klõpsake nuppu **cifs** > **OK**. Korrake impordi tee majutava arvuti nimi. Korrake vajadusel täiendavad Hyper-V hosti serverid.


### <a name="configure-network-mapping"></a>Võrgu kaardistamine konfigureerimine

Saate seadistada võrgu lähte- ja võrgu.

- Kiire ülevaade sellest, millised võrgu kaardistamine [lugemine](#prepare-for-network-mapping) ei. Klõpsake lisamine [Lugege](site-recovery-network-mapping.md) põhjalikumalt selgituse.
- Veenduge, et virtuaalmasinates VMM serverites on VM võrku ühendatud.

Konfigureerige vastendamise järgmiselt:

1. **Sätete** > **Saidi taastamise taristu** > **Võrgu vastendamise** > **võrgu vastendused** nuppu **+ võrgu vastendamise**.

    ![Võrgu kaardistamine](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. **Vastenduse lisamine võrgu** menüü valige allikas ja suunata VMM serverites. Seotud VMM serverid VM võrkude alla laaditakse.
3. **Source network**, valige seotud esmane VMM server VM võrkude loendist soovitud võrk.
6. Valige **Target võrgus** võrk, mida soovite kasutada teisene VMM Server. Klõpsake nuppu **OK**.

    ![Võrgu kaardistamine](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Mis juhtub, kui võrgu kaardistamine algab siit.

- Mis tahes olemasolevaid koopia virtuaalmasinates, allika VM võrgu teineteisele ühendatakse target VM võrgu.
- Pärast dispersioonanalüüs ühendatakse uue andmeallika VM võrku ühendatud virtuaalmasinates target vastendatud võrku.
- Kui te muuta olemasolevat vastendamise uue võrguga, ühendatakse koopia virtuaalmasinates uute sätete abil.
- Kui target võrgus on mitu alamvõrku ja üks nende alamvõrku on alamvõrgu allika virtuaalse masina asub sama nimi, siis koopia virtuaalse masina ühendatakse selle target alamvõrgu Tõrkesiirde pärast. Kui pole target alamvõrgu kattuvad nimega, virtuaalse masina ühendatud esimese alamvõrgu võrgus.

### <a name="configure-storage-mapping"></a>Konfigureerige salvestusruumi vastendamine
Vaikimisi kui teil korrata target Hyper-V hosti server, virtuaalse masina allikas Hyper-V hosti server kopeeritud andmed on salvestatud target Hyper-V Server Hyper-V halduri ettevõttevälise vaikeasukoha. Kopeeritud andmete talletuskoht rohkem kontrolli, saate konfigureerida salvestusruumi kaardistamine<br/><br/> Salvestusruumi vastenduse konfigureerimiseks peate häälestamine salvestusruumi liigitusi allikas ja suunata VMM serverid enne alustamist juurutamise. Praegu ei toetata salvestusruumi vastenduse uue Azure portaali kaudu. Siiski saate lubatud PowerShelli kaudu. [Lisateavet leiate teemast](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-6-configure-storage-mapping).

## <a name="step-5-capacity-planning"></a>Juhis 5: Võimsuse planeerimine

Nüüd, kui teil on oma basic taristu saate häälestada saate mõelda võimsuse planeerimine ja aru saada, kas teil on vaja täiendavaid ressursse.

Mõni Exceli-põhine võimsus Plaanur aitavad eraldada õige ressursside allika keskkonna, saidi taastamise komponendid, võrgunduse ja pakub saidi taastamine. Saate käivitada soovitud Plaanur kiirrežiimis valmisväärtusi põhinevad keskmine arv VMs, ketast ja salvestusruumi või üksikasjalik režiimis, kus saate sisestada töökoormus tasandi andmed. Enne alustamist peate:

- Koguge teavet dispersioonanalüüs keskkonna, sh VMs kohta VMs ketast ja ketta salvestusruumi.
- Hindamiseks igapäevane muutmine (piimapütt) peate delta kopeeritud andmete jaoks. [Võimsus Plaanur Hyper-V koopia jaoks](https://www.microsoft.com/download/details.aspx?id=39057) abil saate seda teha.

1.  Klõpsake tööriista alla laadida ja seejärel käivitage see **alla laadida** . [Lugege artiklit](site-recovery-capacity-planner.md) , mis kaasneb tööriist.
2.  Kui olete lõpetanud valige **Jah** **on käivitate võimsus Plaanur**?

    ![Võimsuse planeerimine](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="control-bandwidth"></a>Juhtelemendi läbilaskevõime

Pärast seda, kui olete kogunud reaalajas delta dispersioonanalüüs teabe võimsus Plaanur Hyper-V koopia, Exceli-põhine võimsus Plaanur tööriista abil saate arvutada vajalikku läbilaskevõimet kopeerimise (ekstraktimine ja delta). Määrata läbilaskevõimet kasutatakse dispersioonanalüüs saate konfigureerida NetQos poliitika rühmapoliitika või Windows PowerShelli abil. On paar võimalust, saate seda teha.

**PowerShelli** | **Üksikasjad**
--- | ---
**Uus - NetQosPolicy "QoS sihtkoha alamvõrgu"** | Throttle liikluse Hyper-V Host, kus töötab Windows Server 2012 R2 teisene alamvõrgu. Kui teie alamvõrku esmaseid ja teiseseid erinevad kasutada.
**Uus - NetQosPolicy "QoS, et sihtport"** | Throttle liikluse Hyper-V Host, kus töötab Windows Server 2012 R2 sihtkoha pordi.
**Uus - NetQosPolicy "Ahendamise liiklus VMM"** | Throttle vmms.exe liiklus. See on throttle Hyper-V liikluse ja Live migreerimise liikluse. Saate sobitada IP-protokollid ja portide piiritlemiseks.

Saate kasutada läbilaskevõime paksus sätted või piirata liikluse bittide teisese kohta. Kui te kasutate klaster, peate selleks kõik kobar sõlme. Lisateavet leiate järgmistest teemadest.


- [Pidurdamise Hyper-V koopia](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/) liikluse Thomas Maurer ajaveeb
- Lisateavet [cmdlet-käsu New-NetQosPolicy](https://technet.microsoft.com/library/hh967468.aspx).


## <a name="step-6-enable-replication"></a>Samm 6: Luba dispersioonanalüüs

Nüüd lubada kopeerimine järgmiselt:

1. Klõpsake **Samm 2: rakenduse ise** > **Allikas**. Kui olete lubanud dispersioonanalüüs esimest korda, klõpsake **+ ise** võlvkelder lubamiseks dispersioonanalüüs arvutite jaoks.

    ![Lubada kopeerimine](./media/site-recovery-vmm-to-vmm/enable-replication1.png)

2. **Andmeallika** tera > valige VMM server ja pilves, kus asuvad Hyper-V hosts, mida soovite korrata. Klõpsake nuppu **OK**.

    ![Lubada kopeerimine](./media/site-recovery-vmm-to-vmm/enable-replication2.png)

3. Kontrollige **Target** tera teisene VMM server ja pilve.
4. Valige **virtuaalmasinates** VMs, mida soovite kaitsta loendist.

    ![Luba virtuaalse masina kaitse](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

Saate jälgimiseks **Lubamine kaitse** toimingu sätted > **töö** > **saidi taastamise tööd**. Pärast **Viimistlemine kaitse** töö töötab virtuaalse masina on valmis Tõrkesiirde.


>[AZURE.NOTE] Samuti saate lubada kaitse virtuaalmasinates VMM konsooli. Klõpsake tööriistaribal virtuaalse masina atribuudid nuppu **Luba kaitse** > **Azure saidi taastamise** menüü.

Kui olete lubanud dispersioonanalüüs saate vaadata atribuudid **sätted**VM > **Kopeeritud üksused** > vm nime. **Essentialsi** armatuurlaual näete VM ja tema oleku teabe kopeerimine poliitika kohta. Lisateabe saamiseks klõpsake nuppu **Atribuudid** .

### <a name="onboard-existing-virtual-machines"></a>Olemasoleva pardal virtuaalmasinates

Kui teil on olemasolevaid VMM virtuaalmasinates, mis on imitatsiooniga Hyper-V koopia abil saate rongis neile Azure saidi taastamise kaitse järgmiselt:

1. Veenduge, et Hyper-V server hosting olemasoleva VM asub esmane pilves ja et Hyper-V server hosting koopia virtuaalse masina asub teisel pilveteenuses.
2. Veenduge, et dispersioonanalüüs poliitika on konfigureeritud lubama esmane VMM pilve.
2. Lubada kopeerimine esmane virtuaalse masina jaoks. Azure'i saidi taastamine ja VMM tagab, et sama koopia host ja virtuaalse masina tuvastatakse ja Azure saidi taastamise taaskasutamine ja kopeerimise abil määratud sätete taastamiseks.


## <a name="step-7-test-your-deployment"></a>Juhis 7: Testige oma juurutamine

Testige oma juurutuse saate käivitage test Tõrkesiirde ühe virtuaalse masina jaoks või taastamise kava, mis sisaldab ühe või mitme virtuaalmasinates loomine.

### <a name="prepare-for-failover"></a>Tõrkesiirde ettevalmistamine

- Juurutamise täielikult kontrollimiseks peate infrastruktuur tiražeeritud masina õigesti töötada. Kui soovite testida Active Directory ja DNS-i saate luua virtuaalse masina nimega domeenikontrolleri DNS- ja kopeerida see Azure'i Azure saidi taastamise abil. Lugege rohkem tekstivormingus [test Tõrkesiirde kaalutluste kohta Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Selles artiklis antud juhiseid kirjeldatakse käivitage test Tõrkesiirde pole võrguga. See suvand testib VM üle nurjub, kuid ei testida VM võrgu sätteid. [Lisateavet](site-recovery-failover.md#run-a-test-failover) muude võimaluste kohta.
- Kui soovite käivitada planeerimata Tõrkesiirde asemel test Tõrkesiirde võtke arvesse järgmist.

    - Võimaluse korral tuleks sulgeda esmane masinad enne käivitamist planeerimata Tõrkesiirde. See tagab, et teil pole nii lähte-kui ka koopia masinad töötab samal ajal.
    - Kui käivitate planeerimata Tõrkesiirde lõpetab andmete kopeerimine esmane masinad nii, et kõik andmed delta ei üle pärast planeerimata Tõrkesiirde hakkab. Lisaks kui käivitate planeerimata Tõrkesiirde taastamis see töötab seni, kuni olete lõpetanud, isegi kui ilmneb tõrge.




### <a name="run-a-test-failover"></a>Käivitage test Tõrkesiirde

1. Nurjumise üle ühe VM **sätted** > **Kopeeritud üksused**, klõpsake VM > **+ Test Tõrkesiirde**.

    ![Test Tõrkesiirde](./media/site-recovery-vmm-to-vmm/test-failover.png)

2. Kasutada taastamise kava **sätted** > **Taastamine lepingud**, paremklõpsake leping > **Test Tõrkesiirde**. Luua enam kavandamine, [järgige neid juhiseid](site-recovery-create-recovery-plans.md).
2. Valige **Test Tõrkesiirde**, **pole**. Selle suvandi abil saate testida VM ei üle oodatud viisil, kuid tiražeeritud VM ei ühendatud võrgule.

    ![Test Tõrkesiirde](./media/site-recovery-vmm-to-vmm/test-failover1.png)

3. Klõpsake nuppu **OK** , et alustada selle Tõrkesiirde. Saate jälgimiseks, klõpsates VM selle atribuudid avamiseks või **Test Tõrkesiirde** töö **sätted** > **töö** > **saidi taastamise tööd**.
4. Kui Tõrkesiirde töö **lõpuleviimine testimine** etapp, tehke järgmist.

    -  Saate vaadata teisene VMM pilveteenuses VM koopia.
    -  Klõpsake nuppu testi Tõrkesiirde häälestuse lõpuleviimiseks **lõpuleviimine test** .
    -  Klõpsake nuppu **märkmed** salvestada ja salvestada märkusi, mis on seotud test Tõrkesiirde.

5. Testi virtuaalse masina luuakse sama nimega host, millel on olemas koopia virtuaalse masina Host. See on lisatud, kus asub koopia virtuaalse masina sama pilveteenusesse.
6. Pärast selle VMs start edukalt nuppu **testi Tõrkesiirde on lõpule viidud**. Selles etapis kustutatakse kõik saidi taastamine test Tõrkesiirde ajal automaatselt loodud elemendid.  

    > [AZURE.NOTE] Kui test Tõrkesiirde kestab kauem kui kaks nädalat lõpuleviimist jõuga.



### <a name="update-dns-with-the-replica-vm-ip-address"></a>DNS-i värskendada koopia VM IP-aadress

Pärast Tõrkesiirde VM koopia ei pruugi olla esmane virtuaalse masina nimega sama IP-aadress.

- Virtuaalmasinates update DNS-i server, mida nad kasutavad pärast Alustuseks.
- Saate ka käsitsi värskendada DNS-i järgmiselt:

#### <a name="retrieve-the-ip-address"></a>Saate tuua IP-aadress

Käivitage see valimi skript toomiseks IP-aadress.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="update-dns"></a>DNS-i värskendamine

Käivitage see valimi skripti värskendada DNS-i, täpsustades proovisite peidust välja tuua, kasutades eelmises valimi skripti IP-aadress.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

## <a name="next-steps"></a>Järgmised sammud

Pärast juurutamise on häälestatud ja töötab, [Lugege lisateavet](site-recovery-failover.md) eri tüüpi failovers kohta.
