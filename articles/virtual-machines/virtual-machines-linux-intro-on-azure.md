<properties
    pageTitle="Sissejuhatus Linux Azure | Microsoft Azure'i"
    description="Lugege teemat Azure Linuxi."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

#<a name="introduction-to-linux-on-azure"></a>Azure'i Linuxi tutvustus

Selles teemas antakse ülevaade mõne aspektide abil Linuxi Azure'i pilves. Juurutamine Linux virtuaalse masina on lihtne protsess pilt Galerii kaudu.


## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Autentimine: Kasutajanimed, paroolid ja SSH võtmed

Azure'i klassikaline portaalis Linux virtuaalse masina loomisel palutakse sisestada kasutajanimi, parool või mõne SSH avalik võti. Kehtivad järgmised piirang on kasutajanimi Linux virtuaalse masina Azure kasutamise kohta: süsteemi kontod nimed (UID < 100) juba olemas virtuaalse masina on lubatud, root' näiteks.


 - Vaadake [luua töötab Linux](virtual-machines-linux-quick-create-cli.md)
 - Vaadake, [Kuidas kasutada SSH Linux Azure](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="obtaining-superuser-privileges-using-sudo"></a>Saada superkasutaja õigusi abil`sudo`

Azure virtuaalse masina eksemplari juurutamisel määratud kasutajakonto oleks õigustega kontot. See konto on konfigureeritud Azure Linux agent saama tõsta juursait (superkasutaja konto) kasutamise õigusi selle `sudo` kasuliku. Kui selle kasutajakontoga sisse loginud, siis saab käivitada käsud root käsu süntaksi kasutamine

    # sudo <COMMAND>

Soovi korral saate hankida root shell, kasutades **sudo -s**.

- Vt [kaudu administraatoriõigusi Linux virtuaalmasinates Azure kohta](virtual-machines-linux-use-root-privileges.md)


## <a name="firewall-configuration"></a>Tulemüüri konfigureerimine

Azure'i pakub sissetuleva paketi filter, mis piirab ühenduvuse pordid määratud Azure klassikaline portaalis. Vaikimisi on lubatud ainult pordi SSH. Saate avada juurdepääsu täiendavad pordid Linux virtual arvutis konfigureerimisega Azure klassikaline portaalis lõpp-punktid:

 - Vaadake teemat: [Kuidas luua virtuaalse masina lõpp-punktid](virtual-machines-windows-classic-setup-endpoints.md)

Azure'i galeriis Linux pildid ei luba *iptables* tulemüüri vaikimisi. Soovi korral võib esitada täiendavaid filtreerimine konfigureeritud tulemüüri.


## <a name="hostname-changes"></a>Hostname (hostinimi) muudatused

Pildi Linux eksemplari algselt juurutamisel peavad virtuaalse masina hosti nimi. Kui virtuaalse masina töötab, see hostname on avaldatud platvormi DNS-serverid nii, et mitu omavahel ühendatud virtuaalmasinates saab teha IP address otsingud hostinimed abil.

Kui hostname muudatused on soovitud pärast virtuaalse masina kasutusele võetud, kasutage käsku

    # sudo hostname <newname>

Azure'i Linux Agent sisaldab funktsiooni tuvasta automaatselt selle nime muutmine ja konfigureerimiseks õigesti virtuaalse masina jää püsima selle muudatuse ja avaldada selle muudatuse platvormi DNS-serverid.

 - [Azure'i Linux Agent kasutusjuhend](virtual-machines-linux-agent-user-guide.md)

### <a name="cloud-init"></a>Pilveteenuse – käivitamise
**Ubuntu** ja **CoreOS** pilte kasutada Azure, mis pakub lisavõimalusi eellaadimisel virtuaalse masina pilveteenuses – käivitamise.

 - [Kuidas sisestab kohandatud andmed](virtual-machines-windows-classic-inject-custom-data.md)
 - [Kohandatud andmed ja pilveteenuse – käivitamise Microsoft Azure](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [Azure'i Vaheta sektsioonid pilveteenuses – käivitamise abil loomine](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [Azure'i CoreOS kasutamine](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="virtual-machine-image-capture"></a>Virtuaalse masina kujutise salvestamine

Azure'i pakub võimalus jäädvustada mõne olemasoleva virtuaalse masina olek pildiks, mida saab kasutada hiljem juurutamiseks täiendavad virtuaalse masina eksemplarid. Azure'i Linux Agent võib osa kohandamine, mis on sooritatud ebausaldusväärsete tagasivõtmisel kasutada. Võib-olla tehke jäädvustada pildina virtuaalse masina alltoodud juhiseid.

1. Käivitage **waagent-deprovision** tagasivõtmiseks ettevalmistamise kohandamine. Või **waagent-deprovision + kasutaja** soovi korral saate kustutada kasutajakonto määratud ajal ettevalmistamise ja kõik seotud andmed.

2. Virtuaalse masina alla ja lülitage välja.

3. Klõpsake *jäädvustada* Azure klassikaline portaalis või PowerShelli või CLI tööriistade abil saate jäädvustada pildina virtuaalse masina.

 - Vaadake teemat [kuidas jäädvustada Linux virtuaalse masina mallina kasutada](virtual-machines-linux-classic-capture-image.md)


## <a name="attaching-disks"></a>Manustamise ketast

Iga virtuaalse masina on ajutine, kohaliku *Ressursi ketta* manustatud. Kuna ressursi asuvad andmed ei pruugi kogu taaskäivitamisega püsival, seda kasutatakse sageli rakendused ja protsessid tööle, virtuaalse masina andmete ajutised ja **ajutine** säilitamine. Seda kasutatakse lehe salvestamine või Vaheta faile operatsioonisüsteemi.

Linux, ressursside ketas on tavaliselt haldab Azure Linux Agent ja automaatselt paigaldatud **/mnt/resource** (või **/mnt/mnt** Ubuntu pildid).


>[AZURE.NOTE] Tähele, et ressursi ketas on **ajutine** ketas, ja võivad olla kustutatud ja ümber kujundatud kui VM taaskäivitatakse.

Linux võib nimega andmed kettale, mis salvestab `/dev/sdc`, ning kasutajad peavad partition, vormindamine ja mount selle ressursi. See hõlmab üksikasjalike õpetuses: [Kuidas lisada andmed kettale virtuaalse masina](virtual-machines-linux-classic-attach-disk.md).

 - **Vt ka:** [Tarkvara RAID Linuxi konfigureerimine](virtual-machines-linux-configure-raid.md)  &  [LVM Linuxi VM Azure konfigureerimine](virtual-machines-linux-configure-lvm.md)

