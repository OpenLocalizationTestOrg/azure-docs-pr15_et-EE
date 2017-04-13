<properties
    pageTitle="Azure'i tegevuste Logi sündmus jaoturi voona | Microsoft Azure'i"
    description="Saate teada, kuidas Azure'i tegevuste Logi sündmus jaoturi voona."
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
    ms.date="10/03/2016"
    ms.author="johnkem"/>

# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Voo sündmuse jaoturi Azure tegevuste Logi
[**Azure'i tegevuste Logi**](./monitoring-overview-activity-logs.md) saate voona peaaegu reaalajas mõnda rakendust suvandiga sisseehitatud "Ekspordi" portaalis või teenuse siini reegli Id Logi profiili Azure PowerShelli cmdlet-käskude või Azure CLI kaudu võimaldab.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Mida saab teha Log tegevuste ja sündmuste jaoturi
Siin on paar võimalust streaming võimalus kasutada tegevuste Logi.

- **Voo kolmanda osapoole logimine ja telemeetria süsteemid** – muutuvad aja jooksul, sündmuse jaoturi streaming süsteem oma tegevuste Logi toru kolmanda osapoole SIEMs ja logige analytics lahendusi.
- **Luua kohandatud telemeetria ja logimine platvormi** – kui teil juba on lähtuvalt ehitatud telemeetria platvormi või on ainult üks koostamise mõelda, väga paindlik avaldada-Telli laadi sündmuse jaoturi võimaldab paindlikult neelata tegevuste Logi. [Lugege teemat Dan Rosanova juhendi abil ürituse jaoturi globaalne skaala telemeetria platvormi siin.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Tegevuste Logi streaming lubamine
Saate lubada streaming tegevuste Logi programmiliselt või portaali kaudu. Mõlemal juhul saate valida teenuse siini Namespace ja selle nimeruumi ühiskasutusega juurdepääsu poliitika ja mõni sündmus jaoturi on loodud selle nimeruumi, kui esimene uue tegevuse Log sündmuse. Kui teil pole teenuse siini Namespace, peate esmalt loomiseks. Kui teil on varem voona tegevuse sündmuste selle teenuse siini Namespace, sündmuse jaoturi, mis on varem loodud taaskasutada. Ühiskasutusega juurdepääsu poliitika määratleb õigused, mis on streaming süsteem. Täna, voogesitus mõni sündmus jaoturi nõuab õiguste **haldamine**, **lugemine**ja **saatmine** . Saate luua või muuta oma teenuse siini Namespace teenuse siini Namespace ühiskasutuses juurdepääsupoliitikaid klassikaline portaalis vahekaardil "Konfigureerimine". Tegevuste Logi Logi profiili kaasamiseks streaming värskendamiseks olema kasutaja muudatuste tegemist ListKey õigus see teenus siini autoriseerimine reegel.

### <a name="via-azure-portal"></a>Azure portaali kaudu 
1. Liikuge **Tegevuse Log** tera portaali vasakus servas menüü kaudu.

    ![Liikuge tegevuse Log portaalis](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klõpsake nuppu **ekspordi** tera ülaosas.

    ![Ekspordi nuppu portaalis](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Tera, mis kuvatakse, saate valida regioonid, mille jaoks soovite voo sündmuste ja teenuse siini Namespace, kus soovite mõne sündmuse jaoturi streaming need sündmused luua.

    ![Tegevuste Logi blade eksportimine](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klõpsake sätete salvestamiseks **Salvesta** . Sätted rakenduvad kohe tellimuse.


### <a name="via-powershell-cmdlets"></a>PowerShelli cmdlet-käskude kaudu
Kui Logi profiili juba olemas, peate esmalt selle profiili eemaldada.

1. Kasutage `Get-AzureRmLogProfile` tuvastada, kas Logi profiili olemas
2. Sel juhul kasutada `Remove-AzureRmLogProfile` selle eemaldada.
3. Kasutage `Set-AzureRmLogProfile` profiili loomiseks:

```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

Teenuse siini reegel ID on string, mis on selles vormingus: {teenuse siini ressursi ID} /authorizationrules/ {võtme nimi}, nt 

### <a name="via-azure-cli"></a>Azure'i CLI kaudu
Kui Logi profiili juba olemas, peate esmalt selle profiili eemaldada.

1. Kasutage `azure insights logprofile list` tuvastada, kas Logi profiili olemas
2. Sel juhul kasutada `azure insights logprofile delete` selle eemaldada.
3. Kasutage `azure insights logprofile add` profiili loomiseks:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

Teenuse siini reegel ID on string, mis on selles vormingus: `{service bus resource ID}/authorizationrules/{key name}`.
 
## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Kuidas kasutada log andmed sündmuse jaoturi mobiilirakendustes?
[Skeemi Logi tegevuste jaoks on saadaval](./monitoring-overview-activity-logs.md). Iga sündmuse on massiivis JSON plekid nimega "kirjete."

## <a name="next-steps"></a>Järgmised sammud
- [Tegevuste Logi salvestusruumi konto arhiivimine](./monitoring-archive-activity-log.md)
- [Lugege ülevaadet Azure'i tegevuste Logi](./monitoring-overview-activity-logs.md)
- [Teatise põhjal tegevuse Log sündmuse häälestamine](./insights-auditlog-to-webhook-email.md)
