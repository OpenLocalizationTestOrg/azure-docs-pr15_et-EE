<properties
    pageTitle="Ülevaade: Azure'i tegevuste Logi | Microsoft Azure'i"
    description="Siit saate teada, mis on Azure tegevuste Logi ja kuidas kasutada seda mõista sündmused, mis ilmnevad Azure tellimuse."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="johnkem"/>

# <a name="overview-of-the-azure-activity-log"></a>Azure'i tegevuste Logi ülevaade
**Azure'i tegevuse Log** on log, mis annab ülevaate toimingud, mis olid läbi ressursid teie tellimus. Tegevuste Logi kandis varem nime "Auditilogid" või "Funktsionaalseid logid", kuna see aruannete juhtelemendi-plane sündmuste jaoks tellimuste. Tegevuste Logi kasutamisel saate määrata selle ", mis, kes ja kui" mis tahes kirjutage toimingud (panna, postituse, DELETE) teie tellimus ressursside kohta. Saate aru, oleku ja muud asjakohased atribuudid. Tegevuste Logi ei sisalda read (saada) toimingud.

Tegevuste Logi erineb [Diagnostikalogid](./monitoring-overview-of-diagnostic-logs.md), mis on kõik logid kiiratava ressursi. Need logid sisestada andmeid selle ressursi toimimist, mitte selle ressursi toiminguid.

Saate tuua sündmuste oma tegevuste Logi Azure'i portaalis, CLI PowerShelli cmdlet-käskude ja Azure kuvari REST API-ga.

Saate vaadata selle [video tegevuse Log tutvustus](https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz).  

## <a name="what-you-can-do-with-the-activity-log"></a>Mida saab teha tegevuste Logi
Järgnevalt mõned asjad, mida saate teha tegevuste Logi.

- Päringu ja seda vaadata **Azure portaali**.
- Päringu see REST API-ga, PowerShelli cmdleti või CLI kaudu.
- [Saate luua e-posti või webhook teatise, mis käivitab sündmuse tegevuste Logi välja.](./insights-auditlog-to-webhook-email.md)
- [Salvestage see **Salvestusruumi konto** arhiivimine või käsitsi kontrollimiseks](./monitoring-archive-activity-log.md). Säilituspoliitika aeg (päevades) **Profiilid**abil saate määrata.
- Analüüsida PowerBI abil [**sisu PowerBI keelepaketi**](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/).
- [See **Sündmus jaoturi** voona](./monitoring-stream-activity-logs-event-hubs.md) kolmanda osapoole teenusele või kohandatud analytics lahenduse, nt PowerBI kohta.

## <a name="export-the-activity-log-with-log-profiles"></a>Tegevuste Logi profiilid eksportimine
**Logi profiili** määrab, kuidas oma tegevuse Log eksporditakse. Logi profiili abil saate konfigureerida.

- Kus tuleks saata tegevuse Log (salvestusruumi konto või sündmuse jaoturi)
- Saadetakse sündmuse kategooriate (kirjutamine, Kustuta, toimingu)
- Tuleb eksportida piirkonnad (asukohad)
- Kui kaua tuleks tegevuste Logi säilitada salvestusruumi konto – säilitamine null päeva tähendab, et logifailid terve igavik. Muul juhul väärtus võib olla mis tahes päevade arv vahemikus 1 – 2147483647. Kui säilituspoliitikate määratakse, kuid logide salvestamise salvestusruumi konto (nt kui sündmus jaoturi või OMS suvandid on valitud ainult) on keelatud, säilituspoliitikate ei mõjuta.

Neid sätteid saab konfigureerida tegevuse Log tera portaalis suvand "Ekspordi" kaudu. Neid saab konfigureeritud programmiliselt, [kasutades Azure kuvari REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx), PowerShelli cmdlet-käskude või CLI. Tellimus võib olla ainult üks Logi profiili.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Azure'i portaalis profiilid konfigureerimine
Saate voona jaoturiga sündmuste logi tegevuse või neid talletada salvestusruum konto Azure portaali "Ekspordi" suvandi abil.

1. Liikuge **Tegevuse Log** tera portaali vasakus servas menüü kaudu.

    ![Liikuge tegevuse Log portaalis](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klõpsake nuppu **ekspordi** tera ülaosas.

    ![Ekspordi nuppu portaalis](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Tera, mis kuvatakse, saate valida:  
    - regioonid, mille soovite eksportida sündmused
    - Salvestusruumi konto, mida soovite salvestada sündmused
    - soovite säilitada nende sündmuste salvestusruumi päevade arv. Säte 0 päevade säilitab logid terve igavik.
    - Teenuse siini Namespace, kus soovite mõne sündmuse jaoturi streaming need sündmused luua.

    ![Tegevuste Logi blade eksportimine](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klõpsake sätete salvestamiseks **Salvesta** . Sätted rakenduvad kohe tellimuse.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Azure'i PowerShelli cmdlet-käskude kasutamine profiilid konfigureerimine
#### <a name="get-existing-log-profile"></a>Olemasoleva Logi profiili hankimine
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Logi profiili lisamine
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Atribuut         | Nõutav | Kirjeldus   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nimi             | Jah      | Logi profiili nimi.                                 |
| StorageAccountId | Ei       | Salvestusruumi konto, kuhu salvestatakse tegevuste Logi ressursi ID-d.                         |
| serviceBusRuleId | Ei       | Teenuse siini reegli ID teenus siini nimeruumi soovite on loodud sündmuse jaoturi. On string, mis on selles vormingus: `{service bus resource ID}/authorizationrules/{key name}`. |
| Asukohad        | Jah      | Komaga eraldatud loend piirkondade jaoks, mille soovite koguda tegevuse sündmused.              |
| RetentionInDays  | Jah      | Millised sündmused tuleks säilitada, vahemikus 1 – 2147483647 päevade arv. Väärtuseks null talletab logid lõputult (terve igavik). |
| Kategooriad       | Ei       | Sündmuse kategooriad, mida tuleks komaga eraldatud loend. Võimalikud väärtused on kirjutamine, kustutus- ja toiming.                                 |

#### <a name="remove-a-log-profile"></a>Logi profiili eemaldamine
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cross-platform-cli"></a>Azure'i platvormidel CLI abil profiilid konfigureerimine
#### <a name="get-existing-log-profile"></a>Olemasoleva Logi profiili hankimine
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
Funktsiooni `name` atribuut peaks olema Logi profiili nimi.

#### <a name="add-a-log-profile"></a>Logi profiili lisamine
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Atribuut         | Nõutav | Kirjeldus   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nimi             | Jah      | Logi profiili nimi.                                 |
| storageId        | Ei       | Millele tuleks salvestada tegevuse Log salvestusruumi konto ressursi ID-d.                         |
| serviceBusRuleId | Ei       | Teenuse siini reegli ID teenus siini nimeruumi soovite on loodud sündmuse jaoturi. On string, mis on selles vormingus: `{service bus resource ID}/authorizationrules/{key name}`. |
| asukohad        | Jah      | Komaga eraldatud loend piirkondade jaoks, mille soovite koguda tegevuse sündmused.              |
| retentionInDays  | Jah      | Millised sündmused tuleks säilitada, vahemikus 1 – 2147483647 päevade arv. Väärtuseks null talletab logid lõputult (terve igavik).     |
| Kategooriad       | Ei       | Sündmuse kategooriad, mida tuleks komaga eraldatud loend. Võimalikud väärtused on kirjutamine, kustutus- ja toiming.                                 |

#### <a name="remove-a-log-profile"></a>Logi profiili eemaldamine
```
azure insights logprofile delete --name my_log_profile
```

## <a name="event-schema"></a>Sündmuse skeem
Iga sündmuse tegevuste Logi on JSON bloobimälu, mis on sarnane selles näites.

```
{
  "value": [ {
    "authorization": {
      "action": "microsoft.support/supporttickets/write",
      "role": "Subscription Admin",
      "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
    },
    "caller": "admin@contoso.com",
    "channels": "Operation",
    "claims": {
      "aud": "https://management.core.windows.net/",
      "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
      "iat": "1421876371",
      "nbf": "1421876371",
      "exp": "1421880271",
      "ver": "1.0",
      "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
      "puid": "20030000801A118C",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
      "name": "John Smith",
      "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
      "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
      "appidacr": "2",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.microsoft.com/claims/authnclassreference": "1"
    },
    "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "description": "",
    "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
    "eventName": {
      "value": "EndRequest",
      "localizedValue": "End request"
    },
    "eventSource": {
      "value": "Microsoft.Resources",
      "localizedValue": "Microsoft Resources"
    },
    "httpRequest": {
      "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
      "clientIpAddress": "192.168.35.115",
      "method": "PUT"
    },
    "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
    "level": "Informational",
    "resourceGroupName": "MSSupportGroup",
    "resourceProviderName": {
      "value": "microsoft.support",
      "localizedValue": "microsoft.support"
    },
    "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
    "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "operationName": {
      "value": "microsoft.support/supporttickets/write",
      "localizedValue": "microsoft.support/supporttickets/write"
    },
    "properties": {
      "statusCode": "Created"
    },
    "status": {
      "value": "Succeeded",
      "localizedValue": "Succeeded"
    },
    "subStatus": {
      "value": "Created",
      "localizedValue": "Created (HTTP Status Code: 201)"
    },
    "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
    "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
    "subscriptionId": "s1"
  } ],
"nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```

| Elemendi nime         | Kirjeldus             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| luba        | Bloobimälu RBAC atribuutide sündmuse. Tavaliselt sisaldab "toiming", "roll" ja "ulatus" atribuudid.|
| helistaja               | E-posti aadress on sooritatud toiming, UPN-i taotlust või SPN taotluste põhjal kättesaadavus kasutaja.|
| kanalid             | Üks järgmistest väärtustest: "Admin", "Seda toimingut"|
| correlationId        | Tavaliselt GUID string vormingus. Sama uber toimingu kuuluvad sündmused, mis on correlationId ühiskasutusse anda.   |
| kirjeldus          | Sündmuse kirjeldus staatiliseks tekstiks.                              |
| eventDataId          | Sündmuse ainuidentifikaator.    |
| eventSource          | Azure'i teenus või taristu, mis on loodud sündmuse nimi.    |
| httpRequest          | Bloobimälu, mis kirjeldab Http-päring. Tavaliselt hõlmab "clientRequestId", "clientIpAddress" ja "meetod" (HTTP-meetod. For example, Pange).                            |
| tase                | Sündmuse tase. Üks järgmistest väärtustest: "Kriitiline", "Tõrge", "Hoiatus", "Teatised" ja "Verbose"  |
| resourceGroupName    | Kannatavas ressursi ressursirühma nimi.               |
| resourceProviderName | Ressursi pakkuja nimi kannatavas ressursi             |
| resourceUri          | Ressursi id kannatavas ressursi.                               |
| operationId          | GUID sündmusi, mis vastavad ühekordne jagada.         |
| operationName        | Toimingu nimi.  |
| atribuudid           | Kogum `<Key, Value>` paari (ehk teisisõnu öeldes sõnastiku), mis kirjeldab Sündmuse üksikasjade.                                |
| olek               | String, mis kirjeldab toimingu olekut. Mõned levinud väärtused: alustamine edenemist, õnnestus, nurjus, aktiivne, lahendatud.                                |
| subStatus            | Tavaliselt HTTP olekukoodi vastavate ülejäänud helistada, kuid võite ka muude stringide kirjeldamiseks on substatus, nagu need levinud väärtused: OK (HTTP olekukoodi: 200), loodud (HTTP olekukoodi: 201), aktsepteeritud (HTTP olekukoodi: 202), mitte sisu (HTTP olekukoodi: 204), vigane päring (HTTP olekukoodi: 400), ei leitud (HTTP olekukoodi: 404), konflikti (HTTP olekukoodi : 409), Sisemine serveritõrge (HTTP olekukoodi: 500), teenus pole saadaval (HTTP olekukoodi: 503), lüüsi ajalõpp (HTTP olekukoodi: 504). |
| eventTimestamp       | Kui sündmus on loodud sündmuse vastava päringu töötlemise Azure'i teenus võttis ajatempli.     |
| submissionTimestamp  | Ajatempel, kui sündmus sai päringute jaoks saadaval.             |
| subscriptionId       | Azure'i Tellimuse ID-ga.  |
| nextLink             | Jätkamiseks luba toomiseks täitmisega saate tulemusi, kui need on jagatud mitme vastuse. Tavaliselt vaja, siis on üle 200 kirjet.     |

## <a name="next-steps"></a>Järgmised sammud
- [Lisateave tegevuste Log (varem auditilogid)](../resource-group-audit.md)
- [Voo sündmuse jaoturi Azure tegevuste Logi](./monitoring-stream-activity-logs-event-hubs.md)
