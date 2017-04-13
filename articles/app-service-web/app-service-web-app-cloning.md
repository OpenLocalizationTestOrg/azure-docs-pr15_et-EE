<properties
    pageTitle="Web Appi kloonimine PowerShelli abil"
    description="Saate teada, kuidas klooni oma veebirakenduste uute veebirakenduste PowerShelli kaudu."
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
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure'i rakendust Service rakenduse kloonimine PowerShelli abil#

Microsoft Azure'i PowerShelli versiooni 1.1.0 väljaandes uus võimalus on lisatud uus AzureRMWebApp, mis annaks kasutaja võimalus klooni olemasoleva Web App vastloodud rakendusse teises regioonis või piirkonna. See võimaldab klientide juurutamiseks rakenduste arvust eri piirkondade kiire ja lihtne.

Rakenduse kloonimine pole praegu toetatud ainult premium taseme rakenduse teenuse lepingud. Uus funktsioon kasutab sama piirangud Web Appsi varundamise funktsioon, leiate [Azure'i rakendust Service veebirakenduse varundada](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Lugege teemat Azure ressursihaldur vastavalt Azure PowerShelli cmdlet-käskude oma veebirakenduste haldamine märkige [Azure'i ressursihaldur vastavalt PowerShelli käske Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>Olemasoleva rakenduse kloonimine ##

Stsenaarium: Olemasolevate veebirakendus piirkonna Lõuna keskse meile, kasutaja soovib klooni Põhja keskse meil regioonis uue veebirakenduse sisu. Seda saab teha abil Azure'i ressursihaldur versiooni PowerShelli cmdlet - SourceWebApp suvand uue veebirakenduse loomine.

Teab sisaldava andmeallika veebirakenduse ressursside rühma nime, saame kasutada PowerShelli käsk allika veebirakenduse teavet (sel juhul nimega allika-Web Appis):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Luua uue rakenduse teenuse kavandamine, saame kasutada New-AzureRmAppServicePlan käsuga, nagu järgmises näites

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Käsu New-AzureRmWebApp, saame Põhja keskse meil regioonis uue veebirakenduse loomine ja siduda see mõne olemasoleva premium taseme rakenduse teenuse kavandamine, Lisaks saame kasutada sama ressursirühm allika web app või määratlemine uue ressursirühma, järgmist näitab, et:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

Olemasoleva veebirakenduse, sh kõik seotud juurutamise klooni teenindusaegade, kasutaja peate kasutama IncludeSourceWebAppSlots parameeter, PowerShelli käsk näitab selle käsu New-AzureRmWebApp parameetri kasutamine:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots $true

Klooni olemasoleva web app samasse piirkonda, kasutaja peate looma uue ressursirühma ja uue rakenduse teenuse piirkonna kavandamine ja klooni veebirakenduse PowerShelli järgmise käsu abil

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Kloonimine olemasoleva rakenduse App teenuse keskkonnas ##

Stsenaarium: Olemasolevate veebirakendus piirkonna Lõuna keskse meile, kasutaja soovib klooni sisu web Appi abil mõne olemasoleva rakenduse teenuse keskkonnas (ASE).

Teab sisaldava andmeallika veebirakenduse ressursside rühma nime, saame kasutada PowerShelli käsk allika veebirakenduse teavet (sel juhul nimega allika-Web Appis):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Funktsiooni ASE nimi ja ressursside rühma nime, mis kuulub ASE teab, kasutaja saab kasutada käsu New-AzureRmWebApp uue veebirakenduse loomine olemasoleva ASE, järgmist näitab, et:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

Asukoha parameeter on nõutav pärand põhjusel, kuid puhul loomise rakendus on ASE seda ignoreeritakse. 

## <a name="cloning-an-existing-app-slot"></a>Mõne olemasoleva rakenduse pesa kloonimine ##

Stsenaarium: Kasutaja soovib klooni mõne olemasoleva Web Appi pesa kas uue veebirakenduse või uue veebirakenduse pesa. Uue veebirakenduse võib olla sama piirkonna algse veebirakenduse pesa või teises regioonis.

Teab sisaldava andmeallika veebirakenduse ressursside rühma nime, saame kasutada PowerShelli käsk allika web appi pesa teavet (sel juhul nimega allika-webappslot) seotud veebirakenduse allika-Web Appis:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Järgmine näitab klooni allika web Appi uue veebirakenduse loomine:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Liikluse haldur ajal kloonimine vastuseks appi konfigureerimine ##

Mitme piirkond veebirakenduste loomine ja konfigureerimine Azure'i liikluse haldur liikluse marsruutimiseks nende veebirakenduste, on n olulised stsenaarium kindlustada, et klientide rakendused on väga saadaval, kui kloonimine olemasoleva web app teil võimalus nii veebirakendustes ühenduse liikluse halduri uut profiili või olemasoleva – Pange tähele, et ainult Azure ressursihaldur liikluse haldur versiooni ei toetata.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Ajal kloonimine vastuseks Appi uue liikluse haldur profiili loomine ###

Stsenaarium: Kasutaja soovib klooni konfigureerimisel Azure'i ressursihaldur liikluse haldur profiili, mis sisaldavad nii veebirakendustes web app teise alale. Järgmine näitab konfigureerimisel liikluse haldur uue profiili klooni allika web Appi uue veebirakenduse loomine:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Uue lisamine olemasolevale profiilile liikluse haldur kloonitud Web App ###

Stsenaarium: Kasutaja on juba Azure'i ressursihaldur liikluse haldur profiili, mida soovite ta lisada nii veebirakendustes lõpp-punktid. Selleks tuleb esmalt otsustama olemasoleva liikluse haldur profiili id, vajame tellimuse id, ressursside rühma nime ja olemasoleva liikluse haldur profiili nimi.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Pärast liikluse sõim id, järgmist näitab klooni allika web Appi uue veebirakenduse loomise ajal liikluse haldur olemasolevale profiilile lisamine:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Praeguse piirangud ##

See funktsioon on praegu eelvaade, tegeleme lisada uusi võimalusi aja jooksul, järgmises loendis on praegune versioon rakenduse kloonimine teadaolevad piirangud:

- Automaatne skaala sätteid on pole kloonitud
- Varunduse ajakava sätted on pole kloonitud
- VNET sätted on pole kloonitud
- Rakenduse ülevaateid ei ole automaatselt seadistatud sihtkoha web app
- Lihtne Auth sätted on pole kloonitud
- Kudu laiend on pole kloonitud
- Näpunäide reeglid on pole kloonitud
- Andmebaasi sisu on pole kloonitud


### <a name="references"></a>Viited ###
- [Azure'i ressursihaldur põhjal PowerShelli käske Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Web Appi kloonimine Azure'i portaalis](app-service-web-app-cloning-portal.md)
- [Azure'i rakendust Service veebirakenduse varundamine](web-sites-backup.md)
- [Azure'i ressursihaldur tugi Azure liikluse haldur eelvaade](../../articles/traffic-manager/traffic-manager-powershell-arm.md)
- [Sissejuhatus rakenduse teenuse keskkonnas](app-service-app-service-environment-intro.md)
- [Azure'i ressursihaldur Azure PowerShelli abil](../powershell-azure-resource-manager.md)
