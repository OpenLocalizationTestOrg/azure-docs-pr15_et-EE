<properties 
   pageTitle="Riistvara StorSimple 10 New York liidesed | Microsoft Azure'i"
   description="Seadme StorSimple kirjeldatakse toetatud väikestele vormi-tegur ühendatav (SFP) transiiverite, kaabel ja 10 New York võrgu liidesed võtmed."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="supported-hardware-for-the-10-gbe-network-interfaces-on-your-storsimple-device"></a>Toetatud riistvara 10 New York võrgu liidesed StorSimple seadmes

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse täiendava riistvara, mis töötab Microsoft Azure StorSimple seadme.

## <a name="list-of-devices-tested-by-microsoft"></a>Microsofti testitud seadmete loend

Microsoft on testinud järgmised väikestele vormi-tegur ühendatav (SFP) transiiverite, kaabel ja selleks, et tagada, et nad optimaalselt seadmete parameetrid. (Järgmised tabelid värskendatakse uue riistvara testitakse.)

### <a name="sfp-transceivers"></a>SFP + transiiverite

|Tegemine|Mudel|
|---|---|
|Cisco|SFP-10G-SR|

### <a name="cables"></a>Kaabel

|S. Ei. |Tegemine|Mudel|
|---|---|---|
| 1.|Cisco|SFP-H10GB-CU1M|
| 2.|Cisco|SFP-H10GB-CU2M|
| 3.|Cisco|SFP-H10GB-CU3M|
| 4.|Tripp Lite|N820 - 05M (OM3)|

### <a name="switches"></a>Parameetrid

|S. Ei.|Tegemine|Mudel|
|---|---|---|
| 1. |Cisco|N3K-C3172PQ-10GE|
| 2. |Cisco|N3K C3048-ZM F|
| 3. |Cisco|N5K-C5596UP-FA|

## <a name="list-of-devices-tested-in-the-field"></a>Seadmete loend testitud väli

See jaotis sisaldab seadmetes, mis on edukalt kasutusele võetud väli StorSimple klientide loendi. Need pole testitud Microsoft, kuid tõenäoliselt StorSimple seadme töötamine.
 
| Parameetri                         | Väärtus                                    |
|-----------------------------------|------------------------------------------|
| Üleminek tegemine                     | Kadakas                                  |
| Vaheta mudelit                    | ex4550-32F                               |
| Vaheta operatsioonisüsteemi versiooni | JunOS 12.3R9.4                           |
| Blade mudel                     | Pordid rongis (PIC 0)                    |
| Transiiver tegemine                  | Kadakas                                  |
| Transiiver mudel               | Üksuste arv 740-021308 <br></br> Üksuste arv 740-030658                   |
| Transiiver püsivara versioon    | Rev 01 versioon 0,0 (teatatud)            |
| Kaabel mudel                     | Kahepoolne hüppaja LC/LC 50/125µ, OM3, LSZH |
| StorSimple mudel                | 8600                                     |
| StorSimple tarkvara versioon     | 6.3.9600.17491                           |


## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>Loendi testitakse OEM pakkuja (Mellanox)  

Mellanox on testitud järgmised small vormi-tegur ühendatav (SFP) transiiverite, kaabel ja funktsioon optimaalselt Mellanox võrgu liidesed näiteks 10 New York võrgu liidesed StorSimple seadme abil tagada parameetrid.

### <a name="cables-and-modules-supported-by-mellanox"></a>Kaabel ja moodulid Mellanox ei toeta 

Järgmises tabelis on loetletud kaabel ja moodulid Mellanox ei toeta. Need pole testitud Microsoft, kuid tõenäoliselt StorSimple seadme töötamine.

| S. Ei. | Kiiruse | Mudel                 | Kirjeldus                                            | Tegemine                |
|--------|-------|-----------------------|--------------------------------------------------------|-----------------------|
| 1.     | 10 New York| KABIINI-SFP-SFP - 1M        | passiivne vasest SFP + 10 Gb/s 1m                   | Arista                |
| 2.     | 10 New York| KABIINI-SFP-SFP - 2M        | passiivne vasest SFP + 10 Gb/s 2m                   | Arista                |
| 3.     | 10 New York| KABIINI-SFP-SFP - 3M        | passiivne vasest SFP + 10 Gb/s 3m                   | Arista                |
| 4.     | 10 New York| KABIINI-SFP-SFP - 5M        | passiivne vasest SFP + 10 Gb/s 5m                   | Arista                |
| 5.     | 10 New York| Cisco SFP-H10GBCU1M   | Cisco SFP + kaabel                                       | Cisco                 |
| 6.     | 10 New York| Cisco SFP-H10GBCU3M   | Cisco SFP + kaabel                                       | Cisco                 |
| 7.     |10 New York | Cisco SFP-H10GBCU5M   | Cisco SFP + kaabel                                       | Cisco                 |
| 8.     | 10 New York| J9281B HP X242 10 G    | SFP + SFP + 1m manustamine otse vasest             | HP                    |
| 9.     | 10 New York| 455883-B21 HP BLc     | 10Gb SR SFP + loobumine                                       | HP                    |
| 10.    | 10 New York| 455886-B21 HP BLc     | 10Gb LR SFP + loobumine                                       | HP                    |
| 11.    |10 New York | 487649-B21 HP BLc     | SFP + 0,5 m 10GbE vask kaabel                           | HP                    |
| 12.    |10 New York | 487652-B21 HP BLc     | SFP + 1m 10GbE vask kaabel                             | HP                    |
| 13.    |10 New York | 487655-B21 HP BLc     | SFP + 3m 10GbE vask kaabel                             | HP                    |
| 14.    |10 New York | 487658-B21 HP BLc     | SFP + 7m 10GbE vask kaabel                             | HP                    |
| 15.    |10 New York | 537963-B21 HP BLc     | SFP + 5m 10GbE vask kaabel                             | HP                    |
| 16.    |10 New York | AP784A HP             | 3m C-sarja passiivne vask SFP + kaabel                  | HP                    |
| 17.    |10 New York | AP785A HP             | 5m C-sarja passiivne vask SFP + kaabel                  | HP                    |
| 18.    |10 New York | AP818A HP             | 1m B-sarja aktiivse vask SFP + kaabel                   | HP                    |
| 19.    |10 New York | AP819A HP             | 3m B-sarja aktiivse vask SFP + kaabel                   | HP                    |
| 20.    |10 New York | J9150A HP             | X132 10 G SFP + LC SR transiiver                        | HP                    |
| 21.    |10 New York | J9151A HP             | X132 10 G SFP + LC LR transiiver                        | HP                    |
| 22.    |10 New York | J9283B HP             | X242 10 G SFP + SFP + 3 m DAC kaabel                        | HP                    |
| 23.    |10 New York | J9285B HP             | X242 10 G SFP + SFP + 7 m DAC kaabel                        | HP                    |
| 24.    | 10 New York| JD095B HP             | X240 10 G SFP + SFP + 0,65 m DAC kaabel                     | HP                    |
| 25.    | 10 New York| JD096B HP             | X240 10 G SFP + SFP + 1,2 m DAC kaabel                      | HP                    |
| 26.    | 10 New York| JD097B HP             | X240 10 G SFP + SFP + 3 m isa kaabel                        | HP                    |
| 27.    | 10 New York| MAM1Q00A-sektori Mellanox | QSFP SFP + adapterit                                   | Mellanox tehnoloogiad |
| 28.    | 10 New York| MC2309124-006 Mt      | Passiivne vasest 1 x SFP+ QSFP 10 Gb/s 24awg kuni 7 m   | Mellanox tehnoloogiad |
| 29.    | 10 New York| MC2309124-007 Mt      | Passiivne vasest 1 x SFP+ QSFP 10 Gb/s 24awg kuni 7 m   | Mellanox tehnoloogiad |
| 30.    | 10 New York| MC2309130-003 Mt      | Passiivne vasest 1 x SFP+ QSFP 10 Gb/s 30awg kuni 3 m   | Mellanox tehnoloogiad |
| 31.    | 10 New York| MC2309130-00A Mt      | Passiivne vasest 1 x SFP+ QSFP 10 Gb/s 30awg-0,5 m | Mellanox tehnoloogiad |
| 32.    | 10 New York| MC3309124-005 Mt      | Passiivne vask kaabel 1 x SFP+ 10 Gb/s 24awg 5 m           | Mellanox tehnoloogiad |
| 33.    | 10 New York| MC3309124-007 Mt      | Passiivne vask kaabel 1 x SFP+ 10 Gb/s 24awg 7 m           | Mellanox tehnoloogiad |
| 34.    | 10 New York| MC3309130-003 Mt      | Passiivne vask kaabel 1 x SFP+ 10 Gb/s 30awg 3 m           | Mellanox tehnoloogiad |
| 35.    | 10 New York| MC3309130-00A Mt      | Passiivne vask kaabel 1 x SFP+ 10 Gb/s 30awg 0,5 m         | Mellanox tehnoloogiad |

### <a name="switches-supported-by-mellanox"></a>Mellanox ei toeta parameetrid 

Järgmises tabelis on loetletud parameetreid Mellanox ei toeta. Need pole testitud Microsoft, kuid tõenäoliselt StorSimple seadme töötamine.

| S. Ei. | Kiiruse | Mudel | Kirjeldus                                                         | Tegemine              |
|--------|-------|-------------|---------------------------------------------------------------------|-------------|
| 1.     | 10GbE | 516733-B21  | HP ProCurve 6120XG 10GbE Ethernet tera vahetamine                      | HP    |
| 2.     | 10GbE | 538113-B21  | HP 10GbE läbiv mooduli (PTM)                                  | HP    |
| 3.     | 10GbE | EN4093      | IBM PureFlex süsteemi struktuuri EN4093 10 Gigabit Scalable vahetamise mooduli | IBM   |
| 4.     | 1GbE  | 3020        | Cisco Catalyst 3020 1GbE vahetamise blade                               | Cisco |
| 5.     | 1GbE  | 3020 X       | Cisco Catalyst 3020 X 1GbE vahetamise blade                              | Cisco |
| 6.     | 1GbE  | 438030-B21  | HP 1GbE vahetamise moodul - GbE2c Layer 2/3 Ethernet Blade aktiveerimine       | HP    |
| 7.     | 1GbE  | 6120G       | HP ProCurve 6120G/XG 1GbE vahetamise blade                              | HP    |

## <a name="next-steps"></a>Järgmised sammud

[Lisateavet StorSimple riistavara ja oleku kohta](storsimple-monitor-hardware-status.md).
