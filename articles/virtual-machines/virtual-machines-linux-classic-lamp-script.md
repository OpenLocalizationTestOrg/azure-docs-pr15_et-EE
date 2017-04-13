<properties
    pageTitle="CustomScript laiend kasutamine Linux VM | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada CustomScript laiendamine juurutada rakendused sisse Linux Virtuaalmasinates Azure klassikaline juurutamise mudeli abil loodud."
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    services="virtual-machines-linux"
    authors="gbowerman"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="multiple"
    ms.tgt_pltfrm="linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

#<a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Azure'i CustomScript laiend kasutamise Linux LAMP minirakenduse juurutamine#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Microsoft Azure'i CustomScript laiend Linux võimaldab kohandada oma virtuaalmasinates (VM) käivitades suvalise koodi mistahes skriptimine keeles (nt Python ja Bash) VM ei toeta. See võimaldab väga paindlik rakenduse juurutamine mitme masinad automatiseerimiseks.

CustomScript laiendamine Azure klassikaline portaali, Windows PowerShelli või Azure käsurea liides (Azure'i CLI) abil saate juurutada.

Selles artiklis kasutame klassikaline juurutamise mudeli abil loodud Azure'i CLI juurutamiseks lihtne LAMP rakendus on Ubuntu VM.

## <a name="prerequisites"></a>Eeltingimused

Selle näite puhul esmalt luua kaks Azure VMs Ubuntu 14.04 või uuema versiooni. VMs nimetatakse *skripti-vm* ja *lamp-vm*. Kasutage kordumatute nimede panemine VMs loomisel. Üks kasutatakse CLI käsud ja üks kasutatakse LAMP rakenduse juurutamine.

Samuti vajate Azure Storage konto ja võtit (saad see klassikaline Azure portaalist).

Kui vajate abi loomise Linux VMs Azure viidata [loomine virtuaalse masina töötab Linux](virtual-machines-linux-classic-createportal.md).

Installi käsud endale Ubuntu, kuid saate kohandada mis tahes toetatud NFS install.

Skripti-vm VM peab olema installitud koos töötamine ühenduse Azure'i Azure CLI. Abi saamiseks vaadake [installida](../xplat-cli-install.md)ja konfigureerida Azure käsurea liides.

## <a name="upload-a-script"></a>Skripti üleslaadimine

Kasutame CustomScript laiend skripti käivitada remote VM installida LAMP virnas ja PHP lehe loomine. Selleks, et kasutada igalt skript me laaditakse selle nimega on Azure'i bloobimälu.

### <a name="script-overview"></a>Skripti ülevaade

Skripti näide installid LAMP virnas Ubuntu (sh häälestamise vaikne installimine MySQL), kirjutab lihtsa PHP faili ja käivitab Apache.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Skripti üleslaadimine

Skripti salvestamine tekstifaili, näiteks *install_lamp.sh*, ja seejärel laadige see Azure Storage. Saate teha seda hõlpsalt Azure'i CLI. Järgmises näites lisatud faili salvestusruumi container, nimega "skriptide". Kui ümbris pole, peate esmalt looma.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Luua ka JSON fail, mis kirjeldab, kuidas alla laadida skripti Azure Storage. Selle salvestada *public_config.json* ("mystorage" asendamine oma salvestusruumi konto nimi):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>Pikendamise juurutamine

Nüüd saate järgmise käsu juurutada Linux CustomScript laiend remote VM Azure CLI abil.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

Eelmise käsu allalaaditavate failide ja töötab *install_lamp.sh* skripti nimega *lamp-vm*VM.

Kuna rakendus sisaldab veebiserverile, pidage meeles, et avada järgmise käsu abil HTTP kuulamise port remote VM.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Jälgimine ja tõrkeotsing

Kontrollige kui hästi kohandatud skript käivitub vaadates logifaili remote VM. SSH *lamp-vm* ja saba logifaili järgmise käsu abil.

    cd /var/log/azure/customscript
    tail -f handler.log

Kui käivitate CustomScript laiend, saate sirvida loodud teavet PHP lehele. Selles artiklis näiteks PHP leht on *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Lisaressursid

Saate juurutada keerukamate rakenduste samu juhiseid. Selles näites salvestati installi skripti avaliku bloobimälu Azure Storage. Turvalisemad suvand oleks installi skripti nimega turvaline bloobimälu [Turvaline juurdepääs allkirja](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS) talletamiseks.

Azure'i CLI, Linux ja CustomScript laiend täiendavad ressursid on loetletud edasi.

[Toimingute automatiseerimine Linux VM kohandamise abil CustomScript laiend](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Azure'i Linux laiendid (GitHub)](https://github.com/Azure/azure-linux-extensions)

[Linux ja avatud lähtekoodi arvutuste Azure](virtual-machines-linux-opensource-links.md)
