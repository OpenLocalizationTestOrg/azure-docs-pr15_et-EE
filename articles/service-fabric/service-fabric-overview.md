<properties
   pageTitle="Teenuse struktuuri ülevaade | Microsoft Azure'i"
   description="Teenuse struktuuri, kui palju microservices skaala ja paindlikkuse koosnevad rakenduste ülevaade. Teenus on hajutatud süsteemide platvormi kasutatakse koostamiseks scalable, usaldusväärseid ja hõlpsasti hallatava taotlused pilveteenuses"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="mfussell"/>

# <a name="overview-of-service-fabric"></a>Teenuse struktuuri ülevaade
Teenus on hajutatud süsteemide platvorm, mis hõlbustab paketti, juurutada ja hallata skaleeritav ja usaldusväärne microservices. Teenuse struktuuri käsitletakse raskused arendamise ja pilveteenuse rakenduste haldamine. Arendajate ja administraatorite saate vältida lahendamine keerukate taristu probleemide ja fookuse hoopis olulise, rangete töökoormus teab, et need on scalable, usaldusväärseid ja mõistliku rakendamise kohta. Teenuse struktuuri tähistab edasi-genereerimine vahevara platvormi luua ja hallata neid äriklassi, järk-1 cloud-skaala rakendused.

Selles [lühikeses videos](https://aka.ms/servicefabricvideo) antakse teenuse struktuuri ja microservices tutvustus.


## <a name="applications-composed-of-microservices"></a>Rakenduste koosneb microservices
Teenuse struktuuri võimaldab teil luua ja hallata scalable ja usaldusväärsete rakenduste koosneb microservices töötab väga kõrge tihedusega ühiskasutusega kogumi masinad (edaspidi klaster). Keerukate käitusaja pakub jaotatud, scalable kodakondsuseta ja stateful microservices jaoks. See sisaldab ka täielik rakenduse halduse võimaluste ettevalmistamise, juurutamine, jälgimine, üleminekut/lappimine ja kustutamise juurutatud rakendused.

Miks on microservices lähenemine oluline? On kaks peamist põhjust.

1. Need võimaldavad teil mastaapimiseks erinevate osade rakenduse sõltuvalt oma vajadustele.

2. Arendusmeeskondadel on võimalik olla rohkem dünaamilised jooksvalt muudatusi läbi ja seetõttu pakuvad funktsioone oma klientidele kiiremini ja sagedamini.

Teenuse struktuuri volitused paljude Microsofti teenuste täna, sh Azure'i SQL-andmebaasi, Azure'i DocumentDB, Cortana, Power BI, Microsoft Intune'i, Azure'i sündmuse jaoturi, Azure'i asjade Skype'i ärirakenduse ja paljud core Azure teenused.

Teenuse struktuuri on kohandatud loomine pilveteenuses kohalikke teenuseid, saate alustada väike, vastavalt vajadusele ja suuri skaala sadu või tuhandeliste masinad kasvada.

Tänase Interneti-teenuste ehitatakse microservices. Näited microservices protokolli lüüsid, kasutajaprofiilide ostmine ostukärud, varude töötlemise, järjekordade ja vahemälu. Teenus on microservices platvorm, mis annab iga microservice kordumatu nimi, mis võib olla kodakondsuseta või stateful.

Teenuse struktuuri pakub täielik käitusaja ja elutsükli halduse võimaluste rakendustele koosneb nende microservices. See hosts microservices ümbriste juurutatud ja teenuse struktuuri kobar üle aktiveeritud sees. Üleminek rakenduselt VMs ümbriste teeb võimalik järjestuses, suuruse suurendamine tihedusega. Samuti saab teisaldades ümbriste microservices võimalikuks teise mitmeks suurus tihedusega. Näiteks ühe Azure'i SQL-andmebaasi kobar hõlmab sadu ümbriste majutusteenuse andmebaaside tuhandetele kokku tuhandetele kümneid, kus käitatakse. Iga andmebaas on teenuse struktuuri stateful microservice. Sama kehtib muude teenuste eelnevalt mainitud, mistõttu Termini "hyperscale" kasutatakse teenuse struktuuri võimaluste kirjeldamiseks. Kui ümbriste teile kõrge tihedus, siis microservices teile hyperscale.

Lugege lisateavet microservices lähenemine, [miks microservices lähenemine rakenduste loomisse?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>Container juurutus- ja korraldamise
Teenus on mõni [orchestrator](service-fabric-cluster-resource-manager-introduction.md) microservices kobar masinad üle. Microservices saate välja töötada [teenuse struktuuri programmeerimise mudelite](service-fabric-choose-framework.md) juurutamine [Külastajate täitmisfailid](service-fabric-deploy-existing-app.md)kasutada mitmel viisil. Teenuse struktuuri saate juurutada teenused container pilte ja olulisem segada nii protsesside teenused ja teenuste ümbriste koos sama rakenduses. Kui soovite lihtsalt [juurutada](service-fabric-containers-overview.md) ja hallata container piltide üle kobar masinad, teenus on parim valik.


## <a name="create-service-fabric-clusters-anywhere"></a>Teenuse struktuuri kogumite suvalist loomine
Saate luua teenuse struktuuri kogumite palju keskkonnas, sh Azure või kohapealne, Windows Server või Linux. Lisaks arenduskeskkond SDK on identne tootmiskeskkonda pole kaasatud emulators abil. Teisisõnu, kui see töötab kohaliku arengu klaster selle juurutamine sama kobar muus keskkonnas.

Kogumite kohapealne, lugeda, [luua klaster opsüsteemis Windows Server ja Linux](service-fabric-deploy-anywhere.md) loomise kohta lisateabe saamiseks ja Azure loomine soovitud klaster [Azure portaali kaudu](service-fabric-cluster-creation-via-portal.md).

![Teenuse struktuuri platvorm][Image1]

## <a name="stateless-and-stateful-service-fabric-microservices"></a>Kodakondsuseta ja stateful teenuse struktuuri microservices

Teenuse struktuuri võimaldab teil luua rakendusi, mis koosneb microservices. Kodakondsuseta microservices (lüüside protokolli, web puhverserverite jne) ei säilita muutlik olekus väljaspool mis tahes antud koosolekukutsete ja teenuse oma vastuse. Azure'i pilveteenustega töötaja rollid on kujutatud kodakondsuseta teenus. Stateful microservices (Kasutajakontod, andmebaaside, seadmed, ostmine teisiti, järjekorrad jne) säilitada muutlik, autoriteetsete olekus taotlus ja oma vastus. Tänase Interneti-skaala rakenduste koosnevad kodakondsuseta ja stateful microservices kombinatsiooni.

Miks on stateful microservices koos kodakondsuseta neist? On kaks peamist põhjust.

1. Võimalus luua suure läbilaskevõimega, latentsusajaga, tõrke-tolerantset online tehingu töötlemise (OLTP) teenuste kood ja sulgege andmete säilitamise samasse arvutisse. Mõned näited interaktiivsed storefronts, otsing, Internet, asjade süsteemid, kauplemissüsteemide, krediitkaardi töötlemine ja pettuse tuvastamise süsteemid ja isikliku kirjehalduse.

2. Rakenduse kujundus lihtsustamine. Stateful microservices eemaldamine vajate täiendavaid järjekorrad ja vahemälu, traditsiooniliselt vajalikke puhtalt kodakondsuseta rakenduse kättesaadavus ja latentsusaeg nõuetele. Stateful teenused on loomulikult kõrge-saadavus ja latentsusajaga, liikuvad osad, et hallata oma rakenduse kogu arvu.

Lisateavet rakenduse mustrite teenuse struktuuri, vt [rakenduse stsenaariumid](service-fabric-application-scenarios.md) ja [valides programmeerimise mudeli framework](service-fabric-choose-framework.md) teie teenuse jaoks

## <a name="application-lifecycle-management"></a>Rakenduse elutsükli haldus
Teenuse struktuuri toetab esimese klassi rakenduse täisversioon elutsükli haldus (ALM) pilv rakendusi--arengu juurutus, igapäevane haldus ja hooldus kuni lõpliku.

Teenuse struktuuri ALM võimaluste lubamine administraatorid ja IT tehtemärgid sätet lihtne, madal puutega töövoogude kasutamise juurutada, paik, ja rakenduste jälgimine. Järgmised valmistöövood oluliselt vähendada koormust IT tehtemärkide hoida pidevalt saadaolevad rakendused.

Enamiku rakenduste koosnevad kodakondsuseta ja stateful microservices ja muude täitmisfailid/runtimes juurutatud koos kombinatsiooni. Keeruka tüüpi rakendusi ja pakkimisel microservices lastes teenuse struktuuri võimaldab mitu rakenduse eksemplari juurutamise. Iga eksemplari hallatavate ja uuendatud sõltumatult. Oluline on, on võimalik juurutada *mis tahes* täitmisfailid või käitusaja ja veenduge, et need usaldusväärne teenuse struktuuri. Näiteks teenuse struktuuri kasutab ASP.net-i Core 1, Node.js, Java VMs, skriptide või midagi muud, mis moodustab rakenduse.

Rakendusehalduse elutsükli kohta lisateabe saamiseks lugege [rakenduse elutsükli](service-fabric-application-lifecycle.md) ja juurutamine koodi vt [Deploy külalisena käivitatava](service-fabric-deploy-existing-app.md)

## <a name="key-capabilities"></a>Võtme võimalused
Teenuse struktuuri abil saate teha järgmist.

- Oluliselt scalable rakendusi, mis on iseparandavat arendada.

- Rakendusi, mis koosneb teenuse struktuuri programmeerimise näidise microservices arendada. Või lihtsalt majutada Külastajate Käivitusfailid ja muude rakenduste raamistiku teie valitud, nt ASP.net-i Core 1 või Node.js.

- Väga usaldusväärne kodakondsuseta ja stateful microservices välja.

- Juurutada ja korraldab ümbriste sisaldavad Windows ümbriste ja keskmise suurusega ümbriste klaster üle. Nende ümbriste saate container Külastajate täitmisfailid või usaldusväärne kodakondsuseta ja stateful microservices.  Mõlemal juhul saate container port host port kaardistamine, container leitavust ja automatiseeritud Tõrkesiirde.

- Lihtsustage rakenduse kujunduse stateful microservices asemel vahemälu ja järjekordade abil.

- Azure'i või kohapealse pilved opsüsteemiga Windows Server või Linuxi null kood muudatustega juurutamine. Kirjutage üks kord ja seejärel juurutada suvalist mis tahes teenuse struktuuri kobar.

- Välja "andmekeskuse teie arvutis" lähenemine. Kohaliku arenduskeskkond on sama kood, mis töötab Azure andmekeskuste.

- Juurutada rakendused sekundites.

- Juurutada rakendused suurem tihedus kui virtuaalmasinates, juurutamine sadu või tuhandeliste rakenduste masina kohta.

- Erinevaid versioone kõrvuti, iga sõltumatult ajakohastatavatele sama rakenduse juurutamine.

- Elutsükli ilma mis tahes tööseisakute, sh breaking ja sisestamine täienduste stateful rakenduste haldamine.

- .Net-i API-d, Java (Linux), PowerShelli, Azure'i CLI (Linux) või ülejäänud liidesed rakenduste haldamine.

- Täienduste ja paikade microservices rakenduste sõltumatult.

- Jälgimine ja rakenduste seisundi diagnoosimine ja juurutada poliitikad läbimiseks automaatne parandamine.

- Võimalus skaala- või skaala-sõlmed kobar, samuti skaala üles arv või skaala-down iga sõlme, teab rakenduste automaatselt skaala ja on levitatud vastavalt ressursid suurust.

- Vaata iseparandavat ressursi koormusetasakaalustusteenuse korraldab korduvalt rakenduste klaster üle. Teenuse struktuuri taastab tõrkeid ja optimeerib laadi põhjal saadaval ressursside jaotuse.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud

* Lisateave:
    * [Miks on microservices lähenemine rakenduste loomisse?](service-fabric-overview-microservices.md)
    * [Terminoloogia ülevaade](service-fabric-technical-overview.md)
* Kuidas häälestada teenuse struktuuri [arenduskeskkond](service-fabric-get-started.md)  
* Teie teenuse jaoks [programmeerimise mudeli raamistiku valimine](service-fabric-choose-framework.md)

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
