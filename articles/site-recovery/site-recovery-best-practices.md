<properties
    pageTitle="Saidi taastamine juurutamise ettevalmistamine | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse, kuidas saada valmis kasutama dispersioonanalüüs Azure saidi taastamine."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="prepare-for-azure-site-recovery-deployment"></a>Azure'i saidi taastamine juurutamise ettevalmistamine

Lugege seda artiklit kõrge ülevaate juurutamise nõudeid iga dispersioonanalüüs stsenaarium Azure saidi taastamise teenus ei toeta. Pärast iga stsenaarium nõuete lugemiseks saab linkida konkreetse juurutamise üksikasjad iga juurutamiseks.

Pärast lugemist artiklit postitada kommentaare või küsimusi allosas artikli või [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Ülevaade

Peavad organisatsioonid BCDR strateegia, mis määrab, kuidas rakendused, töökoormus ja andmete püsida töötab ja kättesaadav ajal plaanitud ja plaanimata tööseisakute ja taastamiseks tavaline tingimuste nii kiiresti kui võimalik. BCDR strateegia kavandamine peaks äriandmete turvalised ja saab hoida ja tagama töökoormus pidevalt katastroofi ilmnemisel. 

Saidi taastamine on Azure'i teenus, mis aitab BCDR strateegia kavandamine, orkesteroinnin dispersioonanalüüs füüsilise kohapealsed serverid ja virtuaalmasinates pilveteenusesse (Azure) või teisene andmekeskusesse. Kui katkestuste esineda teie peamine asukoht, teil ei õnnestu üle teisene asukohta hoida rakendused ja töökoormus saadaval. Te ei tagasi oma peamine asukoht kui tagastab see toiminguid. Lisateavet leiate [mis on saidi taastamise?](site-recovery-overview.md)

## <a name="select-your-deployment-model"></a>Valige mudelisse juurutamine

Azure'i on kaks eri [juurutamise mudelite](../resource-manager-deployment-model.md) loomise ja ressursside töötamine: Azure'i ressursihaldur mudel ja klassikaline teenuste haldus mudel. Azure'i on kaks portaalide – [Azure klassikaline portaali](https://manage.windowsazure.com/) klassikaline juurutamise mudeli toetava ja [Azure portaali](https://ms.portal.azure.com/) nii juurutamise mudelid tugi.

Saidi taastamine on saadaval nii klassikaline portaali ja Azure portaali. Azure'i klassikaline portaalis saate toetavad saidi taastamine mudeliga klassikaline teenuste haldus. Azure'i portaalis toetab klassikaline mudel või ressursihaldur juurutuste. [Lisateavet vt](site-recovery-overview.md#site-recovery-in-the-azure-portal) juurutamise Azure'i portaalis.

Kui olete valides juurutamise mudeli märkme mis:

- Soovitame kasutada [Azure portaali](https://portal.azure.com) ja ressursihaldur mudeli võimaluse.
- Saidi taastamine pakub lihtsam ja selgem alustamine kasutusvõimalusi: Azure'i portaalis.
- Azure portaali kasutamisel saaksite korrata masinad nii klassikaline ja Azure ressursihaldur salvestusruumi. Lisaks saate kasutada Azure portaali LRS või GRS salvestusruumi kontod.
- Azure'i portaal ühendab saidi varundus ja taaste teenuste ühe taastamise teenused vault nii, et saate häälestada ja hallata BCDR teenused ühest kohast.
- Azure'i tellimused ette valmistatud programmiga pilve lahenduse pakkuja (CSP) kasutajad saavad nüüd hallata saidi taastamise toimingute Azure'i portaalis.
- VMware VMs või füüsilise masinad imitatsiooniga Azure'i portaalis Azure pakub mitmesuguseid uusi funktsioone, sh tugi premium salvestusruumi ja võimalus teatud ketast välistatakse kopeerimise abil.


## <a name="select-your-deployment-scenario"></a>Valige oma juurutamise stsenaarium

**Juurutamine** | **Üksikasjad** | **Azure'i portaal** | **Klassikaline portaal** | **PowerShelli**
---|---|---|---|---
**Azure'i VMware VMs** | Paljundada VMware VMs Azure'i salvestusruum | Funktsiooni Azure portaali VMs võib nurjuda üle klassikaline või ressursihaldur salvestusruum<br/><br/> Juurutamise [Azure portaali](site-recovery-vmware-to-azure.md) pakub sujuv juurutamise kogemus ja funktsioonide eeliseid arvu. | Azure'i klassikaline portaalis saate juurutada [täiustatud mudeli](site-recovery-vmware-to-azure-classic.md) ja nurjuda üle klassikaline Azure salvestusruumi.<br/><br/> Olemas on ka pärand mudelit, mida ei tohiks kasutada uut juurutuste. |  PowerShelli pole praegu toetatud.
**Azure Hyper-V VMs** | Hyper-V VMs Azure'i salvestusruumi. VMs saab hallata System Center VMM pilved või ilma VMM Hyper-V hosts. | Funktsiooni Azure portaali VMs võib nurjuda üle klassikaline või ressursihaldur salvestusruumi.<br/><br/> Azure'i portaalis on täiustatud juurutamise kogemus. [Lisateavet leiate teemast](site-recovery-vmm-to-azure.md) imitatsiooniga Hyper-V VMs VMM pilved kohta. [Lisateavet leiate teemast](site-recovery-hyper-v-site-to-azure.md) imitatsiooniga Hyper-V VMs (ilma VMM) kohta.| Klassikaline Azure'i portaalis saate ei pruugi VMs üle klassikaline Azure salvestusruum | PowerShelli juurutamise on toetatud.
**Füüsilise serveri Azure** | Paljundada füüsilise Windows/Linux serveri Azure salvestusruum | Funktsiooni Azure portaali serverites võib nurjuda üle klassikaline või ressursihaldur salvestusruumi.<br/><br/> Juurutamise [Azure portaali](site-recovery-vmware-to-azure.md) pakub sujuv juurutamise kogemus ja funktsioonide eeliseid arvu. | Azure'i klassikaline portaalis saate juurutada [täiustatud mudeli](site-recovery-vmware-to-azure-classic.md) ja nurjuda üle klassikaline Azure salvestusruumi.<br/><br/> Olemas on ka pärand mudelit, mida ei tohiks kasutada uut juurutuste. | PowerShelli juurutamise pole praegu toetatud.
**Saidi VMware VMs/füüsilise serverid* | Saidi VMware VMs või füüsilise Windows/Linux serveri korrata. [Lisateavet leiate teemast ja allalaadimine](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) InMage otsima kasutusjuhendis. | Azure'i portaalis pole saadaval | Klassikaline portaalis teil laadida saidi taastamise hoidla InMage otsima. | PowerShelli juurutamise pole praegu toetatud
**Hyper-V VMs teisene saidile** | Ise Hyper-V VMs VMM pilved teisene pilveteenusesse | [Azure portaali](site-recovery-vmm-to-vmm.md) saab paljundada Hyper-V VMs VMM pilved saidi Hyper-V koopia ainult abil. SAN pole praegu toetatud. | Azure'i klassikaline portaalis saate ise Hyper-V VMs VMM pilved saidi [Hyper-V koopia](site-recovery-vmm-to-vmm-classic.md) või [SAN kopeerimise](site-recovery-vmm-san.md) abil | PowerShelli juurutamise on toetatud



## <a name="check-what-you-need-for-deployment"></a>Märkige ruut juurutamiseks peate

### <a name="replicate-to-azure"></a>Azure'i paljundada

**Nõue** | **Üksikasjad** 
---|---
**Azure'i konto** | Peate [Microsoft Azure'i](http://azure.microsoft.com/) kontosse.<br/><br/> Saate alustada [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). [Lisateavet leiate teemast](https://azure.microsoft.com/pricing/details/site-recovery/) saidi taastamine hindade kohta.
**Azure'i salvestusruum** | Kopeeritud andmed on talletatud Azure salvestusruumi ja Azure VMs luuakse Tõrkesiirde korral. Azure'i korrata, peate [Azure storage konto](../storage/storage-introduction.md).<br/><br/>Kui juurutamist saidi taastamine klassikaline portaalis peate ühe või mitme [standardse GRS salvestusruumi kontod](..storage/storage-redundancy.md#geo-redundant-storage).<br/><br/> Kui juurutamist Azure portaalis saate GRS või LRS salvestusruumi.<br/><br/> Kui te olete imitatsiooniga VMware VMs või füüsilise serverites Azure portaali premium salvestusruumi on toetatud. Pange tähele, et kui kasutate premium salvestusruumi konto peate samuti kindlad konto dispersioonanalüüs logid, mis jäädvustada poolelioleva muudatused – kohapealsete andmete talletamiseks. Tavaliselt kasutatakse [Premium salvestusruumi](../storage/storage-premium-storage.md) virtuaalmasinates, mis peab olema püsivalt kõrge IO jõudlus ja madal latentsus host IO intensiivse töökoormus abil.<br/><br/> Kui soovite kopeeritud andmete talletamiseks premium konto abil, peate samuti kindlad konto dispersioonanalüüs logid, mis jäädvustada poolelioleva muudatused – kohapealsete andmete talletamiseks.
**Azure'i võrgu** | Azure'i korrata, peate Azure võrk, mis on Azure VMs ühendatakse, kui need on loodud pärast Tõrkesiirde.<br/><br/> Kui juurutamist klassikaline portaalis saate kasutada klassikaline võrgus. Kui juurutamist Azure portaali, saate kasutada klassikaline või ressursihaldur võrku.<br/><br/> Võrgu peab olema sama piirkonna vault.
**Võrgu kaardistamine (VMM Azure)** | Kui olete imitatsiooniga VMM: Azure'i, [võrgu kaardistamine](site-recovery-network-mapping.md) tagab, et Azure VMs on ühendatud õige võrgu pärast Tõrkesiirde.<br/><br/> Saate seadistada võrgu peate konfigureerimine VM võrkude VMM portaalis.
**Kohapealse** | **VMware VMs**: peate mõne kohapealse seadme töötab saidi taastamine komponendid, VMware vSphere hosts/vCenter server ja VMs, mida soovite korrata. [Lisateavet vt](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).<br/><br/> **Füüsilise serveri**: kui te olete imitatsiooniga füüsilise serveri peate kohapealse masinad, mis töötab saidi taastamine komponendid ja füüsilise serveri, mida soovite korrata. [Lisateavet vt](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Kui soovite pärast Tõrkesiirde Azure'i [nurjuda tagasi](site-recovery-failback-azure-to-vmware.md) peate VMware taristu seda teha.<br/><br/> **Hyper-V VMs**: kui soovite paljundada Hyper-V VMs VMM pilved peate VMM server ja asuvad Hyper-V hosts, mis VMs, mida soovite kaitsta. [Lisateavet vt](site-recovery-vmm-to-azure.md#on-premises-prerequisites).<br/><br/> Kui soovite ise Hyper-V VMs ilma VMM peate Hyper-V hosts VMs asuvad. [Lisateavet vt](site-recovery-hyper-v-site-to-azure.md#on-premises-prerequisites).
**Kaitstud masinad** | Kaitstud masinad, mis on Azure ise peab vastama [Azure eeltingimused](#azure-virtual-machine-requirements) allpool kirjeldatud.


### <a name="replicate-to-a-secondary-site"></a>Saidi paljundada

**Nõue** | **Üksikasjad** 
---|---
**Azure'i konto** | Peate [Microsoft Azure'i](http://azure.microsoft.com/) kontosse.<br/><br/> Saate alustada [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). [Lisateavet leiate teemast](https://azure.microsoft.com/pricing/details/site-recovery/) saidi taastamine hindade kohta.
**Kohapealse** | **VMware VMs**: esmane saidi peate protsessi serveri vahemällu, tihendamise ja dispersioonanalüüs krüptimist enne saatmist selle saidi jaoks. Teisene saidil installimist konfiguratsiooniserveri, hallata ja jälgida juurutamise ja mõne serveri. Soovitame vContinuum serveri lihtsam juhtimine. Lisaks peate ühe või mitme Vmware'i vSphere hosts ja soovi korral vCenter server. [Lisateave](site-recovery-vmware-to-vmware.md)<br/><br/> **Hyper-V VMs VMM pilved**: peate VMM serverite häälestamine ja Hyper-V hosts, mis sisaldab VMs, mida soovite korrata. [Lisateavet vt](site-recovery-vmm-to-vmm.md#on-premises-prerequisites).
**Võrgu kaardistamine (VMM VMM)** | Kui olete imitatsiooniga kaudu VMM VMM, tagab [võrgu kaardistamine](site-recovery-network-mapping.md) koopia VMs pärast Tõrkesiirde vastav võrguga ühendatud ja paigutatakse optimaalselt Hyper-V hosts. Saate seadistada võrgu peate VMM serveritesse VM võrgu konfigureerimine.
**Salvestusruumi kaardistamine** | Kui te olete imitatsiooniga VMM VMM soovi korral saate seadistada [salvestusruumi](site-recovery-storage-mapping.md) määramiseks salvestusruumi target tiražeeritud vms. Salvestusruumi vastenduse saab seadistada nii Hyper-V koopia ja SAN kopeerimise.<br/><br/> Salvestusruumi vastenduse pole praegu toetatud Azure'i portaalis.


## <a name="verify-url-access"></a>URL-i juurdepääsu kontrollimine

Veenduge, et idest pääseb serverites.

**URL-I** | **VMM VMM** | **Azure'i VMM** | **Azure Hyper-V** | **Vmware'i Azure'i**
---|---|---|---|---
*. accesscontrol.windows.net | Luba | Luba | Luba | Luba
*. backup.windowsazure.com | Ei pea | Luba | Luba | Luba
*. hypervrecoverymanager.windowsazure.com | Luba | Luba | Luba | Luba
*. store.core.windows.net | Luba | Luba | Luba | Luba
*. blob.core.windows.net | Ei pea | Luba | Luba | Luba
https://www.msftncsi.com/NCSI.txt | Luba | Luba | Luba | Luba
https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | Ei pea | Ei pea | Ei pea | Luba

## <a name="azure-virtual-machine-requirements"></a>Azure virtuaalse masina nõuded

Saate juurutada kopeerida virtuaalmasinates ja füüsilise serverites, kus töötab mõni operatsioonisüsteem ei toeta Azure saidi taastamine. See hõlmab enamik versioone Windows ja Linux. Peate veenduge, et mis kohapealse Azure nõuetele vastavad virtuaalmasinates, mida soovite kaitsta.


**Funktsioon** | **Nõuded** | **Üksikasjad**
---|---|---
Hyper-V Server | Peaks olema opsüsteemi Windows Server 2012 R2 | Eeltingimused kontrollimine nurjub, kui toetuseta opsüsteem
VMware hypervisor  | Toetatud operatsioonisüsteem | [Märkige ruut nõuded](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment)
Külalisena operatsioonisüsteem | Hyper-V Azure'i kopeerimine: saidi taastamine toetab kõigis opsüsteemides, mis on [Azure ei toeta](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> VMware ja füüsilise serveri dispersioonanalüüs: Kontrollige, et Windows ja Linux [eeltingimused](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) | Eeltingimused kontrollimine nurjub, kui toetuseta.
Külalisena operatsioonisüsteemi arhitektuur | 64-bitine | Eeltingimused kontrollimine nurjub, kui toetuseta
Operatsioonisüsteemi ketta suurus |  1023 GB | Eeltingimused kontrollimine nurjub, kui toetuseta
Operatsioonisüsteemi ketta loendamine | 1 | Eeltingimused kontrollimine nurjub, kui toetuseta.
Andmete kettale loendamine | 16 või (suurim väärtus on selle funktsiooni loodava virtuaalse masina suuruse. 16 = XL) | Eeltingimused kontrollimine nurjub, kui toetuseta
Andmete ketta VHD suurus | Kuni 1023 GB | Eeltingimused kontrollimine nurjub, kui toetuseta
Võrguadapteri | Mitu adapterit on toetatud |
Staatiline IP-aadress | Toetatud | Kui esmane virtuaalse masina kasutab staatiline IP-aadress saate määrata Azure loodud virtuaalse masina staatiline IP-aadress. Pange tähele, et ei toeta Hyper-v töötab linux virtuaalse masina staatiline IP-aadress.
iSCSI kettalt | Pole toetatud | Eeltingimused kontrollimine nurjub, kui toetuseta
Ühiskasutuses VHD | Pole toetatud | Eeltingimused kontrollimine nurjub, kui toetuseta
FC kettale | Pole toetatud | Eeltingimused kontrollimine nurjub, kui toetuseta
Kõvaketta vormindamine| VHD <br/><br/> VHDX | Kuigi VHDX pole praegu toetatud Azure, teisendab saidi taastamine VHDX automaatselt VHD, kui teil ei õnnestu üle Azure Kui teil ei õnnestu tagasi kohapealse on virtuaalmasinates edaspidi VHDX vorming.
BitLockeri abil | Pole toetatud | Enne virtuaalse masina kaitsmine peab olema keelatud BitLockeri abil.
Virtuaalse masina nimi| 1 kuni 63 märki. Ainult tähti, numbreid ja sidekriipse. Peaks algavad ja lõppevad tähe või numbri | Värskendage väärtus virtuaalse masina atribuudid saidi taastamine
Virtuaalne seadme tüüp | <p>Genereerimine 1</p> <p>2 - Windows genereerimine</p> |  Loomine 2 virtuaalse masina OS ketta tüüpi lihtsa ketas, mis sisaldab 1 või 2 andmemahtudega ketta formaat nimega VHDX, mis on väiksem kui 300GB on toetatud. Linux genereerimine 2 virtuaalmasinates ei toetata. [Lugege lisateavet](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)



## <a name="optimizing-your-deployment"></a>Juurutamise optimeerimine

Järgmised näpunäited aitavad teil optimeerida ja Muuda juurutamise.

- **Operatsioonisüsteemi maht**: kui teil korrata virtuaalse masina Azure operatsioonisüsteemi maht peab olema väiksem kui 1 TB. Kui teil on rohkem kui see mahud saate käsitsi liigutamist paika eri ketta enne juurutamist.
- **Andmete kettale suurus**: kui te olete imitatsiooniga Azure'i saate määrata kuni 32 andmete ketast virtuaalse masina, igal versioonil kuni 1 TB. Tõhus saate kopeerida ja ei õnnestu üle virtuaalse masina ~ 32 TB.
- **Taastamise leping piirangud**: saidi taastamine saab skaala virtuaalmasinates tuhandetele. Taastamise on loodud rakendusi, mis ei suuda üle koos nii, et me masinad arvu piiramine taastamise leping 50 mudel.
- **Azure'i teenuste piirangud**: iga Azure'i tellimus sisaldab vaikimisi piirangud kogumi südamike, pilveteenused jne. Soovitame, et käivitada test Tõrkesiirde tellimuse ressursside olemasolu kinnitamiseks. Saate muuta need piirangud Azure'i tugiteenuse kaudu.
- **Võimsuse planeerimine**: Lugege [võimsuse planeerimine](site-recovery-capacity-planner.md) saidi taastamine.
- **Dispersioonanalüüs läbilaskevõime**: kui olete lühike dispersioonanalüüs läbilaskevõime pidage meeles järgmist:
    - **ExpressRoute**: Azure'i ExpressRoute ja WAN optimeerijad, nt jõesäng töötab saidi taastamine. [Lisateavet](http://blogs.technet.com/b/virtualization/archive/2014/07/20/expressroute-and-azure-site-recovery.aspx) ExpressRoute kohta.
    - **Kopeerimise liikluse**: saidi taastamise kasutab sooritab nutikas algne koopia ainult andmete plokid ja pole kogu VHD abil. Ainult muudatused on kopeeritud poolelioleva kopeerimise ajal.
    - **Võrguliikluse**: saate määrata kopeerimise kasutatavaid häälestamist [Windows QoS](https://technet.microsoft.com/library/hh967468.aspx) ja sihtkoha IP-aadress ja pordi poliitika võrguliiklust.  Lisaks, kui te olete imitatsiooniga Azure saidi taastamise agent Azure varukoopia abil saate konfigureerida pidurdamise määramine, et agent. [Lisateavet vt](https://support.microsoft.com/kb/3056159).
- **RTO**: saidi taastamine eeldatavaid eesmärk taastamise aeg (RTO) mõõtmiseks soovitame käivitate test Tõrkesiirde ja Kuva saidi taastamise töö analüüsimiseks palju aega, aega võtab need toimingud. Kui te ei suuda üle Azure, parim RTO soovitame kõik käsitsi toiminguid, integreerimine Azure automatiseerimine ja taastamise lepingud automatiseerida.
- **RPO**: saidi taastamine toetab lähedal sünkroonse taastamine punkti eesmärk (RPO), kui teil korrata Azure. See eeldab piisavalt ribalaiust andmekeskuse ja Azure vahel.


##<a name="service-urls"></a>Teenuse URL-id
Veenduge, et need URL-id juurdepääsetavat serverist


**URL-id** | **VMM VMM** | **Azure'i VMM** | **Azure'i saidi Hyper-V** | **Vmware'i Azure'i**
---|---|---|---|---
 \*. accesscontrol.windows.net | Accessi nõutav  | Accessi nõutav  | Accessi nõutav  | Accessi nõutav
 \*. backup.windowsazure.com |  | Accessi nõutav  | Accessi nõutav  | Accessi nõutav
 \*. hypervrecoverymanager.windowsazure.com | Accessi nõutav  | Accessi nõutav  | Accessi nõutav  | Accessi nõutav
 \*. store.core.windows.net | Accessi nõutav  | Accessi nõutav  | Accessi nõutav  | Accessi nõutav
 \*. blob.core.windows.net |  | Accessi nõutav  | Accessi nõutav  | Accessi nõutav
 https://www.msftncsi.com/NCSI.txt | Accessi nõutav  | Accessi nõutav  | Accessi nõutav  | Accessi nõutav
 https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | | | | Accessi nõutav


## <a name="next-steps"></a>Järgmised sammud

Õppekeskuse ja võrdlemine üldine juurutamise nõudeid pärast saate lugeda üksikasjalik eeltingimused ja alustada juurutamine iga stsenaariumi.

- [Azure'i virtuaalmasinates VMware paljundada](site-recovery-vmware-to-azure-classic.md)
- [Azure'i füüsilise serveri paljundada](site-recovery-vmware-to-azure-classic.md)
- [Paljundada VMM pilved Hyper-V server Azure'i](site-recovery-vmm-to-azure.md)
- [Paljundada Azure'i virtuaalmasinates Hyper-V (ilma VMM)](site-recovery-hyper-v-site-to-azure.md)
- [Paljundada Hyper-V VMs saidi](site-recovery-vmm-to-vmm.md)
- [Paljundada Hyper-V VMs saidi SAN abil](site-recovery-vmm-san.md)
- [Kopeerida ühe VMM server Hyper-V VMs](site-recovery-single-vmm.md)
