<properties 
    pageTitle="Tere tulemast maailma veebirakenduse jaoks Azure Eclipse loomine" 
    description="Selle õpetuse näidatakse, kuidas luua Tere maailma veebirakenduse Azure Azure'i tööriistakomplekt Eclipse abil." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-eclipse"></a>Tere tulemast maailma veebirakenduse jaoks Azure Eclipse loomine

Selle õpetuse näitab, kuidas luua ja juurutada Tere, maailm taotluse Azure Web App abil [Azure'i tööriistakomplekt Eclipse]. Tavalise JSP näide on esitatud lihtne, kuid väga sarnane juhiseid oleks vastav Java servlet, kuna Azure juurutamise on.

Kui olete täitnud selles õpetuses, rakenduse sarnanema järgmisega järgmisel joonisel kuvatava veebibrauseris:

![][01]
 
## <a name="prerequisites"></a>Eeltingimused

* Mõne Java arendaja Kit (JDK), v 1.7 või uuem versioon.
* Eclipse IDE Java EE arendajatele Indigo või uuem versioon. See saab alla laadida <http://www.eclipse.org/downloads/>.
* Java-põhine veebiserverisse või rakenduse server, nt Apache Tomcat või Jetty jaotuse.
* Azure'i tellimuse, <https://azure.microsoft.com/en-us/free/> või <http://azure.microsoft.com/pricing/purchase-options/>omandanud.
* Azure'i tööriistakomplekt Eclipse. Lisateabe saamiseks leiate [Azure'i tööriistakomplekt Eclipse installimist].

## <a name="to-create-a-hello-world-application"></a>Tere tulemast maailma rakenduse loomine

Kõigepealt tuleb Alustame Java projekti loomine.

1. Käivitage Eclipse, ja klõpsake menüü **fail**, nuppu **Uus**ja klõpsake **Dünaamiline Web projekti**. (Kui te ei näe **Dünaamiline Web projekti** loendis Saadaolevad projekti nupu **faili** ja **Uus**, siis tehke järgmist: klõpsake menüüd **fail**, nuppu **Uus**, valige **Project...**, laiendage **Web**, valige **Dünaamiline Web Project**ja klõpsake nuppu **edasi**.)

1. Selle õpetuse huvides nime projekti **MyHelloWorld**. Ekraanil kuvatakse järgmine:

    ![][02]

1. Klõpsake nuppu **valmis**.

1. Eclipse's Project Exploreri vaates, laiendage **MyHelloWorld**. Paremklõpsake **WebContent**, klõpsake nuppu **Uus**ja valige **JSP fail**.

1. Dialoogiboksis **Uue JSP faili** nimi faili **index.jsp**. Hoida emakausta **MyHelloWorld/WebContent**.

1. Eesmärgil selles õpetuses **Uue JSP faili (html)**Valige dialoogiboksis **Valige JSP malli** ja klõpsake siis nuppu **valmis**.

1. Index.jsp faili avamisel Eclipse lisamine dünaamiliselt kuvatav tekst **Tere, maailm!** jäävad `<body>` element. Teie värskendatud `<body>` sisu peaks sarnaneb järgmises näites:

    `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Salvestage index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Azure'i Web Appi Container rakenduse juurutamiseks

On mitu võimalust, mille saate juurutada Azure Java veebirakenduse. Selles õppetükis kirjeldatakse üks kõige lihtsam: rakenduse juurutatakse abil Azure Web Appi Container – pole teisiti projekti tüüp ega täiendavad tööriistad on vaja. Funktsiooni JDK ja tarkvara web container pakutav teile Azure, seega ei ole vaja üles laadida oma; teil on vaja oma Java Web App. Selle tulemusena võtab avaldamise protsessi rakenduse sekundit, mitte minutit.

1. Eclipse's Project Explorer, paremklõpsake **MyHelloWorld**.

1. Valige kontekstimenüüst valige **Azure**ja siis käsku **Avalda Azure Web u...**

    ![][03]
   
    Teise võimalusena web rakenduse projekti valitud Project Exploreri saate klõpsake tööriistaribal nuppu **Avalda** rippmenüü ja sealt **Avalda Azure Web App** valige.
   
    ![][14]
   
1. Kui te pole veel Azure Eclipse logitud, palutakse teil oma Azure'i kontosse:

    ![][04]
   
    Märkus: Kui teil on mitu Azure kontod, mõned viipasid ajal sisselogimine protsess võib kasutada rohkem kui üks kord, isegi juhul, kui need olevat sama. Kui see juhtub, jätkake pärast sisselogimist juhiseid.
1. Pärast seda, kui olete edukalt Azure'i kontosse sisse loginud, kuvatakse dialoogiboks **Tellimuste haldamine** loend tellimused, mis on seotud oma kasutajanimi ja parool. Kui määratud on loetletud mitu tellimust ja te soovite töötada vaid teatud osa neist, võib-olla tühjendage ruut võite need, mida soovite kasutada. Kui olete valinud tellimuste, klõpsake nuppu **Sule**.

    ![][05]
   
1. Kui kuvatakse dialoogiboks **Deploy Azure Web Appi Container abil** , kuvatakse mis tahes Web Appi ümbriste, mida olete varem loonud; kui te pole loonud mis tahes ümbriste, loend on tühi.

    ![][06]
   
1. Kui te pole loonud Azure Web Appi Container enne või kui soovite avaldada oma rakenduse uus ümbris, siis tehke järgmist. Muul juhul valige mõne olemasoleva Web Appi ümbris ja jätkake juhisega 7 all.

    1. Klõpsake **Uus...**

    1. Kuvatakse dialoogiboks **Uue Web Appi Container** :

        ![][07]

    1. Sisestage **DNS-i silt** Web Appi Container; lehe DNS-i silt oma veebirakenduse URL-i hosti moodustavad Azure. Märkus: Nimi peab olema saadaval ja Azure Web Appi nime andmise nõuded vasta.

    1. Valige rippmenüüst **Web Container** vastav tarkvara rakenduse.

        Praegu saate valida Tomcat 8 Tomcat 7 või Jetty 9. Viimaste jaotumine valitud tarkvara pakutav Azure ja see töötab tehtud jaotuse JDK 8 loodud Oracle'i ja Azure järgi.

    1. Valige rippmenüü **tellimus** see tellimus, mida soovite kasutada seda juurutamiseks.

    1. Valige rippmenüüst **Ressursirühm** ressursirühm, kellega soovite oma veebirakenduse seostada.

        Märkus: Azure'i ressursi rühmade abil saate omavahel seotud ressursid rühmitada nii, et näiteks nende kustutamist koos.

        Saate valida olemasoleva ressursirühm (kui teil on mõni) ja jäta samm g all või kasutage neid juhiseid uue ressursirühma loomiseks järgmist:

        * Klõpsake **Uus...**

        * Dialoogiboksi **Uue ressursirühma** kuvatakse:

            ![][08]

        * Klõpsake soovitud **nimi** rühmitusaluse määramiseks nimi uue ressursirühma.

        * Klõpsake soovitud **regioon** rippmenüüst menüü, valige sobiv Azure'i andmed keskele ressursirühma asukoht.

        * Klõpsake nuppu **OK**.

    1. Rippmenüü **Rakenduse teenuse leping** on loetletud rakenduse teenuse lepingud, mis on seotud teie valitud ressursirühma.

        Märkus: Rakenduse teenuse leping määrab teabe nt asukoht oma veebirakenduse, hinnakirjad taseme ja Arvuta eksemplari suurus. Ühe rakenduse teenuse leping saab kasutada mitut veebirakendusi, mis on põhjus, miks see säilib eraldi kindla veebirakenduse juurutamise.

        Saate valida mõne olemasoleva rakenduse teenuse kavandamine (kui teil on mõni) ja jäta samm h allpool või kasutage neid juhiseid luua uue rakenduse teenuse Plan järgmist:

        * Klõpsake **Uus...**

        * Kuvatakse dialoogiboks **Uue rakenduse teenuse kavandamine** :

            ![][09]

        * Klõpsake soovitud **nimi** rühmitusaluse määramiseks nimi oma uue rakenduse teenuse kavandamine.

        * Rakenduses soovitud **asukohta** rippmenüüst menüü, valige sobiv Azure'i andmed keskele leping asukoht.

        * Klõpsake soovitud **Hinnad taseme** rippmenüü, valige sobiv hinnad lepingu raames. Katsetamiseks saate **tasuta**.

        * Klõpsake soovitud funktsiooni **Eksemplari suurus** rippmenüü, valige sobiv eksemplari suuruse lepingu raames. Katsetamiseks saate valida **Small**.

    1. Kui olete täitnud kõiki ülaltoodud juhiseid, näeb dialoogiboksi Uus Web Appi ümbris järgmisel joonisel:

        ![][10]

    1. Klõpsake nuppu **OK** , et viia lõpule oma uue veebirakenduse container.

        Oodake paar sekundit veebirakenduse ümbriste loendi värskendamist ja oma vastloodud web appi container peaks olema loendist valitud.

1. Nüüd olete valmis oma veebirakenduse Azure esialgse kasutuselevõtu:

    ![][11]

    Klõpsake nuppu **OK** , et teie valitud veebirakenduse container Java rakenduse juurutamine.

    Märkus: Juurutatakse nimega alamkausta serveri vaikimisi rakenduse. Kui soovite, et see rakendus juurkausta kasutusele võtta, märkige ruut **Deploy root** enne nupu **OK klõpsamist**.

1. Järgmiseks peaksite **Azure'i tegevuse Log** vaade, mis näitab oma veebirakenduse juurutamise olekut.

    ![][12]

    Protsessi juurutamine oma veebirakenduse Azure peaks tegema, vaid mõne hetke. Kui teie taotlus valmis, näete linki nimega **avaldatud** veerus **olek** . Kui klõpsate linki, viib teid avalehele oma juurutatud veebirakenduse.

## <a name="updating-your-web-app"></a>Oma veebirakenduse värskendamine

Värskendamine olemasoleva töötab Azure Web App on kiire ja lihtne protsess ja on kaks võimalust värskendamine:

* Juurutamise olemasoleva Java Web App saate värskendada.
* Java taotlus sama ümbris Web Appi abil saate avaldada.

Mõlemal juhul protsess on identne ja võtab aega ainult mõne sekundi:

1. Eclipse project explorer, paremklõpsake soovitud Java rakendus, mida soovite värskendada või lisada mõne olemasoleva Web Appi ümbrises.

2. Kontekstimenüü kuvamisel valige **Azure** ja seejärel **Avalda Azure Web u...**

3. Kuna teil on juba sisse logitud varem, kuvatakse teie olemasoleva veebirakenduse ümbriste loendit. Valige see, mille soovite avaldada või uuesti avaldada oma Java taotlus ja klõpsake nuppu **OK**.

Mõne hetke hiljem **Azure'i tegevuse Log** vaates kuvatakse kujul **avaldatud** värskendatud juurutamise ja saab kontrollida värskendatud rakenduse veebibrauseris.

## <a name="stopping-an-existing-web-app"></a>Olemasoleva Web App peatamine

Lõpetamiseks mõne olemasoleva Azure Web Appi container, (sh juurutatud Java rakendusi ei), saate kasutada **Azure Exploreri** vaade.

Kui **Azure Exploreri** vaade on juba avatud, saate selle avamiseks klõpsata seejärel **akna** menüü Eclipse, ja seejärel klõpsake nuppu **Kuva vaade**, seejärel **muud...**, siis **Azure**ja klõpsake **Azure Explorer**. Kui te ei ole varem sisse loginud, palutakse teil seda teha.

**Azure'i Exploreri** vaade kuvamisel kasutamine tehke järgmist peatamiseks oma veebirakenduse. 

1. Laiendage **Azure** .

1. Laiendage **Web Apps** . 

1. Paremklõpsake soovitud Web App.

1. Kontekstimenüü kuvamisel klõpsake nuppu **Lõpeta**.

    ![][13]

## <a name="next-steps"></a>Järgmised sammud

Lisateabe saamiseks vaadake järgmisi linke:

* [Java Arenduskeskus](/develop/java/).
* [Web Appsi ülevaade](app-service-web-overview.md)

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure'i tööriistakomplekt Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure'i tööriistakomplekt Eclipse installimine]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[01]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/01-Web-Page.png
[02]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/02-Dynamic-Web-Project.png
[03]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/03-Context-Menu.png
[04]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/04-Log-In-Dialog.png
[05]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/05-Manage-Subscriptions-Dialog.png
[06]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/06-Deploy-To-Azure-Web-Container.png
[07]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/07-New-Web-App-Container-Dialog.png
[08]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/08-New-Resource-Group-Dialog.png
[09]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/09-New-Service-Plan-Dialog.png
[10]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/11-Completed-Deploy-Dialog.png
[12]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/12-Activity-Log-View.png
[13]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/13-Azure-Explorer-Web-App.png
[14]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/publishDropdownButton.png