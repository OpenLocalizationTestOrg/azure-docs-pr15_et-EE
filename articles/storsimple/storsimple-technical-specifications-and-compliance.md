<properties 
   pageTitle="StorSimple tehnilised näitajad | Microsoft Azure'i"
   description="Kirjeldatakse tehnilised näitajad ja seaduses sätestatud standardeid vastavuse teave riistavara StorSimple jaoks."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>Tehnilised näitajad ja StorSimple seadme nõuetele vastavus

## <a name="overview"></a>Ülevaade

Microsoft Azure'i StorSimple seadme riistvara järgima tehnilised näitajad ja seaduses sätestatud standardeid käesolevas artiklis kirjeldatud. Tehnilised saate kirjeldada Power ja jahutus moodulid (PCMs), kettadraivide, mälumaht ja Kirjalisandid. Vastavuse teave hõlmab rahvusvaheline standardid, ohutus- ja heitkoguste ja kaabeldus.


## <a name="power-and-cooling-module-specifications"></a>Power ja jahutus mooduli tehnilised andmed  

StorSimple seade on kaks 100-240V kahekordne fänn, SBB nõuetele vastavuse Power jahutus moodulid (PCMs). See pakub liigsete power konfigureerimine. Kui soovitud PCM nurjub, seadme jätkub Töövool tavapäraselt muude PCM kuni nurjunud mooduli.  

EBOD ruumi kasutab 580 W PCM ja esmane ruumi kasutab 764 W PCM. Järgmises tabelis on loetletud selle PCMs seotud tehnilised.

| Määratlus           | 580 W PCM (EBOD)                                    | 764 W PCM (primaarne)                                |
|------------------------ | --------------------------------------------------- | -------------------------------------------------- |
| Suurim lubatud võimsus    | 580 W                                               | 764                                                |
| Frequency               | 50/60 Hz                                            | 50/60 Hz                                           |
| Pinge valimine | Automaatne siniseni: 90 – 264 V AC, 47/63 Hz               | Automaatne siniseni: 90-264 V AC, 47/63 Hz               |
| Suurim lubatud sünkroniseerimisseadmega praeguse  | 20 A                                                | 20 A                                               |
| Power tegur parandamine | > Sisestuskeel nimipinge 95%                          | > Sisestuskeel nimipinge 95%                         |
| Harmoonilised               | Vastab EN61000-3-2                                   | Vastab EN61000-3-2                                  |
| Väljund                  | 5 v puhkerežiimis olev pinge @ 2.0 A                          | 5 v puhkerežiimis olev pinge @ 2.7 A                         |
|                         | + 5 v @ 42 A                                          | + 5 v @ 40 A                                         |
|                         | + 12 @ 38 A                                         | + 12 @ 38 A                                        |
| Ühendatav otsetee           |  Jah                                                | Jah                                                |
| Parameetrid ja LED       | AC on / lüliti ja neli Olekunäidik LED     | AC on / lüliti ja kuus Olekunäidik LED     |
| Ruumi jahutus       | Axial ventilaatorid koos muutuv ventilaatori kiiruse reguleerimine  | Axial ventilaatorid koos muutuv ventilaatori kiiruse reguleerimine |

 
## <a name="power-consumption-statistics"></a>Power tarbimine statistika  

Järgmises tabelis on loetletud erinevate mudelite StorSimple seadme tüüpilised power tarbimine andmed (tegelike väärtuste võivad erineda avaldatud). 
 
 Tingimused | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC 
 ---------- | -------- | -------- | -------- | -------- | -------- | -------- 
 Ventilaatoreid aeglane, draivid jõude | 1.45 A  |0,31 kW | 1057.76 BTU tunnis | 3.19 A | 0,34 kW | 1160.13 BTU tunnis 
 Ventilaatoreid aeglane draivid juurdepääs | 1,54 A | 0,33 kW | 1126.01 BTU tunnis | 3,27 A | 0,36 kW | 1228.37 BTU tunnis 
 Ventilaatoreid kiiresti, draivid jõude, mida kaks PSUs | 2.14 A | 0,49 kW  | 1671.95 BTU tunnis | 4.99 A | 0,54 kW | 1842.56 BTU tunnis 
 Kiire, ventilaatoreid draivid jõude, üks PSU mida üks jõude | 2,05 A | 0,48 kW | 1637.83 BTU tunnis | 4.58 A | 0,50 kW | 1706.07 BTU tunnis 
 Ventilaatoreid kiiresti, draivid juurdepääs, mida kaks PSUs | 2.26 A | 0,51 kW | 1740.19 BTU tunnis | 4,95 A | 0,54 kW | 1842.56 BTU tunnis 
 Fast ventilaatoreid, draivid juurdepääs, üks PSU mida üks jõude | 2.14 A |0,49 kW | 1671.95 BTU tunnis | 4.81 A  | 0,53 kW | 1808.44 BTU tunnis 

## <a name="disk-drive-specifications"></a>Kettale tehnilised andmed  

Seadme StorSimple toetab kuni 12 3.5-tolli vormi tegur järjestikune manustatud SCSI (SAS) kettadraivide. Tegelik draivid võivad olla kombinatsiooni välkdraivide (SSD) või kõvaketta draivid (kõvaketast) toote konfiguratsioonist. 12 kettale teenindusaegade asuvad 3, 4 konfiguratsiooni ette ruumi. Täiendav salvestusruum teise 12 kettadraivide võimaldab EBOD ruumi. Need on alati HDD-d.  

## <a name="storage-specifications"></a>Salvestusruumi tehnilised andmed
StorSimple seadmete on nii on 8100 ja 8600 kõvaketta draivid ja välkdraivid. Funktsiooni 8100 ja 8600 kasutuskõlblikud koguvõimsus on umbes 15 TB ja 38 TB vastavalt. Järgmises tabelis dokumentide üksikasjade SSD, HDD ja pilveteenuse võimsus StorSimple lahenduse võimsus kontekstis.

| Seadme mudelist / maht                         | 8100                                                    | 8600                                                    |
|------------------------------------------------|---------------------------------------------------------|---------------------------------------------------------|
| Arvu kõvaketta draivid (HDD-d).              |   8                                                     |  19                                                     |
| Arvu välkdraivide (SSD).            |   4                                                     |  5                                                      |
| Ühe HDD võimsusega                            |   4 TB                                                  |  4 TB                                                   |
| Ühe SSD võimsus                            |   400 GB                                                |  800 GB                                                 |
| Vaba mahu                                 |   4 TB                                                  | 4 TB                                                    |
| Kasutatav HDD võimsusega                            | 14 TB                                                   | 36 TB                                                   |
| Kasutatav SSD võimsus                            | 800 GB                                                  | 2 TB                                                    |
| Kokku kasutatav võimsus *                         | ~ 15 TB                                                 | ~ 38 TB                                                 |
| Suurim lubatud lahenduse võimsus (sh cloud)    | 200 TB                                                  | 500 TB                                                  |


<sup> * </sup> -  *Kasulikumad koguvõimsus sisaldab andmeid, metaandmete ja buffers.* saadaval võimsus

## <a name="enclosure-dimensions-and-weight-specifications"></a>Ruumi mõõtmed ja kaal tehnilised andmed  

Järgmises tabelis on loetletud eri ruumi kirjeldused mõõtmed ja kaal.  

### <a name="enclosure-dimensions"></a>Ruumi mõõtmed
Järgmises tabelis on loetletud mõõtmed ümbrist millimeetriteks ja tolli.

|Ruumi |Millimeetriteks |Tolli |
|----------|------------|-------| 
| Kõrgus |87,9 | 3.46 |
| Laius üle kinnitusäärikus | 483 | 19.02 |
| Kogu sisu ruumi laius | 443 | 17.44 |
| Sügavus ees kinnitusäärikus otsas ruumi keha abil | 577 | 22.72 |
| Viimasele otsas ruumi sügavust toimingute juhtpaneeli kaudu | 630.5 | 24,82 |
| Sügavus kinnitusäärikus kaugemal otsas ruumi abil |   603 | 23.74 |

### <a name="enclosure-weight"></a>Ruumi Weight (kaal)  

Konfiguratsioonist, täielikult laaditud esmane ruum võib kaaluda 21 33 kg ja nõuab kahte isikud hakkama. 
 
| Ruumi | Weight (kaal) |
|-----------|--------| 
| Suurim lubatud paksus (sõltub konfiguratsiooni) |30 kg 33 kg |
| Tühi (draivid paigaldatud) |21 – 23 kg. |

## <a name="enclosure-environment-specifications"></a>Ruumi keskkonna tehnilised andmed  

Selles jaotises loetletud kirjeldused seotud ruumi keskkonnas. Temperatuur, niiskus, kõrgus, šokk, vibratsiooni, suund, Turve ja elektromagnetilise ühilduvuse (EMC) on kaasatud selle kategooria.  

### <a name="temperature-and-humidity"></a>Temperatuuri- ja

| Ruumi        | Ümbritsev temperatuur vahemikus  | Suhteline õhuniiskus | Suurim lubatud märg pirnid   |
|------------------|----------------------------|---------------------------|--------------------|
| Funktsionaalseid      | 5-35° C (41° F - 95° F)    | 20-80% mitte-kondenseeruv- | 28 OC (82° F)        |
| Mitte-töö  | -C 40-70° (40° F - 158° F) | 5 – 100% mittekondenseeruv  | 29 OC (84° F)        |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Õhuvoolu, kõrgus, šokk, vibratsiooni, paigutust, Turve ja EMC
 
| Ruumi          | Funktsionaalseid tehnilised andmed                                                |
|--------------------|---------------------------------------------------------------------------| 
| Õhuvoolu            | Süsteemi õhuvoolu on ees tahapoole. Süsteemi peab töötama rõhu, tagumine heitgaaside installiga. Tagasi rõhk loodud sektsioon uksed ja takistused ei tohi olla suurem kui 5 paskalit (0,5 mm veesammast). |
| Kõrgus, funktsionaalseid  | meetrit 3045 meetrit-100 kuni 10 000 jalga koos 5 oC kohal 7000 jalad, tühistage hinnatud tegeva maksimum-30. |
| Kõrgus – funktsionaalseid  | -305 meetrit 12,192 meetrit-1,000 kuni 40 000 jalga |
| Šokk, funktsionaalseid  | 5g 10 ms ½ siinuse |
| Šokk-töö  | 30g 10 ms ½ siinuse |
| Vibratsioon, funktsionaalseid  | 0,21 g RMS-i 5-500 Hz juhusliku |
| Vibratsioon-töö  | 1.04g RMS-i 2-200 Hz juhusliku |
| Vibratsioon, ümberpaigutamise  | 3g 2-200 Hz siinuse |
| Suund ja Ava  | 19" sektsioon mount (2 KMH ühikud) |
| Vasturööpad  | Minimaalne 700 mm (31.50 tolli) sügavus mahutamiseks nagid nõuetele vastavust IEC 297 |
| Ohutus- ja kinnituste  |   CE ja UL EN 61000-3, IEC 61000-3, UL 61000-3 |
| EMC  | EN55022 (CISPR – A), FCC A |

## <a name="international-standards-compliance"></a>Rahvusvaheline standardid nõuetele vastavus
Microsoft Azure'i StorSimple seadme vastab järgmistele rahvusvaheline:  

- CE - EN 60950-1  
- CB aruande IEC 60950-1  
- UL ja cUL UL 60950-1  

## <a name="safety-compliance"></a>Turvalisus, nõuetele vastavus  

Microsoft Azure'i StorSimple seadme vastab järgmised turvalisuse hinnangute:  

- Toote tüüp kinnituse: UL cUL CE  
- Turvalisus, nõuetele vastavus: UL 60950, IEC 60950, EN 60950  

## <a name="emc-compliance"></a>EMC nõuetele vastavus 

Microsoft Azure'i StorSimple seadme vastab järgmised EMC hinnangute.  

### <a name="emissions"></a>Heitkoguste

Seade on EMC nõuetele vastavuse läbi ja Õhkuv tasemed.  

- Läbi heitkoguseid piirata tasemed: CFR 47 osa 15B klassi A EN55022 klassi A CISPR klassi A  
- Kiirgusega piirata tasemed: CFR 47 osa 15B klassi A EN55022 klassi A CISPR klassi A   

### <a name="harmonics-and-flicker"></a>Harmoonilised ja Flickris  

Seade vastab EN61000-3-2/3.  

### <a name="immunity-limit-levels"></a>Kaitse limiit tasemed  
Seade vastab EN55024.  

## <a name="ac-power-cord-compliance"></a>AC power kaabli abil nõuetele vastavus
  
Lisandmooduli ja täielik power kaabli abil komplekti peavad vastama vastav riik, kus on kasutusel seadme ning nad peavad olema turvalisuse kinnituste, mis on aktsepteeritav riikides. Järgmises tabelis on loetletud USA ja Euroopa.  

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>AC juhtmed - USA-s (peab olema loetletud NRTL)

| Komponent       | Määratlus                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Kaabli abil tüüp       | SV või SVT 18 AWG miinimum, 3 dirigent, 2.0 meetrit maksimumpikkus |
| Ühendage            | NEMA 5-15P maandus-tüüpi manuse ühendage hinnatud 120 V, 10 A; või IEC 320 C14, 250 V, 10 A |
| Turvasoklite          | IEC 320 C-13, 250 V, 10 A                                         |

### <a name="ac-power-cords---europe"></a>AC juhtmed - Europe

| Komponent       | Määratlus                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Kaabli abil tüüp       | Ühtlustada, H05-VVF-3G1.0                                         |
| Turvasoklite          | IEC 320 C-13, 250 V, 10 A                                         |

## <a name="supported-network-cables"></a>Toetatud võrgukaablid  

10 New York võrgu liidesed andmete 2 ja 3 andmeid, vaadake [loendi toetatud võrgukaableid ja moodulid](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Järgmised sammud

Nüüd olete valmis kasutama oma andmekeskuses StorSimple seadme. Lisateabe saamiseks lugege teemat [seadme kohapealse juurutamine](storsimple-deployment-walkthrough-u2.md).  
