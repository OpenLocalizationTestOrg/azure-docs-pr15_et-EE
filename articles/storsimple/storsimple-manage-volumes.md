<properties
   pageTitle="Teie mahus StorSimple haldamine | Microsoft Azure'i"
   description="Selgitatakse, kuidas lisada, muuta, jälgimine ja kustutada StorSimple mahud ja võrguühenduseta neid vajaduse korral tehke järgmist."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/11/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes"></a>StorSimple halduri teenuse abil saate hallata mahud

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Ülevaade

Selles õppeteemas selgitatakse, kuidas StorSimple halduri teenuse abil luua ja hallata mahud StorSimple seadme ja StorSimple virtuaalse seadme.

StorSimple halduri teenus on Azure klassikaline portaali, mille abil saate hallata oma StorSimple lahenduse ühe web kasutajaliidese kaudu pikendada. Lisaks haldamise maht, saate StorSimple halduri teenuse loomine ja haldamine StorSimple teenuste, vaadata ja seadmete haldamine, teatised, vaadata ja vaadata ja hallata varukoopia poliitika ja varunduse kataloogis.

> [AZURE.NOTE] Azure'i StorSimple saate luua ainult õhukesed ettevalmistatud draividel. Te ei saa luua ettevalmistatud täielikult või osaliselt ettevalmistatud mahud Azure StorSimple abil.
>
> Õhuke ettevalmistamise on virtualization tehnoloogia saadaolevat näib ületada füüsilised ressursid. Asemel broneerimist piisavalt salvestusruumi eelnevalt, Azure StorSimple kasutab õhuke ettevalmistamise eraldada praeguse nõuetele vastavuse lihtsalt piisavalt ruumi. Elastne laadi salvestusruumi hõlbustab see lähenemine, kuna Azure StorSimple saate suurendada või vähendada salvestusruumi muutuvate vajaduste rahuldamiseks.

## <a name="the-volumes-page"></a>Lehe maht

Lehe **maht** võimaldab teil hallata Salvestusruumi mahud, mis on ette valmistatud Microsoft Azure StorSimple seadme jaoks oma algataja (serverid). See kuvab loendi mahud StorSimple seadmes.

 ![Lehe maht](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

Sarja atribuute koosneb draiv:

- **Nimi** – kirjeldav nimi, mis peab olema kordumatu ja aitab tuvastada maht. See nimi on kasutusel järelevalve aruanded, kui valite kindla maht.

- **Olek** – võib olla võrguühenduse lubamine või keelamine. Kui maht, kui ühenduseta, ei kuvata on lubatud juurdepääs kasutada helitugevuse algataja (serverid).

- **Võimsus** – saate määrata, kui suur maht on, nagu tajuvad algataja (server). Võimsus kogusumma andmeid, mida saab salvestada algataja (server), saate määrata. Maht on õhukesed ette valmistatud ja andmed on deduplicated. See tähendab, et teie seade ei eelnevalt eraldada füüsilise mälumaht sees või pilveteenuses vastavalt konfigureeritud maht. Selle maht on eraldatud ja tarbitud nõudmisel.

- **Tippige** – helitugevuse tüüp võib olla astmeline või arhiivimine (mitmetasandilise alamtüüp.)

- **Juurdepääs** – saate määrata algataja (serverid), mis on lubatud juurdepääs sellele draivile. Algataja, et access juhtelement kirje (ACR), mis on seostatud maht ei ei kuvata maht.

- **Jälgimine** – määrab, kas jälgitakse maht. Maht on vaikimisi lubatud, kui see on loodud jälgimine. Jälgimine saavad, kuid, olla keelatud helitugevuse klooni. Luba jälgimine maht, järgige kuvari maht.

Enamlevinud tööülesanded seostatud maht on:

- Lisage maht
- Muutke maht
- Kustutage maht
- Võrguühenduseta maht
- Kuvari maht

## <a name="add-a-volume"></a>Lisage maht

Teie [loodud maht](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) StorSimple lahenduse juurutamisel. Lisamine maht on sarnane protseduur.

### <a name="to-add-a-volume"></a>Lisada maht

1. Klõpsake lehel **seadmed** valige seade, topeltklõpsake seda ja seejärel klõpsake vahekaarti **Helitugevuse ümbriste** .

2. Valige helitugevuse ümbris ja klõpsake vastava rea juurde pääseda seostatud ümbris mahud noolt.

3. Klõpsake lehe allosas **Lisa** . Lisa helitugevuse viisard käivitatud.

     ![Helitugevuse viisardiga põhisätted](./media/storsimple-manage-volumes/AddVolume1.png)

4. Helitugevuse viisardit, klõpsake jaotises **Põhisätted**lisamine tehke järgmist.

  1. Lisage oma helitugevuse **nimi** .
  2. Määrake oma helitugevuse **Ette valmistatud võimsus** GB või TB. Võimsus peab olema 1 GB ja 64 TB füüsiline seade. Suurim lubatud maht, mis saab ette valmistatud StorSimple virtuaalse seadme maht on 30 TB.
  3. Valige oma maht **Kasutamine tüüp** . Kui kasutate astmeline helitugevuse arhiivimine andmed, märkige ruut **selle helitugevuse vähem sageli külastatud arhiivimine andmete jaoks kasutada** muudab korduste eemaldamise kogumi suurust draivi jaoks 512 KB. Kui valite selle suvandi, kasutatakse vastavate astmeline helitugevuse kogumi suurus 64 KB. Korduste eemaldamise kogumi suuremaks võimaldab seadme kiirendamiseks suurte arhiivimine andmete kandmine pilveteenusesse. (Astmeline mahud varem nimetati esmane mahud.)
  5. Nooleikooni ![nooleikooni](./media/storsimple-manage-volumes/HCS_ArrowIcon.png)lehele **Täiendavaid sätteid** .

        ![Helitugevuse viisardi täiendavate sätete lisamine](./media/storsimple-manage-volumes/AddVolume2.png)

5. Klõpsake jaotises **Täpsemad sätted**uue Accessi juhtelement kirje (ACR) lisamiseks tehke järgmist.

  1. Valige loendist drop-down juurdepääsu juhtelement kirje (ACR). Teise võimalusena saate lisada uue ACR. ACRs kindlaks teha, mis hosts pääsevad teie mahus sobitamine host IQN, et loendis kirje.
  2. Soovitame teil lubada vaikimisi varukoopia märkige ruut **Luba selle draivi jaoks vaikimisi varukoopia** .
   3. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-manage-volumes/HCS_CheckIcon.png) helitugevuse loomiseks määratud sätetega.

Teie uus draiv on kasutamiseks valmis.

## <a name="modify-a-volume"></a>Muutke maht

Kui teil on vaja laiendada või hosts, et juurdepääs Helitugevuse muutmiseks, saate muuta maht.

> [AZURE.IMPORTANT]
>
> - Kui seadme helitugevuse suuruse muutmiseks on vaja muuta ka hosti mahu suurus.
> - Host-side siin kirjeldatud juhised kehtivad Windows Server 2012 (2012R2). Linux või muud opsüsteemid host teistsugused. Vaadake oma host operatsioonisüsteemi juhiseid muu opsüsteemiga hosti helitugevuse muutmisel.

### <a name="to-modify-a-volume"></a>Muuta maht

1. Lehel **seadmed** valige seade, topeltklõpsake seda ja seejärel klõpsake vahekaarti **Helitugevuse ümbrises** . Sellel lehel loetletakse tabelina kõik helitugevuse ümbriste, mis on seotud seade.

2. Valige helitugevuse container ja klõpsake seda, et kuvada kõik mahud pakendi sees loendit.

3. Klõpsake lehel **mahud** valida draiv ja klõpsake käsku **Muuda**.

4. Muuda helitugevuse viisardis jaotises **Põhisätted**saate teha järgmist:

  - Kui soovite muuta, valides ruut **Kasuta seda helitugevuse vähem sageli külastatud arhiivimine andmete** korduste eemaldamise kogumi suuruse jaoks oma Helitugevuse muutmiseks 512 KB astmeline helitugevuse arhiivimine maht on redigeerida **nimi** ja **Tüüp** .
  - **Ette valmistatud võimsus**suurendamine Ainult tõsta **Ette valmistatud võimsus** . Te ei saa mahtu vähendada pärast selle loomist.

    > [AZURE.NOTE] Helitugevuse ümbris ei saa muuta, kui see on määratud.

5. Jaotises **Täpsemad sätted**saate teha järgmist:

  - Muutke ACRs, tingimusel, et helitugevus on ühenduseta. Kui maht on veebis, peate esmalt ühenduseta võtta. Vaadake juhiseid [võtta maht ühenduseta](#take-a-volume-offline) enne iga-aastase muutmine.
  - ACRs loendi muutmine, kui maht on ühenduseta.

    > [AZURE.NOTE] Helitugevuse suvandi **lubamiseks vaikimisi varundamise selle maht** ei saa muuta.

6. Muudatuste salvestamiseks klõpsake ikooni Otsi ![sisse-ikoon](./media/storsimple-manage-volumes/HCS_CheckIcon.png). Azure'i klassikaline portaalis kuvatakse meilisõnumi värskendamine helitugevust. See kuvab edu sõnum, kui maht on nüüd värskendatud.

7. Kui on laiendamine maht, täitke järgmised juhised Windowsi hosti arvutis:

   1. Avage **arvuti haldamine** ->**ketta haldus**.
   2. Paremklõpsake **Ketta haldus** ja valige **Ketast uuesti**.
   3. Loendit ketast, valige soovitud maht, mida saate värskendada, paremklõpsake seda ja seejärel valige **Laiendamine helitugevuse**. Käivitub laiendamine helitugevuse viisard. Klõpsake nuppu **edasi**.
   4. Täitke viisardi vastu võtmist vaikeväärtused. Kui viisard on lõpule jõudnud, peaksite Helitugevuse kuva suurema suuruse.

![Video saadaval](./media/storsimple-manage-volumes/Video_icon.png) **Video saadaval**

Vaadake videot, mis näitab, kuidas laiendada maht, klõpsake [siin](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="take-a-volume-offline"></a>Võrguühenduseta maht

Kui peate tegema maht ühenduseta, kui kavatsete seda muuta või kustutada. Kui maht on ühenduseta, pole saadaval lugemis-ja kirjutamisõigused. Peate võtma helitugevuse ühenduseta hosti samuti seadmes. Järgmiste toimingute tegemiseks maht ühenduseta.

### <a name="to-take-a-volume-offline"></a>Kui soovite maht võrguühenduseta

1. Veenduge, et kõnealuse helitugevuse ei kasuta enne ühenduseta.

2. Võtta helitugevuse ühenduseta hosti esimene. See kaob võimalikud ohtu mahu andmete rikkumist. Teatud juhiseid, vaadake juhiseid host operatsioonisüsteem.

3. Pärast selle hosti on ühenduseta, tehke helitugevuse seadme ühenduseta, toimige järgmiselt.

  1. Klõpsake lehel **seadmed** valige seade, topeltklõpsake seda ja seejärel klõpsake vahekaarti **Helitugevuse ümbriste** . **Helitugevuse ümbriste** menüü tabelina on loetletud kõik helitugevuse ümbriste, mis on seotud seade.
  2. Valige helitugevuse container ja klõpsake seda, et kuvada kõik mahud pakendi sees loendit.
  3. Valige maht ning **teha ühenduseta**.
  4. Kinnituse küsimisel nuppu **Jah**. Helitugevuse peaks olema nüüd ühenduseta.

    Kui maht on ühenduseta, **Tuua Online** valik muutub kättesaadavaks.

> [AZURE.NOTE] Käsk **Teha ühenduseta** saadab taotluse seadme võrguühenduseta mahtu. Kui hosts endiselt rakendust maht, tulemuseks katkenud ühendusi, kuid võttes helitugevuse ühenduseta režiimis ei õnnestu.

## <a name="delete-a-volume"></a>Kustutage maht

> [AZURE.IMPORTANT] Saate kustutada maht ainult siis, kui see on ühenduseta.

Järgmiste juhiste kustutamiseks maht.

### <a name="to-delete-a-volume"></a>Kustutamiseks maht

1. Klõpsake lehel **seadmed** valige seade, topeltklõpsake seda ja seejärel klõpsake vahekaarti **Helitugevuse ümbriste** .

2. Valige helitugevuse ümbris, mille maht, mille soovite kustutada. Klõpsake helitugevuse ümbris **mahud** lehele.

3. Kõik selles ümbrises seostatud mahud kuvatakse tabelina. Oleku maht, mille soovite kustutada. Kui maht, mille soovite kustutada pole ühenduseta, võrguühenduseta see esmalt juhistele [võtta maht ühenduseta](#take-a-volume-offline).

4. Pärast maht on ühenduseta, klõpsake lehe allservas **kustutada** .

5. Kinnituse küsimisel nuppu **Jah**. Helitugevuse kustutatakse ja **mahud** leht kuvatakse mahud s.o ümbris, mille jooksul värskendatud loendit.

## <a name="monitor-a-volume"></a>Kuvari maht

Helitugevuse jälgimine võimaldab teil maht ma/O seotud statistika kogumiseks. Jälgimine on vaikimisi esimene 32 maht, mille loote. Vaikimisi on keelatud täiendava jälgimine. Vaikimisi ka keelatud kloonitud mahud jälgimine.

Järgmiste toimingute lubamiseks või keelamiseks maht jälgimine.

### <a name="to-enable-or-disable-volume-monitoring"></a>Lubada või keelata helitugevuse jälgimine

1. Klõpsake lehel **seadmed** valige seade, topeltklõpsake seda ja seejärel klõpsake vahekaarti **Helitugevuse ümbriste** .

2. Valige helitugevuse ümbris, kus asub maht, ja klõpsake helitugevuse container **mahud** lehele.

3. Kõik selles ümbrises seostatud mahud on loetletud tabeli kuvamine. Klõpsake ja valige helitugevuse või helitugevuse klooni.

4. Klõpsake lehe allosas nuppu **Muuda**.

5. Jaotises **Põhisätted**viisardis Helitugevuse muutmiseks valige **lubamine** või **keelamine** **jälgimis** ripploendist.

    ![Muutke maht põhisätted](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)


## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas [klooni StorSimple helitugevust](storsimple-clone-volume.md).

- Siit saate teada, [StorSimple halduri teenuse haldamiseks StorSimple seadme kasutamise](storsimple-manager-service-administration.md)kohta.
