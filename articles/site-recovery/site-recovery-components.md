<properties
    pageTitle="Saidi taastamine tööpõhimõtted | Microsoft Azure'i"
    description="Selles artiklis antakse ülevaade sellest, saidi taastamine arhitektuur"
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
    ms.topic="get-started-article"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="how-does-azure-site-recovery-work"></a>Azure'i saidi taastamine tööpõhimõtted

Lugege seda artiklit mõista arhitektuur on Azure saidi taastamise teenuse ja komponendid, mis muudavad selle töötada. 

Postitada kommentaare või küsimusi allosas käesoleva artikli või [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Ülevaade

Peavad organisatsioonid BCDR strateegia, mis määrab, kuidas rakendused, töökoormus ja andmete püsida töötab ja kättesaadav ajal plaanitud ja plaanimata tööseisakute ja taastamiseks tavaline tingimuste nii kiiresti kui võimalik. BCDR strateegia kavandamine peaks äriandmete turvalised ja saab hoida ja tagama töökoormus pidevalt katastroofi ilmnemisel. 

Saidi taastamine on Azure'i teenus, mis aitab BCDR strateegia kavandamine, orkesteroinnin dispersioonanalüüs füüsilise kohapealsed serverid ja virtuaalmasinates pilveteenusesse (Azure) või teisene andmekeskusesse. Kui katkestuste esineda teie peamine asukoht, teil ei õnnestu üle teisene asukohta hoida rakendused ja töökoormus saadaval. Te ei tagasi oma peamine asukoht kui tagastab see toiminguid. Lisateavet leiate [mis on saidi taastamise?](site-recovery-overview.md)

## <a name="site-recovery-in-the-azure-portal"></a>Saidi taastamine Azure'i portaalis

Azure'i on kaks eri [juurutamise mudelite](../resource-manager-deployment-model.md) loomise ja ressursside töötamine: Azure'i ressursihaldur mudel ja klassikaline teenuste haldus mudel. Azure'i on kaks portaalide – [Azure klassikaline portaali](https://manage.windowsazure.com/) klassikaline juurutamise mudeli toetava ja [Azure portaali](https://portal.azure.com) nii juurutamise mudelid tugi.

Saidi taastamine on saadaval nii klassikaline portaali ja Azure portaali. Azure'i klassikaline portaalis saate toetavad saidi taastamine mudeliga klassikaline teenuste haldus. Azure'i portaalis toetab klassikaline mudel või ressurss mudeli juurutuste. [Lisateavet vt](site-recovery-overview.md#site-recovery-in-the-azure-portal) juurutamise Azure'i portaalis.

Selle artikli teave kehtib klassikaline- ja Azure portaali juurutuste. Erinevused on märkida vajaduse korral.

## <a name="deployment-scenarios"></a>Juurutamise stsenaariumid

Saidi taastamine loovad korraldab dispersioonanalüüs arv stsenaariumid:

- **Ise VMware virtuaalmasinates**: Azure'i või sekundaarne andmekeskuse saaksite korrata kohapealse VMware virtuaalmasinates.
- - **Ise füüsilise masinad**: saaksite füüsilise seadmetes, kus töötab Windows või Linux, Azure'i või sekundaarne andmekeskuse korrata. Füüsilise masinad imitatsiooniga protsess on peaaegu sama imitatsiooniga VMware VMs protsess
- **Ise Hyper-V VMs (ilma VMM)**: saaksite korrata pole haldab VMM Azure Hyper-V VMs.
- **Hyper-V VMs ise hallata System Center VMM pilved**: saaksite korrata kohapealse Hyper-V virtuaalmasinates Azure'i või sekundaarne andmekeskuse VMM pilved Hyper-V hosti serverites töötab. Saate korrata, kasutades standard Hyper-V koopia või SAN kopeerimise abil.
- **Migreerimine VMs**: saate kasutada saidi taastamine [migreerimine Azure IaaS VMs](site-recovery-migrate-azure-to-azure.md) piirkondade vahel või migreerida [AWS Windowsi eksemplari](site-recovery-migrate-aws-to-azure.md) Azure IaaS vms. Praegu ainult migreerimise on toetatud, mis tähendab, et teil võib nurjuda üle need VMs, kuid te ei saa neid tagasi ei.

Saidi taastamine saate kopeerida need VMs ja füüsilise serverites töötab enamik rakendusi. Saate avada täielik Kokkuvõte toetatud rakendused [mis töökoormus saate Azure saidi taastamise kaitsta?](site-recovery-workload.md)


## <a name="replicate-to-azure-vmware-virtual-machines-or-physical-windowslinux-servers"></a>Azure'i dubleeriva: VMware virtuaalmasinates või füüsilise Windows/Linux serveri

On mitu moodust VMware VMs saidi taastamine korrata.

- **Azure'i portaalis**– kui juurutate saidi taastamine võib nurjuda üle VMs klassikaline service manager salvestusruumi või et ressursihaldur Azure'i portaalis. Imitatsiooniga VMware VMs Azure'i portaalis toob mitmeid eeliseid, sealhulgas võimalus klassikaline või ressursihaldur salvestusruumi Azure korrata. [Lisateavet leiate teemast](site-recovery-vmware-to-azure.md).
- **Klassikaline portaalis**-saidi taastamise abil täiustatud rakendusi klassikaline portaalis juurutamist. See tuleks kasutada kõigi uute juurutuste klassikaline portaalis. See juurutuse te saate ainult ei õnnestu üle VMs klassikaline salvestusruumi Azure, mitte ressursihaldur salvestusruumi. [Lisateavet leiate teemast](site-recovery-vmware-to-azure-classic.md). Olemas on ka [pärand kogemus](site-recovery-vmware-to-azure-classic-legacy.md) häälestamise VMware dispersioonanalüüs klassikaline portaalis. See ei tohiks kasutada uut juurutuste.  Kui olete juba juurutanud, pärand kasutuskogemuse [kohta lugege](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment) täiustatud juurutusega abil.



Arhitektuuri nõuete juurutamine saidi taastamine kopeerida VMware VMs/füüsilise serverid Azure portaali või Azure klassikaline portaali (täiustatud) sarnanevad paar erinevused.

- Kui juurutate Azure'i portaalis saate salvestusruumi ressursihaldur põhinev korrata ja kasutada ressursihaldur võrkude ühendamiseks Azure VMs pärast Tõrkesiirde.
- Kui juurutate Azure'i portaalis nii LRS ja GRS salvestusruumi on toetatud. Klassikaline portaalis GRS on nõutav.
- Juurutamise protsess on liht- ja rohkem kasutajasõbralik Azure'i portaalis.


Siin on teil vaja:

- **Azure'i konto**: peate Microsoft Azure'i kontosse.
- **Azure'i salvestusruumi**: peate konto Azure storage kopeeritud andmete talletamiseks. Saate kasutada klassikaline konto või ressursihaldur salvestusruumi. Konto võib olla LRS või GRS juurutamisel Azure'i portaalis. Kopeeritud andmed on talletatud Azure salvestusruumi ja Azure VMs on kedratud üles Tõrkesiirde korral. 
- **Azure'i võrgu**: peate Azure virtuaalse võrgu, mis Azure VMs ühendatakse, kui need on loodud Tõrkesiirde. Azure'i portaalis neid saab võrkude loodud klassikaline service manager mudel või ressursihaldur mudel.
- **Kohapealse konfiguratsiooniserveri**: peate kohapealse Windows Server 2012 R2 arvuti, mis käivitatakse konfiguratsiooniserveri ja muud komponendid saidi taastamine. Kui te olete imitatsiooniga VMware VMs see peaks olema saadaval väga Vmware'i VM. Kui soovite ise füüsilise serveri seade võib olla füüsilise. Saidi taastamine järgmised komponendid on arvutisse installitud:
    - **Konfiguratsiooniserveri**: koordinaadid oma kohapealse keskkonna ja Azure suhtlemine ja haldab andmete kopeerimine ja taastamine.
    - **Protsessi server**: dispersioonanalüüs lüüsi toimib. Saab dispersioonanalüüs kaitstud allika masinad, optimeerib vahemällu, tihendamise ja krüptimise ja saadab andmed Azure salvestusruumi. Lisaks tegeleb tõuketeatised installi Mobility teenuse kaitstud masinad ja sooritab VMware VMs automaatne tuvastamine. Kui juurutamise kasvab saate lisada täiendavad eraldiseisev sihtotstarbeline protsess servers suuremal hulgal kopeerimise liikluse käsitlema.
    - **Juhtlehed target server**: tegeleb dispersioonanalüüs andmete migreerite Azure'i ajal. 
- **VMware VMs või füüsilise serveri kopeerida**: peate igas arvutis, kus soovite paljundada Azure Mobility teenuste osa installitud. See teenus salvestab andmed kirjutab arvutis ja edastab need protsessi serveriga. Selle komponendi saate käsitsi installida või saab lükata ja installitakse automaatselt protsess server kui lubate dispersioonanalüüs mõne seadme jaoks.
- **vSPhere hosts/vCenter server**: peate ühe või mitme vSphere hosti serverites töötab VMware VMs. Soovitame juurutada Vcenteri serveri hallata neid hosts.
- **Failback**: on teil vaja järgmist:
    - **Füüsilise-füüsilise failback ei toetata**: See tähendab, et kui teil ei õnnestu füüsilise serveri Azure üle ja seejärel uuesti nurjumise, peab jätate tagasi Vmware'i VM. Te ei saa ei tagasi füüsilise serveri. Peate mõne Azure VM nurjumise tagasi ja kui juurutate ei konfiguratsiooniserveri nimega Vmware'i VM peate häälestamine on eraldi serveri nimega Vmware'i VM. See on vajalik, kuna serveri suhtleb ja manustab VMware salvestusruumi soovitud ketast taastamiseks Vmware'i VM.
    - - **Ajutiste protsessi server Azure'i**: kui soovite pärast Tõrkesiirde peate häälestamine on konfigureeritud protsessi server, Azure VM dispersioonanalüüs Azure käsitlema tagasi: Azure'i nurjuda. Saate kustutada selle VM kui failback on lõpule viidud.
    - **VPN-ühendus**: peate failback VPN ühendus (või Azure'i ExpressRoute) häälestage Azure võrgust kohapealse saidile.
    - **Eraldi kohapealse juhtlehed target server**: kohapealse serveri tegeleb failback. Serveri installitud dokumendihalduse serveris vaikimisi, kuid kui teil on tagasi suuremat arvu ei peaks häälestamist on eraldi kohapealse serveri selleks.

**Üldine arhitektuur**

![Täiustatud](./media/site-recovery-components/arch-enhanced.png)

**Juurutamise komponendid**

![Täiustatud](./media/site-recovery-components/arch-enhanced2.png)

**Failback**

![Täiustatud failback](./media/site-recovery-components/enhanced-failback.png)


- [Lisateavet leiate teemast](site-recovery-vmware-to-azure.md#azure-prerequisites) Azure portaali juurutamiseks nõuete kohta.
- [Lisateavet leiate teemast](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) klassikaline portaalis täiustatud juurutamise nõuete kohta.
- [Lisateavet leiate teemast](site-recovery-failback-azure-to-vmware.md) umbes failback Auzre portaalis.
- [Lisateavet leiate teemast](site-recovery-failback-azure-to-vmware-clas- [Learn more](site-recovery-failback-azure-to-vmware-classic.md) about failback in the Auzre portal.sic.md) umbes failback klassikaline portaalis.

## <a name="replicate-to-azure-hyper-v-vms-not-managed-by-vmm"></a>Azure'i dubleeriva: Hyper-V VMs ei halda VMM

Saate ise Hyper-V VMs pole hallatavate System Center VMM Azure saidi taastamine järgmiselt:

- **Azure'i portaalis**– kui juurutate saidi taastamine võib nurjuda üle VMs klassikaline salvestusruumi või et ressursihaldur Azure'i portaalis. [Lisateavet leiate teemast](site-recovery-hyper-v-site-to-azure.md).
- **Klassikaline portaalis**-saidi taastamine klassikaline portaalis juurutamist. See juurutuse te saate ainult ei õnnestu üle VMs klassikaline salvestusruumi Azure, mitte ressursihaldur salvestusruumi. [Lisateavet leiate teemast](site-recovery-hyper-v-site-to-azure-classic.md).

Nii juurutuste arhitektuur on sarnane, kuid:

- Kui juurutate Azure'i portaalis saate korrata ressursihaldur salvestusruumi ja kasutada ressursihaldur võrkude ühendamiseks Azure VMs pärast Tõrkesiirde.
- Juurutamise protsess on liht- ja rohkem kasutajasõbralik Azure'i portaalis.

Siin on teil vaja:

- **Azure'i konto**: peate Microsoft Azure'i kontosse.
- **Azure'i salvestusruumi**: peate konto Azure storage kopeeritud andmete talletamiseks. Azure'i portaalis saate klassikaline konto või ressursihaldur salvestusruumi. Klassikaline portaalis saate kasutada ainult mõne klassikaline kontoga. Kopeeritud andmed on talletatud Azure salvestusruumi ja Azure VMs luuakse Tõrkesiirde korral.
- **Azure'i võrgu**: Azure'i võrk, mis on Azure VMs ühendatakse, kui need on loodud pärast Tõrkesiirde peate. 
- **Hyper-v server**: peate ühe või mitme Windows Server 2012 R2 Hyper-V hosti server. Saidi taastamine juurutamise käigus saate installida Azure saidi taastamise pakkuja ja Microsoft Azure taastamise teenused agendi Host.
- **Hyper-V VMs**: peate ühe või mitme VMs Hyper-V hosti server. Azure taastamise pakkujalt ja Azure taastamise teenused agendi Hyper-V Host saidi taastamine juurutamisel. Pakkuja koordinaadid ja orchestrates dispersioonanalüüs saidi taastamise teenuse Interneti kaudu. Agent tegeleb andmete kopeerimine andmete üle HTTPS 443. Suhtlus pakkuja ja agent on turvaline ja krüptitud. Kopeeritud andmete Azure storage krüptitakse ka.

**Üldine arhitektuur**

![Azure'i saidi Hyper-V](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


- [Lisateavet leiate teemast](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) Azure portaali juurutamiseks nõuete kohta.
- [Lisateavet leiate teemast](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) klassikaline portaali juurutamiseks nõuete kohta.



## <a name="replicate-to-azure-hyper-v-vms-managed-by-vmm"></a>Azure'i dubleeriva: Hyper-V VMs haldab VMM

Saate ise Hyper-V VMs VMM pilved Azure saidi taastamine järgmiselt:

- **Azure'i portaalis**– kui juurutate saidi taastamine võib nurjuda üle VMs klassikaline salvestusruumi või et ressursihaldur Azure'i portaalis. [Lisateavet leiate teemast](site-recovery-vmm-to-azure.md).
- **Klassikaline portaalis**-saidi taastamine klassikaline portaalis juurutamist. See juurutuse te saate ainult ei õnnestu üle VMs klassikaline salvestusruumi Azure, mitte ressursihaldur salvestusruumi. [Lisateavet leiate teemast](site-recovery-vmm-to-azure-classic.md).

Nii juurutuste arhitektuur on sarnane, kuid:

- Kui juurutate Azure'i portaalis saate salvestusruumi ressursihaldur põhinev korrata ja kasutada ressursihaldur võrkude ühendamiseks Azure VMs pärast Tõrkesiirde.
- Juurutamise protsess on liht- ja rohkem kasutajasõbralik Azure'i portaalis.


Siin on teil vaja:

- **Azure'i konto**: peate Microsoft Azure'i kontosse.
- **Azure'i salvestusruumi**: peate konto Azure storage kopeeritud andmete talletamiseks. Azure'i portaalis saate klassikaline konto või ressursihaldur salvestusruumi. Klassikaline portaalis saate kasutada ainult mõne klassikaline kontoga. Kopeeritud andmed on talletatud Azure salvestusruumi ja Azure VMs luuakse Tõrkesiirde korral.
- **Azure'i võrgu**: peate häälestada võrku kaardistamine nii, et kui need on loodud pärast Tõrkesiirde vastav võrguga ühendatud Azure VMs. 
- **VMM server**: saate ühe või mitme kohapealse VMM serverites, kus töötab System Center 2012 R2 vaja ja mis ühe või mitme privaatne õhupalli häälestamine. Kui soovitud Azure juurutamist portaali peate loogilise ja VM võrgu häälestamine nii, et saate konfigureerida võrgu kaardistamine. Klassikaline portaalis see pole kohustuslik.  VM võrk peaks olema seotud loogika võrk, mis on seotud pilve.
- **Hyper-v server**: peate ühe või mitme Windows Server 2012 R2 Hyper-V hosti serveri VMM pilveteenuses.
- **Hyper-V VMs**: peate ühe või mitme VMs Hyper-V hosti server.

**Üldine arhitektuur**

![Azure'i VMM](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

- [Lisateavet leiate teemast](site-recovery-vmm-to-azure.md#azure-requirements) Azure portaali juurutamiseks nõuete kohta.
- [Lisateavet leiate teemast](site-recovery-vmm-to-azure-classic.md#before-you-start) klassikaline portaali juurutamiseks nõuete kohta.




## <a name="replicate-to-a-secondary-site-vmware-virtual-machines-or-physical-servers"></a>Paljunda saidi: VMware virtuaalmasinates või füüsilise serveri 

VMware VMs või füüsilise serveri alla laaditav InMage otsima, mis on Azure saidi taastamise tellimus sisaldab saidi teise kopeerida. Saate alla laadida, Azure portaali kaudu või Azure klassikaline portaalist. 

Iga ala (konfiguratsiooni, protsessi, juhtslaidi target) komponent serverid häälestamine ja installida Agent ühendatud arvutites, mida soovite korrata. Pärast algse dispersioonanalüüs agent igas arvutis saadab delta dispersioonanalüüs muudatuste protsessi server. Protsessi serveri optimeerib andmed ja edastab selle serveri saidi. Konfiguratsiooniserveri haldab dispersioonanalüüs protsess.

Siin on teil vaja järgmist:

**Azure'i konto**: juurutamist selle stsenaariumi abil InMage otsima. Selle peate Azure tellimuse. Kui olete loonud saidi taastamise hoidla InMage otsima alla laadida ja installige uusimad värskendused juurutamise häälestamiseks.
**Protsessi server (primaarne saidi)**: häälestamine protsessi serveri komponendi esmane saidi vahemällu tihendamise ja andmete optimeerimine. Käsitlemisel ka tõuketeatised installi ühendatud agent masinad, mida soovite kaitsta. 
**VMware ESX/ESXi ja vCenter server (primaarne saidi)**: kui te olete kaitse VMware VMs peate VMware EXS/ESXi hypervisor ja soovi korral VMware vCenter server hypervisors haldamiseks.
- **VMs/füüsilise servers (esmane saidi)**: VMware VMs või Windows/Linux füüsilise serveri, mida soovite kaitsta saavad vaja installitud ühendatud Agent. Ühendatud Agent abistas serveri masinad ka installitud. Agent toimib kõik komponendid vahelise side pakkuja. 
- - **Konfiguratsiooniserveri (saidi)**: konfiguratsiooniserveri on esimese komponendi installimist ja see on saidi haldamine, konfigureerimine ja jälgida oma juurutuse, kasutades kontohalduse veebisait või vContinuum konsooli teise seadmesse installitud. On ainult üks konfiguratsiooniserveri juurutamine ja see peab olema installitud arvutis, kus töötab Windows Server 2012 R2.
- **vContinuum server (saidi)**: see on installitud samasse asukohta (saidi), nagu konfiguratsiooniserveri. Kaitstud keskkonna juhtimiseks ja pakub konsooli. Vaikimisi installi vContinuum server on esimene serveri ja ühendatud Agent installitud.
- **Juhtlehed target server (saidi)**: serveri hoiab kopeeritud andmed. Saab protsessi server, loob koopia arvutisse saidi ja hoiab andmepunktide säilitus. Juhtslaidi target serverid vajaliku arvu sõltub masinad te olete kaitsmine arv. Kui soovite, et see ei õnnestu tagasi esmane saidi peate juhtslaidi target server on liiga. 

**Üldine arhitektuur**

![Vmware'i VMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="replicate-to-a-secondary-site-hyper-v-vms-managed-by-vmm"></a>Paljunda saidi: Hyper-V VMs haldab VMM


Saate ise Hyper-V VMs, mida hallatakse System Center VMM teisene andmekeskusesse saidi taastamine järgmiselt:

- **Azure'i portaalis**– kui juurutate saidi taastamine Azure'i portaalis. [Lisateavet leiate teemast](site-recovery-hyper-v-site-to-azure.md).
- **Klassikaline portaalis**-saidi taastamine klassikaline portaalis juurutamist. [Lisateavet leiate teemast](site-recovery-hyper-v-site-to-azure-classic.md).

Nii juurutuste arhitektuur on sarnane, kuid:

- Kui juurutate peate häälestama võrgu kaardistamine Azure'i portaalis. See pole kohustuslik klassikaline portaalis.
- Juurutamise protsess on liht- ja rohkem kasutajasõbralik Azure'i portaalis.
- - Kui juurutate on Azure klassikaline portaali [salvestusruumi vastenduse](site-recovery-storage-mapping.md) on saadaval.

Siin on teil vaja:

- **Azure'i konto**: peate Microsoft Azure'i kontosse.
- **VMM server**: soovitame VMM server esmane saidi ja üks teisene saidil kumbki sisaldab vähemalt ühte VMM privaatne pilve. Server olema vähemalt opsüsteemi süsteem keskele 2012 SP1 uusimate värskendustega ja Interneti-ühendus. Pilved peaks olema Hyper-V võimalus profiili seadmine. Saate installida VMM server Azure'i saidi taastamise pakkuja. Pakkuja koordinaadid ja orchestrates dispersioonanalüüs saidi taastamise teenuse Interneti kaudu. Pakkuja ja Azure vahelise suhtluse on turvaline ja krüptitud.
- **Hyper-V server**: Hyper-V hosti serverite peaks asuma esmaseid ja teiseseid VMM pilved. Selle hosti serverid olema vähemalt opsüsteemi Windows Server 2012 uusimaid värskendusi installitud ja Interneti-ühendus. Andmed on kopeeritud esmaseid ja teiseseid Hyper-V hosti serverid üle LAN või Kerberose või serdi autentimist kasutades VPN vahel.  
- **Kaitstud masinad**: allikas Hyper-V hosti server peaks olema vähemalt üks VM, mida soovite kaitsta.

**Üldine arhitektuur**

![Kohapealse kohapealse](./media/site-recovery-components/arch-onprem-onprem.png)


- [Lisateavet leiate teemast](site-recovery-vmm-to-vmm.md#azure-prerequisites) Azure portaali juurutamise nõuete kohta.
- - [Lisateavet leiate teemast](site-recovery-vmm-to-vmm-classic.md#before-you-start) Azure klassikaline portaalis juurutamise nõuete kohta.




## <a name="replicate-to-a-secondary-site-with-san-replication-hyper-v-vms-managed-by-vmm"></a>Paljunda saidi SAN kopeerimise abil: Hyper-V VMs haldab VMM

Saate kopeerida Hyper-V VMs hallatava VMM pilved saidi Azure klassikaline portaalis SAN kopeerimise abil. Sel juhul pole praegu toetatud uue Azure'i portaalis. 

Sel ajal saidi taastamine teil installida Azure saidi taastamise pakkuja VMM serverites. Pakkuja koordinaadid ja orchestrates dispersioonanalüüs saidi taastamise teenuse Interneti kaudu. Andmed on kopeeritud vahel esmaseid ja teiseseid salvestusruumi massiivides sünkroonse SAN kopeerimise abil.

Siin on teil vaja:

**Azure'i konto**: peate Azure tellimuse
- **SAN massiiv**: on [toetatud SAN massiiv](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) haldab esmane VMM server. SAN jagab võrgu taristule teise SAN massiivi teisel saidil.
- **VMM server**: soovitame VMM server esmane saidi ja üks teisene saidil kumbki sisaldab vähemalt ühte VMM privaatne pilve. Server olema vähemalt opsüsteemi süsteem keskele 2012 SP1 uusimate värskendustega ja Interneti-ühendus. Pilved peaks olema Hyper-V võimalus profiili seadmine.
- **Hyper-V server**: Hyper-V hosti serverid asuvad esmaseid ja teiseseid VMM pilved. Selle hosti serverid olema vähemalt opsüsteemi Windows Server 2012 uusimaid värskendusi installitud ja Interneti-ühendus.
- **Kaitstud masinad**: allikas Hyper-V hosti server peaks olema vähemalt üks VM, mida soovite kaitsta.

**SAN dispersioonanalüüs arhitektuur**

![SAN dispersioonanalüüs](./media/site-recovery-components/arch-onprem-onprem-san.png)

[Lisateavet leiate teemast](site-recovery-vmm-san.md#before-you-start) juurutamise nõuete kohta.
### <a name="on-premises"></a>Kohapealse



## <a name="hyper-v-protection-lifecycle"></a>Hyper-V kaitse elutsükkel

See töövoog on kujutatud kaitsmine, imitatsiooniga ja suuda Hyper-V virtuaalmasinates üle. 

1. **Luba kaitse**: saidi taastamise hoidla häälestamine dispersioonanalüüs VMM pilveteenuses või Hyper-V saidi sätete konfigureerimine ja luba kaitse vms. Nn **Lubamine kaitse** töö käivitatakse ja **klõpsake vahekaarti** saab jälgida. Töö kontrollib, et arvuti vastab eeltingimused ja seejärel käivitab [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) meetod, mille olete konfigureerinud sätetega Azure kopeerimine. **Luba kaitse** töö käivitab ka [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) meetodit lähtestamine VM täielik koopia.
2. **Algne dispersioonanalüüs**: virtuaalse masina hetktõmmise võetakse ja virtuaalse kõvaketta on tiražeeritud ükshaaval seni, kuni kõik kopeeritakse need Azure'i või sekundaarne andmekeskuse. See läbimiseks aeg sõltub VM suurus, võrgu läbilaskevõime ja algse dispersioonanalüüs meetod. Kui ketta muudatused toimuvad algse dispersioonanalüüs ajal Hyper-V koopia Dispersioonanalüüs jälgimine jälitab neid muudatusi Hyper-V kopeerimine logid (.hrl), mis asub selle ketast samasse kausta. Iga ketta on seotud .hrl faili, mis saadetakse teisese salvestusruumi. Pange tähele, et hetktõmmise ja logige failide tarbimine ketta ressursse, algse kopeerimise ajal. Algne dispersioonanalüüs lõpulejõudmisel VM hetktõmmise kustutatakse ja delta ketta muudatuste logi on sünkroonitud ja ühendatud.
3. **Finalize kaitse**: kui algse kopeerimine on lõpule viidud **Finalize kaitse** töö konfigureerib võrgu- ja muud sätted pärast dispersioonanalüüs nii, et virtuaalse masina on kaitstud. Kui te olete imitatsiooniga Azure'i peate selleks, et see on valmis Tõrkesiirde töödelda virtuaalse masina sätteid. Selles etapis käivitada test Tõrkesiirde kontrollida, kas kõik toimib ootuspäraselt.
4. **Kopeerimine**: pärast algse dispersioonanalüüs algab delta sünkroonimise vastavalt dispersioonanalüüs sätted. 
    - **Kopeerimise tõrge**: kui delta kopeerimine nurjub, ja täielik koopia oleks kulukas läbilaskevõime või aeg, siis ilmneb resynchronization. Jaoks resynchronization märgitakse näiteks kui .hrl failide kuni 50% ketta mahtu, siis VM. Resynchronization minimeeritakse andmehulga saadetud arvutuste kontrollimisel, lähte- ja virtuaalmasinates ja saatmine ainult delta. Kui resynchronization on lõpule viidud jätkub delta kopeerimine. Vaikimisi on resynchronization ajastatud väljaspool tööaega automaatselt käivituma, kuid saate sünkroonige uuesti virtuaalse masina käsitsi.
    - **Kopeerimise tõrge**: kui ilmneb tõrge dispersioonanalüüs on sisseehitatud uuesti. Kui see on mitte-taastuv viga nagu autentimist või luba tõrge või koopia arvutisse on sobimatu oleku, siis no uuesti proovitakse. Kui see on taastuv tõrge, nt võrgu tõrge või vähese kettaruumi ruumi/mälu ja seejärel uuesti ilmneb korduskatsed vahelisel järjest (1, 2, 4, 8, 10, ja seejärel iga 30 minuti järel).
4. **Kavandatud/planeerimata failovers**: käivitada kavandatud või planeerimata failovers vastavalt vajadusele. Kui saate käivitada kavandatud Tõrkesiirde siis andmeallika VMs on suletud, et tagada kaotamata andmeid. Pärast koopia VMs luuakse nad on panna soovitud Kinnita ootele. Peate need on tõrkesiire lõpuleviimiseks kinnitamiseks, v.a juhul, kui te olete koos SAN imitatsiooniga, mille puhul on automaatne kinnitamine. Pärast esmane sait on tööks failback võib ilmneda. Kui te olete kopeeritud Azure'i tagant dispersioonanalüüs on automaatselt. Muul juhul saate võrgukoosolekuga tagant dispersioonanalüüs käsitsi.
 

![töövoo](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Järgmised sammud

[Juurutamise ettevalmistamine](site-recovery-best-practices.md)
