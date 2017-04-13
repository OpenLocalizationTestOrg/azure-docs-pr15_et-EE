<properties 
   pageTitle="StorSimple seadme sisse või välja lülitada | Microsoft Azure'i"
   description="Selgitab, kuidas uue StorSimple seadme sisselülitamine, lülitage seade, mis oli välja lülitada või see katkes power ja töötab seadme välja lülitada."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/23/2016"
   ms.author="alkohli" />

# <a name="turn-your-storsimple-device-on-or-off"></a>StorSimple seadme sisse- või väljalülitamine 

## <a name="overview"></a>Ülevaade

Microsoft Azure'i StorSimple seadme sulgemise ei pea tavalise kasutamine osana. Siiski peate uuele seadmele või seadme, mis oli välja lülitada sisse lülitada. Üldiselt suletakse on vajalik juhtudel, kus peate nurjunud riistvara asendada, füüsilise üksuse teisaldamine või võtta mõne seadme välja. Selles õppetükis kirjeldatakse nõutav sisselülitamine ja sulgemise StorSimple seadme erinevates stsenaariumides.

Järgmises tabelis on loetletud erinevate stsenaariumide sisselülitamine ja sulgemise StorSimple seadme ja lingid juhistest.

|Stsenaarium|Viide Teemad|
|:-------|:---------------|
|Uue seadme sisselülitamine|[Uue seadme sisselülitamine](#turn-on-a-new-device)<ul><li>[Uue seadme esmane ruumi ainult](#new-device-with-primary-enclosure-only)</li><li>[Uue seadme EBOD ruumi](#new-device-with-ebod-enclosure)</li></ul>|
|Pärast sulgemist seadme sisselülitamine|[Pärast sulgemist seadme sisselülitamine](#turn-on-a-device-after-shutdown)<ul><li>[Esmane ruumi ainult seade](#device-with-primary-enclosure-only)</li><li>[Seadme EBOD ruumi](#device-with-ebod-enclosure)</li></ul>|
|Power kaotsimineku pärast seadet sisselülitamine|[Power kaotsimineku pärast seadet sisselülitamine](#turn-on-a-device-after-a-power-loss)<ul><li>[Esmane ruumi ainult seade](#8100)</li><li>[Seadme EBOD ruumi](#8600)</li></ul>|
|Pärast esmase seadme sisselülitamine ja EBOD ühendus on kadunud|[Lülitage sisse seadme pärast esmast ja EBOD ruumi ühendus on kadunud](#turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost)|
|Sulgege töötab seade|[Töötava seadme väljalülitamine](#turn-off-a-running-device)<ul><li>[Esmane ruumi ainult seade](#8100a)</li><li>[Seadme EBOD ruumi](#8600a)</li></ul>|

## <a name="turn-on-a-new-device"></a>Uue seadme sisselülitamine

Olenevalt sellest, kas seade on mõni 8100 või 8600 mudelit, mis erinevad sisselülitamise StorSimple seadme esimest korda juhised. Funktsiooni 8100 on ühe esmane ruumi, on 8600 on kahe-ruum seadme esmane ruumi ja EBOD ruumi. Üksikasjalikud juhised mõlema asuvad järgmistes lõikudes.

- [Uue seadme esmane ruumi ainult](#new-device-with-primary-enclosure-only)

- [Uue seadme EBOD ruumi](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Uue seadme esmane ruumi ainult

Kui seate mudeli StorSimple 8100 on ühe ruumi seade. Seadme sisaldab liigsete Power ja jahutus moodulid (PCMs). Nii PCMs olema installitud ja ühendatud power eri allikatest kõrge kättesaadavuse tagamiseks.

Järgmiste toimingute abil kaabel seadme Power.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

>[AZURE.NOTE]Täieliku seadme häälestamine ja kaabeldus juhiseid, avage [seadme StorSimple 8100](storsimple-8100-hardware-installation.md)installimiseks. Veenduge, et järgite juhiseid täpselt.

### <a name="new-device-with-ebod-enclosure"></a>Uue seadme EBOD ruumi

StorSimple 8600 mudelil on nii esmane ruumi ja EBOD ruumi. Selleks on vaja olema järjestikune manustatud SCSI (SAS) Ühenduvus ja power koos komplekslõng üksused.

Häälestamisel selle seadme esimest korda, toimingute jaoks SAS kaabeldus esmalt ja seejärel täitke juhised power kaabeldus.

[AZURE.INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

>[AZURE.NOTE]Täieliku seadme häälestamine ja kaabeldus juhiseid, avage [seadme StorSimple 8600](storsimple-8600-hardware-installation.md)installimiseks. Veenduge, et järgite juhiseid täpselt.

## <a name="turn-on-a-device-after-shutdown"></a>Pärast sulgemist seadme sisselülitamine

Olenevalt sellest, kas seade on mõni 8100 või 8600 mudelit, mis erinevad juhised StorSimple seadme sisselülitamine, kui see on suletud. Funktsiooni 8100 on ühe esmane ruumi, on 8600 on kahe-ruum seadme esmane ruumi ja EBOD ruumi.

- [Esmane ruumi ainult seade](#device-with-primary-enclosure-only)

- [Seadme EBOD ruumi](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Esmane ruumi ainult seade

Pärast suletakse, kasutage järgmist StorSimple seadme esmane ruumi ja pole EBOD ruumi sisse lülitada.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Seade on esmane ruumi ainult sisselülitamine

1. Veenduge, et power lülitub sisse mõlemad Power ja jahutus moodulid (PCMs) on väljas. Kui parameetreid pole väljas, pöörata neid olekusse väljas ja oodake, kuni valgustus konfidentsiaalne.

2. Seadme sisselülitamiseks ümberpööramine power parameetrid kohta nii PCMs asendisse Sees. Seadme peaks välja lülitada.

3. Kontrollige, et kontrollida, kas seade on täielikult sisse järgmist:

    1. OK LED nii PCM moodulid on roheline.

    2. Oleku LED nii domeenikontrollerid on ühtlane roheline.

    3. Ühele hulka kuuluvad on sinine LED vilgub, mis näitab, et on aktiivne.

    Kui kõik need tingimused pole täidetud, siis pole seadme terve. Palun [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Seadme EBOD ruumi

Pärast suletakse, kasutage järgmist StorSimple seadme esmane ruumi ja EBOD ruumi sisse lülitada. Juhist iga järjest täpselt nii, nagu on kirjeldatud. Selleks võib põhjustada andmekao.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Esmane ja EBOD ruumi seadme sisselülitamine

1. Veenduge, et EBOD ruumi on ühendatud esmane ruumi. Lisateabe saamiseks lugege teemat [seadme StorSimple 8600 installida](storsimple-8600-hardware-installation.md).

2. Veenduge, et Power ja jahutus moodulid (PCMs) EBOD ja esmane Kirjalisandid on väljas. Kui parameetreid pole väljas, pöörata neid olekusse väljas ja oodake, kuni valgustuse konfidentsiaalne.

3. Sisselülitamiseks EBOD ruumi esimese ümberpööramine power parameetrid kohta nii PCMs asendisse Sees. PCM LED peaks olema roheline. On roheline EBOD kontrolleril LED see üksus on märgitud EBOD ruumi kohta.

4. Esmane ruumi sisselülitamiseks ümberpööramine power parameetrid kohta nii PCMs asendisse Sees. Kogu süsteemi peaks nüüd olema.

5. Veenduge, et SAS LED on roheline, mis tagab, et EBOD ruumi ja esmane ruumi vahelise ühenduse on hea.

## <a name="turn-on-a-device-after-a-power-loss"></a>Power kaotsimineku pärast seadet sisselülitamine

Power katkestuste või katkemise saate sulgeda StorSimple seadme. Power katkestuste võib juhtuda power tarvikute üks või mõlemad power tarvikute. Sõltuvalt sellest, kas seade on 8100 või 8600 mudelit, mis erinevad taastamise juhiseid. Funktsiooni 8100 on ühe esmane ruumi, on 8600 on kahe-ruum seadme esmane ruumi ja EBOD ruumi. Selles jaotises kirjeldatakse taastamine iga stsenaariumi.

- [Esmane ruumi ainult seade](#8100)

- [Seadme EBOD ruumi](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Esmane ruumi ainult seade<a name="8100">

Süsteemi saate jätkata tavapärase toimimise ühele oma power tarvikute power kaotsimineku korral. Siiski seadme kättesaadavuse tagamiseks taastada power power pakkumise nii kiiresti kui võimalik.

Kui seal on power katkestuste või power katkemise kohta nii power tarvikute, süsteem sulgub korrapärase ja kontrollitud viisil. Kui power on taastatud, süsteemi lülitatakse automaatselt sisse.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Seadme EBOD ruumi<a name="8600">

#### <a name="power-loss-on-one-power-supply"></a>Lisage Power kaotsimineku sisse ühe power

Süsteemi saate jätkata tavapärase toimimise ühele oma power tarvikute esmane ruum või EBOD ruum power kaotsimineku korral. Siiski seadme kättesaadavuse tagamiseks Taasta power power tarne nii kiiresti kui võimalik.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Power kaotsimineku kohta nii põhi- ja EBOD Kirjalisandid power teenustega

Kui on power katkestuste või power katkemise kohta nii power tarvikute, EBOD ruumi kohe sulgeda ja esmane ruumi sulgub korrapärase ja kontrollitud viisil. Kui power on taastatud, käivitub seade automaatselt.

Kui power on välja lülitatud käsitsi, siis järgige järgmisi juhiseid power taastamiseks süsteem.

1. Lülitage sisse EBOD ruumi.

2. Kui EBOD ruumi on sisse lülitatud, lülitage esmane ruumi.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Power kaotsimineku mõlemad power teenustega EBOD ruumi kohta

Kui häälestate oma kaabel, veenduge, et selle EBOD on kunagi ühendatud eraldi eraldi Probleemse. Kui EBOD ja esmane ruum ei õnnestu, samal ajal, taastub süsteem.

Kui ainult EBOD ruumi nii power tarvikute nurjub, süsteemi automaatselt ei saa tagasi. Tehke sisselülitamine süsteem ja korralik olukorra taastada, tehke järgmist.

1. Kui esmane ruumi on sisse lülitatud, lülitage nii Power ja jahutus moodulid (PCMs).

2. Oodake mõni minut süsteemi sulgeda.

3. Lülitage sisse EBOD ruumi.

4. Kui EBOD ruumi on sisse lülitatud, lülitage esmane ruumi.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Lülitage sisse seadme pärast esmast ja EBOD ruumi ühendus on kadunud

Kui ühendus katkeb valige domeenikontrolleri ja vastavate EBOD kontrolleril vahel, seadme jätkab tööd. Süsteemi aktiivne domeenikontrolleri ja vastavate EBOD kontrolleril vahelise ühenduse katkemisel Tõrkesiirde peaks toimuma ja seadme peaks jätkama nagu tavaliselt.

Kui nii järjestikune manustatud SCSI (SAS) kaabel on eemaldatud või EBOD ruumi ja esmane ruumi vaheline ühendus katkeb, seade seisma. Selles etapis tehke järgmist.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Pärast seda, kui ühendus on kadunud seadme sisselülitamine

1. Juurdepääs seadme tagasi.

2. Kui EBOD ruumi ja esmane ruumi vahelise ühenduse SAS kaabel on katkenud, kõik SAS sõidurajale LED EBOD ruum on välja lülitatud.

3. Sulgege Power nii jahutus moodulid (PCMs) EBOD ruumi ja esmane ruumi.

4. Oodake, kuni kõik valgustus, nii on Kirjalisandid tagasi välja lülitada.

5. Sisestage SAS kaabel, ja veenduge, et on hea EBOD ruumi ja esmane ruumi vahelise ühenduse.

6. Sisselülitamiseks EBOD ruumi esimese või mõlemad PCM lülitid asendisse Sees.

7. Veenduge, et, märkides ruudu, et rohelise LED on sees on EBOD ruumi.

8. Esmane ruumi sisse lülitada.

9. Veenduge, et, märkides ruudu, et selle domeenikontrolleri roheline LED on sees on esmane ruumi.

10. Veenduge, et EBOD ruumi ühendus esmane ruumi oleks hea, märkides ruudu, et SAS lane LED (neli kaarti EBOD kontrolleril) on kõik sees.

>[AZURE.IMPORTANT] Kui SAS kaabel on defektne või EBOD ruumi ja esmane ruumi vahelise ühenduse on pole hea, kui lülitate süsteemi, läheb taastamise režiimi. Palun sel juhul [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) .

## <a name="turn-off-a-running-device"></a>Töötava seadme väljalülitamine

Töötava StorSimple seade võib tekkida vajadus sulgeda kui see paigutatakse, kasutusest, või on rikutud osa, mis tuleb asendada. Juhised on erinevad olenevalt sellest, kas seade StorSimple on mõni 8100 või 8600 mudelit, mis. Funktsiooni 8100 on ühe esmane ruumi, on 8600 on kahe-ruum seadme esmane ruumi ja EBOD ruumi. Selles jaotises details sulgeda töötab seadme juhiseid.

- [Esmane ruumi seade](#8100a)

- [Seadme EBOD ruumi](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Esmane ruumi seade<a name="8100a"> 

Sulgeda seadme korrektse ja kontrollitud viisil, saate seda teha Azure klassikaline portaali kaudu või Windows PowerShelli kaudu StorSimple. 

>[AZURE.IMPORTANT] Sule töötab seade seadme tagasi power nupu abil.
>
>Enne sulgemist seade, veenduge, et et kõik seadme komponendid on terved. Azure'i klassikaline portaalis, liikuge **seadmete** > **hooldustööde** > **Riistvara oleku**, ja veenduge, et kõik komponendid olekut roheline. See kehtib ainult terve süsteem. Kui süsteemi on suletud, et asendada probleemset osa, kuvatakse nurjunud (punane) või halvenenud vastav komponent **Riistvara oleku**olek (kollane).

Pärast pääsete Windows PowerShelli StorSimple või Azure klassikaline portaali, järgige juhiseid [StorSimple seadme sulgeda](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Seadme EBOD ruumi<a name="8600a">

>[AZURE.IMPORTANT] Enne sulgemist esmane ruumi ja EBOD ruumi, et tagada seadme komponendid terve. Azure'i klassikaline portaalis, liikuge **seadmete** > **hooldus** > **Riistvara oleku**, ja veenduge, et kõik komponendid on terved.

#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Sulgeda EBOD ruumi töötab seade

1. Järgige kõiki loetletud [sulgeda StorSimple seadme](storsimple-manage-device-controller.md#shut-down-a-storsimple-device) jaoks esmane ruumi.

2. Pärast esmane ruum on välja lülitatud, sulgeda soovitud EBOD ümberpööramine välja nii Power ja jahutus mooduli ühenduse parameetrid.

3. Veenduge, et selle EBOD on sulgeda, kontrollige, kas kõik valgustus tagasi EBOD ruumi on välja.

>[AZURE.NOTE] SAS kaabel on kasutatud ruumi EBOD ühenduse esmane ruumi ei saa eemaldada, kuni, kui süsteemi on välja lülitatud.

## <a name="next-steps"></a>Järgmised sammud

Kui teil tekib probleeme, kui sisselülitamine või sulgemise StorSimple seadme [Pöörduge Microsofti tootetoe poole](storsimple-contact-microsoft-support.md) .

