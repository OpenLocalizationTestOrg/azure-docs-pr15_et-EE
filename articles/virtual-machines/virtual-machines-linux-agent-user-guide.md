<properties 
    pageTitle="Linux Agent kasutusjuhendi | Microsoft Azure'i" 
    description="Siit saate teada, kuidas installida ja konfigureerida virtuaalse masina töötamise Azure struktuuri kontrolleril Linux Agent (waagent)." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



# <a name="azure-linux-agent-user-guide"></a>Azure'i Linux Agent kasutusjuhend

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>Sissejuhatus

Microsoft Azure Linux Agent (waagent) haldab Linux ja FreeBSD ettevalmistamise ja VM Azure struktuuri kontrolleril suhtlemine. See sisaldab järgmisi funktsioone Linux ja FreeBSD IaaS juurutuste:

> [AZURE.NOTE] Leiate Azure'i Linux agent [seletusfail](https://github.com/Azure/WALinuxAgent/blob/master/README.md) täiendavad üksikasjad.

* **Pildi ettevalmistamine**
  - Kasutajakonto loomine
  - SSH autentimistüüpidest konfigureerimine
  - SSH avalikud võtmed ja võtme paari kasutuselevõtt
  - Hosti nimi säte
  - Avaldamine platvormi DNS-i hosti nimi
  - Aruandlusteenuste SSH hosti võtme sõrmejälje platvormi
  - Ressursside ketta haldamine
  - Vormingu ja ressursside ketta paigaldusega
  - Vaheta ruumi konfigureerimine

* **Võrgunduse**
  - Haldab marsruudib DHCP serverites ühilduvuse parandamiseks
  - Tagab stabiilsuse kasutajaliidese võrgu nimi

* **Tuum**
  - Konfigureerib virtuaalse NUMA (tuum keelata < 2.6.37)
  - Tarbib Hyper-V entroopia /dev/random jaoks
  - Konfigureerib SCSI ajalõpud root seadme (mis võib olla remote)

* **Diagnostika**
  - Järjestikune port konsooli ümbersuunamise

* **SCVMM juurutuste**
    - Tuvastab ja bootstraps VMM agent Linuxi System Center virtuaalse masina Manager 2012 R2 keskkonnas töötamisel

* **VM laiend**
    - Annavad komponent autoriks Microsofti ja partnerite Linux VM (IaaS) lubamiseks tarkvara ja konfiguratsiooni automatiseerimine
    - Viide VM laiend [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions) rakendamine


## <a name="communication"></a>Suhtlus
Teabe liikumist platvormi agent ilmneb kahe kanali kaudu:

* Buutimine täpsel lisatud DVD IaaS juurutuste. See DVD sisaldab OVF nõuetele vastavuse konfiguratsioonifail, mis sisaldab kõiki ettevalmistamise teavet tegeliku SSH keypairs peale.
* TCP lõpp-punkti asetades REST API kasutada juurutus- ja topoloogia konfigureerimine.


## <a name="requirements"></a>Nõuded
Järgmised süsteemid on testitud ja teadaolevalt Azure Linux Agent töötamine.

> [AZURE.NOTE] Selles loendis võib erineda ametlik loendi toetatud süsteemid Microsoft Azure'i platvormil, nagu on kirjeldatud allpool: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)

* CoreOS
* CentOS 6.3 +
* Punane rolli Enterprise Linux 6,7 +
* Debian 7.0 +
* Ubuntu 12.04
* openSUSE 12.3 +
* SLES 11 SP3 +
* Oracle'i Linux 6.4 +

Muud toetatud süsteemid:

* FreeBSD 10 + (Azure Linux Agent v2.0.10 +)

Linux agent sõltub mõned süsteemi paketid selleks, et toimida:

* Python 2,6 +
* OpenSSL 1.0 +
* OpenSSH 5.3 +
* Failisüsteemi Utiliidid: sfdisk, fdisk, mkfs, parted
* Parooli Tööriistad: chpasswd sudo
* Teksti töötlemine Tööriistad: sed grep
* Võrgu Tööriistad: ip-marsruutimiseks
* Tuum toetavad UDF failisüsteemi puhul.

## <a name="installation"></a>Installimine
Installi soovitud pööret MINUTIS või DEB paketi kaudu oma jaotuse paketi hoidlasse on installimine ja täiendamine Azure Linux Agent eelistatud meetod. Kõik [kinnitatud jaotuse pakkujate](virtual-machines-linux-endorsed-distros.md) integreerida oma pilte ja hoidlate Azure'i Linux agent paketi.

Vaadake [Azure Linux Agent repo github](https://github.com/Azure/WALinuxAgent) täpsemalt installi suvandeid, nt allikast või kohandatud asukohtadesse või eesliiteid installimise dokumendid.


## <a name="command-line-options"></a>Käsurea võtmed

### <a name="flags"></a>Lipud

- Paljusõnaline: Jaarittelu määratud käsu suurendamine
- Jõusta: vahele jätta interaktiivse kinnitus osad käsud

### <a name="commands"></a>Käsud

- spikker: loetletakse toetatud käsud ja lipud.

- deprovision: katse eemaldada süsteemist ja veenduge, et see sobib uuesti ettevalmistamise. Selle toimingu kustutatud järgmist:
 * Kõik SSH võtmed (kui Provisioning.RegenerateSshHostKeyPair on konfiguratsioonifailis "y")
 * /Etc/resolv.conf nimeserveri konfigureerimine
 * Root parool /etc/shadow (kui Provisioning.DeleteRootPassword on konfiguratsioonifailis "y")
 * Vahemällu salvestatud DHCP klient liisingute
 * Lähtestab hostinime localhost.localdomain


> [AZURE.WARNING] Deprovisioning tagada, et pilt on tühi tundlikku teavet ja sobib korduvalt.


- deprovision + kasutaja: teostab kõik all - deprovision (ülal) ja ka kustutab viimase ettevalmistatud kasutajakonto (saadud /var/lib/waagent) ja seotud andmeid. See parameeter on kui tühistage ettevalmistamise pilt, mis on varem ettevalmistamise Azure'i nii, et see võib võtta ja uuesti kasutada.

- versioon: kuvatakse waagent versioon

- serialconsole: konfigureerib GRUB märkimiseks ttyS0 (esimene järjestikune port) nimega buutimine konsooli. See tagab, et tuum alglaadimisel logid järjestikune pordi saatmiseks ja silumine kättesaadavaks teha.

- Daemon: Käivita waagent daemon töötamise platvormi. See argument on määratud waagent waagent käivitamise skripti abil.

- Alustamine: käivitada waagent tausta protsess


## <a name="configuration"></a>Konfigureerimine

Konfiguratsioonifail (/ etc/waagent.conf) määrab waagent toimingud. Konfiguratsiooni näidisfaili on allpool näidatud:

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

Erinevate suvandite konfigureerimine on kirjeldatud allpool üksikasjad. Konfiguratsiooni suvandid on kolme tüüpi; Kahendmuutuja, tekstistring või täisarv. Kahendmuutujaga konfiguratsiooni suvandid saab määrata "y" või "n". Teisiti märksõna "Pole" võib kasutada mõne stringi tüüpi konfiguratsiooni kirjed kirjeldatud allpool.

**Provisioning.Enabled:**  
Tüüp: kahendmuutujaga  
Vaikimisi: y

See võimaldab kasutajal lubada või keelata agent ettevalmistamise funktsioone. Sobivad väärtused on "y" või "n". Kui ettevalmistamise on keelatud, SSH host ja kasutajale klahvid pildil säilivad ja Azure, ettevalmistamise API määratud konfiguratsiooni ignoreeritakse.

> [AZURE.NOTE] Funktsiooni `Provisioning.Enabled` parameetri vaikeväärtus on "n" Ubuntu Cloud pilte, mis ettevalmistamise pilveteenuses – käivitamise kasutamiseks.

  
**Provisioning.DeleteRootPassword:**  
Tüüp: kahendmuutujaga  
Vaikimisi: n

Kui ebausaldusväärsete käigus kustutatakse kogum, vari faili/jne/root parool.


**Provisioning.RegenerateSshHostKeyPair:**  
Tüüp: kahendmuutujaga  
Vaikimisi: y

Kui kogum, SSH hosti võtme paari (ecdsa, dsa ja rsa) kustutatakse ebausaldusväärsete kaudu/etc/ssh/ajal. Ja ühe värske võti paari luuakse.

Värske võtme paari krüptimise tüüp on konfigureeritav Provisioning.SshHostKeyPairType sisestamise teel. Pange tähele, et mõned jaotuse uuesti luua SSH võtme paari mis tahes puuduvad krüptimise tüüpi, SSH deemon taaskäivitamisel (nt pärast taaskäivitamist).


**Provisioning.SshHostKeyPairType:**  
Tüüp: String  
Vaikimisi: rsa

Seda saab seadistada krüptimise algoritmi tüüp, mis toetab SSH deemon virtual arvutisse. Tavaliselt toetatud väärtused "rsa", "dsa" ja "ecdsa". Pange tähele, et "putty.exe" Windows ei toeta "ecdsa". Jah, kui kavatsete kasutada putty.exe Windows Linux juurutusega ühenduse, kasutage "rsa" või "dsa".


**Provisioning.MonitorHostName:**  
Tüüp: kahendmuutujaga  
Vaikimisi: y

Kui määratud, waagent jälgib Linux virtuaalse masina hostname muudatuste (nagu tagastatud käsk "hostname") ja võrgu konfigureerimisel pildil selle muudatuse kajastamiseks automaatselt värskendada. Selleks, et lükata nime muutmine DNS-serverid, taaskäivitatakse võrgunduse virtual kohapeal. Selle tulemusena lühidalt vähenemist Interneti-ühendus.


**Provisioning.DecodeCustomData**  
Tüüp: kahendmuutujaga  
Vaikimisi: n

Kui määratud, waagent kuvatakse dekodeerida CustomData Base64 kaudu.


**Provisioning.ExecuteCustomData**  
Tüüp: kahendmuutujaga  
Vaikimisi: n

Kui määratud, waagent täitma CustomData pärast ettevalmistamise.


**Provisioning.PasswordCryptId**  
Tüüp: String  
Vaikimisi: 6

Algoritmi crypt parooli räsi loomisel kasutada.  
 1 - MD5  
 2a - Blowfish  
 5 - SHA 256  
 6 - SHA 512  


**Provisioning.PasswordCryptSaltLength**  
Tüüp: String  
Vaikimisi: 10

Pikkus juhusliku soola parooli räsi loomisel kasutada.


**ResourceDisk.Format:**  
Tüüp: kahendmuutujaga  
Vaikimisi: y

Kui määratud, ressursside ketas platvormi esitatud vormindatud ja paigaldatud waagent, kui kasutaja "ResourceDisk.Filesystem" sisse nõutud failisüsteemi tüüp on midagi muud kui "ntfs". Ühe sektsiooni tüüpi Linux (83) tehakse kettal. Pange tähele, et selle sektsiooni ei vormindatakse kui see edukalt.


**ResourceDisk.Filesystem:**  
Tüüp: String  
Vaikimisi: ext4

Seda määrab ressursside ketta failisüsteemi tüüp. Toetatud väärtused erineda Linuxi jaotuse. Kui string on X, siis mkfs. X peaks olema Linux pildi kohal. SLES 11 pilte kasutada tavaliselt "ext3". FreeBSD pilte kasutada 'ufs2' siin.


**ResourceDisk.MountPoint:**  
Tüüp: String  
Vaikimisi: / mnt/ressurss 

Seda määrab tee, millega on ühendatud ressursside ketas. Pange tähele, et ressursi ketas on *ajutine* ketas tühjendatakse VM on eemaldatud.


**ResourceDisk.MountOptions**  
Tüüp: String  
Vaikimisi: pole

Saate määrata ketta mount suvandid edastatavate ühendamine -o käsu. See on komaga eraldatud väärtuste loendi. "nodev nosuid". Vaadake lisateavet mount(8).


**ResourceDisk.EnableSwap:**  
Tüüp: kahendmuutujaga  
Vaikimisi: n

Kui määratud, Vaheta faili (/ i saalefailile) luuakse kettal ressursside ja lisatakse süsteemi Vaheta ruumi.


**ResourceDisk.SwapSizeMB:**  
Tüüp: täisarv  
Vaikimisi: 0

Vaheta faili (MB) suurust.


**Logs.Verbose:**  
Tüüp: kahendmuutujaga  
Vaikimisi: n

Kui kogum, log Jaarittelu on võimendada. Waagent logib /var/log/waagent.log ja mõjutab süsteemi logrotate funktsioonid pööramiseks logid.


**OS. EnableRDMA**  
Tüüp: kahendmuutujaga  
Vaikimisi: n

Kui määratud, agent proovib installige ja laadige siis RDMA tuum draiveri, mis vastab aluseks oleva riistvara püsivara versiooni.


**OS. RootDeviceScsiTimeout:**  
Tüüp: täisarv  
Vaikimisi: 300

See konfigureerib SCSI ajalõpp sekundites OS kettale ja andmete kaudu. Kui ei vasta määratud, kasutatakse vaikesätted süsteem.


**OS. OpensslPath:**  
Tüüp: String  
Vaikimisi: pole

Seda saab kasutada mõne muu tee jaoks kasutada cryptographic toimingute kahendarvu openssl määramiseks.


**HttpProxy.Host HttpProxy.Port**  
Tüüp: String  
Vaikimisi: pole

Kui seadmiseks agent kasutavad puhverserveri juurdepääsuks Interneti-ühendus. 


## <a name="ubuntu-cloud-images"></a>Ubuntu Cloud pildid

Pange tähele, et Ubuntu Cloud pilte kasutada teha paljusid konfiguratsiooni toiminguid, mis muidu haldab Azure Linux Agent [pilveteenuses – käivitamise](https://launchpad.net/ubuntu/+source/cloud-init) .  Võtke arvesse järgmisi erinevusi.


- **Provisioning.Enabled** vaikimisi "n" Ubuntu Cloud pilte, mis ettevalmistamise ülesannete pilveteenuses – käivitamise abil.

- Konfiguratsiooni järgmisi ei mõjuta Ubuntu Cloud pilte, mis on pilv – käivitamise abil saate hallata ressursside kettale ja Vaheta ruumi:

 - **ResourceDisk.Format**
 - **ResourceDisk.Filesystem**
 - **ResourceDisk.MountPoint**
 - **ResourceDisk.EnableSwap**
 - **ResourceDisk.SwapSizeMB**

- Lugege konfigureerimine ühenduspunkti ressursi kettale ja Vaheta ruumi Ubuntu Cloud piltide ajal ettevalmistamise järgmistest allikatest:

 - [Ubuntu viki: Konfigureerimine Vaheta sektsioonid](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
 - [Kohandatud andmed on Azure virtuaalse masina süstimist](virtual-machines-windows-classic-inject-custom-data.md)

