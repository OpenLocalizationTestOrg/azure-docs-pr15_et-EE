<properties
    pageTitle="Marsruutimise meetodite liikluse haldur konfigureerimine | Microsoft Azure'i"
    description="Selles artiklis selgitatakse, kuidas konfigureerib marsruutimine eri meetodite liikluse haldur"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-traffic-manager-routing-methods"></a>Marsruutimise meetodite liikluse haldur konfigureerimine

Azure'i liiklust Manager pakub marsruutimise kolm võimalust, et määrata, kuidas liiklust marsruuditakse saadaval teenuse lõpp-punktid. Liikluse marsruutimist meetodit rakendatakse iga DNS-i päringu määratlemiseks, mille lõpp-punkti tuleks tagastada vastuseks DNS-i saanud.

Saadaval liikluse haldur on kolm liikluse marsruutimise viisi:

- **Priority (prioriteet):** Valige "Prioriteet", kui soovite kasutada mõnda esmane teenuse lõpp-punkti ja sisestage varukoopiate juhuks, kui esmane on saadaval.
- **Kaalutud:** Valige "kaalutud" kui soovite jaotada liikluse kogu kogumi lõpp-punktid, kas ühtlaselt või kaalu, mis teile vastavalt määratleda.
- **Jõudluse:** Valige "Jõudlus", kui lõpp-punktid on erinevate geograafiliste asukohtade ja soovite lõppkasutajad kasutada "lähima" lõpp-punkti osas madalam Võrgu latentsusaeg.

## <a name="configure-priority-routing-method"></a>Priority (prioriteet) marsruutimise meetod konfigureerimine

Veebisaidi režiimi, olenemata Azure veebisaitide juba pakkuda Tõrkesiirde funktsionaalsust veebilehed jooksul andmekeskuses (tuntud ka kui piirkond). Liikluse haldur pakub Tõrkesiirde veebisaitide erinevate andmekeskuste.

Levinud mustri teenuse Tõrkesiirde on saata liiklust esmane teenuse ja identse varukoopia teenuste Tõrkesiirde kogumit. Järgmised juhised selgitavad konfigureerida selle tähtsuse Tõrkesiirde Azure pilveteenustega ja veebisaidid:

1. Azure'i klassikaline portaalis vasakul paanil ikooni **Liikluse haldur** liikluse haldur paani avamiseks.
2. Leidke Azure klassikaline portaalis liikluse haldur paanil liikluse haldur profiili, mis sisaldab sätteid, mida soovite muuta. Profiili sätete lehe avamiseks klõpsake profiili nimi paremal olevat noolt.
3. Oma profiili lehel klõpsake lehe ülaservas **lõpp-punktid** . Kontrollige, et nii pilveteenustega ja veebisaidid, mida soovite kaasata konfiguratsioonist ei esita.
4. Ülaosas konfiguratsiooni lehe avamiseks klõpsake nuppu **Konfigureeri** .
5. **Liikluse marsruutimise meetodi sätted**, veenduge, et liikluse marsruutimise meetod on **Tõrkesiirde**. Kui see pole, valige ripploendist **Tõrkesiirde** .
6. **Tõrkesiirde loendis prioriteet**, saate reguleerida Tõrkesiirde lõpp-punktide jaoks. Kui valite **Tõrkesiirde** liikluse marsruutimise meetod, valdkondades järjestuse valitud lõpp-punktid. Esmane lõpp-punkti on alguses. Kasutage üles- ja allanoolt järjestuse muutmiseks vastavalt vajadusele. Lisateavet Windows PowerShelli abil Tõrkesiirde prioriteet seadmise kohta leiate teemast [Set-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Veenduge, et **Jälgimise sätted** on õigesti konfigureeritud. Jälgimine tagab, et lõpp-punktid, mis on ühenduseta ei saadeta liikluse. Lõpp-punktid jälgimiseks, peate määrama tee ja faili nimi. A kaldkriips "/" on kehtiv kirje suhteline tee ja tähendab, et fail on juurkaust (vaikesäte).
8. Pärast konfiguratsiooni muudatuste tegemiseks klõpsake nuppu **Salvesta** lehe allosas.
9. Testige oma konfiguratsiooni muudatusi.
10. Kui profiili liikluse haldur ei tööta, Redigeeri DNS-i kirje serverisse autoriteetsete DNS-i osutamiseks teie ettevõtte domeeninime domeeninimi halduri liikluse.

## <a name="configure-weighted-routing-method"></a>Kaalutud marsruutimise meetod konfigureerimine

Levinud liikluse marsruutimise meetod mustri on identse lõpp-punktid, mis sisaldavad pilveteenustega ja veebisaidid, kogumit ja iga round-jaan mood liikluse suunamiseks. Järgmised toimingud liigendamine konfigureerimine seda tüüpi liikluse marsruutimise meetod.

>[AZURE.NOTE] Azure'i veebisaitidel toodud juba round-jaan laadi funktsioonid veebilehed andmekeskuses (tuntud ka kui piirkond) jooksul tasakaalustamiseks. Liikluse haldur võimaldab teil määrata round-jaan liikluse marsruutimise meetod veebilehed eri andmekeskuste.

1. Azure'i klassikaline portaalis vasakul paanil ikooni **Liikluse haldur** liikluse haldur paani avamiseks.
2. Leidke paanil liikluse haldur liikluse haldur profiili, mis sisaldab sätteid, mida soovite muuta. Profiili sätete lehe avamiseks klõpsake profiili nimi paremal olevat noolt.
3. Oma profiili lehel, klõpsake lehe ülaosas **lõpp-punktid** ja veenduge, et teenuse lõpp-punktid, mida soovite kaasata oma konfiguratsioon on olemas.
4. Oma profiili lehel nuppu **Konfigureeri** avamiseks konfiguratsiooni lehe ülaosas.
5. **Liikluse marsruutimise meetodi sätted**, veenduge, et liikluse marsruutimise meetod on **ringi jaan**. Kui pole, klõpsake ripploendis **Round jaan** .
6. Veenduge, et **Jälgimise sätted** on õigesti konfigureeritud. Jälgimine tagab, et lõpp-punktid, mis on ühenduseta ei saadeta liikluse. Lõpp-punktid jälgimiseks, peate määrama tee ja faili nimi. A kaldkriips "/" on kehtiv kirje suhteline tee ja tähendab, et fail on juurkaust (vaikesäte).
7. Pärast konfiguratsiooni muudatuste tegemiseks klõpsake nuppu **Salvesta** lehe allosas.
8. Testige oma konfiguratsiooni muudatusi.
9. Kui profiili liikluse haldur ei tööta, Redigeeri DNS-i kirje serverisse autoriteetsete DNS-i osutamiseks teie ettevõtte domeeninime domeeninimi halduri liiklust.

## <a name="configure-performance-traffic-routing-method"></a>Jõudluse liikluse marsruutimise meetod konfigureerimine

Jõudluse liikluse marsruutimise meetod, mis võimaldab teil suunaks liikluse lõpp-punkti koos madalam latentsus kliendi võrgu kaudu. Tavaliselt andmekeskuse koos madalam latentsus on selle lähima kauguse. See liikluse marsruutimise meetod ei saa reaalajas muudatused võrgukonfiguratsioon või laadimine.

1. Azure'i klassikaline portaalis vasakul paanil ikooni **Liikluse haldur** liikluse haldur paani avamiseks.
2. Leidke paanil liikluse haldur liikluse haldur profiili, mis sisaldab sätteid, mida soovite muuta. Profiili sätete lehe avamiseks klõpsake profiili nimi paremal olevat noolt.
3. Oma profiili lehel, klõpsake lehe ülaosas **lõpp-punktid** ja veenduge, et teenuse lõpp-punktid, mida soovite kaasata oma konfiguratsioon on olemas.
4. Oma profiili lehel nuppu **Konfigureeri** avamiseks konfiguratsiooni lehe ülaosas.
5. **Liikluse marsruutimise meetodi sätted**, veenduge, et liikluse marsruutimise meetod on * *jõudlust*. Kui pole, klõpsake * *jõudluse** rippmenüüst.
6. Veenduge, et **Jälgimise sätted** on õigesti konfigureeritud. Jälgimine tagab, et lõpp-punktid, mis on ühenduseta ei saadeta liikluse. Lõpp-punktid jälgimiseks, peate määrama tee ja faili nimi. A kaldkriips "/" on kehtiv kirje suhteline tee ja tähendab, et fail on juurkaust (vaikesäte).
7. Pärast konfiguratsiooni muudatuste tegemiseks klõpsake nuppu **Salvesta** lehe allosas.
8. Testige oma konfiguratsiooni muudatusi.
9. Kui profiili liikluse haldur ei tööta, Redigeeri DNS-i kirje serverisse autoriteetsete DNS-i osutamiseks teie ettevõtte domeeninime liikluse haldur domeeni nimi.

## <a name="next-steps"></a>Järgmised sammud

* [Liikluse haldur profiilide haldamine](traffic-manager-manage-profiles.md)
* [Marsruutimise meetodite liikluse haldur](traffic-manager-routing-methods.md)
* [Testimine liikluse haldur sätted](traffic-manager-testing-settings.md)
* [Osutage ettevõtte Interneti-domeeni domeeniga liikluse haldur](traffic-manager-point-internet-domain.md)
* [Liikluse haldur lõpp-punktid haldamine](traffic-manager-manage-endpoints.md)
* [Tõrkeotsingu liikluse haldur halvenenud olek](traffic-manager-troubleshooting-degraded.md)
