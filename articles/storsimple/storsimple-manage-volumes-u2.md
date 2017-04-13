<properties
   pageTitle="Hallata oma StorSimple mahud (U2) | Microsoft Azure'i"
   description="Selgitatakse, kuidas lisada, muuta, jälgimine ja kustutada StorSimple mahud ja võrguühenduseta neid vajaduse korral tehke järgmist."
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
   ms.date="10/28/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes-update-2"></a>StorSimple halduri teenuse abil saate hallata mahud (Värskenda 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Ülevaade

Selles õppeteemas selgitatakse, kuidas StorSimple halduri teenuse abil luua ja hallata mahud StorSimple seadme ja StorSimple virtuaalse seadme abil installitud Update 2.

StorSimple halduri teenus on laiend Azure klassikaline portaalis, mille abil saate hallata oma StorSimple lahenduse ühe web kasutajaliidese kaudu. Lisaks haldamise maht, saate StorSimple halduri teenuse loomine ja haldamine StorSimple teenuste, vaadata ja seadmete haldamine, teatised, vaadata ja vaadata ja hallata varukoopia poliitika ja varunduse kataloogis.

## <a name="volume-types"></a>Helitugevuse tüübid

StorSimple maht võib olla:

- **Kohalik kinnitatud maht**: andmete need mahud oleks kohaliku StorSimple seadme igal ajal.
- **Mitmetasandilise mahud**: andmete need mahud võivad kõrvalmõju pilveteenusesse.

On arhiivimine maht on teatud tüüpi astmeline helitugevust. Arhiivimise mahud kasutatakse korduste eemaldamise kogumi suuremaks võimaldab seadme suuremat sektorit andmete edastamiseks pilveteenusesse. 

Kui vaja, saate muuta helitugevuse kohalikust mitmetasandilise või kaudu mitmetasandilise kohalik. Lisateabe saamiseks minge [Helitugevuse tüübi muutmine](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Kohalik kinnitatud mahud

Kohalik kinnitatud on täielikult ettevalmistatud draividel, mida teha pole taseme andmete pilveteenusesse, tagades kohaliku tagab lähteandmeid sõltumatu pilvepõhist ühenduvust. Andmete kohalikult kinnitatud draividel pole deduplicated ja tihendatud; siiski hetktõmmiste kohalikult kinnitatud maht on deduplicated. 

Kohalik kinnitatud maht on täielikult ette valmistatud; Seetõttu peab teil olema teie seadmes kui loote neid piisavalt ruumi. Saate ettevalmistamise kohalikult kinnitatud mahud kuni maksimummaht 8 TB seadmes StorSimple 8100 ja 8600 seadme 20 TB. StorSimple jätab ülejäänud kohaliku ruumi seadmes pilte, metaandmete ja andmete töötlemiseks. Te saate suurendada kohalikult kinnitatud mahu ülempiir ruumi, mis on saadaval, kuid te ei saa vähendamine ühe korra loodud maht.

Kohalik kinnitatud helitugevuse loomisel on vähendatud astmeline mahud loomine vaba ruumi. Kehtib ka vastupidi: kui teil on olemasolev astmeline maht, jaoks loomise kohalikult kinnitatud mahud ruumi on väiksem kui eespool ülempiirid. Kohaliku draividel lisateabe saamiseks vaadake [kohalikult kinnitatud draividel korduma kippuvad küsimused](storsimple-local-volume-faq.md).   

### <a name="tiered-volumes"></a>Astmeline mahud

Astmeline on õhukesed ettevalmistatud draividel sageli külastatud andmete jääb kohaliku seadmes ja vähem sagedamini kasutatavad andmed on automaatselt mitmetasandilise pilveteenusesse. Õhuke ettevalmistamise on virtualization tehnoloogia saadaolevat näib ületada füüsilised ressursid. Asemel broneerimist piisavalt salvestusruumi eelnevalt, StorSimple kasutab õhuke ettevalmistamise eraldada praeguse nõuetele vastavuse lihtsalt piisavalt ruumi. Salvestusruumi elastne laadi hõlbustab see lähenemine, kuna StorSimple saate suurendada või vähendada salvestusruumi muutuvate vajaduste rahuldamiseks.

Kui kasutate astmeline helitugevuse arhiivimine andmed, märkige ruut **selle helitugevuse vähem sageli külastatud arhiivimine andmete jaoks kasutada** muudab korduste eemaldamise kogumi suurust draivi jaoks 512 KB. Kui valite selle suvandi, kasutatakse vastavate astmeline helitugevuse kogumi suurus 64 KB. Korduste eemaldamise kogumi suuremaks võimaldab seadme kiirendamiseks suurte arhiivimine andmete kandmine pilveteenusesse.

>[AZURE.NOTE] Arhiivimise mahud StorSimple eelse Update 2 versioonis loodud imporditakse nimega astmeline arhiivimine ruut märgitud.

### <a name="provisioned-capacity"></a>Ettevalmistatud võimsus

Vaadake järgmisest tabelist ettevalmistatud maksimaalne maht iga seadme ja helitugevuse tüüp. (Pange tähele, et kohalikult kinnitatud mahud pole saadaval, virtuaalse seadme.)

|             | Astmeline maksimaalne maht | Maksimum kohalikult kinnitatud maht |
|-------------|----------------------------|------------------------------------|
| **Füüsilise seadmed** |       |       |
| 8100                 | 64 TB | 8 TB |
| 8600                 | 64 TB | 20 TB |
| **Virtuaalne seadmed**  |       |       |
| 8010                | 30 TB | N/A   |
| 8020               | 64 TB | N/A   |

## <a name="the-volumes-page"></a>Lehe maht

Lehe **maht** võimaldab teil hallata Salvestusruumi mahud, mis on ette valmistatud Microsoft Azure StorSimple seadme jaoks oma algataja (serverid). See kuvab loendi mahud StorSimple seadmes.

 ![Lehe maht](./media/storsimple-manage-volumes-u2/VolumePage.png)

Sarja atribuute koosneb draiv:

- **Helitugevuse nimi** – kirjeldav nimi, mis peab olema kordumatu ja aitab tuvastada maht. See nimi on kasutusel järelevalve aruanded, kui valite kindla maht.

- **Olek** – võib olla võrguühenduse lubamine või keelamine. Kui maht on ühenduseta, ei kuvata on lubatud juurdepääs kasutada helitugevuse algataja (serverid).

- **Võimsus** – saate määrata kogusumma andmeid, mida saab salvestada algataja (server) alusel. Kohalik kinnitatud maht on täielikult ette valmistatud ja asuvad StorSimple seadme. Astmeline maht on õhukesed ette valmistatud ja andmed on deduplicated. Õhukesed ettevalmistatud draividel, kus eelnevalt ei seadme eraldada füüsilise mälumaht sees või pilveteenuses vastavalt konfigureeritud maht. Selle maht on eraldatud ja tarbitud nõudmisel.

- **Tippige** – näitab, kas maht on **testimiste** (vaikimisi) või **kohalikult kinnitatud**.

- **Varukoopia** – näitab, kas vaikepoliitika varukoopia olemas helitugevuse.

- **Juurdepääs** – saate määrata algataja (serverid), mis on lubatud juurdepääs sellele draivile. Algataja, et access juhtelement kirje (ACR), mis on seostatud maht ei ei kuvata maht.

- **Jälgimine** – määrab, kas jälgitakse maht. Maht on vaikimisi lubatud, kui see on loodud jälgimine. Jälgimine saavad, kuid, olla keelatud helitugevuse klooni. Luba jälgimine maht, järgige [kuvari maht](#monitor-a-volume). 

Selle õpetuse juhised abil teha järgmist:

- Lisage maht 
- Muutke maht 
- Helitugevuse muutmine
- Kustutage maht 
- Võrguühenduseta maht 
- Kuvari maht 

## <a name="add-a-volume"></a>Lisage maht

Teie [loodud maht](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) StorSimple lahenduse juurutamisel. Lisamine maht on sarnane protseduur.

#### <a name="to-add-a-volume"></a>Lisada maht

1. Klõpsake lehel **seadmed** valige seade, topeltklõpsake seda ja seejärel klõpsake vahekaarti **Helitugevuse ümbriste** .

2. Valige loendist helitugevuse ümbris ja topeltklõpsake seda seostatud ümbris mahud juurdepääsu.

3. Klõpsake lehe allosas **Lisa** . Lisa helitugevuse viisard käivitatud.

     ![Helitugevuse viisardiga põhisätted](./media/storsimple-manage-volumes-u2/TieredVolEx.png)

4. Helitugevuse viisardit, klõpsake jaotises **Põhisätted**lisamine tehke järgmist.

  1. Lisage oma helitugevuse **nimi** .
  2. Valige loendist drop-down **Kasutamine tüüp** . Töökoormus, mis nõuavad andmete kohalikult seadmes saadaval igal ajal, valige **Kohalik kinnitatud**. Kõik muud tüüpi andmete puhul valige **testimiste**. (**Testimiste** on vaikimisi).
  3. Kui olete valinud **testimiste** etappi 2, valite mõne arhiivimine helitugevuse konfigureerimiseks ruut **Kasuta see vähem sageli külastatud arhiivimine andmete maht** .
  4. Sisestage oma draivi GB või TB **Ette valmistatud võimsus** . Vaadake [Provisioned võimsus](#provisioned-capacity) Max suurused iga seadme ja helitugevuse tüüp. Vaadake **Olemasoleva võimsuse** kindlaks teha, kui palju salvestusruumi on tegelikult saadaval teie seadmes.

5. Klõpsake noolt ikooni![Nooleikooni](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Kui olete konfigureerinud kohalikult kinnitatud helitugevust, kuvatakse järgmine teade.

    ![Tippige sõnumite helitugevuse muutmine](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
   
5. Nooleikooni ![nooleikooni](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)minna lehele **Täiendavad sätted** .

    ![Helitugevuse viisardi täiendavate sätete lisamine](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>

6. Klõpsake jaotises **Täpsemad sätted**uue Accessi juhtelement kirje (ACR) lisamiseks tehke järgmist.
  
  1. Valige loendist drop-down juurdepääsu juhtelement kirje (ACR). Teise võimalusena saate lisada uue ACR. ACRs kindlaks teha, mis hosts pääsevad teie mahus sobitamine host IQN, et loendis kirje. Kui määrate mõne ACR, kuvatakse järgmine teade.

        ![Määrake ACR](./media/storsimple-manage-volumes-u2/SpecifyACR.png)

  2. Soovitame teil valida märkeruut **Luba selle draivi jaoks vaikimisi varukoopia** .
  3. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) helitugevuse loomiseks määratud sätetega.

Teie uus draiv on kasutamiseks valmis.

>[AZURE.NOTE] Kui luua kohalikult kinnitatud maht ja seejärel looge teine kohalik kinnitatud helitugevuse kohe pärast seda, helitugevuse loomine töö käivitada järjest. Esimese helitugevuse loomine töö peab lõpule enne alustamist järgmise helitugevuse loomine töö.

## <a name="modify-a-volume"></a>Muutke maht

Kui teil on vaja laiendada või hosts, et juurdepääs Helitugevuse muutmiseks, saate muuta maht.

> [AZURE.IMPORTANT] 
>
> - Kui seadme helitugevuse suuruse muutmiseks on vaja muuta ka hosti mahu suurus. 
> - Host-side siin kirjeldatud juhised kehtivad Windows Server 2012 (2012R2). Linux või muud opsüsteemid host teistsugused. Vaadake oma host operatsioonisüsteemi juhiseid muu opsüsteemiga hosti helitugevuse muutmisel. 

#### <a name="to-modify-a-volume"></a>Muuta maht

1. Klõpsake lehel **seadmed** valige seade, topeltklõpsake seda ja seejärel klõpsake vahekaarti **Helitugevuse ümbriste** .

2. Valige loendist helitugevuse ümbris ja topeltklõpsake seda vaadata seostatud ümbris mahud.

3. Valige draivi ja klõpsake lehe allosas nuppu **Muuda**. Muuda helitugevuse viisard käivitub.

4. Muuda helitugevuse viisardis jaotises **Põhisätted**saate teha järgmist:

  - Redigeerige **nimi**.
  - Teisendamine **Kasutamine tüüp** : Kohalik kinnitatud abil mitmetasandilise või et mitmetasandilise kaudu kohalikult kinnitatud (Lisateavet leiate [Helitugevuse tüübi muutmine](#change-the-volume-type) ).
  - **Ette valmistatud võimsus**suurendamine Ainult tõsta **Ette valmistatud võimsus** . Te ei saa mahtu vähendada pärast selle loomist.

5. Klõpsake jaotises **Täpsemad sätted**saate muuta iga-aastase, tingimusel, et helitugevus on ühenduseta. Kui maht on veebis, peate esmalt ühenduseta võtta. Vaadake juhiseid [võtta maht ühenduseta](#take-a-volume-offline) enne iga-aastase muutmine.

    > [AZURE.NOTE] Te ei saa muuta helitugevuse suvandi **lubamiseks vaikimisi varukoopia** .

6. Muudatuste salvestamiseks klõpsake ikooni Otsi ![sisse-ikoon](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). Azure'i klassikaline portaalis kuvatakse meilisõnumi värskendamine helitugevust. See kuvab edu sõnum, kui maht on nüüd värskendatud.

7. Kui on laiendamine maht, täitke järgmised juhised Windowsi hosti arvutis:

   1. Avage **arvuti haldamine** ->**ketta haldus**.
   2. Paremklõpsake **Ketta haldus** ja valige **Ketast uuesti**.
   3. Loendit ketast, valige soovitud maht, mida saate värskendada, paremklõpsake seda ja seejärel valige **Laiendamine helitugevuse**. Käivitub laiendamine helitugevuse viisard. Klõpsake nuppu **edasi**.
   4. Täitke viisardi vastu võtmist vaikeväärtused. Kui viisard on lõpule jõudnud, peaksite Helitugevuse kuva suurema suuruse.

    >[AZURE.NOTE] Kui laiendada kohalikult kinnitatud helitugevust ja seejärel laiendage teise kohalikult kinnitatud helitugevuse kohe, kui hiljem helitugevuse laiendamise töö käivitada järjest. Esimene helitugevuse laiendamise töö peab lõpule enne alustamist järgmise helitugevuse laiendamise töö.

![Video saadaval](./media/storsimple-manage-volumes-u2/Video_icon.png) **Video saadaval**

Vaadake videot, mis näitab, kuidas laiendada maht, klõpsake [siin](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-the-volume-type"></a>Helitugevuse muutmine

Saate muuta helitugevuse tüüp: mitmetasandilise soovite kohalikult kinnitatud või kaudu kohalikult kinnitatud juurde astmeline. Selle tulemus ei tohiks olla sagedamini esinemiskord. Mõned põhjused, miks teisendamise draiv: mitmetasandilise kohalikult kinnitatud on:

- Kohaliku tagatud andmete saadavus ja jõudlus
- Pilveteenuse latentsused ja pilveteenuse ühenduvusprobleemide kõrvaldamiseks.

Tavaliselt on need small olemasolevate köidete, mida soovite sageli juurde. Kohalik kinnitatud helitugevus on ette valmistatud täielikult loomisel. Kui teisendate astmeline helitugevuse kohalikult kinnitatud helitugevuse, StorSimple kinnitab, et teil on piisavalt ruumi oma seadmes enne selle algust teisendamise. Kui teil on piisavalt ruumi, kuvatakse tõrge ja toiming tühistada. 

> [AZURE.NOTE] Enne alustamist teisendus: mitmetasandilise soovite kohalikult kinnitatud, veenduge, et kaaluge oma muude töökoormus ruumi nõuetele. 

Võite kohalikult kinnitatud helitugevuse astmeline Helitugevuse muutmiseks, kui teil on vaja täiendavat ruumi ettevalmistamise teiste mahud. Kui teisendate kohalikult kinnitatud kogus mitmetasandilise, on vabastatud võimsuse suuruse suurendamine seadme võimsus. Kui ühenduvusprobleemide takistavad maht üleminekul kohaliku tüüp astmeline tüüp, ent kohaliku helitugevuse astmeline atribuutidesse kuni tulemus on lõpetatud. See on mõned andmed võivad olla jäänud pilveteenusesse. Mahavalgunud andmed jäävad hõivata kohaliku ruumi seadmes, mida ei saa vabastada seni, kuni toiming on taaskäivitada ja lõpule viidud.

>[AZURE.NOTE] Teisendamine maht võib võtta aega ja te ei saa tühistada teisendus pärast käivitamist. Teisendamisel, jääb võrgus helitugevuse ja tehtavad varukoopiate, kuid te ei saa laiendamine või ajal teisendamise toimub taastamine.  

Üleminekul on astmeline kohalikult kinnitatud helitugevuse võivad negatiivselt mõjutada seadme jõudlust. Lisaks järgmisi tegureid võib suurendada ülemineku aega:

- Seal on piisavalt ribalaiust.

- On praeguse varukoopiat.

Minimeerimiseks efektid, mis võib olla teguritest:

- Vaadake üle oma pidurdamise poliitikate läbilaskevõime ja veenduge, et sihtotstarbeline 40 Mbps läbilaskevõime on saadaval.
- Teisendamise jaoks aegsasti kavandada.
- Pilveteenuse hetktõmmise tegemiseks saate enne teisendamist.

Kui teisendate mitu draivi (toetavad erinevaid töökoormus), siis tuleks eriti helitugevuse teisendamise nii, et suuremat prioriteet hulka esmalt teisendatakse. Näiteks peaks teisendada virtuaalmasinates (VM) majutavad draivid või mahud koos SQL-i töökoormus enne, kui teisendate mahud abil faili ühiskasutusse andmine töökoormus.

#### <a name="to-change-the-volume-type"></a>Helitugevuse tüüpi muuta

1. Klõpsake lehel **seadmed** valige seade, topeltklõpsake seda ja seejärel klõpsake vahekaarti **Helitugevuse ümbriste** .

2. Valige loendist helitugevuse ümbris ja topeltklõpsake seda vaadata seostatud ümbris mahud.

3. Valige draivi ja klõpsake lehe allosas nuppu **Muuda**. Muuda helitugevuse viisard käivitub.

4. Lehel **Põhisätted** muuta, valides uut tüüpi **Kasutamine tüüp** ripploendist kasutamine tüüp.

    - Kui tüüp on muutmise **kohalikult kinnitatud**, kontrollib StorSimple kas on piisavalt.
    - Kui muudate **testimiste** abil tüüp ja selle maht kasutatakse andmete arhiivimine, märkige ruut **selle helitugevuse vähem sageli külastatud arhiivimine andmete jaoks kasutada** .

        ![Arhiivi märkeruut](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)

5. Nooleikooni ![nooleikooni](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) lehele **Täiendavaid sätteid** . Kui olete konfigureerinud kohalikult kinnitatud helitugevust, kuvatakse järgmine teade.

    ![Tippige sõnumite helitugevuse muutmine](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)

6. Klõpsake noolt ikooni ![nooleikooni](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) uuesti jätkata.

7. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) alustamiseks teisendamine. Azure'i portaalis kuvatakse meilisõnumi värskendamine helitugevust. See kuvab edu sõnum, kui maht on nüüd värskendatud.

## <a name="take-a-volume-offline"></a>Võrguühenduseta maht

Kui peate tegema maht ühenduseta, kui kavatsete seda muuta või kustutada. Kui maht on ühenduseta, pole saadaval lugemis-ja kirjutamisõigused. Peate võtma helitugevuse ühenduseta hosti samuti seadmes. 

#### <a name="to-take-a-volume-offline"></a>Kui soovite maht võrguühenduseta

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

#### <a name="to-delete-a-volume"></a>Kustutamiseks maht

1. Klõpsake lehel **seadmed** valige seade, topeltklõpsake seda ja seejärel klõpsake vahekaarti **Helitugevuse ümbriste** .

2. Valige helitugevuse ümbris, mille maht, mille soovite kustutada. Klõpsake helitugevuse ümbris **mahud** lehele.

3. Kõik selles ümbrises seostatud mahud kuvatakse tabelina. Oleku maht, mille soovite kustutada. Kui maht, mille soovite kustutada pole ühenduseta, võrguühenduseta see esmalt juhistele [võtta maht ühenduseta](#take-a-volume-offline).

4. Pärast maht on ühenduseta, klõpsake lehe allservas **kustutada** .

5. Kinnituse küsimisel nuppu **Jah**. Helitugevuse kustutatakse ja **mahud** leht kuvatakse mahud s.o ümbris, mille jooksul värskendatud loendit.

    >[AZURE.NOTE]Kui kustutate kohalikult kinnitatud helitugevust, ei värskendata uue mahud ruumi kohe. StorSimple halduri teenuse värskendab kohaliku ruumi perioodiliselt. Soovitame teil oodake paar minutit enne, kui proovite luua uus draiv.<br> Lisaks kui kustutate kohalikult kinnitatud helitugevust ja seejärel kustutage teise kohalikult kinnitatud helitugevuse kohe pärast seda, helitugevuse kustutamise töö käivitada järjest. Esimene helitugevuse kustutamise töö peab lõpule enne alustamist järgmise helitugevuse kustutamise töö.
 
## <a name="monitor-a-volume"></a>Kuvari maht

Helitugevuse jälgimine võimaldab teil maht ma/O seotud statistika kogumiseks. Jälgimine on vaikimisi esimene 32 maht, mille loote. Vaikimisi on keelatud täiendava jälgimine. Vaikimisi ka keelatud kloonitud mahud jälgimine.

Järgmiste toimingute lubamiseks või keelamiseks maht jälgimine.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Lubada või keelata helitugevuse jälgimine

1. Klõpsake lehel **seadmed** valige seade, topeltklõpsake seda ja seejärel klõpsake vahekaarti **Helitugevuse ümbriste** .

2. Valige helitugevuse ümbris, kus asub maht, ja klõpsake helitugevuse container **mahud** lehele.

3. Kõik selles ümbrises seostatud mahud on loetletud tabeli kuvamine. Klõpsake ja valige helitugevuse või helitugevuse klooni.

4. Klõpsake lehe allosas nuppu **Muuda**.

5. Jaotises **Põhisätted**viisardis Helitugevuse muutmiseks valige **lubamine** või **keelamine** **jälgimis** ripploendist.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas [klooni StorSimple helitugevust](storsimple-clone-volume.md).

- Siit saate teada, [StorSimple halduri teenuse haldamiseks StorSimple seadme kasutamise](storsimple-manager-service-administration.md)kohta.

 
