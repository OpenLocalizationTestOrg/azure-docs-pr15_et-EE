<properties
    pageTitle="Arhiivimine Diagnostikalogide Azure | Microsoft Azure'i"
    description="Saate teada, kuidas Azure'i diagnostika logid pikaajaline säilitus salvestusruumi konto arhiivida."
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
    ms.date="08/26/2016"
    ms.author="johnkem"/>

# <a name="archive-azure-diagnostic-logs"></a>Azure'i Diagnostikalogid arhiivimine
Selles artiklis näitame kasutamist Azure portaali, PowerShelli cmdlet-käskude, CLI või REST API arhiivida [Azure'i Diagnostikalogid](monitoring-overview-of-diagnostic-logs.md) salvestusruumi konto. See suvand on kasulik, kui soovite säilitada oma Diagnostikalogid koos valikuline säilituspoliitika auditilogi, staatilise analüüsi või varundamine.

## <a name="prerequisites"></a>Eeltingimused
Enne alustamist peate [salvestusruumi konto loomine](../storage/storage-create-storage-account.md#create-a-storage-account) , millele saate oma Diagnostikalogid arhiivida. Soovitame, et te ei kasuta olemasoleva salvestusruumi kontoga, millel on muude, mitte jälgimise nii, et saate paremini kontrollida juurdepääsu andmete jälgimiseks sellel talletatud andmed. Siiski kui arhiivite ka oma tegevuse Log ja diagnostika mõõdikute salvestusruumi konto, siis oleks mõttekas kasutada salvestusruumi konto jaoks oma Diagnostikalogid kõik andmeid hoida ühes keskses kohas. Kasutatava salvestusruumi konto peab olema üldine otstarve salvestusruumi konto, mitte bloobimälu salvestusruumi konto.

## <a name="diagnostic-settings"></a>Diagnostikasätete
Arhiivida oma Diagnostikalogid, kasutades mõnda järgmistest viisidest, määrake **Diagnostika seade** konkreetse ressursi. Mis on, mis on talletatud või voona ja väljundid logib diagnostika säte jaoks ressursi määratleb kategooriates – salvestusruumi konto ja/või sündmuse jaoturi. See määratleb säilituspoliitika (säilitamise päevade arv) iga log kategooria salvestusruumi kontole salvestatud sündmuste jaoks. Kui säilituspoliitika on määratud null, sündmuste logi kategooria on talletatud lõputult (see on öelda terve igavik). Säilituspoliitika vastasel juhul võib olla mis tahes päevade arv vahemikus 1 – 2147483647. [Lugege kohta siin diagnostikasätete](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).

## <a name="archive-diagnostic-logs-using-the-portal"></a>Arhiivimine Diagnostikalogide portaalis

1. Klõpsake portaali, ressurss, kuhu soovite lubada arhiivimine Diagnostikalogide, keelde ressursi.
2. Valige menüü ressurssi sätted jaotises **jälgimine** **diagnostika**.

    ![Ressursi menüü jälgimine](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-sec.png)
3. Märkige ruut **ekspordi salvestusruumi konto**ja seejärel valige salvestusruumi konto. Soovi korral saate määrata, kasutades järgmisi logide säilitamise päevade arv on **säilituspoliitika (päeva)** liugureid. Säilitamine null päeva talletab logid lõpmatuseni.

    ![Diagnostika logid blade](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-blade.png)
4. Klõpsake nuppu **Salvesta**.

Diagnostikalogide arhiivitakse salvestusruumi sellele kontole kohe, kui luuakse uus sündmus andmed.

## <a name="archive-diagnostic-logs-via-the-powershell-cmdlets"></a>Arhiivimine Diagnostikalogide kaudu PowerShelli cmdlet-käsud

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Atribuut         | Nõutav | Kirjeldus                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| ResourceIdkasutamisel       | Jah      | Ressursi ID-d ressurss, millele soovite seada diagnostika seade.                            |
| StorageAccountId | Ei       | Ressursi ID salvestusruumi konto, kuhu salvestatakse Diagnostikalogid.                          |
| Kategooriad       | Ei       | Log kategooriate lubamiseks komaga eraldatud loend.                                                     |
| Lubatud          | Jah      | Kahendväärtus, mis näitab, kas diagnostika on lubatud või keelatud ressurss.                  |
| RetentionEnabled | Ei       | Kahendväärtus, mis näitab, kui säilituspoliitika on lubatud ressurss.                            |
| RetentionInDays  | Ei       | Millele tuleks sündmuste säilitada vahemikus 1 – 2147483647 päevade arv. Väärtuseks null talletab logid lõpmatuseni. |

## <a name="archive-the-activity-log-via-the-cross-platform-cli"></a>Tegevuste Logi kaudu platvormidel CLI arhiivimine

```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| Atribuut         | Nõutav | Kirjeldus                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| ResourceIdkasutamisel       | Jah      | Ressursi ID-d ressurssi, millele soovite seada diagnostika seade.                            |
| storageId        | Ei       | Ressursi ID salvestusruumi konto, kuhu salvestatakse Diagnostikalogid.                          |
| Kategooriad       | Ei       | Log kategooriate lubamiseks komaga eraldatud loend.                                                     |
| lubatud          | Jah      | Kahendväärtus, mis näitab, kas diagnostika on lubatud või keelatud ressurss.                  |

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Arhiivimine Diagnostikalogide REST API kaudu
Lisateavet selle kohta, kuidas saate häälestada diagnostika seade, kasutades Azure kuvari REST API [seda dokumenti vaadata](https://msdn.microsoft.com/library/azure/dn931931.aspx) .

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Diagnostikalogide skeemi salvestusruumi konto
Kui olete loonud arhiivimine, luuakse salvestusruumi container salvestusruumi konto Sündmuse ilmnemise log kategooriate, mida teil on lubatud. Pakendi sees plekid tehke sama vormingut üle Diagnostikalogid ja tegevuste Logi. Need plekid struktuuri on:

> ülevaateid – logid-{Logi kategooria nimi} / ResourceIdkasutamisel = / tellimuste / {Tellimuse ID} /RESOURCEGROUPS/ {ressursirühma nimi} /PROVIDERS/ {ressursinimi pakkuja} / {ressursi tüüp} / {ressursinimi} / y = {arvuline neljakohalise aastaarvu} / m = {kahekohaliste arvuline kuu} / d = {kahekohaliste arvuline päev} / h = {kahekohaliste 24-tunnist kella hour}/m=00/PT1H.json

Või rohkem lihtsalt

> ülevaateid – logid-{Logi kategooria nimi} / ResourceIdkasutamisel = / {ressursi Id} / y = {arvuline neljakohalise aastaarvu} / m = {kahekohalise arvuline kuu} / d = {kahekohalise arvuline päev} / h = {kahekohalise 24-tunnist kella hour}/m=00/PT1H.json

Näiteks võib olla bloobimälu nimi.

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json

Iga PT1H.json bloobimälu sisaldab JSON bloobimälu bloobimälu URL-is määratud tunni jooksul toimunud sündmuste (näiteks h = 12). Käesoleva tunni jooksul sündmused on lisatud faili PT1H.json esinevad. Minute väärtus (m = 00) on alati 00, kuna diagnostika sündmused on jagatud üksikute plekid tunnis.

PT1H.json failis, salvestatakse iga sündmuse "kirjete" massiiv, järgides selles vormingus:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Elemendi nime  | Kirjeldus                                                                                                 |
|---------------|-------------------------------------------------------------------------------------------------------------|
| aeg          | Kui sündmus on loodud sündmuse vastava päringu töötlemise Azure'i teenus võttis ajatempli. |
| ResourceIdkasutamisel    | Ressursi ID kannatavas ressursi.                                                                       |
| operationName | Toimingu nimi.                                                                                      |
| kategooria      | Log kategooria soovitud sündmus.                                                                                  |
| atribuudid    | Kogum `<Key, Value>` paari (st sõnastik), mis kirjeldab Sündmuse üksikasjade.                            |

> [AZURE.NOTE] Atribuudid ja nende atribuutide kasutamist võivad erineda sõltuvalt ressursi.

## <a name="next-steps"></a>Järgmised sammud
- [Laadige alla plekid analüüsi jaoks](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Kui soovite sündmuse jaoturi voo Diagnostikalogid](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Lisateavet vt teemast Diagnostikalogid](monitoring-overview-of-diagnostic-logs.md)
