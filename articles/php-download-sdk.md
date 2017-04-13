<properties
    pageTitle="Azure'i SDK php allalaadimine"
    description="Saate teada, kuidas alla laadida ja installida Azure SDK php."
    documentationCenter="php"
    services="app-service\web"
    authors="allclark"
    manager="douge"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="06/01/2016"
    ms.author="allclark;yaqiyang"/>

#<a name="download-the-azure-sdk-for-php"></a>Azure'i SDK php allalaadimine

## <a name="overview"></a>Ülevaade

Azure'i SDK php sisaldab komponendid, mis võimaldavad teil töötada, juurutada ja hallata Azure PHP rakendusi. Täpsemalt Azure'i SDK php sisaldab järgmist:

* **PHP kliendi teegid Azure**. Nende klassi teekide pakub liidest juurdepääsuks Azure funktsioone, näiteks andmete halduse teenused ja pilveteenused.  
* **Mac, Linux, ja (Azure'i CLI) Windows Azure'i käsurea liides**. See on kogum juurutamine ja Azure teenused, nt Azure veebisaitide ja Azure'i Virtuaalmasinates haldamise käsud. Mis tahes platvormi, sh Mac, Linux ja Windows Azure'i CLI töö.
* **Azure'i PowerShelli (ainult Windows)**. See on kasutamise ja haldamise Azure'i teenuseid, näiteks pilveteenustega ja Virtuaalmasinates PowerShelli cmdlet-käskude kogum.
* **Azure'i Emulators (ainult Windows)**. Arvuta ja salvestusruumi emulaatorid on kohalik emulators pilveteenustega ja andmete haldamise teenuseid, mis lubavad teil testida rakenduse kohalikult. Ainult Windows Azure'i Emulators käivitada.

Järgmistes jaotistes on kirjeldatud, kuidas alla laadida ja installida eespool kirjeldatud komponendid.

Selles teemas juhiseid endale [PHP] [ install-php] installitud.

> [AZURE.NOTE] Peate kasutama PHP kliendi teegid Azure PHP 5,5 või uuem versioon.

##<a name="php-client-libraries-for-azure"></a>PHP kliendi teegid Azure

PHP kliendi teegid Azure pakub liidest juurdepääsuks Azure funktsioone, näiteks andmete halduse teenused ja pilveteenused, mis tahes operatsioonisüsteem. Need teegid installimist helilooja kaudu.

Azure PHP kliendi teekide kasutamise kohta leiate lisateavet teemast [bloobimälu teenust kasutada][blob-service], [tabeli teenuse kasutamise kohta] [ table-service] ja [Kuidas kasutada järjekorda teenuse][queue-service].

###<a name="install-via-composer"></a>Helilooja kaudu installida

1. [Installige Git][install-git].


    > [AZURE.NOTE] Opsüsteemis Windows, samuti peate käivitatava Git lisamiseks oma tee keskkonnas muutujana.

2. Looge fail nimega projekti juurkaustas **composer.json** ja lisada sellele järgmine kood:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Laadige alla ** [composer.phar] [ composer-phar] ** oma projekti root.

4. Avage käsuviip ja täita seda oma projekti juursait

        php composer.phar install

##<a name="azure-powershell-and-azure-emulators"></a>Azure'i PowerShelli ja Azure Emulators

Azure'i PowerShelli on PowerShelli cmdlet-käskude kasutamise ja haldamise Azure'i teenuste (nt pilveteenustega ja Virtuaalmasinates). Azure'i emulaatorid on emulators pilveteenustega ja andmete haldamise teenuseid, mis lubavad teil testida rakenduse kohalikult. Järgmised komponendid on toetatud ainult Windows.

Soovitatav viis installida Azure PowerShelli ja Azure Emulators on kasutada [Microsoft Web platvormi Installer][download-wpi]. Pange tähele, et võite ka muud arengu komponendid, näiteks PHP, SQL Server, Microsoft SQL Server PHP ja WebMatrix Drivers installimiseks.

Azure'i PowerShelli kasutamise kohta leiate teavet teemast [Azure PowerShelli kasutamine][powershell-tools].

##<a name="azure-cli"></a>Azure'i CLI

Azure'i CLI on kogum juurutamine ja Azure teenused, nt Azure veebisaitide ja Azure'i Virtuaalmasinates haldamise käsud. Azure'i CLI installimise kohta leiate teavet teemast [installige Azure'i CLI](xplat-cli-install.md).

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate teemast [PHP Arenduskeskus](/develop/php/).


[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
