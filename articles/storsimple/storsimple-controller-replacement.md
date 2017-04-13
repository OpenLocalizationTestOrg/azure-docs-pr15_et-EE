<properties 
   pageTitle="Asendage StorSimple seadme kontrolleril | Microsoft Azure'i"
   description="Selgitab, kuidas eemaldada ja asendada ühe või mõlema kontrolleril moodulid StorSimple seadmes."
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

# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Selle domeenikontrolleri mooduli StorSimple seadme asendamine

## <a name="overview"></a>Ülevaade

Selle õpetuse selgitab, kuidas eemaldada ja asendada ühe või mõlema kontrolleril moodulid StorSimple seadme. See käsitletakse ka ühe- ja selle domeenikontrolleri asendamine stsenaariumid loogika.

>[AZURE.NOTE] Selle domeenikontrolleri asendamine sooritamiseks soovitame alati värskendada oma kontrolleril püsivara uusim versioon.
>
>Vältimaks StorSimple seadme, mitte dokkida selle domeenikontrolleri kuni LED on nähtavad ühte järgmistest:
>
>- Kõik valgustus on väljas.
>- LED 3, ![roheline märge ikoon](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), ja ![punane rist ikooni](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) Vilkuv, ja LED 0 ja LED 7 on **sees**.

Järgmises tabelis on näidatud toetatud kontrolleril asendamine stsenaariumi.

|Juhul|Asendamine stsenaarium|Rakendatav toiming|
|:---|:-------------------|:-------------------|
|1|Üks on tõrkeolekus, muud kontrolleril terve ja aktiivne.|[Ühe domeenikontrolleri väljavahetamine](#replace-a-single-controller), mis kirjeldab [loogika ühe domeenikontrolleri asendamine](#single-controller-replacement-logic)ning [asendamine juhiseid](#single-controller-replacement-steps).|
|2|Nii kontrolöride ei ole ja asendada. Raami ketast, and.disk ruum on terve.|[Kahekordne kontrolleril väljavahetamine](#replace-both-controllers), mis kirjeldab [loogika kahekordne kontrolleril asendamine](#dual-controller-replacement-logic)ning [asendamine juhiseid](#dual-controller-replacement-steps). |
|3|Vahetatud kontrollerid samasse seadmesse või erinevatest seadmetest. Raami, ketast ja ketta Kirjalisandid on terve.|Kuvatakse pesa lahknevuse hoiatusteate.|
|4|Üks on puudu ja muud kontrolleril nurjub.|[Kahekordne kontrolleril väljavahetamine](#replace-both-controllers), mis kirjeldab [loogika kahekordne kontrolleril asendamine](#dual-controller-replacement-logic)ning [asendamine juhiseid](#dual-controller-replacement-steps).|
|5|Ühe või mõlema kontrollerid nurjus. Kasutatav seade ei pääse järjestikune konsooli või remoting Windows PowerShelli kaudu.|Käsitsi kontrolleril asendamine protseduuri [Pöörduge Microsofti tootetoe poole](storsimple-contact-microsoft-support.md) .|
|6|Kontrolörid on erinevate koostamine versioon, mis võib olla tingitud.<ul><li>Kontrollerid on erinevate tarkvara versiooni.</li><li>Kontrollerid on eri püsivara versiooni.</li></ul>|Kui selle domeenikontrolleri tarkvara versioonid on erinevad, asendamine loogika mis tuvastab ja värskendab tarkvara versiooni kontrolleril asendamine.<br><br>Kui selle domeenikontrolleri püsivara versioonid on erinevad ja vana püsivara versioon **ei** automaatselt täiendatavate, hoiatusteade kuvatakse Azure klassikaline portaalis. Peaksite värskendusi ja installige värskendused püsivara.</br></br>Kui selle domeenikontrolleri püsivara versioonid on erinevad ja vana püsivara versioon on automaatselt täiendatavate, domeenikontrolleri asendamine loogika tuvastab see ja pärast selle domeenikontrolleri käivitub, püsivara värskendatakse automaatselt.|

Peate eemaldama kontrolleril mooduli, kui see on andnud. Ühe või mõlema kontrolleril moodulid võib nurjuda, mille tulemuseks ühe domeenikontrolleri asendamine või kahekordne kontrolleril asendamine. Asendamine toimingute ja nende loogika, vaadake järgmist:

- [Ühe domeenikontrolleri asendamine](#replace-a-single-controller)
- [Nii kontrollerid asendamine](#replace-both-controllers)
- [Mõne selle domeenikontrolleri eemaldamine](#remove-a-controller)
- [Mõne selle domeenikontrolleri lisamine](#insert-a-controller)
- [Aktiivse domeenikontrolleri seadme tuvastamine](#identify-the-active-controller-on-your-device)

>[AZURE.IMPORTANT] Enne eemaldamine ja asendamisel on selle domeenikontrolleri, vaadake ohutus [StorSimple riistvara komponent asendamine](storsimple-hardware-component-replacement.md).

## <a name="replace-a-single-controller"></a>Ühe domeenikontrolleri asendamine

Kui üks hulka kuuluvad on kaks Microsoft Azure StorSimple seadme on nurjunud, on rikutud või puudub, peate asendada ühe domeenikontrolleri. 

### <a name="single-controller-replacement-logic"></a>Ühe domeenikontrolleri asendamine loogika

Ühe domeenikontrolleri väljavahetamine, tuleks esmalt eemaldama nurjunud kontrolleril. (Ülejäänud kontrolleril seadmes on aktiivne kontrolleril.) Asendamine domeenikontrolleri lisamisel tekkida järgmisi toiminguid.

1. Asendamine kontrolleril algab kohe suhtlemine StorSimple seade.

2. Aktiivse kontrolleril virtuaalse kõvaketta (VHD) hetktõmmise kopeeritakse kontrolleril asendamine.

3. Hetktõmmis on muudetud nii, et asendamine domeenikontrolleri käivitamisel: see VHD tuvastatakse valige kontrolleril nimega.

4. Kui soovitud muudatused on lõpetatud, hakkavad asendamine kontrolleril nimega valige kontrolleril.

5. Nii kontrolörid käivitamisel klaster võrguühenduse.

### <a name="single-controller-replacement-steps"></a>Ühe domeenikontrolleri asendamine järgmiselt.

Kui üks, selle hulka kuuluvad Microsoft Azure StorSimple seadme nurjub, tehke järgmist. (Muud kontrolleril peab olema aktiivne ja käivitatud. Kui nii kontrollerid nurjuda või rikkeid, minge [kahekordne kontrolleril asendamine juhiste](#dual-controller-replacement-steps).)

>[AZURE.NOTE] Võib kuluda taaskäivitada ja taastada täielikult ühe domeenikontrolleri asendamine protseduur 30 – 45 minutit. Kogu protseduuri kogukestusega manustamise kaabel, sh on umbes 2 tundi.

#### <a name="to-remove-a-single-failed-controller-module"></a>Ühe nurjunud kontrolleril mooduli eemaldamine

1. Avage Azure'i klassikaline portaalis StorSimple halduri teenusega, klõpsake vahekaarti **seadmed** ja valige seade, mida soovite jälgida nime.

2. Minge **hooldus > riistvara oleku**. Selle domeenikontrolleri 0 või 1 kontrolleril olek peaks olema punane, mis näitab, et tõrge.

    >[AZURE.NOTE] Nurjunud kontrolleril ühe domeenikontrolleri asendamine on alati valige kontrolleril.

3. Otsimine nurjunud kontrolleril mooduli joonisel 1 ja järgmise tabeli abil.  

    ![Backplane seadme esmane ruum moodulid](./media/storsimple-controller-replacement/IC740994.png)

    **Joonis 1** StorSimple seadme tagakülg

  	|Sildi|Kirjeldus|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Selle domeenikontrolleri 0|
  	|4|Selle domeenikontrolleri 1|

4. Nurjunud kontrolleril eemaldada kõik ühendatud võrgukaablid andmete pordid. Kui kasutate mudelit, mis 8600, ka eemaldada SAS kaabel, mis selle domeenikontrolleri ühenduse EBOD kontrolleril.

5. Järgige [eemaldamine on selle domeenikontrolleri](#remove-a-controller) nurjunud kontrolleril eemaldada. 

6. Installige factory asendaja sama pesa, kust on eemaldatud nurjunud kontrolleril. See käivitab ühe domeenikontrolleri asendamine loogika. Lisateavet leiate teemast [ühe domeenikontrolleri asendamine loogika](#single-controller-replacement-logic).

7. Ajal ühe domeenikontrolleri asendamine loogika edenedes taustal, Ühendage kaabel on. Olge ettevaatlik, et ühendada kõik kaabel täpselt samamoodi, et need olid ühendatud enne asendamine.

8. Pärast selle domeenikontrolleri taaskäivitamist, märkige ruut **selle domeenikontrolleri olek** ja soovitud **klaster olek** Azure klassikaline portaalis veendumaks, et selle domeenikontrolleri on tagasi terve riigi ja on puhkerežiimis.

>[AZURE.NOTE] Kui olete seadme kaudu järjestikune konsooli järelevalvet, võidakse kuvada mitu taaskäivitamist ajal selle domeenikontrolleri taastub selle protseduuri kaudu. Kui kuvatakse menüü järjestikune konsooli, siis teate asendamine lõpuleviimist. Kui menüü ei kuvata alates domeenikontrolleri asendamine kahe tunni jooksul, palun [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md).

## <a name="replace-both-controllers"></a>Nii kontrollerid asendamine

Microsoft Azure'i StorSimple seadme nii kontrollerid ei ole, on rikutud või on puudu, peate nii kontrollerid asendada. 

### <a name="dual-controller-replacement-logic"></a>Kahekordne kontrolleril asendamine loogika

Kahekordne kontrolleril lugemisprogrammi esmalt eemaldada nii nurjunud kontrollerid ja seejärel lisa asendamine. Kaks asendamine kontrollerid sisestamisel tekkida järgmisi toiminguid.

1. Asendamine kontrolleril pesa 0 kontrollib järgmist:
 
   1. Seda kasutatakse praeguse versiooni püsivara ja tarkvara

   2. See on osa klaster?

   3. On omavahelistes kontrolleril, kus töötab ja on rühmitatud?
                            
    Kui ükski neist tingimustest on täidetud, otsib selle domeenikontrolleri uusima igapäevane varundamine (asub **nonDOMstorage** kettadraivi S). Selle domeenikontrolleri kopeerib on VHD uusima hetktõmmise varukoopia.

2. Selle domeenikontrolleri pesa 0 kasutab hetktõmmis piltide ise.

3. Samal ajal ootab kontrolleril pesa 1 kontrolleril 0 on pildistamine lõpuleviimine ja alustada.

4. Pärast selle domeenikontrolleri 0 käivitub, tuvastab kontrolleril 1 klaster loodud kontrolleril 0, mis käivitab ühe domeenikontrolleri asendamine loogika. Lisateavet leiate teemast [ühe domeenikontrolleri asendamine loogika](#single-controller-replacement-logic).

5. Pärast seda, töötab nii kontrollerid ja klaster veebis.

>[AZURE.IMPORTANT] Pärast kahekordne kontrolleril lugemisprogrammi pärast StorSimple seade on konfigureeritud, on oluline võtta käsiraamatule seadme varukoopia. Igapäevane seadme konfiguratsiooni varukoopiate on käivitanud kuni pärast 24 tunni möödumist. Töötamine [Microsofti toe](storsimple-contact-microsoft-support.md) poole ja uurige käsiraamatule seadme varukoopia.

### <a name="dual-controller-replacement-steps"></a>Kahekordne kontrolleril asendamine järgmiselt.

See töövoog on vajalik, kui mõlemad selle hulka kuuluvad Microsoft Azure StorSimple seadme nurjus. See võib juhtuda andmekeskuses, kus jahutussüsteemi lakkab töötamast ja seetõttu nii kontrolörid nurjuda aja jooksul. Sõltuvalt sellest, kas StorSimple seade on välja lülitatud või ja kas kasutate mõnda 8600 või 8100 mudelit, mis on vaja teistsuguseid juhiseid.

>[AZURE.IMPORTANT] 1 tund jaoks kontrolleril taaskäivitada ja täielikult taastamine kahekordne kontrolleril asendamine protseduuri võib kuluda 45 minutit. Kogu protseduuri kogukestusega manustamise kaabel, sh on ligikaudu 2,5 tundi.

#### <a name="to-replace-both-controller-modules"></a>Nii kontrolleril moodulid asendamine

1. Kui seade on sisse lülitatud, jätke see juhis vahele ja jätkake järgmise juhisega. Kui seade on sisse lülitatud, lülitage seade.
                                        
    1. Kui kasutate mudelit, mis 8600, väljalülitamine esmane ruumi esimese, ja lülitage EBOD ruumi.

    2. Oodake, kuni seade on täielikult sulgeda. Kõik taga seadme LED on välja lülitatud.

2. Eemaldage kõik võrgukaablid, mis on ühendatud andmete pordid. Kui kasutate mudelit, mis 8600, ka eemaldada SAS kaabel, mis esmane ruumi ühenduse EBOD ruumi.

3. Eemaldage nii kontrollerid StorSimple seadme kaudu. Lisateabe saamiseks vt [on selle domeenikontrolleri eemaldada](#remove-a-controller).

4. Lisage esmalt factory asendust kontrolleril 0, ja seejärel lisage selle domeenikontrolleri 1. Lisateabe saamiseks lugege teemat [lisada mõne selle domeenikontrolleri](#insert-a-controller). See käivitab kahekordne kontrolleril asendamine loogika. Lisateabe saamiseks vt [kahekordne kontrolleril asendamine loogika](#dual-controller-replacement-logic).

5. Ajal kontrolleril asendamine loogika edenedes taustal, Ühendage kaabel on. Olge ettevaatlik, et ühendada kõik kaabel täpselt samamoodi, et need olid ühendatud enne asendamine. Leiate üksikasjalikud juhised kaabel jaotises teie seadme mudelist [StorSimple 8100 seadme installimine](storsimple-8100-hardware-installation.md) või [seadme StorSimple 8600 installida](storsimple-8600-hardware-installation.md).

6. Lülitage sisse StorSimple seade. Kui kasutate 8600 mudelit, mis:

    1. Veenduge, et EBOD ruumi on sisse lülitatud esimene.

    2. Oodake, kuni EBOD ruumi töötab.

    3. Esmane ruumi sisse lülitada.

    4. Pärast esimese kontrolleril taaskäivitamist ja korralik olekus, töötab süsteem.

    >[AZURE.NOTE] Kui olete seadme kaudu järjestikune konsooli järelevalvet, võidakse kuvada mitu taaskäivitamist ajal selle domeenikontrolleri taastub selle protseduuri kaudu. Järjestikune konsooli Menüü kuvamisel klõpsake teate asendamine lõpuleviimist. Kui menüü ei kuvata alates domeenikontrolleri asendamine 2,5 tunni jooksul, palun [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md).

## <a name="remove-a-controller"></a>Mõne selle domeenikontrolleri eemaldamine

Vigase kontrolleril mooduli StorSimple seadme eemaldamiseks tehke järgmist.

>[AZURE.NOTE] Järgmiste illustratsioonide on kontrolleril 0. Selle domeenikontrolleri 1, soovite tühistada need.

#### <a name="to-remove-a-controller-module"></a>Selle domeenikontrolleri mooduli eemaldamine

1. Haarake mooduli lukustatakse oma nimetissõrmega vahel.

2. Pigistage oma nimetissõrmega koos vabastamiseks kontrolleril lukustatakse.

    ![Selle domeenikontrolleri lukustatakse vabastamine](./media/storsimple-controller-replacement/IC741047.png)

    **Joonis 2** Selle domeenikontrolleri lukustatakse vabastamine

2. Kasutage funktsiooni lukustatakse pidet slaidipaigutuse kontrolleril raami välja.

    ![Selle domeenikontrolleri välja raami libistades](./media/storsimple-controller-replacement/IC741048.png)

    **Joonis 3** Libistades kontrolleril välja raami

## <a name="insert-a-controller"></a>Mõne selle domeenikontrolleri lisamine

Pärast vigase mooduli eemaldamine StorSimple seadme factory esitatud kontrolleril mooduli installimiseks tehke järgmist.

#### <a name="to-install-a-controller-module"></a>Selle domeenikontrolleri mooduli installimiseks

1. Kontrollige, kas kahju kasutajaliidese konnektorid. Installige mooduli, kui mis tahes konnektori kontaktid on kahjustatud või painutatud.

2. Selle domeenikontrolleri mooduli lükake raami ajal siis lukustatakse täielikult välja. 

    ![Selle domeenikontrolleri libistades raami sisse](./media/storsimple-controller-replacement/IC741053.png)

    **Joonis 4** Selle domeenikontrolleri libistades raami sisse

3. Selle domeenikontrolleri mooduli lisatud, alustage sulgemist selle lukustatakse jätkates push kontrolleril mooduli raami. Funktsiooni lukustatakse osaleb suunata selle domeenikontrolleri oma kohale.

    ![Selle domeenikontrolleri lukustatakse sulgemine](./media/storsimple-controller-replacement/IC741054.png)

    **Joonis 5** Selle domeenikontrolleri lukustatakse sulgemine

4. Olete valmis, kui selle lukustatakse klõpsatusega koht. **OK** LED peaks nüüd olema.  

    >[AZURE.NOTE] Võib kuluda kuni 5 minutit selle domeenikontrolleri ja LED aktiveerimine.

5. Veenduge, et asendamine on eduka Azure klassikaline portaalis, minge **seadmete** > **hooldustööd** > **Riistvara oleku**, ja veenduge, et nii kontrolleril 0 ja 1 selle domeenikontrolleri on terve (olek on roheline).

## <a name="identify-the-active-controller-on-your-device"></a>Aktiivse domeenikontrolleri seadme tuvastamine

On palju olukordi, esimest korda seadme registreerimise või domeenikontrolleri asendamine, nt leidke aktiivse kontrolleril StorSimple seadme nõudvate. Aktiivse kontrolleril töötleb kõik ketta püsivara ja võrgunduse toimingud. Ühte järgmistest meetoditest abil saate tuvastada, aktiivne kontrolleril:

- [Azure'i klassikaline portaali abil saate tuvastada aktiivne domeenikontrolleri](#use-the-azure-classic-portal-to-identify-the-active-controller)

- [Aktiivse kontrolleril tuvastamine StorSimple Windows PowerShelli abil](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)

- [Kontrollige seadme aktiivne kontrolleril tuvastamiseks](#check-the-physical-device-to-identify-the-active-controller)

Kõiki neid toiminguid kirjeldatakse edasi.

### <a name="use-the-azure-classic-portal-to-identify-the-active-controller"></a>Azure'i klassikaline portaali abil saate tuvastada aktiivne domeenikontrolleri

Azure'i klassikaline portaalis, liikuge **seadmete** > **hooldus**ja liikuge kerides jaotisse **kontrollerid** . Siin saate kontrollida, millist kontrolleril on aktiivne.

![Aktiivse kontrolleril Azure klassikaline portaalis tuvastamine](./media/storsimple-controller-replacement/IC752072.png)

**Joonis 6** Näitab aktiivse kontrolleril Azure klassikaline portaalis

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>Aktiivse kontrolleril tuvastamine StorSimple Windows PowerShelli abil

Kui kasutate oma seadme kaudu järjestikune konsooli, banner sõnum on esitatud. Plakat sõnum sisaldab põhilised teavet, nt mudeli, nimi, installitud tarkvara versioon ja selle domeenikontrolleri avate olekut. Järgmisel pildil on kujutatud banner sõnumi näide:

![Järjestikune banner sõnum](./media/storsimple-controller-replacement/IC741098.png)

**Joonis 7** Teade kuvatakse kontrolleril 0 aktiivseks banner

Plakat sõnumi abil saate määratleda, kas olete loonud ühenduse on aktiivne või passiivne.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Kontrollige seadme aktiivne kontrolleril tuvastamiseks

Aktiivse domeenikontrolleri seadme tuvastamiseks leidke sinine LED ülaltoodud andmete 5 pordi esmane ruumi tagasi.

Kui see LED vilgub, on aktiivne ja muude kontrolleril on puhkerežiimis. Kasutage järgmisi diagramm ja tabel abi.

![Seadme esmane ruumi backplane andmeliideste abil](./media/storsimple-controller-replacement/IC741055.png)

**8-kujund** Peamine andmete pordid ja jälgimisega seotud LED ruum tagakülg

|Sildi|Kirjeldus|
|:----|:----------|
|1-6|ANDMETE 0 – 5 võrgu pordid|
|7|Sinine LED|


## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet [StorSimple riistvara komponent asendamine](storsimple-hardware-component-replacement.md).
