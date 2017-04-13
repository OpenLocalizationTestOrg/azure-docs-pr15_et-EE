<properties 
    pageTitle="Kuidas mastaapimiseks rakendus App teenuse keskkonnas" 
    description="Skaleerimist rakendus App teenuse keskkonnas" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="ccompy"/>

# <a name="scaling-apps-in-an-app-service-environment"></a>Skaleerimise rakendused rakendus teenuse keskkonnas #

Teenuses Azure rakendus on tavaliselt kolm üksust saab.

- hinnad kavandamine
- töötaja suurus 
- eksemplaride arv.

Klõpsake soovitud ASE ei ole vaja valida või muuta hinnakirjad leping.  Võimaluste osas on juba hinnad võimalus taseme lisatasu.  

Seoses töötaja suurused, ASE administraator saab määrata Arvuta ressursi kasutatakse iga töötaja kaust suurust.  See tähendab, saate määrata töötaja kaust 1 P4 Arvuta ressursid ja töötaja kaust 2 koos P1 arvutada ressursse, kui soovitud.  Nad ei saa olla, et suurus.  Ümber suurused ja nende hinnakirjad leiate siit dokumendi üksikasjad jaoks [Azure'i rakenduse teenuse hinnad][AppServicePricing].  See jätab rakenduse teenuse keskkonnas olevat skaleerimise suvandid veebirakenduste ja rakenduse teenuse lepingud:

- töötaja rakenduskausta valiku
- eksemplaride arv

Kas üksuse muutmise tehakse näidatud jaoks oma ASE majutatud rakenduse teenuse lepingute korral UI.  

![][1]

Ei saa skaalal oma ASP Lisaks saadaval Arvuta ressursid on teie ASP töötaja kogumi arv.  Kui teil on vaja arvutada ressursse, et töötaja pargis vajate ASE administraator neid lisada.  Teave ümber konfigureerida uuesti oma ASE lugeda teavet siin: [Kuidas konfigureerida rakenduse teenuse keskkonnas][HowtoConfigureASE].  Võite ka ASE autoscale funktsioonide lisamine ajakava või mõõdikute põhjal võimsus.  Üksikasjade konfigureerida autoscale ASE keskkond teada, [Kuidas konfigureerida autoscale keskkonnas rakenduse teenuse jaoks][ASEAutoscale].

Saate luua mitme rakenduse teenuse Arvuta ressursse kaudu erinevate töötaja kaustu või saate kasutada sama töötaja pool.  Näiteks kui töötaja kaust 1 (10) saadaval Arvuta ressursid on, saate valida ühe rakenduse plaani abil (6) Arvuta ressursid loomiseks ja teine rakendus teenuse leping, mis kasutab (4) Arvutage ressursid.

### <a name="scaling-the-number-of-instances"></a>Skaleerimist eksemplaride arv ###

Kui loote esmalt oma veebirakenduse keskkonnas rakenduse teenuse algab 1 eksemplar.  Seejärel saate välja skaala täiendavad eksemplarides esitada oma rakenduse täiendavad Arvuta ressursse.   

Kui teie ASE on piisavalt, siis see on üsna lihtne.  Lähete oma rakenduse teenuse leping saidid, mida soovite kohandada, ning valige skaala versioonimissätteid.  Avatakse kasutajaliides, kus saate käsitsi seadmine oma ASP skaala või konfigureerimine oma ASP autoscale reeglid.  Käsitsi skaala rakenduse lihtsalt Määrake ***skaalale,*** ***eksemplari arv, mis käsitsi sisestada***.  Siin lohistage liugur soovitud kogus või sisestage selle liuguri kõrval asuvale väljale.  

![][2] 

ASP on ASE autoscale reeglid töötavad sama, nagu tavaliselt.  Saate valida ***CPU protsent*** skaala ***järgi*** ja autoscale reeglite loomine oma ASP CPU protsendi alusel või ***plaanimine ja jõudluse reeglite***kasutamine keerukamate reeglite loomiseks.  Konfigureerimise kohta rohkem kõigi üksikasjade kuvamiseks kasutage autoscale juhend siin [mastaapimiseks rakenduse Azure'i rakendust Service][AppScale]. 


### <a name="worker-pool-selection"></a>Töötaja rakenduskausta valiku ###

Nagu varem, pääseb töötaja rakenduskausta valiku ASP UI.  Saate avada tera ASP, mida soovite skaala ja valige töötaja pool.  Kuvatakse kõik töötaja kaustu, mille olete konfigureerinud oma rakenduse teenuse keskkonnas.  Kui teil on ainult üks töötaja pool siis ainult kuvatakse loendis ühe kausta.  Mis on teie ASP töötaja rakenduskausta muutmiseks valite lihtsalt töötaja kausta, mida soovite, et teie rakendus teenuse leping liikumiseks.  

![][3]

Enne oma ASP töötaja ühest kaustast teise on oluline veenduge, et teil on piisavalt oma ASP võimsus.  Töötaja kaustu loendis töötaja kausta nimi kuvatakse vaid näete ka, kui palju töötajaid on saadaval selle töötaja pool.  Veenduge, et on piisavalt eksemplarid, mis on saadaval rakenduse teenuse leping sisaldab.  Kui teil on vaja rohkem arvutada ressursid töötaja kogumi, mida soovite teisaldada, siis tulemuseks ASE administraator neid lisada.  

> [AZURE.NOTE] Teisaldada ühe töötaja kaustast ASP põhjustab külma käivitub, et ASP rakendused.  See võib põhjustada taotlusi aeglaseks nagu teie rakendus on külm alustada uut Arvuta ressursid.  [Võimalus üles rakendus soe] abil saab vältida külmalt käivitamise[ AppWarmup] teenuses Azure rakendus.  Rakenduse lähtestamise mooduli kirjeldatud artiklis töötab ka külma käivitamise Kuna lähtestamisel on vaja järgida ka siis, kui rakendused on külma alustada uut Arvuta ressursid. 

## <a name="getting-started"></a>Alustamine

Alustamine rakenduse teenuse keskkonnas, lugege teemat [Kuidas soovite luua An rakenduse teenuse keskkonnas][HowtoCreateASE]

Azure'i rakendust Service platform kohta leiate lisateavet teemast [Azure rakenduse teenuse][AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
