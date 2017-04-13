<properties
   pageTitle="Installige seadme StorSimple 8600 | Microsoft Azure'i"
   description="Kirjeldab, kuidas lahti, mount koguda ja kaabel seadme StorSimple 8600 enne juurutada ja konfigureerida tarkvara."
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
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Lahti, sektsioon-mount ja seadme StorSimple 8600 kaabel

## <a name="overview"></a>Ülevaade
Teie Microsoft Azure'i StorSimple 8600 on kaks ruumi seade ja koosneb esmane ja EBOD ruumi. Selles õppeteemas selgitatakse, kuidas lahti, sektsioon – kinnitus ning kaabel StorSimple 8600 seadme riistvara enne konfigureerimine StorSimple tarkvara.

## <a name="unpack-your-storsimple-8600-device"></a>Seadme StorSimple 8600 lahti

Järgmised toimingud esitada selge, üksikasjalikud juhised kohta, kuidas oma StorSimple 8600 mäluseadmesse lahti. Kasutatav seade on saadetud kaks ruudud, üks esmane ruumi ja teine EBOD ruumi. Need kaks kasti paigutatakse seejärel üks kast.

### <a name="prepare-to-unpack-your-device"></a>Teie seade lahti ettevalmistamine

Enne, kui te lahti seadme, lugege läbi järgmine teave.


![Ikoon hoiatus](./media/storsimple-safety/IC740879.png)![raske ikooni](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **hoiatus!**

1. Veenduge, et teil on kaks inimest haldamiseks seadme paksuse, kui teil on selle käsitsi töötlemise saadaval. Täielikult konfigureeritud ruum võib kaaluda kuni 32 kg (70 kg.).
1. Välja paigutamiseks tasapinnalise, taseme pind.

Järgmiseks täitke järgmised juhised teie seade lahti.

#### <a name="to-unpack-your-device"></a>Teie seade lahti.

1. Kontrollige väljal ja pakendit vaht purustav, kärpimine, vee kahju või selge kahju. Kui ruut või pakendil on oluliselt rikutud, avage dialoogiboks. Palun [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) , et saaksite otsustada, kas seade on töökorras.

2. Avage väliseks väljal ja seejärel võtma vastav esmane ja EBOD Kirjalisandid kaks ruudud. Esmane ja EBOD Kirjalisandid saate nüüd lahti. Järgmisel joonisel on ühe selle Kirjalisandid lahti vaadet.

    ![Lahti seadme salvestusruum](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)

    **Seadme salvestusruumi lahti vaade**

     Sildi | Kirjeldus
     ----- | -------------
     1     | Välja
     2     | SAS kaabel (salves tarvikud ja kaabel)
     3     | Alumine vaht
     4     | Seadme
     5     | Parimate
     6     | Keeletarvikute ruut

3. Pärast lahtipakkimine kaks ruudud, veenduge, et teil on:

  - 1 esmane ruum (esmane ruumi ja EBOD ruum on kaks eraldi väljadel)
  - 1 EBOD ruumi
  - 4 toitekaabliga, 2 iga väljale
  - 2 SAS kaabel (esmane ruumi ühenduse EBOD ruum)
  - 1 crossover Ethernet kaabel
  - 2 järjekorranumber konsooli kaabel
  - 1 USB-järjestikust muunduri järjestikune juurdepääsu
  - 4 QSFP – kui soovite-adapterit SFP + 10 New York võrgu liidesed kasutamiseks
  - 2 koguda mount komplektid (4 külgpiiretega paigaldus riistvara, 2 esmane ruumi ja EBOD ruumi), iga väljal 1
  - Töö alustamine dokumentatsioon

    Kui te ei saanud ühte üksusi eelnimetatud [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md).  

Järgmiseks on sektsioon-mount seadme.

## <a name="rack-mount-your-storsimple-8600-device"></a>Sektsioon-mount seadme StorSimple 8600

Järgige järgmisi juhiseid installimiseks oma StorSimple 8600 mäluseadmesse standard 19-tolli sektsioon ees-ja tagumine postitusi. Kasutatav seade on kaks Kirjalisandid: esmane ruumi ja EBOD ruumi. Mõlemad peaksid olema sektsioon-paigaldatud.

Installimise koosneb mitmest juhiseid, mis on kirjeldatud järgmistest toimingutest.

> [AZURE.IMPORTANT]
StorSimple seadmete peab olema sektsioon paigaldatud tööd.



### <a name="site-preparation"></a>Saidi ettevalmistamine

Selle Kirjalisandid peab olema installitud standard 19-tolli sektsioon, mis sisaldab esi- ja tagumine postitusi. Järgmiste toimingute abil sektsioon installi ettevalmistamine.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Saidi ettevalmistamine sektsioon installimine

1. Veenduge, et esmane ja EBOD Kirjalisandid on puhkuse turvaline tasapinnalise, ühed ja taseme töö pind klõpsake (või sarnane).

2. Veenduge, et sait, kuhu kavatsete häälestamine on standardne AC power sõltumatu või sektsioon Power jaotuse ühiku (Probleemse) koos Puhvertoiteallikas (UPS).

3. Veenduge, et ühe 4U (2 X 2U) pesa on saadaval sektsioon, milles soovite ühendada funktsiooni Kirjalisandid.

![Ikoon hoiatus](./media/storsimple-safety/IC740879.png)![raske ikooni](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **hoiatus!**

 Veenduge, et teil on kaks inimest haldamiseks kaalust, kui teil on käsitsemine häälestamine seadmes saadaval. Täielikult konfigureeritud ruum võib kaaluda kuni 32 kg (70 kg.).

### <a name="rack-prerequisites"></a>Sektsioon eeltingimused

Funktsiooni Kirjalisandid on mõeldud installi standard 19-tolli sektsioon kabineti abil:

- Minimaalne sügavus 27.84 tolli sektsioon post postitamiseks kaudu
- Suurim lubatud kaal 32 kg seadme
- Suurim lubatud tagasi rõhk 5 Pascal (0,5 mm veesammast)

### <a name="rack-mounting-rail-kit"></a>Sektsioon – kinnitus raudtee komplekt

Kogumi paigaldus rööpad antakse 19-tolli sektsioon kabineti jaoks. Rööpad on testitud käsitlema maksimaalne ruum paksus. Need rööpad võimaldab ka installi mitme Kirjalisandid ruumis raami kaotamata. Installige kõigepealt EBOD ruumi.

#### <a name="to-install-the-ebod-enclosure-on-the-rails"></a>Installida EBOD ruumi rööpad

2. Seda juhist täitke ainult juhul, kui teie seadmes pole installitud sisemine rööpad. Tavaliselt on sisemine rööpad paigaldatud soovitud factory. Kui rööpad pole installitud, siis installige raudtee vasakule ja paremale-rail slaidide ruumi raami poolele. Nad lisada, kasutades kuus argumendil kruvid mõlemal küljel. Suund hõlbustamiseks raudtee slaidid on märgitud **LH – esi** - ja **RH – ette**ja lõppu, mis on kinnitatud ruumi tahapoole on keritud end.

    ![Manustamise raudtee slaidide ruumi raami](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Manustamise raudtee slaidide poolele ruumi**

    Sildi | Kirjeldus
    ----- | -----------
    1     | M 3 x 4 nupp-head kruvid
    2     | Raami slaidid

3. Kinnitage raudtee vasakus ja paremas raudtee assemblereid sektsioon kabineti vertikaalne liikmed. Sulud on tähistatud **LH**, **RH**ja **selle serva, kuni** juhatavad teid õige suund.

4. Otsige üles raudtee sõrmed esi -raudtee komplekti juures. Laiendamine raudtee mahutamiseks vahele sektsioon postitusi ja asetage esi- ja tagumine-sektsioon postituse vertikaalne liikme auke. Veenduge, et raudtee komplekti on.

5. Turvaline raudtee komplekti seadme ja vertikaalne liikmete kasutades kahte esitatud argumendil kruvid. Kasutage ühte kruvi ees ja teine taga.

6. Korrake neid samme teiste raudtee komplekti.

     ![Manustamise raudtee slaidide kabineti sektsioon](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Manustamise raudtee assemblereid raam**

     Sildi | Kirjeldus
     ----- | -----------
     1     | Soovitud
     2     | Ruutu-Ava ees sektsioon postituse kruvi
     3     | Vasak esi raudtee asukoht sõrmed
     4     | Soovitud
     5     | Vasakul tagumise raudtee asukoht sõrmed

### <a name="mounting-the-ebod-enclosure-in-the-rack"></a>Paigaldus EBOD ümbrist raami

Kasuta sektsioon rööpad lihtsalt installitud, tehke järgmist EBOD ümbrist raami ühendada.

#### <a name="to-mount-the-ebod-enclosure"></a>Ühendada EBOD ruumi

1. Abiline, tõstke ruumi ja joondamiseks sektsioon rööpad.

2. Sisestage hoolikalt rööpad ruumi ja seejärel lükake täielikult raam kabineti.

    ![Seadme raami lisamine](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Raami ümbrist paigaldus**

3. Vasak ja parem esi ääriku suurtähelukk Tõmbamine tasuta suurtähelukk eemaldada. Suurtähelukk ääriku snap lihtsalt peale Mõõtesilindri.

4. Turvaline ruumi, installides ühe esitatud Phillips-kruvi läbi iga äärik, vasak ja parem raami sisse.

4. Installige ääriku suurtähelukk vajutamise kohale ja haaramine neid oma kohale.

     ![Installimise ääriku suurtähelukk](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)

    **Suurtähelukk ääriku installimine**

     Sildi | Kirjeldus
     ----- | -----------
     1     | Ruumi kinnituse kruvi


### <a name="mounting-the-primary-enclosure-in-the-rack"></a>Esmane ümbrist raam paigaldus

Kui olete lõpetanud, paigaldus EBOD ruumi, peate ühendada esmane ruumi, täites samad juhised.

> [AZURE.NOTE]
>
> - On võimalik, et mõned tühje lahtreid vahel esmane ruumi ja EBOD ruumi raami sisse.
> - Esitatud 2m SAS kaabli abil saate esmane ruumi ühenduse EBOD ruumi.
> - Ei pea üksuse EBOD üksuse suhteline paigutuse piirangud. Seetõttu esmane ruumi saate paigutada ülemisel pesa ja EBOD ruumi allpool – või vastupidi.

Järgmiseks on kaabel seadme power, võrgu ja järjestikune juurdepääsu.

## <a name="cable-your-storsimple-8600-device"></a>Seadme StorSimple 8600 kaabel

Järgnevalt selgitatakse kaabel seadme StorSimple 8600 power, võrgu ja järjestikune ühendused.

### <a name="prerequisites"></a>Eeltingimused

Enne alustamist kaabel seadme, peate:

- Teie peamine ruumi ja EBOD ruumi, täielikult lahti
- 4 power kaabel (2 iga esmast ja EBOD ruumi) oma seadmega kaasas
- 2 SAS kaabel EBOD ruumi ühenduse esmane ruumi seadmega kaasas
- Access 2 Power jaotuse kestusühikud (PDUs) (soovitatav)
- Võrgukaablid
- Esitatud järjestikune kaabel
- USB-järjestikust muunduri sobiv juht teie arvutisse installitud (vajadusel)
- Esitatud 4 QSFP-et-adapterit SFP + 10 New York võrgu liidesed kasutamiseks
- [Toetatud riistvara 10 New York võrgu liidesed StorSimple seadmes](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>SAS ja Power kaabeldus

Teie seade on nii esmane ruumi ja EBOD ruumi. Selleks on vaja olema järjestikune manustatud SCSI (SAS) Ühenduvus ja power koos komplekslõng üksused.

Häälestamisel selle seadme esimest korda, toimingute jaoks SAS kaabeldus esmalt ja seejärel täitke juhised power kaabeldus.

[AZURE.INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Võrgu kaabeldus

Teie seade on aktiivne-puhkerežiimis olev konfigureerimisel: korraga, üks kontrolleril moodul on aktiivne ja töötlemise ajal muid kontrolleril mooduli ketas ja võrgu kõik toimingud on puhkerežiimis. Selle domeenikontrolleri tõrke ilmnemisel valige kontrolleril kohe aktiveerib ja jätkub kõik kettale ja võrgunduse toimingud.

Selle domeenikontrolleri liigsete Tõrkesiirde toetamiseks peate seadme võrgu kaabel, nagu on näidatud toimige järgmiselt.

#### <a name="to-cable-for-network-connection"></a>Kui soovite võrguühenduse kaabel

1. Teie seade on kuus võrgu liidesed iga kontrolleril: neli 1 Gbit ja kaks 10 Gbit Ethernet pordid. Vaadake järgmisel joonisel andmete pordid backplane teie seadme kohta.

     ![Backplane 8600 seadme](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Tagasi, seadme nähtaval andmete pordid**

     Sildi   | Kirjeldus
     ------- | -----------
     0,1,4,5 |  1 New York võrgu liidesed
     2,3     | 10 New York võrgu liidesed
     6       | Järjestikune pordid



1. Võrgu kaabeldus jaoks järgmisel joonisel näha. (Miinimum võrgukonfiguratsioon kuvatakse sinine pidevjooned. Kõrge-saadavus ja jõudluse, täiendav konfigureerimine vajalik kuvatakse punktiirjooni.)

![Kaabel 4U seadme, võrgu jaoks](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Võrgu kaabeldus teie seadme jaoks**

Sildi | Kirjeldus
----- | -----------
A    | Interneti-ühendusega LAN
B    | Selle domeenikontrolleri 0
C    | PCM 0
D    | Selle domeenikontrolleri 1
E    | PCM 1
F    | Selle domeenikontrolleri EBOD 0
G    | Selle domeenikontrolleri EBOD 1
H, I  | Hosts (nt faili serverid)
0-5  | Võrgu liidesed
6    | Esmane ruumi
7    | EBOD ruumi

Kui seade kaabeldus, minimaalne konfiguratsioon vaja:


- Vähemalt kaks võrgu liidesed ühendatud iga kontrolleril ühe pilvepõhise juurdepääsu ja üks iSCSI. ANDMETE 0 portide automaatselt lubatud ja konfigureeritud järjestikune konsooli seadme kaudu. Lisaks andmete 0, teise andmete pordi peab olema konfigureeritud Azure klassikaline portaali kaudu. Sel juhul andmete 0 ühendamine port esmane LAN (Interneti-ühendusega võrk). Muud andmed pordid saab ühendada SAN/iSCSI Kohtvõrgu (VLAN) lõigu võrgu, sõltuvalt ettenähtud roll.

- Liidesed iga kontrolleril kättesaadavuse tagamiseks kontrolleril Tõrkesiirde juhul samasse võrku ühendatud. Näiteks kui valite mõne hulka kuuluvad soovitud andmete 0 ja andmete 3 ühenduse, peate vastavaid andmeid 0 ja andmete 3 ühendada muude kontrolleril.

Pidage meeles, kõrge-saadavus ja jõudluse:


- Võimaluse korral konfigureerida kontrolleril iga paari võrgu liidese pilvepõhise juurdepääsu (1 New York) ja teise paar iSCSI (10 New York soovitatav).

- Võimaluse korral ühendage võrgu liidesed iga domeenikontrolleri kaks erinevad lülitid vastu vahetamise tõrge kättesaadavuse tagamiseks. Joonis kujutab kahe 10 New York võrgu liidesed, andmete 2 ja 3 andmeid: iga domeenikontrolleri ühendada kaks erinevat parameetrid. Lisateabe saamiseks vaadake **võrgu liidesed** [kõrge-saadavus nõuded StorSimple seadme](storsimple-system-requirements.md#high-availability-requirements-for-storsimple)all.

>[AZURE.NOTE] Kui SFP + transiiverite koos oma 10 New York võrgu liidesed, kasutage pakutavaid QSFP-adapterit SFP +. Lisateabe saamiseks minge [toetatud riistvara 10 New York võrgu liidesed StorSimple seadmes](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

### <a name="serial-port-cabling"></a>Järjestikune port kaabeldus

Järgmiste toimingute abil kaabel järjestikune pordile.

#### <a name="to-cable-for-serial-connection"></a>Järjestikune ühenduse kaabel

1. Teie seade on järjestikune port iga kontrolleril, mis on tähistatud mutrivõti ikooni. Järjestikune pordid leidmiseks vaadake illustratsioon, milles andmed kuvatakse pordid seadme tagasi.

2. Leidke aktiivse domeenikontrolleri sisse oma seadme backplane. Vilkuv sinine LED näitab on aktiivne.

3. Kasutage pakutavaid seeria kaabel (vajaduse korral sülearvuti USB-seeria muundur) ja ühendage oma konsooli või arvutis (koos terminal imiteerimist seadmele) aktiivne kontrolleril järjestikune porti.

4. Järjestikust-USB-draiverid (saadetud seadmega) oma arvutisse installida.

5. Häälestada järjestikune ühenduse järgmiselt:
   - 115200 baud
   - 8 andmed bittide
   - 1 Peata bitine
   - Pole võrdse
   - Andmevoo juhtimine **puudub**

6. Ühendus, vajutades klahvikombinatsiooni Enter konsooli töötamise kontrollimiseks. Järjestikune konsooli menüü peaks kuvatama.

> [AZURE.NOTE] **Lights-Out haldus:** Kui seade on installitud remote andmekeskuse või arvuti toas piiratud juurdepääsuga, veenduge, et nii kontrollerid järjestikune ühendused on alati ühendatud järjestikune konsooli aktiveerimine või sarnased seadmed. See võimaldab riba-, kaugjuhtimise ja tugiteenuste toimingute puhul võrgu häirete või ootamatuid tõrkeid.

Teie seadme power, võrgule juurdepääsu ja järjestikune ühenduse kaabeldus lõpule jõudnud. Järgmise sammuna oma seadmesse tarkvara konfigureerimiseks.

## <a name="next-steps"></a>Järgmised sammud

Nüüd olete valmis [juurutada](storsimple-deployment-walkthrough-u2.md)ja konfigureerida kohapealse StorSimple seadme.
