<properties
    pageTitle="Azure'i ressursihaldur malli eksportida | Microsoft Azure'i"
    description="Azure'i ressursside haldamine abil saate olemasolevasse rühma ressursi malli eksportimine."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="tomfitz"/>

# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Olemasoleva ressursid on Azure ressursihaldur malli eksportimine

Ressursihaldur võimaldab ressursihaldur malli eksportimine olemasolevate ressursside teie tellimus. Loodud malli saate kasutada malli süntaksi kohta või automatiseerimiseks positsioonide tühistamist lahenduse vastavalt vajadusele.

On oluline on seal ekspordiks malli kahel viisil:

- Saate eksportida tegelik malli, mida kasutasite juurutamine. Eksporditud Mall sisaldab parameetrite ja muutujate täpselt nii, nagu need siis ilmus algne mall. See lähenemine on kasulik, kui olete juurutanud portaali kaudu. Nüüd soovite teada, kuidas luua malli loomiseks need ressursid.
- Saate eksportida Mall, mis tähistab praeguse oleku ressursirühma. Eksporditud malli ei põhine mõnda malli, mida kasutasite juurutamiseks. Selle asemel loob malli, mis on hetktõmmise ressursirühma. Eksporditud mall on raske koodiga väärtused ja ilmselt ei ole nii palju parameetreid nagu tavaliselt oleks määratlete. See lähenemine on kasulik, kui olete muutnud kaudu portaali või skriptide ressursirühma. Nüüd, peate jäädvustada ressursirühma mallina.

Selles teemas kirjeldatakse mõlema lähenemisel.

Selles õppetükis saate Azure portaali sisse logida, salvestusruumi konto loomine ja eksportimine malli salvestusruumi konto jaoks. Saate lisada virtuaalse võrgu muutmiseks ressursirühma. Lõpetuseks, saate eksportida uue malli, mis tähistab selle praeguses olekus. Kuigi see artikkel keskendub lihtsustatud taristu, võite kasutada malli keerulisem lahenduse eksportimiseks samad juhised.

## <a name="create-a-storage-account"></a>Salvestusruumi konto loomine

1. [Azure'i portaalis](https://portal.azure.com)valige **Uus** > **salvestusruumi** > **salvestusruumi konto**.

      ![salvestusruumi loomine](./media/resource-manager-export-template/create-storage.png)

2. Looge konto salvestusruumi nimi **salvestusruumi**, initsiaalid oma ja kuupäeva. Salvestusruumikonto nimi peab olema kordumatu Azure. Kui nimi on juba kasutusel, näete teadet nimi on kasutusel. Proovige variatsioon. Ressursirühma, saate luua uue ressursirühma ja pange sellele **ExportGroup**. Saate vaikeväärtused muid atribuute. Valige **Loo**.

      ![Sisestage väärtused Storage](./media/resource-manager-export-template/provide-storage-values.png)

Juurutamise võib kuluda mõni minut. Kui juurutamise lõpule jõudnud, sisaldab tellimuse salvestusruumi kontot.

## <a name="view-a-template-from-deployment-history"></a>Malli juurutamise ajaloo kuvamine

1. Minge ressursi rühma tera uue ressursirühma. Pange tähele, et tera näitab viimase juurutamise tulemus. Valige järgmine link.

      ![ressursi rühma blade](./media/resource-manager-export-template/resource-group-blade.png)

2. Rühma juurutuste ajaloo kuvamine Teie puhul on loetletud tera ilmselt ainult üks juurutamise. Valige see juurutus.

     ![viimase juurutamine](./media/resource-manager-export-template/last-deployment.png)

3. Tera kuvab juurutamise kokkuvõtte. Kokkuvõte sisaldab oleku ja juurutamise ja esitatud parameetrite väärtused. Malli, mida kasutasite kasutuselevõtuks vaatamiseks valige **Kuva Mall**.

     ![Kuva juurutamise Kokkuvõte](./media/resource-manager-export-template/deployment-summary.png)

4. Ressursihaldur toob järgmised kuus failid saate:

   1. **Malli** - Mall, mis määratleb infrastruktuuri teie lahendus. Portaali kaudu salvestusruumi konto loomisel ressursihaldur malli abil saate juurutada ja sellel mallil edaspidiseks salvestada.
   2. **Parameetrite** - parameeter faili, mida saate läbida väärtused käigus juurutamist. See sisaldab väärtusi, esimese juurutamise käigus ette, kuid need väärtused saate muuta, kui te ümberkorraldamine mall.
   3. **CLI** - An Azure'i command liik kasutajaliidese (CLI) skriptifail malli kasutavate.
   4. **PowerShelli** - An Azure PowerShelli skriptifail, mille abil saate juurutada mall.
   5. **.NET** - A .NET ainekursuse, mille abil saate juurutada mall.
   6. **Ruby** - A Ruby klassi, mille abil saate juurutada mall.

     Failid on kogu tera linkide kaudu. Vaikimisi kuvatakse tera mall.

       ![vaate Mall](./media/resource-manager-export-template/view-template.png)

     Vaatame erilist tähelepanu mall. Malli peaks sarnanema järgmisele.

        {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "parameetrid": {"nimi": {"tüüp": "String"}, "accountType": {"tüüp": "String"}, "asukoht": {"tüüp": "String"}, "encryptionEnabled": {"defaultValue": väär, "tüüp": "Bool"}}, "ressursid": [{"tüüp": "Microsoft.Storage/storageAccounts", "sku": {"nimi": "[parameters('accountType')]"}, "selline": "Storage", "nimi": "[parameters('name')]", "apiVersion": "2016-01-01", "asukoht": "[parameters('location')]", "atribuudid": {"krüptimise": {"teenused": {"bloobimälu": {"lubatud": "[parameters('encryptionEnabled')]"}}, "keySource": "Microsoft.Storage"}}}]}
 
See mall on tegelik malli abil luua oma konto salvestusruumi. Pange tähele, et see sisaldab parameetrit, mis võimaldavad teil juurutada erinevat tüüpi salvestusruumi kontod. Malli struktuuri kohta leiate lisateavet teemast [Azure ressursihaldur loome Mallid](resource-group-authoring-templates.md). Malli saate kasutada funktsiooni täieliku loendi leiate teemast [Azure ressursihaldur malli funktsioonid](resource-group-template-functions.md).


## <a name="add-a-virtual-network"></a>Virtuaalse võrgu lisamine

Eelmises jaotises allalaaditud malli, mida tähistab taristu selle algse juurutamiseks. Aga see pole arvele pärast juurutamise muudatused.
Probleemi kujutamiseks vaatame muuta ressursirühma liites virtuaalse võrgu portaali kaudu.

1. Ressursi rühma tera, valige **Lisa**.

      ![ressursi lisamine](./media/resource-manager-export-template/add-resource.png)

2. Valige **Virtual võrgu** ressursid.

      ![Valige virtuaalse võrgu](./media/resource-manager-export-template/select-vnet.png)

2. **VNET**virtuaalse võrgu nimi ja muud atribuudid on vaikeväärtuste kasutamine. Valige **Loo**.

      ![teatise määramine](./media/resource-manager-export-template/create-vnet.png)

3. Pärast virtuaalse võrgu on edukalt juurutatud ressursirühma, vaadake uuesti juurutamise ajalugu. Nüüd näete kahte juurutuste. Kui te ei näe teise juurutus, peate oma ressursi rühma blade sulgeda ja uuesti avada. Valige uuemasse juurutamise.

      ![juurutamise ajalugu](./media/resource-manager-export-template/deployment-history.png)

4. Saate vaadata selle juurutamiseks mall. Pange tähele, et see määratleb ainult virtuaalse võrku. Ei sisalda salvestusruumi konto, saate juurutada varasemas versioonis. Teil pole enam Mall, mis tähistab kõigi ressursside kohta ressursirühma.

## <a name="export-the-template-from-resource-group"></a>Malli eksportimine ressursirühm

Praegune olek ressursirühma saamiseks eksportida Mall, mis kuvatakse ressursirühma hetktõmmis.  

> [AZURE.NOTE] Te ei saa eksportida malli ressursirühma, mis on üle 200 ressursse.

1. Malli ressursirühma vaatamiseks valige **automatiseerimise skripti**.

      ![ekspordi ressursirühm](./media/resource-manager-export-template/export-resource-group.png)

     Kõik ressursi toeta funktsiooni ekspordi malli. Kui teie ressursirühm sisaldab ainult salvestusruumi konto ja virtuaalse võrgu selles artiklis, te ei näe tõrge. Juhul, kui olete loonud muud tüüpi ressursi, võidakse kuvada tõrge, et on probleem ekspordi. Saate teada, kuidas hallata neid probleeme, klõpsake jaotises [ekspordi probleemidele](#fix-export-issues) .

      

2. Kuvatakse uuesti kuus failid, mille abil saate ümberkorraldamine lahendus, kuid seekord mall on pisut teistsugused. Selle malli on vaid kahe parameetrid: üks salvestusruumikonto nimi ja üks virtuaalne võrgu nimi.

        "parameters": {
          "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
          },
          "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
          }
        },

     Ressursihaldur ei malle, mida kasutasite juurutamisel ei alla laadida. Selle asemel see luuakse uus mall, mis põhineb ressursside konfiguratsioonile. Näiteks Mall määrab salvestusruumi konto asukoht ja dispersioonanalüüs väärtus:

        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },

3. Teil on mitu võimalust jätkata tööd selle malli abil. Saate malli allalaadimiseks ja töötada koos toimetaja JSON kohalikult. Või saate salvestada malli teeki ja sellega töötada portaali kaudu.

     Kui teil on mugav kasutada näiteks [VS koodi](resource-manager-vs-code.md) või [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)JSON-redaktor, võib eelistate Mall kohalikult alla laadida ja selle redaktori abil. Kui teil on pole häälestanud JSON redaktoriga, võib-olla eelistate portaali kaudu malli redigeerimine. Selles teemas ülejäänud eeldab, et mall on salvestatud teeki portaalis. Siiski saate muudatused sama süntaks malli kas töötab kohalikult JSON toimetaja või portaali kaudu.

     Kohalik töötada, klõpsake nuppu **Laadi alla**.

      ![Malli allalaadimiseks](./media/resource-manager-export-template/download-template.png)

     Portaali kaudu, valige **Lisa teeki**.

      ![lisamine teeki](./media/resource-manager-export-template/add-to-library.png)

     Malli lisamisel teeki, Pange malli nimi ja kirjeldus. Seejärel valige **Salvesta**.

     ![malli seadmine väärtused](./media/resource-manager-export-template/set-template-values.png)

4. Teegis salvestatud malli vaatamiseks valige **rohkem teenuseid**, tippige **Mallid** tulemite filtreerimine, valige **Mallid**.

      ![mallide otsimine](./media/resource-manager-export-template/find-templates.png)

5. Valige mall on salvestatud nimega.

      ![Valige mall](./media/resource-manager-export-template/select-library-template.png)

## <a name="customize-the-template"></a>Selle malli saab kohandada

Eksporditud malli toimib hästi, kui soovite sama salvestusruumi konto ja iga juurutamiseks virtuaalse võrgu loomine. Ressursihaldur annab siiski suvandid, et malle palju suurem paindlikkus juurutamist. Näiteks juurutamisel, võiksite määrata tüüpi salvestusruumi konto loomiseks või kasutamiseks virtuaalse võrgu aadress eesliide ja alamvõrgu eesliide väärtused.

Selles jaotises lisate parameetrite eksporditud malli, et saate uuesti kasutada malli järgmiste ressursside juurutamisel muude keskkonnas. Saate lisada ka mõned funktsioonid malli vähendamiseks tõenäosuse tekib tõrge, kui juurutate oma mall. Te ei pea enam mõelda salvestusruumi konto jaoks kordumatu nimi. Selle asemel mall loob kordumatu nimi. Saate piirata salvestusruumi kontotüübi ainult kehtiv suvandid määratud väärtused.

1. Valige **Redigeeri** malli kohandamiseks.

     ![Kuva Mall](./media/resource-manager-export-template/show-template.png)

1. Valige mall.

     ![malli redigeerimine](./media/resource-manager-export-template/edit-template.png)

1. Liigu väärtused, mida võiksite määrata juurutamisel saaks asendada uue parameetri määratlused jaotise **Parameetrid** . Pange tähele **allowedValues** jaoks **storageAccount_accountType**väärtused. Kui annate kogemata sobimatu väärtuse, tuvastatakse viga enne juurutamise algust. Samuti märkate, et te esitate ainult eesliite salvestusruumikonto nimi ja eesliide on piiratud 11 märki. Eesliite 11 märkideks piiramiseks tagate täielik nimi ei ületa salvestusruumi konto märkide arvu ülempiir. Eesliite võimaldab teil rakendada nimereeglistik salvestusruumi kontod. Näete, kuidas luua järgmise juhise juurde kordumatu nimi.

        "parameters": {
          "storageAccount_prefix": {
            "type": "string",
            "maxLength": 11
          },
          "storageAccount_accountType": {
            "defaultValue": "Standard_RAGRS",
            "type": "string",
            "allowedValues": [
              "Standard_LRS",
              "Standard_ZRS",
              "Standard_GRS",
              "Standard_RAGRS",
              "Premium_LRS"
            ]
          },
          "virtualNetwork_name": {
            "type": "string"
          },
          "addressPrefix": {
            "defaultValue": "10.0.0.0/16",
            "type": "string"
          },
          "subnetName": {
            "defaultValue": "subnet-1",
            "type": "string"
          },
          "subnetAddressPrefix": {
            "defaultValue": "10.0.0.0/24",
            "type": "string"
          }
        },

2. Malli **muutujate** osas on tühi. Jaotises **muutujate** loote lihtsustada süntaks malli ülejäänud väärtusi. Selles jaotises asendada uute muutuv määratlus. **StorageAccount_name** muutuja ühendab eesliite parameeter kordumatu string, mis on loodud põhjal identifikaator ressursirühma. Teil pole enam mõelda kordumatu nimi parameetri väärtuse esitamisel.

        "variables": {
          "storageAccount_name": "[concat(parameters('storageAccount_prefix'), uniqueString(resourceGroup().id))]"
        },

3. Parameetrite ja muutuja kasutamine ressursi määratlused asendada uue ressursi määratlused jaotise **ressursse** . Teade, et vähe muutunud ressursi määratlused peale väärtus, mis on määratud atribuudi ressurss. Atribuudid on sama mis eksporditud malli atribuudid. Parameetrite väärtused kõva kodeeritud väärtuste asemel on lihtsalt atribuutide määramine. Ressursside asukoht on seatud kasutada **resourceGroup () .location** avaldise läbi ressursirühma samasse asukohta. Muutuja salvestusruumikonto nimi loodud viidatud **muutujate** avaldise kaudu.

        "resources": [
          {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetwork_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "properties": {
              "addressSpace": {
                "addressPrefixes": [
                  "[parameters('addressPrefix')]"
                ]
              },
              "subnets": [
                {
                  "name": "[parameters('subnetName')]",
                  "properties": {
                    "addressPrefix": "[parameters('subnetAddressPrefix')]"
                  }
                }
              ]
            },
            "dependsOn": []
          },
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccount_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "tags": {},
            "properties": {
                "accountType": "[parameters('storageAccount_accountType')]"
            },
            "dependsOn": []
          }
        ]

4. Kui olete lõpetanud, klõpsake **nuppu OK** malli redigeerimine.

5. **Salvestage** muudatused salvestada malli valimine

     ![malli salvestamine](./media/resource-manager-export-template/save-template.png)

6. Värskendatud malli, valige **Deploy**.

     ![malli juurutamine](./media/resource-manager-export-template/deploy-template.png)

7. Sisestage parameetrite väärtused ja valige juurutamiseks ressursse, mis uue ressursirühma.

## <a name="update-the-downloaded-parameters-file"></a>Parameetrite allalaaditud faili värskendamist.

Kui töötate allalaaditud failid (mitte portaali teek), peate värskendama faili allalaaditud parameeter. See vastab enam teie malli parameetrid. Peaksite kasutama parameetri faili, kuid seda saate lihtsustada protsessi, kui te ümberkorraldamine keskkonnas. Saate kasutada vaikeväärtused, mida on määratletud malli paljude parameetrid nii, et parameetri faili tuleb ainult kahest väärtusest.

Asendage parameters.json faili sisu:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccount_prefix": {
      "value": "storage"
    },
    "virtualNetwork_name": {
      "value": "VNET"
    }
  }
}
```

Faili värskendatud parameetri pakub väärtused ainult parameetrid, mis on vaikeväärtus. Väärtused saate sisestada muude parameetrite, kui soovite, et väärtus, mis on erinev vaikeväärtus.

## <a name="fix-export-issues"></a>Ekspordi seotud probleemide lahendamine

Kõik ressursi toeta funktsiooni ekspordi malli. Ressursihaldur spetsiaalselt eksportida ressursi Mõned sõnumitüübid, et vältida tundliku teabe asetades. Näiteks kui teil on oma saidi config ühendusstringi, te ilmselt ei soovi seda selgesõnaliselt eksporditud mallis kuvada. Saate selle probleemi lahendamiseks käsitsi lisada puuduvad ressursse uuesti sisse malli.

> [AZURE.NOTE] Ainult ekspordi Probleemid ilmneda eksportimise ressursirühma pigem ajaloo juurutamise. Kui viimase juurutamise tähistab täpselt ressursirühma hetkeseisu, tuleks malli eksportimist juurutamise ajaloo pigem ressursirühma. Ainult eksportimine ressursirühma, kui olete muutnud ressursirühma, mis on määratletud ühe malli.

Näiteks kui ekspordite ressursirühm, mis sisaldab web app, SQL-andmebaasiga, ja ühendusstringi saidi config malli, kuvatakse järgmine teade:

![kuvatakse tõrketeade](./media/resource-manager-export-template/show-error.png)

Valige sõnumi näete, täpselt resource milliste ei ekspordita. 
     
![kuvatakse tõrketeade](./media/resource-manager-export-template/show-error-details.png)

Selles teemas kirjeldatakse levinud parandused.

### <a name="connection-string"></a>Ühendusstring

Lisage veebisaitide ressurss, andmebaasi ühendusstringi määratlus:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "type": "config",
      "name": "connectionstrings",
      "dependsOn": [
          "[concat('Microsoft.Web/Sites/', parameters('<site-name>'))]"
      ],
      "properties": {
          "DefaultConnection": {
            "value": "[concat('Data Source=tcp:', reference(concat('Microsoft.Sql/servers/', parameters('<database-server-name>'))).fullyQualifiedDomainName, ',1433;Initial Catalog=', parameters('<database-name>'), ';User Id=', parameters('<admin-login>'), '@', parameters('<database-server-name>'), ';Password=', parameters('<admin-password>'), ';')]",
              "type": "SQLServer"
          }
      }
    }
  ]
}
```    

### <a name="web-site-extension"></a>Veebisaidi laiend

Veebisaidi ressurss, lisage koodi installida määratlus:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "name": "MSDeploy",
      "type": "extensions",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [
        "[concat('Microsoft.Web/sites/', parameters('<site-name>'))]"
      ],
      "properties": {
        "packageUri": "[concat(parameters('<artifacts-location>'), '/', parameters('<package-folder>'), '/', parameters('<package-file-name>'), parameters('<sas-token>'))]",
        "dbType": "None",
        "connectionString": "",
        "setParameters": {
          "IIS Web Application Name": "[parameters('<site-name>')]"
        }
      }
    }
  ]
}
```

### <a name="virtual-machine-extension"></a>Virtuaalse masina laiend

Näiteid virtuaalse masina laiendid, leiate [Azure'i Windows VM laiend konfiguratsiooni näidised](./virtual-machines/virtual-machines-windows-extensions-configuration-samples.md).

### <a name="virtual-network-gateway"></a>Virtuaalse võrgu lüüsi

Lisage virtuaalse võrgu lüüsi ressursi tüüp.

```
{
  "type": "Microsoft.Network/virtualNetworkGateways",
  "name": "[parameters('<gateway-name>')]",
  "apiVersion": "2015-06-15",
  "location": "[resourceGroup().location]",
  "properties": {
    "gatewayType": "[parameters('<gateway-type>')]",
    "ipConfigurations": [
      {
        "name": "default",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('<vnet-name>'), parameters('<new-subnet-name>'))]"
          },
          "publicIpAddress": {
            "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('<new-public-ip-address-Name>'))]"
          }
        }
      }
    ],
    "enableBgp": false,
    "vpnType": "[parameters('<vpn-type>')]"
  },
  "dependsOn": [
    "Microsoft.Network/virtualNetworks/codegroup4/subnets/GatewaySubnet",
    "[concat('Microsoft.Network/publicIPAddresses/', parameters('<new-public-ip-address-Name>'))]"
  ]
},
```

### <a name="local-network-gateway"></a>Lüüsi kohalikus võrgus

Lisage kohtvõrgu lüüsi ressursi tüüp.

```
{
    "type": "Microsoft.Network/localNetworkGateways",
    "name": "[parameters('<local-network-gateway-name>')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "localNetworkAddressSpace": {
        "addressPrefixes": "[parameters('<address-prefixes>')]"
      }
    }
}
```

### <a name="connection"></a>Ühendus

Lisage ühenduse ressursi tüüp.

```
{
    "apiVersion": "2015-06-15",
    "name": "[parameters('<connection-name>')]",
    "type": "Microsoft.Network/connections",
    "location": "[resourceGroup().location]",
    "properties": {
        "virtualNetworkGateway1": {
        "id": "[resourceId('Microsoft.Network/virtualNetworkGateways', parameters('<gateway-name>'))]"
      },
      "localNetworkGateway2": {
        "id": "[resourceId('Microsoft.Network/localNetworkGateways', parameters('<local-gateway-name>'))]"
      },
      "connectionType": "IPsec",
      "routingWeight": 10,
      "sharedKey": "[parameters('<shared-key>')]"
    }
},
```


## <a name="next-steps"></a>Järgmised sammud

Palju õnne! Olete õppinud, kuidas malli eksportimine portaali loodud ressursid.

- Saate juurutada malli [PowerShelli](resource-group-template-deploy.md), [Azure'i CLI](resource-group-template-deploy-cli.md)või [REST API -ga](resource-group-template-deploy-rest.md).
- Kuidas eksportida malli PowerShelli kaudu, leiate teemast [Azure ressursihaldur Azure PowerShelli kasutamine](powershell-azure-resource-manager.md).
- Malli kaudu Azure'i CLI eksportimisest, leiate teemast [Azure CLI Mac, Windows Azure'i ressursihaldur ja Linux, kasutage](xplat-cli-azure-resource-manager.md).
