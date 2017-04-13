<properties
   pageTitle="Testige oma Azure web appi jõudluse | Microsoft Azure'i"
   description="Käivitage Azure web app jõudluse katsed, et kontrollida, kuidas rakenduse käsitleb kasutaja laadi. Mõõtke aega ja otsige tõrkeid, mis võib viidata probleeme."
   services="app-service\web"
   documentationCenter=""
   authors="ecfan"
   manager="douge"
   editor="jimbe"/>

<tags
   ms.service="app-service-web"
   ms.workload="web"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="05/25/2016"
   ms.author="estfan; manasma; ahomer"/>

# <a name="performance-test-your-azure-web-app-under-load"></a>Jõudlust testida oma Azure veebirakenduse koormuse

Märkige oma veebirakenduse jõudluse enne käivitada see või juurutada värskenduste tootmise. Nii saate paremini hinnata, kas teie rakendus on valmis versiooni. Kindlad rohkem, et teie rakendus saab hakkama liiklus tippväärtus kasutamise ajal või oma järgmise turundustegevuse tõuketeatised.

Avaliku eelvaate, saate katse oma rakendust tasuta Azure'i portaal.
Nende abil simuleerida kasutaja koormus rakenduse kindla perioodi jooksul ning mõõta oma rakenduse vastuse. Näiteks oma testi tulemused näitavad, kuidas kiiresti oma rakenduse vastaks teatud kasutajate arv. Sektordiagrammis kuvatakse ka mitu taotlusi nurjus, mis võib viidata probleeme oma rakendusega.      

![Otsige oma veebirakenduse jõudlusprobleemide](./media/app-service-web-app-performance-test/azure-np-perf-test-overview.png)

## <a name="before-you-start"></a>Enne alustamist

* Kui teil pole seda juba, peate on [Azure tellimust](https://account.windowsazure.com/subscriptions). Siit saate teada, kuidas saate [avada Azure'i konto tasuta](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

* Peate [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) konto säilitada ajaloo jõudluse test. Sobiva konto luuakse automaatselt, kui häälestate oma katse. Või saate luua uue konto või kasutada olemasolevat kontot, kui olete konto omanik. 

* Juurutage rakendus testimiseks mitte tekitamiseks keskkonnas. On teie leping valmistamisel kasutatud peale rakenduse teenusleping kasutada. Nii ei mõjuta kõik olemasolevad kliendid või aeglustada rakenduse valmistamisel. 

## <a name="set-up-and-run-your-performance-test"></a>Häälestamine ja käivitage oma katse

0.  [Azure'i portaali](https://portal.azure.com)sisse logima. Visual Studio Team Services konto, mille omanik te olete kasutamiseks logige sisse konto omanik.

0.  Minge oma veebirakenduse.

    ![Valige Sirvi kõiki, veebirakenduste, oma veebirakenduse](./media/app-service-web-app-performance-test/azure-np-web-apps.png)

0.  Avage **jõudluse Test**.

    ![Valige Tööriistad, katse](./media/app-service-web-app-performance-test/azure-np-web-app-details-tools-expanded.png)
 
0. Nüüd kuvatakse link [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) konto säilitada ajaloo jõudluse test.

    Kui teil on meeskonnatöö teenusekonto kasutada, valige selle konto. Kui te ei tee, looge uus konto.

    ![Valige meeskonnatöö teenuste kontot või uue konto loomine](./media/app-service-web-app-performance-test/azure-np-no-vso-account.png)

0.  Saate luua oma katse. Määrake üksikasjad ja käivitage test. 

Saate vaadata tulemusi reaalajas test käitamise ajal.

Oletame näiteks, et meil on rakendus, mis andis välja kupongide eelmine aasta müük. Sel juhul kestis 100 samaaegseid kliendid koormusega tippväärtus 15 minutit. Soovime topeltklõpsake klientide arvu, see aasta. Samuti saate suurendada klientide rahulolu vähendades laadimise ajal sekundi kaks sekundit all. Jah, me test värskendatud rakenduse jõudluse 250 kasutajat 15 minutit.

Meil kuvatakse simuleerida laadi meie rakenduse luues virtuaalse kasutajad (kliendid) kes külastage meie veebisaiti samal ajal. See näitab meile mitu taotlusi on puudumisel või vasta aeglaselt.

  ![Luua, häälestamine ja käivitada oma katse](./media/app-service-web-app-performance-test/azure-np-new-performance-test.png)

   *  Web appi vaike-URL lisatakse automaatselt. 
   Saate muuta testige muudelt lehtedelt (HTTP GET taotluste ainult) URL-i.

   *  Simuleerida kohalike tingimuste ja vähendamiseks latentsus, valige asukoht lähemal kasutajate loomiseks laadi.

  Siin on pooleli test. Esimese minuti jooksul meie lehe laadimise aeglasem kui soovime.

  ![Reaalajas andmed koos poolelioleva katse](./media/app-service-web-app-performance-test/azure-np-running-perf-test.png)

  Pärast test on lõpule jõudnud, saame teada lehe laadimise kiiremini esimese minuti järel. See aitab tuvastada, kus oleme soovida tõrkeotsingu käivitamiseks.

  ![Lõpetatud katse näitab tulemusi, sh nurjunud taotluste](./media/app-service-web-app-performance-test/azure-np-perf-test-done.png)

## <a name="test-multiple-urls"></a>Testida mitut URL-id

Samuti saate käivitada jõudluse kontrollib, mis sisaldavad mitut URL-id, mis tähistab mõne lõpuni – kasutaja stsenaariumi Visual Studio Web testi faili üleslaadimisel. Mõned viisid, kuidas saate luua Visual Studio Web testi fail on:

* [Liiklus kasutades viiuldaja jäädvustada ja Visual Studio Web testi faili eksportida](http://docs.telerik.com/fiddler/Save-And-Load-Traffic/Tasks/VSWebTest)
* [Visual Studio laadi test faili loomine](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release)

Üles laadida ja käivitada Visual Studio Web testi faili:
 
0. Järgige ülaltoodud juhiseid, et avada **Uus jõudlust testida** tera.
   Valige see blade CONFIGFURE TESTIDA kasutamise võimalus tera **konfigureerimine testi abil** avada.  

    ![Testimine blade konfigureerimine laadi avamine](./media/app-service-web-app-performance-test/multiple-01-authoring-blade.png)

0. Valige HTTP arhiivifaili **Visual Studio Web** katsetamiseks on määratud testi tüüp.
    Avage fail Vaateselektori dialoogiboksi ikooni "kaust" abil.

    ![URL-i Visual Studio Web testi mitme faili üleslaadimine](./media/app-service-web-app-performance-test/multiple-01-authoring-blade2.png)

    Kui fail on üles laaditud, näete URL-ide testitakse jaotises URL-i üksikasjad.
 
0. Määrake kasutaja laadimine ja katse kestus, seejärel valige **käivitage test**.

    ![Valige kasutaja laadimine ja kestus](./media/app-service-web-app-performance-test/multiple-01-authoring-blade3.png)

    Kui test on lõpule jõudnud, näete tulemusi kaks paani. Klõpsake vasakpoolsel paanil kuvatakse performnace teave diagrammide reana.

    ![Jõudluse otsingutulemite paan](./media/app-service-web-app-performance-test/multiple-01a-results.png)

    Paremal paanil kuvatakse nurjunud taotlusi, tüüpi vigade ja mitu korda ilmnes selle loendi.

    ![Paani taotluse tõrked](./media/app-service-web-app-performance-test/multiple-01b-results.png)

0. Käivitage test, valides Parempoolse paani ülaosas **taaskäivitage rakenduse** ikooni.

    ![Võrdlusfiltri](./media/app-service-web-app-performance-test/multiple-rerun-test.png)

##  <a name="q--a"></a>K & v

#### <a name="q-is-there-a-limit-on-how-long-i-can-run-a-test"></a>Q: Kas on piiratud kaua ma käitamise test? 

**A**: Jah, saate kasutada oma test kuni tund Azure'i portaal.

#### <a name="q-how-much-time-do-i-get-to-run-performance-tests"></a>Q: kuidas palju aega saan jõudluse käivitada? 

**A**: pärast avaliku eelvaade, saate 20 000 virtuaalse kasutaja minutit (VUMs) tasuta iga kuu Visual Studio Team Services kontoga. Mõne VUM on arv, virtuaalse kasutajate multipled oma test minutite arv. Kui tasuta limiiti ületada teie vajadustele, saate osta rohkem aega ja maksma ainult, mida te kasutate.

#### <a name="q-where-can-i-check-how-many-vums-ive-used-so-far"></a>K: kus saan vaadata, mitu VUMs olen siiani kasutanud?

**A**: saate vaadata selle summa Azure'i portaalis.

![Minge oma meeskonna teenusekonto](./media/app-service-web-app-performance-test/azure-np-vso-accounts.png)

![Märkige ruut VUMs kasutatud](./media/app-service-web-app-performance-test/azure-np-vso-accounts-vum-summary.png)

#### <a name="q-what-is-the-default-option-and-are-my-existing-tests-impacted"></a>Q: mis on vaikimisi valitud ja minu olemasoleva kontrollib mõjutab?

**A**: jõudluse laadi testide vaikevalik on käsitsi test – sama nagu enne mitme URL-i testida suvand on lisatud portaali.
Teie olemasoleva kontrollib edasi kasutada konfigureeritud URL-i ja töötavad nagu enne.

#### <a name="q-what-features-not-supported-in-the-visual-studio-web-test-file"></a>Q: mis funktsioonid pole rakenduses Visual Studio Web testi faili?

**A**: praegu seda funktsiooni ei toeta Web Test lisandmoodulite, andmeallikate ja eraldamine reeglid. Redigeerige Web Proovifail neid eemaldada. Loodame, et lisada need funktsioonid tulevikus värskendused tugi.

#### <a name="q-does-it-support-any-other-web-test-file-formats"></a>Q: Kas see toetab mis tahes muud Web Test failivormingud?
  
**A**: toetatud vormingus failide esitamiseks ainult Visual Studio Web test.
Meil oleks rahul, et kuulda, kui teil on vaja tugi muudes failivormingutes. Saada meile aadressil [vsoloadtest@microsoft.com](mailto:vsoloadtest@microsoft.com).

#### <a name="q-what-else-can-i-do-with-a-visual-studio-team-services-account"></a>Q: mis veel teha Visual Studio Team Services kontoga?

**A**: kontosse leidmiseks minge ```https://{accountname}.visualstudio.com```. Jagada oma kood, koostada, testimine jälgida töö ja tarne tarkvara – kõik pilves, või mis tahes keel. Lisateavet selle kohta, kuidas aidata [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) funktsioone ja teenuseid teie meeskond koostööd teha ja pidevalt juurutamine.

## <a name="see-also"></a>Vt ka

* [Lihtne pilve jõudlus analüüse](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-simple-cloud-load-test)
* [Apache Jmeter jõudluse analüüse](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-jmeter-test)
* [Salvestamine ja kordus pilvepõhist laadi testide](https://www.visualstudio.com/docs/test/performance-testing/getting-started/record-and-replay-cloud-load-tests)
* [Jõudlust testida rakenduse pilveteenuses](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing)
