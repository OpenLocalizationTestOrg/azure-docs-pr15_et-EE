<properties
    pageTitle="Paljundada VMware virtuaalmasinates ja füüsilise serveri Azure'i Azure saidi taastamine | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse, kuidas juurutada Azure saidi taastamist korraldab dispersioonanalüüs, Tõrkesiirde ja kohapealse VMware virtuaalmasinates ja Windows/Linux Azure'i füüsiliste serverite."
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
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery"></a>Paljundada VMware virtuaalmasinates ja füüsilise serveri Azure'i Azure saidi taastamine

> [AZURE.SELECTOR]
- [Azure'i portaal](site-recovery-vmware-to-azure.md)
- [Klassikaline portaal](site-recovery-vmware-to-azure-classic.md)
- [Klassikaline portaali (pärand)](site-recovery-vmware-to-azure-classic-legacy.md)


Azure'i saidi taastamise teenuse aitab teie ettevõtte järjepidevus ja avariijärgne taastamine (BCDR) strateegia orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates ja füüsilise serveri. Masinad saab paljundada Azure'i või sekundaarne asutusesisese andmekeskuse. Kiire ülevaate saamiseks lugege [mis on Azure saidi taastamise?](site-recovery-overview.md).

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse, kuidas:

- **Azure'i korrata VMware virtuaalmasinates**– juurutamine saidi taastamine koordineerimiseks dispersioonanalüüs, Tõrkesiirde ja kohapealse VMware virtuaalmasinates tagastamine Azure salvestusruumi.
- **Ise füüsilise serveri Azure**– juurutada Azure saidi taastamise koordineerimiseks dispersioonanalüüs, Tõrkesiirde ja kohapealse füüsilise Windows ja Linux serverite Azure taastamine.

>[AZURE.NOTE] Selles artiklis kirjeldatakse, kuidas Azure'i korrata. Kui soovite paljundada VMware VMs või Windows/Linux füüsilise serveri teisene andmekeskusesse, järgige [selles artiklis](site-recovery-vmware-to-vmware.md)antud juhiseid.

Postitada kommentaare või küsimusi allosas käesoleva artikli või [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="enhanced-deployment"></a>Täiustatud juurutamine

See artikkel sisaldab sisaldab juhiseid täiendatud juurutamiseks klassikaline Azure'i portaalis. Soovitame kasutada kõigi uute juurutuste selle versiooni. Kui olete juba juurutanud, kasutades pärand varasema versiooni soovitame migreerimine uus versioon. Lugege [Lisateavet](site-recovery-vmware-to-azure-classic-legacy.md##migrate-to-the-enhanced-deployment) migreerimise kohta.

Täiustatud juurutamise on suur update. Siit leiate kokkuvõtte lisasime täiustused:

- **Taristu VMs Azure**: andmete tiražeerib otse Azure storage konto. Lisaks kopeerimise ja Tõrkesiirde ei ole vaja luua mis tahes taristu VMs (konfiguratsiooniserveri, serveri) vastavalt vajadusele me pärand juurutuse.  
- **Ühendatud installi**: üks install pakub lihtne setup ja kohapealse komponentide skaleeritavus.
- **Turvaline juurutamise**: kogu liiklus on krüptitud ja HTTPS 443 saadetakse dispersioonanalüüs halduse suhtlus.
- **Taastamise punkte**: Windows ja Linux keskkonnas võrra tugi krahh ja rakenduse ühtsete taastamine ja toetab nii ühe VM ja mitme-VM ühtsete konfiguratsioone.
- **Testige Tõrkesiirde**: tugi-häiriva test Tõrkesiirde Azure ilma mõjutavad tootmise või pausi kopeerimine.
- **Planeerimata Tõrkesiirde**: planeerimata Tõrkesiirde Azure sulgeda VMs automaatselt Tõrkesiirde enne täiustatud suvandiga tugi.
- **Failback**: integreeritud failback, mis tiražeerib ainult delta muudatused tagasi kohapealse saidi.
- **vSphere 6.0**: piiratud tugi VMware Vsphere 6.0 juurutuste.


## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Kuidas aitab saidi taastamise kaitsta virtuaalmasinates ja füüsilise serveri?


- VMware administraatorid saavad konfigureerida väljapoole kaitse Azure business töökoormus ja VMware virtuaalmasinates töötavad rakendused. Serveri haldurid saab paljundada füüsilise kohapealsed Windows ja Linux serverid Azure.
- Azure'i saidi taastamine konsooli pakub lihtsa häälestamise ja haldamise dispersioonanalüüs, Tõrkesiirde ja taastamise protsesside ühes kohas.
- Kui te ise VMware virtuaalmasinates haldab vCenter server, saidi taastamine võite avastada need automaatselt. Kui masinad on ESXi hosti leiab saidi taastamine VMs Host.
- Käivitage oma kohapealse taristu lihtne failovers Azure ja migreerite (Taasta) Azure'i Vmware'i VM serverid kohapealse saidi.
- Konfigureerige taastamine lepingud, millele rühmitada rakenduse töökoormus, mis on mitmetasandilise mitme arvutites. Teil võib nurjuda üle nende lepingute ja saidi taastamine annab mitme-VM järjepidevuse nii, et sama töökoormus, kus saate taastada koos ühtsete andmepunkti.


## <a name="supported-operating-systems"></a>Toetatud opsüsteemid

### <a name="windows64-bit-only"></a>Windowsi (ainult 64-bitine)
- Windows Server 2008 R2 SP1 +
- Windows Server 2012
- Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (ainult 64-bitine)
- Punane rolli Enterprise Linux 6,7, 7.1, 7.2
- CentOS 6.5, 6.6 6,7, 7.0, 7.1, 7.2
- Oracle'i Enterprise Linux 6.4 6.5 operatsioonisüsteemiga punane rolli ühilduvad tuum või purunematu Enterprise tuum väljaanne 3 (UEK3)
- SUSE Linux Enterprise Server 11 SP3

## <a name="scenario-architecture"></a>Stsenaarium arhitektuur

Stsenaarium komponendid:

- **Kohapealse management server**: management server töötab saidi taastamine komponendid:
    - **Konfiguratsiooniserveri**: koordinaadid side ja haldab andmete kopeerimise ja taastamise abil.
    - **Protsessi server**: dispersioonanalüüs lüüsi toimib. Kaitstud allika masinad saab, optimeerib vahemällu, tihendamise ja krüptimise ja saadab dispersioonanalüüs andmete Azure salvestusruumi. Lisaks tegeleb tõuketeatised installi Mobility teenuse kaitstud masinad ja sooritab VMware VMs automaatne tuvastamine.
    - **Juhtlehed target server**: tegeleb dispersioonanalüüs andmete migreerite Azure'i ajal.
    Saate juurutada management server, mis toimib protsess server ainult, et mastaapimiseks juurutamise.
- **The mobiilsus teenuse**: see osa on juurutatud igas arvutis (Vmware'i VM või füüsiline server), mida soovite korrata Azure. See sisaldab andmeid kirjutab arvutis ja edastab need protsessi serveriga.
- **Azure'i**: teil pole vaja luua mis tahes Azure VMs kopeerimise ja Tõrkesiirde. Saidi taastamise teenuse tegeleb andmehaldus ja andmete tiražeerib otse Azure salvestusruumi. Tiražeeritud Azure VMs on kedratud üles automaatselt ainult siis, kui Tõrkesiirde Azure. Juhul, kui soovite kohapealse saidi tagasi: Azure'i nurjuda peate tegutseda protsessi server Azure'i VM-i häälestamine.


Pilti näitab, kuidas kasutada nende komponendid.

![arhitektuur](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Joonisel 1: Azure'i VMware/füüsilise** (loodud Henry Robalino)


## <a name="capacity-planning"></a>Võimsuse planeerimine

Kui plaanite võimsus, leiate siit peaksite mõtlema, milliseid:

- **Andmeallika keskkonnas**– võimsus kavandamise või VMware taristu ja allika arvuti nõuetele.
- **Management serveri**– kavandamise kohapealse halduse serverid, et käivitada saidi taastamine komponendid.
- **Võrgu läbilaskevõime allikast sihtkohta**-võrgu läbilaskevõime nõutav kopeerimise vahel lähte- ja Azure kavandamine

### <a name="source-environment-considerations"></a>Andmeallika keskkonna kaalutlused

- **Maksimum iga päev muuta**– kaitstud masina saate kasutada ainult üks protsess server ja ühe protsessi serveri saavad hakkama kuni 2 TB päevas andmete muutmine. Seega on 2 TB maksimaalne igapäevane andmeid muuta määr, mis on toetatud kaitstud kohapeal.
- **Maksimaalse läbilaskevõime**– tiražeeritud masina saate kuuluvad ühe salvestusruumi konto Azure. Kindlad konto võite hakkama kuni 20 000 taotlusi sekundis ja soovitame, et hoiate IOPS arvu allika arvutisse üle 20 000. Jaoks näide kui teil on allikas arvutisse ja 5 ketast iga ketta genereeritud 120 IOPS (8K suurus) allikas siis võib Azure'is ketta IOPS limiit 500 kohta. Arvu salvestusruumi kontode nõutav = kokku allika IOPs/20 000.


### <a name="management-server-considerations"></a>Management serveri kaalutlused

Management server töötab saidi taastamine komponendid toime andmete optimeerimine, kopeerimise ja haldus. Seda peaks oskama toime igapäevane muutmine määr võimsus üle kõik töökoormused kaitstud arvutites töötab ja on piisavalt ribalaiust pidevalt korrata andmete Azure salvestusruumi. Täpsemalt:

- Protsessi server saab dispersioonanalüüs kaitstud masinad ja optimeerib vahemällu, tihendamise ja krüptimise enne saatmist Azure. Management server peaks olema piisavalt ressursse, et teha järgmisi toiminguid.
- Protsessi server kasutab vastavalt vahemälu. Soovitame eraldi vahemälu kettale 600 GB või rohkem salvestatud võrgu kitsaskoht või katkestuste korral andmemuudatuste käsitleda. Juurutamise käigus saate konfigureerida vahemälu kettadraivi, millel on vähemalt 5 GB salvestusruumi, kuid 600 GB on minimaalne soovitus.
- Hea tava soovitame, et management server asuma samas võrgus ja LAN lõigu on masinad, mida soovite kaitsta. See võib asuda erinevas võrgus, kuid masinad, mida soovite kaitsta peaks olema L3 võrgu nähtavuse selle.

Järgmises tabelis on kokku võetud management serveri suurus soovitusi.

**Management serveri CPU** | **Mälu** | **Vahemälu ketta suurus** | **Andmete muutmine määr** | **Kaitstud masinad**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 tuuma @ 2,5 GHz) | 16 GB | 300 GB | 500 GB või vähem | Juurutamine management serveri nende sätetega vähem kui 100 masinad korrata.
12 vCPUs (2 sockets * 6 tuuma @ 2,5 GHz) | 18 GB | 600 GB | 500 GB kuni 1 TB | Juurutamine management serveri nende sätetega 100-150 masinad vahel.
16 vCPUs (2 sockets * 8 tuuma @ 2,5 GHz) | 32 GB | 1 TB | 1 TB kuni 2 TB | Juurutamine management serveri nende sätetega 150-200 masinad vahel.
Mõne muu protsess serveri juurutamine | | | > 2 TB | Täiendavad protsessi serverid juurutada, kui te olete imitatsiooniga päevas üle 200 masinad või kui igapäevane andmeid muuta ületab 2 TB.

Kui:

- Iga andmeallika arvuti on konfigureeritud 3 ketast 100 GB.
- Kasutasime võrdleva talletamist 8 SAS draivid 10 k pööret MINUTIS RAID 10 vahemälu kettale mõõtmed.

### <a name="network-bandwidth-from-source-to-target"></a>Võrgu läbilaskevõime allikast sihtkohta
Veenduge, et soovite arvutada läbilaskevõime, mida oleks vaja algse kopeerimise ja delta dispersioonanalüüs [võimsus Plaanur tööriista](site-recovery-capacity-planner.md) abil

#### <a name="throttling-bandwidth-used-for-replication"></a>Pidurdamise läbilaskevõime kasutatakse dispersioonanalüüs

Kopeeritud Azure'i VMware liikluse läbib konkreetse protsessi server. Saate throttle läbilaskevõime, mis on saadaval saidi taastamine dispersioonanalüüs selles serveris järgmiselt:

1. Avage Microsoft Azure'i varundus MMC lisandmooduli peamised dokumendihalduse serveris või dokumendihalduse serveris täiendavad ettevalmistatud protsessi serverites töötab. Poolt vaikimisi otsetee Microsoft Azure varukoopia loomisel töölaual või leiate see: C:\Program Files\Microsoft Azure taastamise teenuste Agent\bin\wabadmin.
2. Lisandmooduli nuppu **Muuda atribuute**.

    ![Ahendamise läbilaskevõime](./media/site-recovery-vmware-to-azure-classic/throttle1.png)

3. Määrake vahekaardil **Throttling** läbilaskevõime, mida saab kasutada saidi taastamine kopeerimise ja rakendatav plaanimine.

    ![Ahendamise läbilaskevõime](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

Soovi korral saate määrata pidurdamise PowerShelli kaudu. Siin on näide:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>Läbilaskevõime kasutuse maksimeerimine
Suurendamiseks kasutatud kopeerimise Azure saidi taastamine, mida oleks vaja muuta registrivõtme ribalaiust.

Järgmine võti juhtelementide kohta imitatsiooniga ketta teemad, kui imitatsiooniga kasutatud arv

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 "Overprovisioned" võrgus selle registrivõtme on vaja muuta vaikimisi väärtuste põhjal. Me ei toeta kuni 32.  


[Lisateavet leiate teemast](site-recovery-capacity-planner.md) üksikasjalik võimsus kavandamise kohta.

### <a name="additional-process-servers"></a>Täiendavad protsessi servers

Kui vajate rohkem kui 200 masinad kaitsmiseks või igapäevane muutmine määr on suurem saate lisada selle 2 TB täiendavad serveritega käsitlema laadi. Mastaapimiseks te teha järgmist.

- Suurendage management serverid. Näiteks saate kaitsta kuni 400 masinad halduse serveritega.
- Täiendavad protsessi serverid lisamine ja nende abil saate teha liikluse asemel (või lisaks) management server.

Selles tabelis kirjeldatakse stsenaarium, kus.

- Saate häälestada algse management serveri konfiguratsiooniserveri ainult seda kasutada.
- Täiendavad protsessi serveri seadistamine.
- Saate konfigureerida kaitstud virtuaalmasinates kasutada täiendavaid protsessi server.
- Iga kaitstud allika arvuti on konfigureeritud kolm ketast 100 GB.

**Algne management server**<br/><br/>(configuration server) | **Täiendavad protsessi server**| **Vahemälu ketta suurus** | **Andmete muutmine määr** | **Kaitstud masinad**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 tuuma @ 2,5 GHz), 16 GB mälu | 4 vCPUs (2 sockets * 2 tuuma @ 2,5 GHz), 8 GB mälu | 300 GB | 250 GB või vähem | Saate kopeerida 85 või.
8 vCPUs (2 sockets * 4 tuuma @ 2,5 GHz), 16 GB mälu | 8 vCPUs (2 sockets * 4 tuuma @ 2,5 GHz), 12 GB mälu | 600 GB | 250 GB kuni 1 TB | Te saate kopeerida 85-150 masinad vahel.
12 vCPUs (2 sockets * 6 tuuma @ 2,5 GHz), 18 GB mälu | 12 vCPUs (2 sockets * 6 tuuma @ 2,5 GHz) 24 GB mälu | 1 TB | 1 TB kuni 2 TB | Te saate kopeerida 150-225 masinad vahel.


Nii, nagu teie serverid skaalal on sõltuvad teie eelistust skaala üles või mastaapimiseks välja mudel.  Kasutades mõne klassi haldus ja protsessi serverite skaalal või mastaapimiseks välja alusel rohkem serverite vähem ressursse rakendades. Näide: kui teil on vaja kaitsta 220 masinad võivad teha üht järgmistest:

- Algse management serveri 12vCPU, 18 GB mälu, täiendavad protsessi serverisse 12vCPU 24 GB mälu, konfigureerimine ja konfigureerige kaitstud ainult server täiendav protsessi kasutama masinad.
- Või teise võimalusena võite kaks management serverid (2 x 8vCPU, 16 GB RAM-i) ja kahe täiendavad protsessi server (1 x 8vCPU) ja 4vCPU x 1 135 + 85 (220) masinad käsitlema konfigureerimine ning konfigureerimine kaitstud täiendavad protsessi serverid ainult kasutama masinad.


[Järgige neid juhiseid](#deploy-additional-process-servers) täiendavate protsessi serveri seadistamine.

## <a name="before-you-start-deployment"></a>Enne alustamist juurutamine

Tabelite summarize eeltingimused juurutamine seda stsenaariumi.


### <a name="azure-prerequisites"></a>Azure'i eeltingimused

**Nõutav** | **Üksikasjad**
--- | ---
**Azure'i konto**| Peate [Microsoft Azure'i](https://azure.microsoft.com/) kontosse. Saate alustada [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). [Lisateavet leiate teemast](https://azure.microsoft.com/pricing/details/site-recovery/) saidi taastamine hindade kohta.
**Azure'i salvestusruum** | Peate konto Azure storage kopeeritud andmete talletamiseks. Kopeeritud andmed on talletatud Azure salvestusruumi ja Azure VMs on kedratud üles Tõrkesiirde korral. <br/><br/>Vajate [standard geograafilise liigne salvestusruumi konto](../storage/storage-redundancy.md#geo-redundant-storage). Konto peab olema sama piirkonna saidi taastamise teenuse ja olema sama tellimusega seostatud. Pange tähele selle kopeerimine premium salvestusruumi kontod pole praegu toetatud ja seda ei tohi kasutada.<br/><br/>Me ei toeta salvestusruumi kontod ressursi rühma [uue Azure portaali](../storage/storage-create-storage-account.md) abil loodud liikuda. [Lugege](../storage/storage-introduction.md) Azure'i salvestusruumi.<br/><br/>
**Azure'i võrgu** | Peate Azure virtuaalse võrgu, mis Azure VMs loob ühenduse Tõrkesiirde korral. Azure virtuaalse võrgu peab olema sama piirkonna saidi taastamise hoidla.<br/><br/>Teate, et peate uuesti pärast Tõrkesiirde Azure nurjumise VPN ühendus (või Azure'i ExpressRoute) häälestamine Azure võrgust kohapealse saidile.


### <a name="on-premises-prerequisites"></a>Kohapealse eeltingimused

**Nõutav** | **Üksikasjad**
--- | ---
**Management server** | Peate kohapealse Windows 2012 R2 server, virtuaalse masina või füüsiline server töötab. Kõik komponendid kohapealse saidi taastamine installitakse selle dokumendihalduse serveris<br/><br/> Soovitatav on väga kättesaadav Vmware'i VM serveri juurutamist. Kohapealse saidi migreerite Azure on alati olema VMware vms sõltumata sellest, kas te ei ole üle VMs või füüsilise serveri. Kui te pole konfigureerida Management server Vmware'i VM peate häälestamine on eraldi serveri nimega Vmware'i VM failback liikluse vastuvõtmiseks.<br/><br/>Server ei tohiks domeenikontrolleri.<br/><br/>Server peaks olema staatiline IP-aadress.<br/><br/>Hosti nimi serveri peaks olema 15 märki või vähem.<br/><br/>Operatsioonisüsteemi lokaat tuleks ainult inglise keele.<br/><br/>Management server nõuab Interneti-ühendus.<br/><br/>Teil on vaja juurdepääsu Väljamineva meili server järgmiselt: HTTP 80 ajutist juurdepääsu installimisel saidi taastamine komponendid (alla laadida MySQL-i); Poolelioleva väljaminev juurdepääsu HTTPS 443 dispersioonanalüüs haldamiseks; Poolelioleva väljamineva juurdepääsu HTTPS 9443 dispersioonanalüüs liikluse (saate muuta selle pordi)<br/><br/> Veenduge, et need URL-id juurdepääsetavat management serverist. <br/>- \*. hypervrecoverymanager.windowsazure.com<br/>- \*. accesscontrol.windows.net<br/>- \*. backup.windowsazure.com<br/>- \*. blob.core.windows.net<br/>- \*. store.core.windows.net<br/>-https://www.msftncsi.com/ncsi.txt<br/>- [https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi]( https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>Kui teil on serveri IP-aadress-põhiste tulemüüri reeglid, veenduge, et reegleid andnud Azure'i teatis. Peate lubama [Azure'i andmekeskuse IP-vahemikke](https://www.microsoft.com/download/details.aspx?id=41653) ja pordi HTTPS (443). Peate ka valge nimekiri IP-aadressid oma tellimuse Azure piirkond ja Lääne US. URL-i [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") on allalaadimise MySQL-i.
**VMware vCenter/ESXi host**: | Peate ühe või mitme vMware vSphere ESX/ESXi hypervisors haldamise VMware virtuaalmasinates ESX/ESXi 6.0, 5,5 või 5.1 versioon töötab uusimaid värskendusi.<br/><br/> Soovitame VMware vCenter server hallata oma ESXi hosts juurutamist. See peaks töötama vCenter versioon 6.0 või 5,5 uusimaid värskendusi.<br/><br/>Pange tähele, et saidi taastamine ei toeta uue vCenter vSphere 6.0 funktsioone nagu cross vCenter vMotion, virtuaalne mahud ja salvestusruumi DRS. Saidi taastamise tugi on piiratud funktsioonid, mis olid ka versioonis 5,5 saadaval.
**Kaitstud masinad**: | **AZURE'I**<br/><br/>Masinad, mida soovite kaitsta peavad vastama [Azure eeltingimused](site-recovery-best-practices.md#azure-virtual-machine-requirements) Azure VMs loomise kohta.<br><br/>Kui soovite luua ühenduse Azure vms pärast Tõrkesiirde siis peate Kaugtöölaua ühendused kohaliku tulemüüri lubamine.<br/><br/>Üksikute ketta maht kaitstud masinad ei tohiks üle 1023 GB. VM võib olla kuni 64 ketast (seega kuni 64 TB). Kui teil on suurem kui 1 TB ketast kaaluge andmebaasi tiražeerimine näiteks SQL serveri alati sisse- või Oracle andmete Guard.<br/><br/>Minimaalne 2 GB vaba ruumi komponendi installimiseks kettal installi.<br/><br/>Ühiskasutuses ketta Külastajate kogumite ei toetata. Kui teil on rühmitatud juurutus, kaaluge andmebaasi tiražeerimine näiteks SQL serveri alati sisse- või Oracle andmete Guard.<br/><br/>Ühendatud laiendatav püsivara Interface (UEFI) / laiendatav püsivara Interface(EFI) buutimine ei toetata.<br/><br/>Arvuti nimed peaks olema vahemikus 1 kuni 63 märki (tähti, numbreid ja sidekriipse). Nimi peab tähe või numbri alguse ja lõpu tähe või numbri. Pärast masina on kaitstud, saate muuta Azure nimi.<br/><br/>**VMware VMs**<br/><br>Peate installima VMware vSphere PowerCLI 6.0. management server (konfiguratsiooniserveri).<br/><br/>VMware VMs, mida soovite kaitsta peaks olema installitud ja töötab VMware tööriistad.<br/><br/>Kui allika VM on NIC lubatud teisendatakse see pärast Tõrkesiirde Azure ühe NIC.<br/><br/>Kui kaitstud VMs on iSCSI kettalt klõpsake saidi taastamine teisendab kaitstud VM iSCSI ketas VHD faili kui VM ei üle Azure. Kui Azure VM pääseb iSCSI target seejärel see ühenduse iSCSI target ja põhiosas leiate kaks plaati – VHD ketas, Azure VM ja allikas iSCSI ketas. Sel juhul peate katkestama iSCSI target, mis kuvatakse nurjunud üle Azure VM.<br/><br/>[Lisateavet leiate teemast](#vmware-permissions-for-vcenter-access) saidi taastamine vajalikke VMware kasutajaõiguste kohta.<br/><br/> **WINDOWS serveri masinad (VMware VM või füüsiline server)**<br/><br/>Server töötama toetatud 64-bitine opsüsteem: Windows Server 2012 R2, Windows Server 2012 või Windows Server 2008 R2 hoolduspaketiga veebisaidil vähemalt SP1.<br/><br/>Operatsioonisüsteem peaks olema installitud C:\ ketas ja OS ketas peaks olema Windowsi lihtsa jaoks (OS ei tohiks installida Windowsi dünaamiline ketas).<br/><br/>Windows Server 2008 R2 masinad peate olema .NET Framework 3.5.1 installitud.<br/><br/>Teil on vaja administraatorikonto (peab olema kohaliku administraatori Windowsi arvutisse) tõuketeatised installi mobiilsus teenuse Windows serverites. Kui on esitatud konto-domeeni konto peate keelata Remote kasutaja juurdepääsu reguleerimine kohalikus arvutis. [Lisateavet leiate teemast](#install-the-mobility-service-with-push-installation).<br/><br/>Saidi taastamine toetab VMs RDM ketas.  Ajal failback saidi taastamine taaskasutamine RDM ketas, kui algne allikas VM ja RDM ketas on saadaval. Kui nad pole saadaval, loob saidi taastamise ajal failback iga ketta VMDK uue faili.<br/><br/>**LINUX MASINAD**<br/><br/>Peate toetatud 64-bitine operatsioonisüsteem on: punane rolli Enterprise Linux 6,7; CentOS 6.5, 6.6,6.7; Oracle'i Enterprise Linux 6.4 6.5 operatsioonisüsteemiga punane rolli ühilduvad tuum või purunematu Enterprise tuum väljaanne 3 (UEK3) SUSE Linux Enterprise Server 11 SP3.<br/><br/>muutsite failid kaitstud masinad peaks sisaldama kirjed, mis kohaliku hosti nimi vastendada kõik võrguadapteri seotud IP-aadressid. <br/><br/>Kui soovite ühendada Azure virtuaalse masina, mis töötab Linux pärast Tõrkesiirde Secure Shell kliendi (ssh), tagada teenuse Secure Shell kaitstud arvutis on määratud käivitamine automaatselt süsteemi käivitamine ja, et lubada tulemüüri reeglid on ssh sellega ühendus.<br/><br/>Kaitse saab lubada ainult Linux masinad järgmised salvestusruumi: Hajusfailisüsteemiga (EXT3, ETX4, ReiserFS, XFS); Silma tarkvara-seadme plaanuri (silma)); Helitugevuse halduri: (LVM2). HP CCISS kontrolleril salvestusruumi füüsilise serverite ei toetata. ReiserFS failisüsteemi toetatakse ainult SUSE Linux Enterprise Server 11 SP3.<br/><br/>Saidi taastamine toetab VMs RDM ketas.  Ajal failback Linuxi, pole saidi taastamine RDM ketas korduvalt kasutada. Selle asemel loob uue VMDK faili iga vastav RDM ketta.

Ainult Linux VM - tagama disk.enableUUID=true säte sätestatud konfiguratsiooni parameetrid VMware VM. Kui see rida olemas, lisage see. See on vajalik soovitud VMDK ühtsete valitud kasutajatunnust anda, et alused õigesti. Samuti võtke arvesse, et ilma seda sätet failback võib põhjustada täis laadida ka siis, kui VM on saadaval prem. Selle sätte lisamise veenduge, et ainult delta muudatused viiakse tagasi ajal failback.

## <a name="step-1-create-a-vault"></a>Samm 1: Loo võlvkelder

1. Logige sisse [haldusportaali](https://manage.windowsazure.com/).
2. Laiendage **Data Services** > **Taastamise teenused** ja klõpsake nuppu **Saidi taastamise hoidla**.
3. Klõpsake nuppu **Loo uus** > **kiire loomine**.
4. Sisestage väljale **nimi**sõbralik nimi, mis tähistavad vault.
5. **Piirkond**, valige piirkonnas vault jaoks. Märkige ruut toetatud regioonide leiate [Azure'i saidi taastamise hinnad üksikasjad](https://azure.microsoft.com/pricing/details/site-recovery/) geograafilised saadavus
6. Klõpsake nuppu **Loo vault**.
    ![Uue vault](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Kontrollige, et kinnitada, et vault on loodud. Vault kirjas **aktiivseks** **Taastamise teenused** avalehele.

## <a name="step-2-set-up-an-azure-network"></a>Samm 2: Azure'i võrgu seadmine

Määrake Azure võrgu nii, et Azure VMs pärast Tõrkesiirde võrguga ühendatud ja nii, et failback kohapealse saidi saate töötada ootuspäraselt.

1. Azure'i portaalis > **Loo virtuaalse võrgu** määrata võrgu nimi. IP address vahemik ja alamvõrgu nimi.
2. Peaksite lisamiseks võrgu VPN-/ ExpressRoute, kui teil on vaja teha failback. VPN-/ ExpressRoute saab lisada ka pärast Tõrkesiirde võrku.

[Lisateavet vt](../virtual-network/virtual-networks-overview.md) Azure võrkude kohta.

> [AZURE.NOTE] [Võrgu migreerimine](../resource-group-move-resources.md) üle ühe tellimuse ressursi rühma või tellimustes võrke kasutatakse juurutamine saidi taastamine ei toetata.

## <a name="step-3-install-the-vmware-components"></a>Samm 3: Installida VMware komponendid

Kui soovite ise VMware virtuaalse installige masinad management server VMware järgmised komponendid.

1. [Laadige alla](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) ja installige VMware vSphere PowerCLI 6.0.
2. Uuesti serverisse.


## <a name="step-4-download-a-vault-registration-key"></a>Samm 4: Allalaadimine vault registreerimise võti

1. Juhtimise avage serveri saidi taastamise konsooli Azure. Klõpsake lehe **Taastamise teenused** vault Kiirkäivituse lehe avamiseks. Lühijuhend saate avada ka ikooni abil.

    ![Lühijuhend ikoon](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)

2. Klõpsake lehel **Kiirkäivituse** **Sihtrakenduse ettevalmistamiseks ressursid** > **registreerimise võti alla laadida**. Registreerimise fail luuakse automaatselt. See kehtib 5 päeva pärast seda, kui see on loodud.


## <a name="step-5-install-the-management-server"></a>Juhis 5: Installida management server
> [AZURE.TIP] Veenduge, et need URL-id juurdepääsetavat management serverist.
>
- *. hypervrecoverymanager.windowsazure.com
- *. accesscontrol.windows.net
- *. backup.windowsazure.com
- *. blob.core.windows.net
- *. store.core.windows.net
- https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi
- https://www.msftncsi.com/NCSI.txt




[AZURE.VIDEO enhanced-vmware-to-azure-setup-registration]

1. **Kiirkäivituse** lehel alla laadida ühendatud installifail server.

2. Käivitage installifail häälestamise saidi taastamise ühendatud häälestusviisardi käivitamiseks.

3.  **Enne alustamist** valige **konfiguratsiooniserveri ja protsessi serveri installimine**.

    ![Enne alustamist](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. **Kolmanda osapoole tarkvara litsentsi** nuppu **nõustun** alla laadima ja installima MySQL-i. 

    ![= Kolmanda osapoole tarkvara](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)

5. **Registreerimise** Sirvi ja valige soovitud hoidlast allalaaditud registreerimise võti.

    ![Registreerimine](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)

6. **Interneti-sätted** määrata, kuidas pakkuja konfigureerimine serveris töötav teenus loob ühenduse Azure saidi taastamise Interneti kaudu.

    - Kui soovite luua ühenduse puhverserveri, mis on seadistatud seadmesse valige **Loo ühendus olemasoleva puhverserveri sätted**.
    - Kui soovite ühendada otse pakkuja valige **ühendus otse ilma proxy**.
    - Kui olemasolev puhverserver nõuab autentimist või soovite kasutada kohandatud puhverserveri ühenduse pakkuja, valige **Loo ühendus kohandatud puhverserveri sätted**.
        - Kui kasutate kohandatud puhverserveri aadressi, portide ja mandaadi määramiseks peate
        - Kui kasutate puhverserverit, mis on teil peaks juba lubatud järgmised URL-ID:
            - *. hypervrecoverymanager.windowsazure.com;    
            - *. accesscontrol.windows.net; 
            - *. backup.windowsazure.com; 
            - *. blob.core.windows.net; 
            - *. store.core.windows.net
            

    ![Tulemüür](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

7. **Eeltingimused, märkige ruut** häälestamise töötab sisse, veenduge, et saate käivitada installimise. 

    
    ![Eeltingimused](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     Kui kuvatakse hoiatus kohta **globaalne kellaaja sünkroonimine märkige** veenduge, et see kellaaeg süsteemikell (**kuupäev ja kellaaeg** sätted) on sama, mis ajavöönd.

    ![TimeSyncIssue](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

8. **MySQL-i** konfiguratsiooni loomine MySQL-i serveri eksemplar, mis installitakse logimiseks mandaat.

    ![MySQL-i](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)

9. **Keskkonna üksikasjad** , valige kas kavatsete VMware VMs korrata. Kui olete, siis Installiprogramm kontrollib, kas PowerCLI 6.0 on installitud.

    ![MySQL-i](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)

10. **Installige asukoht** kuhu soovite installida kahendfaile ja vahemällu talletamine, valige. Valige ketas, millel on vähemalt 5 GB salvestusruumi, kuid soovitame vahemälu ketas vähemalt 600 GB vaba ruumi.

    ![Installi asukoht](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)

11. Määrake **Võrgu valimine** kuulajale (võrguadapteri ja SSL port) aluseks on konfiguratsiooniserveri saatmine ja vastuvõtmine dispersioonanalüüs andmed. Saate muuta vaikimisi port (9443). Lisaks selle pordi pordi 443 kasutab orchestrates dispersioonanalüüs toimingute veebiserverisse. 443 ei tohiks kasutada kopeerimise liikluse vastu.


    ![Võrgu valimine](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



12.  **Kokkuvõttes** teave üle ja klõpsake nuppu **Installi**. Pärast installi lõppemist on loodud parool. Peate selle dispersioonanalüüs lubamisel nii kopeerige see ja hoidke seda turvalises asukohas.

    ![Kokkuvõte](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)



13.  **Kokkuvõttes** lugege teavet.

    ![Kokkuvõte](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)

>[AZURE.WARNING]Microsoft Azure taastamise teenuse Agent puhverserveri peab olema häälestus.
>Kui installimine on lõpule viidud, käivitage rakendus nimega "Microsoft Azure taastamise teenused Shell" Windowsi menüü Start kaudu. Käivitage käsuviiba aken, mis avab järgmised määramine käskude häälestamine puhverserveri sätted.
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine



### <a name="run-setup-from-the-command-line"></a>Installiprogramm käsurea kaudu

Saate käivitada ka ühendatud viisardi käsureale, järgmiselt:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Kui:

- / ServerMode: kohustuslik. Saate määrata, kas installimist peaksite installima konfiguratsiooni ja protsessi serverid või ainult server protsess (kasutatakse installida täiendavad protsessi serverid). Sisestage väärtused: CS, PS.
- InstallDrive: kohustuslik. Määrab kausta, kuhu on installitud komponendid.
- / MySQLCredFilePath. Kohustuslik. Saate määrata, kui MySQL-i serveri mandaadid ei loo faili tee. Malli loomiseks faili avada.
- / VaultCredFilePath. Kohustuslik. Vault identimisteabe faili asukoht
- / EnvType. Kohustuslik. Installi tüüp. Väärtused: VMware, NonVMware
- / PSIP ja /CSIP. Kohustuslik. Protsessi serveri ja konfiguratsiooniserveri IP-aadress.
- / PassphraseFilePath. Kohustuslik. Parooli faili asukoht.
- / ByPassProxy. Valikuline. Määrab management server ühendab Azure ilma proxy.
- / ProxySettingsFilePath. Valikuline. Saate määrata kohandatud proxy (vaikimisi puhverserveri server, mis nõuab autentimist) või kohandatud puhverserveri sätted




## <a name="step-6-set-up-credentials-for-the-vcenter-server"></a>Samm 6: Häälestamine identimisteabe vCenter server

> [AZURE.VIDEO enhanced-vmware-to-azure-discovery]

Protsessi serveri saaksid automaatselt tuvastada VMware VMs, mida haldab vCenter server. Automaatse tuvastamise peab saidi taastamise konto ja mandaat, millele pääsete juurde vCenter server. See pole oluline, kui te olete imitatsiooniga füüsilise serveri ainult.

Seda teha järgmiselt:

1. Funktsiooni vCenter server luua rolli (**Azure_Site_Recovery**) vCenter tasemel [vajalikke õigusi](#vmware-permissions-for-vcenter-access).
2. VCenter kasutajale määrata **Azure_Site_Recovery** roll.

    >[AZURE.NOTE] VCenter kasutajakonto, mis on kirjutuskaitstud rolli saab käitada Tõrkesiirde ilma sulgemise kaitstud allika masinad. Kui soovite need masinad sulgemiseks peate Azure_Site_Recovery roll. Pöörake tähelepanu sellele, kui teil on ainult migreerimine VMs, VMware Azure ja ei pea failback seejärel kirjutuskaitstud roll piisavalt.

3. Konto lisamiseks avage **cspsconfigtool**. See on saadaval töölaua otsetee ja asub kaustas [installida asukoht] \home\svsystems\bin.
2. Klõpsake menüüs **Kontod haldamine** nuppu **Lisa konto**.

    ![Konto lisamine](./media/site-recovery-vmware-to-azure-classic/credentials1.png)

3. Lisage **Konto üksikasjad** identimisteavet, mida saab kasutada vCenter serverit. Pange tähele, et võib kuluda rohkem kui 15 minutit kuvada portaali konto nimi. Klõpsake nuppu Värskenda kohe värskendamiseks vahekaardil **Konfiguratsiooni serverid** .

    ![Üksikasjad](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>Juhis 7: VCenter serverid ja ESXi hosts lisamine

Kui olete imitatsiooniga VMware VMs, peate lisama vCenter server (või ESXi host).

1. **Serverid** > **Konfiguratsiooni serverid** valige konfiguratsiooniserveri > **Lisa vCenter server**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)

2. Lisada vCenter server või ESXi host üksikasjad, määratud vCenter server eelmises etapis ja protsessi server avastada VMware VMs, mida haldab vCenter server kasutatud konto nimi. Pange tähele, et vCenter server või ESXi host peaks asuma server server protsess on installitud samasse võrku.

    >[AZURE.NOTE] Kui lisate vCenter server või ESXi host kontoga, mis ei sisalda administraatoriõigusi vCenter või hosti server, siis veenduge, et selle vCenter või ESXi kontod on need õigused, mis on lubatud: andmekeskuse, andmesalve, kausta, Jost, võrku, ressursside, virtuaalse masina vSphere jaotatud vahetamine. Lisaks vCenter server vajab salvestusruumi vaadete au.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)

3. Kui on discovery lõpule jõudnud vCenter server loetletud vahekaarti **Konfiguratsiooni serverid** .

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)


## <a name="step-8-create-a-protection-group"></a>Samm 8: Kaitse rühma loomine

> [AZURE.VIDEO enhanced-vmware-to-azure-protection]


Rühmade on loogiline rühmitustega virtuaalmasinates või füüsilise serveri, mida soovite kaitsta, kasutades sama kaitse sätteid. Sätted rakenduvad kaitse rühma ja need sätted on rakendatud kõik virtuaalmasinates/füüsilise masinad, et rühma lisada.

1. Avage **Kaitstud üksuste** > **Kaitse rühma** ja klõpsake nuppu kaitse rühma lisada.

    ![Kaitse rühma loomine](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)

2. **Määrake rühma sätted** lehel sisestage rühma nimi ja **kaudu** valige konfiguratsiooniserveri, mida soovite rühma luua. **Target (sihtkoht)** on Azure.

    ![Rühma sätted](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)

3. **Määrake Dispersioonanalüüs sätete** lehel kõik masinad jaotises kasutatud dispersioonanalüüs sätete konfigureerimine.

    ![Kaitse rühma dispersioonanalüüs](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

    - **Mitme VM järjepidevuse**: Kui lülitate selle loob ühiskasutuses rakenduse ühtsete taastamise punkte arvutites jaotises kaitse. See säte on kõige olulisemad, kui kõik jaotises kaitse masinad töötavad sama töökoormus. Kõik masinad on sama andmepunkti taastada. See on saadaval, kas olete imitatsiooniga VMware VMs või Windows/Linux füüsilise serveri.
    - **RPO lävi**: seab selle RPO. Teatiste luuakse siis, kui järjestikusi andmeid kaitse dispersioonanalüüs ületab konfigureeritud RPO lävi väärtus.
    - **Osutage taastamine säilitus**: säilituspoliitika aknas saate määrata. Kaitstud masinad taastada mis tahes punkti see aken.
    - **Rakenduse ühtsete hetktõmmise sagedus**: saate määrata, kui sageli luuakse taastamise punkte rakenduse ühtsete pilte sisaldavate.

Kui klõpsate märke luuakse kaitse rühma teie määratud nimi. In addition luuakse teise kaitse rühma nimega < kaitse rühma-nime Failback). Kui teil ei õnnestu tagasi saidile kohapealse pärast Tõrkesiirde Azure, kasutatakse kaitse see jaotis. Need on loodud **Üksuste kaitstud** lehel saate jälgida kaitse rühmad.

## <a name="step-9-install-the-mobility-service"></a>Samm 9: Installida mobiilsus teenuse

Mobiilsus teenuse installimiseks on esimene samm kaitse virtuaalmasinates ja füüsilise serveri lubamine. Selleks on kaks võimalust:

- Automaatselt tõuketeatised ja teenuse serverist protsessi iga arvutisse installida. Märkus, et kui lisate masinad rühmale kaitse ja need on juba olemas Mobility teenuse tõuketeatised installimise vastav versioon ei esine.
- Teenuse abil oma ettevõtte tõuketeatised meetod, nt WSUS või System Center Configuration Manager automaatselt installida. Veenduge, et olete häälestanud management serveri enne selle tegemist.
- Käsitsi installida igas arvutis, mida soovite kaitsta. AKE kindel, et olete häälestanud management serveri enne selle tegemist.


### <a name="install-the-mobility-service-with-push-installation"></a>Tõuketeatised installimisega mobiilsus teenuse installimine

Kui lisate masinad kaitse rühma mobiilsus teenuse automaatselt lükata ja protsessi server iga arvutisse installitud.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Automaatse tõuketeatised Windows arvutites ettevalmistamine

Siin on, kuidas ette valmistada Windowsi masinad nii, et protsessi server saab automaatselt installida mobiilsus teenuse.

1.  Looge konto, mida saab kasutada protsessi serveri juurde pääseda arvuti. Konto peaks olema administraatoriõigused (kohalik või Domeen). Pange tähele, et neid mandaate kasutatakse ainult mobiilsus teenuse tõuketeatised installi.

    >[AZURE.NOTE] Kui te ei kasuta domeenikonto peate keelata Remote kasutaja juurdepääsu reguleerimine kohalikus arvutis. Selleks register jaotises HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System Lisage DWORD kirje LocalAccountTokenFilterPolicy väärtuseks 1 all. Registri lisamiseks avage kirje lisamine CLI käsk või sisestage PowerShelli kaudu **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  Windowsi tulemüüri masina, mida soovite kaitsta, valige **Luba rakendus või läbi tulemüüri** ja luba **failide ja printeri ühiskasutus** ja **Windows Management seadmeid**. Masinad, mis kuuluvad domeeni saate konfigureerida tulemüüri poliitika rühmapoliitika objekti.

    ![Tulemüüri sätted](./media/site-recovery-vmware-to-azure-classic/mobility1.png)

2. Loodud konto lisamiseks tehke järgmist.

    - Avatud **cspsconfigtool**. See on saadaval töölaua otsetee ja asub kaustas [installida asukoht] \home\svsystems\bin.
    - Klõpsake menüüs **Kontod haldamine** nuppu **Lisa konto**.
    - Lisage loodud konto. Pärast konto lisamist peate mandaat, kui lisate masina kaitse rühma.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Automaatse tõuketeatised Linux serverites ettevalmistamine

1.  Veenduge, et Linux arvutisse, mida soovite kaitsta on toetatud, nagu on kirjeldatud [kohapealse eeltingimused](#on-premises-prerequisites). Veenduge, et seade, mida soovite kaitsta vahel on võrguühendus ja management server, mis töötab serveri protsess.

2.  Looge konto, mida saab kasutada protsessi serveri juurde pääseda arvuti. Konto peaks olema root kasutaja allikas Linux server. Pange tähele, et neid mandaate kasutatakse ainult mobiilsus teenuse tõuketeatised installi.

    - Avatud **cspsconfigtool**. See on saadaval töölaua otsetee ja asub kaustas [installida asukoht] \home\svsystems\bin.
    - Klõpsake menüüs **Kontod haldamine** nuppu **Lisa konto**.
    - Lisage loodud konto. Pärast konto lisamist peate mandaat, kui lisate masina kaitse rühma.

3.  Kontrollige, et muutsite faili allikas Linux server sisaldab kirjeid, mis kohaliku hostname vastendada kõik võrguadapteri seotud IP-aadressid.
4.  Installige uusim openssh, openssh-server, openssl pakettide arvutisse, mida soovite kaitsta.
5.  Veenduge, et SSH port 22 on lubatud ja töötab.
6.  Lubage SFTP alasüsteemi ja parooli autentimine sshd_config faili järgmiselt:

    - Logige sisse root.
    - Otsige fail /etc/ssh/sshd_config rida, mis algab väärtusega PasswordAuthentication.
    - Kommenteerige rea välja ja muutke kaudu **ei** väärtuseks **Jah**.
    - Otsige üles rida, mis algab väärtusega **alasüsteemi** ja Eemalda kommentaar rea.

        ![Linux](./media/site-recovery-vmware-to-azure-classic/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>Mobiilsus teenuse käsitsi installimine

Paigaldajaid on saadaval C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository.

Andmeallika operatsioonisüsteem | Installi Mobility teenusefail
--- | ---
Windows Server (ainult 64-bitine) | Microsoft-ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6.4, 6.5, 6.6 (ainult 64-bitine) | Microsoft-ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 SP3 (ainult 64-bitine) | Microsoft-ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle'i Enterprise Linux 6.4 6.5 (ainult 64-bitine) | Microsoft-ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-manually-on-a-windows-server"></a>Käsitsi installimine Windows server


1. Laadige alla ja käivitage installer oluline.
2. **Enne alustamist **valige **mobiilsus teenuse**.

    ![Mobiilsus teenuse](./media/site-recovery-vmware-to-azure-classic/mobility3.png)

3. Määrake **Konfiguratsiooni serveri üksikasjad** IP-aadress ja management server management serveri komponendid installimisel loodud parool. Saate tuua parool töötab: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** management server.

    ![Mobiilsus teenuse](./media/site-recovery-vmware-to-azure-classic/mobility6.png)

4. **Installige** asukohas jätke vaikeasukoha ja klõpsake nuppu **Järgmine** installimise alustamiseks.
5. **Installi edenemist** jälgida installi ja taaskäivitage arvuti viiba.

Saate installida ka käsurea kaudu.

UnifiedAgent.exe [/ rolli < Agent/MasterTarget >] [/ InstallLocation <Installation Directory>] [/ CSIP <IP address of CS to be registered with>] [/ PassphraseFilePath <Passphrase file path>] [/ LogFilePath <Log File Path>]

Kui:

- / Rolli: kohustuslik. Saate määrata, kas mobiilsus teenuse peaks olema installitud.
- / InstallLocation: kohustuslik. Saate määrata, kus teenuse installida.
- / PassphraseFilePath: kohustuslik. Saate määrata konfiguratsiooni serveri parool.
- / LogFilePath: kohustuslik. Saate määrata log setup failide asukoht

#### <a name="uninstall-mobility-service-manually"></a>Mobiilsus teenuse käsitsi desinstallimine

Mobiilsus teenuse saab lisada eemaldada programmi juhtpaneeli kaudu või kasutades käsurea desinstallida.

Käsk uninstall mobiilsus teenuse käsurea kaudu on

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="modify-the-ip-address-of-the-management-server"></a>Management serveri IP-aadressi muutmine

Pärast viisardi käivitamist saate muuta management serveri IP-aadress järgmiselt:

1. Avage soovitud faili hostconfig.exe (asub töölaual).
2. Klõpsake menüü **Global** saate muuta management serveri IP-aadress.

    >[AZURE.NOTE] Ainult muudate management serveri IP-aadress. Management serveri side pordinumber peab olema 443 ja kasutada HTTPS vasakule sisse lülitatud. Parool ei tohiks muuta.

    ![Management serveri IP-aadress](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-manually-on-a-linux-server"></a>Käsitsi installimine Linux server:

1. Kopeerige vastav tõrva arhiivi põhjal tabeli kohal Linux arvutisse, mida soovite kaitsta.
2. Shell programmi avamiseks ja väljavõte ZIP tar Arhiiv kohalik tee töötab.`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Kohalikku kausta, kuhu ekstraktitud tõrva arhiivi sisu passphrase.txt faili loomine. Teha seda kopeerige parool C:\ProgramData\Microsoft Azure saidi Recovery\private\connection.passphrase management server ja salvestage see passphrase.txt käivitades *`echo <passphrase> >passphrase.txt`* Shellis.
4. Installige mobiilsus teenuse sisestades *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Määrake management serveri sisemine IP-aadress ja veenduge, et pordi 443 oleks märgitud.

**Saate installida ka käsurea kaudu**.

1. Kopeerige parool C:\Program Files (x86) \InMage Systems\private\connection management server ja salvestage see "passphrase.txt" dokumendihalduse serveris. Käivitage järgmised käsud. Käesolevas näites management serveri IP-aadress on 104.40.75.37 ja tuleks HTTPS pordi 443.

Tootmise server installimiseks tehke järgmist.

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Klõpsake serveri installimiseks tehke järgmist.


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>Samm 10: Lubamine masina kaitse

Kaitse lubamiseks lisate virtuaalmasinates ja füüsilise serveri kaitse rühma. Enne alustamist arvestage, et kui te kasutate kaitsmine VMware virtuaalmasinates:

- VMware VMs avastatakse iga 15 minuti ja võib kuluda rohkem kui 15 minutit pärast on discovery saidi taastamine portaalis kuvada.
- Keskkonna muutused virtuaalse masina (nt VMware tööriistad install) võib võtta ka rohkem kui 15 minutit, et saidi taastamine värskendada.
- Saate viimast avastatud VMware vms välja **Viimase kontakti veebisaidil** vCenter server/ESXi Host vahekaardil **Konfiguratsiooni serverid** .
- Kui teil on juba loodud kaitse rühma ja vCenter Server või ESXi hosti lisamine pärast seda, võib kuluda rohkem kui 15 minutit Azure saidi taastamise portaali värskendamiseks ja virtuaalmasinates loetletud dialoogiboksis **Lisa masinad kaitse rühma** .
- Kui soovite kohe jätkamiseks kaitse rühma ootamata ajastatud Discovery masinad lisamise, tõstke esile konfiguratsiooniserveri (ärge klõpsake seda) ja klõpsake nuppu **Värskenda** .

Lisaks Pange tähele järgmist.

- Soovitame teil nii, et nad kajastavad oma töökoormus arhitekt kaitse rühm. Näiteks saate lisada mõne kindla rakenduse samasse, kus käitatakse.
- Masinad lisamisel kaitse rühma protsessi server automaatselt sunnib ja installib mobiilsus teenuse, kui see pole juba installitud. Pange tähele, et peate olema tõuketeatised süsteem, nagu on kirjeldatud eelmises etapis ettevalmistamine.


Kaitse rühma masinad lisamiseks tehke järgmist.

1. Klõpsake **kaitstud üksuste** > **Kaitse rühma** > **masinad** > Lisa masinad. \As hea tava
2. **Valige** virtuaalmasinates kui VMware virtuaalmasinates kaitsmisele valige vCenter server, mis on hallata oma virtuaalmasinates (või EXSi host, mil nad töötab) ja valige masinad.

    ![Luba kaitse](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)

3.  **Valige** virtuaalmasinates kui füüsilise serveri kaitsmisele **Lisada füüsilise masinad** viisardi pakkuda IP-aadress ja sõbralik nimi. Valige operatsioonisüsteemi pere.

    ![Luba kaitse](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)

4. Valige **Määrake Target ressursid** salvestusruumi kontot kasutate kopeerimise ja valige, kas sätted tuleks kasutada kõigi töökoormus. Pange tähele, et premium salvestusruumi konto pole praegu toetatud.

    >[AZURE.NOTE] 1. me ei toeta salvestusruumi kontod ressursi rühma [uue Azure portaali](../storage/storage-create-storage-account.md) abil loodud liikuda.                           2.[migreerimine salvestusruumi kontod](../resource-group-move-resources.md) üle ühe tellimuse ressursi rühma või tellimustes salvestusruumi kontod kasutatakse juurutamine saidi taastamine ei toetata.

    ![Luba kaitse](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)

5. **Määrake kontod** valige konto, saate [konfigureeritud](#install-the-mobility-service-with-push-installation) kasutama automaatse installi mobiilsus teenuse jaoks.

    ![Luba kaitse](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)

6. Klõpsake märkeruutu masinad kaitse rühma lisamise lõpetanud ja käivitada algse dispersioonanalüüs iga seadme jaoks.

    >[AZURE.NOTE] Kui tõuketeatised install on koostatud mobiilsus teenuse installitakse automaatselt arvutites, mis ei ole seda, kui need on lisatud kaitse rühma. Kui teenus on installi kaitse töö algab ja nurjub. Pärast selle peate igas arvutis, kus on installitud mobiilsus teenuse käsitsi taaskäivitama. Pärast uuesti kaitse töö algab uuesti ja algse dispersioonanalüüs esineb.

Saate jälgida olek lehel **tööde haldamine** .

![Luba kaitse](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Lisaks kaitse oleku saab jälgida **Kaitstud üksuste** > <protection group name> > **Virtuaalmasinates**. Pärast algse kopeerimine on lõpule jõudnud ja andmed on sünkroonitud, seadme olekuks** kaitstud**.

![Luba kaitse](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)


## <a name="step-11-set-protected-machine-properties"></a>Samm 11: Set kaitstud seadme atribuudid

1. Pärast masina on **kaitstud** olek saate konfigureerida selle Tõrkesiirde atribuudid. Kaitse rühma üksikasjad valige arvuti ja avage vahekaart **konfigureerimine** .
2. Saidi taastamine soovitab atribuudid Azure VM ja tuvastab asutusesisese võrgusätted automaatselt.

    ![Virtuaalne seadme atribuutide seadmine](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)

3. Saate muuta järgmisi sätteid:

    -  **Azure'i VM nimi**: see on nimi, mida pööratakse masina Azure pärast Tõrkesiirde. Nimi peab vastama Azure nõuetele.

    -  **Azure'i VM suurus**: võrguadapteri arv on tingitud target virtuaalse masina jaoks määratud suurusega. [Lisateavet vt](../virtual-machines/virtual-machines-linux-sizes.md/#size-tables) suurused ja adapterit kohta. Pange tähele järgmist.

        - Kui virtuaalse masina selle suurust muuta ja sätete salvestamiseks, võrguadapteri arvu muutmine **konfigureerimine** vahekaardi järgmisel avamisel. Võrguadapteri, target virtuaalmasinates arv on minimaalne allika virtual arvutisse võrguadapteri arvu ja valitud virtuaalse masina suurust ei toeta võrguadapteri arvu ülempiir.
            - Kui allika arvutisse võrguadapteri arv on väiksem või võrdne arv adapterit masina sihtformaat lubatud, siis sihtkohas on samal hulgal adapterit allikana.
            - Kui adapterit allika virtual masina arv ületab lubatud target suurus ja seejärel soovitud suurus maksimum kasutada number.
            - Näiteks kui allika arvuti on kaks võrguadapteri ja seadme sihtformaat toetab neli, sihtarvutis on kaks adapterit. Kui allika arvuti on kaks adapterit, kuid toetatud sihtformaat toetab ainult üks siis sihtarvutis on ainult üks adapterit.
        - Kui virtuaalse masina on sama Azure võrku ühendatud mitme võrguadapteri kõik adapterit peaks.
    - **Azure'i võrgu**: määrake Azure võrk, mis on seotud pärast Tõrkesiirde Azure VMs. Kui te ei määra ühte siis Azure VMs ei ühendatud võrgule. Lisaks peate määramiseks muu Azure kaudu, kui soovite failback Azure kohapealse saidile. Failback nõuab VPN-ühendus on Azure võrgu ja on kohapealse võrgu vahel.
    - **Azure'i IP address/alamvõrgu**: iga võrguadapteri valite alamvõrku, millele tuleks ühenduse Azure VM. Pange tähele järgmist.
        - Kui võrguadapteri allika masina on konfigureeritud kasutama staatiline IP-aadress seejärel saate määrata staatiline IP-aadress Azure VM. Kui te ei esita staatiline IP-aadress klõpsake mis tahes saadaval IP-aadress eraldatakse. Kui target IP-aadress on määratud, kuid see on juba mõne muu VM Azure nurjub Tõrkesiirde. Kui võrguadapteri allika masina on konfigureeritud kasutama DHCP kuvatakse DHCP Azure sätteks on.

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>Samm 12: Taastamis loomine ja käivitamine Tõrkesiirde

> [AZURE.VIDEO enhanced-vmware-to-azure-failover]

Saate käivitada Tõrkesiirde ühe arvuti jaoks või ei õnnestu üle mitme virtuaalmasinates, mis sama toimingu või käivitage sama töökoormus. Kasutada mitme masinad samal ajal saate lisada need taastamis.

### <a name="create-a-recovery-plan"></a>Taastamis loomine

1. **Lepingute taastamine** lehel klõpsake nuppu **Lisa taastamise kava** ja lisage taastamis. Valige **Azure** mis Sihtvaluuta üksikasjad lepingu raames.

    ![Taastamise kava konfigureerimine](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)

2. **Valige virtuaalse masina** valige kaitse rühma ja seejärel valige jaotises lisamiseks taastamise kava masinad.

    ![Lisage virtuaalmasinates](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

Saate kohandada rühmad ja jada tellimuse loomiseks, kus on nurjunud masinad taastamise kava üle leping. Saate lisada ka skripte ja käsitsi toimingute puhul kuvatavaid juhiseid. Skriptide loomist käsitsi või [Azure'i automaatika tegevusraamatud](site-recovery-runbook-automation.md)abil. [Lisateavet leiate teemast](site-recovery-create-recovery-plans.md) kohandamise taastamise kohta.

## <a name="run-a-failover"></a>Käivitage Tõrkesiirde

Enne Tõrkesiirde käivitamist Pange tähele järgmist.


- Veenduge, et haldamine serveris ja saadaval - muidu Tõrkesiirde nurjub.
- Kui käivitate planeerimata Tõrkesiirde Pange tähele järgmist.

    - Võimaluse korral tuleks sulgeda esmane masinad enne käivitamist planeerimata Tõrkesiirde. See tagab, et teil pole nii lähte-kui ka koopia masinad töötab samal ajal. Kui te olete imitatsiooniga VMware VMs siis planeerimata Tõrkesiirde käivitamisel saate määrata, et saidi taastamine peaksid olema parima allika masinad sulgeda. Sõltuvalt esmane saidi see võib või ei pruugi töötada. Kui te olete imitatsiooniga füüsilise serveri saidi taastamise ei paku see suvand.
    - Planeerimata Tõrkesiirde täitmisel lõpetab andmete kopeerimine esmane masinad nii, et kõik andmed delta ei üle pärast planeerimata Tõrkesiirde hakkab.

- Kui soovite pärast Tõrkesiirde koopia virtuaalse masina Azure ühenduse, lubada kaugtöölaua ühendus allika arvutisse enne, kui käivitate selle Tõrkesiirde ning lubada RDP-ühendus tulemüüri kaudu. Samuti peate lubama RDP avaliku lõpp-punkti Azure virtuaalse masina pärast Tõrkesiirde. Järgige neid [head tavad](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) tagamaks, et RDP töötab pärast Tõrkesiirde.

>[AZURE.NOTE] Parima jõudluse Tõrkesiirde valimisel Azure saamiseks veenduge, et on installitud Azure Agent kaitstud kohapeal. See aitab käivitamist kiiremini ja aitab diagnoosimise küsimuste korral. Linux agent võib olla leitud [siin](https://github.com/Azure/WALinuxAgent) - ja Windowsi agent leiate [siit](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="run-a-test-failover"></a>Käivitage test Tõrkesiirde

Käivitage test Tõrkesiirde simuleerida oma Tõrkesiirde ja taastamise protsessid isoleeritakse võrku, mis ei mõjuta tootmiskeskkonnast ja tavalise dispersioonanalüüs jätkub nagu tavaliselt. Test Tõrkesiirde käivitab allikas ja käivitada seda mitmel viisil.

- **Ei määra muu Azure kaudu**: kui käivitate test Tõrkesiirde ilma võrgu test lihtsalt kontrollida, et virtuaalmasinates alustada ja Azure õigesti kuvada. Virtuaalmasinates ei Azure võrku ühendatud pärast Tõrkesiirde.
- **Määrake soovitud Azure võrgu**: seda tüüpi Tõrkesiirde kontrollib, et kogu dispersioonanalüüs keskkond on kuni ootuspäraselt ja et Azure'i virtuaalmasinates on määratud võrku ühendatud.


1. Lehe **Taastamine lepingute** valige leping ja klõpsake nuppu **Testi Tõrkesiirde**.

    ![Lisage virtuaalmasinates](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)

2. Valige **Kinnitage testida Tõrkesiirde** **pole** näitamaks, et te ei soovi kasutada ka Azure võrgu testi Tõrkesiirde või valige võrgu pärast Tõrkesiirde on ühendatud testi VMs. Klõpsake märkeruutu selle Tõrkesiirde käivitamiseks.

    ![Lisage virtuaalmasinates](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)

3. **Klõpsake vahekaarti** Tõrkesiirde jälgimiseks.

    ![Lisage virtuaalmasinates](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)

4. Pärast selle Tõrkesiirde lõpulejõudmist saate ka peaks olema võimalus leiate Azure'i koopia arvutisse kuvatakse Azure'i portaalis > **Virtuaalmasinates**. Kui soovite käivitada RDP ühenduse Azure VM peate avage port 3389 VM lõpp-punkti.

5. Kui olete lõpetanud, kui Tõrkesiirde jõuab lõpuleviimine testimine etapp, klõpsake täieliku testi lõpuni. Märkmete salvestamine ja salvestamine test Tõrkesiirde seostatud märkusi.

6. Klõpsake linki automaatselt puhastamiseks testi keskkonna **test Tõrkesiirde on lõpule viidud** . Kui seda tehakse test Tõrkesiirde kuvada **lõpuleviimine** olek. Mis tahes elemente või luua automaatselt ajal test Tõrkesiirde VMs kustutatakse. Pange tähele, et kui test Tõrkesiirde kestab kauem kui kaks nädalat lõpuleviimist jõuga.



### <a name="run-an-unplanned-failover"></a>Planeerimata Tõrkesiirde käivitamine

Planeerimata Tõrkesiirde käivitatakse Azure ja võivad täita ka siis, kui esmane sait pole saadaval.


1. Lehe **Taastamine lepingute** valige leping ja klõpsake nuppu **Tõrkesiirde** > **Planeerimata Tõrkesiirde**.

    ![Lisage virtuaalmasinates](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)

2. Kui olete imitatsiooniga VMware virtuaalmasinates saate valida, proovige ja kohapealse VMs sulgeda. See on parim – vaeva ja Tõrkesiirde jätkavad kas vaeva õnnestus või mitte. Kui see ei õnnestu tõrke üksikasjade ilmuvad **klõpsake vahekaarti **> **Planeerimata Tõrkesiirde tööde haldamine**.

    ![Lisage virtuaalmasinates](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

    >[AZURE.NOTE] See suvand pole saadaval, kui te olete imitatsiooniga füüsilise serveri. Peate proovida ja sulgeda need käsitsi kui võimalik.

3. Valige taastamise punkti, mida soovite kasutada, on tõrkesiire **Tõrkesiirde kinnitada,** kontrollige Tõrkesiirde suunda (Azure). Kui olete lubanud mitme-VM konfigureerimisel dispersioonanalüüs atribuudid saate taastada rakendusest või krahh ühtsete taastamine Viimane punkt. Võite valida ka **kohandatud taastamine osutage** taastada varasemasse aega. Klõpsake märkeruutu selle Tõrkesiirde käivitamiseks.

    ![Lisage virtuaalmasinates](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)

3. Oodake planeerimata Tõrkesiirde töö lõpuleviimiseks. **Klõpsake vahekaarti** Tõrkesiirde edenemist saate jälgida. Pange tähele, et isegi juhul, kui esineb viga planeerimata Tõrkesiirde taastamise kava töötab seni, kuni see on lõpule viidud. Peaks olema ka vaadata koopia Azure'i virtuaalmasinates Azure'i portaalis kuvataks kohapeal.

### <a name="connect-to-replicated-azure-virtual-machines-after-failover"></a>Ühenduse loomine kopeeritud pärast Tõrkesiirde Azure'i virtuaalmasinates

Selleks, et ühenduse kopeeritud Azure'i virtuaalmasinates pärast Tõrkesiirde siin on teil vaja:

1. Kaugtöölaua ühendus peaks olema lubatud esmane seade.
2. Windowsi tulemüüri esmane arvutisse RDP lubama.
3. Pärast Tõrkesiirde peate avaliku lõpp-punkti jaoks Azure virtuaalse masina RDP lisamiseks.

[Lisateavet vt](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) häälestamise kohta.


## <a name="deploy-additional-process-servers"></a>Täiendavad protsessi serverid juurutamine

Kui teil on skaala kontorist juurutamise üle 200 allika masinad või kogu päeva piimapütt määr ületab 2 TB, peate täiendavad protsessi serverid käsitlema liikluse maht. Häälestada serveriks täiendavad protsessi kontrollige nõudeid [täiendavate protsessi](#additional-process-servers) serverid ja seejärel järgige juhiseid protsessi serveri seadistamine. Pärast häälestamist serveri saate konfigureerida allika masinad seda kasutada.

### <a name="set-up-an-additional-process-server"></a>Täiendavad protsessi serveri seadistamine

Saate seadistada täiendavaid protsessi serverisse järgmiselt:

- Käivitage viisard ühendatud ainult protsessi server management serveri konfigureerima.
- Kui soovite hallata ainult uue protsessi serveri kasutamine andmete kopeerimine, peate oma kaitstud masinad selleks migreerida.

### <a name="install-the-process-server"></a>Protsessi serverit installida

1. Lehel kiirjuhendi allalaadimine ühendatud installifail saidi taastamine komponendi installimiseks. Käivita häälestus.
2. **Enne alustamist** valige **Lisa täiendavad protsessi serverid välja juurutamise skaala**.

    ![Protsessi serveri lisamine](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)

3. Viisardi lõpuleviimine samamoodi kui tegite [häälestamine](#step-5-install-the-management-server) esimene management server.

4. Määrake **Konfiguratsiooni serveri üksikasjad** IP-aadress algse management server, kuhu installisite tarkvarapaketi konfiguratsiooniserveri ja parool. Käivitage algse dokumendihalduse serveris ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** saada parool.

    ![Protsessi serveri lisamine](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Uus protsess server kasutama masinad migreerimine

1. **Konfiguratsiooni serverid**Avage > **Server** > algse management serveri nimi > **Serveri üksikasjad**.

    ![Protsessi uuendamine.](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)

2. Klõpsake loendis **Protsessi Servers** kõrval, mida soovite muuta serveri **Muuta protsessi serveri** .

    ![Protsessi uuendamine.](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)

3. **Muuda protsessi**serveris > **Target protsessi Server** valige uus management server, ja seejärel valige uus protsess server käsitlemiseks virtuaalmasinates. Klõpsake ikooni teabe serveri kohta teabe saamiseks. Keskmine ruumi, mida on vaja paljundada iga valitud virtuaalse masina uue protsessi server kuvatakse aidata teil teha laadimine otsuseid. Klõpsake märkeruutu imitatsiooniga serveriga uue protsessi käivitamiseks.

    ![Protsessi uuendamine.](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)




## <a name="vmware-permissions-for-vcenter-access"></a>VMware vCenter juurdepääsu õigusi

Protsessi serveri saaksid automaatselt tuvastada VMs vCenter serveris. Automaatse tuvastamise sooritamiseks peate määratleda saidi taastamine Vcenteri serveri juurdepääsu lubamiseks tasemel vCenter rolli (Azure_Site_Recovery). Pange tähele, et kui teil on vaja ainult migreerimine VMware masinad Azure ja ei vaja failback Azure, saate määratleda kirjutuskaitstud rolli, mis on piisavalt. Saate seada õigusi, nagu on kirjeldatud [samm 6: häälestamine identimisteabe vCenter server](#step-6-set-up-credentials-for-the-vcenter-server) järgmises tabelis on kokku võetud rolli õigused.

**Roll** | **Üksikasjad** | **Õigused**
--- | --- | ---
Azure_Site_Recovery roll | Vmware'i VM tuvastamine |V-Center server nende õiguste määramine<br/><br/>Andmesalve -> Eralda ruumi, Sirvi andmesalve, madala taseme faili toimingud., Eemalda faili, värskendamise virtuaalse masina failid<br/><br/>Võrgu -> võrgu määramine<br/><br/>Ressursi -> Määra virtual masina ressursivaru, virtuaalse masina lülitatud migreerimine, virtuaalse masina sisse lülitatud migreerimine<br/><br/>Tööülesannete -> Loo ülesanne, tööülesande värskendamine<br/><br/>Virtuaalse masina -> Konfiguratsioon<br/><br/>Virtuaalse masina -> Interact -> vastust küsimuse seadme ühendus., meediumi konfigureerimine CD-le, konfigureerimine floppy meedia, lülitage Power, installida VMware tööriistad<br/><br/>Virtuaalse masina -> varude -> Loo, Register registreerimise tühistamise<br/><br/>Virtuaalse masina -> Provisioning -> Luba virtual arvutisse alla laadida, luba virtuaalse masina failide üleslaadimine<br/><br/>Virtuaalse masina -> hetktõmmiseid -> Eemalda hetktõmmiseid
vCenter kasutajaroll | Rakenduse discovery Vmware'i VM Tõrkesiirde ilma allika VM sulgemine | V-Center server nende õiguste määramine<br/><br/>Andmekeskuse objekt -> Propagate lapse objektile, rolli = kirjutuskaitstud <br/><br/>Kasutajale on määratud andmekeskuse tasemel ja seega on juurdepääs kõigi objektide andmekeskuses.  Kui soovite piirata juurdepääsu, **Propagate laps** objekt **pole Accessi** rolli määramine lapse objektidele (ESX hosts, datastores, VMs ja võrke).
vCenter kasutajaroll | Tõrkesiirde ja failback | V-Center server nende õiguste määramine<br/><br/>Andmekeskuse objekti – Propagate lapse objektile, rolli = Azure_Site_Recovery<br/><br/>Kasutajale on määratud andmekeskuse tasemel ja seega on juurdepääs kõigi objektide andmekeskuses.  Kui soovite piirata juurdepääsu, koos **Propagate lapse objektile** **juurdepääsu pole **rolli määramine lapse objektile (ESX hosts, datastores, VMs ja võrke).  



## <a name="third-party-software-notices-and-information"></a>Kolmanda osapoole tarkvara teatised ja teave

Ärge tõlkimine või Localize

Tarkvara ja püsivara töötab Microsofti toote või teenuse põhineb või sisaldab materjali projektid, mis on loetletud allpool "(ühiselt kolmanda osapoole kood).  Microsoft on kolmanda osapoole koodiga mitte algse autori.  Algne Autoriõigus ja litsents, millise Microsoft saanud selliste muude tootjate kood, määratakse toodud allpool.

A jaotise teave on seotud kolmandate osapoolte kood komponentide projektid, mis on loetletud allpool. Need litsentsid ja teavet edastatakse illustreerivad.  Kolmanda osapoole järgmine kood on on relicensed teile Microsoft jaotises Microsofti tarkvara litsentsitingimused Microsofti toote või teenuse.  

Teave jaotises b on seotud kolmandate osapoolte kood komponendid, mis tehakse kättesaadavaks teile Microsofti hulgilitsentsimise algse tingimustel.

Terve faili võib leida [Microsofti allalaadimiskeskusest](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft jätab kõik õigused puudub kirjalik kinnitus, kas, kaudselt estoppel või muul viisil.

## <a name="next-steps"></a>Järgmised sammud

[Lisateave failback](site-recovery-failback-azure-to-vmware-classic.md) toob oma nurjunud üle, kus käitatakse Azure naasta kohapealse keskkonna.
