<properties
   pageTitle="Hallata StorSimple seadme kontrollerid | Microsoft Azure'i"
   description="Siit saate teada, kuidas lõpetada, uuesti, sulgeda või lähtestage oma StorSimple seadme kontrollerid."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="manage-your-storsimple-device-controllers"></a>Teie StorSimple seadme kontrollerid haldamine

## <a name="overview"></a>Ülevaade

Selles õppetükis kirjeldatakse erinevaid toiminguid, mida saab teha oma StorSimple seadme kontrollerid. Funktsiooni draiverid StorSimple teie seadmes on üleliigsed (omavahelistes) domeenikontrollerid konfigureerimisel aktiivne passiivne. Kindlal ajahetkel ainult üks kontrolleril on aktiivne ja töötleb kõik kettale ja võrgu toimingud. Muud kontrolleril on passiivne režiim. Kui aktiivne kontrolleril nurjub, passiivne kontrolleril aktiveerub automaatselt.

Õppeteema sisaldab üksikasjalike juhiste abil seadme kontrollerid haldamise on:

- **Kontrollerid** osas **hooldusvaates StorSimple halduri teenus**
- Windows PowerShelli StorSimple.

Soovitame, et haldate seadme kontrollerid StorSimple halduri teenuse kaudu. Kui toimingu saab ainult Windows PowerShelli abil StorSimple, õpetuse teeb selle üles.

Pärast selle õpetuse lugemist, on võimalik:

- Taaskäivitage või sulgeda StorSimple seadme domeenikontrolleri
- Sulgege StorSimple seadme
- Seadme StorSimple algsätted lähtestamine


## <a name="restart-or-shut-down-a-single-controller"></a>Taaskäivitage või sulgeda ühe domeenikontrolleri

Selle domeenikontrolleri taaskäivitus või sulgemine ei ole vaja tavaline kasutamine osana. Toimingute jaoks ühe seadme kontrolleril on levinud ainult juhul, kus nurjunud seadme riistvara komponent nõuab asendamine. Selle domeenikontrolleri uuesti nõuda ka liigse mälukasutuse või probleemset kontrolleril mõjutatud jõudluse olukorras. Peate taaskäivitama on selle domeenikontrolleri pärast eduka kontrolleril asendamine, kui soovite lubada ja testida asemel on nüüd kontrolleril.

Seadme taaskäivitamine pole segab ühendatud algataja, eeldades, et passiivne kontrolleril on saadaval. Kui passiivne kontrolleril pole saadaval või välja lülitatud väljas, siis taaskäivitada aktiivne kontrolleril võib kaasa tuua tööseisakute ning teenuse häirete.

> [AZURE.IMPORTANT]

> - **Töötava kontrolleril peaks kunagi olla viidud nagu vähenemist koondamise ja kaasneb tööseisakute tulemuseks.**

> - Järgmine juhis kehtib StorSimple seadme. Käivitamine ja peatamine virtuaalse taaskäivitada kohta leiate lisateavet teemast [töötamine virtuaalse seadmega](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device).

Saate taaskäivitage või sulgeda ühe seadme kontrolleril StorSimple Azure klassikaline portaali StorSimple halduri teenuse või Windows PowerShelli abil

Azure'i klassikaline portaali kaudu hallata oma seadme kontrollerid, tehke järgmist.

#### <a name="to-restart-or-shut-down-a-controller-in-classic-portal"></a>Taaskäivitage või mõne selle domeenikontrolleri klassikaline portaalis

1. Liikuge **seadmed > hooldustööd**.

1. Minge **Riistvara oleku** ja veenduge, et hulka kuuluvad nii selle seadme olek oleks **terve**.

    ![Veenduge, et StorSimple seadme draiverid on terve](./media/storsimple-manage-device-controller/IC766017.png)

1. **Haldamise** lehe allosas nuppu **Halda kontrollerid**.

    ![StorSimple seadme kontrollerid haldamine](./media/storsimple-manage-device-controller/IC766018.png)</br>

    >[AZURE.NOTE] Kui te ei näe **Kontrollerid haldamine**, siis peate värskenduste installimine. Lisateavet leiate teemast [StorSimple seadme](storsimple-update-device.md).

1. **Selle domeenikontrolleri sätete muutmine** dialoogiboksis tehke järgmist.
    1. Valige ripploendist **Valige selle domeenikontrolleri** domeenikontrolleri, mida soovite hallata. Valikud on kontrolleril 0 ja 1 kontrolleril. Nende kontrollerid ka tuvastatud aktiivne või passiivne.

        >[AZURE.NOTE] Mõne selle domeenikontrolleri ei saa hallata, kui see on saadaval või välja lülitatud väljas ja rippmenüü loendis ei kuvata.

    2. Valige ripploendist **Vali toiming** **taaskäivitate selle domeenikontrolleri** või **domeenikontrolleri sulgeda**.

        ![Taaskäivitage StorSimple seadme passiivne domeenikontrolleri](./media/storsimple-manage-device-controller/IC766020.png)
    3. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-manage-device-controller/IC740895.png).

See uuesti või selle domeenikontrolleri sulgeda. Järgmises tabelis on kokkuvõte üksikasjad, mis juhtub sõltuvalt **Selle domeenikontrolleri sätete muutmine** dialoogiboksis on tehtud valikutega.  


|Valiku #|Kui valite...|See juhtub.|
|---|---|---|
|1.|Taaskäivitage passiivne kontrolleril.|Töö luuakse uuesti selle domeenikontrolleri ja teid teavitatakse pärast töö loomist edukalt. See alustab kontrolleril uuesti. Saate jälgida taaskäivitamisel protsessi juurdepääs **teenus > armatuurlaua > Logi vaadata** ja seejärel filtreerimine parameetritega teenust.|
|2.|Taaskäivitage active kontrolleril.|Kuvatakse järgmine hoiatus: "active kontrolleril taaskäivitamisel seade ei õnnestu üle passiivne kontrolleril. Kas soovite jätkata?" </br>Jätkata seda toimingut valimist järgmiste juhiste korral samamoodi uuesti passiivne kontrolleril (vt valiku 1).|
|3.|Passiivne kontrolleril sulgeda.|Kuvatakse järgmine teade: "kui sulgumist on lõpule jõudnud, peate nupp power kontrolleril sisse lülitada. Kas olete kindel, et soovite selle domeenikontrolleri sulgeda?" </br>Jätkata seda toimingut valimist järgmiste juhiste korral samamoodi uuesti passiivne kontrolleril (vt valiku 1).|
|4.|Sulgege active kontrolleril.|Kuvatakse järgmine teade: "kui sulgumist on lõpule jõudnud, peate nupp power kontrolleril sisse lülitada. Kas olete kindel, et soovite selle domeenikontrolleri sulgeda?" </br>Jätkata seda toimingut valimist järgmiste juhiste korral samamoodi uuesti passiivne kontrolleril (vt valiku 1).|


#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Taaskäivitage või mõne selle domeenikontrolleri Windows PowerShelli StorSimple
Järgmiste toimingute sulgeda või taaskäivitage ühe domeenikontrolleri StorSimple seadme Azure klassikaline portaalist.


1. Juurdepääs seadme järjestikune konsooli või Telneti seansi kaugarvutist abil. Ühenduse kontrolleril 0 või 1 kontrolleril [kasutamine](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)Putty seadme järjestikune konsooli ühenduse juhiste järgi.

1. Järjestikune konsooli menüüs valik, 1, **logige sisse täielik juurdepääs**.

1. Plakat sõnumis, kirjutage kontrolleril, mis on seotud (kontrolleril 0 või domeenikontrolleri 1) ja kas see on aktiivne või passiivne (valige) kontrolleril.
    - Sulgeda ühe administraator, kuvatakse vastav viip, tippige tekst.

        `Stop-HcsController`

        Selle domeenikontrolleri, mis on seotud sulgeda. Kui lõpetate aktiivne kontrolleril, siis see ei õnnestu üle passiivne kontrolleril enne selle lõpetatakse.
    - Taaskäivitage administraator, kuvatakse vastav viip, tippige:

        `Restart-HcsController`

        Selle domeenikontrolleri, mis on seotud uuesti. Kui aktiivne kontrolleril taaskäivitamiseks see ei õnnestu üle passiivne kontrolleril enne taaskäivitamist.


## <a name="shut-down-a-storsimple-device"></a>Sulgege StorSimple seadme

Selles jaotises selgitatakse, kuidas sulgeda jooksva või nurjunud StorSimple seadme kaugarvutist. Seade on välja lülitatud pärast seadet kontrollerid sulgemise. Seadme sulgumist tehakse, kui seadme paigutatakse füüsilise või kasutusest.

> [AZURE.IMPORTANT] Enne seadme sulgemiseks seadme komponentide seisundi kontrollimiseks. Liikuge **seadmed > hooldus > riistvara oleku** ja veenduge, et kõik komponendid LED oleku roheline. Ainult terve seade on roheline olek. Kui teie seade on suletud, et asendada probleemset osa, kuvatakse nurjunud (punane) või halvenenud (kollane) olek jaoks vastavad komponendid.

#### <a name="to-shut-down-a-storsimple-device"></a>Sulgeda StorSimple seadme

1. Toiminguga [taaskäivitada või sulgege on selle domeenikontrolleri](#restart-or-shut-down-a-single-controller) tuvastamine ja sulgeda passiivne kontrolleril seadmes. Saate teha selle toimingu Azure klassikaline portaalis või Windows PowerShelli StorSimple.
2. Korrake ülaltoodud juhiseid active kontrolleril sulgeda.
3. Nüüd peate pilk tagasi lennuki seade. Pärast kaks kontrollerid täielikult sulgeda, tuleks nii kontrolörid oleku LED vilgub punane. Kui teil on vaja välja lülitada seadme täielikult sel ajal, pöörata power parameetrid Power nii jahutus moodulid (PCMs) olekusse väljas. See peaks seadme välja lülitada.


<!--#### To shut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect to the serial console of the StorSimple device by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In the serial console menu, verify from the banner message that the controller you are connected to is the passive controller. If you are connected to the active controller, disconnect from this controller and connect to the other controller.

1. In the serial console menu, choose option 1, **log in with full access**.

1. At the prompt, type:

    `Stop-HCSController`

    This should shut down the current controller. To verify whether the shutdown has finished, check the back of the device. The controller status LED should be solid red.

1. Repeat steps 1 through 4 to connect to the active controller and then shut it down.

1. After both the controllers are completely shut down, the status LEDs on both should be blinking red. If you need to turn off the device completely at this time, flip the power switches on both Power and Cooling Modules (PCMs) to the OFF position.-->

## <a name="reset-the-device-to-factory-default-settings"></a>Lähtestage seade algsätted

> [AZURE.IMPORTANT] Kui teil on vaja oma seadme lähtestamiseks algsätted, pöörduge Microsoft Support. Allpool kirjeldatud tuleks kasutada ainult koos Microsoft Support.

See toiming kirjeldab Microsoft Azure StorSimple seadme lähtestamiseks Windows PowerShelli kaudu StorSimple algsätted.
Parooli lähtestamine eemaldab kõik andmed ja sätted kogu kobar vaikimisi.

Tehke algsätted Microsoft Azure StorSimple seadme lähtestamiseks tehke järgmist.

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Seadme lähtestamiseks vaikesätete Windows PowerShelli StorSimple

1. Juurdepääs seadme kaudu oma järjestikune konsooli. Plakat sõnumit tagamaks, et olete loonud ühenduse aktiivse kontrolleril kontrollida.

1. Järjestikune konsooli menüüs valik, 1, **logige sisse täielik juurdepääs**.

1. Kuvatakse vastav viip, tippige järgmine käsk Lähtesta kogu kobar, eemaldades kõik andmed, metaandmete ja selle domeenikontrolleri sätted:

    `Reset-HcsFactoryDefault`

    Ühe domeenikontrolleri hoopis lähtestamiseks cmdletiga [Lähtestamine-HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) abil soovitud `-scope` parameeter.)

    Käivitatakse süsteem uuesti mitu korda. Kui lähtestamine on edukalt lõpule, teavitatakse teid sellest. Sõltuvalt süsteemi mudel, võib kuluda 45-60 minutit 8100 seadme ja selle protsessi lõpuleviimiseks on 8600 60-90 minutit.

    > [AZURE.TIP]

    > - Kui kasutate Update 1.2 või mõnes varasemas versioonis kasutada funktsiooni `–SkipFirmwareVersionCheck` parameetri püsivara versioon kontrolli vahele (vastasel juhul näete püsivara lahknevuse tõrke: Factory lähtestamine ei saa jätkata, kuna vastuolu püsivara versioonid. ).

    > - Factory reset protseduur ei pruugi StorSimple seadmed, mis töötab Update 1 või 1.1 Government portaali ja on eduka ühe või kahe kontrolleril asendamine (koos asendamine kontrollerid, mis saadeti eelse Update 1 tarkvara). See juhtub, kui selle factory Lähtesta pilt on kinnitatud SHA1 faili kontrolleril, mida pole olemas eelse Update 1 tarkvara olemasolu. Kui näete selle factory lähtestamine tõrge, pöörduge Microsoft Support teid aidata järgmiste juhiste juurde. See probleem ei käsitleta asendamine kontrollerid, mis saadeti Factory Update 1 või uuem versioon tarkvaraga.


## <a name="questions-and-answers-about-managing-device-controllers"></a>Küsimused ja vastused seadme kontrollerid haldamise kohta

Selles jaotises oleme kokku mõningaid korduma kippuvaid küsimusi seoses haldamise StorSimple seadme kontrollerid.

**K.** Mis juhtub, kui mõlemad on draiverid, seadmes on terve ja sisse sisse ja ma taaskäivitage või sulgeda active kontrolleril?

**V-SSE.** Kui mõlemad on draiverid, teie seadmes on terve ja sisse, küsitakse kinnitust. Võite valida:

- **Taaskäivitage aktiivne kontrolleril** – kuvab teate, et taaskäivitada on aktiivne kontrolleril põhjustada seadme nurjumise üle passiivne kontrolleril. Selle domeenikontrolleri uuesti.

- **Sulgeda on aktiivne kontrolleril** – kuvab teate, et sulgemise on aktiivne kontrolleril tulemuseks tööseisakute. Samuti peate seadme sisselülitamiseks kontrolleril nupp power.

**K.** Mis juhtub, kui passiivne oma seadmes on saadaval või välja lülitatud väljas ja ma taaskäivitada või sulgege active kontrolleril?

**V-SSE.** Kui passiivne teie seadmes on saadaval või välja lülitatud väljas ja valite.

- **Taaskäivitage aktiivne kontrolleril** – teavitatakse teid jätkuva toimingu tulemuseks ajutine teenuse katkemise, ja teil palutakse kinnitust.

- **Sulgeda on aktiivne kontrolleril** – kuvab teate, et jätkuva toimingu tulemuseks tööseisakute ja, et peate power nupp ühele või mõlemale domeenikontrollerid seade sisse lülitada. Teil palutakse kinnitust.

**K.** Kui ei kontrolleril taaskäivitus või sulgemine edu nurjub?

**V-SSE.** Taaskäivitada või mõne selle domeenikontrolleri sulgemise võib nurjuda, kui:

- Seadme värskendamine on pooleli.

- Selle domeenikontrolleri uuesti on pooleli.

- Selle domeenikontrolleri sulgumist on pooleli.

**K.** Kuidas saab teie aru saada, kui mõnda selle domeenikontrolleri oli taaskäivitada või sulgeda?

**V-SSE.** Saate selle domeenikontrolleri oleku lehe hooldustööd. Selle domeenikontrolleri olek näitab, kas on selle domeenikontrolleri on uuesti või Sule. Lisaks sisaldab lehe teatiste teavitamise teatise selle domeenikontrolleri oli uuesti või Sule. Selle domeenikontrolleri uuesti ja sulgumist toimingud salvestatakse ka toimingu logid. Logi kohta lisateabe saamiseks minge [vaadata toiming logid](storsimple-service-dashboard.md#view-the-operations-logs).

**K.** Kas mõni mõju väljundtoimingute tulemusena kontrolleril Tõrkesiirde?

**V-SSE.** TCP seoseid algataja ja aktiivne kontrolleril lähtestatakse tulemusena kontrolleril Tõrkesiirde, kuid taastatakse passiivne kontrolleril toiming eeldab. Selle toimingu käigus võib olla I/O tegevuse algataja ja seadme vahel pausi ajutised (vähem kui 30 sekundit).

**K.** Kuidas saan tagasi minu domeenikontrolleri teenuse pärast sulgeda ja eemaldada?

**V-SSE.** Teenus on selle domeenikontrolleri naasmiseks peate lisama selle raami, nagu on kirjeldatud [asendada selle domeenikontrolleri mooduli StorSimple seadmes](storsimple-controller-replacement.md).

## <a name="next-steps"></a>Järgmised sammud

- Kui teil tekib probleeme oma StorSimple seadme kontrollerid ei lahene, kasutades toiminguid, mis on loetletud selles õpetuses, [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md).

- StorSimple halduri teenuse kasutamise kohta lisateabe saamiseks külastage [kasutamine StorSimple halduri teenuse haldamiseks StorSimple seadme](storsimple-manager-service-administration.md).
