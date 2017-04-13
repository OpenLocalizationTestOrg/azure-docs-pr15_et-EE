<properties 
   pageTitle="Asendage PCM, StorSimple seadmes | Microsoft Azure'i"
   description="Selgitab, kuidas eemaldada ja asendada Power ja jahutus mooduli ühenduse StorSimple seadmes"
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Asendage Power ja jahutus mooduli StorSimple seadmes

## <a name="overview"></a>Ülevaade

Power ja jahutus mooduli ühenduse oma Microsoft Azure'i StorSimple seadme koosneb pakkumise ja ventilaatorid, mis kontrollivad esmast ja EBOD Kirjalisandid kaudu. On ainult üks mudel PCM, mis on sertifitseeritud iga ruumi. Esmane ruumi on sertifitseeritud 764 W PCM ja EBOD ruumi on sertifitseeritud 580 W PCM. Kuigi erinevad PCMs esmane ruumi ja EBOD ruumi, selle protseduuri on identne.

Selles õppeteemas selgitatakse, kuidas:

- Mõne PCM eemaldamine
- Installige PCM asendamine

>[AZURE.IMPORTANT] Enne eemaldamine ja asendamisel on PCM, vaadake turvalisuse [StorSimple riistvara komponent asendamine](storsimple-hardware-component-replacement.md).

## <a name="before-you-replace-a-pcm"></a>Enne kui saate asendada ka PCM

Arvestage järgmisi olulisi küsimusi enne asendate oma PCM:

- Funktsiooni PCM power osutamine nurjumisel jätta vigase mooduli installitud, kuid eemaldada kohta. Ventilaator jätkab power saadud ruumi ja proper jahuta jätkata. Kui ventilaator nurjub, kuvatakse PCM vaja asendada kohe.

- Enne selle PCM eemaldamist katkestada power PCM väljalülitamine peamist parameetrit (kui see on olemas) või füüsilise eemaldamise kohta. See pakub hoiatus, et teie süsteemi, et power arvuti ei vahetu.

- Veenduge, et muud PCM on otstarbekas enne asendades vigase PCM jätkuva süsteemi toimingu jaoks. Vigase PCM asendatakse abil on täielikult nii kiiresti kui võimalik.

- PCM mooduli asendamine kulub vaid mõni minut, kuid see peab olema lõpetatud 10 minuti eemaldamine nurjunud PCM ülekuumenemise vältimiseks.

- Pange tähele, et selle Factory tarniti asendamine 764 W PCM moodulid ei sisalda varuaku mooduli. Peate oma vigane PCM küljest eemaldada, ja seejärel sisestage see asendamine sooritamiseks väljavahetamine mooduli. Lisateavet leiate teemast [eemaldamine](storsimple-battery-replacement.md)ja lisamine varuaku mooduli.


## <a name="remove-a-pcm"></a>Mõne PCM eemaldamine

Kui olete valmis Power ja jahutus mooduli ühenduse eemaldamiseks Microsoft Azure StorSimple seadme, järgige neid juhiseid.

>[AZURE.NOTE] Enne oma PCM eemaldamist veenduge, et on õige lugemisprogrammi (764 W esmane ruumi jaoks) või 580 W EBOD ruumi jaoks.

#### <a name="to-remove-a-pcm"></a>Mõne PCM eemaldamine

1. Azure'i klassikaline portaalis **seadmete**nuppu > **hooldus** > **Riistvara olek**. Jaotises **Ühiskasutuses komponentide** tuvastamiseks, mis ei ole PCM PCM komponentide oleku kontrollimine

     - Kui ei ole pakkumise sisse PCM 0, olekut **Power pakkumise PCM 0** on punane.

     - Kui ei ole pakkumise PCM 1, olekut **Power esitama PCM 1** on punane.

     - Kui ei ole fänn PCM 1, oleku **jahutus PCM 0 0** või **1 jahutus PCM 0** on punane.

2. Otsimine nurjunud PCM esmane ruumi tagasi. Kui teil on 8600 mudelit, mis tuvastada esmane ruumi, vaadates süsteemi üksuse ID Number kuvatakse ees paani LED kuvamine. Üksuse ID kuvatakse esmane ruumi vaikimisi on **00**, üksuse ID EBOD ruum kuvatakse vaikimisi on **01**. Järgmine diagramm ja tabel selgitavad front paani LED ekraani.

    ![Süsteemi ID OPS paneel](./media/storsimple-power-cooling-module-replacement/IC740991.png)

     **Joonis 1** Seadme paneel  

  	|Sildi|Kirjeldus|
  	|:---|:-----------|
  	|1|Nupp Vaigista|
  	|2|Süsteemi power|
  	|3|Mooduli viga|
  	|4|Loogika viga|
  	|5|Üksuse ID kuvamine|

3. Kontrolli indikaator LED taga esmane ruumi saate kasutada ka vigase PCM tuvastamiseks. Järgmine diagramm ja tabel aru saada, kuidas kasutada LED vigase PCM leidmiseks vt. Näiteks, kui vastav **Fänn nurjuda** LED põleb, ventilaator on nurjunud. Samuti kui vastav **AC Fail** LED põleb power pakkumise nurjus. 

    ![Backplane PCM kontrolli indikaator seadme LED.](./media/storsimple-power-cooling-module-replacement/IC740992.png)

     **Joonis 2** PCM indikaator LED tagakülg

  	|Sildi|Kirjeldus|
  	|:---|:-----------|
  	|1|Kellaaja|
  	|2|Fänn tõrge|
  	|3|Aku viga|
  	|4|PCM OK|
  	|5|Näiteks Põhiliselt elektrikatkestus|
  	|6|Terve aku|

4. Vaadake tagasi StorSimple seadme otsimine nurjunud PCM mooduli järgmisel joonisel. PCM 0 on vasakul ja paremal on PCM 1. Tabel, mille järgneb selgitatakse moodulid.

     ![Backplane seadme esmane ruum moodulid](./media/storsimple-power-cooling-module-replacement/IC740994.png)

     **Joonis 3** Lisandmooduli moodulid seadme tagakülg 

  	|Sildi|Kirjeldus|
  	|:---|:-----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Selle domeenikontrolleri 0|
  	|4|Selle domeenikontrolleri 1|

5. Vigase PCM välja lülitada ja ühenduse katkestamine power pakkumise kaabli abil. Nüüd saate selle PCM eemaldada.

6. Funktsiooni lukustatakse ja PCM pidet vahel oma nimetissõrmega servas ja pigistada neid koos avamiseks pidet.

    ![Avamine PCM pidet](./media/storsimple-power-cooling-module-replacement/IC740995.png)

    **Joonis 4** PCM pidet avamine

7. Haarake pidet ja selle PCM eemaldada.

    ![Seadme PCM eemaldamine](./media/storsimple-power-cooling-module-replacement/IC740996.png)

    **Joonis 5** Funktsiooni PCM eemaldamine

## <a name="install-a-replacement-pcm"></a>Installige PCM asendamine

Järgige neid juhiseid installimiseks on PCM StorSimple seadme. Veenduge, et sisestasite varuaku mooduli enne installimist asendamine PCM (kehtib ainult 764 W PCMs). Lisateavet leiate teemast [eemaldamine](storsimple-battery-replacement.md)ja lisamine varuaku mooduli.

#### <a name="to-install-a-pcm"></a>Installimine on PCM

1. Veenduge, et teil on õige asendamine PCM selle ruumi. Esmane ruumi 764 W PCM ning EBOD ruum 580 W PCM. Te ei peaks proovida kasutada 580 W PCM esmane ruumi sisse- või 764 W PCM EBOD ruumi sisse. Järgmisel pildil on kujutatud tuvastada silt, mis on kinnitatud selle PCM kohta.

    ![Seadme PCM silt](./media/storsimple-power-cooling-module-replacement/IC740973.png)

    **Joonis 6** PCM silt

2. Otsige kahju ruumi, pöörates erilist tähelepanu konnektorid. 
                                        
    >[AZURE.NOTE] **Installige mooduli, kui mis tahes konnektori kontaktid kõverdatud.**

3. Avatud PCM pidet, kus mooduli lükake ruumi.

    ![Seadme PCM installimine](./media/storsimple-power-cooling-module-replacement/IC740975.png)

    **Joonis 7** Installimine on PCM

4. Sulgege käsitsi PCM pidet. Kuulete ühe klõpsuga nagu pidet lukustatakse tegeleb. 
                                        
    >[AZURE.NOTE] Tagamaks, et konnektori kontaktid on hõivatud, te saate kergelt tug pidet klõpsake soovitud lukustatakse vabastamata. Kui soovitud PCM slaidid välja, see tähendab, et selle lukustatakse suleti enne konnektorid.

5. Ühendage kaabel power power allikas ja selle PCM.

6. Turvaline on pingutamist maksuvabastust pallideks. 

7. Funktsiooni PCM sisse lülitada.

8. Veenduge, et asendamine õnnestus: Azure'i klassikaline portaalis teie haldur StorSimple teenuse liikuge **seadmed** > **hooldustööd** > **Riistvara olek**. Jaotises **Ühiskasutuses komponentide**oleku soovitud PCM peaks olema roheline. 
                                        
    >[AZURE.NOTE] Võib kuluda mõni minut asendamiseks PCM täielikult lähtestada.

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet [StorSimple riistvara komponent asendamine](storsimple-hardware-component-replacement.md).
