<properties
   pageTitle="Sissejuhatus FreeBSD Azure | Microsoft Azure'i"
   description="Lugege teemat Azure FreeBSD virtuaalmasinates"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/27/2016"
   ms.author="kyliel"/>

# <a name="introduction-to-freebsd-on-azure"></a>FreeBSD Azure tutvustus
Selles teemas antakse ülevaade töötab FreeBSD virtuaalse masina Azure.

## <a name="overview"></a>Ülevaade
FreeBSD Microsoft Azure on täpsemalt arvuti operatsioonisüsteem power tänapäevane serverid, lauaarvutid, kasutatud ja manustatud platvormid. FreeBSD 10.3 pilt on esitatud Microsoft Corporation ja on saadaval Azure. See põhineb FreeBSD 10.3 väljaanne ja Azure VM külaline Agent [2.1.4](https://github.com/Azure/WALinuxAgent/releases/tag/v2.1.4) on installitud. Agent vastutab suhtlemine FreeBSD VM ja Azure struktuuri toimingute, nt ettevalmistamise VM esmakordsel kasutamisel (kasutajanimi, parool, hostinimi jne) ja lubada funktsioonid valikuline VM laiendid.
Tulevaste versioonide FreeBSD, nagu on strateegia olla kursis asjakohaste ja uusimate kättesaadavaks varsti pärast seda, kui nad on vabastatud FreeBSD väljaanne engineering meeskond. Eelseisvate vabastamist on [FreeBSD 11](https://www.freebsd.org/releases/11.0R/schedule.html).

## <a name="deploying-a-freebsd-virtual-machine"></a>FreeBSD virtuaalse masina juurutamine
Juurutamine FreeBSD virtuaalse masina on lihtne protsess pilt [Azure'i turuplatsi](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)abil.

## <a name="vm-extensions-for-freebsd"></a>FreeBSD VM laiendid
Järgnevalt on toetatud VM laiendid FreeBSD.

### <a name="vmaccess"></a>VMAccess

[VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) laiend teha järgmist.

- Algne sudo kasutaja parooli lähtestamine.
- Loo uus sudo kasutaja määratud parool.
- Võti antud avaliku hosti võtme seadmiseks.
- Lähtesta ajal VM ettevalmistamise kui ei esitata hosti võtme host avalik võti.
- Avage SSH pordi (22) ja taastada selle sshd_config, kui reset_ssh on seatud väärtusele tõene.
- Olemasoleva kasutaja eemaldamine.
- Kontrollige ketast.
- Parandage on lisatud ketas.

### <a name="customscript"></a>CustomScript

[CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) laiend teha järgmist.

- Kui see on esitatud alla laadida kohandatud skriptide Azure Storage või välise avaliku salvestusruumi (nt GitHub).
- Käivitage skript kirje punkti.
- Toeta Tekstisisese käsud.
- Windowsi-laadi newline shell ja Python skriptide automaatselt teisendada.
- Eemaldage komplekti shell ja Python skriptide automaatselt.
- Kaitsta tundlikku andmete CommandToExecute.

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Autentimine: kasutajanimed, paroolid ja SSH võtmed
Kui loote FreeBSD virtuaalse masina Azure portaali kaudu, peate sisestama kasutajanimi, parool või SSH avalik võti.
Kasutajanimed juurutamiseks FreeBSD virtuaalse masina Azure peab ei vasta süsteemi kontod nimed (UID < 100) juba olemas virtuaalse masina ("juures" näiteks).
Praegu on toetatud ainult RSA SSH võti. Mitmerealise SSH võti peab "---ALUSTAGE SSH2 kohta avalik võti---" alguses ja lõpus "---END SSH2 kohta avalik võti---".

## <a name="obtaining-superuser-privileges"></a>Saada superkasutaja õigusi
Azure virtuaalse masina eksemplari juurutamisel määratud kasutajakonto oleks õigustega kontot. Sudo paketi installiti avaldatud FreeBSD pilt.
Kui olete selle kasutaja konto kaudu sisse logitud, saate käivitada käsud root käsu süntaksi abil.

    # sudo <COMMAND>

Soovi korral saate hankida juurkausta shell sudo -s abil.

## <a name="next-steps"></a>Järgmised sammud
- Minge [Azure'i turuplatsi](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/) luua FreeBSD VM.
- Kui soovite tuua oma FreeBSD Azure, vaadake [loomine ja laadi FreeBSD VHD Azure](../virtual-machines-linux-classic-freebsd-create-upload-vhd.md).
