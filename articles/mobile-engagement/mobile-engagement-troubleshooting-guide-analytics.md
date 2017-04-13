<properties 
   pageTitle="Azure'i mobiilsideseadmete kaasamine Tõrkeotsingujuhendi - Analytics" 
   description="Azure'i Mobile kaasamine analüüsi, jälgimine, osadeks ja armatuurlaua probleemide tõrkeotsing" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Analüüsi, jälgimine, osadeks ja armatuurlaua probleemide tõrkeotsingu juhend

Järgnevalt on võimalikud probleemid võivad ilmneda ja kuidas Azure Mobile kaasamine kogub teavet teie rakendused, seadmed ja kasutajad.

## <a name="missingdelayed-information"></a>Puuduvad hilinenud teave

### <a name="issue"></a>Probleem
- Teave viibib ilmumine Analytics, osadeks või armatuurlaua.
- Teave puudub jälgimine.
- Teave puudub Analytics, osadeks või armatuurlaua.
- Pihta osadeks piirangud.

### <a name="causes"></a>Põhjused

- Saate kasutada Analytics API, kuvari API-ga ja segmente API kas kõik andmed teil on puudu UI on nähtav on API-de kaudu.
- Kui Azure Mobile kaasamine SDK on pole õigesti integreeritud rakenduse siis ei saa te Analytics, osadeks, jälgimine ja armatuurlaudade teabe kuvamiseks.
- Segmente ei saa muuta, kui need on loodud, segmente saab ainult "kloonitud" (kopeeritud) või "hävitatud" (kustutatud). Segmente võib sisaldada ainult 10 kriteeriumid.
- Puuduva teabe kontrollimise testimiseks parim viis on test seadme häälestamise, desinstallige ja/või test seadmes rakendus uuesti.
- Teavet värskendatakse Analytics, osadeks või armatuurlaudade kord 24 tunni jooksul.
- Teave uue segmente kuni 24 tundi pärast loomist isegi juhul, kui lõigu põhineb eelmise teavet ei kuvata.
- UI analytics andmete filtreerimiseks kuvatakse kõik näited sõltumata (version 1 ja 2 versiooni rakenduse kaudu kuvatakse nt "jookseb" nime järgi filtreeritud) rakenduse versioon seda tüüpi.
- Aja jooksul Analyticsi põhineb kuupäeva kasutajate seadme sätted nii, et kasutaja nime, kelle telefon on valesti seatud kuupäev näitaks vale aja jooksul.
- Sunnib pole serveripoolne andmed on sisse logitud, kui kasutate nupp "test", andmed on sisse logitud ainult reaal tõuketeatised kampaaniat jaoks.

## <a name="cant-locate-items-in-ui"></a>Ei leia üksuste UI

### <a name="issue"></a>Probleem
- Ei saa luua segmente põhjal teatud ehitatud või kohandatud rakenduse teave sildistamine kriteeriumid.
- Ma ei leia teatud ehitatud või kohandatud rakenduse teave sildistamine kriteeriumid Analytics, jälgimine ja armatuurlaudu.
- Ei saa tõlgendada Analytics, jälgimine, osadeks või armatuurlaudade andmed.

### <a name="causes"></a>Põhjused

- Mõned ehitatud üksused ja rakenduse teave siltide saadaval ainult tõuketeatised kriteeriumidena, kuid võib olla lisatud segmendi või nähtavaks Analytics, jälgimine ja armatuurlaud. 
- Sisseehitatud üksusi ja rakenduse teave sildid, mida ei saa lisada segmendi, pead häälestamise loendi suunamise kriteeriumid iga kampaanias sama funktsiooni suunatud segmendi sooritamiseks.
- Kontekstimenüüd Analytics, jälgimine, osadeks ja armatuurlaudade jaotiste Azure Mobile kaasamine UI abi saamiseks lugege teemat ja kuidas seda teavet.

## <a name="crash-troubleshooting"></a>Krahhi tõrkeotsing

### <a name="issue"></a>Probleem
- Rakendus jookseb kuvataks Analytics, jälgimine ja armatuurlaud.

### <a name="causes"></a>Põhjused

- Tõrkeotsing rakendus jookseb Analytics, jälgimis või armatuurlaua veenduge, et kontrollida väljalaskemärkmed varasemate versioonide SDK teadaolevad probleemid.
- Tõrkeotsingu sooritamiseks rakendus jookseb teha sündmuse testi seadmesse installitud rakenduse ja seadme ID "Jälgimine – sündmused" osas Azure Mobile kaasamine UI üles otsima. Tehke soovitud sündmus põhjustab rakenduse krahh ja otsida täiendavat teavet Azure Mobile kaasamine UI jaotises "Jälgimine – krahhi". 

 
