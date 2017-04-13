<properties
    pageTitle="Azure'i võtme Vault logimine | Microsoft Azure'i"
    description="Selles õpetuses saate abil alustamine Azure'i klahvi Vault logimine."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/31/2016"
    ms.author="cabailey"/>

# <a name="azure-key-vault-logging"></a>Azure'i võtme Vault logimine #
Azure'i klahvi Vault on saadaval enamikes piirkondades. Lisateabe saamiseks lugege teemat [klahvi Vault hinnad lehe](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Sissejuhatus  
Pärast seda, kui olete loonud vähemalt ühe võtme võlvid, mida tõenäoliselt soovite jälgida kuidas ja millal teie olulisi võlvid on kättesaadav, ja kes. Saate seda teha, võimaldades logimine klahvi Vault, mis salvestab teavet Azure storage konto teie jaoks. Uus ümbris, nimega **ülevaateid – logid-auditevent** luuakse automaatselt teie määratud salvestusruumi konto ja abil saate selle sama salvestusruumi konto jaoks mitu põhilistest võlvid logid kogumiseks.

Pääsete oma logiteabe kõige, 10 minutit pärast võtme võlvkelder toiming. Enamikul juhtudel on kiirem kui see.  See on teil hallata oma konto salvestusruumi logide:

- Standardse Azure access juhtelemendi meetodite abil turvaline piiramine, kes pääsevad teie logid.
- Logide, mida soovite talletada salvestusruum konto pole enam kustutada.

Selle õpetuse abil saate alustamine Azure'i klahvi Vault logimine salvestusruumi konto loomisel, logimise lubamine ja tõlgendamine logimine kogutud teavet.  


>[AZURE.NOTE]  Selle õpetuse juhised, kuidas luua võtme võlvid, võtmed ja saladusi ei sisalda. Seda teavet leiate teemast [Azure klahvi Vault alustamine](key-vault-get-started.md). Või mitu platvormi käsurea liides juhiste saamiseks vaadake teemat [õppeteema samaväärsed](key-vault-manage-with-cli.md).
>
>Praegu ei saa konfigureerida Azure klahvi Vault Azure'i portaalis. Selle asemel kasutada Azure PowerShelli neid juhiseid.

Logide koguda saate visualiseeritakse Log analytics kaudu Operations Management Suite abil. Lisateabe saamiseks vt [Log Analytics lahenduse Azure'i klahvi Vault (eelvaade)](../log-analytics/log-analytics-azure-key-vault.md).

Azure'i klahvi Vault kohta ülevaate leiate teemast [mis on Azure klahvi Vault?](key-vault-whatis.md)

## <a name="prerequisites"></a>Eeltingimused

Selle õpetuse lõpuleviimiseks peab olema järgmised:

- Mõne olemasoleva võtme hoidla, mis te kasutate.  
- Azure'i PowerShelli **1.0.1 minimaalne versioon**. Azure'i PowerShelli installimine ja seostada Azure tellimuse, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md). Kui teil on juba installitud Azure PowerShelli ja ei tea selle versiooni Azure PowerShelli konsooli, tippige `(Get-Module azure -ListAvailable).Version`.  
- Oma klahvi Vault logide piisavalt salvestusruum Azure.


## <a id="connect"></a>Ühenduse loomine oma tellimused ##

Mõne Azure PowerShelli seansi käivitamine ja logige sisse Azure'i kontosse järgmine käsk:  

    Login-AzureRmAccount

Hüpikakende brauseriaknas, sisestage oma Azure'i konto kasutajanimi ja parool. Azure'i PowerShelli saavad kõik tellimused, mis on seotud selle kontoga ja vaikimisi, kasutab esimene.

Kui teil on mitu tellimust, peate oma Azure'i klahvi Vault loomiseks kasutatud konkreetse ühe määramiseks. Tippige oma konto tellimused kuvamiseks järgmist.

    Get-AzureRmSubscription

See tellimus, mis on seotud teie olulisi vault teil olla logimine määramiseks tippige:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Azure'i PowerShelli konfigureerimise kohta lisateabe saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).


## <a id="storage"></a>Teie logide salvestusruumi uue konto loomine ##

Kuigi saate oma logide salvestusruumi konto, loome uue salvestusruumi konto, mis on mõeldud klahvi Vault logid. Mugavuse jaoks, kui peame määrata, et seda hiljem, saab salvestada üksikasjad üheks nimega **sa**muutujana.

Täiendavad hõlpsamaks haldus, kasutame ka sama ressursirühm üks, mis sisaldab meie põhilistest vault. [Alustamise õpetus](key-vault-get-started.md)selle ressursirühma nimega **ContosoResourceGroup** ja jätkame kasutada Ida-Aasia asukohta. Asendada need väärtused ise vastavalt vajadusele:

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name ContosoKeyVaultLogs -Type Standard_LRS -Location 'East Asia'


>[AZURE.NOTE]  Kui otsustate kasutada olemasoleva salvestusruumi konto, seda kasutama ühe tellimuse oma võtme vault ja seda tuleb kasutada ressursihaldur juurutamise mudeli, mitte klassikaline juurutamise mudel.

## <a id="identify"></a>Võtme vault oma logide tuvastamine ##

Meie saamisega alustamine õpetuses meie põhilistest vault nimi oli **ContosoKeyVault**, nii, et kasutada seda nime ja salvestada üksikasjad üheks muutuja nimega **kv**jätkame:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a id="enable"></a>Logimise lubamine ##

Klahv Vault logimise lubamiseks kasutame cmdlet Set-AzureRmDiagnosticSetting koos lõime meie uut salvestusruumi konto ja meie põhilistest vault muutujaid. Määrame ka selle **-lubatud** **$true** lipu ja kategooria määramine (ainult kategooria klahvi Vault logimiseks), AuditEvent:


    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

Väljundi see sisaldab järgmist:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


See kinnitab, et logimine on lubatud nüüd teie võtme vault, salvestades teavet konto salvestusruumi.

Soovi korral ka saate säilituspoliitika oma logide nii, et vanemad logid kustutatakse automaatselt. Näiteks säilituspoliitika **$true** **- RetentionEnabled** lipu seadmine ja määrake **- RetentionInDays** parameetri **90** nii, et logid, mis on vanemad kui 90 päeva kustutatakse automaatselt.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Mis on sisse logitud:

- Kõik autenditud REST API kutsed on sisse logitud, mis sisaldab nurjunud taotlusi juurdepääsuõigused, süsteemi tõrgete või vigased taotlused.
- Toimingute võti võlvkelder ise, mis sisaldab loomine, kustutamine, säte võtme vault juurdepääsupoliitikaid, ja värskendamine võtme vault atribuute nagu sildid.
- Toimingute kiirklahvid ja saladusi võtme võlvkelder, mis sisaldab loomine, muutmine või kustutamine järgmiste klahvide või ametisaladusi; märk, nagu kontrollida, krüptida, dekrüptida, Murra ja lahtipakkimiseks klahvid, saladusi, loendi võtmed ja saladusi ja nende versioone.
- Autentimata taotlused tulemuseks 401 vastuse. Näiteks taotlusi, mis on esitaja märgiks on vigane või aegunud või on mõni Kehtetu luba.  


## <a id="access"></a>Juurdepääs teie logid ##

Võtme vault logid salvestatakse **ülevaateid – logid-auditevent** container andsite salvestusruumi konto. Loetle kõik selles ümbrises plekid, tippige:

    Get-AzureStorageBlob -Container 'insights-logs-auditevent' -Context $sa.Context

Väljund näeb välja umbes selline:

**Container Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**


**Nimi**

**----**

**ResourceIdkasutamisel = / tellimuste/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/pakkujate/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**ResourceIdkasutamisel = / tellimuste/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/pakkujate/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

** ResourceIdkasutamisel = / tellimuste/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/pakkujate/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json***


Nagu näete selle väljund, järgige plekid nimereeglistik: **ResourceIdkasutamisel =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/filename.json**

Kuupäeva ja kellaaja väärtuste kasutada UTC.

Kuna sama salvestusruumi konto saab kasutada mitme ressursid logid kogumiseks, täielik ressursi ID bloobimälu nimi on väga kasulik juurdepääsu või allalaadimiseks peate plekid. Kuid enne seda, kuidas alla laadida kõik plekid esmalt hõlmab.

Esmalt looge kaust, kuhu soovite alla laadida plekid. Näiteks:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Klõpsake loendi kõigi plekid hankimine:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Toru selle loendi kaudu Get-AzureStorageBlobContent allalaadimiseks plekid meie sihtkoht kausta:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

See teise käsu käivitamisel kuvatakse **/** eraldaja bloobimälu nimede loomine täielik kausta struktuuri sihtkausta ja selle struktuuri kasutab alla laadida ja plekid failidena salvestada.

Plekid valikuliselt allalaadimiseks kasutada metamärke. Näiteks:

- Kui teil on mitu võtme võlvid ja soovite alla laadida ainult üks klahv võlvkelder logisid, nimega CONTOSOKEYVAULT3:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3

- Kui teil on mitu ressursi rühmad ja soovite alla laadida ainult ühe ressursirühma logid, kasutage `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'

- Kui soovite alla laadida kõik logid kuu jaanuar 2016, saate kasutada `-Blob '*/year=2016/m=01/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

Nüüd olete valmis alustama, vaadates, mis on logid. Kuid enne peale, et Get-AzureRmDiagnosticSetting, mida peate teadma kahte veel parameetrid:

- Päringu oleku diagnostikasätete oma võtme vault ressurss:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`

- Logimise teie olulisi vault ressursi keelamiseks tehke järgmist.`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`


## <a id="interpret"></a>Oma klahvi Vault logid tõlgendamine ##

Üksikute plekid talletatakse tekstina vormindatud JSON bloobimälu. Järgmine on näide Logi kirje töötab `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


Järgmises tabelis on loetletud välja nimede ja kirjelduste.


| Välja nimi        | Kirjeldus |
| ------------- |-------------|
| aeg      | Kuupäev ja kellaaeg (UTC).|
| ResourceIdkasutamisel      | Azure'i ressursihaldur ressursi ID-ga. Klahv Vault logid, on alati võti Vault ressursi ID-ga.|
| operationName      | Toimingu, nagu dokumenteerida järgmise tabeli nimi.|
| operationVersion      | See on kliendi poolt küsitud REST API versioon.|
| kategooria      | Klahv Vault logide, AuditEvent on üks, mis on saadaval väärtus.|
| resultType      | Tulemus REST API taotluse.|
| resultSignature      | HTTP olek.|
| resultDescription     | Tulemus, kui need on saadaval täiendav kirjeldus.|
| durationMs      | Teenuse REST API taotluse, millisekundites kulunud aeg. See ei sisalda latentsuse võrgu nii, et kliendi poolel mõõta aeg ei kattu seekord.|
| callerIpAddress      | Taotluse esitanud kliendi IP-aadress.|
| correlationId      | Valikuline GUID, mis kliendi oleksid koos teenuse (võtme Vault) logidesse kliendipoolseid logisid saab edastada.|
| identiteedi      | Identiteedi märgiks ei esitatud REST API taotluse tegemisel. See on tavaliselt "kasutaja", "teenuse põhilise" või "kasutaja + appId" kombinatsiooni nagu tuleneb Azure PowerShelli cmdlet-käsu taotluse puhul.|
| atribuudid      | Väli sisaldab erineva teabe põhjal toimingu (operationName). Enamikul juhtudel sisaldab kliendi teavet (User Agent string vastu võetud klient), täpse REST API taotluse URI ja HTTP olekukoodi. Lisaks, kui objekti tagastatakse taotluse (nt KeyCreate või VaultGet) see sisaldab ka klahvi URI (nimega "id"), Vault URI või salajane URI.|




**OperationName** välja väärtused ObjectVerb vormingus. Näiteks:

- Võtme vault kõik toimingud on selle ' Vault`<action>`' vormindada, näiteks `VaultGet` ja `VaultCreate`.

- Kõik peamised toimingud on selle ' klahvi`<action>`' vormindada, näiteks `KeySign` ja `KeyList`.

- Salajane kõik toimingud on selle ' salajane`<action>`' vormindada, näiteks `SecretGet` ja `SecretListVersions`.

Järgmises tabelis on loetletud operationName ja vastava REST API käsu.

| operationName        | REST API käsk |
| ------------- |-------------|
| Autentimine      | Azure Active Directory lõpp-punkti kaudu|
| VaultGet      | [Võtme vault kohta teabe saamine](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx)|
| VaultPut      | [Loomine ja värskendamine võtme vault](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx)|
| VaultDelete      | [Võtme vault kustutamine](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx)|
| VaultPatch      | [Võtme vault värskendamine](https://msdn.microsoft.com/library/azure/mt620025.aspx)|
| VaultList      | [Loendi kõigi võtme võlvid ressursi rühma](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx)|
| KeyCreate      | [Loo võti](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx)|
| KeyGet      | [Klahvi kohta teabe saamine](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx)|
| KeyImport      | [Võlvkelder klahvi importimine](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx)|
| KeyBackup      | [Varundus klahvi](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).|
| KeyDelete      | [Klahvi kustutamine](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx)|
| KeyRestore      | [Võtme taastamine](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx)|
| KeySign      | [Logige võti](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx)|
| KeyVerify      | [Kontrollige võti](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx)|
| KeyWrap      | [Murra klahvi](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx)|
| KeyUnwrap      | [Lahtipakkimiseks klahvi](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx)|
| KeyEncrypt      | [Krüpti parooliga klahvi](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx)|
| KeyDecrypt      | [Võti dekrüptimine](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx)|
| KeyUpdate      | [Värskendage klahvi](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx)|
| KeyList      | [Loendis toodud klahvid võlvkelder](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx)|
| KeyListVersions      | [Loendi klahvi versioone](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx)|
| SecretSet      | [Salajane loomine](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx)|
| SecretGet      | [Salajane hankimine](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx)|
| SecretUpdate      | [Salajane värskendamine](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx)|
| SecretDelete      | [Salajane kustutamine](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx)|
| SecretList      | [Loendi saladusi võlvkelder](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx)|
| SecretListVersions      | [Salajane loendi versioonid](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx)|




## <a id="next"></a>Järgmised sammud ##

Azure'i klahvi Vault kasutava veebirakenduse juhendi, leiate [Kasutamine Azure klahvi Vault veebirakenduse kaudu](key-vault-use-from-web-application.md).

Viited kavandamiseks leiate [Azure'i klahvi Vault arendaja juhendist](key-vault-developers-guide.md).

Loendi Azure'i klahvi Vault 1.0 Azure PowerShelli cmdlet-käsud, leiate [Azure'i klahvi Vault cmdlet-käsud](https://msdn.microsoft.com/library/azure/dn868052.aspx).

Õpetuse võtme pööre ja Logi koos Azure klahvi Vault auditeerimine, vaadake, [Kuidas häälestamise klahvi Vault lõpuni võtme pööramine ja audit](key-vault-key-rotation-log-monitoring.md).
