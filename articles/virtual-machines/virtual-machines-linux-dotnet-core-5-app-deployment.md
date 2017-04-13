<properties
   pageTitle="Toimingute automatiseerimine rakenduse juurutamine virtuaalse masina laiendiga | Microsoft Azure'i"
   description="Azure virtuaalse masina DotNet Core õpetus"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Rakenduse juurutamine Azure'i ressursihaldur mallide kasutamine

Kui kõik Azure infrastruktuuri nõuetele tuvastatud ja tõlkida juurutamise malli tegeliku rakenduse juurutamine peaks käsitlema. Rakenduse juurutamine siin viitab installimist tegeliku rakenduse kahendfaile peale Azure ressursse. Muusika poe proovi, .net NGINX inspektori ja Core tuleb seadistada iga virtual arvutisse. Muusika poe kahendfaile tuleb peale virtual arvutisse olema installitud ja muusika poe andmebaas eelnevalt loodud.

Selle dokumendi üksikasjad, kuidas saate virtuaalse masina laiendid automatiseerida rakenduse juurutamine ja Azure'i virtuaalmasinates konfigureerimine. Kõik sõltuvused ja kordumatute konfiguratsioone on esile tõstetud. Parima kasutuskogemuse saamiseks eelnevalt juurutada eksemplari lahendus Azure tellimuse ja töö koos Azure ressursihaldur mall. Täielik malli leiate siit – [Muusika poe juurutamise Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="configuration-script"></a>Konfigureerimise skripti

Virtuaalse masina laiendid on eriotstarbeline programmid käivitada vastu virtuaalmasinates esitada konfiguratsiooni automatiseerimine. Laiendid on saadaval, nt viirusetõrje, logimine konfigureerimine ja keskmise suurusega konfiguratsiooni mitmel teatud otstarbel. Kohandatud skript laiend saab käivitada igal skripti virtuaalse masina suhtes. Muusika poe valimi, kus on kohandatud skript laiend konfigureerimine Ubuntu virtuaalmasinates ja installida muusika poe rakendus.

Enne üksikasjalikult, kuidas Azure'i ressursihaldur mallis deklareeritud virtuaalse masina laiendid, uurida skripti, mida käitatakse. See skript konfigureerib Ubuntu virtuaalse masina majutada muusika poe rakendus. Kui käivitate, skripti installitakse kõik vajalik tarkvara, installige poe rakendus muusika andmeallika juhtelemendi ja andmebaasi ettevalmistamine. 

Lisateavet mõne .net-i majutusteenuse [Avalda Linux tootmiskeskkonda](https://docs.asp.net/en/latest/publishing/linuxproduction.html)Core Linux rakenduse kohta leiate teemast. 

> See näide on tegevused.

```none
#!/bin/bash

# install dotnet core
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update
sudo apt-get install -y dotnet-dev-1.0.0-preview2-003121

# download application
sudo wget https://raw.github.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/music-store-azure-demo-pub.tar /
sudo mkdir /opt/music
sudo tar -xf music-store-azure-demo-pub.tar -C /opt/music

# install nginx, update config file
sudo apt-get install -y nginx
sudo service nginx start
sudo touch /etc/nginx/sites-available/default
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/nginx-config/default -O /etc/nginx/sites-available/default
sudo cp /opt/music/nginx-config/default /etc/nginx/sites-available/
sudo nginx -s reload

# update and secure music config file
sed -i "s/<replaceserver>/$1/g" /opt/music/config.json
sed -i "s/<replaceuser>/$2/g" /opt/music/config.json
sed -i "s/<replacepass>/$3/g" /opt/music/config.json
sudo chown $2 /opt/music/config.json
sudo chmod 0400 /opt/music/config.json

# config supervisor
sudo apt-get install -y supervisor
sudo touch /etc/supervisor/conf.d/music.conf
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/supervisor/music.conf -O /etc/supervisor/conf.d/music.conf
sudo service supervisor stop
sudo service supervisor start

# pre-create music store database
/usr/bin/dotnet /opt/music/MusicStore.dll &
```

## <a name="vm-script-extension"></a>VM skripti laiend

VM laiendid saab käivitada vastu virtuaalse masina Koosta ajal kaasamise laiend ressursi Azure'i ressursihaldur malli abil. Pikendamise saab lisada viisardiga Visual Studio lisada ressursi või lisades kehtiv JSON mall. Skripti laiend ressursi on pesastatud virtuaalse masina ressurss; See on näha Järgnevas näites.

Seda linki JSON valimi ressursihaldur Mall – [VM skripti laiend](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359)kuvamiseks. 

Teade kuvatakse allpool JSON, et script on talletatud GitHub. Azure'i bloobimälu võib ka salvestatakse see skript. Azure'i ressursihaldur Mallid luba ka skripti täitmise stringi selline, et malli parameetrite väärtused saate kasutada parameetrite skripti täitmist. Sel juhul andmed juurutamisel Mallid ja need väärtused saab kasutada skripti täitmisel.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Kohandatud skript laiend kasutamise kohta leiate lisateavet teemast [kohandatud skript laiendid ressursihaldur mallide abil](./virtual-machines-linux-extensions-customscript.md).

## <a name="next-step"></a>Järgmise juhise juurde

<hr>

[Lisateavet Azure ressursihaldur Mallid uurimine](https://github.com/Azure/azure-quickstart-templates)
