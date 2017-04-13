<properties 
    pageTitle="Hallata Azure Media Services kontod PowerShelli abil" 
    description="Saate teada, kuidas hallata Azure Media Servicesi kontod PowerShelli cmdlet-käskude abil." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"
    ms.author="juliako"/>


#<a name="manage-azure-media-services-accounts-with-powershell"></a>Azure Media Services kontod PowerShelliga haldamine

> [AZURE.SELECTOR]
- [Portaal](media-services-portal-create-account.md)
- [PowerShelli](media-services-manage-with-powershell.md)
- [ÜLEJÄÄNUD](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Looge konto Azure Media Servicesi saama, peab olema Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure tasuta prooviversioon</a>.

##<a name="overview"></a>Ülevaade 

Selles artiklis on loetletud Azure PowerShelli cmdletid Azure Media Services (AMS) Azure'i ressursihaldur raames. Cmdlet-käsud on olemas **Microsoft.Azure.Commands.Media** nimeruumi.

## <a name="versions"></a>Versioonid

**ApiVersion**: "2015-10-01"
               

## <a name="new-azurermmediaservice"></a>Uue AzureRmMediaService

Loob meediumi teenus.

### <a name="syntax"></a>Süntaks

Parameetri: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

Parameetri: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Parameetrid

**-ResourceGroupName &lt;String&gt;**

Määrab nime, kuhu see meediumi teenus kuulub ressursirühma.

Pseudonüümid | ükski
---|---
See on nõutav?   |  True
Paigutage?   |  0
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Aktsepteerige metamärke?  |FALSE

**-Account_name &lt;stringi&gt;**

Saate määrata meediumi teenuse nimi.

Pseudonüümid |Nimi
---|---
See on nõutav? |True
Paigutage? |1
Vaikeväärtus |ükski
Müügivõimaluste sisendit vastu? |FALSE
Aktsepteeri metamärke? |FALSE

**– Asukoht &lt;stringi&gt;**

Saate määrata media service ressursi asukoha.

Pseudonüümid |ükski
---|---
See on nõutav? |True
Paigutage? |2
Vaikeväärtus  |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Aktsepteerige metamärke? |FALSE

**-StorageAccountId &lt;String&gt;**

Määrab esmased konto, mille meediumi teenusega seotud.

- Salvestusruumi (loodud konto ressursihaldur API-ga) toetatud ainult.

- Salvestusruumi konto peab olema ja on meediumi teenuse samasse asukohta.

Pseudonüümid |ükski
---|---
See on nõutav? |True
Paigutage? |3
Vaikeväärtus  |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Parameetri nimi |StorageAccountIdParamSet
Aktsepteerige metamärke?|FALSE

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Saate määrata salvestusruumi kontod, mis on meediumi teenusega seotud.

- Salvestusruumi (loodud konto ressursihaldur API-ga) toetatud ainult.

- Salvestusruumi konto peab olema ja on meediumi teenuse samasse asukohta.

- Esmane saab määrata ainult üks salvestusruumi konto.

Pseudonüümid |ükski
---|---
See on nõutav?  |True
Paigutage?  |3
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Parameetri nimi |StorageAccountsParamSet
Aktsepteerige metamärke? |FALSE

**-Sildid &lt;Hashtable talletatakse&gt;**

Saate määrata räsi tabeli sildi, mis on seotud meediumi teenus.

- Näide:@{"tag1"="value1";"tag2"=:value2"}

Pseudonüümid |ükski
---|---
See on nõutav?  |FALSE
Paigutage?  |nimega
Vaikeväärtus |ükski
Müügivõimaluste sisendit vastu? |FALSE
Aktsepteerige metamärke? |FALSE

**&lt;CommandParameters&gt;**

Selle cmdlet-käsu toetab levinud parameetrid:-silumine - ErrorAction, - ErrorVariable, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Paljusõnaline, - WarningAction, ja - WarningVariable.

### <a name="inputs"></a>Sisendina

Sisendi tüüp on objektid, mida te saate toru cmdlet tüüpi.

### <a name="outputs"></a>Väljundid

Väljundi tüüp on objektid, mis eraldab cmdlet-käsu tüüp.

## <a name="set-azurermmediaservice"></a>Set-AzureRmMediaService

Värskendab media teenus.

### <a name="syntax"></a>Süntaks

    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Parameetrid

**-ResourceGroupName &lt;String&gt;**

Määrab nime, kuhu see meediumi teenus kuulub ressursirühma.

Pseudonüümid |ükski
---|---
See on nõutav?  |True
Paigutage?  |0
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Aktsepteerige metamärke? |FALSE

**-Account_name &lt;stringi&gt;**

Saate määrata meediumi teenuse nimi.

Pseudonüümid |Nimi
---|---
See on nõutav? |True
Paigutage? |1
Vaikeväärtus |Ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Aktsepteerige metamärke? |FALSE

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Saate määrata salvestusruumi kontod, mis on meediumi teenusega seotud.

- Salvestusruumi (loodud konto ressursihaldur API-ga) toetatud ainult.

- Salvestusruumi konto peab olema ja on meediumi teenuse samasse asukohta.

- Esmane saab määrata ainult üks salvestusruumi konto.

Pseudonüümid |ükski
---|---
See on nõutav? |FALSE
Paigutage? |Nimega
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Parameetri nimi |StorageAccountsParamSet
Aktsepteerige metamärke? |FALSE

**-Sildid &lt;Hashtable talletatakse&gt;**

Saate määrata räsi tabeli sildi, mis on seotud meediumi see teenus.

- Sildid, mis on seotud meediumi teenus on asendatud kliendi poolt määratud väärtuse.

Pseudonüümid |ükski
---|---
See on nõutav? |FALSE
Paigutage?  |Nimega
Vaikeväärtus |Ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Aktsepteerige metamärke? |FALSE

**&lt;CommandParameters&gt;**

Selle cmdlet-käsu toetab levinud parameetrid:-silumine - ErrorAction, - ErrorVariable, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Paljusõnaline, - WarningAction, ja - WarningVariable.

### <a name="inputs"></a>Sisendina

Sisendi tüüp on objektid, mida te saate toru cmdlet tüüpi.

### <a name="outputs"></a>Väljundid

Väljundi tüüp on objektid, mis eraldab cmdlet-käsu tüüp.

## <a name="remove-azurermmediaservice"></a>Eemalda – AzureRmMediaService

Eemaldab meediumi teenus.

### <a name="syntax"></a>Süntaks

    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameetrid

**-ResourceGroupName &lt;String&gt;**

Määrab nime, kuhu see meediumi teenus kuulub ressursirühma.

Pseudonüümid |ükski
---|---
See on nõutav? |True
Paigutage? |0
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Aktsepteerige metamärke? |FALSE

**-Account_name &lt;stringi&gt;**

Saate määrata meediumi teenuse nimi.

Pseudonüümid |ükski
---|---
See on nõutav? |True
Paigutage? |2
Vaikeväärtus |Ükski
Aktsepteerige kohaletoimetamisel sisestatud?  |True(ByPropertyName)
Aktsepteerige metamärke? |FALSE

**&lt;CommandParameters&gt;**

Selle cmdlet-käsu toetab levinud parameetrid:-silumine - ErrorAction, - ErrorVariable, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Paljusõnaline, - WarningAction, ja - WarningVariable.

### <a name="inputs"></a>Sisendina

Sisendi tüüp on objektid, mida te saate toru cmdlet tüüpi.

### <a name="outputs"></a>Väljundid

Väljundi tüüp on objektid, mis eraldab cmdlet-käsu tüüp.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService

Saab kõik media services ressursirühma või meediumi teenuse antud nimega.

### <a name="syntax"></a>Süntaks

ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>] 

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameetrid

**-ResourceGroupName &lt;String&gt;**

Määrab nime, kuhu see meediumi teenus kuulub ressursirühma.

Pseudonüümid |ükski
---|---
See on nõutav? |True
Paigutage?  |0
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Parameetri nimi |ResourceGroupParameterSet AccountNameParameterSet
Aktsepteerige metamärke?   FALSE

**-Account_name &lt;stringi&gt;**

Saate määrata meediumi teenuse nimi.

Pseudonüümid |ükski
---|---
See on nõutav? |True
Paigutage?  |1
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Parameetri nimi  |AccountNameParameterSet
Aktsepteerige metamärke? |FALSE

**&lt;CommandParameters&gt;**

Selle cmdlet-käsu toetab levinud parameetrid:-silumine - ErrorAction, - ErrorVariable, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Paljusõnaline, - WarningAction, ja - WarningVariable.

### <a name="inputs"></a>Sisendina

Sisendi tüüp on objektid, mida te saate toru cmdlet tüüpi.

### <a name="outputs"></a>Väljundid

Väljundi tüüp on objektid, mis eraldab cmdlet-käsu tüüp.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys

Saab klahvid meediumi teenuse.

### <a name="syntax"></a>Süntaks

    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameetrid

**-ResourceGroupName &lt;String&gt;**

Määrab nime, kuhu see meediumi teenus kuulub ressursirühma.

Pseudonüümid |ükski
---|---
See on nõutav? |True
Paigutage?  |0
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Aktsepteerige metamärke? |FALSE

**-Account_name &lt;stringi&gt;**

Saate määrata meediumi teenuse nimi.

Pseudonüümid |ükski
---|---
See on nõutav? |True
Paigutage? |1
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Aktsepteerige metamärke? |FALSE

**&lt;CommandParameters&gt;**

Selle cmdlet-käsu toetab levinud parameetrid:-silumine - ErrorAction, - ErrorVariable, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Paljusõnaline, - WarningAction, ja - WarningVariable.

### <a name="inputs"></a>Sisendina

Sisendi tüüp on objektid, mida te saate toru cmdlet tüüpi.

### <a name="outputs"></a>Väljundid

Väljundi tüüp on objektid, mis eraldab cmdlet-käsu tüüp.

## <a name="set-azurermmediaservicekey"></a>Set-AzureRmMediaServiceKey

Taastab meediumi teenuse põhi- või klahvi.

### <a name="syntax"></a>Süntaks

    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Parameetrid

**-ResourceGroupName &lt;String&gt;**

Määrab nime, kuhu see meediumi teenus kuulub ressursirühma.

Pseudonüümid |ükski
---|---
See on nõutav?  |True
Paigutage?  |0
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud?  |True(ByPropertyName)
Aktsepteerige metamärke? |FALSE

**-Account_name &lt;stringi&gt;**

Saate määrata meediumi teenuse nimi.

Pseudonüümid |ükski
---|---
See on nõutav? |True
Paigutage?  |1
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud?   |True(ByPropertyName)
Aktsepteerige metamärke? |FALSE

**-KeyType &lt;KeyType&gt;**

Saate määrata media teenuse tüüp.

- Põhi- või

Pseudonüümid |ükski
---|---
See on nõutav?  |True
Paigutage?  |2
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |FALSE
Aktsepteerige metamärke? |FALSE

**&lt;CommandParameters&gt;**

Selle cmdlet-käsu toetab levinud parameetrid:-silumine - ErrorAction, - ErrorVariable, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Paljusõnaline, - WarningAction, ja - WarningVariable.

### <a name="inputs"></a>Sisendina

Sisendi tüüp on objektid, mida te saate toru cmdlet tüüpi.

### <a name="outputs"></a>Väljundid

Väljundi tüüp on objektid, mis eraldab cmdlet-käsu tüüp.

## <a name="sync-azurermmediaservicestoragekeys"></a>Sünkroonimine-AzureRmMediaServiceStorageKeys

Sünkroonib salvestusruumi konto klahvid meediumi teenusega seotud konto salvestusruumi.

### <a name="syntax"></a>Süntaks

    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameetrid

**-ResourceGroupName &lt;String&gt;**

Määrab nime, kuhu see meediumi teenus kuulub ressursirühma.

Pseudonüümid |ükski
---|---
See on nõutav? |True
Paigutage? |0
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Aktsepteerige metamärke? |FALSE

**-Account_name &lt;stringi&gt;**

Saate määrata meediumi teenuse nimi.

Pseudonüümid |ükski
---|---
See on nõutav? |True
Paigutage? |1
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |True(ByPropertyName)
Aktsepteerige metamärke? |FALSE

**-StorageAccountId &lt;String&gt;**

Saate määrata media teenusega seotud konto salvestusruumi.

Pseudonüümid |ID
---|---
See on nõutav? |True
Paigutage?  |2
Vaikeväärtus |ükski
Aktsepteerige kohaletoimetamisel sisestatud? |      True(ByPropertyName)
Aktsepteerige metamärke? |FALSE

**&lt;CommandParameters&gt;**

Selle cmdlet-käsu toetab levinud parameetrid:-silumine - ErrorAction, - ErrorVariable, - InformationAction - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Paljusõnaline, - WarningAction, ja - WarningVariable.

### <a name="inputs"></a>Sisendina

Sisendi tüüp on objektid, mida te saate toru cmdlet tüüpi.

### <a name="outputs"></a>Väljundid

Väljundi tüüp on objektid, mis eraldab cmdlet-käsu tüüp.

## <a name="next-step"></a>Järgmise juhise juurde 

Tutvuge Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
