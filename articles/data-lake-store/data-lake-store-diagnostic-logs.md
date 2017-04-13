<properties 
   pageTitle="Azure'i andmed Lake poe diagnostikalogid vaatamine | Microsoft Azure'i" 
   description="Aru saada, kuidas häälestamine ja juurdepääs Azure'i andmed Lake poe diagnostikalogid " 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Juurdepääs Azure'i andmed Lake poe diagnostikalogid

Teavet lubamine diagnostikalogimine Lake andmesalve konto ja kuidas vaadata teie konto jaoks kogutud logid.

Ettevõtted, saate lubada diagnostikalogimine Azure andmesalve Lake nimel koguda andmeid Accessi täiendamist leiate teavet, nt loendi kasutajad, kes kasutavad andmeid, kui sageli andmed on kättesaadav, kui palju andmeid on talletatud konto jne.

## <a name="prerequisites"></a>Eeltingimused

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

- **Azure'i andmesalve Lake konto**. Järgige veebisaidil [Alustamine Azure'i Lake andmesalve Azure'i portaalis](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>Luba diagnostikalogimine Lake andmesalve konto jaoks

1. Uue [Azure portaali](https://portal.azure.com)sisse logida.

2. Avage Lake andmesalve konto ja Lake andmesalve konto tera, klõpsake nuppu **sätted**ja klõpsake **Diagnostikasätete**.

3. **Diagnostika** tera teha järgmisi muudatusi Diagnostikaandmete logisse kandmise konfigureerimine.

    ![Diagnostikalogimise lubamiseks] (./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Diagnostikalogide lubamine")

    * **Oleku** määramine **klõpsake** diagnostikalogimise lubamiseks.
    * Kui soovite, et poe/protsessi andmeid kahel viisil.
        * Valige suvand eksportimine **sündmuse jaoturi** voo Logi andmetele Azure'i sündmuse jaoturiga. Tõenäoliselt kasutate see suvand, kui teil on järgneval töötlemine müügivõimaluste sissetulevate logid veebisaidil reaalajas analüüsimiseks. Kui valite selle suvandi, peate sisestama üksikasjad Azure'i sündmuse jaoturi, mida soovite kasutada.
        * Valige suvand eksportimine **salvestusruumi konto** Azure Storage konto Logide salvestamine. See suvand kasutada siis, kui soovite arhiivida andmed, mida saab paketi töödeldud hiljem. Kui valite selle suvandi peate sisestama Azure Storage konto Salvesta logid.
    * Määrake, kas soovite saada auditilogid või taotluse logid või mõlemad.
    * Määrake päevade, mille andmeid tuleb säilitada.
    * Klõpsake nuppu **Salvesta**.

Kui olete lubanud diagnostikasätete, saate vaadata vahekaardil **Diagnostikalogid** logid.

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>Vaate diagnostikalogid Lake andmesalve konto jaoks

On kaks võimalust vaadata logiandmed Lake andmesalve konto jaoks.

* Lake andmesalve konto sätete vaatamine
* Azure Storage konto andmete talletuskoht

### <a name="using-the-data-lake-store-settings-view"></a>Andmete Lake poe sätete vaate kasutamine

1. Klõpsake oma Lake andmesalve konto **sätted** blade **Diagnostikalogid**.

    ![Vaate diagnostika logimine] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Vaate diagnostikalogid") 

2. **Diagnostikalogide** tera, peaksite nägema kategoriseeritud **Auditilogid** ja **Taotleda logid**logid.
    * Taotluse logid jäädvustada iga API taotlus Lake andmesalve kontol.
    * Auditilogide sarnanevad logid koosolekukutse, kuid üksikasjalikum jaotus Lake andmesalve kontol tehtud toimingute. Näiteks taotluse logid üles laadida API kõne võib kaasa tuua mitu "Lisa" toimingute auditilogide.

3. Klõpsake linki **allalaadimine** vastu iga Logi kirje logid alla laadida.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Azure Storage konto kaudu, mis sisaldab logiandmed

1. Avage Azure Storage konto tera seostatud Lake andmesalve logimiseks, ja seejärel klõpsake plekid. Tera **bloobimälu teenus** on loetletud kaks ümbriste.

    ![Vaate diagnostika logimine] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Vaate diagnostikalogid")

    * Container **ülevaateid – logid-auditilogi** sisaldab auditilogide.
    * Container **ülevaateid – logid-taotlusi** sisaldab taotlus logid.

2. Nende ümbriste sees logid on salvestatud all järgmine struktuur.

    ![Vaate diagnostika logimine] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Vaate diagnostikalogid")

    Näiteks võib olla mõni auditilogi täielik tee`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`

    Similary, võib olla taotluse Logi täielik tee`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>Logi andmed struktuuri mõistmine

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
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
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
| operationName   | String | Mis on sisse logitud toimingu nimi. Näiteks getfilestatus.              |
| resultType      | String | Selle toimingu, näiteks 200 olek.                                 |
| callerIpAddress | String | Taotluse kliendi IP-aadress                                |
| correlationId   | String | ID-d log, mida saate kasutada rühmitada seotud kannete kogumi |
| identiteedi        | Objekti | Identiteedi, mis on loodud Logi                                            |
| atribuudid      | JSON   | Vaadake järgmist teavet                                                          |

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

Siin on näide kirje auditilogi JSON-vormingus. Iga bloobimälu on üks juur objekti nimega **kirjete** Logi objektide massiivi sisaldava

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
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
| operationName   | String | Mis on sisse logitud toimingu nimi. Näiteks getfilestatus.              |
| resultType      | String | Selle toimingu, näiteks 200 olek.                                 |
| correlationId   | String | ID-d log, mida saate kasutada rühmitada seotud kannete kogumi |
| identiteedi        | Objekti | Identiteedi, mis on loodud Logi                                            |
| atribuudid      | JSON   | Vaadake järgmist teavet                                                          |

#### <a name="audit-log-properties-schema"></a>Auditilogi log atribuudid skeem

| Nimi       | Tüüp   | Kirjeldus                              |
|------------|--------|------------------------------------------|
| StreamName | String | Selle toimingu tee on läbi  |


## <a name="samples-to-process-the-log-data"></a>Näidised Logi andmete töötlemiseks

Azure'i andmesalve Lake annab Logi andmete töötlemiseks ja valimi. Valimi leiate [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 


## <a name="see-also"></a>Vt ka

- [Azure'i andmesalve Lake ülevaade](data-lake-store-overview.md)
- [Turvaline andmete andmesalve Lake](data-lake-store-secure-data.md)

