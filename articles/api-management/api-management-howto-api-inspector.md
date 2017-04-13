<properties 
    pageTitle="Jälgida inspektor API kasutamine kutsub Azure'i API haldus" 
    description="Saate teada, kuidas jälitada kõnesid API inspektor kasutamine Azure'i API haldus." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>Jälgida inspektor API kasutamine kutsub Azure'i API haldus

API haldus pakub mõne API inspektor tööriista, mis aitab silumine, ja tõrkeotsingu oma API-d. Inspektor API saab kasutada programmiliselt ja saab kasutada ka otse portaalis arendaja. 

Lisaks toimingute jälitamine API inspektor jälgi [poliitika avaldise](https://msdn.microsoft.com/library/azure/dn910913.aspx) hindamisel. Tutvustamise, lugege teemat [Cloud kataks episood 177: rohkem API haldusfunktsioonid](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ja edasikerimine 21.00.

Sellest juhendist leiate walk-through, kasutades API inspektor.

>[AZURE.NOTE] API inspektor jälgi ainult on loodud ja tehtud taotlusi sisaldav tellimus klahvid, mis kuuluvad [administraatori](api-management-howto-create-groups.md) konto jaoks saadaval.

## <a name="trace-call"></a> Kasutamine API inspektor jälgida kõne

API inspektor kasutamiseks lisada mõne **ocp-apim-Jälita: true** taotleda päise toiming kõne, et alla laadida ja kontrolli URL-i tähisega **ocp apim Jälita asukoha** vastuse päises jälitus. Seda saab teha programatically ja saab teha ka otse portaalis arendaja.

Selle õpetuse näitab, kuidas kasutada API inspektor Jälita toimingute abil lihtsa kalkulaator API, mis on konfigureeritud [haldamine oma esimese API](api-management-get-started.md) saamisega alustamine õpetuses. Kui te pole veel täitnud selle õpetuse kulub vaid mõne hetke aega tavaline kalkulaator API importida või kasutada teise API enda valitud Meilikausta nagu kaja API. Iga eksemplari API haldamise tuleb eelnevalt konfigureeritud eksperimenteerida ja õppida API halduse kasutatavate kaja rakendusliidest. Kaja API tagastab tagasi ükskõik sisendit saadetakse. Seda kasutada HTTP-Verbi kutsute ja tagastatav väärtus on lihtsalt, mis saatsite. 



Alustamiseks klõpsake **arendaja portaali** API halduse teenust Azure klassikaline portaalis. Toimingute saab kutsuda otse arendaja portaali, mis pakub mugavam ülevaatus ja testimine API toimingud.

>Kui te pole veel loonud API halduse teenuse eksemplar, teemast [Azure API kasutamist alustada][] õpetuses [loomine API halduse teenuse eksemplar][] .

![API haldusportaali arendaja][api-management-developer-portal-menu]

Klõpsake **API-de** ülalt menüüst ja seejärel nuppu **Tavaline kalkulaator**.

![Kaja API][api-management-api]

Klõpsake **proovima** **lisamine kahe täisarvude** toimingut.

![Proovige järele][api-management-open-console]

Säilita vaikimisi parameetrite väärtused ja valige **märkimise** ripploendis soovitud tellimuse tootenumber.

Arendaja portaalis **Ocp-Apim-Jälita** päis on juba vaikimisi väärtuseks **true**. See päis konfigureerib jälitusteave, mis on loodud või mitte.

![Saatmine][api-management-http-get]

Klõpsake nuppu **saada** autonoomsest toiming.

![Saatmine][api-management-send-results]

Vastuse saab päised **ocp apim Jälita asukoha** väärtusega, mis on sarnane järgmises näites.

    ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742

Jälitus saate alla laadida määratud asukohta ja nagu on näidatud järgmises toimingus üle vaadata.

## <a name="inspect-trace"> </a>Uurimine jälitus

Üle vaadata jälitus väärtused, Jälita faili alla laadida **ocp apim Jälita asukoha** URL. See on tekstifaili JSON-vormingus ja sisaldab kirjeid, mis on sarnane järgmises näites.

    {
        "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
        "traceEntries": {
            "inbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0725926",
                    "data": {
                        "request": {
                            "method": "GET",
                            "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "Connection",
                                    "value": "Keep-Alive"
                                },
                                {
                                    "name": "Host",
                                    "value": "contoso5.azure-api.net"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "mapper",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0726213",
                    "data": {
                        "configuration": {
                            "api": {
                                "from": "/calc",
                                "to": {
                                    "scheme": "http",
                                    "host": "calcapi.cloudapp.net",
                                    "port": 80,
                                    "path": "/api",
                                    "queryString": "",
                                    "query": {},
                                    "isDefaultPort": true
                                }
                            },
                            "operation": {
                                "method": "GET",
                                "uriTemplate": "/add?a={a}&b={b}"
                            },
                            "user": {
                                "id": 1,
                                "groups": [
                                    "Administrators",
                                    "Developers"
                                ]
                            },
                            "product": {
                                "id": 1
                            }
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0727522",
                    "data": {
                        "message": "Request is being forwarded to the backend service.",
                        "request": {
                            "method": "GET",
                            "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "X-Forwarded-For",
                                    "value": "33.52.215.35"
                                }
                            ]
                        }
                    }
                }
            ],
            "outbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1960601",
                    "data": {
                        "response": {
                            "status": {
                                "code": 200,
                                "reason": "OK"
                            },
                            "headers": [
                                {
                                    "name": "Pragma",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Length",
                                    "value": "124"
                                },
                                {
                                    "name": "Cache-Control",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Type",
                                    "value": "application/xml; charset=utf-8"
                                },
                                {
                                    "name": "Date",
                                    "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                                },
                                {
                                    "name": "Expires",
                                    "value": "-1"
                                },
                                {
                                    "name": "Server",
                                    "value": "Microsoft-IIS/8.5"
                                },
                                {
                                    "name": "X-AspNet-Version",
                                    "value": "4.0.30319"
                                },
                                {
                                    "name": "X-Powered-By",
                                    "value": "ASP.NET"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1961112",
                    "data": {
                        "message": "Response headers have been sent to the caller. Starting to stream the response body."
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1963155",
                    "data": {
                        "message": "Response body streaming to the caller is complete."
                    }
                }
            ]
        }
    }

## <a name="next-steps"> </a>Järgmised sammud

-   Vaadake demo jälitamine poliitika avaldiste [Cloud kataks episood 177: rohkem API haldusfunktsioonid](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Edasikerimine 21.00 demo kuvamiseks.

>[AZURE.VIDEO episode-177-more-api-management-features-with-vlad-vinogradsky]

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Azure'i API kasutamist alustada]: api-management-get-started.md
[API halduse teenuse eksemplari loomine]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




 