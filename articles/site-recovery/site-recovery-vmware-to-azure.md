<properties
    pageTitle="Paljundada VMware virtuaalmasinates ja füüsilise serveri Azure'i Azure saidi taastamine Azure'i portaalis | Microsoft Azure'i"
    description="Kirjeldab, kuidas Azure saidi taastamist korraldab dispersioonanalüüs, Tõrkesiirde ja kohapealse VMware virtuaalmasinates ja Azure portaalis Azure Windows/Linux füüsilise serveri juurutamine"
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
    ms.date="08/12/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-machines-to-azure-with-azure-site-recovery-using-the-azure-portal"></a>Paljundada VMware virtuaalmasinates ja füüsilise masinad Azure'i Azure saidi taastamine Azure'i portaalis

> [AZURE.SELECTOR]
- [Azure'i portaal](site-recovery-vmware-to-azure.md)
- [Azure'i klassikaline](site-recovery-vmware-to-azure-classic.md)
- [Azure'i klassikaline (pärand)](site-recovery-vmware-to-azure-classic-legacy.md)

Tere tulemast Azure'i saidi taastamine! Kasutage seda artiklit, kui soovite ise kohapealse VMware virtuaalmasinates või Windows/Linux füüsilise serveri Azure'i Azure portaalis Azure saidi taastamise abil.

> [AZURE.NOTE] Azure'i on kaks eri [juurutamise mudelite](../resource-manager-deployment-model.md) loomise ja ressursside töötamine: Azure'i ressursi Manager (ARM) ja klassikaline. Azure'i on kaks portaalide – Azure klassikaline portaali, mis toetab klassikaline juurutamise mudeli ja Azure portaali nii juurutamise mudelid tugi.

Saidi taastamine Azure'i portaalis pakub mitmesuguseid uusi funktsioone:

- Azure'i varundus ja Azure saidi taastamise teenuseid kombineeritakse ühe taastamise teenused vault nii, et saate häälestada ja hallata järjepidevuse ja avariitaastet (BCDR) ühest kohast. Ühendatud armatuurlaual saate jälgida ja haldamiseks toimingute oma kohapealse saitide ja Azure avaliku pilve.
- Azure'i tellimused ette valmistatud programmiga pilve lahenduse pakkuja (CSP) kasutajad saavad nüüd hallata saidi taastamise toimingute Azure'i portaalis.
- Saidi taastamine Azure'i portaalis saab paljundada masinad ARM salvestusruumi kontod. Veebisaidil Tõrkesiirde, loob saidi taastamine Azure'i ARM-põhised VMs.
- Klassikaline salvestusruumi kontode kopeerimine toetab jätkuvalt saidi taastamine. Veebisaidil Tõrkesiirde, loob saidi taastamine VMs klassikaline abil.

Pärast lugemist selle artikli postituse all Disqus kommentaaride kommentaarid. Tehniline küsimuste [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

## <a name="overview"></a>Ülevaade

Peavad organisatsioonid BCDR strateegia, mis määrab, kuidas rakendused, töökoormus ja andmete püsida töötab ja kättesaadav ajal plaanitud ja plaanimata tööseisakute ja taastamiseks tavaline tingimuste nii kiiresti kui võimalik. BCDR strateegia kavandamine peaks äriandmete turvalised ja saab hoida ja tagama töökoormus pidevalt katastroofi ilmnemisel.

Saidi taastamine on Azure'i teenus, mis aitab BCDR strateegia kavandamine, orkesteroinnin dispersioonanalüüs füüsilise kohapealsed serverid ja virtuaalmasinates pilveteenusesse (Azure) või teisene andmekeskusesse. Kui katkestuste esineda teie peamine asukoht, teil ei õnnestu üle teisene asukohta hoida rakendused ja töökoormus saadaval. Te ei tagasi oma peamine asukoht kui tagastab see toiminguid. Lisateavet leiate [mis on Azure saidi taastamise?](site-recovery-overview.md)

Selles artiklis antakse kogu teavet, mida soovite korrata kohapealse VMware VMs ja Windows/Linux Azure'i füüsiliste serverite. See sisaldab ülevaate arhitektuuri, teavet ja juurutamise juhised konfigureerida Azure, kohapealsed serverid, dispersioonanalüüs sätted ja võimsuse planeerimine kavandamine. Kui olete häälestanud taristu, saate lubada dispersioonanalüüs arvutites, mida soovite kaitsta ja kontrollida selle Tõrkesiirde töötab.

## <a name="business-advantages"></a>Eelised ettevõtetele

- Saidi taastamine pakub väljapoole kaitse business töökoormus ja töötavad VMware VMs ja füüsilise serveri rakendused.
- Taastamise teenused portaalis häälestada, hallata ja jälgida dispersioonanalüüs, Tõrkesiirde ja taastamine ühes kohas.
- Saidi taastamine saaksid automaatselt tuvastada VMware VMs lisatakse vSphere hosts.
- Saate hõlpsalt käivitada failovers oma kohapealse taristu Azure ja migreerite (Taasta) Azure'i Vmware'i VM serverid kohapealse saidi.
- Saate lubada mitme-VM ja dispersioonanalüüs rühmade loomine, et rakenduse töökoormus mitmetasandilise üle mitme masinad juhendist samal ajal. Kõik masinad kopeerimine jaotises on krahh ühtsete ja rakenduse ühtsete taastamise punkte, kui nad ei õnnestu üle. Tõrkesiirde, võite taastamise koguda mitme masinad nii, et astmeline rakenduse töökoormus ei õnnestu üle koos.

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

Need on stsenaarium komponendid.

- **Konfiguratsiooniserveri**: kohapealse arvutisse, mis koordinaadid side ja haldab andmete kopeerimise ja taastamise abil. Selles arvutis käivitate ühe installifail konfiguratsiooniserveri ja nende lisakomponentide installimiseks:
    - **Protsessi server**: dispersioonanalüüs lüüsi toimib. Saab dispersioonanalüüs kaitstud allika masinad, optimeerib vahemällu, tihendamise ja krüptimise ja saadab Azure salvestusruumi. Lisaks tegeleb tõuketeatised installi Mobility teenuse kaitstud masinad ja sooritab VMware VMs automaatne tuvastamine. Vaikimisi protsessi server on installitud konfigureerimine serveris. Saate juurutada täiendavad eraldi protsessi serverid mastaapimiseks juurutamise.
    - **Juhtlehed target server**: tegeleb dispersioonanalüüs andmete migreerite Azure'i ajal.

- **Mobiilsus teenuse**: see osa juurutatakse iga seadme (Vmware'i VM või füüsiline server), mida soovite korrata Azure. See sisaldab andmeid kirjutab arvutis ja edastab need protsessi serveriga.
- **Azure'i**: teil pole vaja luua mis tahes Azure VMs kopeerimise ja Azure Tõrkesiirde.  Peate Azure tellimuse talletamiseks kopeeritud andmed ja on Azure virtuaalse võrgu nii, et Azure VMs on pärast Tõrkesiirde võrku ühendatud konto Azure salvestusruumi. Salvestusruumi konto ja võrgu peab olema sama piirkonna vault taastamise teenused.
- **Failback**: kui olete valmis oma kohapealse saidi pärast Tõrkesiirde tagasi: Azure'i nurjuda, peate mõne Azure VM ajutine protsessi serveriga ühiseid loomiseks. Saate kustutada selle pärast failback on lõpule viidud. Jaoks failback, peate samuti VPN (või Azure'i ExpressRoute) ühenduse oma kohapealse saidi ja Azure võrgu, kus asuvad teie Azure VMs vahel. Kui failback liiklus on raske peate ka häälestada sihtotstarbeline juhtslaidi target serveri kohapealse kohapeal. Heledam liikluse saab kasutada vaikimisi serveri konfigureerimine serveris.


Pilti näitab, kuidas kasutada nende komponendid.

![arhitektuur](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Joonisel 1: VMware füüsiline Azure**

## <a name="azure-prerequisites"></a>Azure'i eeltingimused

Siin on, mida peate Azure juurutamine seda stsenaariumi.

**Nõutav** | **Üksikasjad**
--- | ---
**Azure'i konto**| Peate [Microsoft Azure'i](http://azure.microsoft.com/) kontosse. Saate alustada [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). [Lisateavet leiate teemast](https://azure.microsoft.com/pricing/details/site-recovery/) saidi taastamine hindade kohta.
**Azure'i salvestusruum** | Kopeeritud andmed on talletatud Azure salvestusruumi ja Azure VMs luuakse Tõrkesiirde korral. <br/><br/>Andmete talletamiseks peate standard- või premium salvestusruumi konto taastamise teenused vault sama piirkonna.<br/><br/>Saate kasutada LRS või GRS salvestusruumi konto. Soovitame GRS nii, et andmed on olles piirkondliku katkestuste ilmnemisel või kui esmane piirkond ei saa taastada. [Lisateavet leiate teemast](../storage/storage-redundancy.md).<br/><br/> Tavaliselt kasutatakse [Premium salvestusruumi](../storage/storage-premium-storage.md) virtuaalmasinates, mis peab olema püsivalt kõrge IO jõudlus ja madal latentsus host IO intensiivse töökoormus abil.<br/><br/> Kui soovite kopeeritud andmete talletamiseks premium konto abil, peate samuti kindlad konto dispersioonanalüüs logid, mis jäädvustada poolelioleva muudatused – kohapealsete andmete talletamiseks.<br/><br/> Pange tähele, et salvestusruumi kontod Azure portaali loodud ei saa teisaldada, ressursside rühma. Ka kaitse premium salvestusruumi kontod India Kesk-ja Lõuna India pole praegu toetatud.<br/><br/> [Lugege](../storage/storage-introduction.md) Azure'i salvestusruumi.
**Azure'i võrgu** | Peate Azure virtuaalse võrgu, mis Azure VMs loob ühenduse Tõrkesiirde korral. Azure virtuaalse võrgu peab olema sama piirkonna vault taastamise teenused.
**Azure'i migreerite** | Peate ajutiselt protsessi server Azure'i VM on seadistatud. See saate luua, kui olete valmis nurjuda tagasi ja kustutage see, kui fail uuesti on lõpule jõudnud.<br/><br/> Nurjumise tagasi peate VPN-ühendus (või Azure'i ExpressRoute) Azure võrgust kohapealse saidile.

## <a name="configuration-server--scale-out-process-prerequisites"></a>Konfiguratsiooniserveri / ulatus välja protsessi eeltingimused

Te saate häälestada mõne kohapealse arvutisse kui konfiguratsiooniserveri / skaala-out protsess server.

**Nõutav** | **Üksikasjad**
--- | ---
**Konfiguratsiooniserveri**| Vajate mõne kohapealse füüsilise või töötab Windows Server 2012 R2. Kõik komponendid kohapealse saidi taastamine sellesse arvutisse installitud.<br/><br/>Vmware'i VM dispersioonanalüüs, soovitame juurutate serveri tugevalt saadaval Vmware'i VM. Kui te olete imitatsiooniga füüsilise masinad seejärel seade võib olla füüsiline server.<br/><br/> Kohapealse saidi migreerite Azure on alati VMware vms sõltumata sellest, kas te ei ole üle VMs või füüsilise serveri. Kui juurutate konfiguratsiooniserveri nimega Vmware'i VM peate häälestamine on eraldi serveri nimega Vmware'i VM failback liikluse vastuvõtmiseks.<br/><br/>Kui server kuulub Vmware'i VM, tuleks võrgu adapterit tüübi VMXNET3. Kui kasutate mõnda muud tüüpi võrguadapteri peate installimiseks [värskendada VMware](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1) vSphere 5,5 serveris.<br/><br/>Server peaks olema staatiline IP-aadress.<br/><br/>Server ei tohiks domeenikontrolleri.<br/><br/>Hosti nimi serveri peaks olema 15 märki või vähem.<br/><br/>Operatsioonisüsteem tuleks ainult inglise keele.<br/><br/> Peate installima VMware vSphere PowerCLI 6.0. konfigureerimine serveris.<br/><br/>Konfiguratsiooniserveri peab Interneti-ühendus. Väljamineva juurdepääsu on vaja järgmiselt:<br/><br/>Ajutiste juurdepääsu HTTP 80 saidi taastamine komponendid (MySQL-i allalaadimiseks) häälestamise ajal<br/><br/>Poolelioleva väljaminev HTTPS 443 dispersioonanalüüs haldamiseks juurdepääsu<br/><br/>Poolelioleva väljamineva juurdepääsu HTTPS 9443 dispersioonanalüüs liikluse (saate muuta selle pordi)<br/><br/>Server on vaja ka juurdepääsu järgmised URL-id, et see saab ühenduda Azure: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.Windows.net; *. backup.windowsazure.com; *. blob.Core.Windows.net; *. store.core.windows.net<br/><br/>Kui teil on serveri IP-aadress-põhiste tulemüüri reeglid, veenduge, et reegleid andnud Azure'i teatis. Peate lubama [Azure'i andmekeskuse IP-vahemikke](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja (443) HTTPS-protokolli.<br/><br/>Luba IP-aadresside vahemikud tellimuse Azure piirkond ja Lääne US.<br/><br/>Luba seda URL-i MySQL-i allalaadimine:.http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi


## <a name="vmware-vcentervsphere-host-prerequisites"></a>VMware vCenter/vSphere host eeltingimused

**Nõutav** | **Üksikasjad**
--- | ---
**vSphere**| Teil on vaja ühe või mitme VMware vSphere hypervisors.<br/><br/>Hypervisors peaks töötama vSphere versiooni 6.0, 5,5 või 5.1 uusimaid värskendusi.<br/><br/>Soovitame oma vSphere majutajate ja vCenter serverid asuvad samasse võrku protsessi server (see olla võrgu, kus konfiguratsiooniserveri asub juhul, kui olete häälestanud sihtotstarbeline protsessi server).
**vCenter** | Soovitame juurutada VMware vCenter server hallata oma vSphere hosts. See peaks töötama vCenter versioon 6.0 või 5,5 uusimaid värskendusi.<br/><br/>Pange tähele, et saidi taastamine ei toeta uue vCenter vSphere 6.0 funktsioone nagu cross vCenter vMotion, virtuaalne mahud ja salvestusruumi DRS. Saidi taastamise tugi on piiratud funktsioonid, mis olid ka versioonis 5,5 saadaval.


## <a name="protected-machine-prerequisites"></a>Kaitstud arvuti eeltingimused

**Nõutav** | **Üksikasjad**
--- | ---
**Kohapealse (VMware VMs)** | VMware VMs, mida soovite kaitsta peaks olema installitud ja töötab VMware tööriistad.<br/><br/> Masinad, mida soovite kaitsta peavad vastama [Azure eeltingimused](site-recovery-best-practices.md#azure-virtual-machine-requirements) Azure VMs loomise kohta.<br/><br/>Üksikute ketta maht kaitstud masinad ei tohiks üle 1023 GB. VM võib olla kuni 64 ketast (seega kuni 64 TB). <br/><br/>Minimaalne 2 GB vaba ruumi komponendi installimiseks kettal installi.<br/><br/>Kaitse VMs krüptitud ketast pole toetatud.<br/><br/>Ühiskasutuses ketta Külastajate kogumite ei toetata.<br/><br/>**Pordi 20004** tuleks avada kaitstud virtuaalse masina kohaliku tulemüüri, kui soovite lubada **rakenduse järjepidevuse**.<br/><br/>Mis on ühendatud laiendatav püsivara Interface (UEFI) / laiendatav püsivara Interface(EFI) buutimine ei toetata.<br/><br/>Arvuti nimed peaks olema vahemikus 1 kuni 63 märki (tähti, numbreid ja sidekriipse). Nimi peab tähe või numbri alguse ja lõpu tähe või numbri. Kui olete lubanud dispersioonanalüüs mõne seadme jaoks saate muuta Azure nimi.<br/><br/>Kui allika VM on NIC lubatud teisendatakse see pärast Tõrkesiirde Azure ühe NIC.<br/><br/>Kui kaitstud virtuaalmasinates on iSCSI kettalt klõpsake saidi taastamine teisendab kaitstud VM iSCSI ketas VHD faili kui VM ei üle Azure. Kui Azure VM pääseb iSCSI target seejärel see ühenduse loomiseks ja põhiosas leiate kaks plaati – VHD ketas, Azure VM ja allikas iSCSI ketas. Sel juhul peate katkestama iSCSI target, mis kuvatakse Azure VM.
**Windowsi masinad (füüsilise või VMware)** | Seadme töötama toetatud 64-bitine opsüsteem: Windows Server 2012 R2, Windows Server 2012 või Windows Server 2008 R2 hoolduspaketiga veebisaidil vähemalt SP1.<br/><br/> Operatsioonisüsteem peaks olema installitud kettadraivi C:\. Ketas OS peaks olema Windowsi lihtsa jaoks ja mitte dünaamiline. Andmed kettale võib olla dünaamiline.<br/><br/>Saidi taastamine toetab VMs on RDM ketas. Ajal failback, saidi taastamine taaskasutamine RDM ketas, kui algne allikas VM ja RDM ketas on saadaval. Kui nad pole saadaval, loob saidi taastamise ajal failback iga ketta VMDK uue faili.
**Linux masinad** (phyical või VMware)|  Peate toetatud 64-bitine operatsioonisüsteem: punane rolli Enterprise Linux 6.7,7.1,7.2; CentOS 6.5, 6.6,6.7,7.0,7.1,7.2; Oracle'i Enterprise Linux 6.4 6.5 operatsioonisüsteemiga punane rolli ühilduvad tuum või purunematu Enterprise tuum väljaanne 3 (UEK3) SUSE Linux Enterprise Server 11 SP3.<br/><br/>muutsite failid kaitstud masinad peaks sisaldama kirjed, mis kohaliku hosti nimi vastendada kõik võrguadapteri seotud IP-aadressid.<br/><br/>Kui soovite ühendada Azure virtuaalse masina, mis töötab Linux pärast Tõrkesiirde Secure Shell kliendi (ssh), tagada teenuse Secure Shell kaitstud arvutis on määratud käivitamine automaatselt süsteemi käivitamine ja, et lubada tulemüüri reeglid on ssh sellega ühendus.<br/><br/>Hosti nimi, haakepunktide, seadme nimed, ja Linux süsteemi teed ja faili nimi (nt/jne; /usr) tuleks ingliskeelsena ainult.<br/><br/>Kaitse saab lubada ainult Linux masinad järgmised salvestusruumi: Hajusfailisüsteemiga (EXT3, ETX4, ReiserFS, XFS); Silma tarkvara-seadme plaanuri (silma)); Helitugevuse halduri: (LVM2). HP CCISS kontrolleril salvestusruumi füüsilise serverite ei toetata. ReiserFS failisüsteemi toetatakse ainult SUSE Linux Enterprise Server 11 SP3.<br/><br/>Saidi taastamine toetab VMs on RDM ketas.  Ajal failback Linuxi, pole saidi taastamine RDM ketas korduvalt kasutada. Selle asemel loob uue VMDK faili iga vastav RDM ketta.<br/><br/>Veenduge, et teie määratud disk.enableUUID=true säte konfiguratsiooni parameetrid VMware VM. Looge kirje, kui see pole olemas. Vaja on VMDK ühtsete valitud kasutajatunnust anda, et alused õigesti. Selle sätte lisamine tagab, et ainult delta muudatused kantakse üle naasta kohapealse failback ja pole täielik koopia.
**Mobiilsus teenuse** |  **Windowsi**: saate automaatselt suunata mobiilsus teenuse vms teil on vaja administraatorikonto (kohaliku administraatori Windowsi arvutisse) nii, et protsessi serveri teha tõuketeatised installimine Windowsi opsüsteemi.<br/><br/>**Linux**: automaatselt suunata mobiilsus teenuse vms töötab Linux peate teha tõuketeatised installimise protsessi server kasutatavate konto loomiseks.<br/><br/> Vaikimisi on kõik ketast, arvutisse kopeeritud. [Välistamine ketas, kopeerimine](#exclude-disks-from-replication), mobiilsus teenuse peab olema installitud käsitsi seadmesse enne saate lubada kopeerimine.<br/>

## <a name="prepare-for-deployment"></a>Juurutamise ettevalmistamine

Ettevalmistamiseks juurutamiseks peate:

1. [Azure'i võrgu määramine](#set-up-an-azure-network) , kus Azure VMs paigutatakse kui need on kedratud pärast Tõrkesiirde. Lisaks failback jaoks peate VPN-ühendus (või Azure'i ExpressRoute) häälestamiseks Azure võrgu kaudu oma kohapealse saidi.
2. Kopeeritud andmed [Azure storage konto häälestamine](#set-up-an-azure-storage-account) .
3. [Ettevalmistamine konto](#prepare-an-account-for-automatic-discovery) vCenter server või vSphere majutab nii, et saidi taastamine saab VMware VMs, mis lisatakse automaatselt tuvastada.
4. [Ettevalmistamine konfiguratsiooniserveri](#prepare-the-configuration-server) tagamaks, et see pääsete juurde vajalik URL-id ja installige vSphere PowerCLI 6.0.


### <a name="set-up-an-azure-network"></a>Azure'i võrgu

- Võrgu peaks olema sama Azure piirkonna, kus kuvatakse juurutamist vault taastamise teenused.
- Mudelist ressursi soovite kasutada üle Azure VMs nurjus, saate häälestada Azure võrgu [klassikalise režiimi](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)või [ARM](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .
- Tagasi: Azure'i nurjuda saidile kohapealse VMware peate VPN-ühendus (või Azure'i ExpressRoute ühenduse), kus tiražeeritud Azure VMs asuvad, kus asub konfiguratsiooniserveri kohapealse võrgu Azure võrgust.
- [Vaadake, kuidas](../vpn-gateway/vpn-gateway-site-to-site-create.md) toetatud juurutamise mudelid VPN-saidilt ühendused, ja kuidas [luua ühendus](../vpn-gateway/vpn-gateway-site-to-site-create.md#create-your-virtual-network).
- Teise võimalusena saate häälestada [Azure'i ExpressRoute](../expressroute/expressroute-introduction.md). [Lisateavet leiate teemast](../expressroute/expressroute-howto-vnet-portal-classic.md) Azure võrgu koos ExpressRoute seadmise kohta.

> [AZURE.NOTE] [Võrgu migreerimine](../resource-group-move-resources.md) üle ühe tellimuse ressursi rühma või tellimustes võrke kasutatakse juurutamine saidi taastamine ei toetata.

### <a name="set-up-an-azure-storage-account"></a>Azure'i salvestusruumi konto häälestamine

- Peate standard või premium Azure storage konto Azure kopeeritud andmete mahutamiseks. Konto peab olema sama piirkonna vault taastamise teenused. Mudelist ressursi soovite kasutada üle Azure VMs nurjus, saate häälestada konto [klassikalise režiimi](../storage/storage-create-storage-account-classic-portal.md)või [ARM](../storage/storage-create-storage-account.md) .
- Kui kasutate premium konto kopeeritud andmed, peate looma täiendava standard konto dispersioonanalüüs logid, mis jäädvustada poolelioleva muudatused – kohapealsete andmete talletamiseks.  

> [AZURE.NOTE] [Migreerimise salvestusruumi kontode](../resource-group-move-resources.md) üle ühe tellimuse ressursi rühma või tellimustes salvestusruumi kontod kasutatakse juurutamine saidi taastamine ei toetata.

### <a name="prepare-an-account-for-automatic-discovery"></a>Automaatse tuvastamise konto ettevalmistamine

Saidi taastamise protsess server saaksid automaatselt tuvastada VMware VMs vSphere hosts või vCenter serveris, mida haldab hosts. Täita automaatse discovery saidi taastamine mandaat, et pääsete juurde VMware server. See pole oluline, kui te olete imitatsiooniga füüsilise masinad ainult.

1. Kasutada sihtotstarbeline konto automaatse tuvastamise jaoks [vajalike õiguste](#vmware-account-permissions)tasemele vCenter loomine rolli (nt Azure_Site_Recovery).
2. Luua uus kasutaja vSphere hosti või vCenter server ja kasutajale rolli määramine. Hiljem saate sellest teada neid mandaate, et seda saab teha automaatse tuvastamise saidi taastamine.

    >[AZURE.NOTE] Kirjutuskaitstud rolli vCenter kasutajakonto käitamise Tõrkesiirde, kuid ei saa kaitstud allika masinad sulgeda. Kui soovite need masinad sulgemiseks peate [Azure_Site_Recovery](#vmware-account-permissions) roll. Kui kasutate ainult migreerimine VMs VMware Azure ja ei pea failback on piisavalt kirjutuskaitstud roll.

### <a name="prepare-the-configuration-server"></a>Konfiguratsiooniserveri ettevalmistamine

1.  Veenduge, et kasutate konfiguratsiooniserveri arvuti vastab [eeltingimused](#configuration-server-prerequisites). Eelkõige veenduge, et seade on ühendatud Interneti-ühendus nende sätetega:

    - Luba juurdepääs idest: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.Windows.net; *. backup.windowsazure.com; *. blob.Core.Windows.net; *. store.core.windows.net
    - Luba juurdepääs [http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi](http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi) MySQL-i alla laadida.
    - Luba tulemüüri side Azure ja [Azure andmekeskuse IP-aadresside vahemikud](https://www.microsoft.com/download/confirmation.aspx?id=41653) (443) HTTPS-protokolli.

2.  Laadige alla ja installige [VMware vSphere PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) konfiguratsiooniserveri. (Praegu PowerCLI muid versioone ei toetata, sh R versioonides versioon 6.0.)


## <a name="create-a-recovery-services-vault"></a>Taastamise teenused vault loomine

1. [Azure'i portaali](https://portal.azure.com)sisse logida.
2. Klõpsake nuppu **Uus** > **halduse** > **varundamine ja taastamine saidi (OMS)**. Teise võimalusena võite klõpsata **sirvida** > **Taastamise teenuste hoidla** > **lisamine**.

    ![Uue vault](./media/site-recovery-vmware-to-azure/new-vault3.png)

3. Määrake **nimi** sõbralik nimi, mis tähistavad vault. Kui teil on mitu tellimust, valige üks neist.
4. [Uue ressursirühma Loo](../resource-group-template-deploy-portal.md) või valige olemasoleva. Määrake Azure piirkond. Masinad on kopeeritud selle alale. Pange tähele, et Azure'i salvestusruumi ja võrgu kasutada saidi taastamine tuleb piirkonna. Märkige ruut toetatud regioonide leiate [Azure'i saidi taastamise hinnad üksikasjad](https://azure.microsoft.com/pricing/details/site-recovery/) geograafilised saadavus
4. Kui soovite kiiresti juurde vault armatuurlaualt klõpsake käsku **Kinnita armatuurlaua** ja seejärel klõpsake nuppu **Loo**.

    ![Uue vault](./media/site-recovery-vmware-to-azure/new-vault-settings.png)

Kuvatakse uus vault **armatuurlaua** > **kõik ressursid**, ja peamine **taastamise teenused võlvid** tera.

## <a name="getting-started"></a>Alustamine

Saidi taastamine pakub alustamine kogemusi, juhendit loodud ja töötab nii kiiresti kui võimalik. See kontrollib eeltingimused ja juhendab teid peate hankima saidi taastamine juurutatud juhiseid.

Valite tüüp masinad, mida soovite kopeerida, ja kuhu soovite korrata. Saate häälestada infrastruktuur, sh kohapealsed serverid, Azure sätted, dispersioonanalüüs poliitikate ja võimsuse planeerimine. Kui teie taristu on kohas lubate dispersioonanalüüs VMs ja füüsilise serveri. Saate käivitada failovers teatud masinad või luua taastamine ei õnnestu üle mitme masinad.

Alustage alustamine, valides, kuidas soovite juurutada saidi taastamine. Alustamine voogu muudab veidi vastavalt oma vajadustele kopeerimine.


## <a name="step-1-choose-your-protection-goals"></a>Samm 1: Valige kaitse eesmärgid.

Valige mida soovite korrata ja kuhu soovite korrata.

1. Valige oma vault **taastamise teenused võlvid** tera ja klõpsake nuppu **sätted**.
2. **Sätted** > **Alustamine** nuppu **Saidi taastamine** > **Samm 1: ettevalmistamine infrastruktuur** > **kaitse eesmärk**.

    ![Valige eesmärgid](./media/site-recovery-vmware-to-azure/choose-goals.png)

3. Valige **Abil Azure'i** **kaitse eesmärk** ja valige **Jah, mille VMware vSphere Hypervisor**. Klõpsake nuppu **OK**.

    ![Valige eesmärgid](./media/site-recovery-vmware-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Samm 2: Allika keskkonna häälestamine

Häälestage konfiguratsiooniserveri ja taastamise teenused võlvkelder registreerida. Kui te olete imitatsiooniga VMware VMs VMware kontot kasutate automaatse tuvastamise määrata.

1. Klõpsake **Samm 1: ettevalmistamine taristu** > **Allikas**. **Ettevalmistamine allika** kui teil pole serveri konfiguratsioon nuppu **+ konfiguratsiooniserveri** lisada.

    ![Allika häälestamine](./media/site-recovery-vmware-to-azure/set-source1.png)

2. **Lisage serveri** tera sisse **Konfiguratsiooniserveri** kuvatakse **Server tüüp**.
3. Enne häälestamist konfiguratsiooniserveri kinnitamine [eeltingimused](#configuration-server-prerequisites). Klõpsake kindla sisse, et seade on juurdepääs vajalik URL-id.
4.  Saidi taastamise ühendatud Setup installi faili alla laadida.
5.  Laadige alla vault registreerimise võti. Peate selle ühendatud Setup käivitamisel. Võti kehtib 5 päeva pärast selle loomist.

    ![Allika häälestamine](./media/site-recovery-vmware-to-azure/set-source2.png)

6.  Kasutate konfiguratsiooniserveri arvutis ühendatud installimiseks käivitage installiprogramm konfiguratsiooniserveri, protsessi server ja serveri.


### <a name="run-site-recovery-unified-setup"></a>Käivitage saidi taastamise ühendatud häälestamine

1.  Käivitage installifail ühendatud Setup.
2.  **Enne alustamist** valige **konfiguratsiooniserveri ja protsessi serveri installimine**.

    ![Enne alustamist](./media/site-recovery-vmware-to-azure/combined-wiz1.png)

3. **Kolmanda osapoole tarkvara litsentsi** nuppu **nõustun** alla laadima ja installima MySQL-i.

    ![= Kolmanda osapoole tarkvara](./media/site-recovery-vmware-to-azure/combined-wiz105.PNG)

4. **Registreerimise** Sirvi ja valige soovitud hoidlast allalaaditud registreerimise võti.

    ![Registreerimine](./media/site-recovery-vmware-to-azure/combined-wiz3.png)

5. **Interneti-sätted** määrata, kuidas pakkuja konfigureerimine serveris töötav teenus loob ühenduse Azure saidi taastamise Interneti kaudu.

    - Kui soovite luua ühenduse puhverserveri, mis on seadistatud seadmesse valige **Loo ühendus olemasoleva puhverserveri sätted**.
    - Kui soovite ühendada otse pakkuja valige **ühendus otse ilma proxy**.
    - Kui olemasolev puhverserver nõuab autentimist või soovite kasutada kohandatud puhverserveri ühenduse pakkuja, valige **Loo ühendus kohandatud puhverserveri sätted**.
        - Kui kasutate kohandatud puhverserveri aadressi, portide ja mandaadi määramiseks peate
        - Kui kasutate teil peaks juba lubatud URL-ide kirjeldatud [eeltingimused](#configuration-server-prerequisites)proxy.

    ![Tulemüür](./media/site-recovery-vmware-to-azure/combined-wiz4.png)

6. **Eeltingimused, märkige ruut** häälestamise töötab sisse, veenduge, et saate käivitada installimise. Kui kuvatakse hoiatus kohta **globaalne kellaaja sünkroonimine märkige** veenduge, et see kellaaeg süsteemikell (**kuupäev ja kellaaeg** sätted) on sama, mis ajavöönd.

    ![Eeltingimused](./media/site-recovery-vmware-to-azure/combined-wiz5.png)

7. **MySQL-i** konfiguratsiooni loomine MySQL-i serveri eksemplar, mis installitakse logimiseks mandaat.

    ![MySQL-i](./media/site-recovery-vmware-to-azure/combined-wiz6.png)

8. **Keskkonna üksikasjad** , valige kas kavatsete VMware VMs korrata. Kui olete, siis Installiprogramm kontrollib, kas PowerCLI 6.0 on installitud.

    ![MySQL-i](./media/site-recovery-vmware-to-azure/combined-wiz7.png)

9. **Installige asukoht** kuhu soovite installida kahendfaile ja vahemällu talletamine, valige. Valige ketas, millel on vähemalt 5 GB salvestusruumi, kuid soovitame vahemälu ketas vähemalt 600 GB vaba ruumi.

    ![Installi asukoht](./media/site-recovery-vmware-to-azure/combined-wiz8.png)

10. Määrake **Võrgu valimine** kuulajale (võrguadapteri ja SSL port) aluseks on konfiguratsiooniserveri saatmine ja vastuvõtmine dispersioonanalüüs andmed. Saate muuta vaikimisi port (9443). Lisaks selle pordi pordi 443 kasutab orchestrates dispersioonanalüüs toimingute veebiserverisse. 443 ei tohiks kasutada kopeerimise liikluse vastu.


    ![Võrgu valimine](./media/site-recovery-vmware-to-azure/combined-wiz9.png)



11.  **Kokkuvõttes** teave üle ja klõpsake nuppu **Installi**. Pärast installi lõppemist on loodud parool. Peate selle dispersioonanalüüs lubamisel nii kopeerige see ja hoidke seda turvalises asukohas.

    ![Kokkuvõte](./media/site-recovery-vmware-to-azure/combined-wiz10.png)

12.  Kui registreerimine on lõpule viidud serveri **sätted**kuvatakse > **serverid** blade autoriloomingut.



#### <a name="run-setup-from-the-command-line"></a>Installiprogramm käsurea kaudu

Saate häälestada konfiguratsiooniserveri käsurea kaudu:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Parameetrid:

- / ServerMode: kohustuslik. Saate määrata, kas olema installitud nii konfiguratsiooni ja protsessi serverid või ainult server protsess. Sisestage väärtused: CS, PS.
- InstallLocation: kohustuslik. Kaust, kuhu on installitud komponendid.
- / MySQLCredsFilePath. Kohustuslik. Faili tee, kus talletatakse MySQL-i serveri identimisteavet. Fail peaks olema järgmises vormingus:
    - [MySQLCredentials]
    - MySQLRootPassword = "<Password>"
    - MySQLUserPassword = "<Password>"
- / VaultCredsFilePath. Kohustuslik. Vault identimisteabe faili asukoht
- / EnvType. Kohustuslik. Installi tüüp. Väärtused: VMware, NonVMware
- / PSIP ja /CSIP. Kohustuslik. IP-aadress ja protsessi serveri konfiguratsiooniserveri.
- / PassphraseFilePath. Kohustuslik. Parooli faili asukoht.
- / BypassProxy. Valikuline. Määrab konfiguratsiooniserveri ühendub Azure'i ilma proxy.
- / ProxySettingsFilePath. Valikuline. Puhverserveri sätete (vaikimisi puhverserver nõuab autentimist või kohandatud puhverserveri). Fail peaks olema järgmises vormingus:
    - [ProxySettings]
    - ProxyAuthentication = "Jah/ei"
    - Puhverserveri IP = "IP-aadress >"
    - ProxyPort = "<Port>"
    - ProxyUserName = "<User Name>"
    - ProxyPassword = "<Password>"
- DataTransferSecurePort. Valikuline. Pordi number dispersioonanalüüs andmete jaoks kasutada.
- SkipSpaceCheck. Valikuline. Vahele ruumi Otsi vahemälu.
- AcceptThirdpartyEULA. Kohustuslik. Lipp tähendab kolmanda osapoole EULA.
- ShowThirdpartyEULA. Kohustuslik. Kuvab kolmanda osapoole EULA. Kui sisendina ignoreeritakse kõiki muid parameetreid.

### <a name="add-the-vmware-account-used-for-automatic-discovery"></a>Automaatse tuvastamise kasutatav VMware konto lisamine

 Kui olete valmis juurutamiseks peaks teil olema [loodud VMware konto](#prepare-an-account-for-automatic-discovery) automaatse tuvastamise jaoks saidi taastamine kasutavad. Lisage selle konto järgmiselt:

1. Avage **CSPSConfigtool.exe**. See on saadaval töölaua otsetee ja asub kaustas [installida asukoht] \home\svsystems\bin.
2. Klõpsake nuppu **Halda kontosid** > **Lisa konto**.

    ![Konto lisamine](./media/site-recovery-vmware-to-azure/credentials1.png)

3. Lisage konto, mida kasutatakse automaatse tuvastamise **Konto üksikasjad** . Pange tähele, et võib kuluda 15 minutit või rohkem kuvada portaali konto nimi. Kohe värskendamiseks klõpsake **Konfiguratsiooni serverid** > Serveri nimi > **Serveri värskendamine**.

    ![Üksikasjad](./media/site-recovery-vmware-to-azure/credentials2.png)

### <a name="connect-to-vsphere-hosts-and-vcenter-servers"></a>Ühenduse loomine vSphere majutajate ja vCenter serverid

Kui te olete imitatsiooniga VMware VMs ühenduse vSphere majutajate ja vCenter serverites.

1. Veenduge, et serveri konfiguratsioon on võrgupääs vSphere hosts ja vCenter serverites.
2. Klõpsake **ettevalmistamine taristu** > **Allikas**. Valige konfiguratsiooniserveri **ettevalmistamine allikas** ja nuppu **+ vCenter** lisada vSphere hosti või vCenter server.
3. Määrake **Lisa vCenter** vSphere hosti või vCenter server sõbralik nimi ja määrake IP-aadress või FQDN serveri. Jäta pordi 443 juhul, kui teie VMware serverid on konfigureeritud kuulata taotlusi porti. Valige konto, mida kasutatakse VMware serveriga ühendust luua. Klõpsake nuppu **OK**.

    ![VMware](./media/site-recovery-vmware-to-azure/vmware-server.png)

    >[AZURE.NOTE] Kui lisate vCenter server või vSphere host kontoga, millel pole serveris vCenter või host administraatori õigusi, siis veenduge, et et konto on need õigused, mis on lubatud: andmekeskuse, andmesalve, kausta, Host, võrku, ressursside, virtuaalse masina vSphere jaotatud vahetamine. Lisaks vCenter server vajab salvestusruumi vaadete au.

Saidi taastamine loob ühenduse VMware serverite abil teie määratud ja VMs leiab sätted.

## <a name="step-3-set-up-the-target-environment"></a>Samm 3: Target keskkonna häälestamine

Veenduge, et teil on kopeerimise ja muu Azure kaudu, millele Azure VMs ühendab pärast Tõrkesiirde salvestusruumi konto.

1.  Klõpsake **ettevalmistamine taristu** > **Target** ja valige soovitud Azure'i tellimus, mida soovite kasutada.
2.  Määrata, mida soovite kasutada vms pärast Tõrkesiirde juurutamise mudeli.
3.  Saidi taastamine kontrollib, kas teil on ühe või mitme ühilduvad Azure salvestusruumi kontod ja võrgu.

    ![Target (sihtkoht)](./media/site-recovery-vmware-to-azure/gs-target.png)

4.  Kui te pole loonud salvestusruumi konto ja te soovite luua kasutamise ARM nuppu **+ salvestusruumi konto** teha seda teksti sees.  Määrake **Loo salvestusruumi konto** enne konto nimi, tüüp, tellimus ja asukoht. Konto peaks olema sama piirkonna vault taastamise teenused.

    ![Salvestusruumi](./media/site-recovery-vmware-to-azure/gs-createstorage.png)

    Pange tähele järgmist.

    - Kui soovite luua salvestusruumi konto abil klassikaline saate seda teha Azure'i portaalis. [Lisateave](../storage/storage-create-storage-account-classic-portal.md)
    - Kui kasutate premium salvestusruumi konto kopeeritud andmed peate standard salvestusruumi konto häälestamine logib poe kopeerimine selle jäädvustada poolelioleva tehtud muudatused kohapealsed andmed.

    > [AZURE.NOTE] Kaitse premium salvestusruumi kontod India Kesk-ja Lõuna India pole praegu toetatud.

4.  Valige muu Azure kaudu. Kui te pole loonud võrgus ja soovite seda teha kasutamise ARM nuppu **+ võrgu** teha seda teksti sees. **Loo virtuaalse võrgu** enne määrata võrgu nimi, aadress vahemikus, alamvõrgu üksikasjad, tellimus ja asukoht. Võrgu peaks olema taastamise teenused vault samasse asukohta.

    ![Võrgu](./media/site-recovery-vmware-to-azure/gs-createnetwork.png)

    Kui soovite luua klassikaline kasutades saate seda teha Azure'i portaalis. [Lisateavet leiate teemast](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

## <a name="step-4-set-up-replication-settings"></a>Samm 4: Häälestamine dispersioonanalüüs sätted

1. Uue koopia loomiseks klõpsake poliitika **ettevalmistamine taristu** > **Dispersioonanalüüs sätted** > **+ loomine ja seostada**.
2. **Loomine ja sidusettevõtte poliitika** määrata poliitika nimi.
3. **RPO**läve: määrake RPO limiit. Kui pidev dispersioonanalüüs lubatust loob teatised.
5. **Taastamine osutage säilitamine**, määrake mõne tunni pärast kaua säilituspoliitika akna saab iga taastamine punkti. Kaitstud masinad taastada mis tahes punkti aken. Kuni 24 tundi säilituspoliitika on toetatud masinad kopeeritud premium mälu.
6. **Rakenduse ühtsete hetktõmmise sagedus**, määrake taastamise punkte, mis sisaldavad rakenduse ühtsete hetktõmmiseid luuakse sageduse (minutites).
7. Kui loote dispersioonanalüüs poliitika, vaikimisi vastavaid poliitika luuakse automaatselt failback. Näiteks kui dispersioonanalüüs poliitika on **Vabariik-korraga** , siis failback poliitika saab **Vabariik-poliitika-failback**. See poliitika ei kasutata enne, kui annate on failback.  
8. Klõpsake nuppu **OK** , et luua poliitika.

    ![Poliitika dispersioonanalüüs](./media/site-recovery-vmware-to-azure/gs-replication2.png)

9. Kui loote uue poliitika on automaatselt seotud konfiguratsiooni serveriga. Klõpsake nuppu **OK**.

    ![Poliitika dispersioonanalüüs](./media/site-recovery-vmware-to-azure/gs-replication3.png)


## <a name="step-5-capacity-planning"></a>Juhis 5: Võimsuse planeerimine

Nüüd, kui teil on oma basic taristu saate häälestada saate mõelda võimsuse planeerimine ja aru saada, kas teil on vaja täiendavaid ressursse.

Saidi taastamine pakub võimsus Plaanur aitavad eraldada õige ressursside allika keskkonna, saidi taastamise komponendid, võrgunduse ja salvestusruumi. Saate käivitada soovitud Plaanur kiirrežiimis valmisväärtusi põhinevad keskmine arv VMs, ketast ja salvestusruumi või üksikasjalik režiimis, kus saate sisestada töökoormus tasandi andmed. Enne alustamist peate:

- Koguge teavet dispersioonanalüüs keskkonna, sh VMs kohta VMs ketast ja ketta salvestusruumi.
- Hindamiseks igapäevane muutmine (piimapütt) peate kopeeritud andmed. [VSphere võimsus kavandamise seadme](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) abil saate seda teha.

1.  Klõpsake tööriista alla laadida ja seejärel käivitage see **alla laadida** . [Lugege artiklit](site-recovery-capacity-planner.md) , mis kaasneb tööriist.
2.  Kui olete valige **Jah** lõpetanud **on lõpetatud, võimsuse planeerimine?**

    ![Võimsuse planeerimine](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)

Järgmises tabelis sisaldab arvu punkte, mis aitavad teil mahuga kavandamise seda stsenaariumi.


**Komponent** | **Üksikasjad**
--- | --- | ---
**Dispersioonanalüüs** | **Maksimum iga päev muuta**– kaitstud masina saate kasutada ainult üks protsess server ja ühe protsessi serveris saate iga päev toime muutmine määr on 2 TB. Seega on 2 TB maksimaalne igapäevane andmeid muuta määr, mis on toetatud kaitstud kohapeal.<br/><br/> **Maksimaalse läbilaskevõime**– tiražeeritud masina saate kuuluvad ühe salvestusruumi konto Azure. Kindlad konto võite hakkama kuni 20 000 taotlusi sekundis ja soovitame, et hoiate IOPS arvu allika arvutisse üle 20 000. Jaoks näide kui teil on allikas arvutisse ja 5 ketast iga ketta genereeritud 120 IOPS (8K suurus) allikas siis võib Azure'is ketta IOPS limiit 500 kohta. Arvu salvestusruumi kontode nõutav = kokku allika IOPs/20 000.
**Konfiguratsiooniserveri** | Konfiguratsiooniserveri peaks oskama toime igapäevane muutmine määr võimsus üle kõik töökoormused töötab kaitstud arvutites ja vajab piisavalt ribalaiust pidevalt korrata andmete Azure salvestusruumi.<br/><br/> Hea tava soovitame, et konfiguratsiooniserveri asuma samas võrgus ja LAN lõigu on masinad, mida soovite kaitsta. See võib asuda erinevas võrgus, kuid masinad, mida soovite kaitsta peaks olema L3 võrgu nähtavuse selle.<br/><br/> Järgmises tabelis on kokku võetud suurus konfiguratsiooniserveri soovitused.
**Protsessi server** | Esimene toiming server on installitud vaikimisi konfigureerimine serveris. Saate juurutada täiendavad protsessi serverid mastaapimiseks keskkonna. Pange tähele järgmist.<br/><br/> Protsessi server saab dispersioonanalüüs kaitstud masinad ja optimeerib vahemällu, tihendamise ja krüptimise enne saatmist Azure. Protsessi serveri arvuti peaks olema piisavalt ressursse, et teha järgmisi toiminguid.<br/><br/> Protsessi server kasutab vastavalt vahemälu. Soovitame eraldi vahemälu kettale 600 GB või rohkem salvestatud võrgu kitsaskoht või katkestuste korral andmemuudatuste käsitleda.

### <a name="size-recommendations-for-the-configuration-server"></a>Suurus konfiguratsiooniserveri soovitused

**CPU** | **Mälu** | **Vahemälu ketta suurus** | **Andmete muutmine määr** | **Kaitstud masinad**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 tuuma @ 2,5 GHz) | 16 GB | 300 GB | 500 GB või vähem | Vähem kui 100 masinad korrata.
12 vCPUs (2 sockets * 6 tuuma @ 2,5 GHz) | 18 GB | 600 GB | 500 GB kuni 1 TB | 100-150 masinad vahel kopeerida.
16 vCPUs (2 sockets * 8 tuuma @ 2,5 GHz) | 32 GB | 1 TB | 1 TB kuni 2 TB | 150-200 masinad vahel kopeerida.
Mõne muu protsess serveri juurutamine | | | > 2 TB | Täiendavad protsessi serverid juurutada, kui te olete imitatsiooniga päevas üle 200 masinad või kui igapäevane andmeid muuta ületab 2 TB.

Kui:

- Iga andmeallika arvuti on konfigureeritud 3 ketast 100 GB.
- Kasutasime võrdleva talletamist 8 SAS draivid 10 k pööret MINUTIS RAID 10 vahemälu kettale mõõtmed.

### <a name="size-recommendations-for-the-process-server"></a>Suuruse soovitused protsessi server

Kui vajate rohkem kui 200 masinad kaitsmiseks või igapäevane muutmine määr on suurem kui 2 TB saate lisada täiendavad protsessi serverid käsitlema dispersioonanalüüs laadi. Mastaapimiseks te teha järgmist.

- Konfiguratsiooni servers arvu suurendamine Näiteks saate kaitsta kuni 400 masinad konfiguratsiooni serveritega.
- Täiendavad protsessi serverid lisamine ja nende abil saate teha liikluse asemel (või lisaks) konfiguratsiooniserveri.

Selles tabelis kirjeldatakse stsenaarium, kus.

- Te ei kavatse kasutada konfiguratsiooniserveri protsessi server.
- Olete häälestanud serveriks täiendavad protsess.
- Konfigureerige olete kaitstud virtuaalmasinates kasutada täiendavaid protsessi server.
- Iga kaitstud allika arvuti on konfigureeritud kolm ketast 100 GB.

**Konfiguratsiooniserveri** | **Täiendavad protsessi server**| **Vahemälu ketta suurus** | **Andmete muutmine määr** | **Kaitstud masinad**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 tuuma @ 2,5 GHz), 16 GB mälu | 4 vCPUs (2 sockets * 2 tuuma @ 2,5 GHz), 8 GB mälu | 300 GB | 250 GB või vähem | 85 või kopeerida.
8 vCPUs (2 sockets * 4 tuuma @ 2,5 GHz), 16 GB mälu | 8 vCPUs (2 sockets * 4 tuuma @ 2,5 GHz), 12 GB mälu | 600 GB | 250 GB kuni 1 TB | Vahel 85-150 masinad korrata.
12 vCPUs (2 sockets * 6 tuuma @ 2,5 GHz), 18 GB mälu | 12 vCPUs (2 sockets * 6 tuuma @ 2,5 GHz) 24 GB mälu | 1 TB | 1 TB kuni 2 TB | 150-225 masinad vahel kopeerida.


Nii, nagu teie serverid skaalal on sõltuvad teie eelistust skaala üles või mastaapimiseks välja mudel.  Kasutades mõne klassi konfiguratsiooni ja protsessi servers skaalal või mastaapimiseks välja alusel rohkem serverite vähem ressursse rakendades. Näide: kui teil on vaja kaitsta 220 masinad võivad teha üht järgmistest:

- Konfiguratsiooniserveri 12vCPU, 18 GB mälu, täiendavad protsessi serverisse 12vCPU 24 GB mälu, häälestada ja konfigureerida kaitstud ainult server täiendav protsessi kasutama masinad.
- Teise võimalusena võib kaks konfiguratsiooni serverid (2 x 8vCPU, 16 GB RAM-i) ja kahe täiendavad protsessi server (1 x 8vCPU) ja 4vCPU x 1 135 + 85 (220) masinad käsitlema konfigureerimine ja kaitstud täiendavad protsessi serverid ainult kasutama masinad konfigureerimine.

[Järgige neid juhiseid](#deploy-additional-process-servers) täiendavate protsessi serveri seadistamine.

### <a name="network-bandwidth-considerations"></a>Võrgu läbilaskevõime kasutuse

Võimsus Plaanur tööriista abil saate arvutada vajalikku läbilaskevõimet kopeerimise (algse kopeerimise ja seejärel delta). Kontrollida läbilaskevõime kasutuse kopeerimise summa on teil mitu võimalust.

- **Throttle läbilaskevõime**: Azure'i tiražeerib VMware liiklus läheb konkreetse protsessi serveri kaudu. Saate throttle läbilaskevõime masinad protsessi serverites töötab.
- **Läbilaskevõime mõju**: saate mõjutada kopeerimise abil registrivõtmete paar kasutatav läbilaskevõime:
    - **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure'i Backup\UploadThreadsPerVM** registriväärtuse määrab teemad, mida kasutatakse andmeedastus (esmane või delta dispersioonanalüüs) ketta arvu. Suurem väärtus suurendab võrgu läbilaskevõime, kasutatakse kopeerimine.
    - **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure'i Backup\DownloadThreadsPerVM** määrab kasutada andmete edastamiseks ajal failback arvu.

#### <a name="throttle-bandwidth"></a>Ahendamise läbilaskevõime

1. Avage Microsoft Azure'i varundus MMC lisandmooduli abistas protsessi Serveri arvutis. Vaikimisi on saadaval töölaual või C:\Program Files\Microsoft Azure taastamise teenused Agent\bin\wabadmin otsetee Microsoft Azure varukoopia.
2. Lisandmooduli nuppu **Muuda atribuute**.

    ![Ahendamise läbilaskevõime](./media/site-recovery-vmware-to-azure/throttle1.png)

3. Vahekaardil **Throttling** valige **Luba Interneti läbilaskevõime kasutuse varukoopia toimingute pidurdamise**, ja töö piirangud ja mitte-töö tundi. Lubatud vahemikud on 512 Kbps 102 Mbps sekundis.

    ![Ahendamise läbilaskevõime](./media/site-recovery-vmware-to-azure/throttle2.png)

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

## <a name="step-6-replicate-applications"></a>Samm 6: Dubleeriva rakendused

Veenduge, et masinad, mida soovite korrata on valmis Mobility teenuse Installimise, ja seejärel lubada kopeerimine.

### <a name="install-the-mobility-service"></a>Mobiilsus teenuse installimine

Mobiilsus teenuse installimiseks on esimene samm kaitse virtuaalmasinates ja füüsilise serveri lubamine. Saate teha seda mitmel viisil.

- **Protsessi serveri push**: dispersioonanalüüs arvutisse lubamisel tõuketeatised ja installige see protsess server Mobility teenuste osa. Pange tähele, et push install ei ilmneda juhul, kui masinad on juba üles-todate versiooni komponent.
- **Ettevõtte tõuketeatised**: automaatselt installida komponent, kasutades oma ettevõtte tõuketeatised protsessi nagu WSUS või System Center Configuration Manager või [Azure automatiseerimine soovitud riik konfigureerimine](./site-recovery-automate-mobility-service-install.md). Häälestage konfiguratsiooniserveri enne selle tegemist.
- **Käsitsi installimine**: komponendi käsitsi installida igas arvutis, mida soovite korrata. Häälestage konfiguratsiooniserveri enne selle tegemist.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Automaatse tõuketeatised Windows arvutites ettevalmistamine

Siin on, kuidas ette valmistada Windowsi masinad nii, et protsessi server saab automaatselt installida mobiilsus teenuse.

1.  Looge konto, mida saab kasutada protsessi serveri juurde pääseda arvuti. Konto peaks olema administraatoriõigused (kohalik või Domeen) ja seda kasutatakse ainult selle tõuketeatised installi.

    >[AZURE.NOTE] Kui te ei kasuta domeenikonto peate keelata Remote kasutaja juurdepääsu reguleerimine kohalikus arvutis. Selleks vastavalt HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System registrisse Lisage DWORD-i kirje LocalAccountTokenFilterPolicy väärtuseks 1. Registrikirje lisada CLI tüüp **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  Windowsi tulemüüri masina, mida soovite kaitsta, valige **Luba rakendus või läbi tulemüüri**. Luba **failide ja printeri ühiskasutus** ja **Windows Management seadmeid**. Masinad, mis kuuluvad domeeni saate konfigureerida tulemüürisätteid rühmapoliitika objekti.

    ![Tulemüüri sätted](./media/site-recovery-vmware-to-azure/mobility1.png)

2. Loodud konto lisamiseks tehke järgmist.

    - Avatud **cspsconfigtool**. See on saadaval töölaua otsetee ja asub kaustas [installida asukoht] \home\svsystems\bin.
    - Klõpsake menüüs **Kontod haldamine** nuppu **Lisa konto**.
    - Lisage loodud konto. Pärast konto lisamist peate mandaat, kui lubate dispersioonanalüüs mõne seadme jaoks.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Automaatse tõuketeatised Linux serverites ettevalmistamine

1.  Veenduge, et Linux arvutisse, mida soovite kaitsta toetatakse [kaitstud masina eeltingimused](#protected-machine-prerequisites)kirjeldatud. Veenduge, et on võrguühendus Linux seadme ja protsessi serveri vahel.

2.  Looge konto, mida saab kasutada protsessi serveri juurde pääseda arvuti. Konto peaks olema root kasutaja andmeallika Linux serveris ja seda kasutatakse ainult selle tõuketeatised installi.

    - Avatud **cspsconfigtool**. See on saadaval töölaua otsetee ja asub kaustas [installida asukoht] \home\svsystems\bin.
    - Klõpsake menüüs **Kontod haldamine** nuppu **Lisa konto**.
    - Lisage loodud konto. Pärast konto lisamist peate mandaat, kui lubate dispersioonanalüüs mõne seadme jaoks.

3.  Kontrollige, et muutsite faili allikas Linux server sisaldab kirjeid, mis kohaliku hostname vastendada kõik võrguadapteri seotud IP-aadressid.
4.  Installige uusim openssh, openssh-server, openssl pakettide arvutisse, mida soovite korrata.
5.  Veenduge, et SSH port 22 on lubatud ja töötab.
6.  Lubage SFTP alasüsteemi ja parooli autentimine sshd_config faili järgmiselt:

    - Logige sisse root.
    - Otsige fail /etc/ssh/sshd_config rida, mis algab väärtusega **PasswordAuthentication**.
    - Kommenteerige rea välja ja muutke kaudu **ei** väärtuseks **Jah**.
    - Otsige üles rida, mis algab väärtusega **alasüsteemi** ja Eemalda kommentaar rea.

        ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>Mobiilsus teenuse käsitsi installimine

Paigaldajaid on **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**konfigureerimine serveris saadaval.

Andmeallika operatsioonisüsteem | Installi Mobility teenusefail
--- | ---
Windows Server (ainult 64-bitine) | Microsoft-ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6.4, 6.5, 6.6 (ainult 64-bitine) | Microsoft-ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 SP3 (ainult 64-bitine) | Microsoft-ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle'i Enterprise Linux 6.4 6.5 (ainult 64-bitine) | Microsoft-ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-mobility-service-on-a-windows-server"></a>Installige Windows Server mobiilsus teenuse


1. Laadige alla ja käivitage installer oluline.
2. **Enne alustamist** valige **mobiilsus teenuse**.

    ![Mobiilsus teenuse](./media/site-recovery-vmware-to-azure/mobility3.png)

3. Määrake **Konfiguratsiooni serveri üksikasjad** konfiguratsiooniserveri ja parool, kui käivitasite ühendatud installiprogrammi loodud IP-aadress. Saate tuua parool töötab: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – v** konfigureerimine serveris.

    ![Mobiilsus teenuse](./media/site-recovery-vmware-to-azure/mobility6.png)

4. **Installige** asukohas jätke vaikesäte ja klõpsake nuppu **Järgmine** installimise alustamiseks.
5. **Installi edenemist** jälgida installi ja taaskäivitage arvuti viiba. Pärast installimist teenuse võib kuluda umbes 15 minutit oleku värskendamiseks portaalis.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>Installige Windows Server käsuviiba abil mobiilsus teenuse

1. Kopeerige installer kohalikus kaustas (st C:\Temp) server, mida soovite kaitsta. Installiprogrammi leiate jaotises **[installida asukoht] \home\svsystems\pushinstallsvc\repository**konfigureerimine serveris. Windowsi opsüsteemid paketi on sarnaselt Microsoft-ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe nimi
2. **Ümber nimetada** seda faili MobilitySvcInstaller.exe
3. Käivitage järgmine käsk MSI Installeri eraldamiseks </br>

        C:\> cd C:\Temp
        C:\Temp> MobilitySvcInstaller.exe /q /xC:\Temp\Extracted
        C:\Temp> cd Extracted
        C:\Temp\Extracted> UnifiedAgent.exe /Role "Agent" /CSEndpoint "IP Address of Configuration Server" /PassphraseFilePath <Full path to the passphrase file>

#####<a name="full-command-line-syntax"></a>Täielik käsurea süntaks

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Parameetrid**

- **/Role:** Kohustuslik. Saate määrata, kas mobiilsus teenuse peaks olema installitud. Sisestusmeetodi väärtused Agent | MasterTarget
- **/InstallLocation:** Kohustuslik. Saate määrata, kus teenuse installida.
- **/PassphraseFilePath:** Kohustuslik. Konfiguratsiooni serveri parool.
- **/LogFilePath:** Kohustuslik. Kus luuakse installi logifailide asukohta.



#### <a name="uninstall-mobility-service-manually"></a>Mobiilsus teenuse käsitsi desinstallimine

Mobiilsus teenuse saab lisada eemaldada programmi juhtpaneeli kaudu või kasutades käsurea desinstallida.

Käsk desinstallida mobiilsus teenuse käsurea kaudu

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}


#### <a name="install-mobility-service-on-a-linux-server-using-command-line"></a>Käsurea kaudu Linux server mobiilsus teenuse installimine

1. Kopeerige vastav tõrva arhiivi põhjal tabeli kohal Linux arvutisse, mida soovite korrata.
2. Shell programmi avamiseks ja väljavõte ZIP tar Arhiiv kohalik tee töötab.`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Kohalikku kausta, kuhu ekstraktitud tõrva arhiivi sisu passphrase.txt faili loomine. Teha seda kopeerige parool C:\ProgramData\Microsoft Azure saidi Recovery\private\connection.passphrase konfigureerimine serveris ja salvestage see passphrase.txt käivitades *`echo <passphrase> >passphrase.txt`* Shellis.
4. Mobiilsus teenuse installida käivitades *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Määrake konfiguratsiooniserveri sisemise IP-aadress ja veenduge, et pordi 443 oleks märgitud. Pärast installimist teenuse võib kuluda umbes 15 minutit oleku värskendamiseks portaalis.

**Saate installida ka käsurea kaudu**.

1. Kopeerige parool C:\Program Files (x86) \InMage Systems\private\connection konfigureerimine serveris ja salvestage see "passphrase.txt" konfigureerimine serveris. Käivitage järgmised käsud. Käesolevas näites konfiguratsiooni serveri IP-aadress on 104.40.75.37 ja tuleks HTTPS pordi 443.

Tootmise server installimiseks tehke järgmist.

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Klõpsake serveri installimiseks tehke järgmist.


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


### <a name="enable-replication"></a>Lubada kopeerimine

#### <a name="before-you-start"></a>Enne alustamist

Kui te olete imitatsiooniga VMware virtuaalse masinad võtke arvesse järgmist.

- VMware VMs avastatakse iga 15 minuti ja võib kuluda 15 minutit või enam pärast on discovery portaalis kuvada. Samuti discovery võib kuluda 15 minutit või rohkem kui lisate uue vCenter server või vSphere host.
- Keskkonna muutused virtuaalse masina (nt VMware tööriistad install) võib võtta ka 15 minutit või rohkem portaalis värskendada.
- Saate viimast avastatud VMware vms välja **Viimase kontakti veebisaidil** vCenter server/vSphere Host **Konfiguratsiooni serverid** enne.
- Masinad kopeerimise ootamata ajastatud Discovery lisamiseks esiletõstmine konfiguratsiooniserveri (ärge klõpsake seda) ja klõpsake nuppu **Värskenda** .
- Kui lubate dispersioonanalüüs, kui seade on valmis protsess server automaatselt installib mobiilsus teenuse seda.

#### <a name="exclude-disks-from-replication"></a>Ketast välistamine dispersioonanalüüs

Kui lubate dispersioonanalüüs, vaikimisi kõik ketast arvutisse on kopeeritud. Saate ketast välistamine kopeerimine. Näiteks võite soovida pole ketast koos ajutised või andmeid, mis on värskendada iga kord, kui mõnda arvutisse kopeerida või rakenduse taaskäivitamist (nt pagefile.sys või SQL serveri tempdb). Kui soovite välistada ketast pidage meeles järgmist:

- Saate ainult ketast, mis on juba installitud mobiilsus teenuse välja jätta. Peate [käsitsi](#install-the-mobility-service-manually) installimine mobiilsus teenuse Kuna mobiilsus teenuse installitakse ainult kasutamist tõuketeatised pärast kopeerimine on lubatud.
- Saate välja jätta ainult ketta kopeerimine. OS või dünaamiline ketast, ei saa välja jätta.
- Pärast kopeerimine on lubatud ei saa lisada või eemaldada ketast kopeerimise. Kui soovite lisada või välistamine kettal peate keelata masina kaitse ja seejärel lubage uuesti.
- Kui ketas, mis on vajalikud rakenduse tegutseda välistamiseks pärast Tõrkesiirde Azure peate selle käsitsi looma Azure et tiražeeritud rakenduse käivitada. Teise võimalusena võib integreerida taastamise kava loomiseks ketas ajal Tõrkesiirde masina Azure automatiseerimine.
- Saate luua käsitsi Azure ketast on tagasi nurjus. Näiteks kui nurjuda kolme ketast ja luua kaks otse Azure'i, kuvatakse kõik viis tagasi nurjus. Käsitsi loodud failback ketast ei saa välja jätta.

**Nüüd lubada kopeerimine järgmiselt**:

1. Klõpsake **Samm 2: rakenduse ise** > **Allikas**. Kui olete lubanud dispersioonanalüüs esimest korda võite nuppu **+ ise** võlvkelder lubamiseks dispersioonanalüüs arvutite jaoks.
2. **Andmeallika** tera > **andmeallika** valimine konfiguratsiooniserveri.
3. Valige **seadme tüüp** **Virtuaalmasinates** või **Füüsilise masinad**.
4. Valige vCenter server vSphere host haldava **vCenter/vSphere Hypervisor** või valige selle hosti. See säte pole oluline, kui te olete imitatsiooniga füüsilise masinad.
5. Valige protsess server. Kui te pole loonud mis tahes täiendavaid protsessi serverid sellest saab konfiguratsiooni serveri nimi. Klõpsake nuppu **OK**.

    ![Lubada kopeerimine](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. **Target (sihtkoht)** valige vault tellimus ja valige **Postita-Tõrkesiirde juurutamise mudeli** mudeli (klassikaline või ressursside haldamine), mida soovite kasutada Azure pärast Tõrkesiirde.
7. Valige Azure storage konto, saate kasutada imitatsiooniga andmete jaoks. Pange tähele järgmist.

    - Saate valida kindlad konto või premium. Kui valite premium konto, tuleb teil määrata poolelioleva dispersioonanalüüs logide konto standard täiendav salvestusruum. Kontode peab olema sama piirkonna vault taastamise teenused.
    - Kui soovite kasutada erinevaid salvestusruumi konto, kui teil on te saate [luua](#set-up-an-azure-storage-account). Näidise ARM konto storage loomiseks klõpsake nuppu **Loo uus**. Kui soovite luua salvestusruumi konto abil klassikaline, mida saate teha selle [Azure'i portaalis](../storage/storage-create-storage-account-classic-portal.md).

8. Valige Azure võrgu- ja millele loob ühenduse Azure VMs, kui need on kedratud pärast Tõrkesiirde alamvõrgu. Võrgu peab olema sama piirkonna vault taastamise teenused. Valige võrgu sätte rakendamiseks kõik masinad valite kaitse **nüüd jaoks valitud masinad konfigureerimine** . Valige **Konfigureeri hiljem** Azure võrgu masina kohta. Kui teil pole võrgu peate selle [loomiseks](#set-up-an-azure-network). Võrgu ARM mudeli loomiseks klõpsake nuppu **Loo uus**. Kui soovite luua võrgu klassikaline teen selle [Azure'i portaalis](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Valige soovitud alamvõrgu võimalusel. Klõpsake nuppu **OK**.

    ![Lubada kopeerimine](./media/site-recovery-vmware-to-azure/enable-replication3.png)

9. **Virtuaalmasinates** > **Valige virtuaalmasinates** klõpsake ja valige igas arvutis, mida soovite korrata. Saate valida ainult masinad dispersioonanalüüs saate lubatud. Klõpsake nuppu **OK**.

    ![Lubada kopeerimine](./media/site-recovery-vmware-to-azure/enable-replication5.png)

10. **Atribuutide** > **atribuutide konfigureerimine**, valige konto, mida kasutatakse protsessi server automaatselt mobiilsus teenuse soovitud arvutisse installida. Vaikimisi on kõik ketast kopeeritud. Klõpsake **Kõigi ketta** ja tühjendage kõik ketast, kui te ei soovi kopeerida. Klõpsake nuppu **OK**. Täiendavad atribuudid saate määrata hiljem.

    ![Lubada kopeerimine](./media/site-recovery-vmware-to-azure/enable-replication6.png)

11. **Dispersioonanalüüs**sätted > **sätete konfigureerimine dispersioonanalüüs** veenduge, et õige dispersioonanalüüs poliitika. Saate muuta dispersioonanalüüs poliitikasätted **sätted** > **Dispersioonanalüüs poliitikad** > poliitika nimi > **Muuda sätteid**. Saate rakendada poliitika muudatused rakendatakse juurde imitatsiooniga ja uus.

12. Luba **mitme-VM järjepidevuse** , kui soovite koguda masinad rühma kopeerimine ja määrake soovitud rühma nimi. Klõpsake nuppu **OK**. Pange tähele järgmist.

    - On paljundamine juhendist rühmitada ja on andnud krahh ühtsete ja rakenduse ühtsete taastamise punkte, kui nad ei õnnestu üle.
    - Soovitame teil koguda VMs ja füüsilise serveri koos, et nad kajastavad oma töökoormus. Võimaldab mitme-VM järjepidevuse võib mõjutada jõudlust töökoormus ja kasutada ainult kui masinad töötavad sama töökoormus ja teil on vaja järjepidevuse.

    ![Lubada kopeerimine](./media/site-recovery-vmware-to-azure/enable-replication7.png)

13. Klõpsake nuppu **Luba kopeerimine**. Saate jälgida edenemist **Lubamine kaitse** töö **sätted** > **töö** > **Saidi taastamise tööd**. Pärast **Viimistlemine kaitse** töö töötab seade on valmis Tõrkesiirde.

> [AZURE.NOTE] Kui seade on valmis tõuketeatised installi Mobility teenuste osa installitakse kaitse on lubatud. Pärast osa on installitud arvuti kaitse tööga ja nurjub. Pärast selle peate igas arvutis käsitsi taaskäivitama. Pärast uuesti kaitse töö algab uuesti ja algse dispersioonanalüüs esineb.

### <a name="view-and-manage-vm-properties"></a>Saate vaadata ja hallata VM atribuudid

Me soovitame, et veenduda masina andmeallika atribuudid. Pidage meeles, et Azure'i VM nimi peab vastama [Azure virtuaalse masina](site-recovery-best-practices.md#azure-virtual-machine-requirements)nõuetele.

1. Klõpsake nuppu **sätted** > **paljundatud üksused** > ja valige seade. **Essentialsi** tera kuvatakse teave masinad sätted ja olek.

2. **Atribuudid** kuvatakse kopeerimise ja Tõrkesiirde teabe VM.

    ![Lubada kopeerimine](./media/site-recovery-vmware-to-azure/test-failover2.png)

3. **Arvuta**ja võrgu > **arvutada atribuudid** saate määrata Azure VM nimi ja sihtkoht suurus. Azure'i nõuete täitmiseks, kui teil on vaja nime muuta.
Saate vaadata ja lisada teavet target võrgus, alamvõrgu ja IP-aadress, mis määratakse Azure VM. Võtke arvesse järgmist.

    - Saate seada target IP-aadress. Kui te ei esita aadressi nurjunud üle arvuti kasutab DHCP. Kui seate aadressi, mis pole saadaval Tõrkesiirde, on tõrkesiire ei tööta. Sama target IP-aadressi saab kasutada test Tõrkesiirde kui aadress on test Tõrkesiirde võrgus saadaval.
    - Võrguadapteri arv on tingitud suurus saate määrata target virtuaalse masina, järgmiselt:

        - Kui allika arvutisse võrguadapteri arv on väiksem või võrdne arv adapterit masina sihtformaat lubatud, siis sihtkohas on samal hulgal adapterit allikana.
        - Kui adapterit allika virtual masina arv ületab lubatud target suurus ja seejärel soovitud suurus maksimum kasutada number.
        - Näiteks kui allika arvuti on kaks võrguadapteri ja seadme sihtformaat toetab neli, sihtarvutis on kaks adapterit. Kui allika arvuti on kaks adapterit, kuid toetatud sihtformaat toetab ainult üks siis sihtarvutis on ainult üks adapterit.     
    - Kui VM on mitu võrguadapteri need kõik ühendada samasse võrku.

    ![Lubada kopeerimine](./media/site-recovery-vmware-to-azure/test-failover4.png)

4. **Ketast** näete operatsioonisüsteem ja andmete ketast VM, mis kuvatakse kopeeritud.


## <a name="step-7-test-the-deployment"></a>Juhis 7: Testige juurutamise

Selleks, et juurutamise käivitage test Tõrkesiirde ühe virtuaalse masina või taastamise kava, mis sisaldab ühe või mitme virtuaalmasinates.


### <a name="prepare-for-failover"></a>Tõrkesiirde ettevalmistamine

- Käivitage test Tõrkesiirde soovitame luua uue Azure võrk, mis on eraldatud Azure tootmise võrgu (see on vaikekäitumine, kui loote uue võrgu Azure). [Lisateavet leiate teemast](site-recovery-failover.md#run-a-test-failover) kohta, kuidas testi failovers.
- Saada parimaid tulemusi, kui teil ei õnnestu üle Azure, installige Azure'i Agent kaitstud arvutisse. See muudab käivitamist kiiremini ja aitab tõrkeotsing. Installige [Linux](https://github.com/Azure/WALinuxAgent) või [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) agent.
- Juurutamise täielikult kontrollimiseks peate infrastruktuur tiražeeritud masina õigesti töötada. Kui soovite testida Active Directory ja DNS-i saate luua virtuaalse masina nimega domeenikontrolleri DNS- ja kopeerida see Azure'i Azure saidi taastamise abil. Lugege rohkem tekstivormingus [test Tõrkesiirde kaalutluste kohta Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Veenduge, et konfiguratsiooni server töötab. Muul juhul Tõrkesiirde nurjub.
- Kui olete ketast välistatud dispersioonanalüüs peate need ketta loomiseks käsitsi Azure pärast Tõrkesiirde nii, et rakendus töötab ootuspäraselt.
- Kui soovite käivitada planeerimata Tõrkesiirde asemel test Tõrkesiirde võtke arvesse järgmist.

    - Võimaluse korral tuleks sulgeda esmane masinad enne käivitamist planeerimata Tõrkesiirde. See tagab, et teil pole nii lähte-kui ka koopia masinad töötab samal ajal. Kui te olete imitatsiooniga VMware VMs seejärel saate määrata, et saidi taastamine peaksid olema parima allika masinad sulgeda. Sõltuvalt esmane saidi see võib või ei pruugi töötada. Kui te olete imitatsiooniga füüsilise serveri saidi taastamise ei paku see suvand.
    - Kui käivitate planeerimata Tõrkesiirde lõpetab andmete kopeerimine esmane masinad nii, et kõik andmed delta ei üle pärast planeerimata Tõrkesiirde hakkab. Lisaks kui käivitate planeerimata Tõrkesiirde taastamis see töötab seni, kuni olete lõpetanud, isegi kui ilmneb tõrge.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Pärast Tõrkesiirde Azure VMs ühenduse ettevalmistamine

Kui soovite ühendada Azure VMs abil RDP pärast Tõrkesiirde, veenduge, et saate teha järgmist:

**Kohapealse arvutisse enne Tõrkesiirde**:

- Interneti kaudu juurdepääsu lubamine RDP, veenduge, et TCP- ja UDP reeglid lisatakse ka **avalik**ja veenduge, et **Windowsi tulemüür**on lubatud RDP -> **lubatud rakendused ja funktsioonid** kõigi profiilide.
- Saitide ühenduse kaudu juurdepääsu lubamine RDP arvutis ja veenduge, et **Windowsi tulemüür**on lubatud RDP -> **lubatud rakendused ja funktsioonid** **domeeni** ja **Privaatne** võrgustikke.
- Kohapealse arvutisse installida [Azure VM agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) .
- [Käsitsi installimine mobiilsus teenuse](#install-the-mobility-service-manually) masinad asemel protsessi serveri push teenuse automaatselt. See on tõuketeatised installimise juhtub ainult siis, kui seade on lubatud kopeerimine.
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

**Klõpsake the Azure Windows/Linux VM pärast Tõrkesiirde**:

Kui teil on seostatud virtuaalse masina või alamvõrgu, millele seade kuulub võrgu turberühma, veenduge, et võrgu turberühma on Väljamineva reegli lubamiseks HTTP-või HTTPS. Veenduge, et DNS-i, mis virtuaalse masina võrgu on saada nurjus üle on õigesti konfigureeritud. Veel on tõrkesiire võib tõrkega-"PreFailoverWorkflow tööülesande WaitForScriptExecutionTask aegus". Mõistmaks üksikasjalikult, vt jaotist [jälgimis- ja tõrkeotsingujuhendi](site-recovery-monitoring-and-troubleshooting.md#recovery)taastamise kohta.

## <a name="run-a-test-failover"></a>Käivitage test Tõrkesiirde

1. Nurjumise üle ühe masina, **sätteid** > **Kopeeritud üksused**, klõpsake VM > **+ Test Tõrkesiirde** ikooni.

    ![Test Tõrkesiirde](./media/site-recovery-vmware-to-azure/test-failover1.png)

2. Kasutada taastamise kava **sätted** > **Taastamine lepingud**, paremklõpsake leping > **Test Tõrkesiirde**. Luua enam kavandamine, [järgige neid juhiseid](site-recovery-create-recovery-plans.md).

3. Valige **Test Tõrkesiirde** Azure võrgu, millega on ühendatud Azure VMs, kui Tõrkesiirde.
4. Klõpsake nuppu **OK** , et alustada selle Tõrkesiirde. Saate jälgimiseks, klõpsates VM selle atribuudid avamiseks või **Test Tõrkesiirde** töö vault nimi > **sätted** > **töö** > **saidi taastamise tööd**.
5. Kui selle Tõrkesiirde jõuab **lõpuleviimine testimine** olek, tehke järgmist:

    1. Saate vaadata koopia virtuaalse masina Azure'i portaalis. Veenduge, et virtuaalse masina õnnestub.
    2. Kui te pole Accessi virtuaalmasinates kuni määramine kohapealse võrgu kaudu saate algatada virtuaalse masina kaugtöölaua ühendus.
    3. Klõpsake nuppu **täielik testida** seda lõpetada.

        ![Test Tõrkesiirde](./media/site-recovery-vmware-to-azure/test-failover6.png)


    4. Klõpsake nuppu **märkmed** salvestada ja salvestada märkusi, mis on seotud test Tõrkesiirde.
    5. Klõpsake linki automaatselt puhastamiseks testi keskkonna **test Tõrkesiirde on lõpule viidud** . Kui seda tehakse test Tõrkesiirde kuvada **lõpuleviimine** olek.
    6.  Selles etapis mis tahes elemente või saidi taastamine test Tõrkesiirde ajal automaatselt loodud VMs kustutatakse. Teie loodud test Tõrkesiirde täiendavad andmed ei kustutata.

    > [AZURE.NOTE] Kui test Tõrkesiirde kestab kauem kui kaks nädalat lõpuleviimist jõuga.


6. Pärast selle Tõrkesiirde lõpulejõudmist saate ka peaks olema võimalus leiate Azure'i koopia arvutisse kuvatakse Azure'i portaalis > **Virtuaalmasinates**. Veenduge, et VM on sobiv suurus, mis see on sobiv võrku ühendatud ja see töötab.
7. Kui te [pärast Tõrkesiirde ühenduste valmis](#prepare-to-connect-to-azure-vms-after-failover) teiega ühendust luua Azure VM.

## <a name="monitor-your-deployment"></a>Juurutamise jälgimine

Siin on, kuidas saate jälgida konfiguratsioonisätted, olek ja seisund oma saidi taastamine juurutamiseks.

1. Klõpsake vault nime juurdepääsu **Essentialsi** armatuurlaud. Armatuurlaua saate saidi taastamise tööde haldamine, kopeerimise olek, taastamise, serveri seisundi ja sündmusi.  Saate kohandada Essentialsi paanid ja paigutusi, mis on kõige kasulikumad teile, sh saidi varundus ja taaste võlvid oleku kuvamiseks.<br>
![Essentialsi](./media/site-recovery-vmware-to-azure/essentials.png)

2. Paani **seisundit** saate jälgida saidi serverid (VMM või konfiguratsiooni servers), mis on tekkinud probleem ja sündmuste tõstatatud saidi taastamine viimase 24 tunni jooksul.
3. Saate hallata ja jälgida **Kopeeritud üksused**, **Taastamine lepingud**, kopeerimise ja **Saidi taastamise töö** paanid. Saab süvitsi minna tööle **sätted** -> **töö** -> **Saidi taastamise tööd**.


## <a name="deploy-additional-process-servers"></a>Täiendavad protsessi serverid juurutamine

Kui teil on skaala välja juurutamise üle 200 allika masinad või kogu päeva piimapütt määr on rohkem kui 2 TB, peate täiendavad protsessi serverid käsitlema liikluse maht.

Kontrollige [suurus soovitused protsessi serverid](#size-recommendations-for-the-process-server) ja järgige neid juhiseid, et protsessi serveri häälestamine. Pärast häälestamist server migreerite allika masinad seda kasutada.

### <a name="install-an-additional-process-server"></a>Installima serveri täiendavad protsess

1. **Sätted** > **saidi taastamine serverid** nuppu konfiguratsiooniserveri > **protsess server**.

    ![Protsessi serveri lisamine](./media/site-recovery-vmware-to-azure/migrate-ps1.png)

2. **Serveri tüüp** nuppu **protsess server (kohapealse)**.

    ![Protsessi serveri lisamine](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

3. Saidi taastamise ühendatud Setup faili alla laadida ja käivitage see protsess serverit installida ja registreerida autoriloomingut.
4. **Enne alustamist** valige **Lisa täiendavad protsessi serverid välja juurutamise skaala**.
5. Viisardi lõpuleviimine samamoodi kui tegite [häälestamine](#step-2-set-up-the-source-environment) konfiguratsiooniserveri.

    ![Protsessi serveri lisamine](./media/site-recovery-vmware-to-azure/add-ps1.png)

6. Määrake **Konfiguratsiooni serveri üksikasjad** IP-aadress konfiguratsiooniserveri ja parool. Saada parool käivitada ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** konfigureerimine serveris.

    ![Protsessi serveri lisamine](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Uus protsess server kasutama masinad migreerimine

1. **Sätted** > **saidi taastamine serverid** konfiguratsiooniserveri nuppu ja seejärel laiendage **protsessi serverites**.

    ![Protsessi uuendamine.](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

2. Paremklõpsake praegu kasutusel protsessi server ja klõpsake nuppu **Aktiveeri**.

    ![Protsessi uuendamine.](./media/site-recovery-vmware-to-azure/migrate-ps3.png)

3. **Valige target protsessi server**, valige uus protsess server, mida soovite kasutada, ja seejärel valige uus protsess server käsitlemiseks virtuaalmasinates. Klõpsake ikooni teabe serveri kohta teabe saamiseks. Aidata teil teha otsuseid laadimine, kuvatakse Keskmine ruumi, mida on vaja paljundada iga valitud virtuaalse masina uue protsessi server. Klõpsake märkeruutu imitatsiooniga serveriga uue protsessi käivitamiseks.

## <a name="vmware-account-permissions"></a>VMware konto õigused

Protsessi serveri saaksid automaatselt tuvastada VMs vCenter serveris. Automaatse tuvastamise sooritamiseks peate määratleda [rolli (Azure_Site_Recovery)](#prepare-an-account-for-automatic-discovery) saidi taastamine VMware server juurdepääsu lubamine. Pange tähele, et kui teil on vaja ainult migreerimine VMware masinad Azure ja ei vaja failback Azure, saate määratleda kirjutuskaitstud rolli, mis on piisavalt. Järgmises tabelis on kokku võetud vajalikud rolli õigused.

**Roll** | **Üksikasjad** | **Õigused**
--- | --- | ---
Azure_Site_Recovery roll | Vmware'i VM tuvastamine |V-Center server nende õiguste määramine<br/><br/>Andmesalve -> Eralda ruumi, Sirvi andmesalve, madala taseme faili toimingud., Eemalda faili, värskendamise virtuaalse masina failid<br/><br/>Võrgu -> võrgu määramine<br/><br/>Ressursi -> Määra virtual masina ressursivaru, virtuaalse masina lülitatud migreerimine, virtuaalse masina sisse lülitatud migreerimine<br/><br/>Tööülesannete -> Loo ülesanne, tööülesande värskendamine<br/><br/>Virtuaalse masina -> Konfiguratsioon<br/><br/>Virtuaalse masina -> Interact -> vastust küsimuse seadme ühendus., meediumi konfigureerimine CD-le, konfigureerimine floppy meedia, lülitage Power, installida VMware tööriistad<br/><br/>Virtuaalse masina -> varude -> Loo, Register registreerimise tühistamise<br/><br/>Virtuaalse masina -> Provisioning -> Luba virtual arvutisse alla laadida, luba virtuaalse masina failide üleslaadimine<br/><br/>Virtuaalse masina -> hetktõmmiseid -> Eemalda hetktõmmiseid
vCenter kasutajaroll | Rakenduse discovery Vmware'i VM Tõrkesiirde ilma allika VM sulgemine | V-Center server nende õiguste määramine<br/><br/>Andmekeskuse objekt -> Propagate lapse objektile, rolli = kirjutuskaitstud <br/><br/>Kasutajale on määratud andmekeskuse tasemel ja seega on juurdepääs kõigi objektide andmekeskuses.  Kui soovite piirata juurdepääsu, **ei pääse** roll **Propagate laps** objekt määrata lapse objektide (vSphere hosts, datastores, VMs ja võrke).
vCenter kasutajaroll | Tõrkesiirde ja failback | V-Center server nende õiguste määramine<br/><br/>Andmekeskuse objekti – Propagate lapse objektile, rolli = Azure_Site_Recovery<br/><br/>Kasutajale on määratud andmekeskuse tasemel ja seega on juurdepääs kõigi objektide andmekeskuses.  Kui soovite piirata juurdepääsu, **ei pääse** rolli koos **Propagate lapse objektile** määramine lapse objekti (vSphere hosts, datastores, VMs ja võrke).  
## <a name="next-steps"></a>Järgmised sammud

- [Lisateavet](site-recovery-failover.md) eri tüüpi Tõrkesiirde kohta.
- [Lisateave failback](site-recovery-failback-azure-to-vmware.md) toob oma nurjunud üle, kus käitatakse Azure naasta kohapealse keskkonna.

## <a name="third-party-software-notices-and-information"></a>Kolmanda osapoole tarkvara teatised ja teave

Ärge tõlkimine või Localize

Tarkvara ja püsivara töötab Microsofti toote või teenuse põhineb või sisaldab materjali projektid, mis on loetletud allpool "(ühiselt kolmanda osapoole kood).  Microsoft on kolmanda osapoole koodiga mitte algse autori.  Algne Autoriõigus ja litsents, millise Microsoft saanud selliste muude tootjate kood, määratakse toodud allpool.

A jaotise teave on seotud kolmandate osapoolte kood komponentide projektid, mis on loetletud allpool. Need litsentsid ja teavet edastatakse illustreerivad.  Kolmanda osapoole järgmine kood on on relicensed teile Microsoft jaotises Microsofti tarkvara litsentsitingimused Microsofti toote või teenuse.  

Teave jaotises b on seotud kolmandate osapoolte kood komponendid, mis tehakse kättesaadavaks teile Microsofti hulgilitsentsimise algse tingimustel.

Terve faili võib leida [Microsofti allalaadimiskeskusest](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft jätab kõik õigused puudub kirjalik kinnitus, kas, kaudselt estoppel või muul viisil.
