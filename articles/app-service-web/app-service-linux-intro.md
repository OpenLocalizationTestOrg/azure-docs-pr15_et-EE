<properties 
    pageTitle="Sissejuhatus rakenduse teenuse Linux | Microsoft Azure'i" 
    description="Lisateavet rakenduse teenuse Linux." 
    keywords="Azure'i rakendust service, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="introduction-to-app-service-on-linux"></a>Sissejuhatus rakenduse teenuse Linux
Rakenduse teenuse Linux on praegu avaliku eelvaate ja toetab töötava veebirakenduste algupäraselt Linux. 

## <a name="overview"></a>Ülevaade ##
Klientide teenuse abil saate rakenduse Linux host veebirakenduste algupäraselt Linuxi jaoks toetatud rakenduse virnadena. Järgmised funktsioonid jaotises on loetletud praegu toetatud rakenduse virnadena.

## <a name="features"></a>Funktsioonid ##
Rakenduse teenuse Linux toetab praegu järgmist rakenduse virnadena

- Node.js
- PHP

Kliente saate juurutada nende rakenduste abil

- FTP.
- Kohaliku Git.
- GitHub või BitBucket.

Rakenduse skaleerimist jaoks


- Klientide saate oma veebirakenduse skaala üles ja alla, muutes taseme sisse oma rakenduse teenuse kavandamine. 
- Klientide saate mastaapimiseks välja nende rakenduste välja ja käivitage rakendust üle mitme eksemplari piirides oma SKU-ga.

Kudu osa funktsionaalsust ei tööta

- Keskkonnas.
- Juurutuste.
- Tavaline konsooli.

## <a name="limitations"></a>Piirangud ##

Azure'i haldusportaal ainult praegu toetatud funktsioonid rakenduse teenuse Linux kuvamine ja peitmine ülejäänud. Meie meeskond funktsioonide lubamine me ei säilita peegeldab seda portaalis haldus. Mõned funktsioonid nagu VNET integratsioon ja AAD / kolmanda osapoole autentimine või Kudu saidi laiendid praegu ei tööta. Aga kui need töötavad saame uuendame meie dokumentatsiooni ja ajaveebi muudatuste kohta.

See avaliku eelvaade on praegu saadaval ainult järgmistes piirkondades

-   Lääne US.
-   Lääne Euroopa.
-   Kagu-Aasia.

Veebirakenduse Linux toetab ainult spetsiaalne rakenduse teenuse lepingud ja on tasuta või ühiskasutuses taseme. Samuti rakenduse teenuse harilik ja Linux veebirakenduste lepingud üksteist välistavad nii, et ei saa luua Linux web appi-Linux rakenduse teenusleping.

Ressursirühm, mis ei sisalda-Linux veebirakenduste piirkonna tuleb luua veebirakenduse Linux.

Kattuv ringlussevõtu web Appsi puuduvad kliendid tuleb arvestada small tööseisakute web appi korral sai taaskäivitada. 

## <a name="next-steps"></a>Järgmised sammud ##

Järgige järgmisi linke alustada rakenduse teenuse Linux. Saatke [meie](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview)Foorum küsimused ja probleemid.

* [Rakenduse teenuse Linux veebirakenduste loomine](./app-service-linux-how-to-create-a-web-app.md)
* [Kasutamise Node.js veebirakendustes Linux PM2 konfigureerimine](./app-service-linux-using-nodejs-pm2.md)

