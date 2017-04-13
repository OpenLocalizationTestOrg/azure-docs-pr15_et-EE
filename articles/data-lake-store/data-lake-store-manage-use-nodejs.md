<properties 
   pageTitle="Azure'i andmed Lake poed Node.js Azure'i SDK kasutamise alustamine | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada Node.js Lake andmesalve kontod ja failisüsteemi töötamiseks." 
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
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a>Azure'i Lake andmesalve Node.js Azure'i SDK kasutamise alustamine

> [AZURE.SELECTOR]
- [Portaal](data-lake-store-get-started-portal.md)
- [PowerShelli](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-GA](data-lake-store-get-started-rest-api.md)
- [Azure'i CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)


Saate teada, kuidas Azure'i SDK jaoks Node.js abil saate luua konto Azure andmesalve Lake ja põhiliste toimingute näiteks luua kaustu, üles- ja allalaadimine andmefailid, kustutada oma konto jne. Lake andmesalve kohta leiate lisateavet teemast [Andmesalve Lake ülevaade](data-lake-store-overview.md). SDK toetab praegu,

  *  **Node.js versioon: 0.10.0 või uuem versioon**
  *  **Konto REST API versioon: 2015-10-01-eelvaade**
  *  **Failisüsteemi REST API versioon: 2015-10-01-eelvaade**

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist selles artiklis, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

- Saate **luua rakenduse Azure Active Directory**. Saate Azure AD rakendus autentida Lake andmesalve rakenduse Azure AD. On erinevate lähenemisviisi autentimiseks Azure AD, mis on **lõppkasutaja autentimist** või **teenusest autentimist**. Juhised ja autentida Lisateavet leiate teemast [andmete Lake poe autentimise abil Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="how-to-install"></a>Kuidas installida

```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a>Azure Active Directory abil autentimine

Allpool Koodilõigud kuvada eraldi kahel viisil Lake andmesalve autentimist kasutades Azure AD. Üksikasjalik arutelu erineval Lake andmesalve autentimise jaoks, vaadake [autentimise andmete Lake poe kaudu Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

Allpool koodilõigu nõuab ka sisendina nagu Azure AD domeeni nimi, kliendi ID Azure AD Rakenduse jne. Kõiki neid andmeid saab tuua rakendusest Azure AD, peate loonud, mille üksikasjad sisalduvad ka linki.

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-store-clients"></a>Looge Lake andmesalve kliendid

```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a>Lake andmesalve konto loomine

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location
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

## <a name="create-a-file-with-content"></a>Faili sisu loomine
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var fileToCreate = '/myfolder/myfile.txt';
var options = {
  streamContents: new Buffer('some string content')
}

filesystemClient.fileSystem.listFileStatus(accountName, fileToCreate, options, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    // no result is returned, only a 201 response for success.
    console.log('response is: ' + util.inspect(response, {depth: null}));
  }
});
```

## <a name="get-a-list-of-files-and-folders"></a>Failide ja kaustade loendi hankimine

```javascript
var util = require('util');
var accountName = 'testadlsacct';
var pathToEnumerate = '/myfolder';
filesystemClient.fileSystem.listFileStatus(accountName, pathToEnumerate, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Vt ka

- [Microsoft Azure'i SDK Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure'i SDK Node.js - Lake Analytics andmehaldus](https://www.npmjs.com/package/azure-arm-datalake-analytics)
