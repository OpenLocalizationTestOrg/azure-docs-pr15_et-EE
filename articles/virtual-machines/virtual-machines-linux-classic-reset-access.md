<properties
        pageTitle="Linux VM parooli lähtestamine ja SSH võtme CLI | Microsoft Azure'i"
        description="Kuidas kasutada VMAccess laiend: Azure'i käsurea liides (CLI) SSH võti või Linux VM parooli lähtestada, lahendada SSH konfiguratsiooni ja kontrollida ketta järjepidevuse"
        services="virtual-machines-linux"
        documentationCenter=""
        authors="cynthn"
        manager="timlt"
        editor=""
        tags="azure-service-management"/>

<tags
        ms.service="virtual-machines-linux"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="06/14/2016"
        ms.author="cynthn"/>

# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a>Kuidas SSH võti või Linux VM parooli lähtestada, lahendada SSH konfiguratsiooni ja kontrollida ketta järjepidevuse abil VMAccess laiend


Kui te ei saa Linuxi virtuaalse masina Azure tõttu unustatud parooli, vale Secure Shell (SSH) klahvi, või probleem SSH konfiguratsiooni, kasutage VMAccessForLinux laiend Azure'i CLI SSH võti ja parooli lähtestada, lahendada SSH konfiguratsiooni ja kontrollige ketta järjepidevuse. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Siit saate teada, kuidas [nende](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess)toimingute abil ressursihaldur mudeli.

Azure'i CLI, kasutate käsku **Azure'i vm laiend seada** oma käsurea liides (Bash, Terminal, Käsuviip) juurdepääsu käske. Käivitage üksikasjalikud laiendamine kasutus **azure abi vm laiend seadmine** .

Azure'i CLI, saate teha järgmisi toiminguid:

+ [Parooli lähtestamine](#pwresetcli)
+ [Lähtesta SSH võti](#sshkeyresetcli)
+ [Lähtestage parool ja SSH võti](#resetbothcli)
+ [Sudo uue kasutajakonto loomine](#createnewsudocli)
+ [Lähtesta SSH konfigureerimine](#sshconfigresetcli)
+ [Kasutaja kustutamine](#deletecli)
+ [VMAccess pikendamise oleku kuvamine](#statuscli)
+ [Märkige ruut lisatud ketast järjepidevus](#checkdisk)
+ [Lisatud ketast oma Linux VM parandamine](#repairdisk)


## <a name="prerequisites"></a>Eeltingimused

Peate tegema järgmist:

- Pead [installida Azure CLI](../xplat-cli-install.md) ja [ühenduse tellimuse](../xplat-cli-connect.md) Azure kontoga seotud ressursside kaudu.
- Määrata õige režiimi klassikaline juurutamise mudeli, tippides käsureale.
        
        azure config mode asm
        
- Kui soovite lähtestada kas üks on uus parool või SSH võtmed, kogum. Kui soovite lähtestada SSH konfiguratsiooni ei pea neid.


## <a name="pwresetcli"></a>Parooli lähtestamine

1. Looge fail nimega PrivateConf.json nende ridadega. Asendage sulgudes ja & #60; kohatäide & #62; väärtused oma andmetega.

        {
        "username":"<currentusername>",
        "password":"<newpassword>",
        "expiration":"<2016-01-01>"
        }

2. Käivitage see käsk, asendades virtual arvuti jaoks & #60; vm nime & #62; nime.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json

## <a name="sshkeyresetcli"></a>Lähtesta SSH võti

1. Looge fail nimega PrivateConf.json sisuga. Asendage sulgudes ja & #60; kohatäide & #62; väärtused oma andmetega.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>"
        }

2. Käivitage see käsk, asendades virtual arvuti jaoks & #60; vm nime & #62; nime.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Lähtestage parool nii SSH võti

1. Looge fail nimega PrivateConf.json sisuga. Asendage sulgudes ja & #60; kohatäide & #62; väärtused oma andmetega.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>",
        "password":"<newpassword>"
        }

2. Käivitage see käsk, asendades virtual arvuti jaoks & #60; vm nime & #62; nime.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="createnewsudocli"></a>Sudo uue kasutajakonto loomine

Kui olete unustanud oma kasutajanimi, saate luua uue asutusega sudo VMAccess. Sel juhul olemasoleva kasutajanime ja parooli ei muudeta.

Parooli juurdepääsuga sudo uue kasutaja loomiseks kasutada skripti [parooli lähtestamine](#pwresetcli) ja määrake uus kasutaja nimi.

Sudo uue kasutaja loomiseks SSH võtme juurdepääsu [lähtestamine SSH võti](#sshkeyresetcli) skript kasutamine ja määrake uus kasutaja nimi.

Samuti saate [Lähtesta parool ja SSH võti](#resetbothcli) luua uus kasutaja parooli nii SSH juurdepääsu.

## <a name="sshconfigresetcli"></a>Lähtesta SSH konfigureerimine

Kui SSH konfigureerimine on soovimatu olekus, võite ka kaotada juurdepääsu VM. Saate VMAccess laiend vaikeolekusse lähtestamiseks konfiguratsiooni. Selleks, peate lihtsalt määrata klahvi "reset_ssh", "TRUE". Pikendamise uuesti SSH server, avage oma VM SSH pordi ja lähtestamine SSH konfiguratsioon vaikeväärtused. Kasutajakonto (nime, parooli või SSH klahvid) ei saa muuta.

> [AZURE.NOTE] SSH konfiguratsiooni fail, mille saab reset asub /etc/ssh/sshd_config.

1. Looge fail nimega PrivateConf.json selle sisutüübiga.

        {
        "reset_ssh":"True"
        }

2. Käivitage see käsk, asendades virtual arvuti jaoks & #60; vm nime & #62; nime. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="deletecli"></a>Kasutaja kustutamine

Kui soovite logimise sisse VM otse ilma kasutajakonto kustutamine, saate selle skripti.

1. Selle sisutüübiga, asendades kasutajanimi eemaldamiseks & #60; usernametoremove & #62; PrivateConf.json nimega faili loomine. 

        {
        "remove_user":"<usernametoremove>"
        }

2. Käivitage see käsk, asendades virtual arvuti jaoks & #60; vm nime & #62; nime. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="statuscli"></a>VMAccess pikendamise oleku kuvamine

VMAccess pikendamise oleku kuvamiseks käivitage see käsk.

        azure vm extension get

## <a name="a-namecheckdiskacheck-consistency-of-added-disks"></a>< nimi = 'checkdisk' <</a>lisatud ketast järjepidevuse kontrollimine

Kõik ketast arvuti Linux virtual fsck käitamiseks peate teha järgmist:

1. Looge fail nimega PublicConf.json selle sisutüübiga. Kontrollige, kas ketas võtab on kahendväärtus, kas kontrollida ketast manustatud virtual arvuti või mitte. 

        {   
        "check_disk": "true"
        }

2. Käivitage see käsk käivitada, asendades virtual arvuti jaoks & #60; vm nime & #62; nime.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 

## <a name='repairdisk'></a>Ketast parandamine 

Ketast, mis on paigaldamiseks või mount konfiguratsiooni vead parandamiseks kasutage VMAccess laiend lähtestamiseks mount konfiguratsiooni Linux virtual teie arvutis. Asendava ketta & #60; yourdisk & #62; nime.

1. Looge fail nimega PublicConf.json selle sisutüübiga. 

        {
        "repair_disk":"true",
        "disk_name":"<yourdisk>"
        }

2. Käivitage see käsk käivitada, asendades virtual arvuti jaoks & #60; vm nime & #62; nime.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json



## <a name="next-steps"></a>Järgmised sammud

* Kui soovite kasutada Azure PowerShelli cmdlet-käskude või Azure ressursihaldur Mallid SSH võti ja parooli lähtestada, lahendada SSH konfiguratsiooni, ja kontrollige ketta järjepidevuse, vaadake [VMAccess laiend dokumentatsiooni github](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 

* [Azure'i portaalis](https://portal.azure.com) saate kasutada ka parooli lähtestamiseks või SSH võti Linux VM juurutatud klassikaline juurutamise mudeli. Praegu ei saa kasutada portaali kas sellele Linux VM juurutatud mudelis ressursihaldur juurutamise.

* Lisateavet Azure'i virtuaalmasinates VM laiendid kasutamise kohta leiate [virtuaalse masina laiendid ja funktsioonide kohta](virtual-machines-linux-extensions-features.md) .
