<properties
    pageTitle="Azure'i tegevuste Logi arhiivimine | Microsoft Azure'i"
    description="Saate teada, kuidas oma Azure'i tegevuse logifail pikaajaline säilitus salvestusruumi konto arhiivida."
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
    ms.date="08/23/2016"
    ms.author="johnkem"/>

# <a name="archive-the-azure-activity-log"></a>Azure'i tegevuste Logi arhiivimine
Selles artiklis näitame kasutamist Azure portaali, PowerShelli cmdlet-käskude või mitu platvormi CLI arhiivida [**Azure'i tegevuse Log**](monitoring-overview-activity-logs.md) salvestusruumi konto. See suvand on kasulik, kui soovite säilitada oma tegevust samamoodi kui 90 päeva (üle täielik kontroll säilituspoliitika) audit, staatiline analüüs või varundamine. Kui soovite säilitada oma sündmused 90 päeva või vähem teil pole vaja luua arhiivimine salvestusruumi konto, kuna tegevuse sündmuste säilitatakse Azure'i platvormi 90 päeva ilma lubamine arhiivimine.

## <a name="prerequisites"></a>Eeltingimused
Enne alustamist peate [salvestusruumi konto loomine](../storage/storage-create-storage-account.md#create-a-storage-account) , millele saate oma tegevuste Logi arhiivida. Soovitame, et te ei kasuta olemasoleva salvestusruumi kontoga, millel on muude, mitte jälgimise nii, et saate paremini kontrollida juurdepääsu andmete jälgimiseks sellel talletatud andmed. Siiski kui arhiivite ka Diagnostikalogid ja mõõdikute salvestusruumi konto, siis oleks mõttekas kasutada salvestusruumi konto jaoks oma tegevuse Log kõik andmeid hoida ühes keskses kohas. Kasutatava salvestusruumi konto peab olema üldine otstarve salvestusruumi konto, mitte bloobimälu salvestusruumi konto.

## <a name="log-profile"></a>Logi profiili
Arhiivida tegevuste logi abil mõnda järgmistest viisidest, saate määrata **Logi profiili** tellimusele. Logi profiili määratleb sündmused, mis on talletatud või voona ja väljundid – salvestusruumi konto ja/või sündmuse jaoturi. See määratleb säilituspoliitika (säilitamise päevade arv) talletatud salvestusruumi konto sündmuste jaoks. Kui säilituspoliitika on määratud null, talletatakse sündmuste lõpmatuseni. Muul juhul saate selle määrata mis tahes väärtus vahemikus 1 – 2147483647. [Lugege kohta log Profiilid siin](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles).

## <a name="archive-the-activity-log-using-the-portal"></a>Tegevuste Logi portaalis arhiivimine
1. Portaalis linki **Tegevuse Log** vasakul navigeerimispaanil. Kui te ei näe linki Logi tegevuste jaoks, klõpsake linki **Veel teenuste** esmalt.

    ![Liikuge tegevuse Log blade](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. Tera ülaosas nuppu **ekspordi**.

    ![Klõpsake nuppu ekspordi](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. Tera, mis kuvatakse, märkige ruut **eksportimine salvestusruumi kontole** ja valige salvestusruumi konto.

    ![Salvestusruumi konto määramine](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. Kasutades liugurit või tekstivälja, määrata mitu päeva, mis tuleks hoida tegevuse sündmuste kontol salvestusruumi. Kui eelistate andmete säilis salvestusruumi konto lõpmatuseni, määrake see arv null.
5. Klõpsake nuppu **Salvesta**.

## <a name="archive-the-activity-log-via-powershell"></a>Tegevuste Logi PowerShelli kaudu arhiivimine
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| Atribuut         | Nõutav | Kirjeldus                                                                                                                                                                                                                                                                                       |
|------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| StorageAccountId | Ei       | Ressursi ID salvestusruumi konto, kuhu salvestatakse logid.                                                                                                                                                                                                                        |
| Asukohad        | Jah      | Komaga eraldatud loend piirkondade jaoks, mille soovite koguda tegevuse sündmused. Loendi kõigi piirkondade [külastades selle lehe](https://azure.microsoft.com/en-us/regions) või [Azure haldamine REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx)abil saate vaadata. |
| RetentionInDays  | Jah      | Millised sündmused tuleks säilitada, vahemikus 1 – 2147483647 päevade arv. Väärtuseks null talletab logid lõputult (terve igavik).                                                                                                                                                                                             |
| Kategooriad       | Jah      | Sündmuse kategooriaid, mis tuleks komaga eraldatud loend. Võimalikud väärtused on kirjutamine, kustutus- ja toiming.                                                                                                                                                                                 |
## <a name="archive-the-activity-log-via-cli"></a>Tegevuste Logi kaudu CLI arhiivimine
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| Atribuut        | Nõutav | Kirjeldus                                                                                                                                                                                                                                                                                       |
|-----------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nimi            | Jah      | Logi profiili nimi.                                                                                                                                                                                                                                                                         |
| storageId       | Ei       | Ressursi ID salvestusruumi konto, kuhu salvestatakse logid.                                                                                                                                                                                                                        |
| asukohad       | Jah      | Komaga eraldatud loend piirkondade jaoks, mille soovite koguda tegevuse sündmused. Loendi kõigi piirkondade [külastades selle lehe](https://azure.microsoft.com/en-us/regions) või [Azure haldamine REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx)abil saate vaadata. |
| retentionInDays | Jah      | Millised sündmused tuleks säilitada, vahemikus 1 – 2147483647 päevade arv. Väärtuseks null talletada logid lõputult (terve igavik).                                                                                                                                                                                             |
| Kategooriad      | Jah      | Sündmuse kategooriad, mida tuleks komaga eraldatud loend. Võimalikud väärtused on kirjutamine, kustutus- ja toiming.                                                                                                                                                                                 |

## <a name="storage-schema-of-the-activity-log"></a>Salvestusruumi skeemi tegevuste Logi
Kui olete loonud arhiivimine, luuakse salvestusruumi container salvestusruumi konto niipea, kui tegevuste Logi sündmus. Plekid pakendi sees tehke sama vormingut üle tegevuse Log ja Diagnostikalogid. Need plekid struktuuri on:

> ülevaateid-funktsionaalseid-logid/name = vaikimisi/ResourceIdkasutamisel = / tellimuste / {Tellimuse ID} / y = {arvuline neljakohalise aastaarvu} / m = {kahekohalise arvuline kuu} / d = {kahekohalise arvuline päev} / h = {kahekohalise 24-tunnist kella hour}/m=00/PT1H.json

Näiteks võib olla bloobimälu nimi.

> Insights-Operational-Logs/Name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON

Iga PT1H.json bloobimälu sisaldab JSON bloobimälu bloobimälu URL-is määratud tunni jooksul toimunud sündmuste (nt h = 12). Käesoleva tunni jooksul sündmused on lisatud faili PT1H.json esinevad. Minute väärtus (m = 00) on alati 00, kuna tegevuse sündmused on jagatud üksikute plekid tunnis.

PT1H.json failis, salvestatakse iga sündmuse "kirjete" massiiv, järgides selles vormingus:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
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
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| Elemendi nime    | Kirjeldus                                                                                                    |
|-----------------|----------------------------------------------------------------------------------------------------------------|
| aeg            | Kui sündmus on loodud sündmuse vastava päringu töötlemise Azure'i teenus võttis ajatempli.    |
| ResourceIdkasutamisel      | Ressursi ID kannatavas ressursi.                                                                          |
| operationName   | Toimingu nimi.                                                                                         |
| kategooria        | Kategooria toimingu, nt. Kirjutage, lugemise, toiming.                                                               |
| resultType      | Tippige tulemuse, nt. Edu, tõrge, käivitamine                                                            |
| resultSignature | Sõltub ressursi tüüp.                                                                                  |
| durationMs      | Kestuse millisekundites toiming.                                                                      |
| callerIpAddress | IP-aadress on sooritatud toiming, UPN-i taotlust või SPN taotluste põhjal kättesaadavus kasutaja.         |
| correlationId   | Tavaliselt GUID string vormingus. Sama uber toimingu kuuluvad sündmused, mis on correlationId ühiskasutusse anda.         |
| identiteedi        | JSON bloobimälu autoriseerimine ja taotluste kirjeldavad.                                                             |
| luba   | Bloobimälu RBAC atribuutide sündmuse. Tavaliselt sisaldab "toiming", "roll" ja "ulatus" atribuudid.            |
| tase           | Sündmuse tase. Üks järgmistest väärtustest: "Kriitiline", "Tõrge", "Hoiatus", "Teatised" ja "Verbose" |
| asukoht        | Piirkonna kohta toimunud (või globaalne).                                                             |
| atribuudid      | Kogum `<Key, Value>` paari (st sõnastik), mis kirjeldab Sündmuse üksikasjade.                             |

> [AZURE.NOTE] Atribuudid ja nende atribuutide kasutamist võivad erineda sõltuvalt ressursi.

## <a name="next-steps"></a>Järgmised sammud
- [Laadige alla plekid analüüsi jaoks](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Voo tegevuse jaoturi sündmuste logi](monitoring-stream-activity-logs-event-hubs.md)
- [Lisateavet vt teemast tegevuse Log](monitoring-overview-activity-logs.md)
