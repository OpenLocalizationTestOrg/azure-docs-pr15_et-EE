<properties
   pageTitle="Kuidas 'big data' andmeallikatega töötamise | Microsoft Azure'i"
   description="Juhise esiletõstmine mustrite 'big data' andmeallikatega, sh Azure'i bloobimälu, Azure'i andmed Lake ja Hadoopi HDFS Azure andmekataloogi kasutamise kohta."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Kuidas töötada suurte andmeallikate Azure'i andmekataloogis

## <a name="introduction"></a>Sissejuhatus
**Microsoft Azure'i andmekataloogi** on täielikult hallatud pilveteenuses, mis toimib registreerimise süsteemi ja ettevõtte andmeallikaid discovery süsteem. Teisisõnu, on **Azure andmekataloogi** kõike aidata inimestel tuvastada, mõista ja andmeallikate ja aidata ettevõtted abil saate rohkem väärtust olemasolevates andmeallikates, sh suur andmed.

**Azure'i andmekataloogi** toetab Azure ajaveebi salvestusruumi plekid ja kataloogide kui ka Hadoopi HDFS failide ja kataloogide registreerimise. Poolstruktuur laadi need andmeallikad antakse suurt paindlikkust, kuid see ka tähendab, et kasutajad peavad selleks, et enamik väärtuse toomine registreerumist **Azure'i andmekataloogi**andmeallikad korraldamine.

## <a name="directories-as-logical-data-sets"></a>Loogika andmekogumi kataloogid

Levinud mustri suurte andmeallikate korraldamiseks on loogiline andmekogumi kataloogide käsitleda. Kõrgeima taseme kataloogide abil saate määratleda andmehulgaga, samal ajal alamkaustad määratlemine sektsioonid ja need sisaldavad faile talletada andmed ise.

Näiteks see muster võib olla:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

Selles näites vehicle_maintenance_events ja location_tracking_events tähistavad loogilise andmekogumi. Kõik need kaustad sisaldab andmefailid, mis on korraldatud aasta ja kuu alamkaustad. Kõik need kaustad võib sisaldada potentsiaalselt sadu või tuhandeliste faile.

Selle muster, üksikuid faile registreerumist **Azure'i andmekataloogi** ilmselt ei oleks. Selle asemel registreerida kataloogide, mis tähistab jaoks tähenduslik andmetega töötamine kasutajatele andmekogumi.

## <a name="reference-data-files"></a>Viide andmefailid

Täiendavad mustri on viide andmekogumi eraldi failina salvestada. Nende andmekogumi võib vaadelda kui "väike" servas big data ja sarnanevad sageli mõõtmed analytical andmemudelis. Viide andmefailid sisaldavad kirjeid, mida kasutatakse kontekstis suurema osa talletatud mujal suur andmesalve andmefailid.

Näiteks see muster võib olla:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Mõni analüütik või andmete teadlane töötamisel suuremat kataloogi struktuurid sisalduvate andmete andmete neid faile saab kasutada üksused, mis on nimetatud vaid nime või ID suurema andmehulga üksikasjalikumat teavet.

Selle muster, on mõttekas registreeruma **Azure'i andmekataloogi**individuaalsete andmefailid. Iga faili tähistab andmehulga ja iga saate selgitustega ja avastanud ükshaaval.

## <a name="alternate-patterns"></a>Alternatiivsete mustrite

Eespool kirjeldatud mustreid on kaks võimalust, suur andmesalve võib korraldada, kuid iga rakendamine teistsugused. Sõltumata sellest, kuidas oma andmeallikad on struktureeritud, registreerimisel suurte andmeallikate **Azure'i andmekataloogi**, keskenduda registreerimisel faile ja katalooge, mis tähistab andmekogumi, mis on väärtuse teistele oma ettevõttes. Kõik failid ja kataloogide registreerimisel saate tarbetu kataloogi, raskem otsimise, mida nad vajavad.

## <a name="summary"></a>Kokkuvõte
Andmeallikate registreerumist **Azure'i andmekataloogi** lihtsam neid leida ja mõista. Registreerimisel ja suured failid ja kataloogid, mis tähistab loogilise andmekogumi marginaalid lisanud, saate otsida ja need tuleb suurte andmeallikate kasutamine kasutajate.
