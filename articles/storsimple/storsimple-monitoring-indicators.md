<properties 
    pageTitle="StorSimple jälgimise näidikud | Microsoft Azure'i" 
    description="Kirjeldatakse valgusdioodide (LED) ja jälgida StorSimple seadme kasutatud hüpikteatise alarmid."
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
    ms.date="08/18/2016"
    ms.author="alkohli" />

# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>StorSimple jälgimise näidikute abil saate hallata oma seadmesse   

## <a name="overview"></a>Ülevaade

Seadme StorSimple sisaldab valgusdioodide (LED) ja valveseadmed, et saate jälgida moodulid ja üldine StorSimple seadme olek. Riistvara seadme esmane ruumi ja EBOD ruumi leiate jälgimisega seotud näidikud. Jälgimisega seotud näidikud võib olla LED või hüpikteatise alarmid.

On kolme LED olekus näitab mooduli olekut: vilkuv roheline punane-amber või punane-amber roheline.  

- Roheline LED tähistavad terve tegeva olek.  
- Vilkuv roheline – punane-amber LED jaotiste mittekriitilise tingimused, mis võivad nõuda kasutaja sekkumine olemasolu.  
- Punase-amber LED, et näitab Esita mooduli kriitiline viga.  

Ülejäänud selles artiklis kirjeldatakse mitmesuguste jälgimisega seotud indikaator LED, nende asukohti seadmes StorSimple seadme LED Ühendriigid ja mis tahes seotud hüpikteatise alarmid olek.

## <a name="front-panel-indicator-leds"></a>Paneel indikaator LED

Selle paneel, nimetatakse ka *toimingute paneeli* või *ops paani*, kuvatakse kõik moodulid liitväärtuse olek süsteemi. Selle ees on identne esmane StorSimple ja EBOD ruumi, ja on näidatud allpool.  

   ![Seadme paneel][1]
 
Funktsiooni paneel sisaldab järgmisi näitajaid.  

1. Nupp Vaigista
2. Power indikaator LED (roheline/punane-amber)
3. Mooduli viga näidik viis (punane-amber või välja lülitada)
4. Loogika viga näidik viis (punane-amber või välja lülitada
5. Üksuse ID kuvamine  

Suur vahe on paneel seadme LED ja EBOD ruumi puhul on kuvatud LED-kuval **Süsteemi ühiku koodi** . Vaikimisi üksuse ID, kuvatakse seade on **00**, EBOD ruum kuvatakse vaikimisi üksuse ID on **01**. See võimaldab teil kiiresti eristada EBOD ruumi vahel, kui seade on sisse lülitatud. Kui teie seade on sisse lülitatud, [sisselülitamine uuele seadmele](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) teavet abil eristada seadme EBOD ruumi.  

## <a name="front-panel-led-status"></a>Paneel LED olek  

Järgmise tabeli abil saate tuvastada selle seadme või EBOD ruumi paneel LED tähistatud olek.  

|Süsteemi power | Mooduli viga | Loogika viga | Alarm | Olek|
|-------------|---------------|-----------------|-------|-------|
|Punase-amber | VÄLJALÜLITAMINE     | VÄLJALÜLITAMINE | N/A | AC power kaotsi, töötavad varukoopia power või AC power ja selle domeenikontrolleri moodulid eemaldatud.|
|Roheline | SEES | SEES | N/A | Ops paani power (5s) testida olek|
|Roheline | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE | N/A | Lülitage kõik funktsioonid, mis on hea|
|Roheline | SEES |N/A | PCM viga LED, fänn viga LED | PCM temast fänn viga, temperatuur alla või üle|
| Roheline | SEES | N/A | Mooduli LED  | Selle domeenikontrolleri mooduli temast|
| Roheline | SEES | N/A | N/A | Ruumi loogika viga|
| Roheline | Flash | N/A | Selle domeenikontrolleri mooduli võeti mooduli olek. PCM viga LED, fänn viga LED | Tundmatu kontrolleril mooduli tüüp installitud, I2C siini tõrge, domeenikontrolleri mooduli oluline toote andmete (VPD) konfiguratsiooni tõrge |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Jahutus mooduli (PCM) indikaator LED Power   

Power jahutuse mooduli (PCM) indikaator LED leiate iga PCM moodul esmane ruum või EBOD ruum tagasi. Selles teemas kirjeldatakse, kuidas kasutada järgmist LED StorSimple seadme olekut jälgida.  

- PCM LED esmane ruumi
- PCM LED EBOD ruumi

## <a name="pcm-leds-for-the-primary-enclosure"></a>PCM LED esmane ruumi  

StorSimple seadmel 764W PCM mooduli on täiendavad isegi. Järgmisel joonisel on kujutatud seadme LED paani.  

   ![PCM LED esmane ruumi kohta][2]

LED legend:

1. Kellaaja
2. Fänn tõrge
3. Aku viga
4. PCM OK
5. Näiteks Põhiliselt nurjumise
6. Hea aku  

Funktsiooni PCM oleku on märgitud LED paneel. Seadme PCM LED paani on kuus LED. Neli neid LED power tarnimine ja ventilaator oleku kuvamine Ülejäänud kahest LED näitavad selle PCM varuaku mooduli olek. Järgmistes tabelites abil saate määrata selle PCM olekut.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>PCM näitaja LED toite ja fänn
| Olek | PCM OK (roheline) | AC fail (kollane) | Fänn fail (kollane) | Näiteks Põhiliselt fail (kollane) |
|--------|----------------|-----------------------|------------------|----------------------|
| Pole AC õigus (ruum) | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE|
| Pole AC power (ainult selle PCM) | VÄLJALÜLITAMINE | SEES | VÄLJALÜLITAMINE | SEES |
| AC esitamine PCM sees - OK     | SEES | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE |
| PCM nurjuda (fänn fail) | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE | SEES | N/A |
| PCM viga (üle amp ülepinge üle praeguse) | VÄLJALÜLITAMINE | SEES | SEES | SEES |
| PCM (fänn väljaspool lubatud) | SEES | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE | SEES |
| Valige režiim | Vilkuvad | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE |
| PCM püsivara allalaadimine | VÄLJALÜLITAMINE | Vilkuvad | Vilkuvad | Vilkuvad |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>Varukoopia asuvat PCM näitaja LED  

| Olek | Hea aku (roheline) | Aku viga (amber) |
|--------|----------------------|-----------------------|
| Aku ei esita | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE |
| Aku esitamine ja tasulised | SEES | VÄLJALÜLITAMINE |
| Laadimine- või tühjenemist | Vilkuvad | VÄLJALÜLITAMINE |
| Aku "pehmete" viga (Taastatavad) | VÄLJALÜLITAMINE | Vilkuvad |
| Aku "suur" viga (mitte Taastatavad) | VÄLJALÜLITAMINE | SEES |
| Aku relvadest | Vilkuvad | VÄLJALÜLITAMINE |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>PCM LED EBOD ruumi  

EBOD ruumi on 580W PCM ja pole lisaakusid. PCM paneel EBOD ruumi on indikaator LED ainult power tarvikute ja ventilaatori jaoks. Järgmisel joonisel on need LED.

   ![PCM LED EBOD ruumi kohta][3] 
 
Järgmise tabeli abil saate määravad selle PCM.  

| Olek | PCM OK (roheline) | AC fail (kollane) | Fänn fail (kollane) | Näiteks Põhiliselt fail (kollane) |
|--------|---------------|------------------------|------------------|----------------------|
| Pole AC õigus (ruum) | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE |
| Pole AC power (ainult selle PCM) | VÄLJALÜLITAMINE | SEES | VÄLJALÜLITAMINE | SEES |
| AC esitamine PCM sees – OK | SEES | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE |
| PCM nurjuda (fänn fail) | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE | SEES | X |
| PCM viga (üle amp ülepinge üle praeguse | VÄLJALÜLITAMINE | SEES | SEES | SEES |
| PCM (fänn väljaspool lubatud) | SEES | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE | SEES |
| Valige mudel | Vilkuvad | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE |
| PCM püsivara allalaadimine | VÄLJALÜLITAMINE | Vilkuvad | Vilkuvad | Vilkuvad |

## <a name="controller-module-indicator-leds"></a>Selle domeenikontrolleri mooduli indikaator LED  

Esmane domeenikontrolleri ja EBOD kontrolleril moodulid sisaldab StorSimple seadme LED.   

### <a name="monitoring-leds-for-the-primary-controller"></a>Esmane kontrolleril LED jälgimine
Järgmisel joonisel aitab tuvastada LED esmane kontrolleril. (Kõik komponendid on loetletud abi suund.)  

   ![LED - esmane kontrolleril jälgimine][4]
 
Järgmise tabeli abil saate määratleda, kas selle domeenikontrolleri mooduli töötab õigesti.  

### <a name="controller-indicator-leds"></a>Selle domeenikontrolleri indikaator LED  

| LED | Kirjeldus                                                                            
|---- | ----------- |
| ID LED (sinine) | Näitab, et mooduli on määratletud. Kui sinine LED vilgub töötava kontrolleril, on aktiivne domeenikontrolleri ja teine on valige kontrolleril. Lisateabe saamiseks vt [tuvastamine aktiivne kontrolleril seadmes](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Viga LED (amber) | Näitab veaks selle domeenikontrolleri.        
| OK LED (roheline) | Roheline näitab on OK. Roheline vilkuv näitab on selle domeenikontrolleri VPD konfiguratsiooni tõrge. |
| SAS tegevuse LED (roheline) | Roheline näitab ühenduse pole praeguse töö. Vilkuv roheline näitab, et ühendus on pidev tegevus. |
| Ethernet olek LED | Paremal pool näitab link/võrgu tegevust: (roheline) link aktiivne, (vilkuvad roheline) võrgu tegevust. Vasakul küljel näitab võrgu kiiruse: (kollane) 1000 Mb/s, (roheline) 100 Mb/s ja (väljas) 10 Mb/s. Komponent mudelist, võib see hele vilgub ka siis, kui võrgu liidese pole lubatud. |
| POSTITUSE LED | Näitab buutimine edenemist, kui selle domeenikontrolleri on sisse lülitatud. Kui StorSimple seade ei käivitu, aitab see LED Microsoft Support tuvastada, kus ilmnes tõrge buutimine protsessi punkti. |

>[AZURE.IMPORTANT] 
Kui viga LED põleb, on probleem kontrolleril moodul, mis võib lahendada selle domeenikontrolleri taaskäivitada. Kui selle domeenikontrolleri taaskäivitamine probleemi ei lahenda, pöörduge Microsoft Support.  


### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>Jälgimine LED EBOD (ruum EBOD)  

Iga hulka kuuluvad selle 6 Gb/s SAS EBOD on LED, mis näitab selle olek, nagu on näidatud järgmisel joonisel.  

  ![LED - EBOD ruum jälgimine][5]

Järgmise tabeli abil saate määratleda, kas EBOD kontrolleril mooduli töötab normaalselt.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD kontrolleril mooduli indikaator LED  

|Olek | Mooduli OK (roheline) | I/O mooduli viga (amber) | Host pordi tegevuse (roheline) |
|-------|----------------------|-------------------------------|----------------------------|
| Selle domeenikontrolleri mooduli OK | SEES | VÄLJALÜLITAMINE | - |
| Selle domeenikontrolleri mooduli viga | VÄLJALÜLITAMINE | SEES | - |
| Välise host ühendust pole | - | - | VÄLJALÜLITAMINE |
| Välise host ühendust – pole tegevus | - | - | SEES |
| Välise host ühendust - tegevus | - | - | Vilkuvad |
| Selle domeenikontrolleri mooduli metaandmete tõrge | Vilkuvad | - | - |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Esmane ruumi ja EBOD ruumi kettale indikaator LED

StorSimple seadmel kettadraivide asuvad nii esmane ruumi ja EBOD ruumi. Iga kettale sisaldab jälgimise indikaator LED, nagu on kirjeldatud selles jaotises. 

Kettadraivide, ketas olek on tähistatud rohelise LED ja punane-amber LED paigaldatud ketas carrier mooduli ees. Järgmisel joonisel on need LED.

  ![Kettale LED][6]
 
Järgmise tabeli abil saate määratleda iga kettale, mis omakorda mõjutab üldist esi LED oleku olek.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>EBOD ruumi kettale näitaja LED  

| Olek | Tegevuste OK LED (roheline) | Viga LED (punane amber) | Seotud ops paani LED |
|-------|--------------------------|----------------------|-------------------------|
| Pole installitud ketas | VÄLJALÜLITAMINE | VÄLJALÜLITAMINE | Ükski |
| Ketas installitud ja funktsionaalseid | Tegevuste vilkuvad sisse-ja väljalülitamine | X | Ükski |
| SCSI ruum (SES) seadme identiteedi määramine | SEES | Vilkuvad 1 teise kohta/1 teise väljalülitamine | Ükski |
| SES seadme viga bitine määramine | SEES | SEES | Loogika viga (punane) |
| Ringi elektrikatkestus juhtelement | VÄLJALÜLITAMINE | SEES | Mooduli viga (punane) |

## <a name="audible-alarms"></a>Heli alarmid  

StorSimple seadmel hüpikteatise alarmid seostatud nii esmane ruumi ja EBOD ruumi. Helisignaal asub nii Kirjalisandid esi paneeli (tuntud ka kui ops paneel). Funktsiooni valgussignaal näitab, kui esineb viga tingimus. Järgmised tingimused on aktiveerib selle:  

- Fänn viga või nurjumise
- Pinge vahemikust
- Temperatuur alla või üle
- Termilise alistamine
- Süsteemi viga
- Loogika viga
- Viga
- Mingisse astmesse jahutus mooduli (PCM) eemaldamine  

Järgmises tabelis kirjeldatakse mitmesuguste alarmi olekus.  

### <a name="alarm-states"></a>Alarm Ühendriigid  

| Alarm olek | Toiming | Toimingute all nuppu Vaigista |
|------------|---------|---------------------------------|
| S0 | Tavaline režiim: vaikne | Kaks korda piiks |
| S1 | Viga režiimi: 1 teise kohta/1 teise väljalülitamine | Üleminek S2 või S3 (vt märkmed) |
| S2 | Meelde tuletada režiimi: vahelduva piiks | Ükski |
| S3 | Vaigistatud režiimi: vaikne | Ükski |
| S4 | Kriitiline viga režiimi: pidev alarm | Pole saadaval: Vaigista pole aktiivne |

> [AZURE.NOTE] 

>  - Alarmi olekus S1 vajutamisel ei Vaigista kahe minuti olek automaatselt üleminekud S2 või S3.  
>  - S4 alarm riikide S1 naaske S0 pärast viga tingimus on tühi.  
>  - Kriitiline viga olekus S4 saab sisestada teistele.  

Saate vaigistada, vajutades Vaigista ops paanil soovitud valgussignaal. Automaatse vaigistamise toimub kahe minuti pärast kui vaigistamine parameetrit on käsitsi. Kui alarm on välja lülitatud, jätkatakse heli lühike vahelduva piiks näitamaks, et probleem on endiselt olemas. Alarmi saab vaikne, kui kõik probleemid on tühjad.  

Järgmine tabel kirjeldab erinevaid alarm tingimusi.  

### <a name="alarm-conditions"></a>Alarm tingimused  

| Olek | Raskusaste | Alarm | Ops paneeli LED |
|--------|---------|--------|----------------|
| PCM teatis – näiteks Põhiliselt power kaudu ühe PCM kadu | Viga – kaotamata koondamine | S1 | Mooduli viga|
|PCM teatis – näiteks Põhiliselt power kaudu ühe PCM kadu | Viga – kaotsimineku koondamine | S1 | Mooduli viga |
| PCM fänn fail | Viga – kaotsimineku koondamine | S1 | Mooduli viga |
| SBB mooduli tuvastatud PCM viga | Viga | S1 | Mooduli viga |
| PCM eemaldatud | Konfiguratsiooni tõrge | Ükski | Mooduli viga |
| Ruumi konfiguratsiooni tõrge | Viga – kriitiline | S1 | Mooduli viga |
| Väike hoiatus temperatuur teatis | Hoiatus | S1 | Mooduli viga |
| Kõrge hoiatus temperatuur teate | Hoiatus | S1 | Mooduli viga |
| Temperatuuri alarm üle | Viga – kriitiline | S1 | Mooduli viga |
| I2C siini tõrge | Viga – kaotsimineku koondamine | S1 | Mooduli viga |
| Ops paneeli ilmnes (I2C) | Viga – kriitiline     | S1 | Mooduli viga |
| Selle domeenikontrolleri tõrge | Viga – kriitiline | S1 | Mooduli viga |
| SBB kasutajaliidese mooduli viga | Viga – kriitiline | S1 | Mooduli viga |
| SBB kasutajaliidese mooduli viga – pole allesjäänud toimiva moodulid | Viga – kriitiline | S4 | Mooduli viga |
| SBB Kasutajaliidese moodul eemaldatud | Hoiatus | Ükski | Mooduli viga |
| Ketas power juhtelemendi viga | Hoiatus – ketas power kaotamata | S1 | Mooduli viga |
| Ketas power juhtelemendi viga | Viga – kriitiline; ketas power kadu | S1 | Mooduli viga |
| Draiv eemaldatud | Hoiatus | Ükski | Mooduli viga |
| Seadme ja ei ole saadaval | Hoiatus | ükski | Mooduli viga |

## <a name="next-steps"></a>Järgmised sammud

Lisateave [StorSimple riistavara ja olek](storsimple-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png

 
