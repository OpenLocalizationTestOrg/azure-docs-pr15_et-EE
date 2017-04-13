<properties
    pageTitle="Azure'i virtuaalmasinates Hyper-V VMM pilved paljundada | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse, kuidas kopeerida Hyper-V virtuaalmasinates asuvad System Center VMM pilved Azure Hyper-V hosts."
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
    ms.topic="hero-article"
    ms.date="05/06/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure"></a>Azure'i virtuaalmasinates Hyper-V VMM pilved paljundada

> [AZURE.SELECTOR]
- [Azure'i portaal](site-recovery-vmm-to-azure.md)
- [PowerShelli - ressursihaldur](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klassikaline portaal](site-recovery-vmm-to-azure-classic.md)
- [PowerShelli – klassikaline](site-recovery-deploy-with-powershell.md)



Azure'i saidi taastamise teenuse aitab teie ettevõtte järjepidevus ja avariijärgne taastamine (BCDR) strateegia orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates ja füüsilise serveri. Masinad saab paljundada Azure'i või sekundaarne asutusesisese andmekeskuse. Kiire ülevaate saamiseks lugege [mis on Azure saidi taastamise?](site-recovery-overview.md).

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse, kuidas kopeerida Hyper-V virtuaalmasinates Hyper-V hosti serverites, mis asuvad VMM privaatne pilved Azure saidi taastamine juurutamiseks.

See artikkel sisaldab eeltingimuste seda stsenaariumi ja näidatakse, kuidas häälestada saidi taastamise hoidla, saada Azure saidi taastamise pakkuja VMM-i serverisse installitud, registreerida serveri vault, Azure storage konto lisamine installida Azure taastamise teenused agendi Hyper-V hosti serverid, VMM pilved, mis rakenduvad kogu kaitstud virtuaalmasinates kaitse sätete konfigureerimine , ja seejärel lubada nendes virtuaalmasinates kaitse. Lõpule jõuab testida Tõrkesiirde veendumaks, et kõik töötab ootuspäraselt.

Postitada kommentaare või küsimusi allosas käesoleva artikli või [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Arhitektuur

![Arhitektuur](./media/site-recovery-vmm-to-azure-classic/topology.png)

- Azure saidi taastamise pakkuja on installitud on VMM saidi taastamine juurutamisel ja VMM server on registreeritud saidi taastamise hoidla. Pakkuja suhtleb saidi taastamine dispersioonanalüüs korraldamise käsitlema.
- Azure taastamise teenused agendi on installitud Hyper-V hosti serverites saidi taastamine juurutamisel. Käsitlemisel andmete kopeerimise Azure salvestusruumi.


## <a name="azure-prerequisites"></a>Azure'i eeltingimused

Siit leiate Azure'i vajate.

**Nõutav** | **Üksikasjad**
--- | ---
**Azure'i konto**| Peate [Microsoft Azure'i](https://azure.microsoft.com/) kontosse. Saate alustada [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). [Lisateavet leiate teemast](https://azure.microsoft.com/pricing/details/site-recovery/) saidi taastamine hindade kohta.
**Azure'i salvestusruum** | Peate konto Azure storage kopeeritud andmete talletamiseks. Kopeeritud andmed on talletatud Azure salvestusruumi ja Azure VMs on kedratud üles Tõrkesiirde korral. <br/><br/>Vajate [standard geograafilise liigne salvestusruumi konto](../storage/storage-redundancy.md#geo-redundant-storage). Konto peab piirkonna saidi taastamine teenuste ja olema sama tellimusega seostatud. Pange tähele selle kopeerimine premium salvestusruumi kontod pole praegu toetatud ja seda ei tohi kasutada.<br/><br/>[Lugege](../storage/storage-introduction.md) Azure'i salvestusruumi.
**Azure'i võrgu** | Peate Azure virtuaalse võrgu, mis Azure VMs loob ühenduse Tõrkesiirde korral. Azure virtuaalse võrgu peab olema sama piirkonna saidi taastamise hoidla.

## <a name="on-premises-prerequisites"></a>Kohapealse eeltingimused

Siin on teil vaja kohapealse.

**Nõutav** | **Üksikasjad**
--- | ---
**VMM** | Teil peab olema vähemalt üks VMM serveris juurutatud füüsilise või virtuaalse autonoomse serveri või virtuaalse kobar. <br/><br/>VMM server peaks töötama System Center 2012 R2 kumulatiivne uusimate värskendustega.<br/><br/>Peate vähemalt üks cloud VMM Server on konfigureeritud.<br/><br/>Allikas cloud, mida soovite kaitsta peab sisaldama vähemalt ühte VMM hosti rühmad.<br/><br/>VMM pilved häälestamise kohta lisateabe saamiseks [tutvustust: loomine privaatne pilved System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) Keith Mayer ajaveebist.
**Hyper-V** | Peate Hyper-V hosti serverite või kogumite VMM pilveteenuses. Host server peaks olema ja üks või mitu VMs. <br/><br/>Hyper-V server peab töötama vähemalt **Windows Server 2012 R2** Hyper-V roll või **Microsoft Hyper-V Server 2012 R2** ja olete installinud uusimaid värskendusi.<br/><br/>Mis tahes Hyper-V server, mis sisaldab VMs, mida soovite kaitsta peavad asuma VMM pilveteenusesse.<br/><br/>Kui kasutate operatsioonisüsteemi Hyper-V kobar märget selle kobar ta ei luuakse automaatselt, kui teil on staatiline IP address-põhiste kobar. Peate kobar ta käsitsi konfigureerimine. [Lisateavet leiate teemast](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) Aidan Finn ajaveebikirje.
**Kaitstud masinad** | VMs, mida soovite kaitsta järgima [Azure nõudeid](site-recovery-best-practices.md#azure-virtual-machine-requirements).


## <a name="network-mapping-prerequisites"></a>Võrgu vastenduse eeltingimused
Kui kasutate kaitse virtuaalmasinates Azure võrgu vastenduse kaartidel vahel VM VMM-i serverisse ja suunata Azure võrkude lubamiseks järgmist:

- Kõik masinad mis Tõrkesiirde samas võrgus saate luua ühenduse üksteisest, olenemata sellest, millist taastamise kava need on.
- Kui võrgu lüüsi on seatud Azure võrgu häälestamine, virtuaalmasinates saate luua ühenduse muude kohapealse virtuaalmasinates.
- Kui konfigureerite võrgu vastenduse ainult virtuaalmasinates mis ei õnnestu üle sama taastamine lepingus on võimalik omavahel ühendada pärast Tõrkesiirde Azure.

Kui soovite võrgu kaardistamine juurutada, läheb teil vaja järgmist:

- Virtuaalmasinates, mida soovite kaitsta VMM-i serverisse peaks olema ühendatud VM võrk. Selle võrgu peaks olema seotud loogika võrk, mis on seotud pilveteenuses.
- Azure'i võrgustik, millele saab ühendada tiražeeritud virtuaalmasinates pärast Tõrkesiirde. Valite selle võrgu Tõrkesiirde ajal. Võrgu peaks olema sama piirkonna tellimuse Azure saidi taastamine.

Ettevalmistamine võrgu vastendamise järgmiselt:

1. [Lugege](site-recovery-network-mapping.md) võrgu vastenduse nõuetele.
2. Ettevalmistused VM võrkude VMM:

    - [Loogilise võrgu häälestamine](https://technet.microsoft.com/library/jj721568.aspx).
    - [VM võrgu häälestamine](https://technet.microsoft.com/library/jj721575.aspx).


## <a name="step-1-create-a-site-recovery-vault"></a>Samm 1: Loo saidi taastamise hoidla

1. Logige sisse [Haldusportaali](https://portal.azure.com) VMM Server soovite registreerida.
2. Klõpsake nuppu **andmeteenused** > **taastamise teenused** > **saidi taastamise hoidla**.
3. Klõpsake nuppu **Loo uus** > **kiire loomine**.
4. Sisestage väljale **nimi**sõbralik nimi, mis tähistavad vault.
5. **Piirkond**, valige piirkonnas vault jaoks. Märkige ruut toetatud regioonide leiate [Azure'i saidi taastamise hinnad üksikasjad](https://azure.microsoft.com/pricing/details/site-recovery/)geograafilised saadavus.
6. Klõpsake nuppu **Loo vault**.

    ![Uue Vault](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Kontrollige, et kinnitada, et vault on loodud. Vault kirjas **aktiivseks** taastamise teenused avalehele.

## <a name="step-2-generate-a-vault-registration-key"></a>Samm 2: Luua vault registreerimise võti

Luua autoriloomingut registreerimise võti. Pärast Azure saidi taastamise pakkuja Laadige alla ja installige see VMM server, saate selle klahvi abil registreerida VMM server autoriloomingut.

1. Klõpsake lehe **Taastamine teenused** vault Kiirkäivituse lehe avamiseks. Lühijuhend saate avada ka ikooni abil.

    ![Lühijuhend ikoon](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)

2. Valige ripploendis, **anda kohapealse VMM saidi ja Microsoft Azure'i vahel**.
3. **Ettevalmistused VMM serverid**, klõpsake nuppu **Genereeri registreerimise võti** fail. Võtme faili luuakse automaatselt ja 5 päeva pärast seda, kui see on loodud kehtib. Kui ei pääsete Azure portaali VMM server, peate selle faili kopeerimisel serverisse.

    ![Registreerimise võti](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Samm 3: Installida Azure saidi taastamise pakkuja

1. **Lühijuhendis** > **ettevalmistamine VMM serverid**, valige pakkuja installifail uusima versiooni hankimiseks **Alla laadida Microsoft Azure taastamise pakkujalt installi VMM serverites** .
2. Käivitage selle faili VMM-i serverisse.

    >[AZURE.NOTE] Kui VMM juurutatakse klaster ja installite selle pakkuja esimest korda installimine aktiivse sõlme ja lõpule viimiseks autoriloomingut VMM server registreerimiseks. Installige pakkuja teisi sõlmi. Pange tähele, et kui värskendate pakkuja peate versioonile kõik sõlmed, kuna need peaks kõik arvutis olema sama pakkuja versioon.

3. Installiprogramm ei prerequirements sisse ja taotleb õiguste VMM teenuse pakkuja häälestamise alustamiseks peatada. Teenuse VMM taaskäivitatakse automaatselt, kui install on lõpule viidud. Kui installite VMM kobar küsitakse peatamiseks kobar roll.

4. **Microsoft Update** saate valida rakenduses värskendusi. See säte on lubatud pakkuja vastavalt teie Microsoft Update installib värskendused.

    ![Microsofti värskendused](./media/site-recovery-vmm-to-azure-classic/updates.png)


5.  Installi asukoht pakkuja väärtuseks ** <SystemDrive>\Programmifailid\Microsoft System Center 2012 R2\Virtual masina Manager\bin**. Klõpsake nuppu **Installi**.

    ![InstallLocation](./media/site-recovery-vmm-to-azure-classic/install-location.png)

6. Pärast pakkuja installimist klõpsake **registreerida** registreerida serveri autoriloomingut.

    ![InstallComplete](./media/site-recovery-vmm-to-azure-classic/install-complete.png)

9. Kontrollige **Vault nimi**name server registreeritakse Vault. Klõpsake nuppu *edasi*.

    ![Server registreerimine](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)

7. Määrake **Interneti-ühenduse** pakkuja serveris VMM Interneti-ühenduse loomise viisi. Valige **Loo ühendus olemasoleva puhverserveri sätted** on vaikimisi Interneti-ühenduse sätteid server on konfigureeritud kasutama.

    ![Interneti-sätted](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

    - Kui soovite kasutada kohandatud puhverserveri peaks häälestamist enne installimist pakkuja. Kohandatud puhverserveri sätete konfigureerimisel käivituvad test puhverserveri ühenduse kontrollimiseks.
    - Kui kasutate kohandatud puhverserveri või teie vaikimisi puhverserver nõuab autentimist, peate sisestama puhverserveri üksikasjad, sh puhverserveri aadressi ja pordi.
    - Järgmiste URL-ide peaks olema VMM Server ja Hyper-v hostide kaudu juurdepääsetavad
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Luba [Azure'i andmekeskuse IP-vahemikke](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja HTTPS (443) protokollis kirjeldatud IP-aadressid. Mida oleks valge nimekiri IP-aadresside vahemikud Azure piirkond, mida soovite kasutada, ja mis Lääne US.
    - Kui kasutate kohandatud puhverserveri VMM RunAs konto (DRAProxyAccount) luuakse automaatselt määratud puhverserveri mandaadi abil. Puhverserveri konfigureerida nii, et selle konto saate edukalt autentimiseks. VMM RunAs konto sätteid saate muuta VMM konsooli. Selle tegemiseks **sätete** tööruumi avada, laiendage **Turvalisus**, käsku **Käivita kontod**ja seejärel muuta parooli DRAProxyAccount. Peate nii, et see säte jõustub VMM teenust uuesti.


8. Valige **Registreerimise võti**, võti, mida saate alla laadida Azure saidi taastamine ja kopeeritud VMM Server.


10.  Krüptimise säte kasutatakse ainult siis, kui te olete imitatsiooniga Hyper-V VMs VMM pilved Azure. Kui te olete imitatsiooniga saidi seda ei kasutata.

11.  **Serveri nimi**Määrake tuvastamiseks VMM server autoriloomingut sõbralik nimi. Määrake kobar konfiguratsiooni VMM kobar rolli nimi.
12.  **Sünkroonimine pilveteenuse metaandmete** , valige kas soovite sünkroonida kõik parkimise VMM server metaandmete vault. Seda toimingut tuleb ainult juhtub üks kord igale serveris. Kui te ei soovi sünkroonida kõik pilved, saate jätke see säte on märkimata ja sünkroonida iga pilv eraldi cloud atribuutide VMM konsooli.

13.  Klõpsake nuppu **Järgmine** lõpule viia. Pärast registreerimist tuuakse metaandmete serverist VMM Azure saidi taastamine. Server kuvatakse lehel **Servers** autoriloomingut vahekaardil **VMM serverid** .

    ![Lastpage](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

Pärast registreerimist tuuakse metaandmete serverist VMM Azure saidi taastamine. Server kuvatakse lehel **serverid** autoriloomingut vahekaardil **VMM serverid** .

### <a name="command-line-installation"></a>Käsurea installimine

Azure saidi taastamise pakkuja saab installida ka järgmised käsurea kaudu. See meetod saab installida pakkuja lisamine Server Core jaoks Windows Server 2012 R2.

1. Laadige pakkuja installimise faili ja registreerimise võti kausta. Näide: C:\ASR.
2. System Center virtuaalse masina Manager teenuse peatamine
3. Kaudu käsuviipa, eraldada pakkuja Installeri abil need käsud:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Provideri installimine järgmiselt:

        C:\ASR> setupdr.exe /i

5. Registreerida pakkuja järgmiselt:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Kui parameetrid on järgmised:

 - **/Credentials** : kohustuslik parameeter, mis määrab, kus asub registreerimise võtme faili asukoht  
 - **/FriendlyName** : kohustuslik parameeter Hyper-V hosti server Azure'i saidi taastamine portaalis kuvatavat nime.
 - **/EncryptionEnabled** : valikuline parameeter, et määrata, kas soovite krüptimise oma virtuaalmasinates Azure (rest krüptimise) juures. Faili nimi peab olema **pfx** laiendamine.
 - **/proxyAddress** : valikuline parameeter, mis määrab puhverserveri aadressi.
 - **/proxyport** : valikuline parameeter, mis määrab puhverserveri pordi.
 - **/proxyUsername** : valikuline parameeter, mis määrab puhverserveri kasutajanimi.
 - **/proxyPassword** : valikuline parameeter, mis määrab puhvri parooli.  


## <a name="step-4-create-an-azure-storage-account"></a>Samm 4: Azure'i salvestusruumi konto loomine

1. Kui teil pole Azure storage konto nuppu **konto Azure salvestusruumi lisamine** konto loomiseks.
2. Loo konto geo-dispersioonanalüüs lubatud. See peab piirkonna Azure saidi taastamise teenuste ja olema sama tellimusega seostatud.

    ![Salvestusruumi konto](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [AZURE.NOTE] [Migreerimise salvestusruumi kontode](../resource-group-move-resources.md) üle ühe tellimuse ressursi rühma või tellimustes salvestusruumi kontod kasutatakse juurutamine saidi taastamine ei toetata.

## <a name="step-5-install-the-azure-recovery-services-agent"></a>Juhis 5: Installida Azure taastamise teenused Agent

Installida Azure taastamise teenused agendi iga Hyper-V hosti server VMM pilveteenuses.

1. Klõpsake **Kiirkäivituse** > **allalaadimine Azure saidi taastamine Services Agent ja installige hosts** agent installifail uusima versiooni hankimiseks.

    ![Installige taastamine Services Agent](./media/site-recovery-vmm-to-azure-classic/install-agent.png)

2. Käivitage installifail iga Hyper-V hosti server.
3. **Eeltingimused märkige** lehel klõpsake nuppu **edasi**. Vajaliku puudu installitakse automaatselt.

    ![Eeltingimused taastamine Services Agent](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)

4. Määrake **Installimise sätted** lehel, kuhu soovite installida agent ja valige vahemälu asukoht, kuhu on installitud metaandmed. Klõpsake nuppu **Installi**.
5. Pärast installi lõpuleviimist klõpsake nuppu **Sule** viisardi lõpuleviimine.

    ![Registri MARS Agent](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Käsurea installimine

Microsoft Azure taastamise Services Agent saate installida ka selle käsu käsurea kaudu.

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>Samm 6: Konfigureerimine pilve sätted

Pärast VMM server on registreeritud, saate selle konfigureerida cloud sätted. Märkisite selle ruudu **Sünkroonimine pilveteenuse andmete vault** installimisel pakkuja nii, et kuvataks kõik parkimise VMM server <b>Kaitstud üksuste</b> menüüs autoriloomingut.

![Avaldatud pilveteenuses](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. Klõpsake lehel Kiirkäivituse **VMM pilved kaitse häälestamine**.
2. Klõpsake menüü **Kaitstud üksused** soovite konfigureerida ja minge menüüsse **konfigureerimine** pilve.
3. Valige **Target** **Azure**.
4. Valige **Salvestusruumi konto** kopeerimise kasutatava Azure storage konto.
5. Seadke **Krüpti talletatud andmed** **välja**. See säte määrab, et andmete krüptitud kopeeritud kohapealse saidi ja Azure vahel.
6. **Kopeerige sagedus** jätta vaikesäte. See väärtus saate määrata, kui sageli tuleks andmete sünkroonitud lähte- ja asukohtade vahel.
7. **Säilita taastamise punkte jaoks**, jätke vaikesäte. Vaikimisi väärtus on null, talletatakse ainult Viimane taastamine punkti esmane virtuaalse masina koopiat hosti serveris.
8. **Sagedus rakenduse ühtsete pilte**, jätke vaikesäte. See väärtus täpsustab, kuidas sageli hetktõmmiseid luua. Hetktõmmiseid Kasuta helitugevuse vari Service (VSS) on rakenduste ühtsete olekus kui hetktõmmis.  Kui teil on väärtus, veenduge, et see on väiksem kui saate konfigureerida täiendavaid taastamine punktide arv.
9. Määrake **Dispersioonanalüüs alguskellaaeg**, kui algne koopia Azure'i andmed peab algama. Ajavöönd Hyper-V hosti server kasutab. Soovitame ajastada algse dispersioonanalüüs aegsasti ajal.

    ![Pilveteenuse dispersioonanalüüs sätted](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

Pärast salvestamist sätted on töö luuakse ja **klõpsake vahekaarti** saab jälgida. Kõik Hyper-V hosti serverid pilveteenuses VMM allika konfigureeritakse kopeerimise.

Pärast salvestamist pilves sätteid saab muuta menüü **konfigureerimine** . Sihtkoht või target salvestusruumi konto muutmiseks peate cloud konfiguratsiooni eemaldada, ja seejärel konfigureerima pilve. Pange tähele, et kui muudate salvestusruumi konto ainult rakendatakse tehtud muudatus jaoks virtuaalmasinates pärast salvestusruumi konto on muudetud kaitse jaoks lubatud. Olemasoleva virtuaalmasinates ei migreerita salvestusruumi uuele kontole.

## <a name="step-7-configure-network-mapping"></a>Juhis 7: Konfigureerige võrgu vastendamine
Enne alustamist võrgu kaardistamine veenduge, et virtuaalmasinates VMM-i serverisse on ühendatud võrku VM. Lisaks saate luua ühe või mitme Azure virtuaalse võrgu. Pange tähele, et mitme VM võrgu saab vastendada ühe Azure võrgus.

1. Klõpsake lehel Kiirkäivituse **võrkude kaart**.
2. Valige vahekaardil **võrkude** **andmeallikate asukoht**, allikas VMM server. Valige **sihtkoht** Azure.
3. **Andmeallika** võrkude kuvatakse seotud VMM server VM võrkude loendit. **Target** võrkude kuvatakse tellimusega seotud Azure võrkude.
4. Valige allikas VM võrgu ja klõpsake nuppu **Vastenda**.
5. Valige lehel **Valige Target võrgus** siht Azure võrk, mida soovite kasutada.
6. Klõpsake märkeruutu vastenduse lõpule viia.

    ![Pilveteenuse dispersioonanalüüs sätted](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

Pärast salvestamist sätete töö käivitab vastenduse edenemist jälgida ja seda saab jälgida klõpsake vahekaarti. Mis tahes olemasolevaid koopia virtuaalmasinates, allika VM võrgu teineteisele ühendatakse target Azure'i võrkude. Pärast dispersioonanalüüs ühendatakse uue andmeallika VM võrku ühendatud virtuaalmasinates vastendatud Azure võrku. Kui te muuta olemasolevat vastendamise uue võrguga, ühendatakse koopia virtuaalmasinates uute sätete abil.

Pange tähele, et kui target võrgus on mitu alamvõrku ja üks nende alamvõrku on alamvõrgu, mis asub allika virtuaalse masina ja seejärel koopia virtuaalse masina ühendatakse selle target alamvõrgu Tõrkesiirde pärast sama nimi. Kui pole target alamvõrgu kattuvad nimega, virtuaalse masina ühendatud esimese alamvõrgu võrgus.

> [AZURE.NOTE] [Võrgu migreerimine](../resource-group-move-resources.md) üle ühe tellimuse ressursi rühma või tellimustes võrke kasutatakse juurutamine saidi taastamine ei toetata.

## <a name="step-8-enable-protection-for-virtual-machines"></a>Samm 8: Lubamine kaitse virtuaalmasinates

Pärast serverid pilved ja võrgu on õigesti konfigureeritud, saate lubada kaitse virtuaalmasinates pilveteenuses. Võtke arvesse järgmist.

- Virtuaalmasinates peab vastama [Azure nõuetele](site-recovery-best-practices.md#azure-virtual-machine-requirements).
- Kaitse lubamiseks operatsioonisüsteem ja operatsioonisüsteemi seatakse virtuaalse masina ketta atribuudid. Kui loote virtuaalse masina VMM virtuaalse masina malli abil saate atribuuti. Samuti saate nende atribuutide olemasolevaid virtuaalmasinates virtuaalse masina atribuudid vahekaartidel **üldist** ja **Riistvara konfiguratsiooni** . Kui te ei määra atribuutidest VMM kuvatakse saama need konfigureerida Azure saidi taastamise portaalis.

    ![Virtuaalse masina loomine](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![Virtuaalne seadme atribuutide muutmine](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)


1. Kaitse **Virtuaalmasinates** menüü pilves, kus asub virtuaalse masina, klõpsake nuppu **Luba kaitse** > **Lisa virtuaalmasinates**.
2. Loendist virtuaalmasinates pilves, valige see, mida soovite kaitsta.

    ![Luba virtuaalse masina kaitse](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    **Klõpsake vahekaarti, sh algse dispersioonanalüüs** **Lubamine kaitse** toimingu jälgimiseks. Pärast **Viimistlemine kaitse** töö töötab virtuaalse masina on valmis Tõrkesiirde. Pärast kaitse on lubatud ja virtuaalmasinates on kopeeritud, saate küll Azure neid vaadata.


    ![Virtuaalse masina kaitse töö](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

3. Kontrollige virtual seadme atribuutide ja muutmine vastavalt vajadusele.

    ![Veenduge, et virtuaalmasinates](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)


4. Virtuaalse masina atribuudid vahekaardil **konfigureerimine** saab muuta järgmised atribuudid.





- **Arvu võrguadapteri sihtarvutis virtual** - võrguadapteri arv on tingitud target virtuaalse masina jaoks määratud suurusega. Märkige ruut [virtuaalse masina suurus andmed](../virtual-machines/virtual-machines-linux-sizes.md#size-tables) ei toeta virtuaalse masina suurus adapterit arvu. Kui virtuaalse masina selle suurust muuta ja sätete salvestamiseks, võrguadapteri arvu muuta lehe **konfigureerimine** järgmisel avamisel. Võrguadapteri target virtuaalmasinates, on väikseima arvu võrguadapteri allika virtual arvutisse ja valitud järgmiselt virtuaalse masina suurust ei toeta võrguadapteri arvu ülempiir:

    - Kui allika arvutisse võrguadapteri arv on väiksem või võrdne arv adapterit masina sihtformaat lubatud, siis sihtkohas on samal hulgal adapterit allikana.
    - Kui adapterit allika virtual masina arv ületab lubatud target suurus ja seejärel soovitud suurus maksimum kasutada number.
    - Näiteks kui allika seadme on kaks võrguadapteri ja seadme sihtformaat toetab neli, sihtarvutis on kaks adapterit. Kui allika arvuti on kaks adapterit, kuid toetatud sihtformaat toetab ainult üks siis sihtarvutis on ainult üks adapterit.    

- **Sihtrakenduse virtuaalse masina võrgu** - millele virtuaalse masina loob ühenduse võrgu määratakse võrgu vastenduse andmeallika virtuaalse masina võrgu. Kui allika virtuaalse masina on rohkem kui üks võrguadapteri ja allika võrkude vastendatakse erinevate võrkude suunata, siis peate valima target võrgu.
- **Iga võrguadapteri alamvõrgu** - iga võrgu-adapterit, saate valida alamvõrgu, millele on nurjunud üle virtuaalse masina soovite ühendada.
- **Target IP-aadress** – kui allika virtuaalse masina võrguadapteri on konfigureeritud kasutama staatiline IP-aadress, siis võib pakkuda target virtuaalse masina IP-aadress. Selle funktsiooni kasutamiseks säilitamise pärast Tõrkesiirde allika virtuaalse masina IP-aadress. Kui IP-aadress on esitatud klõpsake mis tahes saadaval IP-aadress on antud võrguadapteri Tõrkesiirde ajal. Kui target IP-aadress on määratud, kuid kasutab juba mõni muu virtuaalse masina töötab Azure nurjub Tõrkesiirde.  

    ![Võrgu atribuutide muutmine](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

>[AZURE.NOTE] Staatiline IP-aadressiga Linux virtuaalmasinates ei toetata.

## <a name="test-the-deployment"></a>Juurutamise

Teie juurutamise saate käivitage test Tõrkesiirde jaoks ühe virtuaalse masina, või luua taastamise kava koosneb mitmest virtuaalmasinates ja käivitage test Tõrkesiirde lepingu raames.  

Test Tõrkesiirde jäljendab oma Tõrkesiirde ja taastamise süsteem isoleeritakse võrgus. Pange tähele järgmist.

- Kui soovite pärast selle Tõrkesiirde kaugtöölaua kasutamise Azure virtuaalse masina ühenduse, lubada kaugtöölaua ühendus virtuaalse masina enne, kui käivitate test Tõrkesiirde.
- Pärast Tõrkesiirde saate kasutada avaliku IP-aadressi ühenduse, virtuaalse masina Azure kaugtöölaua kasutamise. Kui soovite seda teha, veenduge, et teil pole mõnda domeeni poliitika, mis takistada ühenduse virtuaalse masina avalik aadressidel.

>[AZURE.NOTE] Parima jõudluse Tõrkesiirde valimisel Azure saamiseks veenduge, et on installitud Azure Agent kaitstud kohapeal. See aitab käivitamist kiiremini ja aitab diagnoosimise küsimuste korral. Linux agent võib olla leitud [siin](https://github.com/Azure/WALinuxAgent) - ja Windowsi agent leiate [siit](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="create-a-recovery-plan"></a>Taastamis loomine

1. Menüü **Taastamine lepingud** lisada uuele lepingule. Määrake nimi, **VMM** **Reaallika tüüp**, ja allika VMM server **Allikas**, sihtkohas saab Azure.

    ![Looge taastamise kava](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)

2. Valige lehel **Valige Virtuaalmasinates** virtuaalmasinates taastamise kava lisada. Nende virtuaalmasinates lisatakse taastamise leping vaikerühma – rühma 1. Maksimaalselt 100 virtuaalmasinates sisse ühe taastamis on testitud.

- Kui soovite kontrollida virtuaalse masina atribuutide enne lisamist leping, virtuaalse masina Cloud, kus ta lehe atribuudid nuppu kasutaja asuvad. Samuti saate konfigureerida virtuaalse masina atribuutide VMM konsooli.
- Kõik virtuaalmasinates, mis on kuvatud on lubatud kaitse. Loend sisaldab nii virtuaalmasinates kaitse ja algse dispersioonanalüüs on lõppenud ja need, mis on lubatud kaitse esialgne dispersioonanalüüs ootel lubatud. Ainult virtuaalmasinates koos algse dispersioonanalüüs lõpetatud võib nurjuda üle taastamis osana.

    ![Looge taastamise kava](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

Pärast taastamis on loodud see kuvatakse vahekaardil **Taastamine lepingud** . [Azure'i automaatika tegevusraamatud](site-recovery-runbook-automation.md) saate lisada ka taastamise kava Tõrkesiirde ajal toimingute automatiseerimiseks.

### <a name="run-a-test-failover"></a>Käivitage test Tõrkesiirde

On kaks võimalust Azure test Tõrkesiirde käivitamiseks.

- **Testimine ilma on Azure võrgu Tõrkesiirde**– seda tüüpi test Tõrkesiirde kontrollib, kas virtuaalse masina ilmub õigesti Azure. Virtuaalse masina ei mis tahes Azure'i võrku ühendatud pärast Tõrkesiirde.
- **Testige Tõrkesiirde on Azure võrguga**– seda tüüpi Tõrkesiirde kontrolle, et kogu dispersioonanalüüs keskkond on kuni ootuspäraselt ja üle selle virtuaalmasinates nurjunud ühendatakse Azure võrgu määratud sihtkohta. Alamvõrgu käsitlemiseks, katsetamiseks Tõrkesiirde alamvõrgu testi virtuaalse masina on mustriga läbi alamvõrgu koopia virtuaalse masina põhjal. See on erinevate tavaline dispersioonanalüüs, kui koopia virtuaalse masina alamvõrgu põhineb alamvõrgu allika virtuaalse masina.

Kui soovite käivitada test Tõrkesiirde jaoks virtuaalse masina lubatud kaitse Azure ilma Azure target võrk, mis ei pea te midagi ette valmistada. Käivitage test Tõrkesiirde siht Azure võrgu peate luua uue Azure võrk, mis on eraldatud Azure tootmise võrgu (vaikekäitumine kui loote uue võrgu Azure). Vaadake lisateavet [käivitage test Tõrkesiirde](site-recovery-failover.md#run-a-test-failover) .


Samuti peate häälestamine taristu tiražeeritud virtual masina õigesti töötada. Näiteks virtuaalse masina domeenikontrolleri ja DNS-i abil saab paljundada Azure'i Azure saidi taastamise abil ja testi võrgu testimine Tõrkesiirde abil saab luua. Vaadake [testida Tõrkesiirde kaalutluste kohta active directory](site-recovery-active-directory.md#considerations-for-test-failover) lõik rohkem üksikasju.

Käivitamiseks test Tõrkesiirde tehke järgmist:

1. Menüü **Taastamine lepingute** valige leping ja klõpsake nuppu **Testi Tõrkesiirde**.
2. Valige lehel **Kinnitage Test Tõrkesiirde** **pole** või teatud Azure võrgus.  Pange tähele, et kui valite pole test Tõrkesiirde kontrollib, kas virtuaalse masina kopeeritud õigesti Azure, kuid ei kontrolli oma dispersioonanalüüs võrgu konfigureerimise.

    ![Võrk](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)

3. Kui andmete krüptimine on lubatud pilves, **Krüptovõtme** valige soovitud suvand andmete krüptimine pilv sisselülitatuna installimisel pakkuja serveris VMM väljastatud serti.
4. **Klõpsake vahekaarti** saate silma peal Tõrkesiirde edenemist. Mida peaks olema näeksid virtuaalse masina testi koopia Azure'i portaalis. Kui te pole Accessi virtuaalmasinates kuni määramine kohapealse võrgu kaudu saate algatada virtuaalse masina kaugtöölaua ühendus.
5. **Täielik testimine** etapp jõudmisel on tõrkesiire klõpsake **Täieliku testida** test Tõrkesiirde häälestuse lõpuleviimiseks. Saate minna allapoole menüü **töö** jälgimiseks Tõrkesiirde ja olekut ja kõik toimingud, mida on vaja teha.
6. Pärast Tõrkesiirde küll vaadata virtuaalse masina testi koopia Azure'i portaalis. Kui te pole Accessi virtuaalmasinates kuni määramine kohapealse võrgu kaudu saate algatada virtuaalse masina kaugtöölaua ühendus. Tehke järgmist.

    1. Veenduge, et selle virtuaalmasinates edukalt käivituda.
    2. Kui soovite pärast selle Tõrkesiirde kaugtöölaua kasutamise Azure virtuaalse masina ühenduse, lubada kaugtöölaua ühendus virtuaalse masina enne, kui käivitate test Tõrkesiirde. Samuti peate lisada lõpp RDP virtual arvutisse. [Azure'i automaatika tegevusraamatud](site-recovery-runbook-automation.md) selleks saate kasutada.
    3. Pärast Tõrkesiirde kui kasutate avaliku IP-aadressi ühenduse virtuaalse masina Azure Kaugtöölaud abil tagada te ei pea iga domeeni poliitika, mis takistada ühenduse virtuaalse masina avalik aadressidel.

7.  Kui katsetamine on lõpule jõudnud, tehke järgmist.
    - Klõpsake nuppu **testi Tõrkesiirde on lõpule viidud**. Puhasta testimiskeskkonnas, automaatselt väljalülitamise ja kustutada virtuaalmasinates testi.
    - Klõpsake nuppu **märkmed** salvestada ja salvestada märkusi, mis on seotud test Tõrkesiirde.

>

## <a name="next-steps"></a>Järgmised sammud

Teavet [taastamine lepingute häälestamise](site-recovery-create-recovery-plans.md) ja [Tõrkesiirde](site-recovery-failover.md).
