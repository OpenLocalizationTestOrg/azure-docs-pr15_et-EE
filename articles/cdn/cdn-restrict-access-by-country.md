<properties
    pageTitle="Azure'i CDN sisu vastavalt riigile juurdepääsu piiramiseks | Microsoft Azure'i"
    description="Saate teada, kuidas piirata juurdepääsu Azure CDN sisu Geo filtreerimise funktsiooni abil."
    services="cdn"
    documentationCenter=""
    authors="camsoper, rli"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="casoper"/>

#<a name="restrict-access-to-your-content-by-country---verizon"></a>Juurdepääsu piiramiseks riigi - Verizon sisu

> [AZURE.SELECTOR]
- [Verizon](cdn-restrict-access-by-country.md)
- [Akamai Standard](cdn-restrict-access-by-country-akamai.md)

##<a name="overview"></a>Ülevaade

Kui kasutaja taotleb oma sisu vaikimisi, serveeritakse sisu sõltumata sellest, kui kasutaja tehtud taotluse kaudu. Mõnel juhul soovite piirata juurdepääsu sisu vastavalt riigile. See teema selgitab **Geo filtreerimine** funktsiooni kasutamine selleks, et konfigureerida teenusele, et lubada või Blokeeri juurdepääs vastavalt riigile.

> [AZURE.IMPORTANT] Verizon ja Akamai tooted pakuvad sama geo filtreerimine funktsiooni, kuid kasutajaliidese erineb. Selles dokumendis kirjeldatakse kasutajaliideses **Azure'i CDN Standard Verizon** ja **Azure CDN Premium Verizon**. Geo filtreerimine koos **Azure CDN Standard Akamai kaudu**, leiate teemast [riigi - Akamai sisu juurdepääsu piirata](cdn-restrict-access-by-country-akamai.md).

Asjaolu rakendamine seda tüüpi piirangut konfigureerimise kohta leiate jaotisest [kaalutlused](cdn-restrict-access-by-country.md#considerations) teema lõpus.  

>[AZURE.NOTE] Kui konfiguratsiooni on häälestatud, rakendatakse kõik selle Azure'i CDN profiili **Azure'i CDN Verizon** lõpp-punktid.

![Riigi filtreerimine](./media/cdn-filtering/cdn-country-filtering.png)

##<a name="step-1-define-the-directory-path"></a>Samm 1: Määratleda kausta tee

Filtri riigi konfigureerimisel Määrake asukoht, kuhu kasutajad on lubatud või keelatud juurdepääsu suhteline tee. Saate rakendada kõigi failide filtreerimine riik "/" või valitud kaustad, määrates directory teed.

Näide directory tee filter:

    /                                 
    /Photos/
    /Photos/Strasbourg

##<a name="step-2-define-the-action-block-or-allow"></a>Samm 2: Määratle toiming: blokeerimine või lubamine

**Blokeerimine:** Kasutajate määratud riigid keelatakse juurdepääs varad taotleda Rekursiivsed teel. Kui muid riigi filtreerimise võimalusi pole konfigureeritud seda asukohta, siis kõik teised kasutajad on lubatud juurdepääs.

**Luba:** Ainult need kasutajad määratud riikidest lubatakse varad Rekursiivsed teel taotleda juurdepääsu.

##<a name="step-3-define-the-countries"></a>Samm 3: Määratlemine riikide

Valitud riikides, mida soovite blokeerida või lubada tee. Lisateabe saamiseks leiate [Azure'i CDN Verizon riigi koodide kaudu](https://msdn.microsoft.com/library/mt761717.aspx).

Näiteks reegli blokeerimise /Photos/Strasbourg/filtreeritakse failid, sh:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


##<a name="country-codes"></a>Riikide koodid

**Geo filtreerimise** funktsioon kasutab riigi koodide määratlemiseks riigid, millest on taotluse lubatud või blokeeritud turvalise kataloogi. Riikide koodid leiate [Azure'i CDN Verizon riigi koodide kaudu](https://msdn.microsoft.com/library/mt761717.aspx). Kui määrate "EL" (Europe) või "AP" (Aasia Vaikse ookeani), alamhulga IP-aadressid, et riik, et piirkondades on pärit blokeeritud või lubatud.


##<a id="considerations"></a>Kaalutlused

- Teie riigis filtreerimise konfiguratsiooni muudatuste jõustumiseks võib kuluda kuni 90 minutit muudatuste.
- See funktsioon ei toeta metamärkide (nt "*").
- Filtreerimise konfigureerimine seostatud suhteline tee riik on rakendatud rekursiivselt selle tee.
- Ainult üks reegel saab rakendada sama suhteline tee (ei saa luua mitme riigi filtreid, mis viitavad sama suhteline tee. Kuid kausta võib olla mitu riigi filtrit. See on rekursiivse laadist riigi filtrid. Teisisõnu on varem konfigureeritud alamkaust saab määrata eri riigi filtri.
