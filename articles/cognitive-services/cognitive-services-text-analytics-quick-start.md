<properties
    pageTitle="Lühijuhend: seadme õ teksti Analytics API-de | Microsoft Azure'i"
    description="Azure'i masina Õppekeskuse teksti analüüsi - Lühijuhend"
    services="cognitive-services"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>

# <a name="getting-started-with-the-text-analytics-apis-to-detect-sentiment-key-phrases-topics-and-language"></a>Alustamine: teksti Analytics API tuvastamiseks meeleolu, fraase, teemade ja keel

<a name="HOLTop"></a>

Dokumendis kirjeldatakse, kuidas endal teie teenuse või rakenduse kasutamine [Teksti Analytics API -d](//go.microsoft.com/fwlink/?LinkID=759711).
Nende API-de abil saate tuvastada teie tekstist meeleolu, fraase, teemade ja keel. [Klõpsake siin on interaktiivne demo kogemus.](//go.microsoft.com/fwlink/?LinkID=759712)

Vaadake [API määratlused](//go.microsoft.com/fwlink/?LinkID=759346) tehnilise dokumentatsiooni selle API-de jaoks.

Sellest juhendist leiavad versiooni 2 on API-de jaoks. Lisateavet API-d, [vaadake selle dokumendi](../machine-learning/machine-learning-apps-text-analytics.md)versiooni 1.

Õppeteema lõpus on võimalik programatically tuvasta:

- **Meeleolu** - tekst on positiivne või negatiivne?

- **Klahv fraasid** – Millised inimesed arutada artiklis?

- **Teemade** – Millised inimesed arutada üle palju artikleid?

- **Keele** - keeles on tekst kirjutatud?

Pange tähele, et see API kulude 1 tehingu kohta edastatud dokument. Näiteks, kui soovite meeleolu 1000 dokumentide ühe telefonil 1000 tehingud maha.



<a name="Overview"></a>
## <a name="general-overview"></a>Ülevaade ##

See dokument on samm-sammult juhendi. Meie eesmärk on sõelub koolitada mudeli ja osutage ressursse, mis võimaldab teil panna see valmistamisel tarvilikud toimingud. See ülesanne võtab aega umbes 30 minutit.

Nende toimingute jaoks on vaja toimetaja ja kõne rahulik lõpp-punktid teie valitud keeles.

Alustagem!

## <a name="task-1---signing-up-for-the-text-analytics-apis"></a>Ülesanne 1 - allkirjastamiseks üles teksti Analytics API-d ####

Selles ülesandes kas registreeru teksti Kasutusanalüüsi teenus.

1. Liikuge **Kognitiivse teenuste** [Azure portaali](//go.microsoft.com/fwlink/?LinkId=761108) ja veenduge, et **Tekst Analytics** on valitud soovitud API tüüp.

1. Valige leping. Võite valida **tasuta taseme 5000 tehingud kuus**. On tasuta leping, mida ei tuleb tasuda teenuse kasutamise kohta. Peate Azure tellimuse sisse logida. 

1. Täitke muud väljad ja looge oma konto.

1. Pärast registreerumist teksti Analytics, otsige oma **API võti**. Kopeerige primaarvõtme, peate selle API teenuste kasutamisel.


## <a name="task-2---detect-sentiment-key-phrases-and-languages"></a>Ülesanne 2 - avastada meeleolu, fraase ja keeled ####

See on lihtne teksti tuvastamine meeleolu, fraase ja keeled. Programatically saate sama tulemuse nagu [demo kogemus](//go.microsoft.com/fwlink/?LinkID=759712) annab.

>[AZURE.TIP] Meeleolu analüüsi, soovitame teksti jagatud lausete. See viib üldiselt suurema täpsusega meeleolu prognoose.

Pange tähele, et toetatud keeltes on järgmised:

| Funktsioon | Toetatud keelte tähised |
|:-----|:----|
| Meeleolu | `en`(Inglise keeles), `es` (Hispaania), `fr` (prantsuse keel), `pt` (Portugali) |
| Fraase | `en`(Inglise keeles), `es` (Hispaania), `de` (saksa) `ja` (Jaapani keel) |


1. Peate päised seadmiseks järgmist. Pange tähele, et JSON on praegu ainult aktsepteeritud sisestamise Vorming soovitud API-de jaoks. XML-i ei toetata.

        Ocp-Apim-Subscription-Key: <your API key>
        Content-Type: application/json
        Accept: application/json

1. Järgmiseks vormindada oma JSON Sisestuskeel read. Meeleolu, fraase ja keel, vorming on sama. Pange tähele, et iga ID peavad olema kordumatud ja ID tagastatakse süsteem. Ühe dokumendi, mida saab esitada maksimaalne maht on 10KB ja esitatud sisendit kokku maksimaalne maht on 1MB. Rohkem kui 1000 dokumentide esitamise ühe kõne. Rate piiramine on olemas, mis moodustab 100 kõned minutis – Seetõttu soovitame teil esitada suure hulga dokumentide ühe kõne. Keel on valikuline parameeter, mis peaks olema määratud kui mitte-ingliskeelsed teksti analüüsimine. Sisestusmeetodi näide kuvatakse allpool, kus valikuline parameeter `language` meeleolu analüüsi või klahv fraasi eraldamine on kaasatud:

        {
            "documents": [
                {
                    "language": "en",
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "language": "en",
                    "id": "100",
                    "text": "Final document"
                }
            ]
        }

1. Helistamine **postituse** süsteemi sisendi meeleolu, fraase ja keel. URL-ide näeb välja järgmine:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages

1. Selle kõne tulemuseks on JSON vormindatud vastus koos ID-d ja tuvastatud atribuudid. Väljundi meeleolu näide kuvatakse allpool (koos tõrke üksikasjade välja jätta). Puhul tunne, tagastatakse 0 ja 1 vahel on Keskmine iga dokumendi:

        // Sentiment response
        {
            "documents": [
                {
                    "id": "1",
                    "score": "0.934"
                },
                ...
                {
                    "id": "100",
                    "score": "0.002"
                },
            ]
        }

        // Key phrases response
        {
            "documents": [
                {
                    "id": "1",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
                ...
                {
                    "id": "100",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
            ]
        }

        // Languages response
        {
            "documents": [
                {
                    "id": "1",
                    "detectedLanguages": [
                        {
                            "name": "English",
                            "iso6391Name": "en",
                            "score": "1"
                        }
                    ]
                },
                ...
                {
                    "id": "100",
                    "detectedLanguages": [
                        {
                            "name": "French",
                            "iso6391Name": "fr",
                            "score": "0.985"
                        }
                    ]
                }
            ]
        }


## <a name="task-3---detect-topics-in-a-corpus-of-text"></a>Ülesanne 3 – teemade lisamine kogumina teksti tuvastamine ####

See on äsja avaldatud API, mis tagastab ülaosas tuvastatud teemade loendit esitatud teksti kirjed. Teema on tähistatud võtme fraas, mis võib olla üks või rohkem seotud sõnu. API on mõeldud töötavad hästi lühikesed ja inimeste kirjutatud teksti, nt kommentaare ja kasutajale tagasiside.

See API nõuab **minimaalselt 100 teksti kirjed** tuleb esitada, kuid on mõeldud tuvastamiseks teemade üle sadu tuhandetele kirjed. Mis tahes mitte-ingliskeelsed või kirjete alla 3 sõnad hüljatakse ja seetõttu ei määrata teemade. Teema: avastamine, ühte dokumenti, mida saab esitada maksimaalne maht on 30KB ja edastatud sisendi kokku maksimaalne maht on 30MB. Teema tuvastamise on piiratud 5 edastuste iga 5 minuti järel.

On kaks täiendavad **Valikuline** sisendparameetrid, mis aitab tulemuste täiustamiseks.

- **Peatage sõnu.**  Neid sõnu ja nende Sule vormide (nt mitmusvormid) välistatud teemas tuvastamise kohaletoimetamisel. Kasuta seda levinud sõnu (näiteks "küsimus", "tõrge" ja "kasutaja" võib olla vastavad Valikud klientide kaebuste tarkvara). Iga stringi peaks olema üksiku sõna.
- **Lõpeta laused** - neist fraasidest välistatakse tagastatud teemade loendit. Kasutage seda välistamine üldise teemad, mida te ei soovi tulemuste kuvamiseks. Näiteks "Microsoft" ja "Azure" oleks teemade välistamiseks vastavad Valikud. Stringide võib sisaldada mitut sõna.

Järgmiste juhiste abil tuvastada teemade teksti.

1. Vormindage JSON sisendi. Seekord Peata sõnade ja fraaside peatada.

        {
            "documents": [
                {
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "id": "100",
                    "text": "Final document"
                }
            ],
            "stopWords": [
                "issue", "error", "user"
            ],
            "stopPhrases": [
                "Microsoft", "Azure"
            ]
        }

1. Ülesanne 2 määratletud, kasutades sama päised, teha **postituse** kõne teemade lõpp-punkti:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/topics

1. See tagastab soovitud `operation-location` nimega vastus, mille väärtus on tulemuseks teemade päringu URL-i päise:

        'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

1. Päringu tagastatud `operation-location` perioodiliselt koos **GET** -päring. Soovitatav on üks kord minutis.

        GET https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>

1. Lõpp-punkti tagastavad vastust, kaasa arvatud `{"status": "notstarted"}` enne töötlemist `{"status": "running"}` töötlemise ajal ja `{"status": "succeeded"}` lõpuleviidud väljundit. Seejärel saate kasutada mobiilirakendustes väljundi, mis on järgmises vormingus (Märkus üksikasjad nagu selles näites on välja tõrke vorming ja kuupäevad):

        {
            "status": "succeeded",
            "operationProcessingResult": {
                "topics": [
                    {
                        "id": "8b89dd7e-de2b-4a48-94c0-8e7844265196"
                        "score": "5"
                        "keyPhrase": "first topic name"
                    },
                    ...
                    {
                        "id": "359ed9cb-f793-4168-9cde-cd63d24e0d6d"
                        "score": "3"
                        "keyPhrase": "final topic name"
                    }
                ],
                "topicAssignments": [
                    {
                        "topicId": "8b89dd7e-de2b-4a48-94c0-8e7844265196",
                        "documentId": "1",
                        "distance": "0.354"
                    },
                    ...
                    {
                        "topicId": "359ed9cb-f793-4168-9cde-cd63d24e0d6d",
                        "documentId": "55",
                        "distance": "0.758"
                    },            
                ]
            }
        }

Pange tähele, et eduka vastus teemade kaudu soovitud `operations` lõpp-punkti on järgmine skeem:

    {
            "topics" : [{
                "id" : "string",
                "score" : "number",
                "keyPhrase" : "string"
            }],
            "topicAssignments" : [{
                "documentId" : "string",
                "topicId" : "string",
                "distance" : "number"
            }],
            "errors" : [{
                "id" : "string",
                "message" : "string"
            }]
        }

Iga osa sellest vastuse selgitused on järgmised:

**teemade**

| Klahv | Kirjeldus |
|:-----|:----|
| ID | Iga teema ainuidentifikaator. |
| Keskmine | Teema määratud dokumentide arv. |
| keyPhrase | Koondväljavõtteid sõna või fraasi teema. |

**topicAssignments**

| Klahv | Kirjeldus |
|:-----|:----|
| documentId | Dokumendi ID. Võrdub kaasatud sisend ID-d. |
| topicId | Mis on määratud dokumendi ID teema. |
| kauguse | Dokumendi-teema liitumine Keskmine vahemikus 0 kuni 1. Väiksem kaugus Keskmine tugevam liitumine teema on. |

**tõrked**

| Klahv | Kirjeldus |
|:-----|:----|
| ID | Sisestage viga viitab dokumendi ainuidentifikaator. |
| sõnumi | Tõrketeadet ei kuvata. |

## <a name="next-steps"></a>Järgmised sammud ##

Palju õnne! Nüüd on lõpetatud, teksti analytics andmete kasutamine. Nüüd võite uurida, nt [Power BI](//powerbi.microsoft.com) tööriista kasutamine andmete visualiseerimiseks, samuti oma teadmiste teile teksti andmete reaalajas vaate automatiseerimine.

Näha, kuidas teksti Analytics võimalusi, nagu meeleolu, saate kasutada osana robot, robot Framework saidil näha [Emotsionaalne robot](http://docs.botframework.com/en-us/bot-intelligence/language/#example-emotional-bot) näide.
