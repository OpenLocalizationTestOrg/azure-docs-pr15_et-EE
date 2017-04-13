<properties
    pageTitle="Juurutamine LAMP Linux virtual arvutisse | Microsoft Azure'i"
    description="Siit saate teada, kuidas installida LAMP virnas Linux VM"
    services="virtual-machines-linux"
    documentationCenter="virtual-machines"
    authors="jluk"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="juluk"/>

# <a name="deploy-lamp-stack-on-azure"></a>LAMP virnas Azure juurutamine
Selles artiklis juhendab teid juurutamise Apache veebiserver, MySQL-i ja Azure PHP (LAMP virnas). Peate konto Azure ([tasuta prooviversiooni](https://azure.microsoft.com/pricing/free-trial/)) ja [Azure CLI](../xplat-cli-install.md) , mis on [Azure kontoga ühendatud](../xplat-cli-connect.md).

On kaks võimalust installimist LAMP artikkel.

## <a name="quick-command-summary"></a>Kiirülevaate käsk Kokkuvõte

1) Uue VM LAMP juurutamine

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

2) Olemasoleva VM LAMP juurutamine

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Uue VM kiirtutvustus LATERN juurutamine

Saate hakata uue [Ressursirühma](../azure-resource-manager/resource-group-overview.md) VM sisaldavaid loomisega.

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

VM ise luua, saate kasutada juba kirjutatud Azure'i ressursihaldur Mall, mis leitud [github siin](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Peaksite nägema küsimata mõned rohkem sisendina vastuse:

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment to complete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

Nüüd olete loonud Linux VM laternaga, mis on juba installitud. Kui soovite, saate kontrollida installimist hüpates [kinnitamine edukalt paigaldatud] allapoole.

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Olemasoleva VM kiirtutvustus LATERN juurutamine

Kui vajate abi Linux VM loomisel saate pea [siit saate teada, kuidas luua Linux VM] (. / virtual-machines-linux-quick-create-cli.md). Järgmiseks peate SSH Linuxi sisse. Kui vajate abi mõne SSH võti loomise saate pea [siit saate teada, kuidas luua mõne SSH võti Linux-või Mac-] (. / virtual-machines-linux-mac-create-ssh-keys.md).
Kui teil on juba mõne SSH võti, minna edasi ja SSH oma Linux VM koos `ssh username@uniqueDNS`.

Nüüd, kui töötate oma Linux VM sees, juhendab me LAMP virnas installimine Debiani-põhise jaotuse. Täpse käsud teisi Linuxi distros võib erineda.

#### <a name="installing-on-debianubuntu"></a>Debian/Ubuntu installimine

Peate installitud järgmised paketid: `apache2`, `mysql-server`, `php5`, ja `php5-mysql`. Saate need otse haardeseadised need paketid või kasutades Tasksel installida. Juhised mõlema variandi on loetletud allpool.
Enne installimist peate alla laadima ja paketi loendite värskendamine.

    user@ubuntu$ sudo apt-get update
    
##### <a name="individual-packages"></a>Üksikute pakettide
Kasutades apt-get:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Tasksel abil
Teise võimalusena saate alla laadida Tasksel Debian/Ubuntu tööriista, mis installitakse mitu seotud pakettide koordineeritud "ülesanne" peale teie süsteemi.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

Pärast opsüsteemi kummale eeltoodud võimalustest palutakse installida need paketid ja mitmesuguseid muid sõltuvused. Vajutage "y" ja seejärel "Enter" jätkamiseks ja teiste viipasid, MySQL-i administraatori parooli määramiseks järgige. See installib minimaalne vajalik PHP laiendid vaja PHP MySQL-i abil. 

![][1]

Muud PHP laiendid, mis on saadaval paketid kuvamiseks järgmine käsk:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Info.php dokumendi loomine

Mida peaks nüüd olema saate vaadata, millist versiooni Apache, MySQL-i ja PHP peate käsurea kaudu, tippides `apache2 -v`, `mysql -v`, või `php -v`.

Kui soovite testida täiendavate, saate luua kiirülevaate PHP teabeleht brauseri kaudu vaadata. Uue faili loomine nano tekstiredaktoris see käsk:

    user@ubuntu$ sudo nano /var/www/html/info.php

Sees GNU Nano tekstiredaktoris, lisage järgmised read.

    <?php
    phpinfo();
    ?>

Seejärel salvestage ja sulgege tekstiredaktor.

Taaskäivitage Apache see käsk nii, et kõik uue installid jõustub.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Veenduge, et LAMP on installitud

Nüüd saate kontrollida äsja loodud brauseris minnes http://youruniqueDNS/info.php PHP teabeleht, see peaks välja nägema umbes järgmine.

![][2]

Saate vaadata oma Apache installi Apache2 Ubuntu vaikimisi lehe vaatamiseks saate http://youruniqueDNS/. Peaksite nägema umbes selline.

![][3]

Palju õnne, teil on lihtsalt setup LAMP virnas oma Azure VM!

## <a name="next-steps"></a>Järgmised sammud

Vaadake LAMP virnas Ubuntu dokumentatsiooni:

- [https://Help.Ubuntu.com/Community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/virtual-machines-linux-deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/virtual-machines-linux-deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/virtual-machines-linux-deploy-lamp-stack/apachesuccesspage.png
