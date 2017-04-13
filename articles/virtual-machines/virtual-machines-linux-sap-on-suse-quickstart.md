<properties
   pageTitle="SAP NetWeaver sisse Microsoft Azure'i SUSE Linux VMs testimine | Microsoft Azure'i"
   description="Klõpsake Microsoft Azure'i SUSE Linux VMs SAP NetWeaver testimine"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Töötava SAP NetWeaver Microsoft Azure'i SUSE Linux VMs kohta


Selles artiklis kirjeldatakse mitmesuguste asjad, mida arvesse võtta, kui teil on SAP NetWeaver Microsoft Azure'i SUSE Linux virtuaalmasinates (VMs). Seisuga mai 19 2016 on SAP NetWeaver toeta SUSE Linux VMs Azure kohta. Kõik üksikasjad Linuxi versioonid, SAP-i tuum versioonid ja jne SAP-i märge 1928533 leiate "SAP-i rakenduste Azure: toetatud tooted ja Azure VM tüüpi".
Edasise dokumentatsiooni kohta SAP-i Linux VMs kohta leiate siit: [Abil SAP-i kohta Linux virtuaalmasinates (VMs)](virtual-machines-linux-sap-get-started.md).


Järgmine teave aitavad vältida potentsiaalseid probleeme osa.

## <a name="suse-images-on-azure-for-running-sap"></a>SUSE piltide Azure töötab SAP-i

SAP NetWeaver sõitmiseks Azure'i kasutada ainult SUSE Linux Enterprise Server SLES 12 (SPx) – vt ka SAP-i lisa 1928533. Teisiti SUSE pilt on Azure'i turuplatsi ("SLES 11 SP3 SAP-i CAL"), kuid see ei ole mõeldud kasutuseks. Kasutage selle pildi, kuna see on mõeldud [SAP-i Cloud seadme teegi](https://cal.sap.com/) lahendus.  

Kasutage Azure'i ressursihaldur kõigi uute katsete ja Azure installe. Vaadata SUSE SLES pilte ja versioonid Azure PowerShelli või Azure käsurea liides (CLI), saate kasutada järgmisi käske. Seejärel saate väljund, näiteks määratleda OS pilt JSON malli uue SUSE Linux VM kasutamise kohta.
Need PowerShelli käsud on kehtiv Azure PowerShelli versioon 1.0.1 ja uuemad versioonid.

* Vaadake olemasoleva tootjad, sh SUSE.

   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```

* Otsige SUSE olemasoleva pakkumisi.

   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```

* Otsige SUSE SLES pakkumisi.

   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   CLI : azure vm image list-skus westeurope SUSE SLES
   ```

* Otsige teatud versiooni SLES SKU-ga.

   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12"
   CLI : azure vm image list westeurope SUSE SLES 12
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>Installimise WALinuxAgent SUSE VM

Nimega WALinuxAgent on osa: Azure'i turuplatsi SLES pilte. See käsitsi (nt SLES OS virtuaalse kõvaketta (VHD) üleslaadimisel kohapealse) installimise kohta leiate artiklist:

- [OpenSUSE] (http://software.opensuse.org/package/WALinuxAgent)

- [Azure'i] (virtuaalne-masinad-linux-kinnitatud-distros.md)

- [SUSE] (https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP-i "põhjalikuma"

SAP-i "põhjalikuma" on kohustuslikud nõutav Azure SAP-i käivitamiseks. Kontrollige üksikasjad SAP-i Märkus 2191498 "SAP-i kohta Linux Azure: täiustatud jälgimise".

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Manustamise Azure'i andmed ketast on Azure Linux VM

Peaksite kunagi mount Azure'i andmed ketast on Azure Linux VM abil seadme ID-ga. Kasutage selle asemel üldiselt ainuidentifikaator (UUID). Olge graafiline tööriistad mount Azure'i andmed ketast, näiteks kasutamisel. Kontrollige, kas /etc/fstab sissekandeid.

Seadme ID-ga probleem on see võib muutuda, ja seejärel Azure VM võib hanguda buutimine käigus. Probleemi leevendada, võib /etc/fstab lisada nofail parameeter. Kuid olge nofail Kuna rakenduste võib kasutada ühenduspunkti nagu enne ja võib kirjutada juurkausta failisüsteemi juhuks, kui on Azure välisandmete kettale ei olnud paigaldatud ajal kuvatakse buutimine.

Paigutuse kaudu UUID erandiks on manustamise on OS ketta tõrkeotsingu eesmärgil, nagu on kirjeldatud järgmises jaotises.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>SUSE VM, mis pole enam juurdepääsetav tõrkeotsing

Võib olla olukordades, kus SUSE VM Azure hangub buutimine protsessi (nt tõrkega seotud paigaldus ketast). Azure'i Virtuaalmasinates v2 Azure'i portaalis buutimine diagnostika funktsiooni abil saate kontrollida selle probleemi. Lisateavet leiate teemast [buutimine diagnostika] (https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

OS ketta kahjustatud VM manustamiseks teise SUSE VM Azure on üks viis, kuidas probleemi lahendada. Tehke vajalikud muudatused /etc/fstab redigeerimine või eemaldamine võrgu udev reeglid, nagu järgmises jaotises kirjeldatud.

On üks oluline silmas pidada. Mitme SUSE VMs sama Azure'i turuplatsi pilt (nt SP4 SLES 11) juurutamine põhjustab alati ühendada sama UUID OS kettale. Seetõttu on UUID abil manustamine on OS kettale, kasutades sama Azure'i turuplatsi pilt juurutatud erinevate VM tulemuseks kaks identse neis. See põhjustab probleeme ja saanud tähendab, et mõeldud tõrkeotsingu VM on tegelikult käivitada manustatud ja kahjustatud OS ketta asemel algsest.

On kaks võimalust selle vältimiseks.

* Azure'i turuplatsi pilti kasutada tõrkeotsingu VM (nt SLES 11 SPx asemel SLES 12).
* Ei manustada kahjustatud OS ketta teise VM abil UUID--kasutamine midagi muud.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>SUSE VM asutusesisesest Azure'i üleslaadimine

Vajalikud toimingud, et laadida SUSE VM asutusesisesest Azure kirjelduse, vt [ettevalmistamine SLES või openSUSE virtuaalse masina Azure] (virtual-machines-linux-suse-create-upload-vhd.md).

Kui soovite üles laadida ilma deprovision etappi lõpus VM (näiteks Säilita olemasolevat SAP-i installi, samuti hosti nimi), kontrollige järgmist.

* Veenduge, kas OS ketas töötab, kasutades UUID ja seadme ID. Ainult rakenduses /etc/fstab UUID muutmise ei piisa OS ketas. Samuti, ärge unustage kohandada buutimine laadur või redigeerimise /boot/grub/menu.lst YaST kaudu.
* Kui kasutate VHDX vorming SUSE OS ketta ja teisendamine VHD üleslaaditava Azure, on tõenäoline, et võrgu seade muutub eth0 eth1. Kui olete käivitamine Azure hiljem probleemide vältimiseks muuta tagasi eth0 kirjeldatud [probleemide eth0 kloonitud SLES 11 VMware] (https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Lisaks, mida on kirjeldatud artiklis, soovitame teil eemaldada see:

   /lib/udev/rules.d/75-Persistent-net-Generator.rules

Samuti saate installida Azure Linux Agent (waagent) aitavad vältida võimalikke probleeme, kui mitu NICs pole.

## <a name="deploying-a-suse-vm-on-azure"></a>SUSE VM Azure juurutamine

Uue SUSE VMs peaks loote uue Azure ressursihaldur mudeli JSON mallifailid abil. JSON mallifail on loodud, saate juurutada VM, kasutades PowerShelli alternatiivina CLI järgmine käsk:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
JSON mallifailid kohta leiate lisateavet teemast [loome Azure'i ressursihaldur Mallid] (ressursi-rühma loome-templates.md) ja [Azure Kiirjuhend Mallid] (https://azure.microsoft.com/documentation/templates/).

CLI ja Azure ressursihaldur kohta leiate lisateavet teemast [kasutamine Azure CLI Mac, Linux ja Windows Azure'i ressursihaldur] (xplat-cli-azure-ressursside-manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP-i litsents ja riistvara võti

SAP-Azure'i ametliku kinnitamise, eest võeti kasutusele uue süsteemi arvutada SAP-i riistvara võti, mida kasutatakse SAP-i litsentsi. SAP-i tuum oli kohandada, et seda kasutada. Endise SAP-i tuum versioonide Linuxi pole kaasanud selle muudatuse kood. Seetõttu teatud juhtudel (nt Azure VM suuruse muutmine), SAP-i riistvara võti muutnud ja SAP-i litsentsi oli enam ei kehti. See on lahendatud uusima SAP-i Linuxi kernelites. Üksikasju vaadake SAP-i Märkus 1928533.

## <a name="suse-sapconf-package--tuned-adm"></a>SUSE sapconf paketi / häälestatud adm

SUSE pakub paketi nimega "sapconf", mida haldab kogumi SAP-i sätted. Täpsemat teavet selle paketi mida ja kuidas installida ja kasutada, leiate teemast [sapconf abil valmistada SUSE Linux Enterprise serveri SAP-i süsteemide] (https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) ja [mis on sapconf või kuidas ette valmistada SUSE Linux Enterprise Server töötab SAP-i süsteemide?] (http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

Vahepeal on uue tööriista, mis asendab sapconf - häälestatud adm. Üks leiate täpsemat teavet selles tööriistas jälgimise kaks saamiseks allpool olevaid linke.

SLES dokumentatsiooni häälestatud adm profiili sap-hana kohta leiate [siit](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

SAP-i töökoormus koos häälestatud adm - häälestamise süsteemide leiate [siit](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) peatüki 6.2


## <a name="nfs-share-in-distributed-sap-installations"></a>NFS ühiskasutus jaotatud SAP-i installid

Kui teil on jaotatud installimise – näiteks, kus soovite andmebaasi ja SAP-i serverid rakenduse installimiseks eraldi VMs--saate jagada /sapmnt kataloogi kaudu Network File System (NFS). Kui teil on probleeme installimise juhiseid, kui olete loonud /sapmnt jaoks NFS osa, kontrollige, kui "no_root_squash" on seatud ühiskasutus.

## <a name="logical-volumes"></a>Loogiline mahud

Varem vajadusel üks loogiline suuremahulise üle mitme Azure'i andmed ketast (näiteks SAP-i andmebaas), on soovitatav kasutada mdadm lvm ei olnud täielikult veel Azure'i kinnitatud. Saate teada, kuidas häälestada Linux RAID Azure mdadm abil, vt [konfigureerimine tarkvara RAID Linux](virtual-machines-linux-configure-raid.md). Vahepeal mai 2016 alguse seisuga ka lvm Azure on täielikult toetatud ja seda saab kasutada alternatiivina mdadm. Vt lisateavet seoses lvm Azure [Konfigureerimine LVM Linux VM Azure](virtual-machines-linux-configure-lvm.md).


## <a name="azure-suse-repository"></a>Azure'i SUSE hoidla

Kui teil on probleeme juurdepääsuga standard Azure'i SUSE hoidlasse, saate lihtsa käsuga lähtestage. See võib juhtuda siis, kui loote privaatne OS pilt Azure piirkonnas ja seejärel kopeerige pilt muule alale, kuhu soovite uue VMs, mis põhinevad privaatne OS pilt juurutamine. Lihtsalt käivitage sees VM järgmine käsk:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>GNOME töölaua

Kui soovite kasutada Gnome töölaua installimiseks valmis SAP-i demo süsteemi sees ühe VM--SAP-i GUI, sh brauseri ja SAP-i halduskonsooli--abil vihje selle installida Azure SLES pildid:

   Jaoks SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   Jaoks SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>SAP-i tugi Oracle'i Linux pilveteenuses

On tugi piirang Oracle'i Linux virtualiseeritud keskkonnas. Kuigi see pole Azure'i seotud teema, on oluline mõista. SAP-i ei toeta Oracle'i SUSE või punane rolli nagu Azure'i avaliku pilveteenuses. Selles teemas käsitletakse, pöörduge otse Oracle'i.
