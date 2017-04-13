<properties
    pageTitle="Testi valmistamisel Web Appsi kasutamise alustamine"
    description="Lisateavet tootmise (Vihje) funktsiooni Azure'i rakenduse teenuse veebirakendustes Test."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-test-in-production-for-web-apps"></a>Testi valmistamisel Web Appsi kasutamise alustamine

Testimine valmistamisel või live testimine oma veebirakenduse abil reaalajas kliendi liiklus, on test strateegia, mille rakenduste arendajatele järjest integreerida nende [dünaamilised arengu](https://en.wikipedia.org/wiki/Agile_software_development) meetod. See võimaldab teil tootmiskeskkonnas vastandina testimiskeskkonnas sünteesitud andmete reaalajas kasutaja liiklus rakenduste kontrollimine. Asetada kätte uus rakendus otse kasutajatele, saate teavitada tegelike probleemide, rakenduse võib tekkida siis, kui see on juurutatud. Saate kontrollida funktsioone, jõudlus ja oma rakenduse värskenduste helitugevust, kiiruse ja mitmel real kasutaja liiklus, mis te saate kunagi testimiskeskkonnas ligikaudne väärtus.

## <a name="traffic-routing-in-app-service-web-apps"></a>Liikluse marsruutimist rakenduse teenuse veebirakendustes

Liikluse marsruutimist funktsiooni [Azure'i rakendust Service](http://go.microsoft.com/fwlink/?LinkId=529714), saate määrata ühe või mitme [juurutamine](web-sites-staged-publishing.md)reaalajas kasutaja-liikluse osa ja seejärel analüüsida rakenduse [Azure'i rakenduse ülevaated](/services/application-insights/) [Windows Azure Hdinsightiga](/services/hdinsight/)või kolmanda osapoole tööriista nagu [Uus reliikvia](/marketplace/partners/newrelic/newrelic/) muudatuse kinnitamiseks. Näiteks saate rakendada rakenduse teenusega järgmistel juhtudel:

- Tutvumine otstarbekas vead või Pinpointi täitmise kitsaskohtadest värskenduste enne kogu saidi juurutamine
- Teha "kontrollitud testi lendude" tehtud muudatuste mõõta usibility mõõdikute beeta rakenduse kohta
- Järk-järgult kaldtee kuni uus värskendus ja nõtkelt uuesti praegune versioon kui ilmneb tõrge 
- Optimeerida oma rakenduse business tulemuste käivitades [A / B katsed](https://en.wikipedia.org/wiki/A/B_testing) või [mitme muutujaga kontrollib](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) mitme juurutamise Slots

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>Nõuded liikluse marsruutimise abil veebirakendustes

- Oma veebirakenduse käivitama **Standard** - või **Premium** astme, kui see on vajalik mitme juurutamise Slots.
- Õigesti töötamiseks liikluse marsruutimist nõuab küpsised tuleb lubada kasutajate brauseris. Liikluse marsruutimist küpsiseid, et kinnitada Kliendi brauser juurutamise pesa elu kliendi seansi.
- Liikluse marsruutimist toetab täpsemalt näpunäite stsenaariumid Azure PowerShelli cmdlet-käskude abil.

## <a name="route-traffic-segment-to-a-deployment-slot"></a>Marsruutimiseks liikluse lõigu juurutamise pesa

Tavaline tasemel iga Näpunäide stsenaarium marsruutimine eelmääratletud protsendi reaalajas-liikluse mitte tekitamiseks juurutamine pesa. Selleks tehke järgmist:

>[AZURE.NOTE] Siin toodud juhistest eeldab, et teil on juba [mitte tekitamiseks juurutamine pesa](web-sites-staged-publishing.md) ja soovitud web appi sisu on juba [juurutanud](web-sites-deploy.md) see.

1. Logige [Azure portaali](https://portal.azure.com/).
2. Oma veebirakenduse tera, klõpsake nuppu **sätted** > **Liikluse marsruutimist**.
  ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Valige pesa, mida soovite marsruutida liikluse ja osakaal kokku liikluse soov, siis klõpsake nuppu **Salvesta**.

    ![](./media/app-service-web-test-in-production/02-select-slot.png)

4. Minge juurutamine pesa tera. Nüüd näete reaalajas liiklus selle marsruutimist.

    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

Kui liikluse marsruutimise on konfigureeritud, suunatakse kliendid määratud protsendini CEIP oma mitte tekitamiseks pesa. Siiski on oluline märkida, et kui klient suunatakse automaatselt teatud pesa, see on olema "kinnitatud" selle pesa kliendi seansi elu. Seda tehakse kinnitamine kasutaja seansi küpsise abil. Kui te kontrolli HTTP päringuid, siis leiate mõne `TipMix` küpsise iga järgmise taotluse.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a>Jõusta kliendi taotluste teatud pesa

Lisaks automaatse liikluse marsruutimine, on võimalik marsruutimiseks taotluste teatud pesa rakenduse teenus. See on kasulik, kui soovite valida, kas sisse või beeta rakenduse loobuda kasutajatele. Selleks saate kasutada funktsiooni `x-ms-routing-name` päringu parameeter.

Et kasutajad teatud pesa abil marsruudi `x-ms-routing-name`, peate kas liikluse kirjemarsruutimisloendisse pesa on juba lisatud. Kuna soovite marsruutimiseks pesa otseselt, saate määrata tegeliku marsruutimise protsenti järjekord oluline. Kui soovite, saate käsitöö "beeta link", mida kasutajad saavad klõpsata beeta rakenduse avamiseks.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>Kasutajalt beeta rakenduse loobumine

Selleks, et kasutajad beeta rakenduse loobuda, näiteks saate panna selle lingi oma veebilehele:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>

Stringi `x-ms-routing-name=self` määrab tootmise pesa. Kui Kliendi brauser juurdepääs link, suunatakse tootmise pesa, vaid iga järgmise taotlus sisaldab selle `x-ms-routing-name=self` küpsis viiku seansil jõuda tootmise pesa.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a>Kasutajate beeta rakenduse loobumine

Kui soovite lasta kasutajatel valida rakenduse beeta, määrake sama Päringuparameetri nime mitte tekitamiseks pesa, näiteks:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>Veel ressursse ##

-   [Lavastus keskkonnas web Appsi teenuses Azure rakenduse häälestamine](web-sites-staged-publishing.md)
-   [Keerukate rakenduses ootuspäraselt Azure juurutamine](app-service-deploy-complex-application-predictably.md)
-   [Dünaamilised tarkvara arendamine Azure'i rakenduse teenusega](app-service-agile-software-development.md)
-   [DevOps keskkonnas tõhusaks kasutamiseks oma veebirakendustes](app-service-web-staged-publishing-realworld-scenarios.md)