<properties
   pageTitle="Ressursside PowerShelli ja malli juurutamine | Microsoft Azure'i"
   description="Juurutamine on ressursid Azure'i Azure ressursihaldur ja Azure PowerShelli abil. Ressursside on määratletud ressursihaldur malli."
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

# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Ressursside ressursihaldur mallide ja Azure PowerShelli juurutamine

> [AZURE.SELECTOR]
- [PowerShelli](resource-group-template-deploy.md)
- [Azure'i CLI](resource-group-template-deploy-cli.md)
- [Portaal](resource-group-template-deploy-portal.md)
- [REST API-GA](resource-group-template-deploy-rest.md)

See teema selgitab, kuidas ressursihaldur mallide abil oma ressursse juurutama Azure'i Azure PowerShelli kasutamine.  

> [AZURE.TIP] Tõrke silumine juurutamisel abi järgmistest teemadest.
>
> - [Vaate juurutamise toimingute Azure'i PowerShelliga](resource-manager-troubleshoot-deployments-powershell.md) kohta teavet, mis aitab teil saada tõrkeotsing oma tõrge
> - [Tõrkeotsing levinud vigade ressursid Azure'i Azure ressursihaldur juurutamisel](resource-manager-common-deployment-errors.md) juurutuse levinud vigade lahendamise kohta teavet

Malli võib olla, kas kohalik fail või välise faili, mis on saadaval URI kaudu. Kui malli asub salvestusruumi konto, saate piirata juurdepääsu Mall ja pakuvad juurutamisel märgiks ühiskasutusega juurdepääsu allkirja (SAS).

## <a name="quick-steps-to-deployment"></a>Kiirtoimingud juurutamine

Selles artiklis kirjeldatakse saadaolevate juurutamisel erinevaid võimalusi. Sageli ainult peate siiski kaks lihtsat käsud. Kiire alustamine juurutus, kasutage järgmised käsud:

    New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
    New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

Jätkake juurutamine, mis võib olla paremini stsenaariumist võimaluste kohta lisateabe saamiseks selle artikli lugemist.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-powershell"></a>PowerShelliga juurutamine

1. Azure'i kontosse sisse logida.

        Add-AzureRmAccount

     Tagastatakse konto kokkuvõte.

        Environment : AzureCloud
        Account     : someone@example.com
        ...

2. Kui teil on mitu tellimust, sisestage Tellimuse ID, mida soovite kasutada käsu **Set-AzureRmContext** juurutamiseks. 

        Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

3. Tavaliselt uue malli juurutamisel soovite luua sisaldama ressursside ressursirühma. Kui teil on olemasoleva ressursirühm, mida soovite võtta kasutusele, saate selle sammu vahele jätta ja kasutada seda ressursirühma. 

     Ressursi rühma loomiseks, sisestage nimi ja asukoht ressursirühma. Tuleb sisestada ressursirühma asukoht, kuna ressursirühma talletab metaandmete ressursside kohta. Nõuetele vastavuse tagamiseks, võite määrata, et metaandmete talletuskoht. Soovitame üldiselt, määrake asukoht, kus enamik oma ressursse hakkavad asuma. Samas kohas abil saate lihtsustada malli.

        New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   
     Uue ressursirühma kokkuvõtte tagastatakse.
   
        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
             Actions  NotActions
             =======  ==========
             *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

4. Enne juurutamise, saate oma juurutamise sätted kinnitada. **Test-AzureRmResourceGroupDeployment** cmdlet-käsk võimaldab teil enne loomise tegelik ressursid probleemide otsimine. Järgmises näites kujutatakse valideerimiseks juurutamine.

        Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

5. Ressursside juurutamiseks ressursirühma käsu **New-AzureRmResourceGroupDeployment** ja sisestage vajalikud parameetrid. Parameetrid lisage juurutamise nimi, olete loonud malli ressursirühma, tee või URL-i nimi ja muud parameetrid, milline olukord teie jaoks vajalik. Kui parameeter **režiimis** pole määratud, kasutatakse **Inkrementmuutus** vaikeväärtus. Täieliku juurutamise käivitamiseks seatud **režiimi** **lõpuleviimine**. Olge ettevaatlik, nagu saate kustutada tahtmatult ressursse, mis pole teie malli täieliku režiimi kasutades.

     Kohaliku malli juurutada, kasutage parameetrit **TemplateFile** :

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

     Juurutamine mõni välise Mall, kasutage parameetrit **TemplateUri** :

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate>
   
     Teil esitada parameetrite väärtused järgmised suvandid. 
   
     1. Tekstisisene parameetrite kasutamine.

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"

     2. Kasutage parameetrit objekti.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters

     3. Kasutage faili kohaliku parameeter. Mallifail kohta leiate teavet teemast [parameetri fail](#parameter-file).

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

     4. Kasutada välise parameetri faili. Mallifail kohta leiate teavet teemast [parameetri fail](#parameter-file). 

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri <LinkToParameterFile>

        Failina välise parameetri kasutamisel ei saa läbi muid väärtusi, kas Tekstisisese või kohalik fail. Lisateavet leiate teemast [järjestuse parameeter](#parameter-precendence).

     Pärast ressursside on kasutusele võetud, kuvatakse juurutamise kokkuvõte.

        DeploymentName    : ExampleDeployment
        ResourceGroupName : ExampleResourceGroup
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2015 7:00:27 PM
        Mode              : Incremental
        ...

     Kui teie Mall sisaldab sama nimega üks parameetritest parameetri PowerShelli käsu, palutakse sisestada selle parameetri väärtuse. Parameetri mallist sisaldab postfix **FromTemplate**. Näiteks parameetri nime **ResourceGroupName** teie mall on vastuolus cmdlet-käsu [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) **ResourceGroupName** parameeter. Teil palutakse **ResourceGroupNameFromTemplate**väärtuse ette. Üldiselt tuleks vältida seda segadust pole nime panemise parameetrid koos parameetrite juurutamise toimingute puhul kasutada sama nimi.

6. Kui soovite lisateavet juurutamise, mis võivad aidata teil tõrkeotsing juurutamise vigu, kasutage parameetrit **DeploymentDebugLogLevel** sisse logida. Saate määrata taotluse sisu, vastuse sisu või mõlemad logitud juurutamise toiminguga.

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
        
     Tõrkeotsingu juurutuste silumine sisu kasutamise kohta leiate lisateavet teemast [tõrkeotsing ressursi rühma juurutuste Azure PowerShelli abil](resource-manager-troubleshoot-deployments-powershell.md).

## <a name="deploy-template-from-storage-with-sas-token"></a>Salvestusruumi SAS luba malli juurutamine

Saate lisada oma mallid salvestusruumi konto ja linkida ajal juurutus, nii et SAS sõnet.

> [AZURE.IMPORTANT] Alltoodud juhiseid järgides bloobimälu, mis sisaldab mall on juurdepääsetav ainult konto omanik. Siiski SAS luba jaoks soovitud bloobimälu loomisel kuvatakse bloobimälu on kättesaadav kõigile, kellel on see URI. Kui teise kasutaja intercepts URI, see kasutaja on Mall juurde pääseda. Märgiks SAS kaudu on hea võimalus oma mallide juurdepääsu piiramise, kuid ei tohiks hõlmata tundlikku teavet nagu paroolid otse sisse mall.

### <a name="add-private-template-to-storage-account"></a>Privaatne malli salvestusruumi konto lisamine

Salvestusruumi konto Mallid järgmist:

1. Ressursi rühma loomine.

        New-AzureRmResourceGroup -Name ManageGroup -Location "West US"

2. Looge konto salvestusruumi. Salvestusruumikonto nimi peab olema kordumatu Azure'i üle, et anda oma konto nime.

        New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"

3. Määrake praeguse salvestusruumi konto.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

4. Looge ümbris. Luba seatakse **välja** mis tähendab, et ümbris pääseb ainult omanik.

        New-AzureStorageContainer -Name templates -Permission Off
        
5. Malli lisamine ümbris.

        Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Sisestage SAS luba juurutamisel

Privaatne malli salvestusruumi konto juurutamiseks tuua märgiks SAS ja kaasamine URI malli.

1. Kui olete muutnud praeguse salvestusruumi konto, praeguse salvestusruumi konto määramine üks, mis sisaldab teie mallid.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

2. Luua lugemisõigust SAS luba ja juurdepääsu piiramiseks soovitud aeg. Saate tuua täielik URI malli, sh SAS sõnet.

        $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

3. Malli esitada URI, mis sisaldab SAS sõnet.

        New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri

Näiteks SAS sõnet lingitud mallide abil, vt [lingitud mallide Azure'i ressursihaldur kasutamine](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="parameter-precedence"></a>Järjestuse parameeter.

Tekstisisene parameetrite ja kohaliku parameetri faili sama juurutamise käigus saate kasutada. Näiteks saate määrata teatud väärtuste faili kohaliku parameeter ja lisada muid väärtusi Tekstisisese juurutamisel. Kui väärtused kohaliku parameetri faili ja Tekstisisese parameetri ette Tekstisisese väärtus alistab.

Siiski ei saa kasutada Tekstisisese parameetrite välise parameetri failiga. Kui määrate parameetri faili parameetrit **TemplateParameterUri** , ignoreeritakse Tekstisisese kõik parameetrid. Peate esitama välise faili kõigi parameetrite väärtused. Kui teie Mall sisaldab tundliku loomuga väärtus, mis ei tohi sisaldada, parameeter faili, selle väärtuse lisamine soovitud klahv hoidlasse ja viidata võtme vault välise parameeter faili või dünaamiliselt pakuvad kõigi parameetrite väärtused teksti sees.

Turvaline väärtused läbida viite KeyVault kasutamise kohta lisateavet [turvaline väärtuste juurutamisel edasi](resource-manager-keyvault-parameter.md).

## <a name="next-steps"></a>Järgmised sammud
- Näiteks juurutamine .net-i kliendi teek abil, vt [Deploy ressursid .NET teekide ja malli abil](virtual-machines/virtual-machines-windows-csharp-template.md).
- Parameetrite määratlemine malli, lugege teemat [kasutamisel Mallid](resource-group-authoring-templates.md#parameters).
- Oma lahenduse juurutamine viibite juhised leiate teemast [arendamine ja testi keskkonnas Microsoft Azure](solution-dev-test-environments.md).

