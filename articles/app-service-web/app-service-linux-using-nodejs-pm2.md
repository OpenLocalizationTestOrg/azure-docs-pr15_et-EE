<properties 
    pageTitle="PM2 konfiguratsiooni kasutamise NodeJS veebirakendustes Linux | Microsoft Azure'i" 
    description="Kasutamise NodeJS veebirakendustes Linux PM2 konfigureerimine" 
    keywords="Azure'i rakendust service, veebirakenduse, nodejs, pm2, linux, oss"
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

# <a name="using-pm2-configuration-for-nodejs-in-web-apps-on-linux"></a>Kasutamise Node.js veebirakendustes Linux PM2 konfigureerimine

Kui seate taotluse virnas Node.js veebirakendustes Linux, saate suvandi Node.js käivitus faili, nagu on näidatud järgmisel pildil.

![][1]

Kasutage seda kas

-   Määrake oma Node.js rakenduse käivitamisel script (nt: /bin/server.js)
-   Määrake oma Node.js rakenduse kasutamiseks PM2 konfiguratsioonifail (nt: /foo/process.json)

 >[AZURE.NOTE] Kui soovite oma sõlm protsessi automaatselt uuesti, kui on muudetud teatud faile, peate kasutada PM2 konfigureerimine. Teisiti rakenduse kuvatakse uuesti, kui ta saab muudatuste teatisi, näiteks pideva rakenduse koodis muutumisel.

Te saate kontrollida Node.js [protsessi faili dokumentatsiooni](http://pm2.keymetrics.io/docs/usage/application-declaration/) kõigi suvandite, kuid all on näide sellest, mida kasutaksite process.json-failina

        {
          "name"        : "worker",
          "script"      : "/bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["/bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

Olulised asjad, mida selle konfiguratsiooni on 

-   Atribuudi "skript" määrab teie taotlus start skript.
-   Atribuudi "eksemplari" määrab, mitu eksemplari sõlm protsessi käivitamiseks. Kui teil on rakenduse suuremad VM suurused, mis on mitme protsessorituuma, mida soovite oma ressursse maksimeerimine, seades suurem väärtus siin.
-   "Jälgimine" massiiv saate määrata kõik failid, mille muutmine soovite taaskäivitage oma sõlm protsessid.
-   "Watch_options", peate praegu määrata "usePolling" true sellepärast, et teie rakenduse sisu on ühendatud.


## <a name="next-steps"></a>Järgmised sammud ##

* [Mis on rakenduse teenus Linux?](./app-service-linux-intro.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png