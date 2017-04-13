<properties 
   pageTitle="Saate vaadata ja hallata StorSimple teatiste | Microsoft Azure'i"
   description="Kirjeldatakse StorSimple teatis tingimuste ja raskusaste, kuidas konfigureerida teatiste ja kuidas StorSimple halduri teenuse abil teatiste haldamine."
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
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="anbacker" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-alerts"></a>StorSimple halduri teenuse abil saate vaadata ja StorSimple teatiste haldamine

## <a name="overview"></a>Ülevaade

StorSimple halduri teenuse vahekaarti **teatised** võimaldab teil läbi vaadata ja tühjendage StorSimple seadme – seotud teatised reaalajas alusel. Sellel vahekaardil saate jälgida ühes keskses kohas seadmetes StorSimple ja üldise Microsoft Azure'i StorSimple lahenduse seisundi probleeme.

Selles õppetükis kirjeldatakse levinud teatiste tingimusi, teatiste raskusaste tasemeid ja kuidas konfigureerida teatiste. Lisaks sisaldab teatis kiirülevaate tabelid, mis võimaldavad kiiresti leida kindla teatise ja vastata õigesti.

![Lehe teatiste](./media/storsimple-manage-alerts/HCS_AlertsPage.png)

## <a name="common-alert-conditions"></a>Levinud teatis tingimused

Seadme StorSimple loob teatiste vastuseks erinevaid tingimusi. Järgnevalt on levinumat tüüpi teatiste tingimusi.

- **Riistvara probleemid** – need teatised öelda riistvara seisundi kohta. Need teile teada, kui püsivara uuendamine on vajalik, kui võrgu liidese on probleeme või kui on probleem ühe draivi andmed.

- **Ühenduvusprobleemide** – need teatised ilmneda juhul, kui seal on raskusi andmeid. Side võib ilmneda ajal andmete ja Azure storage kontolt või Ühenduvus seadmete ja StorSimple halduri teenuse tõttu. Side on mõned raskem, sest seal on nii palju punkti tõrgete parandamiseks. Peaksite kõigepealt alati veenduma, et võrguühenduse ja Interneti-ühendus on olemas enne jätkamist edasi täpsemate tõrkeotsing. Abi tõrkeotsingu, minge [tõrkeotsing cmdletiga testi ühendust](storsimple-troubleshoot-deployment.md).

- **Jõudlusega seotud probleemide** – need teatised on põhjustatud, kui teie süsteemi ei toimi optimaalselt, näiteks kui see on suur koormus.

Lisaks võidakse kuvada turvalisus, värskendused või töö vead seotud teatised.

## <a name="alert-severity-levels"></a>Teatiste raskusaste tasemed

Teatiste olla erinevad raskusaste, sõltuvalt teatiste olukord on mõju ja vajate vastust Teavita. Raskusaste tasemed:

- **Kriitiline** – see teatis on tingimus, mis mõjutavad teie süsteemi eduka täitmise vastuseks. Toiming on vajalik tagamaks, et StorSimple, teenus ei katkestata.

- **Hoiatus** – see tingimus muutuda kriitilised, kui ei lahene. Peaksite uurima olukorda ja tühjendage probleemi pea selleks midagi tegema.

- **Teave** teemasse sisaldab teavet, mis võib olla kasulik jälgimine ja haldamine teie süsteemi.

## <a name="configure-alert-settings"></a>Teatiste sätete konfigureerimine

Saate valida, kas soovite e-posti teatis tingimused iga seadme StorSimple teavitama. Lisaks saate tuvastada hoiatus teistele adressaatidele, sisestades nende meiliaadressid väljale **teistele adressaatidele e-posti** , eraldades need semikooloniga.

>[AZURE.NOTE] Saate sisestada kuni 20 e-posti aadressid, seadme kohta.

Pärast e-posti teatis seadme lubamiseks teatise loendis liikmed saavad meilisõnumi iga kord, kui ilmneb kriitiline teade. Sõnumid saadetakse *storsimple-alerts-noreply@mail.windowsazure.com* ja kirjeldatakse teatiste tingimus. Adressaadid saavad klõpsake nuppu **Tühista tellimus** ise eemaldamiseks loendist e-posti teatis.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>E-posti teavituse teatisi seadme jaoks

1. Avage jaotis **seadmed** > seadme**konfigureerimine** .

2. Jaotises **Teatiste sätete**määramiseks järgmist.

    1. **Saada e-posti teatise** väljal valige **Jah**.

    2. Välja **e-posti teenus administraatorid** , valige **Jah** , kui soovite teenuse administraator ja kõik kaasadministraatorite teateid teatis.

    3. Sisestage väljale **e-posti teiste adressaatide** meiliaadressid, kõigile teistele adressaatidele, kes peaksid saama teatise teatised. Sisestage nimed vormingus *someone@somewhere.com*. Kasutage meiliaadressid eraldamiseks semikooloneid. Saate konfigureerida kuni 20 e-posti aadressid, seadme kohta. 

        ![Teatiste teatis konfigureerimine](./media/storsimple-manage-alerts/AlertNotify.png)

3. Teavitus test e-posti saatmiseks klõpsake käsku **testi meilisõnumi saatmine**kõrval olevat noolt ikooni. StorSimple halduri teenuse kuvab olekuteade, nagu see edastab testi teatis. 

4. Kui kuvatakse järgmine teade, klõpsake nuppu **OK**. 

    ![Teatiste testimiseks saadetud meiliteatise](./media/storsimple-manage-alerts/HCS_AlertNotificationConfig3.png)

    >[AZURE.NOTE] Kui test teavitussõnumi ei saa saata, kuvatakse StorSimple halduri teenuse vastav sõnum. Klõpsake nuppu **OK**, oodake mõni minut ja proovige seejärel uuesti saata sõnum testi teatis. 

## <a name="view-and-track-alerts"></a>Saate vaadata ja jälgida teatised

Armatuurlaua StorSimple halduri teenuse annab teile põgusalt seadmetes korraldatud raskusaste taseme teatiste arv.

![Teatiste armatuurlaud](./media/storsimple-manage-alerts/admin_alerts_dashboard.png)

Raskusaste taseme avatakse vahekaarti **teatised** . Tulemused sisaldavad ainult selle raskusaste taseme vastavad teatised.

![Teatiste aruande rakendatud teatiste tüüp](./media/storsimple-manage-alerts/admin_alerts_scoped.png)

Klõpsates teatise loendis pakub täiendavaid üksikasju teatise, sh teatise teatati, viimast esinemiskordade teatise seade ja lahendamiseks teatise soovitatav toiming. Kui see on riistvara teatis, tuvastab see ka riistvara komponent.

![Riistvara töölauateate näide](./media/storsimple-manage-alerts/admin_alerts_hardware.png)

Kui soovite saada teavet Microsoft Support saate kopeerida tekstifaili teatiste üksikasjad. Olete jälgitavad soovitust ja teatiste tingimus asutusesisese lahendatud, peaksite kustutama teatise seadme valimine teatise vahekaarti **teatised** ja **tühjendage**. Tühjendage mitme teatiste, valige iga teatisega, klõpsake mis tahes veeru peale **teatise** veeru, ja seejärel klõpsake nuppu **Tühjenda** kui olete valinud kõik teatised tühjendamist. Pange tähele, et mõned teatised automaatselt tühi kui probleem on lahendatud või kui süsteemi värskendab teatise uut teavet.

Kui klõpsate käsku **Tühjenda**, on teil võimalus esitada kommentaarid teatise ja toiminguid, mis viisite probleemi lahendamiseks. Kui mõne muu sündmuse käivitamisel uut teavet, kustutatakse teatud sündmuste süsteem. Sellisel juhul kuvatakse järgmine teade.

![Tühjendage ruut hoiatusteate](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Teatiste sortimine ja läbivaatus

Võite lugeda aruannete käitamine teatised, et saate vaadata ja tühjendage nende rühmade tõhusamaks. Lisaks saate vahekaarti **teatised** kuvada kuni 250 teatised. Kui olete ületanud teatiste numbrit, kuvatakse vaikevaates kuvada kõik teatised. Saate ühendada kohandada, millised teatised kuvatakse järgmised väljad:

- **Olek** – saate kuvada **aktiivne** või **tühjendatud** teatised. Aktiivse teatised endiselt on käivitatakse teie süsteemi ajal kustutatud teatiste on kas käsitsi tühjendatud administraator või programmiliselt tühjendatud, kuna süsteemis värskendatud teatiste tingimus uut teavet.

- **Raskusaste** – saate kuvada teatiste raskusaste kõigil (kriitiline, hoiatus, teave) või lihtsalt on teatud raskusaste, nt ainult kriitilised teatised.

- **Andmeallika** – saate kuvada kõik allikatest teatiste või teatised, mis on pärit kas teenuse või ühe või kõigi seadmete piirata.

- **Ajavahemiku** – **alates** ja **kuni** kuupäevad ja ajatemplid, määrates saate vaadata teatiste ajal, mil olete huvitatud.

## <a name="alerts-quick-reference"></a>Teatiste kiirülevaade

Järgmises tabelis on loetletud mõned Microsoft Azure'i StorSimple teatised, mis võivad ilmneda, samuti täiendavat teavet ja soovitused võimaluse. StorSimple seadme teatiste kuuluvad ühte järgmisest kategooriast:

- [Pilveteenuse ühenduvuse teatised](#cloud-connectivity-alerts)

- [Teatiste kobar](#cluster-alerts)

- [Teatiste katastroofi taastamine](#disaster-recovery-alerts)

- [Riistvara teatised](#hardware-alerts)

- [Töö nurjumise teatised](#job-failure-alerts)

- [Kohalik kinnitatud helitugevuse teatised](#locally-pinned-volume-alerts)

- [Võrgunduse teatised](#networking-alerts)

- [Jõudluse teatised](#performance-alerts)

- [Turbeteatiste](#security-alerts)

- [Tugiteenuste pakett teatised](#support-package-alerts)

- [Hoiatused](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Pilveteenuse ühenduvuse teatised

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Ühenduvus <*cloud mandaati nimi*> ei õnnestu.|Salvestusruumi kontot ei saa ühendust luua.|Tundub, et võib olla võrguühenduse probleemi seadmega. Käivitage selle `Test-HcsmConnection` StorSimple liides Windows PowerShelli cmdlet seadme ära tunda ja probleemi lahendamiseks. Kui sätted on õiged, võib probleem olla salvestusruumi konto, mille teatise esitati identimisteabega. Sel juhul kasutage funktsiooni `Test-HcsStorageAccountCredential` cmdlet-käsu, kui on probleeme, saate lahendada.<ul><li>Kontrollige oma võrgu sätteid.</li><li>Märkige talletusmahu konto identimisteave.</li></ul>|
|Me ei saanud soovitud südamelöögi ajal seadmest viimase <*arv*> minutit.|Seade ei saa ühendust luua.|Tundub, et on võrguühenduse probleemi seadmega. Kasutage funktsiooni `Test-HcsmConnection` StorSimple liides Windows PowerShelli cmdlet seadme tuvastamine ja probleemi lahendamiseks või pöörduda võrguadministraatori poole.|

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>Pilveteenuse ühenduvuse nurjub StorSimple käitumine

Mis juhtub, kui pilvepõhist ühenduvust nurjub minu StorSimple seadme valmistamisel töötab?

Pilveteenuse ühenduvuse nurjumisel seadme StorSimple tootmise seejärel sõltuvalt teie seadme järgmist võivad tekkida. 

- **Kohalikud andmed seadme jaoks**: juba mõnda aega, tekib ei katkemise ja loeb kasutab ka edaspidi kätte. Siiski, nagu tasumata iOS-i arv suureneb ja ületab on piiratud, loeb võiks alustada nurjumise. 

    Sõltuvalt hulka andmeid oma seadmes on kirjutab ka jätkuvalt ilmneda esimese paari tunni pärast häireid ühenduvuse pilve. Funktsiooni kirjutab seejärel aeglane ja lõpuks alustada nurjuda, kui mitu tundi on katkenud pilvepõhist ühenduvust. (Andmeid, mis tuleb lükata pilveteenusesse seadmes on ajutine salvestusruum. Selles jaotises on tühjendada, kui andmed on saadetud. Ühenduvus nurjumisel teave selle salvestusruumi ei lükata pilveteenusesse ja IO nurjub.)   

 
- **Andmete pilves**: enamik cloud ühenduvuse tõrkeid, tagastatakse tõrge. Kui soovitud ühendus on taastatud, IOs jätkamise ilma vajaduseta tuua võrgus helitugevuse kasutaja. Harvaesinevate eksemplari, võidakse nõuda tuua tagasi võrgus helitugevuse Azure klassikaline portaalist kasutaja sekkumist. 
 
- **Pilveteenuse hetktõmmiseid pooleli**: toimingu uuesti proovida paar korda 4-5 tunni jooksul ja taastamisel Ühenduvus pole pilve pilte ei õnnestu.


### <a name="cluster-alerts"></a>Teatiste kobar

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Seadme üle <*seadme nimi*> nurjus.|Seade on hooldus režiimis.|Seadme nurjus üle sisestamise või väljumist hooldus režiimis. See on tavaline ja midagi on vaja. Kui te olete tunnistatud teemasse, tühjendage teatiste lehe kaudu.|
|Seadme üle <*seadme nimi*> nurjus.|Seadme püsivara või tarkvara lihtsalt värskendada.|Esines kobar Tõrkesiirde värskenduse tõttu. See on tavaline ja midagi on vaja. Kui te olete tunnistatud teemasse, tühjendage teatiste lehe kaudu.|
|Seadme üle <*seadme nimi*> nurjus.|Selle domeenikontrolleri on sulgeda või taaskäivitada.|Seadme nurjus üle, kuna aktiivse kontrolleril on sulgeda või taaskäivitada administraator. Midagi on vaja. Kui te olete tunnistatud teemasse, tühjendage teatiste lehe kaudu.|
|Seadme üle <*seadme nimi*> nurjus.|Kavandatud Tõrkesiirde.|Veenduge, et see oli kavandatud Tõrkesiirde. Pärast toimingu korral tühjendage teemasse teatiste lehe kaudu.|
|Seadme üle <*seadme nimi*> nurjus.|Planeerimata Tõrkesiirde.|StorSimple ehitatakse planeerimata failovers automaatselt taastada. Kui te ei näe neid teatiste suure hulga, pöörduge Microsoft Support.|
|Seadme üle <*seadme nimi*> nurjus.|Muud/teadmata põhjus.|Kui te ei näe neid teatiste suure hulga, pöörduge Microsoft Support. Kui probleem on lahendatud, tühjendage teemasse teatiste lehe kaudu.|
|Kriitiline teenuse aruannete olek nagu nurjus.|Andmeteed tõrge. |Abi saamiseks pöörduge Microsoft Support.|
|Võrgu liidese virtuaalse IP-aadress <*andmete #*> aruanded oleku, nagu nurjus. |Muud/teadmata põhjus. |Mõnikord ajutised võivad põhjustada järgmisi teatisi. Kui see on nii, siis see teatis automaatselt kustutatakse mõne aja pärast. Kui probleem ei lahene, pöörduge Microsoft Support.|
|Võrgu liidese virtuaalse IP-aadress <*andmete #*> aruanded oleku, nagu nurjus.|Liidese nimi: <*andmete #*> IP-aadress <IP address> ei saa esitada online, kuna võrgu tuvastati korduv IP-aadress. |Veenduge, et korduv IP-aadress on võrgust eemaldada või taaskonfigureerimine kasutajaliidese eri IP-aadressiga.|


### <a name="disaster-recovery-alerts"></a>Teatiste katastroofi taastamine

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Taastamine toiminguid ei saa taastada kõik selle teenuse sätted. Seadme konfiguratsiooni andmed on vastuolu mõne seadme jaoks.|Pärast avariitaastet tuvastata andmete vastuolu.|Krüptitud andmed teenus ei sünkroonita, et seadmes. Lubada <*seadme nimi*> seadme kaudu StorSimple halduri sünkroonimise käivitamiseks. Windows PowerShelli kasutajaliidese StorSimple abil saate käivitada soovitud `Restore-HcsmEncryptedServiceData` seadme <*seadme nimi*> cmdlet-käsk, pakkudes vana parool andmeid, et see cmdlet-käsu taastamiseks turvalisus profiili. Käivitage selle `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet krüptovõtme teenuse andmete värskendamiseks. Pärast toimingu korral tühjendage teemasse teatiste lehe kaudu.|


### <a name="hardware-alerts"></a>Riistvara teatised

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Riistvara komponent <*komponent ID-d*> aruannete <*olek*> olek.||Mõnikord ajutised võivad põhjustada järgmisi teatisi. Kui jah, kustutatakse see teatis automaatselt mõne aja pärast. Kui probleem ei lahene, pöörduge Microsoft Support.|
|Passiivne kontrolleril rikutud.|Passiivne (teisene) kontrolleril ei tööta.|Teie seade töötab, kuid ühte hulka kuuluvad teie on rikutud. Taaskäivitage selle domeenikontrolleri. Kui probleem ei lahene, pöörduge Microsoft Support.|

### <a name="job-failure-alerts"></a>Töö nurjumise teatised

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Varundus <*Allika helitugevuse rühma ID-d*> nurjus.|Varundustöö nurjus.|Ühenduvusprobleemide võivad takistada varukoopia toimingu lõpuleviimist edukalt. Kui pole ühenduvusprobleemide, võib-olla olete jõudnud varukoopiate arvu ülempiir. Kustutage kõik varukoopiate, mis ei ole enam vaja ja proovige uuesti. Pärast toimingu korral tühjendage teemasse teatiste lehe kaudu.|
|<*Sihtkoha helitugevuse järjenumbreid*> <*Allika varukoopia elemendi ID-d*> klooni nurjus.|Klooni töö nurjus.|Värskendage varukoopia loendit ja veenduge, et varundamine on kehtiv. Kui varundus on lubatud, on võimalik, et pilveteenuses ühenduvusprobleemide takistavad klooni toimingu lõpuleviimist edukalt. Kui pole ühenduvusprobleemide, võib-olla olete jõudnud talletuslimiidi. Kustutage kõik varukoopiate, mis ei ole enam vaja ja proovige uuesti. Pärast vastav toiming selle probleemi lahendamisega, tühjendage teemasse teatiste lehe kaudu.|
|<*Allika varukoopia elemendi ID-d*> Taasta nurjus.|Taastada töö nurjus.|Värskendage varukoopia loendit ja veenduge, et varundamine on kehtiv. Kui varundus on lubatud, on võimalik, et pilveteenuses ühenduvusprobleemide takistavad taastetoimingu edukalt lõpule viimise. Kui pole ühenduvusprobleemide, võib-olla olete jõudnud talletuslimiidi. Kustutage kõik varukoopiate, mis ei ole enam vaja ja proovige uuesti. Pärast vastav toiming selle probleemi lahendamisega, tühjendage teemasse teatiste lehe kaudu.|

### <a name="locally-pinned-volume-alerts"></a>Kohalik kinnitatud helitugevuse teatised

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Kohaliku helitugevuse <*maht nimi*> loomine nurjus.| Töö helitugevuse loomine nurjus. <*Tõrge nurjunud tõrkekood vastav sõnum*>.|Ühenduvusprobleemide võivad takistada ruumi loomise toimingu lõpuleviimist edukalt. Kohalik kinnitatud maht on paksult ette valmistatud ja ruum hõlmab kallab astmeline mahud pilveteenusesse. Kui pole ühenduvusprobleemide, võib täitnud seadmes kohaliku ruumi. Kindlaks määrata, kui ruumi seadme enne selle toimingu proovitakse uuesti.|
|Kohaliku helitugevuse <*maht nimi*> laiendamine nurjus.|Helitugevuse muutmise töö on nurjunud <*tõrge nurjunud tõrkekood vastav sõnum*> tõttu.|Ühenduvusprobleemide võivad takistada helitugevuse laiendamise toimingu lõpuleviimist edukalt. Kohalik kinnitatud maht on paksult ette valmistatud ja ulatub olemasoleva ruumi hõlmab kallab astmeline mahud pilveteenusesse. Kui pole ühenduvusprobleemide, võib täitnud kohaliku seadmes alale. Kindlaks määrata, kui ruumi seadme enne selle toimingu proovitakse uuesti.|
|Teisendamine helitugevuse <*maht nimi*> nurjus.|Helitugevuse muutmise töö teisendada draivi tüüp: Kohalik kinnitatud mitmetasandilise nurjunud.|Helitugevuse üleminekul tüüp kohalikult kinnitatud mitmetasandilise ei saa lõpule viia. Tagada, et pole ühenduvusprobleemide vältimine toimingu lõpulejõudmist. Tõrkeotsingu ühenduvusprobleemide minge [cmdletiga Test-HcsmConnection tõrkeotsing](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Algne kohalikult kinnitatud helitugevuse nüüd märgitud astmeline helitugevuse Kuna mõned andmed kohalikult kinnitatud maht on peale pilveteenusesse teisendamise ajal. Astmeline tulemuseks maht on endiselt töötavate kohaliku ruumi tulevaste kohaliku maht ei saa taastatud seadme.<br>Ühenduvus seotud probleemide lahendamine tühjendage teatise ja teisendage see maht tagasi algse kohalikult kinnitatud helitugevuse tüüpi tagamaks, et kõik andmed on kättesaadav kohalik uuesti.|
|Teisendamine helitugevuse <*maht nimi*> nurjus.|Helitugevuse muutmise töö teisendada draivi tüüp: mitmetasandilise soovite kohalikult kinnitatud nurjus.|Helitugevuse üleminekul tüüp mitmetasandilise kohalikult kinnitatud abil ei saa lõpetada. Tagada, et pole ühenduvusprobleemide vältimine toimingu lõpulejõudmist. Tõrkeotsingu ühenduvusprobleemide minge [cmdletiga Test-HcsmConnection tõrkeotsing](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Nüüd märgitud kohalikult kinnitatud helitugevuse teisendamise käigus jätkuvalt andmete elavate pilves, kuigi see maht seadmes paksult ettevalmistatud ruumi saate enam taastatud tulevaste kohaliku maht on algse astmeline maht.<br>Ühenduvus seotud probleemide lahendamine tühjendage teatise ja teisendage see maht tagasi algse astmeline helitugevuse tüüpi tagamaks, et saate taastatud seadme paksult ette valmistatud kohaliku ruumi.|
|Kohalik tarbimine jaoks kohaliku hetktõmmiste lähenemas <*Helitugevuse rühma nimi*>|Kohaliku varukoopia poliitika hetktõmmiste võib kiiresti otsa ruumi ja host kirjutamine tõrgete vältimiseks kehtetuks.|Sagedased kohaliku hetktõmmiseid kõrge andmete piimapütt mahu, mis on seotud varukoopia poliitika selle rühma kõrval põhjustavad seadmes kasutada kiiresti kohaliku ruumi. Kustuta kohalik pilte, mis ei ole enam vaja. Värskendada oma kohaliku hetktõmmise ajakava selle varukoopia poliitika harvem kohaliku hetktõmmiseid luua ning tagama, et pilveteenuses hetktõmmiseid regulaarselt. Kui neid toiminguid ei tehta, võib kohaliku ruumi neid pilte kiiresti lõpe ja süsteem kustutab need tagamaks, et host kirjutab jätkuvalt edukalt töödelda automaatselt.|
|Kohaliku hetktõmmiseid <*Helitugevuse rühma nimi*> luba kehtetuks.|Kohaliku hetktõmmiseid <*Helitugevuse rühma nimi*> on kehtetuks ja seejärel kustutada, kuna need on üle kohaliku seadmes alale.|See korduvaks tulevikus tagamiseks läbi vaadata selle varukoopia poliitika kohaliku hetktõmmise ajakavasid ja Kustuta kohalik pilte, mis ei ole enam vaja. Sagedased kohaliku hetktõmmiseid kõrge andmete piimapütt mahu, mis on seotud varukoopia poliitika selle rühma kõrval võib põhjustada kohaliku ruumi kasutada kiiresti seadmes.|
|<*Allika varukoopia elemendi ID-d*> Taasta nurjus.|Töö taastamine nurjus.|Kui teil on kohalik kinnitatud või kombinatsiooni kohalikult kinnitatud ja astmeline mahud selle varukoopia poliitika, värskendage varukoopia loendit ja veenduge, et varundamine on kehtiv. Kui varundus on lubatud, on võimalik, et pilveteenuses ühenduvusprobleemide takistavad taastetoimingu edukalt lõpule viimise. Kohalik kinnitatud mahud, mis olid taastatakse selle hetktõmmise rühma on kõik oma seadmesse alla laaditud andmed ja, kui teil on selles rühmas hetktõmmise astmeline ja kohalikult kinnitatud mahu segu, nad ei ole omavahel sünkroonida. Edukalt lõpule taastetoimingu, teha mahud selles rühmas Ühenduseta hosti ja proovige uuesti taastamine. Pange tähele, et helitugevuse andmeid, mis on tehtud ajal taastamine muudatustest töötlemine lähevad kaotsi.|

### <a name="networking-alerts"></a>Võrgunduse teatised

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Ei saa käivitada StorSimple teenused.|Andmeteed tõrge |Kui probleem ei lahene, pöörduge Microsoft Support.|
|Korduv IP-aadress "Data0" ei leitud.| |Süsteemi on tuvastanud konflikti IP-aadress "10.0.0.1". Seadme võrguressurssi 'Data0' *<device1>* on ühenduseta. Veenduge, et IP-aadressi ei kasutata muu üksus, võrku. Võrgu tõrkeotsing, minge [cmdletiga Get-NetAdapter tõrkeotsing](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Selle probleemi lahendamiseks pöörduge võrguadministraatori poole. Kui probleem ei lahene, pöörduge Microsoft Support. |
|IPv4 (või IPv6) "Data0" aadress on ühenduseta.| |Võrguressurssi 'Data0' IP-aadressiga "10.0.0.1." ja eesliite pikkus "22" seadme *<device1>* on ühenduseta. Veenduge, millega on ühendatud selle kasutajaliidese vahetamise pordid toimivad. Võrgu tõrkeotsing, minge [cmdletiga Get-NetAdapter tõrkeotsing](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
 

### <a name="performance-alerts"></a>Jõudluse teatised

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Laadi seade on ületanud <*lävi*>.|Aeglasem kui oodatud vastuse korda.|Seadme aruannete kasutamine sisend koormuse all. See seade ei toimi nii nagu peaks. Vaadake üle töökoormus on lisatud seade ja kindlaks teha, kui on olemas mis on saanud teisaldada mõnda teise seadmesse või mis pole enam vajalikud.<br>Praeguse oleku mõista, minge [StorSimple halduri teenuse jälgida oma seadme kasutamine](storsimple-monitor-device.md)|

### <a name="security-alerts"></a>Turbeteatiste

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Microsoft Support seanss on alanud.|Muude tootjate juurde tugi seanss.|Kinnitage see juurdepääsu saamiseks. Pärast toimingu korral tühjendage teemasse teatiste lehe kaudu.|
|<*Aja*> aegub <*elemendi*> parool.|Parooli aegumise on lähenemas.|Muutke oma parooli, enne selle lõppemist.|
|Puuduvad <*elemendi ID*> Turve konfiguratsiooniteavet.||See helitugevuse ümbris seostatud mahud ei saa kasutada konfiguratsioonist StorSimple korrata. Veenduge, et teie andmed on salvestatud turvaline, soovitame helitugevuse ümbris ja mis tahes mahtu, mis on seotud helitugevuse container kustutamine. Pärast toimingu korral tühjendage teemasse teatiste lehe kaudu.|
|<*arv*> login katsete <*elemendi ID*> nurjus.|Mitme nurjunud püüab.|Teie seade võib olla rünnaku all või autoriseeritud kasutaja proovib ühendust vale parool.<ul><li>Võtke ühendust oma autoriseeritud kasutajad ja veenduge, et need katsed olid pärit usaldusväärsest allikast. Kui jätkate vaatamiseks suure hulga nurjunud sisselogimise katsete, kaaluge keelamise kaughalduse ja poole pöördumist võrguadministraatori poole. Pärast toimingu korral tühjendage teemasse teatiste lehe kaudu.</li><li>Veenduge, et teie haldur hetktõmmise eksemplarid on konfigureeritud õige parool. Pärast toimingu korral tühjendage teemasse teatiste lehe kaudu.</li></ul>Lisateabe saamiseks minge [on aegunud seadme parooli muutmine](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password).|
|Krüptovõtme teenuse andmete muutmisel ilmnes üks või mitu ebaõnnestumist.||Tõrkeid ilmnes teenuse andmete krüptovõtme muutmine. Pärast on adresseeritud tõrke tingimustega, käivitage selle `Invoke-HcsmServiceDataEncryptionKeyChange` StorSimple liides Windows PowerShelli cmdlet teenuse värskendada oma seadmes. Kui see probleem ei lahene, pöörduge Microsofti klienditoe poole. Pärast selle probleemi lahendamiseks tühjendage teemasse teatiste lehe kaudu.|

### <a name="support-package-alerts"></a>Tugiteenuste pakett teatised

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Tugiteenuste paketi loomine nurjus.|StorSimple ei saanud luua paketi.|Toimingut. Kui probleem ei lahene, pöörduge Microsoft Support. Kui probleem on lahendatud, tühjendage teemasse teatiste lehe kaudu.|

### <a name="update-alerts"></a>Hoiatused

|Teatise tekst|Sündmuse|Lisateavet / Soovitatavad toimingud|
|:---|:---|:---|
|Kiirparandus on installitud.|Tarkvara/püsivara värskendamine on lõpule viidud.|Teie seadmes on edukalt installitud kiirparandus.|
|Värskenduste käsitsi saadaval.|Esitamist saadaolevad värskendused.|Windows PowerShelli kasutajaliidese abil StorSimple seadme installima järgmised värskendused. <br>Lisateabe saamiseks avage [seadme StorSimple 8000 seeria värskendada](storsimple-update-device.md).|
|Uued värskendused on saadaval.|Esitamist saadaolevad värskendused.|**Haldamise** lehe kaudu või oma seadmes StorSimple Windows PowerShelli kasutajaliidese abil saate installida järgmised värskendused. <br>Lisateabe saamiseks avage [seadme StorSimple 8000 seeria värskendada](storsimple-update-device.md).|
|Värskenduste installimine nurjus.|Värskendused on installitud.|Teie süsteemi ei saanud installida. **Haldamise** lehe kaudu või oma seadmes StorSimple Windows PowerShelli kasutajaliidese abil saate installida järgmised värskendused. Kui probleem ei lahene, pöörduge Microsoft Support. <br>Lisateabe saamiseks avage [seadme StorSimple 8000 seeria värskendada](storsimple-update-device.md).|
|Ei saa automaatselt uue värskendusi.|Automaatse nurjus.|Saate käsitsi uute värskenduste **haldamise** lehe kaudu.|
|Uue WUA agent saadaval.|Saadaval värskenduse teatis.|Uusima Windows Update Agent alla laadida ja installida Windows PowerShelli kasutajaliidese kaudu.|
|Riistvaraga püsivara komponendi <*komponendi ID*> versioon ei vasta.|Püsivara update(s) on installitud.|Pöörduge Microsofti klienditoe poole.|

## <a name="next-steps"></a>Järgmised sammud

Lisateave [StorSimple tõrked ja tõ funktsionaalseid seadmes](storsimple-troubleshoot-operational-device.md).

