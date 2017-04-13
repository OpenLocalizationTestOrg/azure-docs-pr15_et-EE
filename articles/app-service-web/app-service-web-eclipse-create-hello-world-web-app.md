<properties 
    pageTitle="Tere tulemast maailma veebirakenduse loomine Azure Eclipse | Microsoft Azure'i" 
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
    ms.date="08/26/2016" 
    ms.author="robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-eclipse"></a>Tere tulemast maailma veebirakenduse jaoks Azure Eclipse loomine

Selle õpetuse näitab, kuidas luua ja juurutada Tere, maailm taotluse Azure Web App abil [Azure'i tööriistakomplekt Eclipse]. Tavalise JSP näide on esitatud lihtne, kuid väga sarnane juhiseid oleks vastav Java servlet, kuna Azure juurutamise on.

Kui olete täitnud selles õpetuses, rakenduse sarnanema järgmisega järgmisel joonisel kuvatava veebibrauseris:

![Tere tulemast eelvaate maailma rakendus][01]
 
## <a name="prerequisites"></a>Eeltingimused

* Mõne Java arendaja Kit (JDK), v 1,8 või uuem versioon.
* Eclipse IDE Luna Java EE arendajatele või uuem versioon. See saab alla laadida <http://www.eclipse.org/downloads/>.
* Java-põhine veebiserverisse või rakenduse server, nt Apache Tomcat või Jetty jaotuse.
* Azure'i tellimuse, <https://azure.microsoft.com/free/> või <http://azure.microsoft.com/pricing/purchase-options/>omandanud.
* Azure'i tööriistakomplekt Eclipse. Lisateabe saamiseks leiate [Azure'i tööriistakomplekt Eclipse installimist].

## <a name="to-create-a-hello-world-application"></a>Tere tulemast maailma rakenduse loomine

Kõigepealt tuleb Alustame Java projekti loomine.

1. Käivitage Eclipse, ja klõpsake menüü **fail**, nuppu **Uus**ja klõpsake **Dünaamiline Web projekti**. (Kui te ei näe **Dünaamiline Web projekti** loendis Saadaolevad projekti nupu **faili** ja **Uus**, siis tehke järgmist: klõpsake menüüd **fail**, nuppu **Uus**, valige **Project...**, laiendage **Web**, valige **Dünaamiline Web Project**ja klõpsake nuppu **edasi**.)

1. Selle õpetuse huvides nime projekti **MyWebApp**. Ekraanil kuvatakse järgmine:

    ![Dünaamiliste Web uue projekti loomine][02]

1. Klõpsake nuppu **valmis**.

1. Eclipse's Project Exploreri vaates, laiendage **MyWebApp**. Paremklõpsake **WebContent**, klõpsake nuppu **Uus**ja valige **JSP fail**.

1. Dialoogiboksis **Uue JSP faili** nimi faili **index.jsp**hoida emakausta **MyWebApp/WebContent**nimega ja seejärel klõpsake nuppu **edasi**.

1. Eesmärgil selles õpetuses **Uue JSP faili (html)**Valige dialoogiboksis **Valige JSP malli** ja klõpsake siis nuppu **valmis**.

1. Index.jsp faili avamisel Eclipse lisamine dünaamiliselt kuvatav tekst **Tere, maailm!** jäävad `<body>` element. Teie värskendatud `<body>` sisu peaks sarnaneb järgmises näites:

    `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Salvestage index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Azure'i Web Appi Container rakenduse juurutamiseks

On mitu võimalust, mille saate juurutada Azure Java veebirakenduse. Selles õppetükis kirjeldatakse üks kõige lihtsam: rakenduse juurutatakse abil Azure Web Appi Container – pole teisiti projekti tüüp ega täiendavad tööriistad on vaja. Funktsiooni JDK ja tarkvara web container pakutav teile Azure, seega ei ole vaja üles laadida oma; teil on vaja oma Java Web App. Selle tulemusena võtab avaldamise protsessi rakenduse sekundit, mitte minutit.

1. Eclipse's Project Explorer, paremklõpsake **MyWebApp**.

1. Valige kontekstimenüüst valige **Azure**ja siis käsku **Avalda Azure Web u...**

    ![Avaldada nii Azure Web App][03]
   
    Teise võimalusena web rakenduse projekti valitud Project Exploreri saate klõpsake tööriistaribal nuppu **Avalda** rippmenüü ja sealt **Avalda Azure Web App** valige.
   
    ![Avaldada nii Azure Web App][14]
   
1. Kui te pole veel Azure Eclipse logitud, palutakse teil oma Azure'i kontosse:

    ![Azure sisselogimine dialoogiboks][04]
   
    Kui teil on mitu Azure kontod, mõned viipasid ajal sisselogimine protsess võib kasutada rohkem kui üks kord, isegi juhul, kui need olevat sama. Kui see juhtub, jätkake pärast sisselogimist juhiseid.

1. Pärast seda, kui olete edukalt Azure'i kontosse sisse loginud, kuvatakse dialoogiboks **Tellimuste haldamine** loend tellimused, mis on seotud oma kasutajanimi ja parool. Kui määratud on loetletud mitu tellimust ja te soovite töötada vaid teatud osa neist, võib-olla tühjendage ruut võite need, mida soovite kasutada. Kui olete valinud tellimuste, klõpsake nuppu **Sule**.

    ![Dialoogiboks tellimuste haldamine][05]
   
1. Kui kuvatakse dialoogiboks **Deploy Azure Web Appi Container abil** , kuvatakse mis tahes Web Appi ümbriste, mida olete varem loonud; kui te pole loonud mis tahes ümbriste, loend on tühi.

    ![Azure'i Web Appi Container dialoogiboksis juurutamine][06]
   
1. Kui te pole loonud Azure Web Appi Container enne või kui soovite avaldada oma rakenduse uus ümbris, siis tehke järgmist. Muul juhul valige mõne olemasoleva Web Appi ümbris ja jätkake juhisega 7 all.

    1. Klõpsake **Uus...**

        ![Azure'i Web Appi Container dialoogiboksis juurutamine][15]

    1. Kuvatakse dialoogiboks **Uue Web Appi Container** :

        ![Dialoogiboks uus Web Appi Container][07a]

    1. Sisestage **DNS-i silt** Web Appi Container; lehe DNS-i silt oma veebirakenduse URL-i hosti moodustavad Azure. (Tähele, et nimi peab kättesaadav ja vasta Azure'i veebirakenduse nime andmise nõuded.)

    1. Valige rippmenüüst **Web Container** vastav tarkvara rakenduse.

        Praegu saate valida Tomcat 8 Tomcat 7 või Jetty 9. Viimaste jaotumine valitud tarkvara pakutav Azure ja see töötab tehtud jaotuse JDK 8 loodud Oracle'i ja Azure järgi.

    1. Valige rippmenüü **tellimus** see tellimus, mida soovite kasutada seda juurutamiseks.

    1. Valige rippmenüüst **Ressursirühm** ressursirühm, kellega soovite oma veebirakenduse seostada. (Azure'i ressursi rühmad võimaldavad nii, et näiteks nende kustutamist koos rühmitada seotud ressursid.)

        Saate valida olemasoleva ressursirühm (kui teil on mõni) ja jäta samm g all või kasutage neid juhiseid uue ressursirühma loomiseks järgmist:

        * Klõpsake **Uus...**

        * Dialoogiboksi **Uue ressursirühma** kuvatakse:

            ![Uue ressursirühma dialoogiboks][08]

        * Klõpsake soovitud **nimi** rühmitusaluse määramiseks nimi uue ressursirühma.

        * Klõpsake soovitud **regioon** rippmenüüst menüü, valige sobiv Azure'i andmed keskele ressursirühma asukoht.

        * Valikuline: Vaikimisi tehtud jaotuse Java 8 juurutatakse, Azure'i automaatselt abil oma web appi container nimega oma töötab. Siiski saate määrata muu versioon ja selle töötab jaotuse kui teie Web Appis nõuab. Määrake soovitud JDK veebirakenduse jaoks, klõpsake vahekaarti **JDK** ja valige üks järgmistest suvanditest:

            * **Vaikimisi pakutud Azure'i veebirakenduste teenuse JDK Deploy**: see suvand on tehtud jaotuse Java 8 juurutamine.

            * **Mõne 3. osapoole JDK saadaval Azure Deploy**: see suvand võimaldab teil valida loendist JDKs, mida pakub Microsoft Azure'i.

            * **Deploy oma JDK allalaadimine järgmisest asukohast**: see suvand, saate määrata oma JDK jaotuse, mis peavad olema pakitud ZIP-failina ja üles laadida avalikult kättesaadavaks allalaadimiskoht või Azure storage konto, millele teil on juurdepääs.

            ![Dialoogiboks uus Web Appi Container][07b]

    * Klõpsake nuppu **OK**.

    1. Rippmenüü **Rakenduse teenuse leping** on loetletud rakenduse teenuse lepingud, mis on seotud teie valitud ressursirühma. (Rakenduse teenuse lepingute määrata teavet, näiteks asukoht oma veebirakenduse, hinnakirjad taseme ja Arvuta eksemplari suurust. Ühe rakenduse teenuse leping saab kasutada mitut veebirakendusi, mis on põhjus, miks see säilib eraldi kindla veebirakenduse juurutamise.)

        Saate valida mõne olemasoleva rakenduse teenuse kavandamine (kui teil on mõni) ja jäta samm h allpool või kasutage neid juhiseid luua uue rakenduse teenuse Plan järgmist:

        * Klõpsake **Uus...**

        * Kuvatakse dialoogiboks **Uue rakenduse teenuse kavandamine** :

            ![Uus dialoogiboks rakenduse teenuse kavandamine][09]

        * Klõpsake soovitud **nimi** rühmitusaluse määramiseks nimi oma uue rakenduse teenuse kavandamine.

        * Rakenduses soovitud **asukohta** rippmenüüst menüü, valige sobiv Azure'i andmed keskele leping asukoht.

        * Klõpsake soovitud **Hinnad taseme** rippmenüü, valige sobiv hinnad lepingu raames. Katsetamiseks saate **tasuta**.

        * Klõpsake soovitud funktsiooni **Eksemplari suurus** rippmenüü, valige sobiv eksemplari suuruse lepingu raames. Katsetamiseks saate valida **Small**.

    1. Kui olete täitnud kõiki ülaltoodud juhiseid, näeb dialoogiboksi Uus Web Appi ümbris järgmisel joonisel:

        ![Dialoogiboks uus Web Appi Container][10]

    1. Klõpsake nuppu **OK** , et viia lõpule oma uue veebirakenduse container.

        Oodake paar sekundit veebirakenduse ümbriste loendi värskendamist ja oma vastloodud web appi container peaks olema loendist valitud.

1. Nüüd olete valmis oma veebirakenduse Azure esialgse kasutuselevõtu:

    ![Azure'i Web Appi Container dialoogiboksis juurutamine][11]

    Klõpsake nuppu **OK** , et teie valitud veebirakenduse container Java rakenduse juurutamine.

    Vaikimisi on rakenduse juurutatud alamkausta serveri nimega. Kui soovite, et see rakendus juurkausta kasutusele võtta, märkige ruut **Deploy root** enne nupu **OK klõpsamist**.

1. Järgmiseks peaksite **Azure'i tegevuse Log** vaade, mis näitab oma veebirakenduse juurutamise olekut.

    ![Azure'i tegevuse Log][12]

    Protsessi juurutamine oma veebirakenduse Azure peaks tegema, vaid mõne hetke. Kui teie taotlus valmis, näete linki nimega **avaldatud** veerus **olek** . Kui klõpsate linki, viib teid avalehele oma juurutatud veebirakenduse.

## <a name="updating-your-web-app"></a>Oma veebirakenduse värskendamine

Värskendamine olemasoleva töötab Azure Web App on kiire ja lihtne protsess ja on kaks võimalust värskendamine:

* Juurutamise olemasoleva Java Web App saate värskendada.
* Java taotlus sama ümbris Web Appi abil saate avaldada.

Mõlemal juhul protsess on identne ja võtab aega ainult mõne sekundi:

1. Eclipse project explorer, paremklõpsake soovitud Java rakendus, mida soovite värskendada või lisada mõne olemasoleva Web Appi ümbrises.

1. Kontekstimenüü kuvamisel valige **Azure** ja seejärel **Avalda Azure Web u...**

1. Kuna teil on juba sisse logitud varem, kuvatakse teie olemasoleva veebirakenduse ümbriste loendit. Valige see, mille soovite avaldada või uuesti avaldada oma Java taotlus ja klõpsake nuppu **OK**.

Mõne hetke hiljem **Azure'i tegevuse Log** vaates kuvatakse kujul **avaldatud** värskendatud juurutamise ja saab kontrollida värskendatud rakenduse veebibrauseris.

## <a name="stopping-an-existing-web-app"></a>Olemasoleva Web App peatamine

Lõpetamiseks mõne olemasoleva Azure Web Appi container, (sh juurutatud Java rakendusi ei), saate kasutada **Azure Exploreri** vaade.

Kui **Azure Exploreri** vaade on juba avatud, saate selle avamiseks klõpsata seejärel **akna** menüü Eclipse, ja seejärel klõpsake nuppu **Kuva vaade**, seejärel **muud...**, siis **Azure**ja klõpsake **Azure Explorer**. Kui te ei ole varem sisse loginud, palutakse teil seda teha.

**Azure'i Exploreri** vaade kuvamisel kasutamine tehke järgmist peatamiseks oma veebirakenduse. 

1. Laiendage **Azure** .

1. Laiendage **Web Apps** . 

1. Paremklõpsake soovitud Web App.

1. Kontekstimenüü kuvamisel klõpsake nuppu **Lõpeta**.

    ![Olemasoleva Web App peatamine][13]

## <a name="next-steps"></a>Järgmised sammud

Vaadake lisateavet Azure tööriistakomplektid Java interaktiivset järgmisi linke:

- [Azure'i tööriistakomplekt Eclipse]
  - [Azure'i tööriistakomplekt Eclipse installimine]
  - *Tere tulemast maailma veebirakenduse loomine Azure Eclipse (selles artiklis)*
  - [Mis on uut rakenduses Azure tööriistakomplekt Eclipse]
- [Azure'i tööriistakomplekt huvitav jaoks]
  - [Azure'i tööriistakomplekt huvitav installimisega]
  - [Tere tulemast maailma veebirakenduse jaoks huvitav Azure loomine]
  - [Mis on uut rakenduses Azure tööriistakomplekt huvitav jaoks]

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus].

Azure'i veebirakenduste loomise kohta lisateabe saamiseks vt [Web Appsi ülevaade].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure'i tööriistakomplekt Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure'i tööriistakomplekt huvitav jaoks]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Tere tulemast maailma veebirakenduse jaoks huvitav Azure loomine]: ./app-service-web-intellij-create-hello-world-web-app.md
[Azure'i tööriistakomplekt Eclipse installimine]: ../azure-toolkit-for-eclipse-installation.md
[Azure'i tööriistakomplekt huvitav installimisega]: ../azure-toolkit-for-intellij-installation.md
[Mis on uut rakenduses Azure tööriistakomplekt Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Mis on uut rakenduses Azure tööriistakomplekt huvitav jaoks]: ../azure-toolkit-for-intellij-whats-new.md

[Azure'i Java Arenduskeskus]: https://azure.microsoft.com/develop/java/
[Web Appsi ülevaade]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/app-service-web-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/app-service-web-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/app-service-web-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/app-service-web-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/app-service-web-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/app-service-web-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/app-service-web-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/app-service-web-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/app-service-web-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/app-service-web-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/app-service-web-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/app-service-web-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/app-service-web-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/app-service-web-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
