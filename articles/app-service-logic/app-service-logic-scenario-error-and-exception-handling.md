<properties
    pageTitle="Logimise ja veakäsitluse loogika rakendustes | Microsoft Azure'i"
    description="Tegeliku elu kasutusmall täpsemalt tõrke töötlemise ja loogika rakendustega logimise kuvamine"
    keywords=""
    services="logic-apps"
    authors="hedidin"
    manager="anneta"
    editor=""
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="b-hoedid"/>

# <a name="logging-and-error-handling-in-logic-apps"></a>Logimise ja veakäsitluse loogika rakendustes

Selles artiklis kirjeldatakse, kuidas saate laiendada loogika rakenduse nende paremaks erandi töötlemine. See on tegeliku elu kasutamine juhtumi ja meie vastata küsimus: "Kas loogika rakendused toetab erand ja vigade käsitlemise?"

>[AZURE.NOTE]Funktsiooni loogika rakenduste Microsoft Azure'i rakenduse teenuse praegune versioon annab standard malli toimingu vastuseid.
>See hõlmab nii sisemise valideerimisreegli tõrge vastuste tagastatud API rakenduse kaudu.

## <a name="overview-of-the-use-case-and-scenario"></a>Kasutage juhul ja stsenaarium ülevaade

Järgmine lugu on käesoleva artikli puhul kasutada.
Tuntud tervishoiuteenuste korraldamise tööle meile Azure lahenduse, mis tekitaks patsientide portaalis, kasutades Microsoft Dynamics CRM Online'i. Nad on vaja saata kohtumise kirjet Dynamics CRM Online'i patsientide portaali ja Salesforce'i vahel.  Olid palutakse kasutada standardit [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) patsientide kõik kirjed.

Projekti oli kaks põhilist nõuded:  

 -  Meetodi kirjed, mis on saadetud Dynamics CRM Online'i portaali sisse logida
 -  Võimalus vaadata selle töövoo toimunud vigu


## <a name="how-we-solved-the-problem"></a>Kuidas me ei lahendanud probleemi

>[AZURE.TIP] Saate vaadata projekti ületaseme video [Integreerimine kasutaja rühma](http://www.integrationusergroup.com/do-logic-apps-support-error-handling/ "Integreerimine kasutaja rühma").

Valisime [Azure'i DocumentDB](https://azure.microsoft.com/services/documentdb/ "Azure'i DocumentDB") nagu hoidla log ja viga kirjete (DocumentDB viitab dokumentidena kirjeid). Kuna loogika rakendused on kõik vastused standard malli, me ei pea luua kohandatud skeem. Me ei saanud luua API rakenduse tõrge nii log kirjete **lisamine** ja **päringu** . Me ka määratleda skeemi iga API rakenduse kaudu.  

Teine nõue oli likvideerite kirjed pärast teatud kuupäeva. DocumentDB on [eluiga](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Time to Live") (TTL), mis võimaldas meil **eluiga** väärtus iga kirje või saidikogumi atribuudi. See kõrvaldada vajadust DocumentDB kirjete käsitsi kustutada.

### <a name="creation-of-the-logic-app"></a>Loogika rakenduse loomine

Esimene samm on loogika rakenduse loomine ja paigutada see designer. Selles näites me kasutame ema-tütre loogika rakendused. Oletame, et saaksime juba loonud ema ja luua ühe lapse loogika rakendus.

Kuna me olema logimine tulevad välja Dynamics CRM Online'i kirje, alustame ülaosas. Läheb vaja kasutada taotluse päästik, kuna ema loogika rakenduse käivitab see laps.

> [AZURE.IMPORTANT] Selle õpetuse tegemiseks peate luua andmebaasi DocumentDB ja kahe saidikogumid (logimise ja vead).

### <a name="logic-app-trigger"></a>Loogika rakenduse päästik

Kasutame taotluse päästik nagu on näidatud järgmises näites.

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


### <a name="steps"></a>Juhised

Peame logige Dynamics CRM Online'i portaalis patsientide kirje allikas (soovi korral).

1. Peame uue kohtumise kirje toomine Dynamics CRM Online'i.
    Käivitab pärit CRM annab meile selle **CRM PatentId**, **kirje tüüp**, **Uus või värskendatud kirje** (uus või värskendada kahendväärtus), ja **SalesforceId**. **SalesforceId** võib olla null, kuna seda kasutatakse ainult värskendust.
    Saame CRM-i kirje, kasutades CRM **PatientID** ja **Kirje tüüp**.
1. Järgmiseks tuleb lisada meie DocumentDB API rakenduse **InsertLogEntry** toiming, nagu on näidatud järgmises arvud.


#### <a name="insert-log-entry-designer-view"></a>Lisa Logi kirje päringukujundaja vaade

![Logi kirje lisamine](./media/app-service-logic-scenario-error-and-exception-handling/lognewpatient.png)

#### <a name="insert-error-entry-designer-view"></a>Tõrge kirje kujundaja vaate lisamine
![Logi kirje lisamine](./media/app-service-logic-scenario-error-and-exception-handling/insertlogentry.png)

#### <a name="check-for-create-record-failure"></a>Otsi loomine kirje tõrge

![Tingimus](./media/app-service-logic-scenario-error-and-exception-handling/condition.png)


## <a name="logic-app-source-code"></a>Loogika rakenduse lähtekoodi

>[AZURE.NOTE]  Järgmised näited ainult. Kuna selles õpetuses põhineb praegu valmistamisel rakendamist, ei kuvata **Allikas sõlme** väärtus atribuudid, mis on seotud Kohtumise plaanimine.

### <a name="logging"></a>Logimine
Loogika rakenduse koodi järgmises näites kujutatakse käsitlema logimine.

#### <a name="log-entry"></a>Logikirjet
See on loogika rakenduse lähtekoodi lisamine logikirjet.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>Log taotlus

See on postitatud API rakenduse Logi taotlus sõnum.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}"
        }
    }

```


#### <a name="log-response"></a>Log vastus

See on rakendusest API log vastuse sõnumi.

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "\"0400fc2f-0000-0000-0000-575b3ff00000\"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

Nüüd vaatame juhiseid tõrketöötluse.


### <a name="error-handling"></a>Tõrge töötlemine

Loogika rakenduste järgmine kood näidis näitab, kuidas rakendada tõrge töötlemine.

#### <a name="create-error-record"></a>Tõrge kirje loomine

See on tõrge-kirje loomise lähtekoodi loogika rakendused.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}          
```

#### <a name="insert-error-into-documentdb--request"></a>Tõrge lisamine rakendusse DocumentDB--taotlemine

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MasterID_mp__c\":\"\",\"C_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"ONY_ID__c\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-documentdb--response"></a>Sisestage viga DocumentDB--vastus


``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "\"0c00eaac-0000-0000-0000-575b3fdc0000\"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\":\"DO - Degree level is DO\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MterID_mp__c\":\"\",\"Medicis_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"XXXXXXX\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>Salesforce'i tõrge vastus

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="returning-the-response-back-to-the-parent-logic-app"></a>Esitus vastuse ema loogika rakendus

Kui teil on vastus, saab selle tagasi rakendusse ema loogika edastada.

#### <a name="return-success-response-to-the-parent-logic-app"></a>Tagastab edu vastuse ema loogika rakendus

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "   Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-the-parent-logic-app"></a>Tagastab tõrke vastuse ema loogika rakendus

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="documentdb-repository-and-portal"></a>DocumentDB hoidla ja portaal

Meie lahendus on lisatud täiendavaid võimalusi [DocumentDB](https://azure.microsoft.com/services/documentdb).

### <a name="error-management-portal"></a>Tõrge haldusportaal

Tõrgete kuvamiseks saate luua MVC web app näidata DocumentDB tõrge kirjeid. Praegune versioon kaasatakse toimingute **loend**, **üksikasjad**, **redigeerimine**ja **kustutamine** .

> [AZURE.NOTE]Toimingu redigeerimine: DocumentDB ei asenda kogu dokumendi.
> Kuvatakse **loend** ja **üksikasjad** vaadete kirjed on ainult näidised. Neid ei tegelik patsientide kohtumise kirjed.

Allpool on näited meie MVC rakenduse üksikasjade loodud eelnevalt kirjeldatud lähenemine.

#### <a name="error-management-list"></a>Tõrge halduse loend

![Tõrge loend](./media/app-service-logic-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>Tõrge halduse üksikasjade kuvamine

![Tõrke üksikasjade](./media/app-service-logic-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>Log haldusportaal

Logide vaatamiseks lõime MVC web app.  Allpool on näited meie MVC rakenduse üksikasjade loodud eelnevalt kirjeldatud lähenemine.

#### <a name="sample-log-detail-view"></a>Valimi log üksikasjade kuvamine

![Logi Detail View](./media/app-service-logic-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>API rakenduse üksikasjad

#### <a name="logic-apps-exception-management-api"></a>Loogika rakenduste erandi halduse API

Meie avatud lähtekoodi loogika rakenduste erandi halduse API rakenduse pakub järgmisi funktsioone.

On kaks kontrollerid.

- **ErrorController** lisab DocumentDB saidikogumi tõrge kirje (dokument).
- **LogController** Lisab DocumentDB kogumi telefonilogi kirje (dokument).

> [AZURE.TIP] Nii kontrollerid kasutada `async Task<dynamic>` toimingud. See võimaldab toimingute käitusajal, lahendada nii, et saame luua DocumentDB skeemi toimingu kehas.

Iga dokumendi DocumentDB peab olema kordumatu ID-ga. Kasutame `PatientId` ja lisada ajatempli, mis on teisendatud Unix ajatempli väärtus (kahekordne). Me kärpida selle eemaldamiseks murdarvuline väärtus.

Saate vaadata meie tõrge kontrolleril API [kaudu GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs)lähtekoodi.

Me kutsume API loogika rakenduse kaudu, kasutades järgmist süntaksit.

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

Eelmise proovi kood avaldise otsib *Create_NewPatientRecord* olek on **nurjunud**.

## <a name="summary"></a>Kokkuvõte

- Saate hõlpsalt rakendada logimine ja veakäsitluse loogika rakenduse.
- Saate kasutada DocumentDB hoidla log ja viga kirjete (dokumendid).
- MVC abil saate luua portaali logi ja viga kirjete kuvamiseks.

### <a name="source-code"></a>Lähtekoodi
Lähtekoodi loogika rakenduste erandi halduse API rakenduse jaoks on saadaval selle [GitHub hoidla](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Loogika rakenduse erandi haldamine API").


## <a name="next-steps"></a>Järgmised sammud
- [Saate vaadata veel näiteid loogika rakendused ja stsenaariumid](app-service-logic-examples-and-scenarios.md)
- [Lisateavet loogika rakenduste jälgimine tööriistad](app-service-logic-monitor-your-logic-apps.md)
- [Loogika rakenduse automatiseeritud juurutamise malli loomine](app-service-logic-create-deploy-template.md)
