<properties 
    pageTitle="Jälgimine ja hallata voo Analytics PowerShelli | Microsoft Azure'i" 
    description="Saate teada, kuidas Azure'i PowerShelli ja cmdlet-käskude abil saate jälgida ja hallata voo Analytics." 
    keywords="Azure'i PowerShelli, azure PowerShelli cmdlet-käskude, PowerShelli käsu, PowerShelli skriptimise" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Jälgimine ja hallata voo Analytics Azure PowerShelli cmdletid

Saate teada, kuidas jälgimine ja haldamine voo Analytics ressursse Azure PowerShelli cmdlet-käskude ja PowerShelli skripti, mis põhitoimingud voo analüüsi käivitada.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Azure'i PowerShelli cmdlet-käskude voo Analytics eeltingimused

 - Teie tellimus Azure'i ressursi rühma loomine. Järgmine on näide Azure PowerShelli skripti. Azure'i PowerShelli leiate teemast [installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md);  

Azure'i PowerShelli 0.9.8:  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>
 
        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure'i PowerShelli 1.0:  

        # Log in to your Azure account
        Login-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
        
        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
        


> [AZURE.NOTE] Loodud programmiliselt voo Analytics töökohtade pole jälgimise vaikimisi sisse lülitatud.  Saate käsitsi lubada projekti jälgimine lehele liikumine ja klõpsake nuppu Luba Azure'i portaalis jälgimise või selleks programmiliselt asub [Azure'i voo Analytics - kuvari voo Analytics töö Programatically](stream-analytics-monitor-jobs.md)juhiste järgi.

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Azure'i voo Analytics PowerShelli cmdlet-käsud
Järgmised Azure PowerShelli cmdlet-käsud saab jälgida ja hallata Azure voo Analytics. Pange tähele, et Azure PowerShelli erinevaid versioone. 
**Esimene käsk on ära toodud näidetes Azure PowerShelli 0.9.8, teine käsk on Azure PowerShelli 1.0.** Azure'i PowerShelli 1.0 käsud on alati "AzureRM" käsk.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob
Loetleb kõik voo Analytics tööd määratletud Azure tellimuse või määratud ressursirühm või saab töö teavet kindla töö jooksul ressursirühma.

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure'i PowerShelli 1.0:  

    Get-AzureRMStreamAnalyticsJob

Selle PowerShelli käsu tagastab Azure tellimuse teabe voo Analytics tööde haldamine.

**Näide 2**

Azure'i PowerShelli 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure'i PowerShelli 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Selle PowerShelli käsu tagastab ressursirühm StreamAnalytics vaike-Kesk-USA teavet voo Analytics tööde haldamine.

**Näide 3**

Azure'i PowerShelli 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure'i PowerShelli 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

See PowerShelli käsk tagastab ressursirühm StreamAnalytics vaike-Kesk-USA-voo Analytics töö StreamingJob teavet.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput
Loetleb kõik sisendeid, mis on määratletud määratud voo Analytics töö või saab teatud sisestatud teave.

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure'i PowerShelli 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

PowerShelli käsk tagastab väärtuse küljed töö StreamingJob määratletud teavet.

**Näide 2**

Azure'i PowerShelli 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure'i PowerShelli 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Selle PowerShelli käsu annab teavet sisendi nimega EntryStream määratletud töö StreamingJob.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput
Loetleb kõik väljundid, mis on määratletud määratud voo Analytics töö või saab teavet teatud väljundi.

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure'i PowerShelli 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Selle PowerShelli käsu annab teavet määratletud töö StreamingJob väljundid.

**Näide 2**

Azure'i PowerShelli 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure'i PowerShelli 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Selle PowerShelli käsu annab teavet nimega väljundi määratletud töö StreamingJob väljund.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota
Saab teavet kvoodi streaming määratud piirkonna üksused.

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure'i PowerShelli 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

Selle PowerShelli käsu tagastab Kesk-USA piirkonnas teavet kvoodi ja streaming üksuste kasutamist.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Saab teavet teatud teisendus, mis on määratletud voo Analytics töö.

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure'i PowerShelli 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Selle PowerShelli käsu tagastab teabe kohta tuntud StreamingJob töö StreamingJob muutmist.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Uue AzureStreamAnalyticsInput | Uue AzureRMStreamAnalyticsInput
Loob uue sisendi voo Analytics projekti raames või olemasoleva määratud sisend värskendused.
  
Nime, siis sisendit saab määrata .json faili või käsu. Kui mõlemad on määratud, käsu nimi peab olema sama, mis fail.

Kui määrate sisendit, mis on juba olemas ja määrake – Jõusta parameeter, cmdlet küsib, kas soovite asendada olemasoleva sisestatud.

Kui määrate – jõustada parameeter ja määrake olemasoleva sisestatud nimi, asendatakse sisend ilma kinnituse.

JSON faili struktuuri ja sisu kohta leiate üksikasjalikumat teavet leiate [Loomine sisendit (Azure'i voo Analytics)] [ msdn-rest-api-create-stream-analytics-input] jaotises [voo Analytics REST API viide teegis][stream.analytics.rest.api.reference].

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure'i PowerShelli 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Selle PowerShelli käsu loob uue sisendi Input.json failist. Kui määratud on juba olemasolevad sisend määratud Sisestuskeel määratluse faili nimega, paluge cmdlet, kas soovite asendada.

**Näide 2**

Azure'i PowerShelli 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure'i PowerShelli 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Selle PowerShelli käsu loob uue sisendi nimega EntryStream töö. Kui määratud on juba sama nimega olemasoleva sisend, paluge cmdlet, kas soovite asendada.

**Näide 3**

Azure'i PowerShelli 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure'i PowerShelli 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Selle PowerShelli käsu asendab olemasoleva Sisestuskeel allika nimega EntryStream määratluse faili määratlus.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Uue AzureStreamAnalyticsJob | Uue AzureRMStreamAnalyticsJob
Loob uue voo Analytics töö Microsoft Azure'i või värskendab mõne olemasoleva määratud töö määratlus.

Töö nimi saab määrata .json faili või käsu. Kui mõlemad on määratud, käsu nimi peab olema sama, mis fail.

Kui määrate töö nimi, mis on juba olemas ja määrake – Jõusta parameeter, cmdlet küsib, kas soovite olemasoleva töö asendada.

Kui määrate – parameetri jõustamine ja olemasoleva töö nimi, määrake töö määratlus asendatakse ilma kinnituse.

JSON faili struktuuri ja sisu kohta leiate üksikasjalikumat teavet leiate [Loomine voo Analytics töö] [ msdn-rest-api-create-stream-analytics-job] jaotises [voo Analytics REST API viide teegis][stream.analytics.rest.api.reference].

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure'i PowerShelli 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Selle PowerShelli käsu loob uue töö JobDefinition.json määratlus. Kui on olemasolev töökohtade määratud töö määratluse faili nimi on juba määratud, cmdlet küsida, kas soovite asendada.

**Näide 2**

Azure'i PowerShelli 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure'i PowerShelli 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Selle PowerShelli käsu asendab töö definitsiooni StreamingJob.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Uue AzureStreamAnalyticsOutput | Uue AzureRMStreamAnalyticsOutput
Loob uue väljundi voo Analytics projekti raames või värskendab olemasoleva väljund.  

Väljund nime saab määrata failis .json või käsu. Kui mõlemad on määratud, käsu nimi peab olema sama, mis fail.

Kui määrate väljundi, mis on juba olemas ja määrake – Jõusta parameeter, cmdlet küsib, kas soovite asendada olemasoleva väljundi.

Kui määrate – jõustada parameeter ja määrake olemasoleva väljund nimi, asendatakse väljund ilma kinnituse.

JSON faili struktuuri ja sisu kohta leiate üksikasjalikumat teavet leiate [Väljundi loomine (Azure'i voo Analytics)] [ msdn-rest-api-create-stream-analytics-output] jaotises [voo Analytics REST API viide teegis][stream.analytics.rest.api.reference].

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure'i PowerShelli 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Selle PowerShelli käsu loob uue väljundi, nimega "väljundi" töö StreamingJob. Kui määratud on juba sama nimega olemasoleva väljund, paluge cmdlet, kas soovite asendada.

**Näide 2**

Azure'i PowerShelli 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure'i PowerShelli 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Selle PowerShelli käsu asendab "väljundi" definitsiooni töö StreamingJob.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Uue AzureStreamAnalyticsTransformation | Uue AzureRMStreamAnalyticsTransformation
Luuakse uus teisendus voo Analytics projekti raames või värskendab olemasoleva teisendus.
  
Teisenduse nime saab määrata failis .json või käsu. Kui mõlemad on määratud, käsu nimi peab olema sama, mis fail.

Kui määrate teisendus, mis on juba olemas ja määrake – Jõusta parameeter, cmdlet küsib, kas soovite asendada olemasoleva teisendus.

Kui määrate – jõustada parameeter ja määrake olemasoleva teisendus nime, muutmist, asendatakse ilma kinnituse.

JSON-faili struktuuri ja sisu kohta leiate üksikasjalikumat teavet leiate [Loomine transformatsioon (Azure'i voo Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] jaotises [voo Analytics REST API viide teegis][stream.analytics.rest.api.reference].

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure'i PowerShelli 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Selle PowerShelli käsu luuakse uus teisendus, nimetatakse StreamingJobTransform töö StreamingJob. Kui mõne olemasoleva teisendus on juba sama nimega määratletud, cmdlet küsida, kas soovite asendada.

**Näide 2**

Azure'i PowerShelli 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure'i PowerShelli 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 Selle PowerShelli käsu asendab töö StreamingJob StreamingJobTransform määratlus.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Eemalda – AzureStreamAnalyticsInput | Eemalda – AzureRMStreamAnalyticsInput
Kustutab teatud sisendi asünkroonselt voo Analytics töö Microsoft Azure.  
Kui määrate – Jõusta parameeter, siis sisendit kustutatakse ilma kinnituse.

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure'i PowerShelli 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Selle PowerShelli käsu eemaldab Sisestuskeel EventStream töö StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Eemalda – AzureStreamAnalyticsJob | Eemalda – AzureRMStreamAnalyticsJob
Kustutab asünkroonselt konkreetse voo Analytics töö Microsoft Azure.  
Kui määrate – Jõusta parameeter, töö kustutatakse ilma kinnituse.

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure'i PowerShelli 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Selle PowerShelli käsu eemaldab töö StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Eemalda – AzureStreamAnalyticsOutput | Eemalda – AzureRMStreamAnalyticsOutput
Kustutab teatud väljundi asünkroonselt voo Analytics töö Microsoft Azure.  
Kui määrate – Jõusta parameeter, kustutatakse väljundisse ilma kinnituse.

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure'i PowerShelli 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

See PowerShelli käsu eemaldatakse väljund väljund töö StreamingJob.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Algus-AzureStreamAnalyticsJob | Algus-AzureRMStreamAnalyticsJob
Asünkroonselt kasutajaks ja alustab Microsoft Azure voo Analytics tööd.

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure'i PowerShelli 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Selle PowerShelli käsu käivitab töö StreamingJob koos kohandatud väljundi algusaeg väärtuseks 12 detsember 2012 12:12:12 UTC.


### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Lõpeta – AzureStreamAnalyticsJob | Lõpeta – AzureRMStreamAnalyticsJob
Asünkroonselt lõpetab voo Analytics töö töötab Microsoft Azure ja tühistage eraldab, mis olid, mida kasutati. Metaandmete ja töö määratlus jäävad tellimuse Azure portaali ja halduse API-d, kaudu kättesaadavaks nii, et töö saab redigeerida ja uuesti. Te ei lisandu töö peatatud olekus.

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure'i PowerShelli 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Selle PowerShelli käsu lõpetab töö StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Test-AzureStreamAnalyticsInput | Test-AzureRMStreamAnalyticsInput
Testib voo Analytics võimalus ühenduse määratud sisestatud.

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure'i PowerShelli 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Selle PowerShelli käsu katsete ühenduse oleku Sisestuskeel EntryStream StreamingJob.  

###<a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Test-AzureStreamAnalyticsOutput | Test-AzureRMStreamAnalyticsOutput
Testib voo Analytics võimalus ühenduse määratud väljundi.

**Näide 1**

Azure'i PowerShelli 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure'i PowerShelli 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

See PowerShelli käsu kontrollib ühenduse oleku väljundisse väljund StreamingJob.  

## <a name="get-support"></a>Tootetugi
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics). 


## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 



[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
