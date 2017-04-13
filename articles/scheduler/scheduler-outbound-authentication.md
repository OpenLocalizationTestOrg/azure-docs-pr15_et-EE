<properties
 pageTitle="Ajasti väljaminevat autentimist."
 description="Ajasti väljaminevat autentimist."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/15/2016"
 ms.author="deli"/>

# <a name="scheduler-outbound-authentication"></a>Ajasti väljaminevat autentimist.

Ajasti töö võib tekkida vajadus kõne välja teenuseid, mis nõuab autentimist. Sellisel viisil nimega teenuse saate määrata, kui ajasti töö, pääsete juurde oma ressursse. Mõnda neist teenustest kaasata muude Azure teenused, Salesforce.com, Facebook ja turvaliste kohandatud veebisaitide.

## <a name="adding-and-removing-authentication"></a>Lisamine ja eemaldamine autentimine

Lisamise ajasti töö autentimine on lihtne – lisamine on JSON tütarelement `authentication` abil soovitud `request` elemendi loomisel või värskendamine tööd. Saladusi edasi Scheduler teenusega panna, paik või POSTITUSSE taotluse osana selle `authentication` objekti – kunagi ei kuvata tagastatud vastuseid. Vastuste, salajase teabe väärtuseks null või võib-olla avaliku luba, mis tähistab autenditud üksus.

Autentimise eemaldamiseks sellele või paik töö, milles on `authentication` objekti null. Ei kuvata tagasi vastuseks atribuutidest autentimist.

Praegu on ainus toetatud autentimistüüpidest on `ClientCertificate` mudeli (kasutamiseks SSL/TLS kliendi sertide), on `Basic` mudeli (autentimiseks lihtsa), ja `ActiveDirectoryOAuth` mudeli (autentimiseks Active Directory OAuthi.)

## <a name="request-body-for-clientcertificate-authentication"></a>Koosolekukutse kehasse ClientCertificate autentimine

Autentimist kasutades lisamisel kuvatakse `ClientCertificate` mudel, saate määrata järgmised täiendavad elemendid koosolekukutse kehasse.  

|Elemendi|Kirjeldus|
|:---|:---|
|_autentimine (emaelement)_|Autentimise objekti kasutamise kliendi SSL-sert.|
|_tüüp_|Nõutav. Autentimise tüüp. SSL-sertide klient, peab olema `ClientCertificate`.|
|_pfx_|Nõutav. Base64-kodeeringuga PFX-faili sisu.|
|_parooli_|Nõutav. Parooli PFX-fail.|


## <a name="response-body-for-clientcertificate-authentication"></a>Vastuse keha ClientCertificate autentimine

Taotluse saatmisel autentimise info vastus sisaldab järgmisi autentimine seotud elemente.

|Elemendi |Kirjeldus |
|:--|:--|
|_autentimine (emaelement)_ |Autentimise objekti kasutamise kliendi SSL-sert.|
|_tüüp_ |Autentimise tüüp. SSL-sertide klient, väärtus on `ClientCertificate`.|
|_certificateThumbprint_ |Sõrmejälje sertifikaadi.|
|_certificateSubjectName_ |Teema eristatav nimi serti.|
|_certificateExpiration_ |Serdi aegumiskuupäeva.|

## <a name="sample-rest-request-for-clientcertificate-authentication"></a>Valimi ülejäänud kutse ClientCertificate autentimine

```
PUT https://management.azure.com/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-clientcertificate-authentication"></a>Valimi ülejäänud vastuse ClientCertificate autentimine

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="request-body-for-basic-authentication"></a>Taotluse keha jaoks Lihttekstautentimine

Autentimist kasutades lisamisel kuvatakse `Basic` mudel, saate määrata järgmised täiendavad elemendid koosolekukutse kehasse.

|Elemendi|Kirjeldus|
|:--|:--|
|_autentimine (emaelement)_ |Autentimise objekt elementaarautentimine kasutamise kohta.|
|_tüüp_ |Nõutav. Autentimise tüüp. Elementaarautentimine, väärtus peab olema `Basic`.|
|_kasutajanimi_ |Nõutav. Kasutajanimi autentida.|
|_parooli_ |Nõutav. Parooli kinnitamiseks.|

## <a name="response-body-for-basic-authentication"></a>Vastuse keha jaoks Lihttekstautentimine

Taotluse saatmisel autentimise info vastus sisaldab järgmisi autentimine seotud elemente.

|Elemendi|Kirjeldus|
|:--|:--|
|_autentimine (emaelement)_ |Autentimise objekti elementaarautentimine kasutamise kohta.|
|_tüüp_ |Autentimise tüüp. Elementaarautentimine, väärtus on `Basic`.|
|_kasutajanimi_ |Autenditud kasutajanimi.|

## <a name="sample-rest-request-for-basic-authentication"></a>Valimi ülejäänud kutse Lihttekstautentimine

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-basic-authentication"></a>Valimi ülejäänud vastuse Lihttekstautentimine

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="request-body-for-activedirectoryoauth-authentication"></a>Koosolekukutse kehasse ActiveDirectoryOAuth autentimine

Autentimist kasutades lisamisel kuvatakse `ActiveDirectoryOAuth` mudel, saate määrata järgmised täiendavad elemendid koosolekukutse kehasse.

|Elemendi |Kirjeldus |
|:--|:--|
|_autentimine (emaelement)_ |Autentimise objekti ActiveDirectoryOAuth autentimist kasutades.|
|_tüüp_ |Nõutav. Autentimise tüüp. ActiveDirectoryOAuth autentimise väärtus peab olema `ActiveDirectoryOAuth`.|
|_rentniku_ |Nõutav. Rentniku identifikaator Azure AD rentniku jaoks.|
|_sihtrühma_ |Nõutav. See on seatud https://management.core.windows.net/.|
|_clientId_ |Nõutav. Sisestage kliendi identifikaatori Azure AD Rakenduse.|
|_salajane_ |Nõutav. Salajane kliendi, mis taotleb luba.|

### <a name="determining-your-tenant-identifier"></a>Oma rentniku identifikaator määratlemine

Leiate rentniku identifikaator Azure AD rentniku käivitades `Get-AzureAccount` Azure PowerShelli.

## <a name="response-body-for-activedirectoryoauth-authentication"></a>Vastuse keha ActiveDirectoryOAuth autentimine

Taotluse saatmisel autentimise info vastus sisaldab järgmisi autentimine seotud elemente.

|Elemendi |Kirjeldus |
|:--|:--|
|_autentimine (emaelement)_ |Autentimise objekti ActiveDirectoryOAuth autentimist kasutades.|
|_tüüp_ |Autentimise tüüp. ActiveDirectoryOAuth autentimise väärtus on `ActiveDirectoryOAuth`.|
|_rentniku_ |Rentniku identifikaator Azure AD rentniku jaoks. |
|_sihtrühma_ |See on seatud https://management.core.windows.net/.|
|_clientId_ |Kliendi identifikaator Azure AD Rakenduse.|

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a>Valimi ülejäänud kutse ActiveDirectoryOAuth autentimine

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a>Valimi ülejäänud vastuse ActiveDirectoryOAuth autentimine

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "tenant":"microsoft.onmicrosoft.com",
               "audience":"https://management.core.windows.net/",
               "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type":"ActiveDirectoryOAuth"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "lastExecutionTime":"2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime":"2016-03-16T19:11:00Z",
         "executionCount":5,
         "failureCount":5,
         "faultedCount":1
      }
   }
}
```

## <a name="see-also"></a>Vt ka


 [Mis on ajasti?](scheduler-intro.md)

 [Azure'i Scheduler põhimõtet, terminoloogia ja üksuse hierarhia](scheduler-concepts-terms.md)

 [Azure'i portaalis Scheduler kasutamise alustamine](scheduler-get-started-portal.md)

 [Lepingud ja arvelduse Azure'i ajasti](scheduler-plans-billing.md)

 [Azure'i Scheduler REST API viide](https://msdn.microsoft.com/library/mt629143)

 [Azure'i Scheduler PowerShelli cmdlet-käskude viitamine.](scheduler-powershell-reference.md)

 [Azure'i Scheduler kõrge-saadavus ja usaldusväärsus](scheduler-high-availability-reliability.md)

 [Azure'i Scheduler piirangud, vaikesätted ja kuvatavad tõrkekoodid](scheduler-limits-defaults-errors.md)
