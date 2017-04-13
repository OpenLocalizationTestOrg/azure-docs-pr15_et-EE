<properties
   pageTitle="Konfigureerimine liikluse haldur round jaan liikluse marsruutimise meetod | Microsoft Azure'i"
   description="See artikkel aitab teil konfigureerida round jaan laadi liikluse haldur lõpp-punktide jaoks."
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

# <a name="configure-round-robin-routing-method"></a>Konfigureerimine Round jaan marsruutimise meetod

Levinud liikluse marsruutimise meetod mustri on identne lõpp-punktid, mis sisaldavad pilveteenustega ja veebisaidid, kogumit ja saata liiklust iga round-jaan mood. Alltoodud juhiseid liigendamine konfigureerimine liikluse haldur täitmiseks seda tüüpi liikluse marsruutimise meetod. Erinevate liikluse marsruutimise kohta leiate lisateavet teemast [liikluse haldur liikluse marsruutimise meetodid](traffic-manager-routing-methods.md).

>[AZURE.NOTE] Azure'i veebisaitide pakub juba round-jaan laadi funktsioonid veebilehed andmekeskuses (tuntud ka kui piirkond) jooksul tasakaalustamiseks. Liikluse haldur võimaldab teil määrata round-jaan liikluse marsruutimise meetod veebilehed eri andmekeskuste.

## <a name="routing-traffic-equally-round-robin-across-a-set-of-endpoints"></a>Marsruutimise liiklust võrdselt (round jaan) kogu kogumi lõpp-punktid:

1. Azure'i klassikaline portaalis vasakul paanil ikooni **Liikluse haldur** liikluse haldur paani avamiseks. Kui te pole veel profiili liikluse haldur loonud, vt [Haldamine liikluse haldur profiilid](traffic-manager-manage-profiles.md) üksikasjalikud juhised lihtsa liikluse haldur profiili loomine.
2. Azure'i klassikaline portaalis paanil liikluse haldur üles liikluse haldur profiili, mis sisaldab sätteid, mida soovite muuta, ja seejärel klõpsake soovitud profiili nimi paremal olevat noolt. See avab profiili lehel sätted.
3. Oma profiili lehel, klõpsake lehe ülaosas **lõpp-punktid** ja veenduge, et teenuse lõpp-punktid, mida soovite kaasata oma konfiguratsioon on olemas. Lisamiseks või eemaldamiseks lõpp-punktid juhised leiate teemast [Liikluse haldur lõpp haldamine](traffic-manager-endpoints.md).
4. Oma profiili lehel nuppu **Konfigureeri** avamiseks konfiguratsiooni lehe ülaosas.
5. **Liikluse marsruutimise meetodi sätted**, veenduge, et liikluse marsruutimise meetod on **ringi jaan**. Kui pole, klõpsake ripploendis **Round jaan** .
6. Veenduge, et **Jälgimise sätted** on õigesti konfigureeritud. Jälgimine tagab, et lõpp-punktid, mis on ühenduseta ei saadeta liikluse. Selleks, et jälgida lõpp-punktid, peate määrama tee ja faili nimi. Pange tähele, et edastamiseks kaldkriips "/" on kehtiv kirje suhteline tee ja tähendab, et fail on juurkaust (vaikesäte). Jälgimise kohta leiate lisateavet teemast [Kohta liikluse haldur jälgimine](traffic-manager-monitoring.md).
7. Pärast konfiguratsiooni muudatuste tegemiseks klõpsake nuppu **Salvesta** lehe allosas.
8. Testige oma konfiguratsiooni muudatusi. Lisateavet leiate teemast [Testimine liikluse haldur sätted](traffic-manager-testing-settings.md).
9. Kui teie haldur liikluse profiili on häälestamise ja töötamise, Redigeeri DNS-i kirje serverisse autoriteetsete DNS-i osutamiseks teie ettevõtte domeeninime domeeninimi halduri liikluse. Lisateavet selle kohta, kuidas seda teha [punkti ettevõtte Interneti-domeeni domeeniga liikluse haldur](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Järgmised sammud


[Osutage ettevõtte Interneti-domeeni domeeniga liikluse haldur](traffic-manager-point-internet-domain.md)

[Marsruutimise meetodite liikluse haldur](traffic-manager-routing-methods.md)

[Tõrkesiirde marsruutimise meetod konfigureerimine](traffic-manager-configure-failover-routing-method.md)

[Jõudluse marsruutimise meetod konfigureerimine](traffic-manager-configure-performance-routing-method.md)

[Tõrkeotsingu liikluse haldur halvenenud olek](traffic-manager-troubleshooting-degraded.md)

[Liiklus Manager - Keela Luba või profiili kustutamine](disable-enable-or-delete-a-profile.md)

[Liikluse haldur - keelamine või lubamine lõpp](disable-or-enable-an-endpoint.md)

