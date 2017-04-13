<properties 
    pageTitle="Java Web Appiga Azure huvitav silumine | Microsoft Azure'i" 
    description="Selle õpetuse näidatakse, kuidas kasutada Azure tööriistakomplekt jaoks huvitav silumine Java Web Appi töötavate Azure." 
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
    ms.date="09/20/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="debug-a-java-web-app-on-azure-in-intellij"></a>Java Web Appiga Azure huvitav silumine

Selle õpetuse kujutatakse silumine Java Web App töötab Azure'i [Azure tööriistakomplekt huvitav]abil. Lihtne, huvides kasutate selle õpetuse tavalise Java Server lehe (JSP) näide, kuid juhiseid oleks sarnane Java servlet, kui teil on silumine Azure.

Kui olete täitnud selles õpetuses, rakenduse prindituna välja näeb sarnaneb järgmisel joonisel on teil selle huvitav silumine:

![][01]
 
## <a name="prerequisites"></a>Eeltingimused

* Mõne Java arendaja Kit (JDK), v 1,8 või uuem versioon.
* Huvitav idee Ultimate Edition. See saab alla laadida <https://www.jetbrains.com/idea/download/index.html>.
* Java-põhine veebiserverisse või rakenduse server, nt Apache Tomcat või Jetty jaotamine.
* Azure'i tellimuse, <https://azure.microsoft.com/en-us/free/> või <http://azure.microsoft.com/pricing/purchase-options/>omandanud.
* Azure'i tööriistakomplekt huvitav jaoks. Lisateabe saamiseks lugege teemat [installimist Azure tööriistakomplekt huvitav jaoks].
* Dünaamiliste Web projekti loodud ja juurutatud Azure'i rakendust Service; näiteks vt [Tere maailma veebirakenduse jaoks huvitav Azure loomine].

## <a name="to-debug-a-java-web-app-on-azure"></a>Kui soovite Java Web Appi Azure silumine

Selles jaotises nende juhiste abil saate olemasoleva dünaamilise Web projekti, mille olete juba juurutanud Java Web Appi Azure, saate alla laadida [Valimi dünaamiline Web projekti] ja järgige juhiseid [loomine Tere maailma veebirakenduse jaoks huvitav Azure] juurutada selle Azure. 

1. Java veebirakenduse projekti, mis teil juurutatud Azure huvitav avada.

1. Klõpsake menüüd **Käivita** ja seejärel klõpsake nuppu **Redigeeri konfiguratsioone**.

    ![][02]

1. Kui avab dialoogiboksi **Käivita/silumine konfiguratsioone** : 

    1. Valige **Azure Web App**.
    1. Klõpsake **+** uue konfiguratsiooni lisamiseks.
    1. Sisestage **nimi** konfiguratsiooni.
    1. Aktsepteerige ülejäänud vaikeväärtused, mis on pakutud Azure'i tööriistakomplekt ja seejärel klõpsake nuppu **OK**.

        ![][03]

1. Valige Azure Web Appi silumine konfiguratsioon vastloodud menüü paremas ülaosas ja klõpsake **silumine**

    ![][04]

1. Kui teil palutakse **lubamine Kaug silumine remote Web Appis nüüd?**, klõpsake nuppu **OK**.

1. Kui kuvatakse vastav viip, et **oma veebirakenduse on nüüd valmis Kaug silumine**, klõpsake nuppu **OK**.

    ![][05]

1. Valige Azure Web Appi silumine konfiguratsiooni vastloodud menüü paremas ülaosas ja seejärel klõpsake **silumine**.

1. Unix shell või Windowsi Käsuviip avamine ja valmistada vajalikud ühenduse silumine; peate ootama, kuni ühendust oma remote Java veebirakenduse õnnestub enne jätkamist. Kui kasutate opsüsteemi Windows, näeb järgmisel joonisel:

    ![][06]

1. Lisada leheküljepiiri punkti JSP lehel kuvatud, siis avage URL brauseri Java veebirakenduse jaoks:

    1. Avage **Azure'i Exploreri** huvitav.
    1. Liikuge **Veebirakenduste** ja soovite silumine Java Web App.
    1. Paremklõpsake Web App ja klõpsake nuppu **Ava brauseris**.
    1. Nüüd alustab huvitav silumine režiim.

## <a name="next-steps"></a>Järgmised sammud

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus].

Azure'i veebirakenduste loomise kohta lisateabe saamiseks vt [Web Appsi ülevaade].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure'i tööriistakomplekt huvitav jaoks]: ../azure-toolkit-for-intellij.md
[Azure'i tööriistakomplekt huvitav installimisega]: ../azure-toolkit-for-intellij-installation.md
[Tere tulemast maailma veebirakenduse jaoks huvitav Azure loomine]: ./app-service-web-intellij-create-hello-world-web-app.md
[Proovi dünaamilise projekti]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure'i Java Arenduskeskus]: https://azure.microsoft.com/develop/java/
[Web Appsi ülevaade]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-intellij/01-debug-java-web-app-in-intellij.png
[02]: ./media/app-service-web-debug-java-web-app-in-intellij/02-configure-intellij-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-intellij/03-debug-configuration.png
[04]: ./media/app-service-web-debug-java-web-app-in-intellij/04-select-debug.png
[05]: ./media/app-service-web-debug-java-web-app-in-intellij/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-intellij/06-windows-command-prompt-connection-successful-to-remote.png
