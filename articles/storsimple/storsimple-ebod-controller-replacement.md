<properties 
   pageTitle="Asendage StorSimple EBOD kontrolleril | Microsoft Azure'i"
   description="Selgitab, kuidas eemaldada ja asendada ühe või mõlema EBOD kontrollerid StorSimple 8600 seadmes."
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

# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Asendage mõne EBOD kontrolleril StorSimple seadmes

## <a name="overview"></a>Ülevaade

Selle õpetuse selgitab, kuidas asendada vigane EBOD kontrolleril mooduli Microsoft Azure'i StorSimple seadmes. Mõne EBOD kontrolleril mooduli asendamiseks peate:

- Vigane EBOD kontrolleril eemaldamine
- Uue EBOD kontrolleril installimine

Enne alustamist, võtke arvesse järgmist.

- Tühi EBOD moodulid tuleb lisada kõik kasutamata slots. Kui pesa on avatud, mitte lahedad ruumi õigesti.

- EBOD on kuum kiirvahetuse ja saab eemaldada või asendada. Eemaldada nurjunud mooduli, kuni olete asendamine. Kui annate asendamine protsessi, peate selle 10 minuti jooksul lõpule.

>[AZURE.IMPORTANT] Enne, kui proovite eemaldada või mis tahes StorSimple komponent asendada, veenduge, et vaadata [Turvalisus ikooni põhimõtted](storsimple-safety.md#safety-icon-conventions) ja muud [ettevaatusabinõusid](storsimple-safety.md).

## <a name="remove-an-ebod-controller"></a>Mõne EBOD kontrolleril eemaldamine

Enne asendades nurjunud EBOD kontrolleril mooduli StorSimple seadme, veenduge, et muude EBOD kontrolleril mooduli on aktiivne ja käivitatud. Järgmine kord ja tabelis selgitatakse, kuidas eemaldada EBOD kontrolleril mooduli.

#### <a name="to-remove-an-ebod-module"></a>Mõne EBOD mooduli eemaldamine

1. Avage Azure'i klassikaline portaal.

2. Liikuge **seadmete** > **hooldus** > **Riistvara oleku**, ja veenduge, et aktiivne EBOD kontrolleril mooduli LED olek on roheline ja nurjunud EBOD kontrolleril mooduli LED on punane.

3. Otsimine nurjunud EBOD kontrolleril mooduli taga seade.

4. Eemaldage kaabel, mis EBOD mooduli süsteemist välja enne selle domeenikontrolleri EBOD kontrolleril mooduli ühenduse.

5. Kirjutage täpselt SAS pordi EBOD kontrolleril moodul, mis on ühendatud selle domeenikontrolleri. Pärast mooduli EBOD asendate selle konfiguratsiooni süsteemi taastamiseks tuleb teil. 

    >[AZURE.NOTE] Tavaliselt on see Port A, mis on sildistatud kui **Host** järgmisel diagrammil.

    ![Selle domeenikontrolleri EBOD backplane.](./media/storsimple-ebod-controller-replacement/IC741049.png)

     **Joonis 1** EBOD mooduli tagakülg

  	|Sildi|Kirjeldus|
  	|:----|:----------|
  	|1|Viga LED|
  	|2|Power LED|
  	|3|SAS ühendused|
  	|4|SAS LED|
  	|5|Järjestikune pordid factory kasutamiseks|
  	|6|Port (hosti sisse)|
  	|7|Pordi B (hosti välja)|
  	|8|Pordi C (ainult Factory kasutamine)|

## <a name="install-a-new-ebod-controller"></a>Uue EBOD kontrolleril installimine

Järgmine kord ja tabelis selgitatakse, kuidas StorSimple seadme mõne EBOD kontrolleril mooduli installimiseks.

#### <a name="to-install-an-ebod-controller"></a>Installimine on EBOD domeenikontrolleri

1. Kontrollige seadme EBOD kahju, eriti kasutajaliidese konnektor. Installige uue EBOD domeenikontrolleri, kui kõik kontaktid ei ole muljutud.

2. Koos lingid avatud, lükake mooduli ruumi, kuni soovitud sulgurid kaasata.

    ![Installimise EBOD domeenikontrolleri](./media/storsimple-ebod-controller-replacement/IC741050.png)

    **Joonis 2**  EBOD kontrolleril mooduli installimist

3. Sulgege soovitud lukustatakse. Kuulete ühe klõpsuga nagu soovitud lukustatakse tegeleb.

    ![EBOD lukustatakse vabastamine](./media/storsimple-ebod-controller-replacement/IC741047.png)

    **Joonis 3**  EBOD mooduli lukustatakse sulgemine

4. Ühendage kaabel on. Kasutage täpse konfiguratsiooni, mis oli enne asendamine. Järgmine diagramm ja tabel soovitud kaabel ühenduse loomise kohta vt.

    ![Kaabel seadme 4U Power](./media/storsimple-ebod-controller-replacement/IC770723.png)

    **Joonis 4**. Kaabel taastamine

  	|Sildi|Kirjeldus|
  	|:----|:----------|
  	|1|Esmane ruumi|
  	|2|PCM 0|
  	|3|PCM 1|
  	|4|Selle domeenikontrolleri 0|
  	|5|Selle domeenikontrolleri 1|
  	|6|Selle domeenikontrolleri EBOD 0|
  	|7|Selle domeenikontrolleri EBOD 1|
  	|8|EBOD ruumi|
  	|9|Power jaotuse ühikud|

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet [StorSimple riistvara komponent asendamine](storsimple-hardware-component-replacement.md).
