<properties 
    pageTitle="Kuidas luua Web App rakenduse teenusega Linux | Microsoft Azure'i" 
    description="Web Appi loomine töövoo rakenduse teenuse Linuxi jaoks." 
    keywords="Azure'i rakendust service, veebirakenduse, linux oss"
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

# <a name="create-a-web-app-with-app-service-on-linux"></a>Luua veebirakenduse rakenduse teenusega Linux

## <a name="using-the-management-portal-to-create-your-web-app"></a>Haldusportaali abil oma veebirakenduse loomine
Saate alustada oma veebirakenduse loomine Linux [haldusportaali](https://portal.azure.com) nagu on näidatud järgmisel pildil.

![][1]

Kui valite suvandi allpool, mida kuvatakse loomine tera nagu on näidatud järgmisel pildil. 

![][2]

-   Pange oma veebirakenduse nimi.
-   Valige ressursi olemasolevasse rühma või looge uus. (Vt piirkondades saadaval [piirangud jaotis](./app-service-linux-intro.md)).
-   Valige olemasolev rakenduse teenusleping või looge uus ühte (vt rakenduse teenuse leping märkmed [piirangud jaotis](./app-service-linux-intro.md)). 
-   Valige taotluse virnas kavatsete kasutada. Saate valida mitut versiooni Node.js ja PHP. 

Kui teil on rakendus, mis on loodud, saate muuta taotluse virnas rakenduse sätteid nagu on näidatud järgmisel pildil.

![][3]

## <a name="deploying-your-web-app"></a>Oma veebirakenduse juurutamine

Valides "Juurutussuvandid" haldamise portaalis annab teile võimalus kasutada kohalik Git hoidla või GitHub hoidla juurutada rakendust. Juhised seejärel on samuti-Linux web appi ja meie [kohaliku Git juurutamise](./app-service-deploy-local-git.md) või meie [pideva](./app-service-continuous-deployment.md) artikli juhised GitHub jälgite.

Samuti saate üles laadida rakenduse saidile FTP. Saate FTP lõpp-punkti veebirakenduse jaoks diagnostika logid jaotise, nagu on näidatud järgmisel pildil.

![][4]


## <a name="next-steps"></a>Järgmised sammud ##

* [Mis on rakenduse teenus Linux?](./app-service-linux-intro.md)
* [Kasutamise Node.js veebirakendustes Linux PM2 konfigureerimine](./app-service-linux-using-nodejs-pm2.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
