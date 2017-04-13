<properties
 pageTitle="Tööriistad (Windows 7 +) | Microsoft Azure'i"
 description="Laadige alla ja installige vajalikud tööriistad ja tarkvara esimese valimi rakendamiseks oma Pi Windows 7 ja uuemad versioonid."
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

# <a name="12-get-the-tools-windows-7-"></a>1.2 saada menüü Tööriistad (Windows 7 +) 

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1, mida te teete

Arengu tööriistad ja tarkvara esimese valimi rakendamiseks oma Vaarika Pi 3 alla laadida. Kui vastate probleeme, otsida lahendusi [lehe tõrkeotsing](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="122-what-you-will-learn"></a>1.2.2, mida saate
- Kuidas installida Git ja Node.js
  - [Git](https://git-scm.com) on jaotatud avatud allika versioon juhtelemendi süsteem. Selle õppetüki valimi taotlus on talletatud Git.
  - [Node.js](https://nodejs.org/en/) on JavaScripti käitusaja koos rikkaliku paketi ökosüsteemi.
- Kuidas kasutada NPM installida täiendavaid Node.js arengu tööriistu.
  - Node.js miinimumversioon nõue on 4,5 LTS.
  - [NPM](https://www.npmjs.com) on üks pakett Node.js haldurid.

## <a name="123-what-you-need"></a>1.2.3 vajalik

- Interneti-ühendust arengu tööriistad ja tarkvara allalaadimine
- Arvutis, kus töötab Windows

## <a name="124-install-git-and-nodejs"></a>1.2.4 Git ja Node.js installimine

Klõpsake alla laadima ja installima Git ja Node.js LTS Windowsi linkidest.

- [Windowsi Git hankimine](https://git-scm.com/download/win/)
- [Windowsi jaoks Node.js LTS hankimine](https://nodejs.org/en/)

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 installida täiendavad Node.js arengu tööriistad

[Gulp.js](http://gulpjs.com) abil automatiseerida juurutamise valimi taotluse oma pii. Saate ka [seadme-discovery-cli](https://github.com/Azure/device-discovery-cli) teabe toomiseks kasutada võrgu kohta asjade seadmetes.

Käivita administraatorina käsuviiba. Installige `gulp` ja `device-discovery-cli` , käivitage järgmine käsk:

```bash
npm install -g device-discovery-cli gulp
```
    
Kui teil tekib probleeme Node.js ja nende täiendavate Node.js arengu tööriistade installimine oma arvutisse, lugege teemat [tõrkeotsingujuhendi](iot-hub-raspberry-pi-kit-node-troubleshooting.md) levinud probleemidele lahendusi.

## <a name="126-install-visual-studio-code"></a>1.2.6 installida Visual Studio kood

[Laadige alla](https://code.visualstudio.com/docs/setup/windows) ja installige Visual Studio kood. Visual Studio kood on kerge kuid võimas lähtekoodi editor Windowsi, Linuxi ja macOS. Kasutate selle redaktori hiljem õpetuse redigeerimiseks proovi kood.

## <a name="127-summary"></a>1.2.7 Kokkuvõte

Olete installinud nõutav arengu tööriistad ja tarkvara valimi nõue. Järgmise jaotise saate luua, juurutada ja käivitada valimi rakenduse oma pii.

## <a name="next-steps"></a>Järgmised sammud

[1.3 loomine ja vilkuv valimi rakenduse juurutamine](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)
