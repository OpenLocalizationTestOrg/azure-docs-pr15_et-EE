<properties
   pageTitle="Mitmes eksemplaris ressursside juurutamine | Microsoft Azure'i"
   description="Kopeerimine ja massiivi kasutada kordamiseks Azure'i ressursihaldur mallis mitu korda, kui ressursse rakendades."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/30/2016"
   ms.author="tomfitz"/>

# <a name="create-multiple-instances-of-resources-in-azure-resource-manager"></a>Mitmes eksemplaris ressursside Azure'i ressursihaldur loomine

Selles teemas näidatakse, kuidas kordamiseks Azure'i ressursihaldur malli loomiseks mitu eksemplari ressursi.

## <a name="copy-copyindex-and-length"></a>kopeerimine ja copyIndex pikkus

Sees ressursi loomiseks mitu korda, saate määratleda objekti **kopeerimine** , mis määrab, mitu korda kordamiseks. Kopeeri võtab järgmises vormingus:

    "copy": { 
        "name": "websitescopy", 
        "count": "[parameters('count')]" 
    } 

Pääsete iteratsiooni praegune väärtus funktsiooniga **copyIndex()** , nagu allpool näidatud concat funktsioonis.

    [concat('examplecopy-', copyIndex())]

Loomisel mitme ressursid massiivi väärtustest, saate funktsiooni **pikkus** arvu määramiseks. Esitate parameetrina pikkus funktsiooni massiiv.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

## <a name="use-index-value-in-name"></a>Kasutage registri väärtuse nimi

Saate kasutada Kopeeri toiming loomine mitme eksemplari ressurss, mis on kordumatult nimega incrementing indeksi. Näiteks soovite kordumatu numbri lisamine iga ressursinimi juurutatud lõppu. Juurutamiseks kolme veebisaitide nimega:

- examplecopy-0
- examplecopy-1
- examplecopy-2.

Malli:

    "parameters": { 
      "count": { 
        "type": "int", 
        "defaultValue": 3 
      } 
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', copyIndex())]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[parameters('count')]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          }
      } 
    ]

## <a name="offset-index-value"></a>OFFSET indeksi väärtus

Märkate eelmises näites, mis indeksi väärtus nullist 2. Offset indeksi väärtus, saate funktsiooni **copyIndex()** , nt **copyIndex(1)**väärtust edastada. Iteratsiooni sooritamiseks arv on endiselt määratud Kopeeri elementi, kuid copyIndex väärtus on offset määratud väärtuse alusel. Eelmise näitega sama malli abil, kuid soovite määrata **copyIndex(1)** oleks, juurutada kolme veebisaitide nimega:

- examplecopy-1
- examplecopy-2
- examplecopy-3

## <a name="use-copy-with-array"></a>Kopeeri kasutamine massiiv
   
Koopia toiming on eriti kasulik töötamisel massiive, kuna te saate itereerima läbi massiivi iga element. Juurutamiseks kolme veebisaitide nimega:

- examplecopy-Contoso
- examplecopy-Fabrikam
- examplecopy-Coho

Malli:

    "parameters": { 
      "org": { 
         "type": "array", 
             "defaultValue": [ 
             "Contoso", 
             "Fabrikam", 
             "Coho" 
          ] 
      }
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', parameters('org')[copyIndex()])]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[length(parameters('org'))]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          } 
      } 
    ]

Muidugi seate Kopeeri arvu väärtus massiivi pikkus välja. Näiteks võib massiivi loomine mitme väärtusega ja seejärel edasi parameetri väärtuse, mis määrab, mitu massiivi elementide juurutamiseks. Sel juhul saate määrata Kopeeri count esimese näite. 

## <a name="depending-on-resources-in-a-loop"></a>Sõltuvalt esitatavaks ressursid

Saate määrata, et ressursi kasutusele võtta mõne muu ressursi pärast **ei leia dependsOn** elemendi abil. Kui soovite juurutada ressurss, mis sõltub ressursid esitatavaks kogum, saate kasutada Kopeeri tsükkel **ei leia dependsOn** elementi nimi. Järgmises näites on kujutatud juurutamise enne juurutamist virtuaalse masina 3, salvestusruumi kontod. Täielik virtuaalse masina määratlus ei kuvata. Märkate, et Kopeeri element on **nimi** , mis on seatud **storagecopy** ja **ei leia dependsOn** elemendi jaoks soovitud Virtuaalmasinates luua **storagecopy**.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "resources": [
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Storage/storageAccounts",
                "name": "[concat('storage', uniqueString(resourceGroup().id), copyIndex())]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "accountType": "Standard_LRS"
                 },
                "copy": { 
                    "name": "storagecopy", 
                    "count": 3 
                }
            },
           {
               "apiVersion": "2015-06-15", 
               "type": "Microsoft.Compute/virtualMachines", 
               "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
               "dependsOn": ["storagecopy"],
               ...
           }
        ],
        "outputs": {}
    }

## <a name="looping-on-a-nested-resource"></a>Hoidke pesastatud ressursi

Te ei saa kasutada Kopeeri tsükkel pesastatud ressursi. Kui teil on vaja luua mitu eksemplari ressurss, mis tavaliselt määratlete nimega pesastatud sees mõni muu ressurss, peate selle asemel ülataseme ressurssi ressursi loomine ja suhe ema ressursile kaudu atribuudid **Tüüp** ja **nime** määratlemine.

Oletame näiteks, et määratlete tavaliselt andmekomplekt pesastatud ressurssi andmete Factory sees.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "string"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
        "resources": [
        {
            "type": "datasets",
            "name": "[parameters('dataSetName')]",
            "dependsOn": [
                "[parameters('dataFactoryName')]"
            ],
            ...
        }
    }]
    
Mitme eksemplari andmekomplektide loomiseks oleks vaja muuta malli, nagu allpool näidatud. Täielikult kvalifitseeritud teate tüüp ja nimi sisaldab andmeid factory nimi.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "array"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
    },
    {
        "type": "Microsoft.DataFactory/datafactories/datasets",
        "name": "[concat(parameters('dataFactoryName'), '/', parameters('dataSetName')[copyIndex()])]",
        "dependsOn": [
            "[parameters('dataFactoryName')]"
        ],
        "copy": { 
            "name": "datasetcopy", 
            "count": "[length(parameters('dataSetName'))]" 
        } 
        ...
    }]

## <a name="create-multiple-instances-when-copy-wont-work"></a>Luua mitmes eksemplaris, kui kopeerimine ei tööta

Saate kasutada ainult **Kopeeri** ressursi tüübid, mitte jooksul ressursi tüüp. See luua teile probleeme, kui soovite luua midagi, mis on osa ressursi mitmes eksemplaris. Stsenaarium on virtuaalse masina jaoks mitu andmete ketta loomiseks. Ei saa kasutada **Kopeeri** andmete ketast, kuna **dataDisks** on atribuut virtuaalse masina, mitte eraldi ressursi tüüp. Selle asemel saate luua massiivi nii palju andmeid ketast, kui vaja, ning edastavad tegelik arv andmete ketast loomiseks. Virtuaalse masina määratlus, saate **teha** funktsiooni ainult arv elemente, mida te tegelikult ei soovi toomine massiiv.

Täielik näide selle mustri on Kuva malli [loomine andmete ketast dünaamiline valik VM](https://azure.microsoft.com/documentation/templates/201-vm-dynamic-data-disks-selection/) .

Juurutamise malli jaotised on näidatud allpool. Palju mall on eemaldatud esile dünaamiliselt loomisega arvu andmete ketast seotud jaotised. Pange tähele parameetri **numDataDisks** , mis võimaldab läbida ketast loomiseks arv. 

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
    "numDataDisks": {
      "type": "int",
      "maxValue": 64,
      "metadata": {
        "description": "This parameter allows you to select the number of disks you want"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'dynamicdisk')]",
    "sizeOfDataDisksInGB": 100,
    "diskCaching": "ReadWrite",
    "diskArray": [
      {
        "name": "datadisk1",
        "lun": 0,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk1.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk2",
        "lun": 1,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk2.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk3",
        "lun": 2,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk3.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk4",
        "lun": 3,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk4.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      ...
      {
        "name": "datadisk63",
        "lun": 62,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk63.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk64",
        "lun": 63,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk64.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      }
    ]
  },
  "resources": [
    ...
    {
      "type": "Microsoft.Compute/virtualMachines",
      "properties": {
        ...
        "storageProfile": {
          ...
          "dataDisks": "[take(variables('diskArray'),parameters('numDataDisks'))]"
        },
        ...
      }
      ...
    }
  ]
}
```


## <a name="next-steps"></a>Järgmised sammud
- Kui soovite lisateavet malli osad, leiate [Azure'i ressursihaldur mallide koostamine](./resource-group-authoring-templates.md).
- Kõik funktsioonid, mida saate kasutada malli, leiate [Azure'i ressursihaldur malli funktsioonid](./resource-group-template-functions.md).
- Saate teada, kuidas kasutada malli, leiate [Deploy rakenduse Azure'i ressursihaldur malli abil](resource-group-template-deploy.md).
