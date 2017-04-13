<properties 
   pageTitle="Azure'i mobiilsideseadmete kaasamine Tõrkeotsingujuhendi - tõuketeatised virnastada" 
   description="Azure'i Mobile kaasamine kasutaja suhtluse ja teavitusi probleemide tõrkeotsing" 
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

# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>Tõrkeotsing juhend tõuketeatised ja probleemid saavutamiseks.

Järgnevalt on võimalikud probleemid võivad ilmneda ja kuidas Azure Mobile kaasamine saadab kasutajatele teavet.
 
## <a name="push-failures"></a>Tõuketeatised tõrked

### <a name="issue"></a>Probleem
- Sunnib ei tööta (rakendus app või mõlemad).

### <a name="causes"></a>Põhjused
- Mitu korda tõuketeatised tõrge selline teade, et Azure Mobile kaasamine, REACHi või mõne muu täpsemalt funktsiooni Azure Mobile kaasamine on õigesti integreeritud või versiooniuuenduse nõutav SDK, uue OS või seadme platvorm teadaolevate probleemide lahendamiseks.
- Testige ainult rakenduse sisse tõuketeatised ja lihtsalt välja rakenduse tõuketeatised määratlemaks, kas miski on rakenduse sisse või välja probleemi.
- Testige UI nii API tõrkeotsingu käigus mis täiendav teave on saadaval nii kohad.
- Appist välja sunnib ei tööta, juhul, kui nii Azure Mobile kaasamine ja REACHi on integreeritud SDK.
- Sunnib ei tööta, kui serdid ei sobi või kasutate TOOTEKATALOOGI vs arendaja õigesti (ainult iOS). (**Märkus:** "välja rakenduse" Tõuketeatiste võib-olla ei saadeta ka iOS-i, kui teil on nii arendamine (arendaja) ja tootmise (TOOTEKATALOOGI) versiooni installitud samasse seadmesse, kuna Turbeloa seostatud teie serdi rakenduse võib lükka Apple'i. Selle probleemi lahendamiseks desinstallige rakenduse arendaja ja TOOTEKATALOOGI versioonid ja uuesti installida ainult ühe versiooni seadme.)
- Välja rakenduse tõuketeatised loendab kasutatakse erinevalt erinevate platvormide (iOS-i kuvatakse vähem teavet kui Androidi seadmes on keelatud kohalikke sunnib, API saate rohkem teavet UI kui tõuketeatised statistika).
- Välja rakenduse sunnib saate blokeerida kliendid OS tasemel (iOS ja Android).
- Välja rakenduse sunnib kuvatakse puudega Azure Mobile kaasamine UI, kui nad pole integreeritud õigesti, kuid võib nurjuda vaikselt API.
- Rakenduses sunnib ei tööta, juhul, kui nii Azure Mobile kaasamine ja REACHi on integreeritud SDK.
- Sunnib GCM ja ADM ei tööta, juhul kui Azure Mobile kaasamine ja teatud server on integreeritud SDK (ainult Androidi).
- Rakenduste ja välja rakenduse sunnib tuleks katsetada eraldi kindlaks teha, kui see on tõuketeatised või REACHi küsimus.
- Rakenduses sunnib nõuda, et rakendus oleks avatud vastu võtta.
- Rakenduse sunnib sageli häälestus on valida sisse või loobumise rakenduse teave sildi järgi filtreerida.
- Kui kasutate kohandatud kategooria REACHi kuvamiseks klõpsake rakenduse teatised, peate järgima õige teate elutsükli, muidu ei tühjendanud teate kui kasutaja jätta.
- Kui alustate kampaaniat pole lõppkuupäeva ja seadme saab tekstivormingus rakenduse teatis, kuid ei ole seda veel, siis kasutaja siiski vastu teatis järgmine kord, kui rakendusse, sisse logida, isegi kui te käsitsi kampaaniat.
- Probleemid Push API, veenduge, et soovite kasutada Push API asemel API saavutamiseks (kuna saavutamiseks API kasutatakse sagedamini) ja et teil on segane "last" ja "taotleja" parameetrid.
- Testige oma tõuketeatised turunduskampaania nii seade ühendatud Wi-Fi- ja 3G võrguühendus allikana võimalike probleemide kõrvaldamiseks.

## <a name="push-testing"></a>Tõuketeatised testimine

### <a name="issue"></a>Probleem
- Sunnib saab saata kindla seadme põhjal mõne seadme ID.

### <a name="causes"></a>Põhjused

- Test seadmed on häälestamise teisiti iga platvormi, kuid põhjustab sündmuse rakenduse seadmes testi ja otsin seadme ID portaalis peaksid tegema, et oma seadme leiab kõik platvormid.
- Test seadmed töötavad rakenduse IDFA vs IDFV (ainult iOS).


## <a name="push-customization"></a>Tõuketeatised kohandamine

### <a name="issue"></a>Probleem
- Täpsemad tõuketeatised sisu üksust ei tööta (nimesildi, Helisevad, värin, pilt, jne).
- Sunnib lingid ei tööta (kontorist väljas rakendus veebisaidile, rakenduse asukohta rakenduses).
- Tõuketeatised statistika näitab, et push saadeti nii palju inimesi ootuspäraselt (liiga palju või pole piisavalt).
- Tõuketeatised dubleeritud ja saanud kaks korda.
- Ei saa registreerida testi seadme jaoks Azure Mobile kaasamine sunnib (koos oma tootekataloogi või arendaja rakendus).

### <a name="causes"></a>Põhjused

- Linkimine kindlasse asukohta rakendus peab olema "kategooria" (ainult Androidi).
- Sügav linkimine skeemid ümber suunata kasutajate alternatiivse asukoha pärast nupu tõuketeatised teatise vaja luua ja hallata otse rakenduse ja seadme OS mitte poolt Mobile kaasamine. (**Märkus:** Appist välja teatised ei saa lingi otse rakendus iOS-i abil asukohti kui nad saavad Android.)
- Välise pilt serverid peavad saama kasutada HTTP "Saada" ja "Pea" jaoks üldpildiga sunnib töötamiseks (ainult Androidi).
- Koodi, saate keelata Azure Mobile kaasamine agent klaviatuuri avamisel ja on teie kood Azure Mobile kaasamine agent uuesti aktiveerida, kui klaviatuuri on suletud, et klaviatuuri ei mõjuta teie teatis (ainult iOS) ilmet.
- Teatud üksusi ei tööta testi simulatsioonid, kuid ainult reaal kampaaniat (nimesildi, Helisevad, värin, pilt, jne).
- Kui nupu abil "test" sunnib logitud serveri side andmed. Andmed on sisse logitud ainult reaal tõuketeatised kampaaniat.
- Selleks, et eristada teie probleemi, kus tõrkeotsing: testida, simuleerida, ja real turunduskampaania, kuna need iga töötavad veidi erineda.
- Pikkus oma "rakenduses" ja "time" kampaaniat on ajastatud käivitamiseks, saate efekti kohaletoimetamise arvude pärast kampaaniat ainult toimetatakse kasutajatele, kellel on "app" ajal turunduskampaania käivitatakse (ja kasutajad, kellel on oma seadme sätted määratud teatisi "välja rakenduse").
- Kuidas Androidi ja iOS-i toime välja rakenduste teatised erinevused keeruline otse võrdlus tõuketeatised statistika vahel rakenduse Androidi ja iOS-i versioon. Android annab OS taseme teatise Lisateavet kui iOS-i. Androidi aruannete kohalikke teatise saadud, klõpsamisel või kustutatud keskel teade, kuid iOS-i aruande seda teavet, v.a juhul, kui teate klõpsamisel. 
- Peamine põhjus, et "tõugatav" arvud on erinev erinevate arvud "kohale toimetatud" saavutamiseks kampaaniat on, et "app" ja "välja rakenduse" teatised loendatakse teisiti. "Rakenduses" Mobile kaasamine tarkvaras teatised, kuid "välja rakenduse" tarkvaras teatised seadme OS teatis keskele.

## <a name="push-targeting"></a>Tõuketeatised suunamise

### <a name="issue"></a>Probleem
- Sisseehitatud suunamise ei tööta ootuspäraselt.
- Rakenduse teave sildi suunamise ei tööta ootuspäraselt.
- Geograafilise asukoha suunamise ei tööta ootuspäraselt.
- Keelesuvandid ei tööta ootuspäraselt.

### <a name="causes"></a>Põhjused

- Veenduge, et on üles laaditud rakenduse teave siltide Azure Mobile kaasamine UI või API kaudu.
- Tõuketeatised kiirust või tõuketeatised kvoodi tasemel ahendamine või piiravad publiku tasemel turunduskampaania saate takistada isiku vastuvõtmine teatud tõuketeatised isegi juhul, kui need vastavad teie suunatud kriteeriumidele. 
- "Keel" on teistsugune kui suunamise riigi või lokaadi, mis on ka erinevas suunamise geograafilise asukoha alusel põhjal telefoni asukoht või GPS asukoht.
- "Vaikekeele" sõnum saadetakse kõigile klientidele, kes ei ole oma seadmes mõni teie määratud keeltesse.


## <a name="push-scheduling"></a>Tõuketeatised plaanimine

### <a name="issue"></a>Probleem
- Tõuketeatised ajastamine ei tööta eeldatud viisil (saadetud liiga ennetähtaegse või hilinenud).

### <a name="causes"></a>Põhjused

- Ajavööndid saate probleeme kavandamine, eriti siis, kui kasutate selle lõppkasutajad ajavöönd.
- Täiustatud tõuketeatised funktsioone saab viivitamine sunnib.
- Telefoni sätted (rakenduse teave sildid) asemel saate viivitamine abil suunatud sunnib Kuna Azure Mobile kaasamine võib olla vajalik nõuda enne saatmist push telefoni reaalajas andmed.
- Ilma lõppkuupäev loodud kampaaniat salvestada push on seadmesse ja Kuva see järgmine kord, kui rakendus on avatud, isegi juhul, kui kampaanias on lõppenud käsitsi.
- Korraga mitu kampaaniat alates võib võtta aega kontrollida oma kasutaja base (proovige alustamiseks ainult ühe turunduskampaania korraga kuni nelja ka eesmärgi ainult aktiivsed kasutajad nii, et vana kasutajad ei pea olema skannitud).
- Kui kasutate suvandi "Ignoreeri publik, tõuketeatised saadetakse kasutajate kaudu API" "Turunduskampaania" osas REACHi turunduskampaania, ei saadeta kampaaniat automaatselt, peate saavutamiseks API käsitsi saata.
- Kui kasutate REACHi-rakenduse teatiste kuvamiseks kohandatud kategooria, peate järgima teatise õige elutsükli, muidu ei tühjendanud teate kui kasutaja jätta.

 
