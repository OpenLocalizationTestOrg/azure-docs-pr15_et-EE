<properties 
   pageTitle="Vaadata ja hallata StorSimple virtuaalse massiiv teatiste | Microsoft Azure'i"
   description="Kirjeldatakse StorSimple virtuaalse massiiv teatis tingimused ja raskusaste ja kuidas StorSimple halduri teenuse abil teatiste haldamine."
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
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-alerts-for-the-storsimple-virtual-array"></a>StorSimple halduri teenuse abil saate vaadata ja StorSimple virtuaalse massiivi teatiste haldamine

## <a name="overview"></a>Ülevaade

StorSimple halduri teenuse vahekaarti **teatised** võimaldab teil läbi vaadata ja tühjendage StorSimple virtuaalse massiiv – seotud teatised reaalajas alusel. Sellel vahekaardil saate jälgida ühes keskses kohas oma StorSimple virtuaalse massiivi (tuntud ka kui StorSimple kohapealse virtuaalne seadmed) ja üldise Microsoft Azure'i StorSimple lahenduse seisundi probleeme.

Selles õppetükis kirjeldatakse, kuidas konfigureerida teatiste levinud teatis tingimused, teatiste raskusaste tasemeid ja kuidas vaadata ja jälgida teatised. Lisaks sisaldab teatis kiirülevaate tabelid, mis võimaldavad kiiresti leida kindla teatise ja vastata õigesti.

![Lehe teatiste](./media/storsimple-ova-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Teatiste sätete konfigureerimine

Saate valida, kas soovite e-posti teatis tingimused iga seadme StorSimple virtuaalse teavitama. Lisaks saate tuvastada hoiatus teistele adressaatidele, sisestades nende meiliaadressid väljale **teistele adressaatidele e-posti** , eraldades need semikooloniga.

>[AZURE.NOTE] Saate sisestada kuni 20 e-posti aadressid, virtuaalse seadme kohta.

Pärast meiliteatise virtuaalse seadme lubamiseks saadetakse teatis loendi liikmete meilisõnumi iga kord, kui ilmneb kriitiline teade. Sõnumid saadetakse *storsimple-alerts-noreply@mail.windowsazure.com* ja kirjeldatakse teatiste tingimus. Adressaadid saavad klõpsake nuppu **Tühista tellimus** ise eemaldamiseks loendist e-posti teatis.

#### <a name="to-enable-email-notification-of-alerts-for-a-virtual-device"></a>E-posti teavituse teatisi virtuaalse seadme

1. Avage jaotis **seadmed** > virtuaalse seadme**konfigureerimine** . Liikuge jaotisse **Teatisesätete** .

    ![Teatisesätete](./media/storsimple-ova-manage-alerts/alerts2.png)

2. Klõpsake jaotises **Teatisesätete**määramiseks järgmist.

    1. **Saada e-posti teatise** väljal valige **Jah**.

    2. Välja **e-posti teenus administraatorid** , valige **Jah** , kui soovite teenuse administraator ja kõik kaasadministraatorite teateid teatis.

    3. Sisestage väljale **e-posti teiste adressaatide** meiliaadressid, kõigile teistele adressaatidele, kes peaksid saama teatise teatised. Sisestage nimed vormingus *someone@somewhere.com*. Kasutage meiliaadressid eraldamiseks semikooloneid. Saate konfigureerida kuni 20 e-posti aadressid, virtuaalse seadme kohta. 

        ![teatiste teatis konfigureerimine](./media/storsimple-ova-manage-alerts/alerts3.png)

3. Klõpsake lehe allosas nuppu **salvestada** oma konfiguratsiooni salvestamiseks.

4. Teavitus test e-posti saatmiseks klõpsake käsku **testi meilisõnumi saatmine**kõrval olevat noolt ikooni. StorSimple halduri teenuse kuvab olekuteade, nagu see edastab testi teatis. 

5. Kui kuvatakse järgmine teade, klõpsake nuppu **OK**. 

    ![Teatiste testimiseks saadetud meiliteatise](./media/storsimple-ova-manage-alerts/alerts4.png)

    >[AZURE.NOTE] Kui test teavitussõnumi ei saa saata, kuvatakse StorSimple halduri teenuse vastav sõnum. Klõpsake nuppu **OK**, oodake mõni minut ja proovige seejärel uuesti saata sõnum testi teatis. 

    Testi teade on järgmine.

    ![Teatiste testi e-posti näide](./media/storsimple-ova-manage-alerts/alerts5.png)

## <a name="common-alert-conditions"></a>Levinud teatis tingimused

Teie StorSimple virtuaalse massiiv loob teatiste vastuseks erinevaid tingimusi. Järgnevalt on levinumat tüüpi teatiste tingimusi.

- **Ühenduvusprobleemide** – need teatised ilmneda juhul, kui seal on raskusi andmeid. Side võib ilmneda ajal andmete ja Azure storage kontolt või Ühenduvus virtuaalne seadmed ja StorSimple halduri teenuse tõttu. Side on mõned raskem, sest seal on nii palju punkti tõrgete parandamiseks. Peaksite kõigepealt alati veenduma, et võrguühenduse ja Interneti-ühendus on olemas enne jätkamist edasi täpsemate tõrkeotsing. Pordid ja tulemüüri sätete kohta lisateabe saamiseks minge [StorSimple virtuaalse massiiv süsteeminõuded](storsimple-ova-system-requirements.md). Abi tõrkeotsingu, minge [tõrkeotsing cmdletiga testi ühendust](storsimple-troubleshoot-deployment.md).

- **Jõudlusega seotud probleemide** – need teatised on põhjustatud, kui teie süsteemi ei toimi optimaalselt, näiteks kui see on suur koormus.

Lisaks võidakse kuvada turvalisus, värskendused või töö vead seotud teatised.

## <a name="alert-severity-levels"></a>Teatiste raskusaste tasemed

Teatiste olla erinevad raskusaste, sõltuvalt teatiste olukord on mõju ja vajate vastust Teavita. Raskusaste tasemed:

- **Kriitiline** – see teatis on tingimus, mis mõjutavad teie süsteemi eduka täitmise vastuseks. Toiming on vajalik tagamaks, et StorSimple, teenus ei katkestata.

- **Hoiatus** – see tingimus muutuda kriitilised, kui ei lahene. Peaksite uurima olukorda ja tühjendage probleemi pea selleks midagi tegema.

- **Teave** teemasse sisaldab teavet, mis võib olla kasulik jälgimine ja haldamine teie süsteemi.

## <a name="view-and-track-alerts"></a>Saate vaadata ja jälgida teatised

Armatuurlaua StorSimple halduri teenuse annab teile põgusalt virtuaalse seadmetes, korraldatud raskusaste taseme teatiste arv.

![Teatiste armatuurlaud](./media/storsimple-ova-manage-alerts/alerts6.png)

Raskusaste taseme avatakse vahekaarti **teatised** . Tulemused sisaldavad ainult selle raskusaste taseme vastavad teatised.

![Teatiste aruande rakendatud teatiste tüüp](./media/storsimple-ova-manage-alerts/alerts7.png)

Klõpsates teatise loendis pakub täiendavaid üksikasju teatise, sh teatise teatati, viimast esinemiskordade teatise seade ja lahendamiseks teatise soovitatav toiming.

Kui soovite saada teavet Microsoft Support saate kopeerida tekstifaili teatiste üksikasjad. Olete jälgitavad soovitust ja teatiste tingimus asutusesisese lahendatud, peaksite kustutama valimisel teatise vahekaarti **teatised** ja **tühjendage**teatise kaudu. Tühjendage mitme teatiste, valige iga teatisega, klõpsake mis tahes veeru peale **teatise** veeru, ja seejärel klõpsake nuppu **Tühjenda** kui olete valinud kõik teatised tühjendamist. Pange tähele, et mõned teatised automaatselt tühi kui probleem on lahendatud või kui süsteemi värskendab teatise uut teavet.

Kui klõpsate käsku **Tühjenda**, on teil võimalus esitada kommentaarid teatise ja toiminguid, mis viisite probleemi lahendamiseks. 

![teatiste kommentaarid](./media/storsimple-ova-manage-alerts/clear-alert.png)

Klõpsake ikooni Otsi ![sisse-ikoon](./media/storsimple-ova-manage-alerts/check-icon.png) saate salvestada oma kommentaarid.

Kui mõne muu sündmuse käivitamisel uut teavet, kustutatakse teatud sündmuste süsteem. Sellisel juhul kuvatakse järgmine teade.

![Tühjendage ruut hoiatusteate](./media/storsimple-ova-manage-alerts/alerts8.png)

## <a name="sort-and-review-alerts"></a>Teatiste sortimine ja läbivaatus

Vahekaarti **teatised** saab kuvada kuni 250 teatised. Kui olete ületanud teatiste numbrit, kuvatakse vaikevaates kuvada kõik teatised. Saate ühendada kohandada, millised teatised kuvatakse järgmised väljad:

- **Olek** – saate kuvada **aktiivne** või **tühjendatud** teatised. Aktiivse teatised endiselt on käivitatakse teie süsteemi ajal kustutatud teatiste on kas käsitsi tühjendatud administraator või programmiliselt tühjendatud, kuna süsteemis värskendatud teatiste tingimus uut teavet.

- **Raskusaste** – saate kuvada teatiste raskusaste kõigil (kriitiline, hoiatus, teave) või lihtsalt on teatud raskusaste, nt ainult kriitilised teatised.

- **Andmeallika** – saate kuvada kõik allikatest teatiste või piirata teatiste neile, mis on pärit teenuse või ühe või kõigi virtuaalse seadmete.

- **Ajavahemiku** – **alates** ja **kuni** kuupäevad ja ajatemplid, määrates saate vaadata teatiste ajal, mil olete huvitatud.

## <a name="alerts-quick-reference"></a>Teatiste kiirülevaade

Järgmises tabelis on loetletud mõned Microsoft Azure'i StorSimple teatised, mis võivad ilmneda, samuti täiendavat teavet ja soovitused võimaluse. StorSimple virtuaalse seadme teatiste kuuluvad ühte järgmisest kategooriast:

- [Pilveteenuse ühenduvuse teatised](#cloud-connectivity-alerts)

- [Teatiste konfigureerimine](#configuration-alerts)

- [Töö nurjumise teatised](#job-failure-alerts)

- [Jõudluse teatised](#performance-alerts)

- [Turbeteatiste](#security-alerts)

- [Hoiatused](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Pilveteenuse ühenduvuse teatised

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Seadme *<device name>* pole ühendatud pilveteenusesse.|Nimega seade ei saa ühendust pilve. |Ei saanud ühendust pilveteenusesse. Põhjuseks võib olla üks järgmistest:<ul><li>Võib esineda probleem võrgusätted seadmes.</li><li>Võib esineda probleem salvestusruumi konto identimisteave.</li></ul>Ühenduvusprobleemide tõrkeotsingu kohta lisateabe saamiseks minge [kohaliku web UI](storsimple-ova-web-ui-admin.md) seade.|


### <a name="configuration-alerts"></a>Teatiste konfigureerimine

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Kohapealse virtuaalse seadme konfiguratsiooni toetuseta.|Aeglane jõudlus.|Praeguse konfiguratsiooni võib kaasa tuua jõudluse vähenemine. Veenduge, et teie server vastab minimaalne konfiguratsioon. Lisateabe saamiseks minge [StorSimple virtuaalse massiiv nõuetele](storsimple-ova-system-requirements.md). 
|Teil on piisavalt kettaruumi ettevalmistatud <*seadme nimi*>.|Kettaruumi hoiatus.|Teil on väikese ettevalmistatud kettal ruumi. Vaba ruumi, kaaluge töökoormus teisaldamine teise helitugevust ja ühiskasutusse anda või andmete kustutamist.

### <a name="job-failure-alerts"></a>Töö nurjumise teatised

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Varukoopia <*seadme nimi*> ei saanud lõpule viia.|Varundustöö tõrge.|Ei saanud luua varukoopia. Võtke arvesse järgmist.<ul><li>Ühenduvusprobleemide võivad takistada varukoopia toimingu lõpuleviimist edukalt. Tagada, et pole ühenduvusprobleemide. Ühenduvusprobleemide tõrkeotsingu kohta lisateabe saamiseks minge [kohaliku web UI](storsimple-ova-web-ui-admin.md) virtuaalse seadme.</li><li>Olete jõudnud saadaoleva talletusmahu piirmäära. Kaaluge ruumi vabastamiseks kustutada kõik varukoopiate, mis ei ole enam vaja.</li></ul> Probleemide lahendamiseks, tühjendage teatise ja proovige uuesti.|
|<*Seadme nimi*> Taasta ei saanud lõpule viia.|Taastada töö tõrge.|Varukoopia põhjal ei saa taastada. Võtke arvesse järgmist.<ul><li>Varukoopia loendisse ei pruugi olla lubatud. Värskendage loendit ja veenduge, et see on kehtiv.</li><li>Ühenduvusprobleemide võivad takistada taastetoimingu edukalt lõpule viimise. Tagada, et pole ühenduvusprobleemide.</li><li>Olete jõudnud saadaoleva talletusmahu piirmäära. Kaaluge ruumi vabastamiseks kustutada kõik varukoopiate, mis ei ole enam vaja.</li></ul>Probleemide lahendamiseks, tühjendage teatise ja proovige uuesti taastamine.|
|<*Seadme nimi*> klooni ei saanud lõpule viia.|Klooni töö tõrge.|Klooni ei saanud luua. Võtke arvesse järgmist.<ul><li>Varukoopia loendisse ei pruugi olla lubatud. Värskendage loendit ja veenduge, et see on kehtiv.</li><li>Ühenduvusprobleemide võivad takistada klooni toimingu lõpuleviimist edukalt. Tagada, et pole ühenduvusprobleemide.</li><li>Olete jõudnud saadaoleva talletusmahu piirmäära. Kaaluge ruumi vabastamiseks kustutada kõik varukoopiate, mis ei ole enam vaja.</li></ul>Probleemide lahendamiseks, tühjendage teatise ja proovige uuesti.|

### <a name="performance-alerts"></a>Jõudluse teatised

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Teil on probleeme andmeedastus ootamatud viivitust.|Andmeedastus on aeglane.|Kui ületate skaleeritavus sihtkohtade salvestusruumi teenuse tekkida ahendamise tõrkeid. Salvestusruumi teenuse see tagada ega ühe kliendi rentnik saate kasutada teenust teiste arvelt. Azure'i salvestusruumi konto tõrkeotsingu kohta lisateabe saamiseks minge [kuvaris, diagnoosimine, ja Microsoft Azure'i Tabelimäluga tõrkeotsing](storage-monitoring-diagnosing-troubleshooting.md).
|Teil on väikese kohaliku broneerimiseks kettaruumi <*seadme nimi*>.|Aeglane aega.|10% ettevalmistatud suuruse <*seadme nimi*> reserveeritud kohaliku seadmes ja nüüd teil on väikese reserveeritud ruumi. Koormust <*seadme nimi*> tekitab piimapütt kiirema või teil võib viimati migreeritud suurt hulka andmeid. See võib põhjustada jõudlust vähendada. Kaaluge üks järgmistest toimingutest probleemi lahendamiseks:<ul><li>Pilveteenuse läbilaskevõime seadme suurendamine</li><li>Vähendada või teisaldada töökoormus teise helitugevust ja ühiskasutusse anda.</li></ul>

### <a name="security-alerts"></a>Turbeteatiste

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Parooli <*seadme nimi*> aegub <*arv*> päeva.|Parooli hoiatus.| Teie parool aegub < arv < päeva. Kaaluge parooli muutmine. Lisateabe saamiseks minge [StorSimple virtuaalse massiiv seadme administraatori parooli muutmine](storsimple-ova-change-device-admin-password.md).

### <a name="update-alerts"></a>Hoiatused

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Uued värskendused on teie seadme jaoks saadaval.|StorSimple virtuaalse massiiv värskendused on saadaval.|Uute värskenduste installimist **haldamise** lehe kaudu.|
|Võiks skannida <*seadme nimi*> uute Värskenduste otsimine.|Värskendage tõrge. |Uute värskenduste installimise ajal ilmnes tõrge. Saate käsitsi installida värskendusi. Kui probleem ei lahene, pöörduge [Microsofti klienditoe poole](storsimple-contact-microsoft-support.md).|


## <a name="next-steps"></a>Järgmised sammud

- [Lisateavet StorSimple virtuaalse massiiv](storsimple-ova-overview.md).


