<properties
   pageTitle="Azure'i Lake andmeanalüüsi Azure'i SDK kasutamise Node.js haldamine | Azure'i"
   description="Saate teada, kuidas andmeid Lake Analyticsi kontod, andmeallikate, töö ja Azure SDK kasutamise Node.js kasutajate haldamine"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-sdk-for-nodejs"></a>Azure'i Lake andmeanalüüsi Node.js Azure'i SDK kasutamise haldamine


[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure'i SDK Node.js jaoks saab kasutada Azure'i andmed Lake Analyticsi kontod, töö ja kataloogid haldamine. Muude tööriistade abil halduse teema kuvamiseks klõpsake menüü valimine ülaltoodud.

See toetab nüüd:

  *  **Node.js versioon: 0.10.0 või uuem versioon**
  *  **Konto REST API versioon: 2015-10-01-eelvaade**
  *  **Kataloogi REST API versioon: 2015-10-01-eelvaade**
  *  **Töö REST API versioon: 2016 03 – 20 eelvaade**

## <a name="features"></a>Funktsioonid

- Konto haldamine: loomine, saada, loendi, värskendamine ja kustutamine.
- Töö haldus: esitada, saamine, loendi, tühistamine.
- Kataloogi haldus: saamine, loendi loomine (saladused), värskendada (saladused), kustutada (saladused).

## <a name="how-to-install"></a>Kuidas installida

```bash
npm install azure-arm-datalake-analytics
```

## <a name="authenticate-using-azure-active-directory"></a>Azure Active Directory abil autentimine

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-analytics-client"></a>Andmeanalüüsi Lake kliendi loomine

```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## <a name="create-a-data-lake-analytics-account"></a>Andmeanalüüsi Lake konto loomine

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-jobs"></a>Töö loendi hankimine

```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-databases-in-the-data-lake-analytics-catalog"></a>Lake Analytics andmekataloogis andmebaaside loendi hankimine
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Vt ka

- [Microsoft Azure'i SDK Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure'i SDK Node.js - Lake andmesalve haldus](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)
