<properties
    pageTitle="Mobiilse kaasamine ekspordi API ülevaade"
    description="Tutvuge eksportimise oma toorandmetega loodud oma kasutaja seadmetes koosolekuteabe puhul kasutada oma tööriistad"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="kpiteira"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile"
    ms.date="04/26/2016"
    ms.author="kpiteira"/>

# <a name="mobile-engagement-export-api-overview"></a>Mobiilse kaasamine ekspordi API ülevaade

## <a name="introduction"></a>Sissejuhatus

Seda dokumenti saate teada, teie loodud oma kasutaja seadmetes koosolekuteabe puhul kasutada oma tööriistad toorandmetega eksportimise kohta leiate altpoolt.

## <a name="pre-requisites"></a>Eeltingimused

Töötlemata andmete eksportimine Mobile kaasamine nõuab tehke järgmist.

- API autentimise häälestamise saama kasutada API-d (vt [autentimise käsitsi häälestus](mobile-engagement-api-authentication-manual.md)),
- Kasutage REST API-d või [.net SDK](mobile-engagement-dotnet-sdk-service-api.md)
- Azure Storage konto.

>[AZURE.NOTE] Ka soovitame suurepäraseid [Microsoft Azure'i salvestusruumi Exploreri](http://storageexplorer.com/)vähemalt ajal nagu pakub mõne lihtsa Kasutajaliidese Azure Storage suhtlemiseks.

## <a name="what-can-be-exported"></a>Mida saab eksportida?

Mobile võimaldab selle kasutajatel mitut tüüpi andmete kogumise ja seetõttu on mitut tüüpi neid erinevaid andmetüüpe, mis vastab ekspordi.
On 2 olulised tüüpi ekspordi.

- Hetktõmmis: kasutatakse tavaliselt andmete eksportimine mis tähistab riigi ja mille Mobile kaasamine jälgida jõudlusandmete ajalugu. See hõlmab näiteks sõned või tõuketeatised turunduskampaania tagasiside silte (rakenduse-teave). Seetõttu on seotud järgmist tüüpi ekspordi kuupäeva.
- ajalooliste: seda tüüpi ekspordi kasutatakse liidetakse aja jooksul, nt sündmused või tegevuste näiteks andmeid.

Järgmises tabelis kirjeldatakse ammendavalt kõigi võimalike ekspordiks.

| Ekspordi tüüp | Andmetüüp | Kirjeldus                                                                                                                                 |
|-------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Hetktõmmis    | Tõuketeatised      | Loob ekspordi Push kampaaniat tagasiside eest deviceid/kasutajanimi alusel.                                                              |
| Hetktõmmis    | Sildi       | Loob ekspordi siltide (rakenduse teave) seotud iga seadmed                                                                       |
| Hetktõmmis    | Seadme    | Loob ekspordi enamik andmeid seadmed, näiteks technicals (mudel, lokaadi, ajavöönd,...), siltide, näinud esimest korda... |
| Hetktõmmis    | Turbeloa     | Loob ekspordi kõik lubatud märgid.                                                                                                 |
| Ajalooliste  | Tegevus  | Loob ekspordi kõik tegevuse iga seadmete jaoks teatud aja jooksul                                                           |
| Ajalooliste  | Sündmuse     | Loob ekspordi kõik tegevuse iga seadmete jaoks teatud aja jooksul                                                           |
| Ajalooliste  | Töö       | Loob ekspordi kõigi tööde iga seadmete jaoks teatud aja jooksul                                                                 |
| Ajalooliste  | Tõrge     | Loob ekspordi kõik vead iga seadmete jaoks teatud aja jooksul                                                               |

## <a name="how-does-it-work"></a>Kuidas see toimib?

Eksport on pikk töötab tööülesandeid, mida võib põhjustada suure andmefailid. Seetõttu ei saa neid kasutada tagastamiseks kohe faili alla laadida.
Selleks, et andmete eksportimine Mobile kaasamine on teil luua **Eksportimine töö** kaudu API kus saate määrata üldiselt:

- Ekspordi (kuvatõmmis või ajalooliste) tüüp
- Andmetüüp,
- **Azure'i salvestusruumi Container** (sh kehtiv SAS koos kirjutamisõigused) kui ekspordi tulemus on kirjutatud.

Pange tähele, võib kuluda paar minutit oma tööd alustada, ja seejärel seda võib Käivita mõne hetke väike rakenduste kaudu rakenduste palju kasutajate või mitu tundi.

Kui töö on loodud, on võimalik selle oleku kuvamiseks, kui see töötab endiselt või kui see on lõppenud.

Pärast töö õnnestus, tulemuseks andmete fail on saadaval oleval esitatud salvestusruumi.
