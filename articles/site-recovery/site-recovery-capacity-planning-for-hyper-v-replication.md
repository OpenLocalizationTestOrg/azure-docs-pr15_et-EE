<properties
    pageTitle="Käitada Hyper-V võimsus Plaanur saidi taastamise jaoks | Microsoft Azure'i"
    description="Käesolevast artiklist leiate Azure'i saidi taastamine Hyper-V võimsus Plaanur tööriista kasutamise juhised"
    services="site-recovery"
    documentationCenter="na"
    authors="rayne-wiselman"
    manager="jwhit"
    editor="" />
<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/12/2016"
    ms.author="raynew" />

# <a name="run-the-hyper-v-capacity-planner-tool-for-site-recovery"></a>Saidi taastamine Hyper-V võimsus Plaanur tööriista käivitamine

Azure'i saidi taastamine juurutamise käigus peate oma kopeerimise ja läbilaskevõime nõuete välja selgitada. Saidi taastamine Hyper-V võimsus Plaanur tööriista abil saate kopeerimise ja läbilaskevõime nõuded Hyper-V virtuaalse masina kopeerimine välja selgitada.


Selles artiklis kirjeldatakse, kuidas Hyper-V võimsus Plaanur tööriista. See tööriist tuleks kasutada koos muude võimsus kavandamise tööriistad ja kirjeldatud [võimsus kavandamise saidi taastamine](site-recovery-capacity-planner.md).


## <a name="before-you-start"></a>Enne alustamist

Käivitate tööriista Hyper-V server või kobar sõlme esmane saidil. Tööriista peab Hyper-V hosti serverid.

- Opsüsteem: Windows Server® 2012 või Windows Server® 2012 R2
- Mälu: 20 MB (miinimum)
- Protsessor: 5 protsenti pea kohal (miinimum)
- Kettaruumi: 5 MB (miinimum)

Enne tööriista käivitamist peate esmane saidi ettevalmistamine. Kui te olete imitatsiooniga vahel kaks asutusesisese saitide ja soovite kontrollida läbilaskevõime, peate ka koopia serverile ettevalmistamine.


## <a name="step-1-prepare-the-primary-site"></a>Samm 1: Ettevalmistamine esmane saidi
1. Esmane saidil kõik Hyper-V virtuaalmasinates, mida soovite korrata ja Hyper-V hosts/rühmad need on asub loendi koostamine. Tööriista saab käitada iga kord, kui mitu autonoomse pakkujate või ühe kobar, kuid mitte mõlemat koos. Samuti peab käivitada eraldi iga operatsioonisüsteemi, seega peaksite kogumine ja pange tähele oma Hyper-V serverite järgmiselt:

  - Windows Server® 2012 eraldi serverid
  - Windows Server® 2012 kogumite
  - Windows Server® 2012 R2 eraldi serverid
  - Windows Server® 2012 R2 kogumite

3. Hyper-V hosts ja kogumite WMI remote juurdepääsu lubamine. Käivitage see käsk iga serveri/kobar veendumaks, et tulemüüri reeglid ja kasutajaõiguste määratakse.

        netsh firewall set service RemoteAdmin enable

5. Luba jõudluse jälgimine serverites ja kogumite, järgmiselt:

  - Avage Windowsi tulemüür **Täiustatud turvalisusega** lisandmoodul ja seejärel lubada sissetulevad reeglid: **COM + võrgupääs (DCOM-IN)** ja **sündmuste logi kaughalduse rühma**kõik reeglid.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Samm 2: Ettevalmistamine koopia serveri (asutusesisene kohapealse kopeerimise abil)

Te ei pea seda teha, kui te olete imitatsiooniga Azure'i.

Soovitame häälestamist ühe Hyper-V server server taastamine nii, et fiktiivsete VM saab paljundada läbilaskevõime kontrollimiseks.  Võite selle lõigu, kuid te ei saa mõõta läbilaskevõime, v.a juhul, kui te seda.

1. Kui soovite kasutada kobar sõlm nagu koopia konfigureerimine Hyper-V koopia ta:

    - Avage **Server Manager** **Tõrkesiirdeklastri halduri**.
    - Ühenduse klaster, esile tõsta kobar nimi ja klõpsake nuppu **toimingud** > **Konfigureerimine rolli** kõrge-saadavus viisardi avamiseks.
    - **Valige roll** nuppu **Hyper-V koopia ta**. Sisestage viisardis **NetBIOS nimi** ja **IP-aadressi** kasutada punkti klaster (ehk kliendi juurdepääsu punkti). **Hyper-V koopia ta** konfigureeritakse tulemuseks kliendi juurdepääsu punkti nimi, mis tuleks teadmiseks.
    - Veenduge, et ta Hyper-V koopia roll võrguühenduse edukalt ja võib nurjuda üle kõik sõlmed klaster vahel. Selleks paremklõpsake roll, valige käsk **Teisalda**ja klõpsake **Valige sõlm**. Valige sõlm > **OK**.
    - Kui kasutate serdi autentimise, veenduge, et iga kobar sõlm ja kõik on installitud serdi pääsupunktile kliendi.
2.  Koopia serveri lubamiseks tehke järgmist.

    - Klaster avage tõrge halduri, klaster ühenduse loomiseks ja käsku **rollid** > valige rolli > **Dispersioonanalüüs säte**s > **Luba see kobar koopia server**. Pange tähele, et kui te kasutate klaster koopia peate olema Hyper-V koopia ta roll Esita klaster esmane saidi samuti.
    - Avage eraldi Server Hyper-V haldur. Paanil **toimingud** nuppu **Hyper-V sätted** server, mida soovite lubada ja **Dispersioonanalüüs konfiguratsioon** nuppu **Luba serveri koopia arvutisse**.
3. Autentimise häälestamiseks tehke järgmist.

    - **Autentimis- ja pordid** valige kuidas autentida esmane server ja pordid autentimist. Kui kasutate klõpsamisel valima ka ühe serdi klõpsake **Serdi valimine** . Kasutage Kerberos, kui esmane ja taastamise Hyper-V, milles on sama domeeni või usaldusväärsed domeenid. Kasutage erinevate domeenide või töörühm juurutamise serdid.
    - **Autoriseerimine ja salvestusruumi** jaotises luba **Kõik** autenditud dispersioonanalüüs andmeid saata selle koopia server (primaarne) server. Klõpsake nuppu **OK** või **Rakenda**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

    - Käivitage **netsh http Kuva servicestate** kontrollida, kas määratud protokoll/Port töötab kuulajale.  
4. Saate häälestada tulemüürid. Hyper-V installimisel luuakse tulemüüri reeglid pordid (HTTPS 443 kohta, klõpsake 80 Kerberos) vaikimisi liikluse lubamiseks. Luba reegleid järgmiselt:

        - Certificate authentication on cluster (443): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}**
        - Kerberos authentication on cluster (80): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}**
        - Certificate authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"**
        - Kerberos authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"**

## <a name="step-3-run-the-capacity-planner-tool"></a>Samm 3: Käitada võimsus Plaanur

Kui olete valmis oma peamine saidi ja luua taastamise server käivitada tööriist.

1. [Laadige alla](https://www.microsoft.com/download/details.aspx?id=39057) tööriist Microsoft Download Center.
2. Käivitage tööriist esmane serverid (või ühe sõlmed esmane kobar kaudu). Paremklõpsake .exe faili ja seejärel valige käsk **Käivita administraatorina**.
3. **Enne alustamist** Määrake kaua soovite koguda andmeid. Soovitame käitada tagamaks, et andmed on esindajaga tootmise tunni jooksul. Kui proovite ainult valideerimiseks võrguühendus, kogute ainult minutiks.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4. **Esmane saidi üksikasjad** Määrake serveri nimi või FQDN autonoomse Host või klaster määrata kliendi FQDN aktsepteerida punkti, kobar nimi või klaster sõlm ja siis klõpsake nuppu **edasi**. Tööriist tuvastab automaatselt see töötab serveri nimi. Tööriista jätkab VMs, mida saate jälgida määratud servereid.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5. **Koopia saidi üksikasjad** kui te olete imitatsiooniga Azure'i või kui olete imitatsiooniga teisene andmekeskuse ja koopia server pole häälestanud, valige **seotud koopia saidi kontrollib vahele jätta**. Kui olete imitatsiooniga teisene andmekeskusesse ja olete häälestanud koopia tüüp FQDN autonoomse serveri või kliendi jaoks kobar **serveri nime (**või) Hyper-V koopia ta ots pääsupunkti.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. **Laiendatud koopia** üksikasjad luba **seotud laiendatud koopia saidi testide vahele jätta**. Ta ei toeta saidi taastamine.
7. **Juhendist** vms valige tööriistade serveri või kobar ja teile kuvatakse VMs ja esmane serveris ketast, sätteid vastavalt teie määratud **Esmane saidi üksikasjade** lehel. Pange tähele, et VMs, mis on juba lubatud kopeerimine või mis ei kasuta ei kuvata. Valige VMs, mille jaoks soovite koguda mõõdikute. Valige soovitud VHDs automaatselt kogub andmeid VMs jaoks liiga.
9. Kui olete konfigureerinud koopia serverisse või kobar, **võrgu** teave määrata ligikaudse WAN läbilaskevõime, mis teie arvates kasutatakse vahele, põhi- ja koopia ning valige serdid, kui olete konfigureerinud serdi autentimist.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

10. **Kokkuvõte** kontrollige sätteid ja klõpsake nuppu **Järgmine** kogumiseks mõõdikute alustamiseks. Tööriista edenemise ja olek kuvatakse lehel **Arvutamine võimsus** . Tööriista lõpulejõudmisel töötab klõpsake nuppu **Kuva aruanne** minna üle väljund. Vaikimisi salvestatakse aruannete ja logide **%systemdrive%\Users\Public\Documents\Capacity Plaanur**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)


## <a name="step-4-interpret-the-results"></a>Samm 4: Tõlgendamine tulemused
Siin on oluline mõõdikute. Võite ignoreerida mõõdikute, mis ei ole loetletud siin. Nad pole oluline saidi taastamise jaoks.

### <a name="on-premises-to-on-premises-replication"></a>Kohapealse kohapealse kopeerimise abil
  - Esmane host Arvuta mälu kopeerimise mõju
  - Esmase, taastamine hosts salvestusruumi kettaruumi IOPS dispersioonanalüüs mõju
  - Kogu läbilaskevõime nõutav delta kopeerimise (MB)
  - Täheldatud võrgu läbilaskevõime esmane host ja taastamise host (MB)
  - Soovitus aktiivse paralleelselt ülekannetes kaks hosts/rühmad optimaalne arv

### <a name="on-premises-to-azure-replication"></a>Kohapealse Azure kopeerimise abil
  - Esmane host Arvuta mälu kopeerimise mõju
  - Mõju esmane host salvestusruumi kettaruumi IOPS dispersioonanalüüs
  - Kogu läbilaskevõime nõutav delta kopeerimise (MB)

## <a name="more-resources"></a>Veel ressursse

- Tööriista kohta lisateabe saamiseks lugege dokumenti, mis kaasneb tööriista allalaadimine.
- Vaadake lühiülevaade tööriista Keith Mayer [TechNeti ajaveebis](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
- [Tulemuse saamiseks](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) meie jõudluse testimine kohapealse abil kohapealse Hyper-V kopeerimine



## <a name="next-steps"></a>Järgmised sammud

Kui olete lõpetanud, võimsuse planeerimine saate hakata juurutamine saidi taastamine.

- [Azure Hyper-V VMs VMM pilved paljundada](site-recovery-vmm-to-azure.md)
- [Paljundada Azure Hyper-V VMs (ilma VMM)](site-recovery-hyper-v-site-to-azure.md)
- [Ise Hyper-V VMs VMM saitide vahel](site-recovery-vmm-to-vmm.md)
- [Ise Hyper-V VMs VMM saite SAN vahel](site-recovery-vmm-san.md)
- [Ise hyper-V VMs ühe VMM serveris](site-recovery-single-vmm.md)