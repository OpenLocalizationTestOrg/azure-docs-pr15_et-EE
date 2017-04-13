<properties
   pageTitle="Jõudluse liikluse marsruutimise meetod konfigureerimine | Microsoft Azure'i"
   description="See artikkel aitab teil konfigureerida jõudluse liikluse marsruutimise meetod liikluse haldur"
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-performance-traffic-routing-method"></a>Jõudluse liikluse marsruutimise meetod konfigureerimine

Marsruutida liikluse pilveteenustega ja veebisaite (lõpp-punktid), mis asuvad eri andmekeskuste kogu maailmas (tuntud ka kui piirkonnad), et te saate otse liiklust lõpp-punkti madalam latentsus taotlemise kliendilt. Tavaliselt andmekeskuse koos madalam latentsus vastab lähimasse geograafilised kaugus. Jõudluse liikluse marsruutimise meetod võimaldab teil levitamine madalam latentsus põhjal, kuid ei saa arvesse reaalajas muutusi võrgukonfiguratsioon või laadi. Azure'i liikluse Manager pakub erinevate liikluse marsruutimise meetodite kohta lisateabe saamiseks vt [liikluse haldur liikluse marsruutimist meetoditest](traffic-manager-routing-methods.md).

## <a name="route-traffic-based-on-lowest-latency-across-a-set-of-endpoints"></a>Marsruutimiseks liikluse põhjal madalam latentsus kogu kogumi lõpp-punktid:

1. Azure'i klassikaline portaalis vasakul paanil ikooni **Liikluse haldur** liikluse haldur paani avamiseks. Kui te pole veel profiili liikluse haldur loonud, vt [Haldamine liikluse haldur profiilid](traffic-manager-manage-profiles.md) juhiseid liikluse haldur põhiprofiil loomiseks.
2. Azure'i klassikaline portaalis paanil liikluse haldur üles liikluse haldur profiili, mis sisaldab sätteid, mida soovite muuta, ja seejärel klõpsake soovitud profiili nimi paremal olevat noolt. See avab profiili lehel sätted.
3. Oma profiili lehel, klõpsake lehe ülaosas **lõpp-punktid** ja veenduge, et teenuse lõpp-punktid, mida soovite kaasata oma konfiguratsioon on olemas. Lisamiseks või eemaldamiseks lõpp-punktid profiilist juhised leiate teemast [Liikluse haldur lõpp haldamine](traffic-manager-endpoints.md).
4. Oma profiili lehel nuppu **Konfigureeri** avamiseks konfiguratsiooni lehe ülaosas.
5. **Liikluse marsruutimise meetodi sätted**, veenduge, et liikluse marsruutimise meetod on * *jõudlust*. Kui pole, klõpsake * *jõudluse** rippmenüüst.
6. Veenduge, et **Jälgimise sätted** on õigesti konfigureeritud. Jälgimine tagab, et lõpp-punktid, mis on ühenduseta ei saadeta liikluse. Selleks, et jälgida lõpp-punktid, peate määrama tee ja faili nimi. Pange tähele, et edastamiseks kaldkriips "/" on kehtiv kirje suhteline tee ja tähendab, et fail on juurkaust (vaikesäte). Jälgimise kohta leiate lisateavet teemast [Kohta liikluse haldur jälgimine](traffic-manager-monitoring.md).
7. Pärast konfiguratsiooni muudatuste tegemiseks klõpsake nuppu **Salvesta** lehe allosas.
8. Testige oma konfiguratsiooni muudatusi. Lisateavet leiate teemast [Testimine liikluse haldur sätted](traffic-manager-testing-settings.md).
9. Kui profiili liikluse haldur on häälestamise ja töötamise, Redigeeri DNS-i kirje serverisse autoriteetsete DNS-i osutamiseks teie ettevõtte domeeninime domeeninimi halduri liiklust. Lisateavet selle kohta, kuidas seda teha [punkti ettevõtte Interneti-domeeni domeeniga liikluse haldur](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Järgmised sammud


[Osutage ettevõtte Interneti-domeeni domeeniga liikluse haldur](traffic-manager-point-internet-domain.md)

[Marsruutimise meetodite liikluse haldur](traffic-manager-routing-methods.md)

[Tõrkesiirde marsruutimise meetod konfigureerimine](traffic-manager-configure-failover-routing-method.md)

[Round jaan marsruutimise meetod konfigureerimine](traffic-manager-configure-round-robin-routing-method.md)

[Tõrkeotsingu liikluse haldur halvenenud olek](traffic-manager-troubleshooting-degraded.md)

[Liiklus Manager - Keela Luba või profiili kustutamine](disable-enable-or-delete-a-profile.md)

[Liikluse haldur - keelamine või lubamine lõpp](disable-or-enable-an-endpoint.md)

