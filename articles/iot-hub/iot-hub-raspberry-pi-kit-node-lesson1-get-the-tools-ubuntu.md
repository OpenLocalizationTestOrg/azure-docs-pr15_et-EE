<properties
 pageTitle="Tööriistad (Ubuntu 16.04) | Microsoft Azure'i"
 description="Laadige alla ja installige vajalikud tööriistad ja tarkvara esimese valimi rakendamiseks oma Pi Ubuntu."
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

# <a name="12-get-the-tools-ubuntu-1604"></a>1.2 saada tööriistad (Ubuntu 16.04)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1, mida te teete

Arengu tööriistad ja tarkvara esimese valimi rakendamiseks oma Vaarika Pi 3 alla laadida. Kui vastate probleeme, otsida lahendusi [lehe tõrkeotsing](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="122-what-you-will-learn"></a>1.2.2, mida saate

Selles jaotises saate:

- Kuidas installida Git ja Node.js
  - [Git](https://git-scm.com) on jaotatud avatud allika versioon juhtelemendi süsteem. Selle õppetüki valimi taotlus on talletatud Git.
  - [Node.js](https://nodejs.org/en/) on JavaScripti käitusaja koos rikkaliku paketi ökosüsteemi.
- Kuidas kasutada NPM installida täiendavaid Node.js arengu tööriistu.
  - Node.js minimaalne vajalik versioon on 4,5 LTS.
  - [NPM](https://www.npmjs.com) on üks pakett haldurid Node.js

## <a name="123-what-do-you-need"></a>1.2.3 mida on vaja

- Interneti-ühendust arengu tööriistad ja tarkvara allalaadimine
- Arvutis, kus töötab Ubuntu 16.04 või uuem versioon 

## <a name="124-install-git-nodejs-and-npm"></a>1.2.4 Git, Node.js ja NPM installimine

Kasutage kiirklahvikombinatsiooni `Ctrl + Alt + T` avage Terminal aknas ja käivitage järgmine käsk:

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 installida täiendavad Node.js arengu tööriistad

[Gulp.js](http://gulpjs.com) abil automatiseerida juurutamise valimi taotluse oma pii. Saate ka [seadme-discovery-cli](https://github.com/Azure/device-discovery-cli) teabe toomiseks kasutada võrgu kohta asjade seadmetes.

Installige `gulp` ja `device-discovery-cli` Terminal aknas järgmise käsu abil:

```bash
sudo npm install -g device-discovery-cli gulp
```

Kui teil tekib probleeme installimist Node.js ja nende tööriistade veel Ubuntu, lugege teemat [tõrkeotsingujuhendi](iot-hub-raspberry-pi-kit-node-troubleshooting.md) levinud probleemidele lahendusi.

## <a name="126-install-visual-studio-code"></a>1.2.6 installida Visual Studio kood

[Laadige alla](https://code.visualstudio.com/docs/setup/linux) ja installige Visual Studio kood. Visual Studio kood on kerge kuid võimas lähtekoodi editor Windowsi, Linuxi ja macOS. Kasutate selle redaktori hiljem õpetuse redigeerimiseks proovi kood.

## <a name="127-summary"></a>1.2.7 Kokkuvõte

Olete installinud nõutav arengu tööriistad ja tarkvara valimi nõue. Järgmise jaotise saate luua, juurutada ja käivitada valimi rakenduse oma pii.

## <a name="next-steps"></a>Järgmised sammud

[1.3 loomine ja vilkuv valimi rakenduse juurutamine](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)