<properties
   pageTitle="Kohandatud skripte Linux VMs | Microsoft Azure'i"
   description="Toimingute automatiseerimine Linux VM konfiguratsiooni, kasutades kohandatud skript laiend"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="nepeters"/>

# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Azure'i Custom Script laiend Linuxi kasutamine

Kohandatud skript laiend allalaaditavate failide ja käivitab skriptide Azure'i virtuaalmasinates. Sellele laiendile on kasulik postituse juurutuse konfigureerimise, tarkvara installimine või mis tahes muud konfiguratsiooni / halduse ülesanne. Skriptide saate alla laadida Azure salvestusruumi või muude puuetega inimestele juurdepääsetavate Interneti-asukohta või esitatud Käitusaeg laiend. Kohandatud skript laiend integreerub Azure'i ressursihaldur Mallid ja ka käivitamist Azure'i CLI, PowerShelli, Azure portaali või Azure virtuaalse masina REST API-ga.

Selle dokumendi üksikasjad, kuidas kasutada kohandatud skript laiendamine: Azure'i CLI ja Azure ressursihaldur malli ja ka üksikasjad Linux süsteemid tõrkeotsingujuhiseid.

## <a name="extension-configuration"></a>Laiend konfigureerimine

Kohandatud skript laiend konfiguratsiooni skripti asukoht ja käitamiseks käsk, näiteks saate määrata. Selle konfiguratsiooni saab salvestada failid, käsureal või Azure ressursihaldur mallis määratud. Tundliku loomuga andmeid saab salvestada kaitstud konfiguratsiooni, mis on krüptitud ja ainult dekrüptida virtual seadmes. Kaitstud konfigureerimine on kasulik, kui käsk täitmise sisaldab saladusi nagu parooli.

### <a name="public-configuration"></a>Avaliku konfigureerimine

Skeemi:

- **commandToExecute**: (nõutav, tekstistringi) kirje punkti skripti käivitada
- **fileUris**: (valikuline, stringi massiiv) URL-id, faile alla laadida.
- **ajatempli** (valikuline, täisarv) kasutada ainult mõne Käivita skripti uuesti käivitamiseks, muutes selle välja väärtuse väli.

```none
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Kaitstud konfigureerimine

Skeemi:

- **commandToExecute**: (valikuline, stringi) kirje punkti skripti käivitada. Kui teie käsk sisaldab saladusi, nt paroolide, kasutage selle asemel väli.
- **storageAccountName**: (valikuline, stringi) salvestusruumi konto nimi. Kui määrate salvestusruumi mandaat, tuleb kõik fileUris URL-ide Azure plekid.
- **storageAccountKey**: (valikuline, stringi) kiirklahv salvestusruumi konto.


```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure'i CLI

Kohandatud skript laiend käivitamiseks Azure'i CLI kasutamisel luua konfiguratsioonifail või faile, mis sisaldavad vähemalt uri fail ja käsk skripti täitmise.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /scirpt-config.json
```

Soovi korral võite käsu saab käitada abil soovitud `--public-config` ja `--private-config` võimalus, mis võimaldab konfiguratsioonis määratud käitamise ajal ja ilma eraldi konfiguratsioonifail.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

### <a name="azure-cli-examples"></a>Azure'i CLI näited

**Näide 1** - skriptifail avaliku konfiguratsiooni.

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
  "commandToExecute": "./hello.sh"
}
```

Azure'i CLI käsk:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Näide 2** - pole skriptifail avaliku konfiguratsiooni.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure'i CLI käsk:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Näide 3** – avaliku konfiguratsioonifail abil saate määrata skriptifail URI ja kaitstud konfiguratsioonifail abil saate määrata käivitatakse käsk.

Avaliku konfiguratsioonifail:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
}
```

Kaitstud konfiguratsioonifail:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Azure'i CLI käsk:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path ./public.json --private-config-path ./protected.json
```

## <a name="resource-manager-template"></a>Ressursihaldur Mall

Azure'i kohandatud skript laiend käitamise ajal virtuaalse masina juurutamise ressursihaldur malli abil. Selleks lisada õigesti vormindatud JSON juurutamise malli.

### <a name="resource-manager-examples"></a>Ressursihaldur näited

**Näide 1** – avaliku konfigureerimine.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Näide 2** - täitmise käsk kaitstud konfiguratsiooni.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
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
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Lugege teemat .net Core muusika poe Demo täieliku näiteks - [Muusika poe Demo](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Tõrkeotsing

Kohandatud skript laiend käitamisel script on loodud või alla laaditud kausta, mis on sarnane järgmises näites. Käsu väljund salvestatakse ka selle kataloogi `stdout` ja `stderr` faili.

```none
/var/lib/azure/custom-script/download/0/
```

Azure'i skripti laiendamine toodab Logi, mille leiate siit.

```none
/var/log/azure/customscript/handler.log
```

Azure'i CLI saab laadida ka kohandatud skript laiend Andmetäite olek.

```none
azure vm extension get <resource-group> <vm-name>
```

Väljund näeb välja umbes järgmine tekst:

```none
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Järgmised sammud

Muud VM skripti laiendid kohta leiate teavet teemast [Azure skripti laiend ülevaade Linuxi jaoks](./virtual-machines-linux-extensions-features.md).