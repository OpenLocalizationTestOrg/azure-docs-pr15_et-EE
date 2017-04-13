<properties
    pageTitle="Paljundada Hyper-V virtuaalmasinates VMM pilved VMM saidi | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse, kuidas paljundada Hyper-V VMs VMM pilved VMM saidi Azure saidi taastamine."
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

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a>Paljundada Hyper-V virtuaalmasinates VMM pilved VMM saidi

> [AZURE.SELECTOR]
- [Azure'i portaal](site-recovery-vmm-to-vmm.md)
- [Klassikaline portaal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShelli - ressursihaldur](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Azure'i saidi taastamise teenuse aitab teie ettevõtte järjepidevus ja avariijärgne taastamine (BCDR) strateegia orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates ja füüsilise serveri. Masinad saab paljundada Azure'i või sekundaarne asutusesisese andmekeskuse. Kiire ülevaate saamiseks lugege [mis on Azure saidi taastamise?](site-recovery-overview.md)

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse, kuidas kopeerida Hyper-V virtuaalmasinates Hyper-V hosti serverites, mis hallatakse VMM pilved VMM saidi Azure saidi taastamise abil.

See artikkel sisaldab eeltingimused, näitab teile, kuidas häälestada saidi taastamise hoidla, installida Azure saidi taastamise pakkuja allikas ja suunata VMM serverid, registreerida serverid autoriloomingut, VMM pilved kaitse sätete konfigureerimine ja seejärel lubada kaitse Hyper-V vms. Lõpule jõuab testida Tõrkesiirde veendumaks, et kõik töötab ootuspäraselt.

Postitada kommentaare või küsimusi allosas käesoleva artikli või [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Arhitektuur

Alloleval pildil on erinevaid ja Azure saidi taastamine pordid korraldamise ja kopeerimine

![E2e topoloogia](./media/site-recovery-vmm-to-vmm-classic/e2e-topology.png)

## <a name="before-you-start"></a>Enne alustamist

Veenduge, et teil on nende eeltingimuste kohas.

**Eeltingimused** | **Üksikasjad**
--- | ---
**Azure'i**| Teil on vaja [Microsoft Azure'i](https://azure.microsoft.com/) kontosse. Saate alustada [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). [Lisateavet leiate teemast](https://azure.microsoft.com/pricing/details/site-recovery/) saidi taastamine hindade kohta.
**VMM** | Peate olema vähemalt üks VMM server.<br/><br/>VMM server olema vähemalt opsüsteemi System Center 2012 SP1 kumulatiivne uusimate värskendustega.<br/><br/>Kui soovite häälestada kaitse ühe VMM serveriga, peate olema vähemalt kahe pilve server on konfigureeritud.<br/><br/>Kui soovite kasutada kaitsmise VMM serveritega, iga server peab olema vähemalt üks cloud konfigureeritud esmane VMM server, mida soovite kaitsta ja ühe pilve konfigureeritud teisene VMM server, mida soovite kasutada kaitsmise ja taastamise<br/><br/>Kõik VMM pilved peab olema seatud Hyper-V võimalus profiili.<br/><br/>Allikas cloud, mida soovite kaitsta peab sisaldama vähemalt ühte VMM hosti rühmad.<br/><br/>VMM pilved häälestamise kohta lisateabe saamiseks [tutvustust: loomine privaatne pilved System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) Keith Mayer ajaveebist.
**Hyper-V** | Teil on vaja ühe või mitme Hyper-V hosti serverites esmaseid ja teiseseid VMM hosti rühmad ja ühe või mitme virtuaalmasinates iga Hyper-V hosti server.<br/><br/>Host ja target Hyper-V serverite peab olema vähemalt opsüsteemi Windows Server 2012 Hyper-V rolliga ja olete installinud uusimaid värskendusi.<br/><br/>Mis tahes Hyper-V server, mis sisaldab VMs, mida soovite kaitsta peavad asuma VMM pilveteenusesse.<br/><br/>Kui teil on Hyper-V klaster, Pange tähele, et kobar ta ei luuakse automaatselt, kui teil on staatiline IP address-põhiste kobar. Peate kobar ta käsitsi konfigureerimine. [Lisateavet leiate teemast](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) Aidan Finn ajaveebikirje.
**Võrgu kaardistamine** | Veenduge, et tiražeeritud virtuaalmasinates optimaalselt paigutatakse teisene Hyper-V hosti serverites pärast Tõrkesiirde ja, et nad saavad ühenduse vastav VM võrgu network vastenduse saate konfigureerida. Kui konfigureerite ei võrgu kaardistamine, ei mis tahes võrku ühendatud koopia VMs, pärast Tõrkesiirde.<br/><br/>Saate seadistada võrgu juurutamisel, veenduge, et et virtuaalmasinates, allikas Hyper-V hosti server on ühendatud võrku VMM VM. Selle võrgu peaks olema seotud loogika võrk, mis on seotud pilve. < br /<br/>Sihtrakenduse pilveteenuses teisene VMM Server taastamine kasutatav peaks olema vastav VM võrgus konfigureeritud ja see omakorda peaks olema seotud vastavate loogilise võrk, mis on seostatud target pilve.<br/><br/>[Lisateavet leiate teemast](site-recovery-network-mapping.md) võrgu vastendamise kohta.
**Salvestusruumi kaardistamine** | Vaikimisi kui teil korrata target Hyper-V hosti server, virtuaalse masina allikas Hyper-V hosti server kopeeritud andmed on salvestatud target Hyper-V Server Hyper-V halduri ettevõttevälise vaikeasukoha. Kopeeritud andmete talletuskoht rohkem kontrolli, saate konfigureerida salvestusruumi kaardistamine<br/><br/> Salvestusruumi vastenduse konfigureerimiseks peate häälestamine salvestusruumi liigitusi allikas ja suunata VMM serverid enne alustamist juurutamise. [Lisateavet leiate teemast](site-recovery-storage-mapping.md).


## <a name="step-1-create-a-site-recovery-vault"></a>Samm 1: Loo saidi taastamise hoidla

1. Logige sisse [Haldusportaali](https://portal.azure.com) VMM Server soovite registreerida.

2. Laiendage **Data Services** > **Taastamise teenused** ja klõpsake nuppu **Saidi taastamise hoidla**.

3. Klõpsake nuppu **Loo uus** > **kiire loomine**.

4. Sisestage väljale **nimi**sõbralik nimi, mis tähistavad vault.

5. Valige **piirkonna** jaoks vault geograafilised piirkond. Märkige ruut toetatud regioonide leiate [Azure'i saidi taastamise hinnad üksikasjad](http://go.microsoft.com/fwlink/?LinkId=389880)geograafilised saadavus.

6. Klõpsake nuppu **Loo vault**.

    ![Vault loomine](./media/site-recovery-vmm-to-vmm-classic/create-vault.png)

Märkige ruut olekuribal, et vault on loodud. Vault kirjas **aktiivseks** taastamise teenused avalehele.

## <a name="step-2-generate-a-vault-registration-key"></a>Samm 2: Luua vault registreerimise võti

Luua autoriloomingut registreerimise võti. Pärast Azure saidi taastamise pakkuja Laadige alla ja installige see VMM server, saate selle klahvi abil registreerida VMM server autoriloomingut.

1. Klõpsake lehe **Taastamine teenused** vault Kiirkäivituse lehe avamiseks. Lühijuhend saate avada ka ikooni abil.

    ![Lühijuhend ikoon](./media/site-recovery-vmm-to-vmm-classic/quick-start-icon.png)

2. Valige rippmenüüst, **kahe kohapealse VMM saitide vahel**.
3. **Ettevalmistused VMM serverid**nuppu **Genereeri registreerimise võtme faili**. Võtme faili luuakse automaatselt ja 5 päeva pärast seda, kui see on loodud kehtib. Kui ei pääsete Azure portaali VMM server, peate selle faili kopeerimisel serverisse.

    ![Registreerimise võti](./media/site-recovery-vmm-to-vmm-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Samm 3: Installida Azure saidi taastamise pakkuja

4. Klõpsake lehel **Kiirkäivituse** **ettevalmistamine VMM serverid**, **Allalaadimine Microsoft Azure taastamise pakkujalt installi VMM serverites** pakkuja installifail uusima versiooni hankimiseks.

2. Käivitage selle faili VMM-i serverisse.

    >[AZURE.NOTE] Kui VMM juurutatakse klaster ja installite selle pakkuja esimest korda installimine aktiivse sõlme ja lõpule viimiseks autoriloomingut VMM server registreerimiseks. Installige pakkuja teisi sõlmi. Pange tähele, et kui värskendate pakkuja peate täiendada kõik sõlmed, kuna need peaks kõik arvutis olema sama pakkuja versioon.

3. Installiprogramm ei mõne **Eelse nõuete kontrollimine** ja taotlusi õiguste VMM teenuse pakkuja häälestamise alustamiseks peatada. Teenuse VMM taaskäivitatakse automaatselt, kui install on lõpule viidud. Kui installite VMM kobar küsitakse peatamiseks kobar roll.

4. **Microsoft Update** saate valida rakenduses värskendusi. See säte on lubatud pakkuja vastavalt teie Microsoft Update installib värskendused.

    ![Microsofti värskendused](./media/site-recovery-vmm-to-vmm-classic/ms-update.png)

5. Installi asukoht on seatud ** <SystemDrive>\Programmifailid\Microsoft System Center 2012 R2\Virtual masina Manager\bin**. Klõpsake pakkuja installimise alustamiseks nuppu Installi.

    ![InstallLocation](./media/site-recovery-vmm-to-vmm-classic/install-location.png)

6. Pärast pakkuja installimist klõpsake **registreerida** registreerida serveri autoriloomingut.

    ![InstallComplete](./media/site-recovery-vmm-to-vmm-classic/install-complete.png)
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

13.  Klõpsake nuppu **Järgmine** lõpule viia. Pärast registreerimist tuuakse metaandmete serverist VMM Azure saidi taastamine. Server ei kuvata **VMM**serverid > vault**serverites** .

    ![Servers](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

### <a name="command-line-installation"></a>Käsurea installimine

Azure saidi taastamise pakkuja saab installida ka käsurea kaudu. See meetod saab installida pakkuja lisamine Server CORE jaoks Windows Server 2012 R2.

1. Laadige pakkuja installimise faili ja registreerimise võti kausta. Näiteks C:\ASR.
2. System Center virtuaalse masina halduri teenuse peatamine
3. Ekstraktida pakkuja Installeri käivitades käsureale **administraatoriõigustega** need käsud:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Installige pakkuja töötab.

        C:\ASR> setupdr.exe /i

5. Pakkuja käivitades registreerimine

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Kus asuvad soovitud parameetrid:

 - **/Credentials**: kohustuslik parameeter, mis määrab, kus asub registreerimise võtme faili asukoht  
 - **/FriendlyName**: kohustuslik parameeter Hyper-V hosti server Azure'i saidi taastamine portaalis kuvatavat nime.
 - **/EncryptionEnabled**: valikuline parameeter, mida peate kasutama ainult neis VMM Azure'i stsenaarium, kui teil on vaja oma virtuaalmasinates veebisaidil korral Azure'i krüptimist. Veenduge, et esitate **pfx** -laiendiga faili nimi.
 - **/proxyAddress**: valikuline parameeter, mis määrab puhverserveri aadressi.
 - **/proxyport**: valikuline parameeter, mis määrab puhverserveri pordi.
 - **/proxyUsername**: valikuline parameeter, mis määrab puhverserveri kasutajanimi (kui puhverserver nõuab autentimist).
 - **/proxyPassword**: valikuline parameeter, mis määrab parooli koos puhverserveri autentimine (kui puhverserver nõuab autentimist).  

## <a name="step-4-configure-cloud-protection-settings"></a>Samm 4: Konfigureerimine pilve sätted

Pärast VMM serverid on registreeritud, saate konfigureerida cloud sätted. Kui märkisite selle ruudu **Sünkroonimine pilveteenuse andmete vault** pakkuja installimisel kuvatakse nii kogu parkimise VMM server autoriloomingut menüü **Kaitstud üksused** . Kui te ei ole teil saab sünkroonida teatud pilve Azure saidi taastamine VMM konsooli lehe pilve atribuudid vahekaardil **üldist** .

![Avaldatud pilveteenuses](./media/site-recovery-vmm-to-vmm-classic/clouds-list.png)

1. Klõpsake lehel Kiirkäivituse **VMM pilved kaitse häälestamine**.
2. Valige vahekaardil **VMM pilved** pilves, mida soovite konfigureerida ja minge menüüsse **konfigureerimine** .
3. **Target (sihtkoht)**, valige **VMM**.
4. Valige **sihtkoht**kohapeal VMM server, mida haldab pilves, mida soovite kasutada taastamise.
4. Valige **Target cloud**target pilveteenuses soovite Tõrkesiirde virtuaalmasinates pilveteenuses allika jaoks kasutada. Pange tähele järgmist.

    - Soovitame teil valida target pilves, mis vastab taastamine virtuaalmasinates, tuleb teil kaitsta.
    - Pilv saab ainult ühe cloud paari kuuluvad – kas esmane või target pilve.

5. **Sagedus kopeerida**, määrake sageduse andmeid tuleks sünkroonitud 5he lähte- ja asukohad. Pange tähele, et see säte on oluline ainult kui Hyper-V server töötab Windows Server 2012 R2. Muudes serverites kasutatakse vaikesäte viis minutit.
6. **Täiendavad taastamise punkte**, määrata, kas soovite luua täiendavaid taastamise punkte. Vaikeväärtus null näitab, et ainult Viimane taastamine punkti esmane virtuaalse masina on salvestatud koopiat hosti serveris. Pange tähele, et lubada mitme taastamise punkte täiendav salvestusruum pilte, mis on talletatud igas etapis taastamise jaoks. Vaikimisi luuakse taastamise punkte iga tund, nii, et iga taastamine punkti sisaldab üks tund väärtuses andmed. Taastamine punkti väärtus, mida määrate virtuaalse masina VMM konsooli ei peaks olema väiksem väärtus, mida saate määrata Azure saidi taastamise konsooli.
7. Määrake **sagedus rakenduse ühtsete pilte**, kui tihti rakenduse ühtsete hetktõmmiseid luua. Hyper-V kasutab kahte tüüpi hetktõmmiseid – standard pildi, mis pakub on kogu virtuaalse masina suureneva hetktõmmise ja rakenduse ühtsete hetktõmmise, mis viib rakenduse andmete virtual seadmes hetktõmmise punkti õigel ajal. Rakenduse ühtsete hetktõmmiseid Kasuta helitugevuse vari Service (VSS) on rakenduste ühtsete olekus kui hetktõmmis. Märkus Kui lubate rakenduse ühtsete pilte, mõjutavad jõudlus töötavad allikast virtuaalmasinates rakendused. Veenduge, et seate väärtus on väiksem kui arvu täiendavad taaste suunab teid konfigureerimine.

    ![Kaitse sätete konfigureerimine](./media/site-recovery-vmm-to-vmm-classic/cloud-settings.png)

8. **Andmete edastamiseks tihendamise**, määrake, kas kopeeritud andmed, mis on üle võimalik tihendada.
9. Määrake **autentimine**, kuidas liikluse autenditakse esmane ja taastamise Hyper-V host meiliserverite vahel. Valige HTTPS, v.a juhul, kui teil on töö Kerberos keskkond, mis on konfigureeritud. Azure'i saidi taastamine automaatselt konfigureerida HTTPS-autentimise serdid. Nõutav on pole käsitsi konfigureerimine. Kui valite Kerberos, kasutatakse Kerberose Piletite hosti serverite vastastikune autentimine. Vaikimisi avatakse 8083 ja 8084 (sert) jaoks pordi Windowsi tulemüür Hyper-V hosti serverites. Pange tähele, et see säte on asjakohane Hyper-V hosti serverites, kus töötab Windows Server 2012 R2 ainult.
10. **Portide**, muuta kopeerimise liikluse oodake lähte- ja host arvutite pordinumber. Näiteks võib seda sätet muuta, kui soovite rakendada teenuse kvaliteedi (QoS) võrgu läbilaskevõime pidurdamise määramine kopeerimise liikluse. Veenduge, et pordi pole kasutada muid ja, et see on avatud tulemüüri sätted.
11. Määrake **Dispersioonanalüüs meetod**algse dispersioonanalüüs asukohad andmeallikast andmete käsitlemise, enne tavaline kopeerimine.

    - **Võrgu kaudu**– andmete kopeerimise võrgus võib olla aeganõudev ja ressursse. Soovitame kasutada see suvand, kui pilveteenuses sisaldab virtuaalmasinates koos suhteliselt virtuaalse kõvaketta ja kui esmane saidi on ühendatud saidi kaudu lai ribalaiust. Saate määrata, et Kopeeri tuleks kohe või valige soovitud aeg. Kui kasutate võrgu dispersioonanalüüs, soovitame selle plaanimine aegsasti ajal.
    - **Ühenduseta**– seda meetodit saate määrata, et tehakse algse kopeerimise abil välise meedia. See on kasulik, kui soovite vältida degradeerumine võrgu jõudlust või geograafiliselt asukohad. Selle meetodi kasutamiseks saate määrata ekspordi asukoht allikas cloud ja impordi asukoha target pilve. Kui lubate virtuaalse masina kaitse, kopeeritakse kettaruumi ekspordi määratud asukohta. Saate saata sihtsaidi ja kopeerige see importimise kohta. Süsteem kopeerib imporditud teave virtuaalmasinates koopia.

12. Valige **Kustuta koopia virtuaalse masina** määramaks, et koopia virtuaalse masina tuleks kustutada, kui lõpetate kaitsmine virtuaalse masina pilve atribuudid vahekaardil Virtuaalmasinates **kustutamine virtuaalse masina kaitse** suvandi valimisel. See säte on lubatud, kaitse keelamisel virtuaalse masina eemaldatakse Azure saidi taastamise, virtuaalse masina saidi taastamine sätted eemaldatakse VMM konsooli ja koopia on kustutatud.

    ![Kaitse sätete konfigureerimine](./media/site-recovery-vmm-to-vmm-classic/cloud-settings-replica.png)

Pärast salvestamist sätted on töö luuakse ja **klõpsake vahekaarti** saab jälgida. Kõik Hyper-V hosti serverid pilveteenuses VMM allika konfigureeritakse kopeerimise. Pilveteenuse saab muuta menüüs **konfigureerimine** . Kui soovite muuta target asukoha või target pilveteenuses peate eemaldama cloud konfiguratsiooni, ja seejärel konfigureerima pilve.

### <a name="prepare-for-offline-initial-replication"></a>Ühenduseta algse kopeerimisel ettevalmistamine

Peate tegema järgmised toimingud ettevalmistamine algse dispersioonanalüüs ühenduseta.

- Andmeallika server, määrake andmete eksportimine toimub asukoha tee. Ekspordi tee VMM teenusega NTFS ja ühiskasutus õiguste määramine Täielik kasutusõigus. Sihtrakenduse server, Määrake tee asukohta, kust ilmneb andmete importimine. Seda impordi teed samu õigusi määrata.
- Kui importimine või eksportimine tee on ühiskasutuses, määrata administraator, Power kasutaja, Prindi tehtemärk või serveri tehtemärk rühmakuuluvus VMM teenusekonto, kus ühiskasutuse asub kaugarvutis.
- Kui kasutate mis tahes käivitada kontode lisamiseks hosts joontega importimise ja eksportimise määramine lugemine ja kirjutusõiguste VMM kontodele Käivita kasutajana.
- Importimise ja eksportimise ühtlasi ei tohi asuda kasutada ka Hyper-V hosti serveri arvutitest, kuna loopback konfiguratsiooni ei toeta Hyper-V.
- Active Directory, iga Hyper-v hosti server, mis sisaldab virtuaalmasinates, mida soovite kaitsta, lubamine ja konfigureerimine piiratud delegeerimine usaldusväärsuse kaugarvutite, mille importimise ja eksportimise teed asuvad, järgmiselt:
    1. Selle domeenikontrolleri, avage **Active Directory kasutajad ja arvutid**.
    2. Klõpsake konsoolipuus **DomainName** > **arvutites**.
    3. Paremklõpsake Hyper-V hosti serveri nimi > **Atribuudid**.
    4. Klõpsake menüü **delegeerimine** T**roosteta delegeerimine määratud teenustele arvuti**.
    5. Klõpsake **mis tahes autentimise protokoll kasutada**.
    6. Klõpsake nuppu **Lisa** > **kasutajate ja arvutites**.
    7. Tippige nimi selle arvuti, mis hostib tee ekspordi > **OK**. Saadaolevad teenused loendis, hoidke all juhtklahvi (CTRL) ja klõpsake nuppu **cifs** > **OK**. Korrake impordi tee majutava arvuti nimi. Korrake vajadusel täiendavad Hyper-V hosti serverid.

## <a name="step-5-configure-network-mapping"></a>Juhis 5: Konfigureerige võrgu vastendamine
1. Klõpsake lehel Kiirkäivituse **võrkude kaart**.
2. Valige allikas VMM server, millest soovite vastendada võrgu ja seejärel target VMM server, millele võrkude vastendatakse. Andmeallika võrkude ja nende seotud target võrkude loend kuvatakse. Võrgu, mis pole praegu vastendatud kuvatakse tühja väärtuse.
3. Valige võrgu **Network allikas** > **kaarti**. Teenuse tuvastab VM võrkude target serveris ja need kuvatakse. Klõpsake ikooni teabe vaatamiseks iga võrgu alamvõrku lähte- ja võrgu nimede kõrval.

    ![Võrgu kaardistamine konfigureerimine](./media/site-recovery-vmm-to-vmm-classic/network-mapping1.png)

4. Dialoogiboksis valige VM võrgu target VMM serverist.

    ![Valige target võrgus](./media/site-recovery-vmm-to-vmm-classic/network-mapping2.png)

5. Kui valite target võrgus, kuvatakse source network kasutavate kaitstud pilved. Saadaval target võrke, mis on seotud mis õhupalli, kasutada kaitsmise kuvatakse. Soovitame teil valida target võrgus, mis on saadaval kõigi pilved kasutate kaitse. Või saate minna VMM Server ja muuta cloud atribuutide lisamiseks loogiline võrgu vastav vm võrk, mida soovite valida.
6. Klõpsake märkeruutu vastenduse lõpule viia. Töö hakkab vastenduse jälgimiseks. Saate vaadata selle kohta, **klõpsake vahekaarti** .


## <a name="step-6-configure-storage-mapping"></a>Samm 6: Konfigureerige salvestusruumi vastendamine
Vaikimisi kui teil korrata target Hyper-V hosti server, virtuaalse masina allikas Hyper-V hosti server kopeeritud andmed on salvestatud target Hyper-V Server Hyper-V halduri ettevõttevälise vaikeasukoha. Kopeeritud andmete talletuskoht rohkem kontrolli, saate konfigureerida salvestusruumi vastendused järgmiselt:



1. Määratleda salvestusruumi liigitusi nii lähte- ja sihtsaitide VMM serverites. [Lisateavet leiate teemast](https://technet.microsoft.com/library/gg610685.aspx). Liigitusi peab olema Hyper-V hosti serverid lähte- ja pilved saadaval. Liigitusi ei pea olema sama tüüpi salvestusruumi. Näiteks saate vastendada allika liigitamine, mis sisaldab SMB aktsiate target liigitamine, mis sisaldab CSV-de kaitse.
2. Pärast liigitusi saate luua vastendused. Toiming lehel **Quick Start** > **kaart salvestusruumi**.
3. Klõpsake vahekaarti **salvestusruum** > **kaart salvestusruumi liigitusi**.
4. Menüü **vastendamine salvestusruumi liigitusi** valige liigitusi allikas ja suunata VMM serverites. Sätete salvestamine.

    ![Valige target võrgus](./media/site-recovery-vmm-to-vmm-classic/storage-mapping.png)


## <a name="step-7-enable-virtual-machine-protection"></a>Juhis 7: Luba virtuaalse masina kaitse
Pärast serverid pilved ja võrgu on õigesti konfigureeritud, saate lubada kaitse virtuaalmasinates pilveteenuses.

1. **Virtuaalmasinates** , kus asub virtuaalse masina pilves, klõpsake menüü **lubamiseks** > **Lisa virtuaalmasinates**.
2. Loendist virtuaalmasinates pilves, valige see, mida soovite kaitsta.

    ![Luba virtuaalse masina kaitse](./media/site-recovery-vmm-to-vmm-classic/enable-protection.png)

3. **Klõpsake vahekaarti, sh algse dispersioonanalüüs** lubamine kaitse toimingu jälgimiseks. Pärast viimistlemine kaitse töö töötab virtuaalse masina on valmis Tõrkesiirde. Pärast kaitse on lubatud ja virtuaalmasinates on kopeeritud, saate küll Azure neid vaadata.

    ![Virtuaalse masina kaitse töö](./media/site-recovery-vmm-to-vmm-classic/vm-jobs.png)

>[AZURE.NOTE] Samuti saate lubada kaitse virtuaalmasinates VMM konsooli. Klõpsake tööriistaribal virtuaalse masina atribuudid vahekaardil **Azure saidi taastamise** nuppu **Kaitse lubamine** .

### <a name="on-board-existing-virtual-machines"></a>Sisseehitatud olemasoleva virtuaalmasinates

Kui teil on olemasolevaid VMM virtuaalmasinates, mis on imitatsiooniga koos Hyper-V koopia peate pardal need Azure saidi taastamise kaitse järgmiselt:

1. Veenduge, et teil on esmaseid ja teiseseid pilved. Veenduge, et Hyper-V server hosting olemasoleva virtuaalse masina asub esmane pilves ja, et hostiteenuse koopia virtuaalse masina Hyper-V server asub teisel pilveteenuses. Veenduge, et olete konfigureerinud pilved kaitse sätted. Sätted peaksid kattuma praegu konfigureeritud Hyper-V koopia. Muul juhul ei pruugi virtuaalse masina dispersioonanalüüs ootuspäraselt.
2. Seejärel lubada esmane virtuaalse masina kaitse. Azure'i saidi taastamine ja VMM tagab, et sama koopia host ja virtuaalse masina tuvastatakse ja Azure saidi taastamise taaskasutamine ja taastamiseks dispersioonanalüüs cloud konfigureerimisel konfigureeritud sätete abil.


## <a name="test-your-deployment"></a>Testige oma juurutamine

Testimiseks käivitage test Tõrkesiirde jaoks ühe virtuaalse masina, või luua taastamise kava koosneb mitmest virtuaalmasinates ja käivitage test Tõrkesiirde lepingu raames juurutamise.  Test Tõrkesiirde jäljendab oma Tõrkesiirde ja taastamise süsteem isoleeritakse võrgus.

### <a name="create-a-recovery-plan"></a>Taastamis loomine

1. Klõpsake menüü **Taastamine lepingute** **Loomine taastamine kavandamine**.
2. Määrake nimi taastamise kava ja lähte- ja VMM serverites. Andmeallika server peab olema virtuaalmasinates Tõrkesiirde ja taastamise jaoks lubatud. Valige **Hyper-V** ainult pilved, mis on konfigureeritud lubama Hyper-V kopeerimine kuvamiseks.

    ![Looge taastamise kava](./media/site-recovery-vmm-to-vmm-classic/recovery-plan1.png)

3. **Valige virtuaalse masina**, valige dispersioonanalüüs rühmad. Valitakse kõik virtuaalmasinates dispersioonanalüüs rühmaga seostatud ja lisatakse taastamise kava. Nende virtuaalmasinates lisatakse taastamise leping vaikerühma – rühma 1. vajadusel saate lisada veel rühmi. Pange tähele, et pärast dispersioonanalüüs virtuaalmasinates käivitub vastavalt taastamise leping rühmad.

    ![Lisage virtuaalmasinates](./media/site-recovery-vmm-to-vmm-classic/recovery-plan2.png)

Pärast lepingu on loodud, kuvatakse see menüü **Taastamine lepingute** loendis.

###<a name="run-a-test-failover"></a>Käivitage test Tõrkesiirde

1. Menüü **Taastamine lepingute** valige leping ja klõpsake nuppu **Testi Tõrkesiirde**.
2. Valige lehel **Kinnitage Test Tõrkesiirde** **pole**. Pange tähele, et see suvand on lubatud nurjunud üle koopia virtuaalmasinates ei ühendatud võrgule. See testib virtuaalse masina ei üle oodatud viisil, kuid testimiseks teie dispersioonanalüüs keskkonnast. [Käivitage test Tõrkesiirde](site-recovery-failover.md#run-a-test-failover) eri võimalused kasutamise kohta lisateabe saamiseks vaadake.
3. Testi virtuaalse masina luuakse sama nimega host, millel on olemas koopia virtuaalse masina Host. See on lisatud, kus asub koopia virtuaalse masina sama pilveteenusesse.

### <a name="run-a-recovery-plan"></a>Käivitage taastamis
Pärast dispersioonanalüüs ei pruugi koopia virtuaalse masina IP-aadress, mis on sama mis esmane virtuaalse masina IP-aadress. Virtuaalmasinates update DNS-i server, mida nad kasutavad pärast Alustuseks. Saate lisada ka skripti värskendada DNS-i Server tagada õigeaegne värskendus.

#### <a name="script-to-retrieve-the-ip-address"></a>Skripti toomiseks IP-aadress
Käivitage see valimi skript toomiseks IP-aadress.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="script-to-update-dns"></a>Skripti värskendada DNS-i
Käivitage see valimi skripti värskendada DNS-i, täpsustades proovisite peidust välja tuua, kasutades eelmises valimi skripti IP-aadress.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="privacy-information-for-site-recovery"></a>Privaatsusteave saidi taastamine

Sellest jaotisest leiate täiendavaid privaatsusteave teenuse Microsoft Azure saidi taastamine ("teenus"). Microsoft Azure'i teenuste privaatsusavalduse vaatamiseks lugege teemat [Microsoft Azure'i privaatsusavaldus](http://go.microsoft.com/fwlink/?LinkId=324899)

**Funktsioon: registreerimine**

- **Mida see tähendab**: registreerib serveri teenuse nii, et kaitstud virtuaalmasinates
- **Kogutud teave**: pärast registreerimisel teenuse kogub, töötleb ja edastab halduse serdi teabe abil teenuse VMM serveri nimi ja pilved virtuaalse masina nimi VMM serverisse avariitaastet osutama on määratud VMM serverist.
- **Teabe kasutus**:
    - Serdi haldus – seda kasutatakse tuvastamiseks ja autentimiseks registreeritud VMM server teenusele juurdepääsu. Teenuse secure luba, et ainult registreeritud VMM server võivad juurde pääseda kasutab serdi avalik võti osa. Server tuleb kasutada seda luba juurde pääseda teenuse funktsioonid.
    - VMM serveri nimi – VMM serveri nimi on vajalik tuvastada ja pilved asuvad VMM sisselogimisserverisse suhelda.
    - Cloud VMM server nimed – pilveteenuses nimi on vaja teenuse pilveteenuses allpool kirjeldatud sidumine/Bluetooth funktsiooni kasutamisel. Kui otsustate sidumiseks mõne muu pilveteenuste andmekeskuse taastamine lähteandmeid keskuse kaudu oma pilve, kõik pilved taastamine andmekeskuse nimed on esitatud.

- **Valik**: see teave on oluline osa teenuse registreerimise käigus, kuna see aitab teil ja teenuse VMM server soovite pakkuda Azure saidi taastamise, ka selle kohta, et tuvastada õige registreeritud VMM server tuvastamiseks. Kui te ei soovi seda teavet saada teenuse, kasutada seda teenust. Kui registreerimine oma serveri ja seejärel hiljem soovite unregister seda, saate seda teha kustutate VMM Serveriteave teenuse portaali (mis on Azure portaali).

**Funktsioon: Lubamiseks Azure saidi taastamine**

- **Mida see tähendab**: Azure saidi taastamise pakkuja VMM-i serverisse installitud on üksuse teenuse suhtlemiseks. Pakkuja on dünaamiliselt Lingitav teek (DLL) majutatud VMM käigus. Pärast installimist pakkuja saab "Andmekeskuse taastamise" funktsioon lubatud VMM administreerimiskonsool. Mis tahes uude või olemasolevasse virtuaalmasinates pilve saate lubada atribuudi "Andmekeskuse taastamise" virtuaalse masina kaitsta. Kui see atribuut on määratud, saadab pakkuja nimi ja ID virtuaalse masina teenuse. Virtuaalne kaitse on lubatud Windows Server 2012 või Windows Server 2012 R2 Hyper-V kopeerimine tehnoloogiaga. Virtuaalse masina andmed saab kopeeritud ühe Hyper-V host teise (tavaliselt asub andmekeskuse erinevate "taastamise").

- **Kogutud teave**: Service kogub, töötleb ja edastab metaandmete virtuaalse masina, mis sisaldab nimi, ID, virtuaalse võrgu ja pilvega nime kuhu ta kuulub.

- **Teabe kasutus**: Service kasutab ülaltoodud teavet asustamiseks virtuaalse masina teave teie teenuse portaalis.

- **Valik**: see on oluline osa teenuse ja ei saa välja lülitada. Kui te ei soovi seda teavet teenuse saadetud, ei luba mis tahes virtuaalmasinates Azure saidi taastamise kaitse. Pange tähele, et kõik andmed, mis on saadetud teenuse pakkuja saadetakse https.

**Funktsioon: Taastamise kava**

- **Mida see tähendab**: selle funktsiooni abil saate koostada korraldamise kavandamine "taastamise" data Centeri kaudu. Saate määratleda, kus tuleb soovitud virtuaalmasinates või rühma virtuaalmasinates alustada taastamine saidi järjestuses. Saate määrata, mis tahes automatiseeritud skriptide olema käitamine või mõnda käsitsi võtma, iga virtuaalse masina taastamise ajal. (Järgmises jaotises kirjeldatakse) Tõrkesiirde käivitatakse tavaliselt koordineeritud taastamise taastamise kava tasemel.

- **Kogutud teave**: Service kogub, töötleb ja edastab metaandmete taastamine lepingule, sh virtuaalse masina metaandmete ja mis tahes automatiseerimise skripte ja märkmete käsitsi toimingu metaandmete.

- **Teabe kasutus**: eespool kirjeldatud metaandmeid kasutatakse koostamiseks taastamise kava oma teenuse portaalis.

- **Valik**: see on oluline osa teenuse ja ei saa välja lülitada. Kui te ei soovi seda teavet teenuse saadetud, pole koostada taastamine lepingud seda teenust.

**Funktsioon: Võrgu kaardistamine**

- **Mida see tähendab**: See funktsioon võimaldab teil võrgu teabe esmane andmekeskuse andmekeskuse taastamine. Kui soovitud virtuaalmasinates on taastatud taastamine saidi, aitab selle vastenduse loomine võrguühenduse neid.

- **Kogutud teave**: osana võrgu vastendamise funktsiooni teenuse kogub, töötleb ja edastab metaandmeid, loogilised võrgud iga saidi (primaarne ja andmekeskusse).

- **Teabe kasutus**: Service kasutab metaandmete asustamiseks oma teenuse portaali kui vastendate võrgu teave.

- **Valik**: see on oluline osa teenuse ja ei saa välja lülitada. Kui te ei soovi seda teavet teenuse saata, ärge kasutage funktsiooni võrgu vastenduse.

**Funktsioon: Tõrkesiirde - plaanitud, ootamatute, test**

- **Mida see tähendab**: See funktsioon aitab virtuaalse masina Tõrkesiirde ühe VMM hallatavate andmekeskuse teise VMM hallatavate andmekeskuse. Oma teenuse portaalis kasutaja käivitab Tõrkesiirde toiming. Võimalikud põhjused on Tõrkesiirde planeerimata sündmuse (nt puhul on loomulikus disaster0; kavandatud sündmus (nt Andmekeskuse koormus tasakaalustamiseks); test Tõrkesiirde (nt taastamise leping harjutamine).

Pakkuja serveris VMM saab teada sündmuse teenuse ja käivitab Tõrkesiirde toimingu Hyper-V hosti VMM liideste kaudu. Windows Server 2012 või Windows Server 2012 R2 Hyper-V kopeerimine tehnoloogia teostab tegelik Tõrkesiirde virtuaalse masina ühe Hyper-V Host teise (tavaliselt töötab andmekeskuse erinevate "taastamise"). Pärast selle Tõrkesiirde on lõpule jõudnud, saadab pakkuja "taastamise" andmekeskuse VMM serverisse installitud teenuse edu teave.

- **Kogutud teave**: Service kasutab ülaltoodud teavet asustamiseks Tõrkesiirde toimingu teabe portaalis oma teenuse olekut.

- **Teabe kasutus**: Service kasutab ülaltoodud teave järgmiselt:

    - Serdi haldus – seda kasutatakse tuvastamiseks ja autentimiseks registreeritud VMM server teenusele juurdepääsu. Teenuse secure luba, et ainult registreeritud VMM server võivad juurde pääseda kasutab serdi avalik võti osa. Server tuleb kasutada seda luba juurde pääseda teenuse funktsioonid.
    - VMM serveri nimi – VMM serveri nimi on vajalik tuvastada ja pilved asuvad VMM sisselogimisserverisse suhelda.
    - Cloud VMM server nimed – pilveteenuses nimi on vaja teenuse pilveteenuses allpool kirjeldatud sidumine/Bluetooth funktsiooni kasutamisel. Kui otsustate sidumiseks mõne muu pilveteenuste andmekeskuse taastamine lähteandmeid keskuse kaudu oma pilve, kõik pilved taastamine andmekeskuse nimed on esitatud.

- **Valik**: see on oluline osa teenuse ja ei saa välja lülitada. Kui te ei soovi seda teavet teenuse saata, ärge kasutage seda teenust.

## <a name="next-steps"></a>Järgmised sammud

Pärast käivitamist test Tõrkesiirde kontrollida keskkonna töötab ootuspäraselt [teave](site-recovery-failover.md) eri tüüpi failovers.
