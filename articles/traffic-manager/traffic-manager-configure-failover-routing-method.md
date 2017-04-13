<properties
   pageTitle="Konfigureerimine liikluse haldur Tõrkesiirde liikluse marsruutimise meetod | Microsoft Azure'i"
   description="See artikkel aitab teil konfigureerida Tõrkesiirde liikluse marsruutimise meetod liikluse haldur"
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

# <a name="configure-failover-routing-method"></a>Tõrkesiirde marsruutimise meetod konfigureerimine

Sageli ettevõtte soovib töökindluse oma teenuste jaoks. See teeb seda pakkuvad varukoopia juhuks, kui nende esmane teenus läheb alla. Levinud mustri teenuse Tõrkesiirde on identse teenuste kogumit ja liikluse suunamiseks esmane teenus, säilitades konfigureeritud loendi ühe või mitme teenuse varukoopia. Seda tüüpi varundamise saate konfigureerida Azure pilveteenustega ja veebisaidid järgides järgmisi variante.

Pange tähele, et Azure veebisaitide pakub juba Tõrkesiirde liikluse marsruutimist meetod funktsioonid veebilehed jooksul andmekeskuses (tuntud ka kui piirkond), olenemata veebisaidi režiimi. Liikluse haldur võimaldab teil määrata Tõrkesiirde liikluse marsruutimise meetod veebisaitide erinevate andmekeskuste.

## <a name="to-configure-failover-traffic-routing-method"></a>Tõrkesiirde liikluse marsruutimise meetod konfigureerimiseks järgmist.

1. Azure'i klassikaline portaalis vasakul paanil ikooni **Liikluse haldur** liikluse haldur paani avamiseks. Kui te pole veel profiili liikluse haldur loonud, vt [Haldamine liikluse haldur profiilid](traffic-manager-manage-profiles.md) üksikasjalikud juhised lihtsa liikluse haldur profiili loomine.
2. Azure'i klassikaline portaalis liikluse haldur paanil üles liikluse haldur profiili, mis sisaldab sätteid, mida soovite muuta, ja seejärel klõpsake soovitud profiili nimi paremal olevat noolt. See avab profiili lehel sätted.
3. Oma profiili lehel klõpsake lehe ülaservas **lõpp-punktid** ja veenduge, et nii pilveteenused veebisaite (lõpp-punktid), mida soovite arvutamiseks viitesse lisada oma konfiguratsiooni on olemas. Lisamiseks või eemaldamiseks lõpp-punktid juhised leiate teemast [Liikluse haldur lõpp haldamine](traffic-manager-endpoints.md).
4. Oma profiili lehel nuppu **Konfigureeri** avamiseks konfiguratsiooni lehe ülaosas.
5. **Liikluse marsruutimise meetodi sätted**, veenduge, et liikluse marsruutimise meetod on **Tõrkesiirde**. Kui see pole, valige ripploendist **Tõrkesiirde** .
6. **Tõrkesiirde loendis prioriteet**, saate reguleerida Tõrkesiirde lõpp-punktide jaoks. Kui valite **Tõrkesiirde** liikluse marsruutimise meetod, valdkondades järjestuse valitud lõpp-punktid. Esmane lõpp-punkti on alguses. Kasutage üles- ja allanoolt järjestuse muutmiseks vastavalt vajadusele. Lisateavet Windows PowerShelli abil Tõrkesiirde prioriteet seadmise kohta leiate teemast [Set-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Veenduge, et **Jälgimise sätted** on õigesti konfigureeritud. Jälgimine tagab, et lõpp-punktid, mis on ühenduseta ei saadeta liikluse. Selleks, et jälgida lõpp-punktid, peate määrama tee ja faili nimi. Pange tähele, et edastamiseks kaldkriips "/" on kehtiv kirje suhteline tee ja tähendab, et fail on juurkaust (vaikesäte). Jälgimise kohta leiate lisateavet teemast [Liikluse haldur jälgimine](traffic-manager-monitoring.md).
8. Pärast konfiguratsiooni muudatuste tegemiseks klõpsake nuppu **Salvesta** lehe allosas.
9. Testige oma konfiguratsiooni muudatusi. Lisateabe saamiseks vaadake [Testimine liikluse haldur sätted](traffic-manager-testing-settings.md) .
10. Kui teie haldur liikluse profiili on häälestamise ja töötamise, Redigeeri DNS-i kirje serverisse autoriteetsete DNS-i osutamiseks teie ettevõtte domeeninime domeeninimi halduri liikluse. Lisateavet selle kohta, kuidas seda teha [punkti ettevõtte Interneti-domeeni domeeniga liikluse haldur](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Järgmised sammud

[Osutage ettevõtte Interneti-domeeni domeeniga liikluse haldur](traffic-manager-point-internet-domain.md)

[Marsruutimise meetodite liikluse haldur](traffic-manager-routing-methods.md)

[Round jaan marsruutimise meetod konfigureerimine](traffic-manager-configure-round-robin-routing-method.md)

[Jõudluse marsruutimise meetod konfigureerimine](traffic-manager-configure-performance-routing-method.md)

[Tõrkeotsingu liikluse haldur halvenenud olek](traffic-manager-troubleshooting-degraded.md)

[Liiklus Manager - Keela Luba või profiili kustutamine](disable-enable-or-delete-a-profile.md)

[Liikluse haldur - keelamine või lubamine lõpp](disable-or-enable-an-endpoint.md)

