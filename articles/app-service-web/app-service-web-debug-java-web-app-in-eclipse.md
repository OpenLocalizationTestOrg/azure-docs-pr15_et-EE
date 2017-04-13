<properties 
    pageTitle="Java Web App on Eclipse Azure silumine | Microsoft Azure'i" 
    description="Selle õpetuse näidatakse, kuidas kasutada Azure tööriistakomplekt Eclipse silumine Java Web Appi töötavate Azure." 
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

# <a name="debug-a-java-web-app-on-azure-in-eclipse"></a>Java Web App on Eclipse Azure silumine

Selle õpetuse kujutatakse silumine Java Web App töötab Azure'i [Azure tööriistakomplekt Eclipse]abil. Lihtne, huvides kasutate selle õpetuse tavalise Java Server lehe (JSP) näide, kuid juhiseid oleks sarnane Java servlet, kui teil on silumine Azure.

Kui olete täitnud selles õpetuses, rakenduse prindituna välja näeb sarnaneb järgmisel joonisel on teil selle Eclipse silumine:

![][01]
 
## <a name="prerequisites"></a>Eeltingimused

* Mõne Java arendaja Kit (JDK), v 1,8 või uuem versioon.
* Eclipse IDE Java EE arendajatele Indigo või uuem versioon. See saab alla laadida <http://www.eclipse.org/downloads/>.
* Java-põhine veebiserverisse või rakenduse server, nt Apache Tomcat või Jetty jaotuse.
* Azure'i tellimuse, <https://azure.microsoft.com/en-us/free/> või <http://azure.microsoft.com/pricing/purchase-options/>omandanud.
* Azure'i tööriistakomplekt Eclipse. Lisateabe saamiseks leiate [Azure'i tööriistakomplekt Eclipse installimist].
* Dünaamiliste Web projekti loodud ja juurutatud Azure'i rakendust Service; näiteks vt [Tere maailma veebirakenduse jaoks Azure Eclipse loomine].

## <a name="to-debug-a-java-web-app-on-azure"></a>Kui soovite Java Web Appi Azure silumine

Selles jaotises nende juhiste abil saate olemasoleva dünaamilise Web projekti, mille olete juba juurutanud Java Web Appi Azure, saate alla laadida [Valimi dünaamiline Web projekti] ja järgige juhiseid [loomine Tere maailma veebirakenduse jaoks Azure Eclipse] juurutada selle Azure. 

1. Avage Eclipse.

1. Konfigureerige ajalõpp Kaug silumine.

    1. Klõpsake **Windowsi** menüü Eclipse ja seejärel klõpsake käsku **eelistused**.
    1. Laiendage **Java** ja seejärel valige **silumine**.
    1. Nii **siluri ajalõpp (ms)** kui ka **käivitamine ajalõpp (ms)** sätteid konfigureerida `120000`.

        ![][02]

    1. Klõpsake nuppu **OK** , et sulgeda dialoogiboks **eelistused** .

1. Eclipse's Project Exploreri vaade, paremklõpsake dünaamilise Web projekti, mis olete juurutanud Azure. Kontekstimenüü kuvamisel valige **Silumine**, ja klõpsake **Azure Web App**.

    ![][03]

1. Kui teil on silumine dünaamiline Web projekti esimest korda, avatakse dialoogiboks **Silumine konfiguratsioone** ; Võite nõustuda vaikeväärtused, mis on määratud tööriistakomplekt vahekaardil **ühendus** . Vahekaardi **Allikas** nuppu **Lisa**ja seejärel **Java projekti**, valige **Dünaamiline Web projekt**ja seejärel klõpsake nuppu **OK**. Kui need juhised on lõpule jõudnud, klõpsake **silumine**.

    ![][04]

1. Kui teil palutakse **lubamine Kaug silumine remote Web Appis nüüd?**, klõpsake nuppu **OK**.

1. Kui kuvatakse vastav viip, et **oma veebirakenduse on nüüd valmis Kaug silumine**, klõpsake nuppu **OK**.

    ![][05]

1. Kui kuvatakse dialoogiboks **Silumine konfiguratsioone** , klõpsake **silumine**.

1. Unix shell või Windowsi Käsuviip avamine ja valmistada vajalikud ühenduse silumine; peate ootama, kuni ühendust oma remote Java veebirakenduse õnnestub enne jätkamist. Kui kasutate opsüsteemi Windows, seda näevad välja nagu järgmisel joonisel.

    ![][06]

1. Lisada leheküljepiiri punkti JSP lehel kuvatud, siis avage URL brauseri Java veebirakenduse jaoks:

    1. Avage Eclipse **Azure'i Explorer** .
    1. Liikuge **Veebirakenduste** ja soovite silumine Java Web App.
    1. Paremklõpsake Web App ja klõpsake nuppu **Ava brauseris**.
    1. Nüüd alustab Eclipse silumine režiim.

## <a name="next-steps"></a>Järgmised sammud

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus].

Azure'i veebirakenduste loomise kohta lisateabe saamiseks vt [Web Appsi ülevaade].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure'i tööriistakomplekt Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure'i tööriistakomplekt Eclipse installimine]: ../azure-toolkit-for-eclipse-installation.md
[Tere tulemast maailma veebirakenduse jaoks Azure Eclipse loomine]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Proovi dünaamilise projekti]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure'i Java Arenduskeskus]: https://azure.microsoft.com/develop/java/
[Web Appsi ülevaade]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-eclipse/01-debug-java-web-app-in-eclipse.png
[02]: ./media/app-service-web-debug-java-web-app-in-eclipse/02-configure-eclipse-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-eclipse/03-debug-as.png
[04]: ./media/app-service-web-debug-java-web-app-in-eclipse/04-debug-configurations.png
[05]: ./media/app-service-web-debug-java-web-app-in-eclipse/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-eclipse/06-windows-command-prompt-connection-successful-to-remote.png
