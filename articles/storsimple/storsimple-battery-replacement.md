<properties 
   pageTitle="Asendage asuvat StorSimple seadmes | Microsoft Azure'i"
   description="Kirjeldatakse, kuidas eemaldada, asendamine ja varuaku mooduli StorSimple seadme haldamine."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>Asendage varuaku mooduli StorSimple seadmes

## <a name="overview"></a>Ülevaade

Esmane ruumi Power ja jahutus mooduli ühenduse Microsoft Azure'i StorSimple seadmes on mõni lisaakusid keelepaketi. See pakett pakub power StorSimple seadme saate andmed salvestada, kui on AC power esmane ruumi kadu. See akupaketti edaspidi *varuaku mooduli*. Varuaku mooduli esineb üksnes esmane ümbrist StorSimple seadme (ruum EBOD ei sisalda varuaku mooduli). 

Selles õppeteemas selgitatakse, kuidas:

- Varuaku mooduli eemaldamine 
- Uue varuaku mooduli installimiseks
- Varuaku mooduli haldamine

>[AZURE.IMPORTANT] Enne eemaldamine ja asendamine varuaku mooduli, lugege turvalisuse teavet [StorSimple riistvara komponent asendamine tutvustus](storsimple-hardware-component-replacement.md).

## <a name="remove-the-backup-battery-module"></a>Varuaku mooduli eemaldamine

Varuaku mooduli StorSimple seadme on välja asendatav üksus. Enne, kui see on installitud on PCM, aku mooduli salvestatakse selle algse pakendit. Järgmiste toimingute varukoopia asuvat eemaldada.

#### <a name="to-remove-the-backup-battery-module"></a>Varuaku mooduli eemaldamine

1. Azure'i klassikaline portaalis valige **seadmed** > **hooldus** > **Riistvara olek**. Jaotises **Ühiskasutuses komponendid**, vaadake olek asuvat.

2. Leidke PCM, kus asuvat on nurjunud. Joonis 1 näitab StorSimple seadme tagasi.

    ![Backplane seadme esmane ruum moodulid](./media/storsimple-battery-replacement/IC740994.png)

    **Joonis 1** Tagasi, põhiseade nähtaval PCM ja selle domeenikontrolleri moodulid

  	|Sildi|Kirjeldus|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Selle domeenikontrolleri 0|
  	|4|Selle domeenikontrolleri 1|

    Nagu on näidatud joonisel 2 arvuga 3, sisse lülitada kontrolli indikaator LED PCM 0, mis vastab et **Aku viga** .

    ![Backplane seadme PCM jälgimise indikaator LED.](./media/storsimple-battery-replacement/IC740992.png)

    **Joonis 2** Tagasi PCM, kus kontrolli indikaator LED

  	|Sildi|Kirjeldus|
  	|:---|:-----------|
  	|1|Kellaaja|
  	|2|Fänn tõrge|
  	|3|Aku viga|
  	|4|PCM OK|
  	|5|Näiteks Põhiliselt elektrikatkestus|
  	|6|Terve aku|

3. PCM nurjunud isegi eemaldamiseks järgige [eemaldamine on PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).

4. PCM, eemaldatakse, tõstke ja pööramine aku mooduli pidet ülespoole, nagu on näidatud järgmisel joonisel ja tõmmake see kuni Eemalda asuvat.

    ![PCM aku eemaldamine](./media/storsimple-battery-replacement/IC741019.png)

    **Joonis 3** PCM asuvat eemaldamine

5. Asetage mooduli pakendamine väljale Asendatav üksus.

6. Naaske defektne ühik Microsoft proper teenindamine ja.

## <a name="install-a-new-backup-battery-module"></a>Uue varuaku mooduli installimiseks

Järgmiste toimingute asendamine aku mooduli installimiseks PCM esmane ruumi StorSimple seadme sisse.

#### <a name="to-install-the-battery-module"></a>Aku mooduli installimiseks

1. Funktsiooni proper suund soovitud PCM varuaku mooduli paigutada.

2. Vajutage aku mooduli pidet allapoole kuni asukoht konnektor.

3. PCM rakenduses esmane ruumi asendamiseks [asendada Power ja jahutus mooduli StorSimple seadme](storsimple-power-cooling-module-replacement.md)toodud juhised.

4. Kui asendamine on lõpule jõudnud, avage **seadmete** > **hooldus** > **Riistvara oleku** Azure klassikaline portaalis. Kontrollige olek asuvat veendumaks, et installimine õnnestus. Roheline olek on märgitud asuvat terve.

## <a name="maintain-the-backup-battery-module"></a>Varuaku mooduli haldamine

Seadme StorSimple varuaku mooduli pakub power selle domeenikontrolleri ajal power kaotsimineku sündmus. See võimaldab StorSimple seadme enne kontrollitud viisil sulgemise kriitiliste andmete salvestamiseks. Kaks täislaetud akut on PCMs, saate süsteemi hakkama kaks järjestikust kaotsimineku sündmused.

Azure'i klassikaline portaalis **Riistvara oleku** lehe **hooldustööd** näitab, kas asuvat on rikutud või kasutuselt kõrvaldatud on lähenemas. Aku olek on tähistatud **aku PCM 0** või **aku PCM 1** jaotises **Ühiskasutuses komponendid**. Sellel lehel kuvatakse **DEGRADED** riigi jaoks kasutuselt kõrvaldatud lähenemas ja kasutuselt kõrvaldatud **NURJUNUD** jõudnud. 

>[AZURE.NOTE] Asuvat saate teatada **NURJUNUD** , kui seda on vaja lihtsalt võetakse.
 
Kui kuvatakse **DEGRADED** seisund, soovitame toimingu käigus järgmist:

- Süsteemi on kogenud tehtud power kaotsimineku või patareid võib toimuvad perioodilise hoolduse. Jälgige süsteemi 12 tundi enne jätkamist.

    - Kui olek on endiselt **DEGRADED** 12-tunnist pidev ühendus AC power kontrollerid ja PCMs töötab, siis asuvat vaja asendada. Palun asendamine varuaku mooduli [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) .

    - Kui olek muutub OK 12 tunni pärast, asuvat toimib ja seda vaja ainult haldamise eest.

- Kui ei ole on seotud kadu AC power ja selle PCM on sisse lülitatud ja ühendatud AC power, asuvat vaja asendada. [Pöörduge Microsofti tootetoe poole](storsimple-contact-microsoft-support.md) asendamine varuaku mooduli tellida.

>[AZURE.IMPORTANT] Nurjunud asuvat aprilli vastavalt kasutada. 

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet [StorSimple riistvara komponent asendamine](storsimple-hardware-component-replacement.md).
