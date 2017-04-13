<properties 
   pageTitle="Alustamine Lake andmeanalüüsi REST API abil | Microsoft Azure'i" 
   description="Andmeanalüüsi Lake toiminguid WebHDFS REST API-de abil" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Azure'i andmed Lake Andmekaevandustööriistade abil REST API-de kasutamise alustamine

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Saate teada, kuidas hallata andmete Lake Analyticsi kontod, tööde haldamine ja kataloogi WebHDFS REST API-d ja andmete Lake Analytics REST API-de abil. 

## <a name="prerequisites"></a>Eeltingimused

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

- Saate **luua rakenduse Azure Active Directory**. Saate Azure AD rakendus autentida Lake andmeanalüüsi rakenduse Azure AD. On erinevate lähenemisviisi autentimiseks Azure AD, mis on **lõppkasutaja autentimist** või **teenusest autentimist**. Juhised ja autentida Lisateavet leiate teemast [autentimiseks Lake andmeanalüüsi Azure Active Directory abil](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

- [cURL](http://curl.haxx.se/). Selles artiklis kasutab cURL demonstreerib REST API helistada Lake andmeanalüüsi konto suhtes.

## <a name="authenticate-with-azure-active-directory"></a>Azure Active Directory autentimiseks

On kaks võimalust Azure Active Directory autentimiseks.

### <a name="end-user-authentication-interactive"></a>Lõppkasutaja autentimine (interaktiivne)

Selle meetodi kasutamise rakenduse kasutajalt sisse logida ja kõik toimingud sooritatakse kasutaja kontekstis. 

Interaktiivne autentimise tehke järgmist.

1. Oma rakenduse kaudu ümber suunata kasutaja järgmine URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<REDIRECT-URI > peab olema kodeeritud URL-is kasutamiseks. Niisiis, https://localhost, kasutada `https%3A%2F%2Flocalhost`)

    Selleks, et selles õppetükis saate kohatäite väärtusi, URL-i kohal asendada ja kleepida veebibrauseri aadressiribale. Teid suunatakse ümber autentida abil oma Azure Logi sisse. Kui teil on edukalt sisse logida, kuvatakse vastus brauseri aadressiribale. Vastuse saab järgmises vormingus:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Jäädvustada autoriseerimine kood vastus. Selles õpetuses saate kopeerige veebibrauseri aadressiribalt autoriseerimine koodi ja edastama selle postituse taotluse Turbeloa lõpp-punkti, nagu allpool näidatud:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] Sel juhul on \<REDIRECT-URI > peate olema kodeeritud.

3. Vastus on JSON-objekti, mis sisaldab juurdepääsu sümboolse (nt `"access_token": "<ACCESS_TOKEN>"`) ja Värskenda luba (nt `"refresh_token": "<REFRESH_TOKEN>"`). Teie rakendus kasutab juurdepääsu luba kasutamisel Azure'i andmesalve Lake ja Värskenda luba saada teise juurdepääsu luba juurdepääsu sümboolse aegumisel.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Kui juurdepääsu luba on aegunud, saate taotleda uut juurdepääsu luba abil Värskenda luba, nagu allpool näidatud:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Interaktiivne kasutaja autentimise kohta leiate lisateavet teemast [meilivoo anda kood](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Teenusest autentimine (mitte-interaktiivne)

Selle meetodi kasutamise rakendus annab oma mandaadi toimingute tegemiseks. Selle, peate probleemi postituse koosolekukutse nagu allpool näidatud: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Selle päringu väljund sisaldab mõnda autoriseerimine luba (tähistatud `access-token` allpool väljund), mida teil läheb hiljem kõnede REST API-ga. Salvestage see autentimise luba tekstifaili; See on vaja selle artikli.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Selles artiklis kasutab funktsiooni **- interaktiivne** moodust. Mitte-interaktiivne (teenusest – kõned) kohta leiate lisateavet teemast [teenuse teenuse kõnedele mandaadi abil](https://msdn.microsoft.com/library/azure/dn645543.aspx).
## <a name="create-a-data-lake-analytics-account"></a>Andmeanalüüsi Lake konto loomine

Enne Lake andmeanalüüsi konto loomist peate looma on Azure ressursirühm ja Lake andmesalve konto.  Vaadake [Lake andmesalve konto](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

Järgmine käsk Curl näitab, kuidas luua konto:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Asendamine \< `REDACTED` \> autoriseerimine luba, kus \< `AzureSubscriptionID` \> oma tellimuse ID-ga \< `AzureResourceGroupName` \> olemasoleva Azure'i ressursirühma nimega ja \< `NewAzureDataLakeAnalyticsAccountName` \> andmete Lake Analytics konto uue nimega. Taotluse last selle käsu sisaldub **CreateDatalakeAnalyticsAccountRequest.json** fail, mille jaoks on esitatud selle `-d` ülaltoodud parameeter. Input.json faili sisu näeb välja järgmine:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Tellimuse loendi andmete Lake Analyticsi kontod

Järgmine käsk Curl näitab, kuidas loendis kontod tellimus:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Asendage \< `REDACTED` \> autoriseerimine luba, kus \< `AzureSubscriptionID` \> oma tellimuse ID-ga. Väljund on sarnane:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Andmeanalüüsi Lake konto kohta teabe saamine

Mõne konto teabe saamiseks kujutatakse Curl järgmine käsk:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Asendamine \< `REDACTED` \> autoriseerimine luba, kus \< `AzureSubscriptionID` \> oma tellimuse ID-ga \< `AzureResourceGroupName` \> olemasoleva Azure'i ressursirühma nimega ja \< `DataLakeAnalyticsAccountName` \> Lake Analytics andmete olemasoleva konto nimi. Väljund on sarnane:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Loendi andmete Lake poed Lake andmeanalüüsi konto

Järgmine käsk Curl näitab, kuidas loendi andmete Lake poed konto:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Asendamine \< `REDACTED` \> autoriseerimine luba, kus \< `AzureSubscriptionID` \> oma tellimuse ID-ga \< `AzureResourceGroupName` \> olemasoleva Azure'i ressursirühma nimega ja \< `DataLakeAnalyticsAccountName` \> Lake Analytics andmete olemasoleva konto nimi. Väljund on sarnane:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>Esitage U-SQL-tööde haldamine

Esitada U-SQL töö kujutatakse Curl järgmine käsk:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Asendage \< `REDACTED` \> autoriseerimine luba, kus \< `DataLakeAnalyticsAccountName` \> Lake Analytics andmete olemasoleva konto nimi. Taotluse last selle käsu sisaldub **SubmitADLAJob.json** fail, mille jaoks on esitatud selle `-d` ülaltoodud parameeter. Input.json faili sisu näeb välja järgmine:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

Väljund on sarnane:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>Loendi A-SQL-i tööde haldamine

Järgmine käsk Curl näitab, kuidas loendi A-SQL-tööde haldamine:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Asendage \< `REDACTED` \> koos autoriseerimine luba, ja \< `DataLakeAnalyticsAccountName` \> Lake Analytics andmete olemasoleva konto nimi. 


Väljund on sarnane:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Kataloogiüksuste hankimine

Järgmine käsk Curl näitab, kuidas andmebaaside toomine kataloogi:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

Väljund on sarnane:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Vt ka

- Keerukama päringu, leiate artiklist [analüüsi veebisaidi logid Azure'i Lake andmeanalüüsi abil](data-lake-analytics-analyze-weblogs.md).
- Alustamiseks U-SQL-i rakenduste arendamise, lugege teemat [arendada U-SQL-i skriptide abil andmete Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- A-SQL-is leiate teemast [Azure andmete Lake Analytics U-SQL-i keele kasutamise alustamine](data-lake-analytics-u-sql-get-started.md).
- Haldamise toiminguid, vt [haldamine Azure'i Lake andmeanalüüsi abil Azure portaali](data-lake-analytics-manage-use-portal.md).
- Andmeanalüüsi Lake ülevaate saamiseks vt [Azure'i andmeanalüüsi Lake ülevaade](data-lake-analytics-overview.md).
- Muude tööriistade abil sama õpetuse vaatamiseks klõpsake menüü lülitid lehe ülaosas.
- Logiteave diagnostika vt [juurdepääs diagnostika logid Azure'i Lake andmeanalüüsi jaoks](data-lake-analytics-diagnostic-logs.md)
