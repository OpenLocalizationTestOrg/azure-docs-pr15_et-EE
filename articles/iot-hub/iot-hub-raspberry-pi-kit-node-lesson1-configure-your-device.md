<properties
 pageTitle="Seadme konfigureerida | Microsoft Azure'i"
 description="Konfigureerimine oma Vaarika Pi 3 esmakordsel kasutamisel alla ja installige Raspbian OS, tasuta operatsioonisüsteem, mis on optimeeritud Vaarika Pi riistvara."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="11-configure-your-device"></a>1.1 seadme konfigureerida

## <a name="111-what-you-will-do"></a>1.1.1, mida te teete

Konfigureerimine oma Pi esmakordsel kasutamisel alla ja installige tasuta operatsioonisüsteem, mis on optimeeritud Vaarika Pi riistvara Raspbian operatsioonisüsteem. Kui vastate probleeme, otsida lahendusi [lehe tõrkeotsing](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="112-what-you-will-learn"></a>1.1.2, mida saate

Selles jaotises saate:

- Kuidas installida Raspbian oma Pi
- Kuidas power üles oma Pi USB-kaabli abil
- Kuidas luua ühendus oma Pi Ethernet kaabel või WiFi-võrgu
- Kuidas lisada LED soovitud breadboard ja ühendage see oma Pi

## <a name="113-what-you-need"></a>1.1.3 vajalik

Selles jaotises lõpuleviimiseks peate järgmised osad Vaarika Pi 3 Starter Kit.

- Tahvli Vaarika Pi 3
- 16GB MicroSD kaardi
- 5 v 2A power pakkuda kuus suu mikro USB-kaabel
- Funktsiooni breadboard
- Konnektor juhtmed
- 560 Ohm takisti
- Hajutatud 10mm LED
- Ethernet kaabel

![Asjad, mida teie komplekt sisaldab](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Samuti vajate:

- Ühenduse loomiseks oma Pi kaabel- või ühenduse
- USB-SD adapterit või mini-SD kaardi kirjutamiseks OS pilt MicroSD kaart.
- Arvutis, kus töötab Windowsi, Maci või Linuxi. Arvuti saab installida Raspbian MicroSD kaart.
- Interneti-ühendust vajalikud tööriistad ja tarkvara allalaadimine

## <a name="114-install-raspbian-on-the-microsd-card"></a>1.1.4 Raspbian installimine microSD-kaart

MicroSD kaardi kirjutamiseks Raspbian pildi ettevalmistamine.

1. Laadige alla Raspbian.
  1. [Laadige alla](https://www.raspberrypi.org/downloads/raspbian/) zip-faili jaoks Raspbian Jessie koos ühe piksli.
  2. Ekstrakti Raspbian pilt oma arvutis asuva kaustaga.
2. Installige Raspbian MicroSD kaart.
  1. [Laadige alla](https://www.etcher.io) ja installige graafik SD kaardi kirjutaja kasuliku.
  2. Käivita graafik ja valige Raspbian pilt ekstraktitud juhises 1.
  3. Valige MicroSD kaardi ketas.
    Märkus: Graafik võivad olla juba valitud õige ketas.
  4. Klõpsake Flash installimiseks Raspbian microSD-kaart.
  5. MicroSD kaardi eemaldada arvutist, kui see on lõpule viidud.
    Märkus: Kui see on ohutu eemaldage microSD otse, kuna graafik automaatselt väljutab või unmounts MicroSD kaardi lõpetamisel.
  6. Sisestage oma Pi MicroSD kaart.

![Sisestage SD-kaart](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="115-power-on-your-pi"></a>1.1.5 klõpsake oma Pi power

Power oma piiga mikro USB-kaabli ja power pakkumise abil.

![Lülitage](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [AZURE.NOTE] See on oluline kasutada power pakkumise komplekt, mis on vähemalt 2A – veenduge, et teie Vaarika juhitakse piisavalt Power õigesti töötada.

## <a name="116-connect-your-raspberry-pi-3-to-the-network"></a>1.1.6 võrguga ühenduse loomiseks oma Vaarika Pi 3

Saate luua ühenduse oma Pi juhtmega ühendatud võrku või traadita võrku. Veenduge, et teie pii on ühendatud arvuti samasse võrku. Näiteks saate luua ühenduse oma Pi sama parameetrit, mis teie arvuti on ühendatud.

### <a name="1161-connect-to-a-wired-network"></a>1.1.6.1 ühenduse juhtmega ühendatud võrku

Ethernet kaabli abil saate oma Pi juhtmega ühendatud võrku ühendada. Kaks LED oma piiga sisse lülitada, kui ühendus on loodud.

![Ühenduse loomiseks kasutada Ethernet kaabel](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="1162-connect-to-a-wireless-network"></a>1.1.6.2 ühenduse traadita võrku

Järgige [juhiseid](https://www.raspberrypi.org/learning/software-guide/wifi/) Vaarika Pi Foundationi oma Pi ühenduse loomiseks oma traadita võrku. Need juhised jaoks on vaja esmalt ühenduse kuvarit ja klaviatuuri oma pii.

## <a name="117-connect-the-led-to-your-pi"></a>1.1.7 LED ühenduse oma Pi

Selle toimingu tegemiseks kasutada [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), konnektori juhtmed, LED ja takisti. Saate ühendada need oma pii (GPIO) [üldotstarbeline sisend/väljund](https://www.raspberrypi.org/documentation/usage/gpio/) pordid. 

![Breadboard, LED ja takisti](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Lühemaks lisamaterjalidele LED ühenduse **GPIO GND (PIN-koodi 6)**.
2. Rohkem lisamaterjalidele LED ühenduse ühe lisamaterjalidele takisti.
3. Ülejäänud etapil takisti ühenduse **GPIO 4 (PIN-koodi 7)**.

Pange tähele, et LED polaarsus on oluline. Selle sätte polaarsus tuntakse aktiivne madal.

![Süsteem](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Palju õnne! Nüüd olete konfigureerinud oma pii.

## <a name="118-summary"></a>1.1.8 Kokkuvõte

Selles jaotises olete õppinud, kuidas konfigureerida oma Pi installimist Raspbian, pii oma võrguga ja LED ühenduse oma pii. Pange tähele, et LED ei veel lihtsustatud üles. Järgmise jaotise installimist ettevalmistamisel valimi rakendus töötab teie Pi tarkvara ja vajalikud tööriistad.

![Riistvara on valmis](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Järgmised sammud

[1.2 saada tööriistad](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
