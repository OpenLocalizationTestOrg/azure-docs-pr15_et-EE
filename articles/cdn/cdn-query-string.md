<properties
    pageTitle="Azure'i CDN vahemällu toimimist taotlused koos päringus juhtimine | Microsoft Azure'i"
    description="Azure'i CDN päringustringi vahemällu juhtelemendid, kuidas failid on vahemällu, kui need sisaldavad päringus."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings"></a>Vahemällu toimimist CDN taotlused koos päringus juhtimine

> [AZURE.SELECTOR]
- [Standard](cdn-query-string.md)
- [Azure'i CDN Premium Verizon](cdn-query-string-premium.md)

##<a name="overview"></a>Ülevaade

Päringustringi vahemällu juhtelemendid, kuidas failid on vahemällu, kui need sisaldavad päringus.

> [AZURE.IMPORTANT] Standard- ja Premium CDN tooted andma vahemällu funktsioonid sama päringustringi, kuid kasutajaliidese erineb.  Selles dokumendis kirjeldatakse kasutajaliideses **Azure'i CDN Standard Akamai kaudu** ja **Azure CDN Standard Verizon**.  Päringu stringi vahemällu **Azure'i CDN**Premium Verizon, leiate teemast [mittekontrollivate CDN taotlusi päringu stringide - Premium koos vahemällu toimimist](cdn-query-string-premium.md).

Kolm režiimid on saadaval.

- **Ignoreeri päringu stringide**: see on vaikimisi režiim.  CDN serva sõlm läheb päringu stringi kaudu imperatiivsus päringu päritolu esimese taotlus ja vahemälu vara.  Kõik edaspidised taotlused vara, mis serveeritakse serva sõlme ignoreerivad päringu string kuni vahemällu talletatud varade aegub.
- **Meediumierandi vahemällu URL-i abil päringu stringide**: selles režiimis pole vahemällu taotlused koos päringus veebisaidil sõlme CDN serva.  Serva sõlm toob vara otse päritolu ja edastab selle planeerimisüksused iga taotlusega.
- **Iga kordumatu URL-i vahemälu**: selles režiimis kohtleb iga taotluse päringustringi kordumatu varade koos oma vahemälu.  Näiteks soovite origin *foo.ashx?q=bar* taotluse vastuse vahemälus talletatud veebisaidil serva sõlm ja edaspidised vahemälu ja selle sama päringustringi puhul tagastatakse.  Taotluse *foo.ashx?q=somethingelse* oleks vahemällu eraldi vara Live'i oma aja jooksul.

##<a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Päringustringi vahemällu standard CDN profiilid sätete muutmine

1. Klõpsake keelest CDN profiili CDN lõpp-punkti soovite hallata.

    ![CDN profiili blade lõpp-punktid](./media/cdn-query-string/cdn-endpoints.png)

    CDN lõpp-punkti tera avaneb.

2. Klõpsake nuppu **Konfigureeri** .

    ![CDN profiili blade haldamise nupp](./media/cdn-query-string/cdn-config-btn.png)

    CDN konfiguratsiooni tera avaneb.

3. Valige rippmenüüst **vahemällu käitumise päringustringi** säte.

    ![CDN päringustringi vahemällu salvestamise suvandid](./media/cdn-query-string/cdn-query-string.png)

4. Pärast valiku tegemist klõpsake nuppu **Salvesta** .

> [AZURE.IMPORTANT] Sätete muudatused ei pruugi kohe näha võtab aega registreerimise levitada on CDN kaudu.  <b>Azure'i CDN kaudu Akamai</b> profiilid, tavaliselt on paljundamine ühe minuti jooksul lõpule.  <b>Azure'i CDN Verizon</b> profiilid paljundamine 90 minuti jooksul tavaliselt täielik, kuid mõnel juhul võib võtta ka rohkem aega.
