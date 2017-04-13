<properties 
   pageTitle="Azure'i andmeanalüüsi Lake diagnostikalogid kuvamise | Microsoft Azure'i" 
   description="Aru saada, kuidas häälestamine ja juurdepääs Azure'i andmed Lake Analyticsi diagnostikalogid " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="Blackmist" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="08/11/2016"
   ms.author="larryfr"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Juurdepääs diagnostikalogid Azure Lake andmeanalüüsi jaoks

Teavet lubamine diagnostikalogimine Lake andmeanalüüsi konto ja kuidas vaadata teie konto jaoks kogutud logid.

Ettevõtted, saate lubada Azure andmeanalüüsi Lake nimel koguda andmeid Accessi täiendamist diagnostikalogimine. Need logid teavet näiteks:

* Mis on andmete kasutajate loendit.
* Kui sageli pääseb andmed.
* Kui palju andmeid on salvestatud konto.

## <a name="prerequisites"></a>Eeltingimused

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
- **Luba Azure tellimuse** andmete Lake Analytics avaliku eelvaate. Vaadake [juhiseid](data-lake-analytics-get-started-portal.md#signup).
- **Azure'i andmeanalüüsi Lake konto**. Järgige veebisaidil [Alustamine Azure'i Lake andmeanalüüsi Azure'i portaalis](data-lake-analytics-get-started-portal.md).

## <a name="enable-logging"></a>Logimise lubamine

1. Uue [Azure portaali](https://portal.azure.com)sisse logida.

2. Avage konto Lake andmeanalüüsi ja Lake andmeanalüüsi konto tera, klõpsake nuppu **sätted**ja klõpsake **Diagnostikasätete**.

3. **Diagnostika** tera teha järgmisi muudatusi Diagnostikaandmete logisse kandmise konfigureerimine.

    ![Diagnostikalogimise lubamiseks] (./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Diagnostikalogide lubamine")

    * **Oleku** määramine **klõpsake** diagnostikalogimise lubamiseks.
    * Kui soovite, et poe/protsessi andmeid kahel viisil.
        * Valige **ekspordi sündmuse jaoturi** voo logiandmed Azure'i sündmuse jaoturiga. Kasutage seda suvandit, kui teil on järgneval töötlemine müügivõimaluste sissetulevate logid reaalajas analüüsimiseks. Kui valite selle suvandi, peate sisestama üksikasjad Azure'i sündmuse jaoturi, mida soovite kasutada.
        * Valige **ekspordi salvestusruumi konto** Azure Storage konto Logide salvestamine. Kasutage seda suvandit, kui soovite arhiivida andmed. Kui valite selle suvandi, peate sisestama Azure Storage konto Salvesta logid.
    * Määrake, kas soovite saada auditilogid või taotluse logid või mõlemad.
    * Määrake päevade, mille andmeid tuleb säilitada.
    * Klõpsake nuppu **Salvesta**.

Kui olete lubanud diagnostikasätete, saate vaadata vahekaardil **Diagnostikalogid** logid.

## <a name="view-logs"></a>Vaate logid

On kaks võimalust vaadata logiandmed Lake andmeanalüüsi konto jaoks.

* Andmeanalüüsi Lake konto sätted
* Azure Storage konto andmete talletuskoht

### <a name="using-the-data-lake-analytics-settings-view"></a>Andmete Lake Analytics sätted vaate kasutamine

1. Klõpsake oma andmeanalüüsi Lake konto **sätted** blade **Diagnostikalogid**.

    ![Vaate diagnostika logimine] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Vaate diagnostikalogid") 

2. **Diagnostikalogide** tera, peaksite nägema kategoriseeritud **Auditilogid** ja **Taotleda logid**logid.
    * Taotluse logid jäädvustada iga API taotlus Lake andmeanalüüsi kontol.
    * Auditilogide sarnanevad logid koosolekukutse, kuid üksikasjalikum jaotus Lake andmeanalüüsi kontol tehtud toimingute. Näiteks taotluse logid üles laadida API kõne võib kaasa tuua mitu "Lisa" toimingute auditilogide.

3. Klõpsake linki **alla laadida** alla laadida logid logikirjet.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Azure Storage konto kaudu, mis sisaldab logiandmed

1. Avage Azure Storage konto tera seostatud Lake andmeanalüüsi logimiseks, ja seejärel klõpsake plekid. Tera **bloobimälu teenus** on loetletud kaks ümbriste.

    ![Vaate diagnostika logimine] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Vaate diagnostikalogid")

    * Container **ülevaateid – logid-auditilogi** sisaldab auditilogide.
    * Container **ülevaateid – logid-taotlusi** sisaldab taotlus logid.

2. Nende ümbriste sees logid on salvestatud all järgmine struktuur.

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json
    
    > [AZURE.NOTE] Funktsiooni `##` tee kirjed sisaldavad aasta, kuu, päev ja tund on loodud Logi. Andmeanalüüsi Lake loob ühe faili iga tund, seega `m=` on alati väärtus `00`.

    Näiteks täielik tee on Auditilogi võib olla:
    
        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Samuti võib olla täielik tee taotlus Logi:
    
        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Log struktuur

Auditi- ja taotluse logid on JSON-vormingus. Selles jaotises pilk JSON struktuuri taotlus ja audit logid.

### <a name="request-logs"></a>Logide taotlemine

Siin on valimi kirje Logi taotlus JSON-vormingus. Iga bloobimälu on ühe juurkausta objekti **kirjete** Logi objektide massiivi sisaldava nimega.

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Log skeemi taotlemine

| Nimi            | Tüüp   | Kirjeldus                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| aeg            | String | Ajatempliga (UTC) Logi                                              |
| ResourceIdkasutamisel      | String | Toimingu ressursi ID paigutamine                            |
| kategooria        | String | Log kategooria. Näiteks **taotlused**.                                   |
| operationName   | String | Mis on sisse logitud toimingu nimi. Näiteks GetAggregatedJobHistory.              |
| resultType      | String | Selle toimingu, näiteks 200 olek.                                 |
| callerIpAddress | String | Taotluse kliendi IP-aadress                                |
| correlationId   | String | Logi ID-d. See väärtus saab kasutada kogumi seotud log kirjete rühmitamine |
| identiteedi        | Objekti | Identiteedi, mis on loodud Logi                                            |
| atribuudid      | JSON   | Leiate järgmisest jaotisest (taotluse log atribuudid skeemi) üksikasjad |

#### <a name="request-log-properties-schema"></a>Log atribuudid skeemi taotlemine

| Nimi                 | Tüüp   | Kirjeldus                                               |
|----------------------|--------|-----------------------------------------------------------|
| HttpMethod           | String | HTTP meetodit kasutada toimingut. Näiteks saate. |
| Tee                 | String | Selle toimingu tee on läbi                   |
| RequestContentLength | int    | HTTP-päringu sisu pikkus                    |
| ClientRequestId      | String | Id, mis tuvastab kordumatult taotlus              |
| StartTime            | String | Kellaaeg, millal server taotluse vastu         |
| EndTime              | String | Aeg, mille server vastuse saatmata              |

### <a name="audit-logs"></a>Auditilogide

Siin on näide kirje auditilogi JSON-vormingus. Iga bloobimälu on üks juurkausta objekti nimega **kirjete** Logi objektide massiivi sisaldava

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job", 
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Auditilogi log skeem

| Nimi            | Tüüp   | Kirjeldus                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| aeg            | String | Ajatempliga (UTC) Logi                                              |
| ResourceIdkasutamisel      | String | Toimingu ressursi ID paigutamine                            |
| kategooria        | String | Log kategooria. Näiteks **Audit**.                                      |
| operationName   | String | Mis on sisse logitud toimingu nimi. Näiteks JobSubmitted.              |
| resultType | String | Mõne substatus töö oleku (operationName). |
| resultSignature | String | Täiendavad üksikasjad töö olek (operationName). |
| identiteedi      | String | Kasutaja soovitud toiming. Näiteks susan@contoso.com.                                 |
| atribuudid      | JSON   | Leiate järgmisest jaotisest (auditilogi log atribuudid skeemi) üksikasjad |

> [AZURE.NOTE] __resultType__ __resultSignature__ teavet toimingu tulem ja ainult on väärtus, kui toiming on lõpule viidud. Näiteks neis väärtus __operationName__ sisaldab väärtust __JobStarted__ või __JobEnded__.

#### <a name="audit-log-properties-schema"></a>Auditilogi log atribuudid skeem

| Nimi       | Tüüp   | Kirjeldus                              |
|------------|--------|------------------------------------------|
| JobId | String | Projektile ID.  |
| JobName | String | Nimi, mille jaoks töö on esitatud |
| JobRunTime | String | Töö kasutada runtime |
| SubmitTime | String | Aeg (UTC) töö võimalik |
| StartTime | String | Töö alustamine töötab pärast (UTC) esitamise ajal. |
| EndTime | String | Aega töö lõ. |
| Paralleelsus | String | Nõutav selle töö esitamise ajal Lake andmeanalüüsi ühikute arv. |

> [AZURE.NOTE] __SubmitTime__, __StartTime__, __EndTime__ ja __paralleelsus__ tegevuse kohta teavet ja ainult on väärtus, kui toiming on võtnud või lõpetatud. Näiteks __SubmitTime__ sisaldab väärtust pärast __operationName__ näitab __JobSubmitted__.

## <a name="process-the-log-data"></a>Logi andmete töötlemiseks

Azure'i Lake andmeanalüüsi annab Logi andmete töötlemiseks ja valimi. Valimi leiate [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 


## <a name="next-steps"></a>Järgmised sammud

- [Azure'i andmeanalüüsi Lake ülevaade](data-lake-analytics-overview.md)


