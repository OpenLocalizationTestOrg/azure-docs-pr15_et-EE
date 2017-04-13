
<properties
    pageTitle="Migreerida Azure kognitiivse teenuste soovituste API DataMarket soovitused API | Microsoft Azure'i"
    description="Azure'i masina Õppekeskuse soovitused--soovitused kognitiivse teenuse migreerimine"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="luisca"/>


# <a name="migrate-to-azure-cognitive-services-recommendations-api-from-the-datamarket-recommendations-api"></a>Azure'i kognitiivse teenuste soovituste API DataMarket soovitused API migratsioon
Selles artiklis kirjeldatakse, kuidas migreerimiseks [Microsoft DataMarket soovitused API](https://datamarket.azure.com/dataset/amla/recommendations) [Microsoft Azure'i kognitiivse teenuste soovituste API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api).

Datamarketi soovitused API katkestab vastu võtmist uutele klientidele 31 Detsember 2016 ja on aegunud 28 veebruar 2017.

## <a name="how-do-i-start-using-the-azure-cognitive-services-recommendations-api"></a>Kuidas Azure'i kognitiivse teenuste soovituste API kasutusele võtta?

Kognitiivne teenuste soovitused API migreerida, tehke järgmist.

1.  Kui teil pole veel Azure'i tellimus, [registreeruda](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) ühe. 

1.  Üksikasjalike juhiste saamiseks [Lühijuhend](cognitive-services-recommendations-quick-start.md) kasutajaks registreerumist kognitiivse teenuste soovitused API ja kasutage seda programmiliselt. 

1.  Avage soovitud [kognitiivse teenuste soovitused API kuvataks lehe](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) teada teenuse ja dokumentatsiooni.

## <a name="i-used-the-recommendations-ui-to-build-my-models-is-there-a-similar-tool-for-the-cognitive-services-recommendations-api"></a>Saab kasutada soovitused UI koostamiseks oma mudelite. On sarnane tööriist kognitiivse teenuste soovitused API?

Täiesti! [Soovitused UI](http://recommendations-portal.azurewebsites.net/) URL on http://recommendations-portal.azurewebsites.net. 

>[AZURE.NOTE] Teie DataMarket mandaadid ei tööta siin. Azure'i portaalis tellimuse kasutajaks, ja saada [Soovitusi Kasutajaliidese](http://recommendations-portal.azurewebsites.net/)kasutamiseks vajalik konto võti.
Kui teil on konto võti, vt [Lühijuhend](cognitive-services-recommendations-quick-start.md)tööülesande 1.

##<a name="is-the-new-api-format-the-same-as-the-datamarket-recommendations-api"></a>Uus API vorming on sama DataMarket soovitused API-ga?

API toetab sama stsenaariumi ja protsessi puhul kui need stsenaariumid DataMarket versioonis toetatud, kuid tegelik API kasutajaliidese uuendamist [Microsoft REST API juhised](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md)vastavaks. Funktsiooni API-d on ühtsema ja töö paremini koos tööriistad, et toetada ärplema.

Mõista iga soovitud API-d, tutvuge [API explorer](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3db).
Kasutage funktsiooni *proovige* seda nuppu testimiseks API kõne. Tarbitud soovitused API (kataloogi ja kasutus-failid) faili vorming on muutunud, nii, et saate edasi kasutada sama failide ja/või taristu on loodud, neid faile luua.

##<a name="what-are-some-new-features-in-the-cognitive-services-recommendations-api"></a>Mis on uute funktsioonide kognitiivse teenuste soovitused API?

Kahe kuu jooksul me välja uued võimalused kognitiivse teenuste soovitused API:
-   Soovitused UI koolitus ja testimine mudelite ilma üks rida koodi kirjutamiseks
-   Paketi teile soovituste tuhandete korraga hinded
-   Koostage päringusse soovitused mudelite kvaliteedi mõõdikute tugi
-   Business reeglid tugi
-   Võimalus loetlemine ja kasutus- ja kataloogi failide allalaadimine
-   Järjestus Koosta tugi päringu kvaliteedi üksuse funktsioonide mudeli soovitused
-   Lisatud võimalus otsida toote kataloogi

## <a name="when-does-microsoft-stop-supporting-the-datamarket-recommendations-api"></a>Kui Microsoft toetavad DataMarket soovitused API peatada?

[Soovitused API DataMarket](https://datamarket.azure.com/dataset/amla/recommendations) lõpetab uutele klientidele vastu pärast 31 Detsember 2016 ja 28 veebruar 2017 on täiesti aegunud. 

## <a name="what-if-i-dont-have-the-data-that-i-need-to-recreate-my-models-in-the-cognitive-services-recommendations-api"></a>Mida teha, kui mul pole mul on vaja uuesti luua oma mudelite kognitiivse teenuste soovitused API andmeid?

Soovime muuta see üleminek võimalikult lihtsaks. Aitame saate teisaldada oma vana mudelite oma DataMarket kontolt tellimuse uue Azure'i kognitiivse soovituste API. Võtke meiega ühendust [mlapi@microsoft.com](mailto://mlapi@microsoft.com). Palume teil esitada oma DataMarket Tellimuse ID ja kognitiivse teenuste Tellimuse ID enne me seostada mudelid uuele kontole.
