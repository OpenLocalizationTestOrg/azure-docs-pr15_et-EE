<properties
    pageTitle="Real-tööajaga-argument statistikud on Azure CDN | Microsoft Azure'i"
    description="Reaalajas statistika leiate Azure'i CDN jõudluse kohta reaalajas andmed sisu esitamisel kliendid."
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

# <a name="real-time-stats-in-microsoft-azure-cdn"></a>Reaalajas statistika rakenduses Microsoft Azure'i CDN-ID

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Ülevaade

Selles dokumendis selgitatakse reaalajas statistika rakenduses Microsoft Azure'i CDN.  See funktsioon pakub reaalajas andmed, nt läbilaskevõime, vahemälu olekud ja samaaegseid ühendused profiili CDN sisu esitamisel kliendid. See võimaldab pidevalt jälgida oma teenuse seisundi igal ajal, sh minge live sündmused.

Saadaval on järgmised graafikute.

* [Läbilaskevõime](#bandwidth)
* [Olekukoodi](#status-codes)
* [Vahemälu käsitsi olekud](#cache-statuses)
* [Ühendused](#connections)


## <a name="accessing-real-time-stats"></a>Juurdepääs reaalajas statistika

1. Sirvige [Azure portaali](https://portal.azure.com)CDN profiili.

    ![CDN profiili blade](./media/cdn-real-time-stats/cdn-profile-blade.png)

2. Keelest CDN profiil nuppu **Halda** .

    ![CDN profiili blade haldamise nupp](./media/cdn-real-time-stats/cdn-manage-btn.png)

    CDN haldusportaali avatakse.

3. Menüü **analüüsi** kursoriga ja seejärel kursoriga **Reaalajas statistika** hüpik.  Klõpsake **HTTP suur objekt**.

    ![CDN haldusportaal](./media/cdn-real-time-stats/cdn-premium-portal.png)

    Kuvatakse reaalajas statistika diagrammid.
    
Iga graafikud kuvatakse reaalajas statistika valitud ajavahemiku, alustades lehe laadimise ajal.  Automaatne uuendamine graafikud iga paari sekundi järel.  Nupp **Värskenda graafik** , kui see on olemas, tühjendage graafik, misjärel kuvatakse ainult valitud andmete.

## <a name="bandwidth"></a>Läbilaskevõime

![Läbilaskevõime graafik](./media/cdn-real-time-stats/cdn-bandwidth.png)

**Läbilaskevõime** graafik kuvab läbilaskevõimet valitud ajavahemiku praeguse platvormi jaoks kasutada. Graafik varjustatud osa näitab läbilaskevõime kasutuse. Praegu kasutusel läbilaskevõime täpse kuvatakse otse all graafik joon.

## <a name="status-codes"></a>Olekukoodi

![Oleku koodi graafik](./media/cdn-real-time-stats/cdn-status-codes.png)

**Olekukoodi** graafik näitab, kui sageli teatud HTTP vastuse koodid on valitud ajavahemiku esinevate.

> [AZURE.TIP]  Kirjelduse iga HTTP oleku valik, leiate [Azure'i CDN HTTP olekukoodi](https://msdn.microsoft.com/library/mt759238.aspx).

Loendi HTTP olek kuvatakse otse kohal graafik. See loend näitab iga olekukoodi, mida saab lisada rea graafik ja sündmuste sekundis selle oleku koodi praegune arv. Vaikimisi kuvatakse rida iga oleku koodid Graphi. Siiski saate valida ainult jälgida olek koode, mis on CDN konfiguratsioonile teisiti tähtsus. Selleks, märkige soovitud olek koodid ja tühjendage kõik muud suvandid ja seejärel klõpsake nuppu **Värskenda graafik**. 

Saate peita ajutiselt kindla olekukoodi logitud andmed.  Klõpsake soovitud liigutate otse, olekukoodi, mida soovite peita. Oleku koodi peidetakse kohe graafik. Klõpsake selle olekukoodi uuesti põhjustab selle suvandi uuesti kuvada.

## <a name="cache-statuses"></a>Vahemälu olekud

![Vahemälu käsitsi olekud graafik](./media/cdn-real-time-stats/cdn-cache-status.png)

**Vahemälu olekud** graafik näitab, kui sageli teatud tüüpi vahemälu olekud toimuvad valitud ajavahemiku. 

> [AZURE.TIP]  [Azure'i CDN vahemälu olek tähiste](https://msdn.microsoft.com/library/mt759237.aspx)kuvamiseks iga vahemälu olek kood suvand kirjeldust.

Loendi vahemälu olek kuvatakse graafik kohal. See loend näitab iga olekukoodi, mida saab lisada rea graafik ja sündmuste sekundis selle oleku koodi praegune arv. Vaikimisi kuvatakse rida iga Graphi koodid olek. Siiski saate valida oleku koodid, millel on eriline tähendus konfiguratsioonist CDN ainult jälgimiseks. Selleks, märkige soovitud olek koodid ja tühjendage kõik muud suvandid ja seejärel klõpsake nuppu **Värskenda graafik**. 

Saate peita ajutiselt kindla olekukoodi logitud andmed.  Klõpsake soovitud liigutate otse, olekukoodi, mida soovite peita. Oleku koodi peidetakse kohe graafik. Klõpsake selle olekukoodi uuesti põhjustab selle suvandi uuesti kuvada.

## <a name="connections"></a>Ühendused

![Ühenduste graafik](./media/cdn-real-time-stats/cdn-connections.png)

See graafik näitab, mitu ühendused on loodud, et teie Edge'i serverid. Kui vara läbib meie CDN tulemused ühenduse iga taotluse.

## <a name="next-steps"></a>Järgmised sammud

- Teatise saamine kindla [Azure'i CDN reaalajas](cdn-real-time-alerts.md) teated
- Tõrkeid süvitsi [Täpsemalt HTTP](cdn-advanced-http-reports.md) aruannetega
- Analüüsi [mustreid](cdn-analyze-usage-patterns.md)

