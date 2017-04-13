<properties
    pageTitle="Azure CDN Premium Verizon vahemällu toimimist taotlused koos päringus juhtimine | Microsoft Azure'i"
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings---premium"></a>Vahemällu toimimist CDN taotlused koos päringu stringide - Premium juhtimine

> [AZURE.SELECTOR]
- [Standard](cdn-query-string.md)
- [Azure'i CDN Premium Verizon](cdn-query-string-premium.md)

##<a name="overview"></a>Ülevaade

Päringustringi vahemällu juhtelemendid, kuidas failid on vahemällu, kui need sisaldavad päringus.

> [AZURE.IMPORTANT] Standard- ja Premium CDN tooted andma vahemällu funktsioonid sama päringustringi, kuid kasutajaliidese erineb.  Selles dokumendis kirjeldatakse **Azure CDN Premium Verizon**kasutajaliides.  Päringu stringi vahemällu **Azure'i CDN Standard Akamai kaudu** ja **Azure CDN Standard Verizon**, leiate teemast [mittekontrollivate CDN taotlusi päringus koos vahemällu toimimist](cdn-query-string.md).

Kolm režiimid on saadaval.

- **Standard-vahemälu**: see on vaikimisi režiim.  CDN serva sõlm läheb päringu stringi kaudu imperatiivsus päringu päritolu esimese taotlus ja vahemälu vara.  Kõik edaspidised taotlused vara, mis serveeritakse serva sõlme ignoreerivad päringu string kuni vahemällu talletatud varade aegub.
- **ei – vahemälu**: selles režiimis pole vahemällu taotlused koos päringus veebisaidil sõlme CDN serva.  Serva sõlm toob vara otse päritolu ja edastab selle planeerimisüksused iga taotlusega.
- **kordumatu vahemälu**: selles režiimis kohtleb iga taotluse päringustringi kordumatu varade koos oma vahemälu.  Näiteks soovite origin *foo.ashx?q=bar* taotluse vastuse vahemälus talletatud veebisaidil serva sõlm ja edaspidised vahemälu ja selle sama päringustringi puhul tagastatakse.  Taotluse *foo.ashx?q=somethingelse* oleks vahemällu eraldi vara Live'i oma aja jooksul.

##<a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Päringustringi vahemällu premium CDN profiilid sätete muutmine

1. Keelest CDN profiil nuppu **Halda** .

    ![CDN profiili blade haldamise nupp](./media/cdn-query-string-premium/cdn-manage-btn.png)

    CDN haldusportaali avatakse.

2. Kursoriga **HTTP suur** vahekaarti ja seejärel kursoriga **Vahemälusätete** hüpik.  Klõpsake **päringu String vahemällu**.

    Päringu stringi vahemällu salvestamise suvandid kuvatakse.

    ![CDN päringustringi vahemällu salvestamise suvandid](./media/cdn-query-string-premium/cdn-query-string.png)

3. Pärast valiku, klõpsake nuppu **Värskenda** .


> [AZURE.IMPORTANT] Sätete muudatused ei pruugi kohe näha võtab aega registreerimise levitada on CDN kaudu.  <b>Azure'i CDN Verizon</b> profiilid paljundamine 90 minuti jooksul tavaliselt täielik, kuid mõnel juhul võib võtta ka rohkem aega.
