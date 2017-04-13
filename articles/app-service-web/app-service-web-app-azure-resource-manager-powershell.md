<properties
    pageTitle="Azure'i ressursihaldur vastavalt PowerShelli käske Azure Web Appi | Microsoft Azure'i"
    description="Saate teada, kuidas hallata oma Azure'i veebirakenduste uue Azure ressursihaldur vastavalt PowerShelli käskude abil."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="aelnably"/>

# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Azure'i ressursi halduri vastavalt PowerShelli kasutamine Azure veebirakenduste haldamine#

> [AZURE.SELECTOR]
- [Azure'i CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure'i PowerShelli](app-service-web-app-azure-resource-manager-powershell.md)

Microsoft Azure'i PowerShelli versioon 1.0.0 uue käsud on lisatud, anda kasutajale võimalus veebirakenduste haldamine Azure'i ressursihaldur vastavalt PowerShelli käskude abil.

Ressursi rühmade haldamise kohta leiate teemast [Azure ressursihaldur Azure PowerShelli kasutamine](../powershell-azure-resource-manager.md). 

Suvandite PowerShelli cmdlet-käskude ja parameetrite täieliku loendi leiate teemast [Täielik cmdleti viide Web Appi Azure'i ressursihaldur vastavalt PowerShelli cmdlet-käsud.](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Rakenduse teenuse haldamise lepingud ##

### <a name="create-an-app-service-plan"></a>Mõni rakendus teenusleping loomine ###
Mõni rakendus teenusleping loomiseks kasutage cmdlet-käsu **New-AzureRmAppServicePlan** .

Eri parameetrite kirjeldused on järgmised:

-   **Nimi**: teenusleping rakenduse nimi.
-   **Asukoht**: teenuse lepingu asukoht.
-   **ResourceGroupName**: ressursirühm, mis sisaldab äsja loodud rakenduse teenusleping.
-   **Esimese taseme**: soovitud hinnakirjad esimese taseme (vaikeväärtus on tasuta, muud suvandid on ühiskasutuses, Basic, Standard ja Premium)
-   **WorkerSize**: suurus töötajate (vaikesäte on väike, kui määratud taseme parameeter Basic, Standard või Premium. Muud suvandid on keskmine ja suur.)
-   **NumberofWorkers**: töötajate rakenduse teenuse lepingu (vaikeväärtus on 1). 

Näide kasutada seda cmdlet-käsk:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Rakenduse teenuse keskkonnas on rakenduse teenusleping loomine ###
Keskkonnas rakenduse teenus on rakenduse teenusleping loomiseks kasutada sama käsu **New-AzureRmAppServicePlan** käsk eest parameetriga määrata selle ASE- ja ressursirühma ASE isiku nimi.

Näide kasutada seda cmdlet-käsk:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Lisateabe saamiseks rakenduse teenuse keskkonnas, märkige ruut [rakenduse teenuse keskkonna tutvustus](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Loendi olemasoleva rakenduse teenuse lepingud ###

Olemasoleva rakenduse plaanide loendis kasutada cmdlet-käsk **Get-AzureRmAppServicePlan** .

Loetle kõik appi lepingud on jaotises tellimuse, saate kasutada. 

    Get-AzureRmAppServicePlan

Loetle kõik appi lepingud on jaotises kindla ressursirühma, saate kasutada.

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Mõne kindla rakenduse teenusleping saamiseks kasutage:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Mõne olemasoleva rakenduse teenuse leping konfigureerimine ###

Olemasoleva rakenduse teenusleping sätete muutmiseks kasutage cmdlet **Set-AzureRmAppServicePlan** . Saate muuta selle taseme, töötaja suurus ja töötajate arvu 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Mõni rakendus teenusleping mastaapimine ####

Mastaapimiseks mõne olemasoleva rakenduse teenuse leping, saate kasutada.

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>Rakenduse teenuse leping töötaja suuruse muutmine ####

Töötajate mõne olemasoleva rakenduse teenuse leping suuruse muutmiseks kasutage.

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>Rakenduse teenuse plaani taseme muutmine ####

Mõne olemasoleva rakenduse teenuse kava taseme muutmiseks kasutage:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Kustutada mõne olemasoleva rakenduse teenuse kavandamine ###

Olemasoleva rakenduse teenusleping kustutamiseks tuleb kõik määratud veebirakenduste teisaldatud või kustutatud esmalt. Seejärel saate kustutada rakenduse teenusleping **Eemalda-AzureRmAppServicePlan** cmdlet-käsu abil.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Rakenduse teenuse veebirakenduste haldamine ##

### <a name="create-a-web-app"></a>Veebirakenduse loomine ###

Veebirakenduse loomiseks kasutage cmdlet-käsu **New-AzureRmWebApp** .

Eri parameetrite kirjeldused on järgmised:

- **Nimi**: web appi nime.
- **AppServicePlan**: kasutatud majutada veebirakenduse teenusleping nimi.
- **ResourceGroupName**: ressursirühm, mis hostib rakenduse teenusleping.
- **Asukoht**: web appi kohta.

Näide kasutada seda cmdlet-käsk:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Luua veebirakenduse rakenduse teenuse keskkonnas ###

Luua web appi sisse ka rakenduse teenuse keskkonnas (ASE). Käsu sama **New-AzureRmWebApp** eest parameetritega ASE nimi ja ressursside rühma nime, mis kuulub ASE määramiseks.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Lisateabe saamiseks rakenduse teenuse keskkonnas, märkige ruut [rakenduse teenuse keskkonna tutvustus](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Kustutage olemasolev Web App ###

Kustutage olemasolev web app saate kasutada cmdlet-käsu **Eemalda-AzureRmWebApp** , peate määrama veebirakenduse nimi ja ressursside rühma nime.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Loendi olemasoleva Web Apps ###

Olemasoleva veebirakenduste loendis kasutage **Get-AzureRmWebApp** cmdlet-käsk.

Loetle kõik veebirakenduste jaotises tellimuse, saate kasutada.

    Get-AzureRmWebApp

Loetle kõik veebirakenduste jaotises kindla ressursirühma, saate kasutada.

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Kindla veebirakenduse saamiseks kasutage:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Mõne olemasoleva Web Appi konfigureerimine ###

Saate muuta sätteid ja konfiguratsioone olemasoleva veebirakenduse cmdlet **Set-AzureRmWebApp** . Parameetrite täieliku loendi kontrolli [cmdleti viide link](https://msdn.microsoft.com/library/mt652487.aspx)

Näide (1): Kasutage seda cmdlet muutmiseks ühendusstringi

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Näide (2): lisamine või rakenduse sätete muutmine

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


(3) näide: Määrake veebirakenduse 64-bitises režiimis

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>Muuta olemasolevaid Web App ###

#### <a name="restart-a-web-app"></a>Taaskäivitage web app ####

Taaskäivitage web appi, peate määrama veebirakenduse rühma nimi ja ressursside.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Web appi peatamine ####

Web appi peatamiseks Määrake veebirakenduse rühma nimi ja ressursside.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Käivitage web app ####

Web appi käivitamiseks peate määrama rühma nimi ja ressursside web app.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Web Appi avaldamise profiilide haldamine ###

Iga web appil avaldamise profiili, mida saab avaldada oma rakendused, mitme toimingu saab teostada avaldamislehtedel profiilid.

#### <a name="get-publishing-profile"></a>Saada avaldamise profiil ####

Web appi avaldamise profiili saamiseks kasutage:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

See käsk kordab avaldamise profiil kui ka väljund käsurea tekstifaili avaldamise profiil.

#### <a name="reset-publishing-profile"></a>Lähtestamine profiilile avaldamise ####

Mõlemad lähtestamiseks avaldamise parooli FTP ja web juurutada web app kasutamine:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Web Appi sertide haldamine ###

Web appi serdid haldamise kohta leiate artiklist [PowerShelli kaudu SSL-sertide sidumine](app-service-web-app-powershell-ssl-binding.md)


### <a name="next-steps"></a>Järgmised sammud ###
- Azure'i ressursihaldur PowerShelli toe kohta leiate teemast [Azure PowerShelli kasutamine Azure ressursihaldur.](../powershell-azure-resource-manager.md)
- Rakenduse teenuse keskkonnas kohta lisateabe saamiseks lugege teemat [Sissejuhatus rakenduse teenuse keskkonnas.](app-service-app-service-environment-intro.md)
- Rakenduse teenuse SSL-sertide PowerShelli kaudu haldamise kohta leiate teemast [PowerShelli kaudu SSL-sertide sidumine.](app-service-web-app-powershell-ssl-binding.md)
- Azure'i veebirakendustes täieliku loendi Azure'i ressursihaldur vastavalt PowerShelli cmdlet-käskude kohta lisateabe saamiseks lugege teemat [Azure cmdleti viide Web Appsi Azure'i ressursihaldur PowerShelli cmdlet-käskude.](https://msdn.microsoft.com/library/mt619237.aspx)
- - Rakenduse teenuse abil CLI haldamise kohta leiate teemast [Using Azure Resource Manager-Based XPlat CLI Azure Web Appi.](app-service-web-app-azure-resource-manager-xplat-cli.md)
