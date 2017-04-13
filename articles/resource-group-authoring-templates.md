<properties
   pageTitle="Azure'i ressursihaldur mallide koostamine | Microsoft Azure'i"
   description="Azure'i ressursihaldur mallide abil deklaratiivseid JSON süntaks juurutada rakendused Azure loomine."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="authoring-azure-resource-manager-templates"></a>Azure'i ressursihaldur mallide koostamine

Selles teemas kirjeldatakse struktuuri Azure'i ressursihaldur malli. Selles esitatakse malli ja atribuudid, mis on saadaval nende jaotiste erinevate. Mall sisaldab JSON ja avaldised, mille abil saate koostada oma juurutamiseks väärtused. 

Olete juba juurutanud ressursid Mall, Kuva [eksportida mõne olemasoleva ressursid Azure'i ressursihaldur malli](resource-manager-export-template.md). Malli loomine juhised leiate teemast [Ressursihaldur Mall ülevaade](resource-manager-template-walkthrough.md). Soovitused mallide loomise kohta vt [Azure'i ressursihaldur mallide loomise head tavad](resource-manager-template-best-practices.md).

Hea JSON editor saate lihtsustada ülesande luua malle. Visual Studio mallide kasutamise kohta leiate artiklist [loomine ja juurutamine Azure ressursi rühma Visual Studio kaudu](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). VS koodi kasutamise kohta leiate teemast [Azure ressursihaldur Mallid Visual Studio kood töötamine](resource-manager-vs-code.md).

Piirata suurust malli 1 MB ja iga parameetri faili 64 KB. 1 MB piirang kehtib lõplik olekusse malli, kui see on laiendatud ja iteratiivsed ressursi määratlused muutujate ja parameetrite väärtused. 

## <a name="template-format"></a>Malli vorming

Lihtsaim struktuuri Mall sisaldab järgmisi elemente:

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| Elemendi nime   | Nõutav | Kirjeldus
| :------------: | :------: | :----------
| $schema        |   Jah    | Malli keele versiooni kirjeldav JSON-skeemifaili asukoht. Kasutage eelmises näites URL-i.
| contentVersion |   Jah    | Malli (nt 1.0.0.0) versiooni. Saate sisestada väärtuse jaoks – see element. Juurutamisel ressursid malli abil saab seda väärtust veenduge, et kasutataks õige mall.
| parameetrid     |   Ei     | Väärtused, mille juurutamise käivitatakse kohandamiseks ressursi juurutamise registreerimisel.
| muutujad      |   Ei     | Väärtused, mida kasutatakse JSON fragmendid malli malli keele avaldiste lihtsustamiseks.
| ressursid      |   Jah    | Ressursi tüübid, mis on juurutatud või värskendatud ressursirühma.
| väljundid        |   Ei     | Pärast juurutuse tagastatud väärtused.

Uurime malli lähemalt selle teema allpool olevatest jaotistest. Nüüd, vaatame läbi mõne süntaks, mis moodustab mall.

## <a name="expressions-and-functions"></a>Avaldiste ja funktsioonide

Malli basic süntaks on JSON. Avaldiste ja funktsioonide pikendada JSON, mis on saadaval mall. Avaldistega, saate luua väärtusi, mis pole range väärtused. Avaldiste on ümbritsetud sulgudes [ja], hinnatakse malli juurutamisel. Avaldiste saate kuvada igal pool JSON stringiväärtus ja alati tagastusväärtus teise JSON. Kui peate kasutama sõnasõnaline string, mis algab sulu [, kasutage kahe sulgudes [[.

Tavaliselt kasutatakse avaldiste funktsioonide konfigureerimine juurutamise toimingute tegemiseks. Nii nagu JavaScript, funktsioon kõnede vormindatud **functionName(arg1,arg2,arg3)**. Viidatava atribuudid, kasutades tehtemärke punkti ja [Register].

Järgmises näites kirjeldatakse, kuidas kasutada mitmeid funktsioone, kui ehitamine väärtused.
 
    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

Malli funktsioonide täieliku loendi leiate teemast [Azure ressursihaldur malli funktsioonid](resource-group-template-functions.md). 


## <a name="parameters"></a>Parameetrid

Parameetrite osas malli, saate määrata, milliseid väärtusi saate sisestada kui ressursse. Järgmiste parameetrite väärtused võimaldavad kohandada juurutamise pakkudes väärtused, mis on kohandatud teatud keskkonnas (nt arendaja, testimine ja tootmise). Teil pole anda parameetrid malli, kuid ilma parameetrite malli oleks alati kasutada sama ressursside sama nimed, asukohad ja atribuudid.

Järgmiste parameetrite väärtused kogu malli abil saate määrata väärtused juurutatud ressursid. Muudesse sektsioonidesse malli saab kasutada ainult deklareeritud parameetrid jaotises parameetrid.

Järgmise ülesehitusega parameetrite määratlemine

    "parameters": {
       "<parameter-name>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<default-value-of-parameter>",
         "allowedValues": [ "<array-of-allowed-values>" ],
         "minValue": <minimum-value-for-int>,
         "maxValue": <maximum-value-for-int>,
         "minLength": <minimum-length-for-string-or-array>,
         "maxLength": <maximum-length-for-string-or-array-parameters>,
         "metadata": {
             "description": "<description-of-the parameter>" 
         }
       }
    }

| Elemendi nime   | Nõutav | Kirjeldus
| :------------: | :------: | :----------
| parameterName  |   Jah    | Parameetri nimi. Peab olema lubatud JavaScripti identifikaator.
| tüüp           |   Jah    | Parameetri väärtuse tüüp. Siit leiate loendi all lubatud tüübid.
| defaultValue   |   Ei     | Parameetri, kui väärtus puudub parameetri vaikeväärtus.
| allowedValues  |   Ei     | Veenduge, et õige väärtus on esitatud parameetri lubatud väärtuste massiivi.
| minValue       |   Ei     | See väärtus on int tüüp parameetrite väikseima väärtuse, kaasa arvatud.
| maxValue       |   Ei     | See väärtus on int tüüp parameetrite suurima väärtuse, kaasa arvatud.
| minLength      |   Ei     | See väärtus on miinimumpikkus stringi, secureString ja parameetrite massiivi tüüp, kaasa arvatud.
| maxLength      |   Ei     | See väärtus on string, secureString ja massiivi tüüp parameetrite maksimumpikkus kaasa arvatud.
| kirjeldus    |   Ei     | Parameeter, mis kuvatakse kasutajatele malli kasutajaliidese portaali kohandatud malli kirjeldus.

Lubatud tüübid ja väärtused on:

- **string**
- **secureString**
- **int**
- **bool**
- **objekti** 
- **secureObject**
- **massiiv**

Määrake valikuline parameeter, sisestage defaultValue (võib olla tühja stringi). 

Kui määrate parameetri nimi, mis vastab mõnele käsk malli parameetrid, palutakse koos postfix **FromTemplate**parameetri väärtuse ette. Näiteks kui kaasate parameeter nimega **ResourceGroupName** malli see on sama, mis **ResourceGroupName** parameeter [New-AzureRmResourceGroupDeployment] [ deployment2cmdlet] cmdlet-käsk, palutakse **ResourceGroupNameFromTemplate**väärtuse ette. Üldiselt tuleks vältida seda segadust pole nime panemise parameetrid koos parameetrite juurutamise toimingute puhul kasutada sama nimi.

>[AZURE.NOTE] Kõik paroolid, võtmed ja muud saladused tuleks kasutada **secureString** tüüp. Malli parameetrid secureString tüüpi ei saa lugeda pärast ressursi juurutamine. 

Järgmises näites on kujutatud parameetrite määratlemine:

    "parameters": {
      "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
      },
      "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
      },
      "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
      },
      "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
      }
    }

Kuidas sisestada parameetrite väärtused juurutamisel kohta leiate teemast [Deploy rakenduse Azure'i ressursihaldur malli abil](resource-group-template-deploy.md#parameter-file). 

## <a name="variables"></a>Muutujad

Jaotises muutujate Koostage väärtusi, mida saab kasutada kogu malli. Tavaliselt põhinevad muutujate alates parameetrite väärtused. Teil pole vaja määratleda muutujad, kuid need sageli lihtsustada malli vähendades keerulistes avaldistes.

Saate määratleda muutujate järgmine struktuur:

    "variables": {
       "<variable-name>": "<variable-value>",
       "<variable-name>": { 
           <variable-complex-type-value> 
       }
    }

Järgmises näites on kujutatud määratlemine muutuja, mis koosneb mitmesugustest kaks parameetrite väärtused:

     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

Järgmises näites on kuvatud muutuja, mis on keerukas JSON tüüp ja muutujate, mis on valmistatud teiste muutujate.

    "parameters": {
       "environmentName": {
         "type": "string",
         "allowedValues": [
           "test",
           "prod"
         ]
       }
    },
    "variables": {
       "environmentSettings": {
         "test": {
           "instancesSize": "Small",
           "instancesCount": 1
         },
         "prod": {
           "instancesSize": "Large",
           "instancesCount": 4
         }
       },
       "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
       "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
       "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
    }

## <a name="resources"></a>Ressursid

Jaotises ressursid saate määratleda ressursse, mis on juurutatud või mida on värskendatud. Selles jaotises saad keeruline, kuna mõistate juurutate sisestama õige väärtused tüübid. 

Saate määratleda ressursid järgmine struktuur:

    "resources": [
       {
         "apiVersion": "<api-version-of-resource>",
         "type": "<resource-provider-namespace/resource-type-name>",
         "name": "<name-of-the-resource>",
         "location": "<location-of-resource>",
         "tags": "<name-value-pairs-for-resource-tagging>",
         "comments": "<your-reference-notes>",
         "dependsOn": [
           "<array-of-related-resource-names>"
         ],
         "properties": "<settings-for-the-resource>",
         "copy": {
           "name": "<name-of-copy-loop>",
           "count": "<number-of-iterations>"
         }
         "resources": [
           "<array-of-child-resources>"
         ]
       }
    ]

| Elemendi nime             | Nõutav | Kirjeldus
| :----------------------: | :------: | :----------
| apiVersion               |   Jah    | Ressursi loomiseks kasutatav REST API versioon.
| tüüp                     |   Jah    | Ressursi tüüp. See väärtus on kombinatsiooni nimeruum ressursi pakkuja ja ressursside tüüp (nt **Microsoft.Storage/storageAccounts**).
| Nimi                     |   Jah    | Ressursi nimi. Nime järgima määratletud RFC3986 URI komponent piirangud. Lisaks Azure teenused, mis edastavad ressursinimi ettevõtteväliste isikute valideerimiseks nime veendumaks, et see ei ole katse võltsida teise isiku. Vaadake, [märkige ruut ressursinimi](https://msdn.microsoft.com/library/azure/mt219035.aspx).
| asukoht                 |   Muutub  | Toetatud geo asukohti esitatud ressursi. Võite valida asukoht saadaolevate asukohtade, kuid tavaliselt on mõttekas valida ühe, mis asub teie kasutajad. Tavaliselt on mõttekas paigutamiseks ressursid, millest piirkonna omavahel suhelda. Enamiku ressursi nõua asukoht, kuid mõned sõnumitüübid (nt Rollimäärang), pole vaja asukoht.
| Sildid                     |   Ei     | Sildid, mis on seotud ressurss.
| Kommentaarid                 |   Ei     | Märkmete jaoks dokumenteerimine ressursside malli
| ei leia dependsOn                |   Ei     | Ressursse, mis on määratletud ressurss sõltub. Ressursside sõltuvuste hinnatakse ja ressursid on juurutatud nende sõltuvad järjestuses. Kui ressursid ei ole üksteisest, hindamise paralleelselt. Väärtus võib olla komaga eraldatud loend ressursi nimed või ressursside ainulaadsed ID-d.
| atribuudid               |   Ei     | Ressursi konfiguratsioon sätted. Atribuutide väärtused on samad, mis esitate koosolekukutse kehasse REST API toimingu (pane meetod) loomine ressursi väärtused. Lingid ressursi skeemi dokumentatsiooni või REST API-ga, vt [ressursihaldur pakkujate ning API versioonid ja skeemid](resource-manager-supported-services.md).
| kopeerimine                     |   Ei     | Mitme eksemplari vajadusel ressursside loomine. Lisateabe saamiseks lugege teemat [mitmes eksemplaris ressursside Azure'i ressursihaldur loomine](resource-group-create-multiple.md). |
| ressursid                |   Ei     | Lapse ressursse, mis on määratletud ressursi sõltuvad. Saate anda ainult ressursi tüübid, mis on lubatud ema ressursile skeemiga. Lapse ressursi tüüp täielik nimi sisaldab ema ressursi tüüp, näiteks **Microsoft.Web/sites/extensions**. Sõltuvus ema ressursile on pole piisavalt; peate määratlema konkreetselt selle sõltuvus. 

Teab, mis väärtuste määramine **apiVersion**, **Tüüp**ja **asukoht** ei ole kohe selge. Õnneks saate määratleda Azure PowerShelli või Azure CLI need väärtused.

Ressursi pakkujate **PowerShelliga**saamiseks kasutage:

    Get-AzureRmResourceProvider -ListAvailable

Tagastatud loendist otsimine olete huvitatud ressursi pakkujad. Ressursi tüüpi ressursi pakkuja (nt salvestusruumi) saamiseks kasutage:

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes

Ressursi tüüp (nt salvestusruumi kontod) API versioone saamiseks kasutage:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).ApiVersions

Toetatud Salvestuskohad ressursi tüüp saamiseks kasutage:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).Locations

**Azure'i**CLI ressursi pakkujate saamiseks kasutage:

    azure provider list

Tagastatud loendist otsimine olete huvitatud ressursi pakkujad. Ressursi tüüpi ressursi pakkuja (nt salvestusruumi) saamiseks kasutage:

    azure provider show Microsoft.Storage

Toetatud asukohad ja API versioonide saamiseks kasutage:

    azure provider show Microsoft.Storage --details --json

Ressursi pakkujate kohta leiate lisateavet teemast [ressursihaldur pakkujate ning API versioonid ja skeemid](resource-manager-supported-services.md).

Ressursside jaotis sisaldab hulgaliselt ressursse juurutamiseks. Sees iga ressurss, võite määrata ka massiivi lapse ressursid. Seetõttu võib jaotist ressursid on struktuuri nagu.

    "resources": [
       {
           "name": "resourceA",
       },
       {
           "name": "resourceB",
           "resources": [
               {
                   "name": "firstChildResourceB",
               },
               {   
                   "name": "secondChildResourceB",
               }
           ]
       },
       {
           "name": "resourceC",
       }
    ]


Järgmises näites on kujutatud **Microsoft.Web/serverfarms** ressursi **Microsoft.Web/sites** ressursi laps **laiendid** ressursi. Pange tähele, et saidi märgitud sõltuv serveripargis, kuna serveripargi kuuluma saidi saab kasutada. Liiga teade, et **laiendid** ressursi on saidi laps.

    "resources": [
      {
        "apiVersion": "2015-08-01",
        "name": "[parameters('hostingPlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "HostingPlan"
        },
        "sku": {
          "name": "[parameters('skuName')]",
          "capacity": "[parameters('skuCapacity')]"
        },
        "properties": {
          "name": "[parameters('hostingPlanName')]",
          "numberOfWorkers": 1
        }
      },
      {
        "apiVersion": "2015-08-01",
        "type": "Microsoft.Web/sites",
        "name": "[parameters('siteName')]",
        "location": "[resourceGroup().location]",
        "tags": {
          "environment": "test",
          "team": "Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Web/serverFarms/', parameters('hostingPlanName'))]"
        ],
        "properties": {
          "name": "[parameters('siteName')]",
          "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
        },
        "resources": [
          {
            "apiVersion": "2015-08-01",
            "type": "extensions",
            "name": "MSDeploy",
            "dependsOn": [
              "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
            ],
            "properties": {
              "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
              "dbType": "None",
              "connectionString": "",
              "setParameters": {
                "Application Path": "[parameters('siteName')]"
              }
            }
          }
        ]
      }
    ]


## <a name="outputs"></a>Väljundid

Määrake jaotises väljundeid, väärtused, mis on tagastatud juurutamise. Näiteks võite tagastada URI juurdepääsu juurutatud ressursi.

Järgmises näites on kujutatud selle väljundi määratluse struktuuri:

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>"
       }
    }

| Elemendi nime   | Nõutav | Kirjeldus
| :------------: | :------: | :----------
| outputName     |   Jah    | Väljundväärtuse nimi. Peab olema lubatud JavaScripti identifikaator.
| tüüp           |   Jah    | Väljundväärtuse tüüp. Väljundi väärtused toetada sama tüüpi mis malli sisendparameetrid.
| väärtus          |   Jah    | Malli keele avaldis, mis on hinnatud ja väljundi väärtusena tagastatud.


Järgmises näites on kujutatud väljundid jaotises tagastatav väärtus.

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

Väljundi töötamise kohta leiate lisateavet teemast [ühiskasutuse riigi Azure'i ressursihaldur Mallid](best-practices-resource-manager-state.md).

## <a name="next-steps"></a>Järgmised sammud
- Täieliku malle palju erinevaid lahendusi, leiate [Azure'i Kiirjuhend Mallid](https://azure.microsoft.com/documentation/templates/).
- Üksikasjalikku teavet saate kasutada malli sees funktsioonide kohta leiate teemast [Azure ressursihaldur malli funktsioonid](resource-group-template-functions.md).
- Mitme malli ühendamiseks juurutamisel teemast [lingitud mallide Azure'i ressursihaldur kasutamine](resource-group-linked-templates.md).
- Võib-olla peate kasutama eri ressursirühm olemasolevate ressursid. See stsenaarium on levinud töötamisel salvestusruumi kontod või virtuaalse võrgu, mis ulatub üle mitme ressursi rühmad. Lisateavet leiate teemast [ResourceIdkasutamisel funktsioon](resource-group-template-functions.md#resourceid).


[deployment2cmdlet]: https://msdn.microsoft.com/library/mt740620(v=azure.200).aspx
