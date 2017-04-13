<properties 
    pageTitle="Azure'i PowerShelli abil ressursihaldur | Microsoft Azure'i" 
    description="Sissejuhatus Azure'i PowerShelli abil saate juurutada mitme ressursid Azure ressursi rühmana." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="tomfitz"/>

# <a name="using-azure-powershell-with-azure-resource-manager"></a>Azure'i ressursihaldur Azure PowerShelli abil

> [AZURE.SELECTOR]
- [Portaal](azure-portal/resource-group-portal.md) 
- [Azure'i CLI](xplat-cli-azure-resource-manager.md)
- [Azure'i PowerShelli](powershell-azure-resource-manager.md)
- [REST API-GA](resource-manager-rest-api.md)


Azure'i ressursihaldur rakendab tänapäevane lähenemine Azure ressursse elutsükli juhtelemendile. Asemel loomise ja haldamise üksikute ressursse, saate Alustuseks kujutada kogu lahenduse, nt ajaveebi, fotogalerii, SharePoint portal või viki. --Deklaratiivseid kujutis lahendus--malli abil saate määratleda ressursirühm, mis sisaldab kõiki ressursse, peate toetavad lahendus. Seejärel saate juurutada ja hallata selle ressursirühma loogilise üksusena. 

Selles õppetükis saate teada, kuidas Azure'i ressursihaldur Azure PowerShelli kasutamine. See juhendab teid lahenduse juurutamine ja selle lahenduse töötamine. Azure'i PowerShelli ja ressursihaldur malli abil saab juurutamine:

- SQL serveri andmebaasi majutamiseks-
- SQL-andmebaasi - andmete talletamiseks
- Tulemüüri reeglid - andmebaasiga ühenduse web appi lubamine
- Rakenduse teenusleping - määratlemiseks võimalused ja maksumus web app
- Www - töötab web app
- Web config - andmebaasi ühendusstringi talletamiseks 
- Teatiste reeglid – jõudluse kontrollimise ja tõrked
- -Rakenduse ülevaated mastaabi automaatselt sätted

## <a name="prerequisites"></a>Eeltingimused

Selle õpetuse tegemiseks vajate:

- Azure'i konto
  + Saate [avada Azure'i konto tasuta](/pricing/free-trial/?WT.mc_id=A261C142F): saate krediiti saate proovida makstud Azure'i teenuste ja isegi juhul, kui nad kasutada kuni hoiate konto ja kasutage tasuta Azure teenused, nt veebisaidid. Krediitkaardi kunagi tuleb tasuda, kui te just teiega sätete muutmine ja paluge võetakse.
  
  + Saate [aktiveerida MSDN-i abonendi eelised](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): teie MSDN-i tellimuse annab teile krediiti iga kuu makstud Azure'i teenuste kasutatavad.
- Azure'i PowerShelli 1.0. Lisateabe saamiseks selles versioonis ja kuidas seda installida, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](powershell-install-configure.md).

Selles õpetuses on mõeldud PowerShelli algajatele, kuid eeldatakse, et mõistate põhimõtted, nt moodulid, cmdlet-käskude ja seansid.

## <a name="get-help-for-cmdlets"></a>Cmdlet-käskude kohta abi saamiseks

Mis tahes cmdlet, mis kuvatakse selles õpetuses üksikasjalikku abi saamiseks kasutage cmdlet abi saamiseks. 

    Get-Help <cmdlet-name> -Detailed

Näiteks tippige Get-AzureRmResource cmdlet-käsu jaoks abi saamiseks:

    Get-Help Get-AzureRmResource -Detailed

Cmdlet-käskude loendi ressursid mooduli kokkuvõtte abi saamiseks tippige: 

    Get-Command -Module AzureRM.Resources | Get-Help | Format-Table Name, Synopsis

Väljund näeb välja umbes järgmine väljavõte:

    Name                                   Synopsis
    ----                                   --------
    Find-AzureRmResource                   Searches for resources using the specified parameters.
    Find-AzureRmResourceGroup              Searches for resource group using the specified parameters.
    Get-AzureRmADGroup                     Filters active directory groups.
    Get-AzureRmADGroupMember               Get a group members.
    ...

Cmdlet-käsu jaoks täielik abi, tippige käsu vorming.

    Get-Help <cmdlet-name> -Full
  
## <a name="login-to-your-azure-account"></a>Azure'i kontosse sisselogimine

Enne töötab teie lahendus, peate oma kontole sisse logida.

Azure'i kontosse sisse logida, kasutage cmdlet-käsu **Lisa-AzureRmAccount** .

    Add-AzureRmAccount

Cmdlet teilt Azure'i kontosse sisselogimise. Pärast sisselogimist, allalaadimist Kontosätete nii, et need on saadaval Azure PowerShelli. 

Konto sätted aeguma nii, et peate värskendama neid aeg-ajalt. Värskendage konto sätteid, käivitage uuesti **Lisa-AzureRmAccount** . 

>[AZURE.NOTE] Ressursihaldur moodulid jaoks on vaja lisa-AzureRmAccount. Avaldamine sätete fail ei piisa.     

Kui teil on mitu tellimust, sisestage tellimuse id, mida soovite kasutada juurutus, nii et cmdlet **Set-AzureRmContext** .

    Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

## <a name="create-a-resource-group"></a>Ressursi rühma loomine

Enne tellimuse kasutusele võtavad ressursse, peate looma ressursside sisaldavate ressursirühma. 

Ressursi rühma loomiseks kasutada cmdlet-käsu **New-AzureRmResourceGroup** .

Käsk kasutab **nimi** parameetri nimi selle asukoha määramine ressursirühma ja parameetri **asukoha** määramiseks. Põhjal, mida me eelmises jaotises avastanud, me kasutame "Lääne USA" asukoht.

    New-AzureRmResourceGroup -Name TestRG1 -Location "West US"
    
Väljund on sarnane:

    ResourceGroupName : TestRG1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1

Teie ressursirühm on loodud.

## <a name="deploy-your-solution"></a>Oma lahenduse juurutamine

See teema ei näitavad, kuidas luua malli või arutada struktuuri mall. Seda teavet leiate teemast [Azure ressursihaldur loome Mallid](resource-group-authoring-templates.md) ja [Ressursihaldur Mall ülevaade](resource-manager-template-walkthrough.md). Juurutamist on eelmääratletud [sätte Web Appi abil SQL-andmebaasi](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) malli [Azure'i Kiirjuhend mallide](https://azure.microsoft.com/documentation/templates/)põhjal.

Teil on oma ressursirühm ja teil on teie malli, et nüüd olete valmis kasutama oma malli ressursirühma määratletud infrastruktuuri. Juurutamist cmdlet-käsu **New-AzureRmResourceGroupDeployment** ressursse. Malli saate määrata palju vaikeväärtused, mida me kasutame nii, et teil pole vaja esitada nende parameetrite väärtused. Tavaline süntaks näeb välja selline:

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -administratorLogin exampleadmin -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

Määrake ressursirühma ja malli asukoht. Kui teie mall on kohalik fail, saate kasutada parameetrit **– TemplateFile** ja määrake malli tee. Saate määrata selle **-režiimis** parameetri **Inkrementmuutus** või **lõpuleviimine**. Vaikimisi teeb ressursihaldur suureneva värskendus juurutamisel; Seega ei ole vaja **-režiimis** kui soovite **Inkrementmuutus**. Juurutamise režiimidest erinevused, vt [Deploy rakenduse Azure'i ressursihaldur malli abil](resource-group-template-deploy.md). 

###<a name="dynamic-template-parameters"></a>Dünaamiliste malli parameetrid

Kui olete tuttav PowerShelli, teate, et liikuda läbi cmdlet-käsu jaoks saadaolevate parameetrite tipite miinusmärk (-) ja seejärel vajutage tabeldusklahvi (TAB). See sama funktsioon töötab ka määratlete malli parameetrid. Kui tipite malli nimi, cmdlet toob Mall, sõelub selle ja lisab malli parameetrid käsu dünaamiliselt. See on väga lihtne Määrake malli parameetrite väärtused.

Kui sisestate käsu, palutakse teil puuduvad kohustuslik parameeter **administratorLoginPassword**. Ja parooli tippimisel varjatud turvaline stringiväärtus. Selle strateegia kõrvaldab riski paroolita lihttekstina.

    cmdlet New-AzureRmResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    administratorLoginPassword: ********

Kui mall sisaldab parameetri nimi, mis vastab mõnele malli (nt koos nimega **ResourceGroupName** teie malli, mis on sama, mis **ResourceGroupName** parameeter cmdlet-käsu [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) parameeter) käsku parameetrid koos, palutakse teil postfix **FromTemplate** (nt **ResourceGroupNameFromTemplate**) koos parameetri väärtuse ette. Üldiselt vältige seda segadust pole nime panemise parameetrid koos parameetrite juurutamise toimingute puhul kasutada sama nimi.

Käsk töötab ja tagastab sõnumite ressursside loomisel. Lõpuks, kuvatakse tulemus juurutamise.

    DeploymentName    : azuredeploy
    ResourceGroupName : TestRG1
    ProvisioningState : Succeeded
    Timestamp         : 4/11/2016 7:26:11 PM
    Mode              : Incremental
    TemplateLink      :
                Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json
                ContentVersion : 1.0.0.0
    Parameters        :
                Name             Type                       Value
                ===============  =========================  ==========
                skuName          String                     F1
                skuCapacity      Int                        1
                administratorLogin  String                  exampleadmin
                administratorLoginPassword  SecureString
                databaseName     String                     sampledb
                collation        String                     SQL_Latin1_General_CP1_CI_AS
                edition          String                     Basic
                maxSizeBytes     String                     1073741824
                requestedServiceObjectiveName  String       Basic

    Outputs           :
                Name             Type                       Value
                ===============  =========================  ==========
                siteUri          String                     websites5wdai7p2k2g4.azurewebsites.net
                sqlSvrFqdn       String                     sqlservers5wdai7p2k2g4.database.windows.net
                    
    DeploymentDebugLogLevel :

Paar sammu, loodud ja juurutatud keerukate veebisaidi jaoks nõutavad ressursid. 

### <a name="log-debug-information"></a>Logiteave silumine

Malli juurutamisel logida Lisateave koosolekukutsete ja kutsele vastamise, määrates parameetri **- DeploymentDebugLogLevel** **New-AzureRmResourceGroupDeployment**käivitamisel. See teave võib aidata teil juurutamise tõrkeotsing. Vaikeväärtus on, **pole** , mis tähendab, et ei või vastuse sisu on sisse logitud. Saate määrata logimise sisu taotlus, vastuse või mõlemad.  Tõrkeotsingu juurutuste ja silumine teave logimise kohta leiate lisateavet teemast [tõrkeotsing ressursi rühma juurutuste Azure PowerShelli abil](resource-manager-troubleshoot-deployments-powershell.md). Järgmises näites logib vastuse sisu juurutamise ja taotluse sisu.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -DeploymentDebugLogLevel All -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

> [AZURE.NOTE] Kui parameetri DeploymentDebugLogLevel hoolega teabetüüp on läbides käigus juurutamist. Logimise kohta leiate lisateavet taotluse või vastuse, võib potentsiaalselt tundliku loomuga andmeid juurutamise toimingute kaudu nähtavaks tegemine. 


## <a name="get-information-about-your-resource-groups"></a>Oma ressursside rühma kohta teabe saamine

Pärast ressursi rühma loomist saate cmdlettide ressursihaldur mooduli oma ressursi rühmade haldamiseks.

- Teie tellimus ressursirühma saamiseks kasutage **Get-AzureRmResourceGroup** cmdlet-käsk:

        Get-AzureRmResourceGroup -ResourceGroupName TestRG1
    
    Mis tagastab järgmine teave:

        ResourceGroupName : TestRG1
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG
        
        ...

    Kui te ei määra ressursi rühma nime, cmdlet Tagastab nende ressursside rühmade tellimuse.

- Ressursirühma ressursside saamiseks **Leidmine-AzureRmResource** ja kasutada cmdlet selle **ResourceGroupNameContains** parameeter. Ilma parameetrid, saab leidmine-AzureRmResource Azure tellimuse kõik ressursid.

        Find-AzureRmResource -ResourceGroupNameContains TestRG1
    
     Mis tagastab loendi ressursside vormindatavat, nt:
        
        Name              : sqlservers5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Sql/servers/sqlservers5wdai7p2k2g4
        ResourceName      : sqlservers5wdai7p2k2g4
        ResourceType      : Microsoft.Sql/servers
        Kind              : v2.0
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
        Tags              : {System.Collections.Hashtable}
        ...
            
- Siltide abil saate loogiliselt korraldamine ressursside teie tellimus ja tuua ressursside **AzureRmResource-otsimine** ja **Leidmine-AzureRmResourceGroup** cmdlet-käskude abil.

        Find-AzureRmResource -TagName displayName -TagValue Website

        Name              : webSites5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Web/sites/webSites5wdai7p2k2g4
        ResourceName      : webSites5wdai7p2k2g4
        ResourceType      : Microsoft.Web/sites
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
                
      There is much more you can do with tags. For more information, see [Using tags to organize your Azure resources](resource-group-using-tags.md).

## <a name="add-to-a-resource-group"></a>Ressursi lisamine rühma

Ressursirühma ressursi lisamiseks saate kasutada cmdlet-käsu **New-AzureRmResource** . Siiski lisada ressursi sellisel viisil võib põhjustada tulevaste segadust Kuna malli pole uue ressursi. Kui olete juurutanud uuesti vana malli, on juurutamist lõpetamata lahenduse. Kui juurutate sageli, leiate selle lihtsamaks ja usaldusväärne lisada uue ressursi malli ja uuesti juurutada.

## <a name="move-a-resource"></a>Ressursi teisaldamine

Saate teisaldada olemasolev ressursid uue ressursirühma. Näiteid leiate [Teisaldamine ressursid uue ressursirühma või tellimusele](resource-group-move-resources.md).

## <a name="export-template"></a>Ekspordi Mall

Olemasoleva ressursi rühma (juurutatud PowerShelli või teiste meetodite portaali umbes üks), saate vaadata ressursirühma ressursihaldur mall. Malli eksportimise pakub kahte eelised:

1. Kuna kõik infrastruktuuri on määratletud malli, saate tulevaste juurutuste lahenduse hõlpsalt automatiseerida.

2. Võite saada tuttav malli süntaks vaadates juures on JavaScript Object märke (JSON) mis tähistab teie lahendus.

Funktsiooni PowerShelli kaudu, saate luua malli, mis tähistab ressursirühma hetkeseisu või malli, mida kasutati kindla juurutamiseks tuua.

Eksportimise ressursirühma mall on kasulik, kui teie tehtud muudatused ressursirühma ja tuua selle praeguses olekus JSON kujutis on vaja. Siiski loodud Mall sisaldab ainult minimaalne arv parameetrite ja ühtegi muutujat. Malli väärtused on raske koodiga. Enne juurutamist loodud malli, soovite teisendada mitu väärtust parameetrid nii, et saate kohandada juurutamise viibite.

Eksportimine kindla juurutamiseks mall on kasulik, kui peate vaatama tegelik malli, mida kasutati juurutamiseks ressursse. Mall sisaldab kõiki parameetrite ja muutujate algse juurutamise jaoks määratletud. Juhul, kui keegi teie ettevõttes on muutnud ressursirühm väljaspool, mis on määratletud malli, ei tähistavad selle malli ressursirühma praeguses olekus.

> [AZURE.NOTE] Ekspordi malli funktsioon on eelvaade ja kõik ressursi tüüpi praegu toetavad eksportimise malli. Kui proovite eksportida malli, võidakse kuvada tõrketeade, mis kinnitab, et ei eksporditud järgmised materjalid. Vajadusel saate määratleda käsitsi nende ressursside malli pärast allalaadimise kaudu.

###<a name="export-template-from-resource-group"></a>Malli eksportimine ressursirühm

Malli ressursirühma kuvamiseks käivitage cmdlet-käsk **Ekspordi-AzureRmResourceGroup** .

    Export-AzureRmResourceGroup -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\TestRG1.json
    
###<a name="download-template-from-deployment"></a>Malli alla laadida juurutamine

Kindla juurutamiseks kasutatava malli allalaadimiseks käivitage cmdlet-käsk **Salvesta-AzureRmResourceGroupDeploymentTemplate** .

    Save-AzureRmResourceGroupDeploymentTemplate -DeploymentName azuredeploy -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\azuredeploy.json

## <a name="delete-resources-or-resource-group"></a>Ressursside või ressursirühm kustutamine

- Ressursi ressursirühma kustutamiseks kasutada cmdlet-käsu **Eemalda-AzureRmResource** . Selle cmdlet-käsu kustutab ressurss, kuid ei kustutata ressursirühma.

    See käsk eemaldab testsait veebisaidi TestRG1 ressursirühma.

        Remove-AzureRmResource -Name TestSite -ResourceGroupName TestRG1 -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01

- Ressursirühma kustutamiseks kasutada cmdlet-käsu **Eemalda-AzureRmResourceGroup** . Selle cmdlet-käsu kustutab ressursirühma ja selle ressursse.

        Remove-AzureRmResourceGroup -Name TestRG1
        
    Küsitakse kustutamise kinnitamiseks.
        
        Confirm
        Are you sure you want to remove resource group 'TestRG1'
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

## <a name="deployment-script"></a>Juurutamise skripti

Varasemate juurutamise näidetes ilmnes ainult üksikute cmdlet-käskude selles teemas vaja juurutada Azure ressursse. Järgmises näites on kujutatud juurutamise skripti, mis loob ressursirühma ja kasutab vahendid.

    <#
      .SYNOPSIS
      Deploys a template to Azure
      
      .DESCRIPTION
      Deploys an Azure Resource Manager template

      .PARAMETER subscriptionId
      The subscription id where the template will be deployed.

      .PARAMETER resourceGroupName
      The resource group where the template will be deployed. Can be the name of an existing or a new resource group.

      .PARAMETER resourceGroupLocation
      Optional, a resource group location. If specified, will try to create a new resource group in this location. If not specified, assumes resource group is existing.

      .PARAMETER deploymentName
      The deployment name.

      .PARAMETER templateFilePath
      Optional, path to the template file. Defaults to template.json.

      .PARAMETER parametersFilePath
      Optional, path to the parameters file. Defaults to parameters.json. If file is not found, will prompt for parameter values based on template.
    #>

    param(
      [Parameter(Mandatory=$True)]
      [string]
      $subscriptionId,

      [Parameter(Mandatory=$True)]
      [string]
      $resourceGroupName,

      [string]
      $resourceGroupLocation,

      [Parameter(Mandatory=$True)]
      [string]
      $deploymentName,

      [string]
      $templateFilePath = "template.json",

      [string]
      $parametersFilePath = "parameters.json"
    )

    #******************************************************************************
    # Script body
    # Execution begins here 
    #******************************************************************************
    $ErrorActionPreference = "Stop"

    # sign in
    Write-Host "Logging in...";
    Add-AzureRmAccount;

    # select subscription
    Write-Host "Selecting subscription '$subscriptionId'";
    Set-AzureRmContext -SubscriptionID $subscriptionId;

    #Create or check for existing resource group
    $resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
    if(!$resourceGroup)
    {
      Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
      if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
      }
      Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
      New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
    }
    else{
      Write-Host "Using existing resource group '$resourceGroupName'";
    }

    # Start the deployment
    Write-Host "Starting deployment...";
    if(Test-Path $parametersFilePath) {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
    } else {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
    }

## <a name="next-steps"></a>Järgmised sammud

- Ressursihaldur mallide loomise kohta leiate lisateavet teemast [Azure ressursihaldur mallide koostamine](./resource-group-authoring-templates.md).
- Mallide kasutamise kohta leiate teemast [Deploy rakenduse Azure'i ressursihaldur malli abil](./resource-group-template-deploy.md).
- Üksikasjalik näide juurutamine projekti, leiate teemast [Deploy microservices ootuspäraselt Azure'i sisse](app-service-web/app-service-deploy-complex-application-predictably.md).
- Juurutamise, mida ei saanud tõrkeotsingu kohta leiate teemast [tõrkeotsing ressursi rühma juurutuste Azure](./resource-manager-troubleshoot-deployments-powershell.md).

