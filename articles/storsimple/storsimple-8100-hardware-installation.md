<properties
   pageTitle="Installige seadme StorSimple 8100 | Microsoft Azure'i"
   description="Kirjeldab, kuidas lahti, mount koguda ja kaabel seadme StorSimple 8100 enne juurutada ja konfigureerida tarkvara."
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
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Lahti, sektsioon-mount ja seadme StorSimple 8100 kaabel

## <a name="overview"></a>Ülevaade

Teie Microsoft Azure'i StorSimple 8100 on ühe ruumi, sektsioon paigaldatud seade. Selles õppeteemas selgitatakse, kuidas lahti, sektsioon – kinnitus ning kaabel StorSimple 8100 seadme riistvara enne konfigureerimine ja juurutamine StorSimple seade.

## <a name="unpack-your-storsimple-8100-device"></a>Seadme StorSimple 8100 lahti

Järgmised toimingud pakuvad selge, üksikasjalikud juhised leiate oma StorSimple 8100 mäluseadmesse lahti. Kasutatav seade on saadetud üks kast.

### <a name="prepare-to-unpack-your-device"></a>Teie seade lahti ettevalmistamine

Enne, kui te lahti seadme, lugege läbi järgmine teave.

![Ikoon hoiatus](./media/storsimple-safety/IC740879.png)![raske ikooni](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **hoiatus!**

1. Veenduge, et teil on kaks inimest saadaval ruumi paksuse haldamine, kui teil on selle töötlemise käsitsi. Täielikult konfigureeritud ruum võib kaaluda kuni 32 kg (70 kg.).
1. Välja paigutamiseks tasapinnalise, taseme pind.

Järgmiseks täitke järgmised juhised teie seade lahti.

#### <a name="to-unpack-your-device"></a>Teie seade lahti.

1. Kontrollige väljal ja pakendit vaht purustav, kärpimine, vee kahju või selge kahju. Kui ruut või pakendil on oluliselt rikutud, avage dialoogiboks. Palun [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) , et saaksite otsustada, kas seade on töökorras.

2. Väljal lahti. Järgmisel pildil on kujutatud StorSimple seadme lahti vaate.

     ![Lahti seadme salvestusruum](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)

    **Seadme salvestusruumi lahti vaade**

     Sildi | Kirjeldus
     ----- | -------------
     1     | Välja
     2     | Alumine vaht
     3     | Seadme
     4     | Parimate
     5     | Keeletarvikute ruut


3. Pärast lahtipakkimine väljale, veenduge, et teil on:

   - 1 ühe ruumi seade
   - 2 juhtmed
   - 1 crossover Ethernet kaabel
   - 2 järjestikune konsooli kaabel
   - 1 USB-järjestikust muunduri järjestikune juurdepääsu
   - 1 kaitstud T10 kruvikeeraja
   - 4 QSFP – kui soovite-adapterit SFP + 10 New York võrgu liidesed kasutamiseks
   - 1 sektsioon-mount kit (2 külgpiiretega paigaldus riistvara)
   - Töö alustamine dokumentatsioon

    Kui te ei saanud ühte üksusi eelnimetatud [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md).

Järgmiseks on sektsioon-mount seadme.

## <a name="rack-mount-your-storsimple-8100-device"></a>Sektsioon-mount seadme StorSimple 8100

Järgige järgmisi juhiseid installimiseks oma StorSimple 8100 mäluseadmesse standard 19-tolli sektsioon ees-ja tagumine postitusi. StorSimple 8100 seadmel ühe esmane ruumi.

Installimise koosneb mitmest juhiseid, mis on kirjeldatud järgmistest toimingutest.

> [AZURE.IMPORTANT]
StorSimple seadmete peab olema sektsioon paigaldatud tööd.

### <a name="prepare-the-site"></a>Saidi ettevalmistamine

Seade peab olema installitud standard 19-tolli sektsioon, mis sisaldab esi- ja tagumine postitusi. Järgmiste toimingute abil sektsioon installi ettevalmistamine.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Saidi ettevalmistamine sektsioon installimine

1. Veenduge, et seade asetamist turvaline tasapinnalise, ühed ja taseme töö Surface'i (või sarnane).

2. Veenduge, et sait, kuhu kavatsete häälestamine on standardne AC power sõltumatu või sektsioon power jaotuse üksuse (Probleemse) Puhvertoiteallikas (UPS).

3. Veenduge, et 2U pesa on saadaval sektsioon, milles soovite ühendada seade.

![Ikoon hoiatus](./media/storsimple-safety/IC740879.png)![raske ikooni](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **hoiatus!**

Veenduge, et teil on kaks inimest haldamiseks kaalust, kui teil on käsitsemine häälestamine seadmes saadaval. Täielikult konfigureeritud ruum võib kaaluda kuni 32 kg (70 kg.).

### <a name="rack-prerequisites"></a>Sektsioon eeltingimused

Installi standard 19-tolli sektsioon kabineti, kus on mõeldud 8100 ruumi.

- Minimaalne sügavus sektsioon post postitamiseks 27.84 tolli.
- Suurim lubatud kaal 32 kg seadme
- Maksimaalne tagasi rõhk 5 Pascal (0,5 mm veesammast).

### <a name="rack-mounting-rail-kit"></a>Sektsioon – kinnitus raudtee komplekt

19-tolli sektsioon kabineti jaoks on esitatud monteerimise rööpad kogum. Rööpad on testitud käsitlema maksimaalne ruum paksus. Need rööpad võimaldab ka mitu Kirjalisandid kaotamata ruumis raami installi.


#### <a name="to-install-the-device-on-the-rails"></a>Seadme installimiseks rööpad

2. Seda juhist täitke ainult juhul, kui teie seadmes pole installitud sisemine rööpad. Tavaliselt on sisemine rööpad paigaldatud soovitud factory. Kui rööpad pole installitud, siis installige rail-vasakule ja paremale-rail slaide ruumi raami poolele. Nad lisada, kasutades kuus argumendil kruvid mõlemal küljel. Suund abil raudtee slaidid on märgitud **LH – esi** - ja **RH – ette**ja lõppu, mis on kinnitatud ruumi tahapoole on keritud end.<br/>

    ![Manustamise raudtee slaidide ruumi raami](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
    **kinnitamine sisemine raudtee slaidide poolele ruumi**

       Sildi | Kirjeldus
       ----- | -----------
       1     | M 3 x 4 nupp-head kruvid
       2     | Raami slaidid

3. Kinnitage väline vasakul raudtee ja väline õige raudtee assemblereid sektsioon kabineti vertikaalne liikmed. Sulud on tähistatud **LH**, **RH**ja **selle serva, kuni** juhatavad teid õige suund.

4. Otsige üles raudtee sõrmed esi -raudtee komplekti juures. Laiendamine raudtee mahutamiseks vahele sektsioon postitusi ja asetage esi- ja tagumine sektsioon postituse vertikaalne liikme auke. Veenduge, et raudtee komplekti on.

5. Kaks esitatud argumendil kruvid abil secure raudtee komplekti seadme ja vertikaalne liikmed. Kasutage ühte kruvi ees ja teine taga.

6. Korrake neid samme teiste raudtee komplekti.<br/>

     ![Manustamise raudtee slaidide kabineti sektsioon](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Manustamise väline raudtee assemblereid raami**

     Sildi | Kirjeldus
     ----- | -----------
     1     | Soovitud
     2     | Ruutu-Ava ees sektsioon postituse kruvi
     3     | Vasakul raudtee eesmise asukoha sõrmed
     4     | Soovitud
     5     | Vasakul raudtee tagumise asukoht sõrmed


### <a name="mounting-the-device-in-the-rack"></a>Ülaosa raami sisse

Kasuta sektsioon rööpad lihtsalt installitud, tehke järgmist ühendada seadme raami sisse.

#### <a name="to-mount-the-device"></a>Ühendada seade

1. Abiline, tõstke ruumi ja joondamiseks sektsioon rööpad.

2. Sisestage hoolikalt rööpad seade ja seejärel Lükake seade täielikult raam kabineti.<br/>

    ![Seadme raami lisamine](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Ülaosa raami sisse**


3. Vasak ja parem esi ääriku suurtähelukk Tõmbamine tasuta suurtähelukk eemaldada. Suurtähelukk ääriku snap lihtsalt peale Mõõtesilindri.

5. Turvaline ümbrist raam, installides ühe esitatud Phillips-kruvi läbi iga äärik, vasak ja parem.

4. Installige ääriku suurtähelukk vajutamise kohale ja haaramine neid kohas.<br/>

     ![Installimise ääriku suurtähelukk](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)

    **Suurtähelukk ääriku installimine**

     Sildi | Kirjeldus
     ----- | -----------
     1     | Ruumi kinnituse kruvi

Järgmiseks on kaabel seadme power, võrgu ja järjestikune juurdepääsu.

## <a name="cable-your-storsimple-8100-device"></a>Seadme StorSimple 8100 kaabel

Järgnevalt selgitatakse kaabel seadme StorSimple 8100 power, võrgu ja järjestikune ühendused.

### <a name="prerequisites"></a>Eeltingimused

Enne alustamist, et teie seadme kaabeldus, peate:

- Teie mäluseade, täielikult lahti ja sektsioon paigaldatud.

- 2 power kaabel oma seadmega kaasas

- Juurdepääs 2 Power jaotus ühikute (soovitatav).

- Võrgukaablid

- Esitatud järjestikune kaabel

- Järjestikune USB muundur koos sobiva draiveri arvutisse installinud (vajadusel)

- Esitatud 4 QSFP-et-adapterit SFP + 10 New York võrgu liidesed kasutamiseks

- [Toetatud riistvara 10 New York võrgu liidesed StorSimple seadmes](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)


### <a name="power-cabling"></a>Power kaabeldus

Seadme sisaldab liigsete Power ja jahutus moodulid (PCMs). Nii PCMs olema installitud ja ühendatud power eri allikatest kõrge kättesaadavuse tagamiseks.

Järgmiste toimingute abil kaabel seadme Power.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Võrgu kaabeldus

Teie seade on aktiivne-puhkerežiimis olev konfiguratsioon: korraga, üks kontrolleril moodul on aktiivne ja töötlemise ajal muid kontrolleril mooduli ketas ja võrgu kõik toimingud on puhkerežiimis. Kui mõnda selle domeenikontrolleri nurjub, valige kontrolleril aktiveeritakse kohe ja jätkub ketas ja võrgu toimingute.

Selle domeenikontrolleri liigsete Tõrkesiirde toetamiseks peate seadme võrgu kaabel, nagu on kirjeldatud järgmistes juhistes.

#### <a name="to-cable-for-network-connection"></a>Kui soovite võrguühenduse kaabel

1. Teie seade on kuus võrgu liidesed iga kontrolleril: neli 1 Gbit, ja kaks 10 Gbit Ethernet pordid. Erinevate andmete pordid backplane oma seadme tuvastada.

    ![Backplane 8100 seadme](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Tagasi seade, kus andmete pordid**

     Sildi   | Kirjeldus
     ------- | -----------
     0,1,4,5 |  1 New York võrgu liidesed
     2,3     | 10 New York võrgu liidesed
     6       | Järjestikune pordid

2. Võrgu kaabeldus jaoks järgmisel joonisel näha. (Miinimum võrgukonfiguratsioon kuvatakse sinine pidevjooned. Kõrge-saadavus ja jõudluse vajalik konfigureerimise kuvatakse punktiirjooni.)


    ![Kaabel 2U seadme, võrgu jaoks](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Võrgu kaabeldus teie seadme jaoks**


  	|Sildi | Kirjeldus |
  	|----- | ----------- |
  	| A    | Interneti-ühendusega LAN |
  	| B    | Selle domeenikontrolleri 0 |
  	| C    | PCM 0 |
  	| D    | Selle domeenikontrolleri 1 |
  	| E    | PCM 1 |
  	| F, G | Hosts |
  	| 0-5  | Võrgu liidesed |



Kui seade kaabeldus, minimaalne konfiguratsioon vaja:


- Vähemalt kaks võrgu liidesed ühendatud iga kontrolleril ühe pilvepõhise juurdepääsu ja üks iSCSI. ANDMETE 0 portide automaatselt lubatud ja konfigureeritud järjestikune konsooli seadme kaudu. Lisaks andmete 0, teise andmete pordi peab olema konfigureeritud Azure klassikaline portaali kaudu. Sel juhul andmete 0 ühendamine port esmane LAN (Interneti-ühendusega võrk). Muud andmed pordid saab ühendada SAN/iSCSI Kohtvõrgu (VLAN) lõigu võrgu, sõltuvalt ettenähtud roll.

- Liidesed iga kontrolleril kättesaadavuse tagamiseks kontrolleril Tõrkesiirde juhul samasse võrku ühendatud. Näiteks kui valite mõne hulka kuuluvad soovitud andmete 0 ja andmete 3 ühenduse, peate vastavaid andmeid 0 ja andmete 3 ühendada muude kontrolleril.

Pidage meeles, kõrge-saadavus ja jõudluse:


- Võimaluse korral konfigureerida kontrolleril iga paari võrgu liidese pilvepõhise juurdepääsu (1 New York) ja teise paar iSCSI (10 New York soovitatav).

- Võimaluse korral ühendage võrgu liidesed iga domeenikontrolleri kaks erinevad lülitid vastu vahetamise tõrge kättesaadavuse tagamiseks. Joonis kujutab kahe 10 New York võrgu liidesed, andmete 2 ja 3 andmeid: iga domeenikontrolleri ühendada kaks erinevat parameetrid.

Lisateabe saamiseks vaadake **võrgu liidesed** [kõrge-saadavus nõuded StorSimple seadme](storsimple-system-requirements.md#high-availability-requirements-for-storsimple)all.

>[AZURE.NOTE] Kui kasutate SFP + transiiverite oma 10 New York võrgu liidesed, kasutage pakutavaid QSFP-adapterit SFP +. Lisateabe saamiseks minge [toetatud riistvara 10 New York võrgu liidesed StorSimple seadmes](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).



### <a name="serial-port-cabling"></a>Järjestikune port kaabeldus

Järgmiste toimingute abil kaabel järjestikune pordile.

#### <a name="to-cable-for-serial-connection"></a>Järjestikune ühenduse kaabel

1. Teie seade on järjestikune port iga kontrolleril, mis on tähistatud mutrivõti ikooni. Vaadake pildil [võrgu kaabeldus](#network-cabling) jaotises seadme backplane järjestikune pordid leidmiseks.

2. Leidke aktiivse domeenikontrolleri sisse oma seadme backplane. Vilkuv sinine LED näitab on aktiivne.

3. Kasutage pakutavaid järjestikune kaabel (vajaduse korral sülearvuti USB-seeria muundur) ja ühendage oma konsooli või arvutis (koos terminal imiteerimist seadmele) aktiivne kontrolleril järjestikune porti.

4. Järjestikust-USB-draiverid (saadetud seadmega) oma arvutisse installida.

5. Häälestada järjestikune ühenduse järgmiselt: 115200 boodi, 8 andmete bittide, 1 stop bit, pole võrdse ja meilivoo kontrolli puudub.

6. Ühendus, vajutades klahvikombinatsiooni Enter konsooli töötamise kontrollimiseks. Järjestikune konsooli menüü peaks kuvatama.

>[AZURE.NOTE] **Lights-Out haldus**: kui seade on installitud remote andmekeskuse või arvuti toas piiratud juurdepääsuga, veenduge, et mõlemad kontrollerid järjestikune ühendused on alati ühendatud järjestikune konsooli aktiveerimine või sarnased seadmed. See võimaldab riba-, kaugjuhtimise ja tegevused, kui võrgu häireid või ootamatuid tõrkeid.

Teie seade on nüüd komplekslõng power, võrgule juurdepääsu ja järjestikune Ühenduvus. Järgmiseks on konfigureerimine tarkvara ja seadme juurutamine.

## <a name="next-steps"></a>Järgmised sammud

Siit saate teada, kuidas [juurutada ja konfigureerida kohapealse StorSimple seadme](storsimple-deployment-walkthrough.md).
