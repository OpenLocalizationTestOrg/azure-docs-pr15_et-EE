<properties
    pageTitle="Azure'i teatis jaoturi ja Node.js Tõuketeatiste saatmine"
    description="Saate teada, kuidas Tõuketeatiste saatmine rakendusest Node.js teatis jaoturi abil."
    keywords="tõuketeatised teatis, tõuketeatised notifications,node.js tõuketeatised, iOS-i tõuketeatised"
    services="notification-hubs"
    documentationCenter="nodejs"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Azure'i teatis jaoturi ja Node.js Tõuketeatiste saatmine
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

##<a name="overview"></a>Ülevaade

> [AZURE.IMPORTANT] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).

Sellest juhendist näitab teile, kuidas saata otse Node.js taotluse Tõuketeatiste, Azure'i teatis jaoturi abil. 

Stsenaariumid, kaetud järgmised rakenduste järgmiste platvormide Tõuketeatiste saata.

* Android
* iOS-i
* Windows Phone
* Universaalne Windowsi platvormi 

Teatis jaoturi kohta leiate lisateavet jaotisest [Järgmised toimingud](#next) .

##<a name="what-are-notification-hubs"></a>Mis on teatis jaoturi?

Azure'i teatis jaoturi ette-kasutatav, mitme platvormi, scalable infrastruktuur saatmist Tõuketeatiste mobiilsideseadmete jaoks. Teenuse taristu kohta täpsema teabe saamiseks vt [Azure'i teatis jaoturi](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) .

##<a name="create-a-nodejs-application"></a>Node.js rakenduse loomine

Selles õpetuses esimene samm on luua uus tühi Node.js rakendus. Node.js rakenduste loomise juhised leiate teemast [loomine ja juurutamine Node.js rakenduse Azure veebisaidi][nodejswebsite], [Node.js pilveteenuses] [ Node.js Cloud Service] Windows PowerShelli või [veebisaidi WebMatrix].

##<a name="configure-your-application-to-use-notification-hubs"></a>Rakenduse kasutamiseks teatis jaoturi konfigureerimine

Azure'i teatis jaoturi kasutamiseks peate alla laadida ja Node.js [Azure'i paketi](https://www.npmjs.com/package/azure), mis sisaldab mõnda valmiskomplekti helper teekidest, mis suhelda tõuketeatised teatis ülejäänud teenuseid kasutada.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Saada pakett sõlm paketi Manager (NPM) abil

1.  Kasutage käsurea liides, nt **PowerShell** (Windows), **terminalis** (Mac) või **Bash** (Linux) ja liikuge kausta, kuhu olete loonud tühja rakenduse.

2.  Tippige käsuviiba aken **npm installida azure-sb** .

3.  Käsitsi käivitada veendumaks, et **ls** või **dir** käsk on **sõlm\_moodulid** loodud kausta. Selle kausta sisse leiate **Azure'i** paketi, mis sisaldab teekide teate jaoturi juurdepääsuks.

>[AZURE.NOTE] Lisateavet leiate teemast ametlik [NPM ajaveebi](http://blog.npmjs.org/post/85484771375/how-to-install-npm)NPM installimise kohta. 

### <a name="import-the-module"></a>Mooduli importimine

Kasutades tekstiredaktoris, lisage järgmine tekst **server.js** faili rakenduse ülaserva:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Mõne Azure'i teate jaoturi ühenduse häälestamine

Objekti **NotificationHubService** võimaldab töötada teatis jaoturi. Järgmine kood loob **NotificationHubService** objekti nimega **hubname**teavitamine jaoturi jaoks. Lisage see **server.js** faili ülaosas pärast lauset azure mooduli importida.

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

[Azure portaali] aadressil ühenduse **connectionstring** väärtuse, toimige järgmiselt:

1. Klõpsake vasakpoolsel navigeerimispaanil nuppu **Sirvi**.

2. Valige **Teatise jaoturi**, ja seejärel otsige jaoturi, mida soovite kasutada valimi. Kui vajate abi uus teatis keskuse loomine võib viidata [õpetuse Windowsi poe alustamine](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) .

3. Valige **sätted**.

4. Klõpsake **Juurdepääsupoliitikaid**. Kuvatakse nii ühis- ja täielik juurdepääs ühendusstringi.

![Azure'i portaal - jaoturi teatis](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [AZURE.NOTE] Saate kuulata ka [Azure PowerShelli](../powershell-install-configure.md) või käsk **Kuva azure sb nimeruum** [Azure'i käsurea liides (Azure'i CLI)](../xplat-cli-install.md)esitatud **Get-AzureSbNamespace** cmdlet-käsu abil ühendusstring.

##<a name="general-architecture"></a>Üldine arhitektuur

Objekti **NotificationHubService** seab objekti järgmistel juhtudel kindlate seadmete ja rakenduste Tõuketeatiste saatmiseks:

* **Android** - **GcmService** objekti, mis on saadaval veebisaidil **notificationHubService.gcm** kasutamine
* **iOS** - **ApnsService** objekti, mis asub aadressil **notificationHubService.apns** kasutamine
* **Windows Phone** - **MpnsService** objekti, mis on saadaval veebisaidil **notificationHubService.mpns** kasutamine
* **Universaalne Windowsi platvormi** - **WnsService** objekti, mis on saadaval veebisaidil **notificationHubService.wns** kasutamine

### <a name="how-to-send-push-notifications-to-android-applications"></a>Kuidas: Android rakendustele saada tõuketeatised

Objekti **GcmService** pakub **saata** meetod, mis saab saata Tõuketeatiste Android rakendused. **Saada** meetodit aktsepteerib järgmisi:

* **Siltide** - silt identifikaator. Kui pole sildi, saadetakse teate al klientidele.
* **Last** – sõnumi JSON või töötlemata stringi last.
* **Tagasihelistamise** - funktsiooni tagasihelistamise.

Last vormingu kohta leiate lisateavet jaotisest **last** [Rakendamise GCM Server](http://developer.android.com/google/gcm/server.html#payload) dokumendi.

Järgmine kood kasutab **GcmService** eksemplari, mis on esitatud **NotificationHubService** tõuketeatised teatise saatmine kõigile registreeritud klientidele.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Kuidas: saada Tõuketeatiste iOS-i rakendustele

Sama nagu eespool kirjeldatud Android rakendused, **ApnsService** objekti pakub **saata** meetod, mis saab saata Tõuketeatiste iOS-i rakendusi. **Saada** meetodit aktsepteerib järgmisi:

* **Siltide** - silt identifikaator. Kui pole sildi, saadetakse teatis kõik kliendid.
* **Last** – sõnumi JSON- või stringi last.
* **Tagasihelistamise** - funktsiooni tagasihelistamise.

Last vorming leiate lisateavet jaotisest **Teatis last** [kohalik ja Push teatis programmeerimisega juhend](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) dokumendi.

Järgmine kood kasutab **ApnsService** eksemplari, mis on esitatud **NotificationHubService** hoiatusteade saatmiseks kõik kliendid:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
        // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Kuidas: Windows Phone rakendustele saada tõuketeatised

**MpnsService** objekti pakub **saata** meetod, mis saab saata Tõuketeatiste Windows Phone rakendused. **Saada** meetodit aktsepteerib järgmisi:

* **Siltide** - silt identifikaator. Kui pole sildi, saadetakse teatis kõik kliendid.
* **Last** – sõnumi XML-i last.
* **TargetName**  -  `toast` töölauateatisega teatiste saamiseks. `token`jaoks pisikuva teatised.
* **NotificationClass** - teatise prioriteet. [Tõuketeatised serverist](http://msdn.microsoft.com/library/hh221551.aspx) dokumendi jaoks sobivad väärtused jaotisest **HTTP Päiseelemendid** .
* **Suvandid** – valikuline taotluse päised.
* **Tagasihelistamise** - funktsiooni tagasihelistamise.

Loendi kehtiv **TargetName**, **NotificationClass** ja päise suvandid, vaadake lehe [tõuketeatised serverist](http://msdn.microsoft.com/library/hh221551.aspx) .

Järgmine proovi kood kasutab **MpnsService** eksemplari, mis on esitatud **NotificationHubService** saata töölauateatise tõuketeatised on:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Kuidas: saada Tõuketeatiste Universal platvormil (UWP) rakendustele

**WnsService** objekti pakub **saata** meetod, mis saab saata Tõuketeatiste ühes kohas Windowsi platvormi rakendused.  **Saada** meetodit aktsepteerib järgmisi:

* **Siltide** - silt identifikaator. Kui pole silti, saadetakse teatis kõik registreeritud klientidele.
* **Last** - XML-i sõnumi last.
* **Tippige** - teatise tüüp.
* **Suvandid** – valikuline taotluse päised.
* **Tagasihelistamise** - funktsiooni tagasihelistamise.

Loendi kehtiv tüübid ja taotluse päised, lugege teemat [Push teatise teenuse koosolekukutsete ja kutsele vastamise päised](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

Järgmine kood kasutab **WnsService** eksemplari, mis on esitatud **NotificationHubService** UWP rakendus on tõuketeatised töölauateatise saatmiseks:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
        // notification sent
      }
    });

## <a name="next-steps"></a>Järgmised sammud

Ülaltoodud valimi pikad võimaldavad hõlpsalt luua teenuse taristu esitamisel Tõuketeatiste erinevaid seadmed. Nüüd, kui olete õppinud põhitõdesid node.js teatis jaoturi abil, järgige neid linke ja Lisateavet selle kohta, kuidas saate neid võimalusi edasi laiendada.

-   Teemast [Azure teatis jaoturi](https://msdn.microsoft.com/library/azure/jj927170.aspx)MSDN-i viide.
-   Külastage [Azure'i SDK sõlm] hoidla github näidiseid ja rakendamise üksikasjad.

  [Azure'i SDK sõlm]: https://github.com/WindowsAzure/azure-sdk-for-node
  [Next Steps]: #nextsteps
  [What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
  [Create a Service Namespace]: #create-a-service-namespace
  [Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
  [Create a Node.js Application]: #Create_a_Nodejs_Application
  [Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
  [How to: Create a Topic]: #How_to_Create_a_Topic
  [How to: Create Subscriptions]: #How_to_Create_Subscriptions
  [How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
  [How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
  [How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
  [How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
  [1]: #Next_Steps
  [Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
  [Azure Classic Portal]: http://manage.windowsazure.com
  [image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
  [2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
  [3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
  [4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
  [5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
  [SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Veebisaidi WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
  [Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
  [nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
  [Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
  [Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
  [Azure'i portaal]: https://portal.azure.com
