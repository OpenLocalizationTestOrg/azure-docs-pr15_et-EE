<properties 
    pageTitle="Tere tulemast maailma veebirakenduse loomine Azure huvitav | Microsoft Azure'i" 
    description="Selle õpetuse näidatakse, kuidas kasutada Azure tööriistakomplekt huvitav jaoks luua Tere maailma veebirakenduse Azure." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="selvasingh" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-intellij"></a>Tere tulemast maailma veebirakenduse jaoks huvitav Azure loomine

Selle õpetuse näitab, kuidas luua ja juurutada Tere, maailm taotluse Azure Web App abil [Azure'i tööriistakomplekt huvitav jaoks]. Tavalise JSP näide on esitatud lihtne, kuid väga sarnane juhiseid oleks vastav Java servlet, kuna Azure juurutamise on.

Kui olete täitnud selles õpetuses, rakenduse sarnanema järgmisega järgmisel joonisel kuvatava veebibrauseris:

![][01]
 
## <a name="prerequisites"></a>Eeltingimused

* Mõne Java arendaja Kit (JDK), v 1,8 või uuem versioon.
* Huvitav idee Ultimate Edition. See saab alla laadida <https://www.jetbrains.com/idea/download/index.html>.
* Java-põhine veebiserverisse või rakenduse server, nt Apache Tomcat või Jetty jaotuse.
* Azure'i tellimuse, <https://azure.microsoft.com/free/> või <http://azure.microsoft.com/pricing/purchase-options/>omandanud.
* Azure'i tööriistakomplekt huvitav jaoks. Lisateabe saamiseks lugege teemat [installimist Azure tööriistakomplekt huvitav jaoks].

## <a name="to-create-a-hello-world-application"></a>Tere tulemast maailma rakenduse loomine

Kõigepealt tuleb Alustame Java projekti loomine.

1. Käivitage huvitav, ja klõpsake menüü **fail**ja seejärel valige **Uus**ja klõpsake **projekti**.

   ![][02]

1. Dialoogiboksis Uus projekt valige **Java**, siis **Veebirakenduse**, ja seejärel klõpsake nuppu **edasi**.

   ![][03a]

   Kui teil palutakse pole määratud SDK jätkata, klõpsake nuppu **Jah**.

   ![][03b]

1. Selleks selles õpetuses, **Java-Web-rakendus – klõpsake-Azure'i**projekti nime ja klõpsake siis nuppu **valmis**.

   ![][04]

1. Huvitav 's Project Exploreri vaates, laiendage **Java Web-rakendus – klõpsake-Azure**, ja seejärel laiendage **web**ja seejärel topeltklõpsake **index.jsp**.

   ![][05]

1. Index.jsp faili avamisel huvitav lisada dünaamiliselt kuvatav tekst **Tere, maailm!** jäävad `<body>` element. Teie värskendatud `<body>` sisu peaks sarnaneb järgmises näites:

   `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Salvestage index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Azure'i Web Appi Container rakenduse juurutamiseks

On mitu võimalust, mille saate juurutada Azure Java veebirakenduse. Selles õppetükis kirjeldatakse üks kõige lihtsam: rakenduse juurutatakse abil Azure Web Appi Container – pole teisiti projekti tüüp ega täiendavad tööriistad on vaja. Funktsiooni JDK ja tarkvara web container pakutav teile Azure, seega ei ole vaja üles laadida oma; teil on vaja oma Java Web App. Selle tulemusena võtab avaldamise protsessi rakenduse sekundit, mitte minutit.

1. Huvitav 's Project Explorer, paremklõpsake **Java Web-rakendus – klõpsake-Azure** projekt. Kontekstimenüü kuvamisel valige **Azure**ja seejärel klõpsake nuppu **Avalda Azure Web u...**

   ![][06]

1. Kui te pole veel huvitav Azure logitud, palutakse teil oma Azure'i kontosse:

   ![][07]

   Märkus: Kui teil on mitu Azure kontod, mõned viipasid ajal sisselogimine protsess võib kasutada rohkem kui üks kord, isegi juhul, kui need olevat sama. Kui see juhtub, jätkake pärast sisselogimist juhiseid.

1. Pärast seda, kui olete edukalt Azure'i kontosse sisse loginud, kuvatakse dialoogiboks **Tellimuste haldamine** loend tellimused, mis on seotud oma kasutajanimi ja parool. Kui määratud on loetletud mitu tellimust ja te soovite töötada vaid teatud osa neist, võib-olla tühjendage ruut võite need, mida soovite kasutada. Kui olete valinud tellimuste, klõpsake nuppu **Sule**.

   ![][08]

1. Kui kuvatakse dialoogiboks **Deploy Azure Web Appi Container abil** , kuvatakse mis tahes Web Appi ümbriste, mida olete varem loonud; kui te pole loonud mis tahes ümbriste, loend on tühi.   

   ![][09]

1. Kui te pole loonud Azure Web Appi Container enne või kui soovite avaldada oma rakenduse uus ümbris, siis tehke järgmist. Muul juhul valige mõne olemasoleva Web Appi ümbris ja jätkake 6 all.

  1. Klõpsake nuppu**+**

        ![][10]

  1. Dialoogiboks **Uus Web Appi ümbris** kuvatakse, milles kasutatakse mitut järgmist.

        ![][11]

  1. Sisestage **DNS-i silt** Web Appi Container; lehe DNS-i silt oma veebirakenduse URL-i hosti moodustavad Azure. Märkus: Nimi peab olema saadaval ja Azure Web Appi nime andmise nõuded vasta.

  1. Valige rippmenüüst **Web Container** vastav tarkvara rakenduse.

        Praegu saate valida Tomcat 8 Tomcat 7 või Jetty 9. Viimaste jaotumine valitud tarkvara pakutav Azure ja see töötab tehtud jaotuse JDK 8 loodud Oracle'i ja Azure järgi.

  1. Valige rippmenüü **tellimus** see tellimus, mida soovite kasutada seda juurutamiseks.

  1. Valige rippmenüüst **Ressursirühm** ressursirühm, kellega soovite oma veebirakenduse seostada.

        Märkus: Azure'i ressursi rühmade abil saate omavahel seotud ressursid rühmitada nii, et näiteks nende kustutamist koos.

        Saate valida olemasoleva ressursirühm (kui teil on mõni) ja jäta samm g all või kasutage neid juhiseid uue ressursirühma loomiseks järgmist:

      * Klõpsake **Uus...**

      * Dialoogiboksi **Uue ressursirühma** kuvatakse:

            ![][12]

      * Klõpsake soovitud **nimi** rühmitusaluse määramiseks nimi uue ressursirühma.

      * Klõpsake soovitud **regioon** rippmenüüst menüü, valige sobiv Azure'i andmed keskele ressursirühma asukoht.

      * Klõpsake nuppu **OK**.

  1. Rippmenüü **Rakenduse teenuse leping** on loetletud rakenduse teenuse lepingud, mis on seotud teie valitud ressursirühma.

        Märkus: Rakenduse teenuse leping määrab teabe nt asukoht oma veebirakenduse, hinnakirjad taseme ja Arvuta eksemplari suurus. Ühe rakenduse teenuse leping saab kasutada mitut veebirakendusi, mis on põhjus, miks see säilib eraldi kindla veebirakenduse juurutamise.

        Saate valida mõne olemasoleva rakenduse teenuse kavandamine (kui teil on mõni) ja jäta samm h allpool või kasutage neid juhiseid luua uue rakenduse teenuse Plan järgmist:

      * Klõpsake **Uus...**

      * Kuvatakse dialoogiboks **Uue rakenduse teenuse kavandamine** :

            ![][13]

      * Klõpsake soovitud **nimi** rühmitusaluse määramiseks nimi oma uue rakenduse teenuse kavandamine.

      * Rakenduses soovitud **asukohta** rippmenüüst menüü, valige sobiv Azure'i andmed keskele leping asukoht.

      * Klõpsake soovitud **Hinnad taseme** rippmenüü, valige sobiv hinnad lepingu raames. Katsetamiseks saate **tasuta**.

      * Klõpsake soovitud funktsiooni **Eksemplari suurus** rippmenüü, valige sobiv eksemplari suuruse lepingu raames. Katsetamiseks saate valida **Small**.

  1. Kui olete täitnud kõiki ülaltoodud juhiseid, näeb dialoogiboksi Uus Web Appi ümbris järgmisel joonisel:

        ![][14]

  1. Klõpsake nuppu **OK** , et viia lõpule oma uue veebirakenduse container.

        Oodake paar sekundit veebirakenduse ümbriste loendi värskendamist ja oma vastloodud web appi container peaks olema loendist valitud.

1. Nüüd olete valmis lõpuleviimiseks esialgse kasutuselevõtu oma veebirakenduse Azure; Klõpsake nuppu **OK** , et teie valitud veebirakenduse container Java rakenduse juurutamine.

    ![][15]

    Märkus: Juurutatakse nimega alamkausta serveri vaikimisi rakenduse. Kui soovite, et see rakendus juurkausta kasutusele võtta, märkige ruut **Deploy root** enne nupu **OK klõpsamist**.

1. Järgmiseks peaksite **Azure'i tegevuse Log** vaade, mis näitab oma veebirakenduse juurutamise olekut.

    ![][16]

    Protsessi juurutamine oma veebirakenduse Azure peaks tegema, vaid mõne hetke. Kui teie taotlus valmis, näete linki nimega **avaldatud** veerus **olek** . Kui klõpsate linki, see viib teid avalehele oma juurutatud veebirakenduse või abil saate juhiseid kohta järgmisest jaotisest otsige sirvides üles oma veebirakenduse.

## <a name="browsing-to-your-web-app-on-azure"></a>Kui soovite oma veebirakenduse Azure sirvimine

Kulmud Azure veebirakendusse, et saate kasutada **Azure Exploreri** vaade.

Kui **Azure Exploreri** vaade on juba avatud, saate selle avamiseks klõpsata seejärel huvitav, klõpsake menüü **Vaade** ja seejärel klõpsake **Tööriista Windows**ja klõpsake **Teenuse Explorer**. Kui te ei ole varem sisse loginud, palutakse teil seda teha.

**Azure'i Exploreri** vaade kuvamisel kasutamine tehke järgmist peatamiseks oma veebirakenduse. 

1. Laiendage **Azure** .

1. Laiendage **Web Apps** . 

1. Paremklõpsake soovitud Web App.

1. Kui kuvatakse kontekstimenüü, klõpsake nuppu **Ava brauseris**.

    ![][17]

## <a name="updating-your-web-app"></a>Oma veebirakenduse värskendamine

Värskendamine olemasoleva töötab Azure Web App on kiire ja lihtne protsess ja on kaks võimalust värskendamine:

* Juurutamise olemasoleva Java Web App saate värskendada.
* Java taotlus sama ümbris Web Appi abil saate avaldada.

Mõlemal juhul protsess on identne ja võtab aega ainult mõne sekundi:

1. Huvitav project explorer, paremklõpsake soovitud Java rakendus, mida soovite värskendada või lisada mõne olemasoleva Web Appi ümbrises.

1. Kontekstimenüü kuvamisel valige **Azure** ja seejärel **Avalda Azure Web u...**

1. Kuna teil on juba sisse logitud varem, kuvatakse teie olemasoleva veebirakenduse ümbriste loendit. Valige see, mille soovite avaldada või uuesti avaldada oma Java taotlus ja klõpsake nuppu **OK**.

Mõne hetke hiljem **Azure'i tegevuse Log** vaates kuvatakse kujul **avaldatud** värskendatud juurutamise ja saab kontrollida värskendatud rakenduse veebibrauseris.

## <a name="starting-or-stopping-an-existing-web-app"></a>Käivitamine või peatamine olemasoleva Web App

Alustamine või lõpetamine olemasoleva Azure Web Appi container, (sh juurutatud Java rakendusi see), kasutage **Azure'i Exploreri** vaade.

Kui **Azure Exploreri** vaade on juba avatud, saate selle avamiseks klõpsata seejärel huvitav, klõpsake menüüd **Vaade** ja seejärel klõpsake **Tööriista Windows**ja klõpsake **Teenuse Explorer**. Kui te ei ole varem sisse loginud, palutakse teil seda teha.

**Azure'i Exploreri** vaade kuvamisel kasutada järgmiste juhiste abil alustamine või lõpetamine oma veebirakenduse: 

1. Laiendage **Azure** .

1. Laiendage **Web Apps** . 

1. Paremklõpsake soovitud Web App.

1. Kontekstimenüü kuvamisel klõpsake **Alustamine** või **lõpetamine**. Pange tähele, et menüü Valikud on kontekstis arvestada, nii saate ainult töötava veebirakenduse peatamine või käivitada web appi, mis on praegu ei tööta.

    ![][18]

## <a name="next-steps"></a>Järgmised sammud

Vaadake lisateavet Azure tööriistakomplektid Java interaktiivset järgmisi linke:

- [Azure'i tööriistakomplekt Eclipse]
  - [Azure'i tööriistakomplekt Eclipse installimine]
  - [Tere tulemast maailma veebirakenduse jaoks Azure Eclipse loomine]
  - [Mis on uut rakenduses Azure tööriistakomplekt Eclipse]
- [Azure'i tööriistakomplekt huvitav jaoks]
  - [Azure'i tööriistakomplekt huvitav installimisega]
  - *Tere tulemast maailma veebirakenduse loomine Azure huvitav (selles artiklis)*
  - [Mis on uut rakenduses Azure tööriistakomplekt huvitav jaoks]

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus].

Azure'i veebirakenduste loomise kohta lisateabe saamiseks vt [Web Appsi ülevaade].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure'i tööriistakomplekt Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure'i tööriistakomplekt huvitav jaoks]: ../azure-toolkit-for-intellij.md
[Tere tulemast maailma veebirakenduse jaoks Azure Eclipse loomine]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Azure'i tööriistakomplekt Eclipse installimine]: ../azure-toolkit-for-eclipse-installation.md
[Azure'i tööriistakomplekt huvitav installimisega]: ../azure-toolkit-for-intellij-installation.md
[Mis on uut rakenduses Azure tööriistakomplekt Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Mis on uut rakenduses Azure tööriistakomplekt huvitav jaoks]: ../azure-toolkit-for-intellij-whats-new.md

[Azure'i Java Arenduskeskus]: https://azure.microsoft.com/develop/java/
[Web Appsi ülevaade]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-No-SDK-Specified.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
