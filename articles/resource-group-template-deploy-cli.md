<properties
   pageTitle="Ressursside Azure'i CLI ja malli juurutamine | Microsoft Azure'i"
   description="Kasutage Azure'i ressursihaldur ja Azure CLI on ressursid Azure. Ressursside on määratletud ressursihaldur malli."
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
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Ressursside ressursihaldur mallide ja Azure CLI juurutamine

> [AZURE.SELECTOR]
- [PowerShelli](resource-group-template-deploy.md)
- [Azure'i CLI](resource-group-template-deploy-cli.md)
- [Portaal](resource-group-template-deploy-portal.md)
- [REST API-GA](resource-group-template-deploy-rest.md)

See teema selgitab, kuidas kasutada Azure CLI ressursihaldur malle juurutada Azure teie ressursse.  

> [AZURE.TIP] Tõrke silumine juurutamisel abi järgmistest teemadest.
>
> - [Vaade juurutamise toimingute Azure'i CLI](resource-manager-troubleshoot-deployments-cli.md) kohta teavet, mis aitab teil saada tõrkeotsing oma tõrge
> - [Tõrkeotsing levinud vigade ressursid Azure'i Azure ressursihaldur juurutamisel](resource-manager-common-deployment-errors.md) juurutuse levinud vigade lahendamise kohta teavet

Malli võib olla, kas kohalik fail või välise faili, mis on saadaval URI kaudu. Kui malli asub salvestusruumi konto, saate piirata juurdepääsu Mall ja pakuvad juurutamisel märgiks ühiskasutusega juurdepääsu allkirja (SAS).

## <a name="quick-steps-to-deployment"></a>Kiirtoimingud juurutamine

Selles artiklis kirjeldatakse saadaolevate juurutamisel erinevaid võimalusi. Sageli ainult peate siiski kaks lihtsat käsud. Kiire alustamine juurutus, kasutage järgmised käsud:

    azure group create -n ExampleResourceGroup -l "West US"
    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

Jätkake juurutamine, mis võib olla paremini stsenaariumist võimaluste kohta lisateabe saamiseks selle artikli lugemist.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-azure-cli"></a>Azure'i CLI juurutamine

Kui te pole varem kasutanud Azure'i CLI koos ressursihaldur, leiate [Azure'i CLI Mac, Linux, ja Windows Azure'i ressursside haldamise abil](xplat-cli-azure-resource-manager.md).

1. Azure'i kontosse sisse logida. Pärast oma mandaatide käsk tagastab tulemi oma kasutajanimi.

        azure login
  
        ...
        info:    login command OK

2. Kui teil on mitu tellimust, sisestage tellimuse id, mida soovite kasutada juurutamiseks.

        azure account set <YourSubscriptionNameOrId>

3. Aktiveerige Azure'i ressursihaldur mooduli. Saate kinnituse uue režiimi.

        azure config mode arm
   
        info:     New mode is arm

4. Kui teil pole olemasoleva ressursi rühma, luua ressursirühma. Sisestage nimi ja ressursirühm asukohta, et peate oma lahenduse. Tuleb sisestada ressursirühma asukoht, kuna ressursirühma talletab metaandmete ressursside kohta. Nõuetele vastavuse tagamiseks, võite määrata, et metaandmete talletuskoht. Soovitame üldiselt, määrake asukoht, kus enamik oma ressursse hakkavad asuma. Samas kohas abil saate lihtsustada malli.

        azure group create -n ExampleResourceGroup -l "West US"

     Uue ressursirühma kokkuvõtte tagastatakse.
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Kinnitage oma juurutuse enne selle käsu **kinnitage azure rühma malli** abil. Juurutamise testimisel anda parameetrid täpselt samuti nagu teeksite täitmisel juurutamise (näidatud järgmise juhise juurde).

        azure group template validate -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup

5. Juurutada ressursid ressursirühma, käivitage järgmine käsk ja sisestage vajalikud parameetrid. Parameetrid sisaldavad juurutamise nimi, mall ja muud parameetrid, milline olukord teie jaoks vajalik ressursirühma, tee või URL-i nimi. 
   
     On teil järgmised kolm võimalust osutamiseks parameetrite väärtused. 

     1. Tekstisisene parameetrite ja kohaliku malli kasutada. Iga parameetri on vormingus: `"ParameterName": { "value": "ParameterValue" }`. Järgmises näites on kujutatud koos paomärke parameetrid.

            azure group deployment create -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     2. Tekstisisene parameetrite ja lingi malli kasutada.

            azure group deployment create --template-uri <LinkToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     3. Kasutage parameetrit faili. Mallifail kohta leiate teavet teemast [parameetri fail](#parameter-file).
    
            azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Pärast ressursside on üks kolmest meetoditega kaudu kasutusele võetud, kuvatakse juurutamise kokkuvõte.
  
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        ...
        info:    group deployment create command OK

     Täieliku juurutamise käivitamiseks seatud **režiimi** **lõpuleviimine**. Olge ettevaatlik kasutamise selle režiimi saate kustutada tahtmatult ressursse, mis pole teie malli.

        azure group deployment create --mode Complete -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

6. Kui soovite lisateavet juurutamise, mis võivad aidata teil tõrkeotsing juurutamise vigu, kasutage parameetrit **silumine-säte** sisse logida. Saate määrata taotluse sisu, vastuse sisu või mõlemad logitud juurutamise toiminguga.

        azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

## <a name="deploy-template-from-storage-with-sas-token"></a>Salvestusruumi SAS luba malli juurutamine

Saate lisada oma mallid salvestusruumi konto ja linkida ajal juurutus, nii et SAS sõnet.

> [AZURE.IMPORTANT] Alltoodud juhiseid järgides bloobimälu, mis sisaldab mall on juurdepääsetav ainult konto omanik. Siiski SAS luba jaoks soovitud bloobimälu loomisel kuvatakse bloobimälu on kättesaadav kõigile, kellel on see URI. Kui teise kasutaja intercepts URI, see kasutaja on Mall juurde pääseda. Märgiks SAS kaudu on hea võimalus oma mallide juurdepääsu piiramise, kuid ei tohiks hõlmata tundlikku teavet nagu paroolid otse sisse mall.

### <a name="add-private-template-to-storage-account"></a>Privaatne malli salvestusruumi konto lisamine

Salvestusruumi konto Mallid järgmist:

1. Ressursi rühma loomine.

        azure group create -n "ManageGroup" -l "westus"

2. Looge konto salvestusruumi. Salvestusruumikonto nimi peab olema kordumatu Azure'i üle, et anda oma konto nime.

        azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates

3. Määrake muutujate salvestusruumi konto ja võti.

        export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
        export AZURE_STORAGE_ACCESS_KEY={storage_account_key}

4. Looge ümbris. Luba seatakse **välja** mis tähendab, et ümbris pääseb ainult omanik.

        azure storage container create --container templates -p Off 
        
4. Malli lisamine ümbris.

        azure storage blob upload --container templates -f c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Sisestage SAS luba juurutamisel

Privaatne malli salvestusruumi konto juurutamiseks tuua märgiks SAS ja kaasamine URI malli.

1. Luua lugemisõigust SAS luba ja juurdepääsu piiramiseks soovitud aeg. Seada juurutuse lõpuleviimiseks piisavalt aega. Saate tuua täielik URI malli, sh SAS sõnet.

        expiretime=$(date -I'minutes' --date "+30 minutes")
        fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")

2. Malli esitada URI, mis sisaldab SAS sõnet.

        azure group deployment create --template-uri $fullurl -g ExampleResourceGroup

Näiteks SAS sõnet lingitud mallide abil, vt [lingitud mallide Azure'i ressursihaldur kasutamine](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Järgmised sammud
- Näiteks juurutamine .net-i kliendi teek abil, vt [Deploy ressursid .NET teekide ja malli abil](virtual-machines/virtual-machines-windows-csharp-template.md).
- Parameetrite määratlemine malli, lugege teemat [kasutamisel Mallid](resource-group-authoring-templates.md#parameters).
- Oma lahenduse juurutamine viibite juhised leiate teemast [arendamine ja testi keskkonnas Microsoft Azure](solution-dev-test-environments.md).
- Turvaline väärtused läbida viite KeyVault kasutamise kohta lisateavet [turvaline väärtuste juurutamisel edasi](resource-manager-keyvault-parameter.md).

