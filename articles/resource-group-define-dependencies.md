<properties
   pageTitle="Ressursihaldur Mallid sõltuvused | Microsoft Azure'i"
   description="Kirjeldab, kuidas seada ühe ressursi sõltuvad teise ressursi ajal tagada ressursid on juurutatud õiges järjestuses."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="defining-dependencies-in-azure-resource-manager-templates"></a>Azure'i ressursihaldur korduvkasutatava sõltuvused määratlemine

Antud ressursi, võib olla muud ressursid, millest kuuluma juurutatakse ressurss. Näiteks SQL serveri peab olemas, enne kui proovite juurutamine SQL-andmebaasi. Saate määratleda selle seosega märkimise üks ressurss nimega sõltuvad muu ressursiga. Tavaliselt määratlete sõltuvus **ei leia dependsOn** element, kuid saate ka määratleda funktsiooni **viide** kaudu. 

Ressursihaldur hindab ressursid sõltuvusi, ja kasutab neid oma sõltuvad järjestuses. Kui teineteisele ei ole ressursse, ressursihaldur kasutab neid samal ajal.

## <a name="dependson"></a>ei leia dependsOn

Sees malli, ei leia dependsOn element võimaldab määratleda üks ressurss sõltub ühe või mitme ressursid. Selle väärtus võib olla ressursside nimed komaga eraldatud loend. 

Järgmises näites on kujutatud virtuaalse masina skaala komplekt, mis sõltub laadi koormusetasakaalustusteenuse, virtuaalse võrgu ja tsükkel, mis loob mitme salvestusruumi kontod. Järgmises näites ei kuvata neid muud ressursid, kuid nad peavad mujal mall on olemas.

    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "name": "[variables('namingInfix')]",
      "location": "[variables('location')]",
      "apiVersion": "2016-03-30",
      "tags": {
        "displayName": "VMScaleSet"
      },
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      ...
    }

Sõltuvus vahel ressursi ja ressursse, mis on loodud Kopeeri tsükkel määratlemiseks seatud ei leia dependsOn elemendi nime kursis. Näiteks vt [mitmes eksemplaris ressursside Azure'i ressursihaldur loomine](resource-group-create-multiple.md).

Kuigi võite olla valmis kasutama ei leia dependsOn vastendamiseks seoseid oma ressursse, on oluline mõista, miks te parajasti teete seda, kuna see võib mõjutada jõudlust juurutamise. Näiteks dokumendi, kuidas on omavahel seotud ressursse, mis ei leia dependsOn pole õige. Te ei saa päringut, millised ressursid on määratletud ei leia dependsOn elementi pärast juurutamine. Ei leia dependsOn abil saate potentsiaalselt mõjutada juurutamise aja Kuna ressursihaldur ei juurutada paralleelselt kahe ressursse, mis sõltuvad. Dokumendi ressursid seoseid, selle asemel kasutada [ressursside linkimine](resource-group-link-resources.md).

## <a name="child-resources"></a>Lapse ressursid

Atribuudi ressursid saate määrata lapse ressursse, mis on seotud ressursside, mis on määratletud. Lapse ressursid saab ainult määratletud viis taset. See on oluline Pange tähele, et on peidetud sõltuvus on loodud lapse ressursi ja ema ressursile vahel. Kui teil on vaja lapse ressursi pärast ema ressursile kasutusele võtta, peate selgesõnaliselt selle atribuudiga ei leia dependsOn sõltuvus. 

Igale ema ressursile aktsepteerib ainult teatud ressursi lapse ressurssidena. Aktsepteeritud ressursi tüübid on määratud [malli skeemi](https://github.com/Azure/azure-resource-manager-schemas) ema ressursile. Lapse ressursi nimi sisaldab peamise ressursi tüüp, nimi, nagu **Microsoft.Web/sites/config** ja **Microsoft.Web/sites/extensions** on nii lapse ressursse **Microsoft.Web/sites**.

Järgmises näites on kujutatud SQL server ja SQL-andmebaasi. Pange tähele, et mõne konkreetsete sõltuvus on määratletud SQL-andmebaas ja SQL serveri, isegi juhul, kui andmebaas on serveri.

    "resources": [
      {
        "name": "[variables('sqlserverName')]",
        "type": "Microsoft.Sql/servers",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "SqlServer"
        },
        "apiVersion": "2014-04-01-preview",
        "properties": {
          "administratorLogin": "[parameters('administratorLogin')]",
          "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
        },
        "resources": [
          {
            "name": "[parameters('databaseName')]",
            "type": "databases",
            "location": "[resourceGroup().location]",
            "tags": {
              "displayName": "Database"
            },
            "apiVersion": "2014-04-01-preview",
            "dependsOn": [
              "[variables('sqlserverName')]"
            ],
            "properties": {
              "edition": "[parameters('edition')]",
              "collation": "[parameters('collation')]",
              "maxSizeBytes": "[parameters('maxSizeBytes')]",
              "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
            }
          }
        ]
      }
    ]


## <a name="reference-function"></a>funktsiooni viide

[Viite funktsioon](resource-group-template-functions.md#reference) lubab avaldise tuletada väärtusega muude JSON nime ja väärtuse paarideks või käitusaja ressursid. Viide avaldiste deklareerida peidetult üks ressurss sõltub teise. 

    reference('resourceName').propertyPath

Saate määrata sõltuvused – see element või ei leia dependsOn element, kuid teil pole vaja kasutada mõlemat sama sõltuvad ressursi. Võimaluse korral kasutada vältimiseks tahtmatult lisamine on mittevajalike sõltuvus on peidetud viide.

Lisateavet leiate teemast [viite funktsioon](resource-group-template-functions.md#reference).

## <a name="next-steps"></a>Järgmised sammud

- Azure'i ressursihaldur mallide loomise kohta leiate lisateavet teemast [funktsiooniga Mallid](resource-group-authoring-templates.md). 
- Malli saadaolevate funktsioonide loendi leiate teemast [malli funktsioonid](resource-group-template-functions.md).

