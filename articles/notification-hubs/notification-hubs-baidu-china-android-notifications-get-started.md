<properties
    pageTitle="Azure'i teatis jaoturi abil Baidu alustamine | Microsoft Azure'i"
    description="Selles õppetükis saate teada, kuidas Azure'i teatis jaoturi abil tõuketeatised ja Androidi seadmed Baidu abil."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="08/19/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-using-baidu"></a>Teatis jaoturi Baidu kasutamise alustamine

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Ülevaade

Baidu cloud tõuketeatised on Hiina pilveteenuses, mille abil saate saata Tõuketeatiste mobiilsideseadmete jaoks. See teenus on eriti kasulik Hiinas, kus pakkuda Tõuketeatiste Android on keeruline tõttu eri app salvestab ja tõuketeatised teenused, lisaks kättesaadavus Androidi seadmetes, mis on tavaliselt seotud GCM (Google Cloud sõnumside).

##<a name="prerequisites"></a>Eeltingimused

Selle õpetuse kasutamiseks on vaja järgmist:

+ Android SDK (me oletame, et kasutate Eclipse), mille saate alla laadida <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android saidi</a>
+ [Mobiil teenuste Android SDK]
+ [Baidu tõuketeatised Android SDK]

>[AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).


##<a name="create-a-baidu-account"></a>Baidu konto loomine

Baidu kasutamiseks peab teil olema Baidu konto. Kui teil juba on üks, [Baidu portaali] sisse logida ja jätkake järgmise juhisega. Muul juhul lugege juhiseid all Baidu konto loomiseks.  

1. [Baidu portaali] ja klõpsake linki**登录**(**Logi sisse**). Klõpsake**立即注册**alustamiseks konto registreerimise käigus.

    ![][1]

2. Sisestage vajalikud andmed – telefon/e-posti aadress ja parool kinnitamine koodi – ja klõpsake nuppu **registreeru**.

    ![][2]

3. Saate saata e-posti e-posti aadress sisestatud link Baidu konto aktiveerimiseks.

    ![][3]

4. Logige sisse oma meilikontoga, avage Baidu aktiveerimise mail ja aktiveerimise link Baidu konto aktiveerimiseks klõpsake.

    ![][4]

Kui teil on aktiveeritud Baidu konto, logige sisse [portaali Baidu].

##<a name="register-as-a-baidu-developer"></a>Registreerida Baidu arendaja

1. Kui teil on [Baidu portaali]sisse logitud, klõpsake**更多 >>** (**rohkem**).

    ![][5]

2. Liikuge kerides allapoole ja klõpsake jaotises**站长与开发者服务 (veebimeistri ja arendaja teenused)** ja klõpsake nuppu**百度开放云平台**(**Baidu avamine pilve platvormi**).

    ![][6]

3. Järgmisel lehel nuppu**开发者服务**(**Arendaja teenused**), klõpsake paremas ülanurgas.

    ![][7]

4. Järgmisel lehel klõpsake paremas ülanurgas käsku**注册开发者**(**Registreeritud arendajad**).

    ![][8]

5. Sisestage oma nimi, kirjeldus ja mobiiltelefoninumbri kinnitamine tekstsõnum ja klõpsake**送验证码**(**Saada kinnituskood**). Pange tähele, et rahvusvahelise telefoninumbri, peate ümbritsema sulgudes riigi kood. Nt Ameerika Ühendriikide mitmete oleks **(1) 1234567890**.

    ![][9]

6. Seejärel peaks saama tekstsõnum kinnitamine numbriga, nagu on näidatud järgmises näites:

    ![][10]

7. Sisestage sõnum**验证码**(**kinnituskood**) kontroll number.

8. Lõpuks arendaja registreerimise lõpuleviimiseks Baidu lepingu vastu võtmist ja klõpsates nuppu**提交**(**Edasta**). Kuvatakse järgmine lehekülg edukaks registreerimist.

    ![][11]

##<a name="create-a-baidu-cloud-push-project"></a>Baidu cloud tõuketeatised projekti loomine

Baidu cloud tõuketeatised projekti loomisel kuvatakse teie rakenduse ID, API võti ja salajane võti.

1. Kui teil on [Baidu portaali]sisse logitud, klõpsake**更多 >>** (**rohkem**).

    ![][5]

2. Liikuge kerides allapoole, klõpsake jaotises**站长与开发者服务**(**veebimeistri ja arendaja teenused**) ja klõpsake nuppu**百度开放云平台**(**Baidu avamine pilve platvormi**).

    ![][6]

3. Järgmisel lehel nuppu**开发者服务**(**Arendaja teenused**), klõpsake paremas ülanurgas.

    ![][7]

4. Järgmisel lehel nuppu**云推送**(**Pilve Push**) jaotises**云服务**(**Pilveteenustega**).

    ![][12]

5. Kui olete registreeritud arendaja, kuvatakse**管理控制台**(**Halduskonsool**) ülaosas menüü. Klõpsake**开发者服务管理**(**arendajate teenuse haldus**).

    ![][13]

6. Järgmisel lehel nuppu**创建工程**(**Loo projekt**).

    ![][14]

7. Sisestage rakenduse nimi ja klõpsake nuppu**创建**(**Loo**).

    ![][15]

8. Pärast Baidu cloud tõuketeatised projekti luua, kuvatakse lehe **AppID**, **API võti**ja **Salajane võti**. Kirjutage API võti ja salajane võti, mida me kasutame hiljem.

    ![][16]

9. Projekti Tõuketeatiste konfigureerimine, klõpsates**云推送**(**Pilve tõuketeatised**) vasakul paanil.

    ![][31]

10. Järgmisel lehel nuppu**推送设置**(**Push sätted**).

    ![][32]  

11. Lisage lehel konfiguratsioon paketi nimi, et teil on kasutamine Androidi projekti**应用包名**(**rakendusepaketi**) välja, ja klõpsake**保存设置**(**Salvesta**).  

    ![][33]

Kuvatakse selle**保存成功!** (**edukalt salvestatud!**) sõnumi.

##<a name="configure-your-notification-hub"></a>Teie teate jaoturi konfigureerimine

1. [Azure'i klassikaline portaali]sisse logida ja valige **+ Uus** ekraani allosas.

2. Klõpsake **Rakenduse teenused**, klõpsake **Teenuse siini**, klõpsake **Teate jaoturi**ja klõpsake **Kiiresti luua**.

3. Sisestage oma **Teate jaoturi**nimi, valige **piirkond** ja **Namespace** , kus see teatis jaoturi luuakse, ja seejärel klõpsake käsku **Loo uus teatis keskus**.  

    ![][17]

4. Klõpsake nimeruumi, mille lõite teie teate jaoturi ja klõpsake **Teate jaoturi** ülaosas.

    ![][18]

5. Valige teate jaoturi loodud ja klõpsake nuppu **Konfigureeri** ülalt menüüst.

    ![][19]

6. Liikuge kerides jaotiseni **baidu teatise sätteid** ja sisestage API võti ja salajane võti, mis on saadud Baidu konsooli varem Baidu cloud tõuketeatised projekti jaoks. Klõpsake nuppu **Salvesta**.

    ![][20]

7. Klõpsake vahekaarti **armatuurlaua** ülaosas teate jaoturi ja klõpsake **Kuva ühendusstring**.

    ![][21]

8. Märkige üles **DefaultListenSharedAccessSignature** ja **DefaultFullSharedAccessSignature** **juurdepääsu ühenduseteavet** aknast.

    ![][22]

##<a name="connect-your-app-to-the-notification-hub"></a>Teate jaoturi rakenduse ühendamine

1. Eclipse ADT, looge uus Androidi projekt (**faili** > **Uus** > **Androidi rakenduse Project**).

    ![][23]

2. Sisestage konto **Nimi** ja veenduge, et **Minimaalne vajalik SDK** versioon on määratud **API 16: Android 4.1**.

    ![][24]

3. Klõpsake nuppu **edasi** ja jätkake pärast viisardi, kuni kuvatakse aken **Tegevuste loomine** . Veenduge, et valitud oleks **Tühi tegevuste** ja lõpuks valige **valmis** uue Androidi rakenduse loomine.

    ![][25]

4. Veenduge, et **Projekt koostada Target** on õigesti seatud.

    ![][26]

5. Teatis – jaoturi-0.4.jar faili alla laadida [Teatis-jaoturi-Android-SDK Bintray klõpsake](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4)menüüd **failid** . Eclipse projekti **kena tegevust jälle** kausta faili lisada ja värskendada *kena tegevust jälle* kausta.

6. Laadige alla ja pakkige see lahti [Baidu Push Android SDK], **kena tegevust jälle** kausta avamiseks ning seejärel kopeerige **pushservice-x.y.z** jar faili- ja **armeabi** & **minimaalsed** kaustade Androidi rakenduse **kena tegevust jälle** kausta.

7. Avage fail **AndroidManifest.xml** Androidi projekti ja lisage Baidu SDK vajalikud õigused.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. **Android: nime** atribuudi lisada oma **rakenduse** element **AndroidManifest.xml**, asendades *yourprojectname* (nt **com.example.BaiduTest**). Veenduge, et selle projekti nimi vastab mõnele, Baidu konsooli konfigureeritud.

        <application android:name="yourprojectname.DemoApplication"

9. Lisage järgmine konfiguratsioon rakenduse elemendi jooksul pärast selle **. MainActivity** tegevuse element, asendades *yourprojectname* (nt **com.example.BaiduTest**):

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. Saate lisada uue nimega projekti **ConfigurationSettings.java** klassi.

    ![][28]

    ![][29]

10. Lisada järgmine kood:

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    Määrake väärtus **API_KEY** laadisite Baidu pilve projekti varasemas versioonis, **NotificationHubName** Azure klassikaline portaalis teie teatise jaoturi nimi ja **NotificationHubConnectionString** koos DefaultListenSharedAccessSignature Azure klassikaline portaalist.

11. Uue klassi nimetatakse **DemoApplication.java**ja lisage selle järgmine kood:

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. Teise uue klassi nimetatakse **MyPushMessageReceiver.java**ja all koodi lisamine. See on ainekursust, mis tegeleb Baidu tõuketeatised serverist saadud tõuketeatisi.

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. Avage **MainActivity.java**ja lisage järgmine **onCreate** meetod.

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. Avage ülaosas impordi järgmistest:

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##<a name="send-notifications-to-your-app"></a>Saada teatisi oma rakenduse


Saate kiiresti testida rakenduse teatiste saatmisega teatised [Azure portaali](https://portal.azure.com/) abil nupp **Testimiseks saata** teate jaoturi, nagu on näidatud allpool ekraani.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Tõuketeatiste tavapäraselt tagaandmebaas teenuses nagu Mobile teenuseid või ASP.net-i abil ühilduvad teeki. Saate REST API otse saatmiseks teated, kui teegis pole saadaval teie tagaandmebaas.

Selles õpetuses me Hoidke sõnum lihtne ja lihtsalt näidata testimine oma kliendi rakenduse saatmisega .NET SDK kasutamise teatis jaoturi konsooli rakendus asemel kirjutamata teenuse teatised. Soovitame [Kasutamine teatis jaoturi abil Tõuketeatiste kasutajatele](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) õpetuse järgmise sammuna teatiste saatmine on ASP.net-i taustväärtus. Järgmistest võimalustest saab saatmise teatised:

* **Ülejäänud kasutajaliidese**: saate toetada teatis mis tahes kirjutamata platvormi [ülejäänud kasutajaliidese](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)abil.

* **Microsoft Azure'i teatis jaoturi .NET SDK**: the Nugeti pakett halduris Visual Studio, käivitage [Installi pakett Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js** : [teatis jaoturi kaudu Node.js kasutamise kohta](notification-hubs-nodejs-push-notification-tutorial.md).

* **Mobiilirakenduste kohta**: Kuidas saata teatised Azure'i rakenduse teenuse mobiilirakenduste kirjutamata, mis on integreeritud teatis jaoturi näide leiate [Lisa Tõuketeatiste oma mobiilirakenduse abil](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).

* **Java / PHP**: näide sellest, kuidas saata teateid, kasutades REST API-de kohta teavet teemast "kasutada teatis jaoturi kaudu Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

##<a name="optional-send-notifications-from-a-net-console-app"></a>(Valikuline) Saada teatisi .net-i konsooli rakenduse kaudu.

Selles jaotises kuvame saata teate, kasutades .net-i konsooli rakendus.

1. Loo uus konsool rakendus Visual C#:

    ![][30]

2. Paketi Manager konsooli aknas määrata **vaikimisi projekti** uue projekti konsooli rakendus ja seejärel konsooli aken, käivitage järgmine käsk:

        Install-Package Microsoft.Azure.NotificationHubs

    Sellega lisatakse Azure'i teatis jaoturi SDK abil <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification jaoturi Nugeti pakett</a>viide.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. Avage fail **Program.cs** ja lisage järgmine kasutamine lause:

        using Microsoft.Azure.NotificationHubs;

4. Klõpsake oma `Program` klassi, lisage järgmise meetodi ja asendage väärtused, mida teil on *DefaultFullSharedAccessSignatureSASConnectionString* ja *NotificationHubName* .

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. Lisage järgmised read oma **Esilehele** meetod.

         SendNotificationAsync();
         Console.ReadLine();

##<a name="test-your-app"></a>Testige oma rakenduse

See rakendus on tegelik telefoniga testimiseks just arvutiga telefon USB-kaabli abil. Sellega laaditakse teie rakendus manustatud telefoni.

See rakendus emulaator, ülemisel tööriistaribal Eclipse testimiseks klõpsake nuppu **Käivita**ja seejärel valige oma rakendus. See käivitab emulaator, ja seejärel laadib ja rakendus töötab.

Rakenduse toob "kasutajanimi" ja 'channelId' Baidu tõuketeatiseteenus ja registreerib teate jaoturi.

Saate kasutada menüü silumine Azure'i klassikaline portaali testi teatise saatmine. Kui varem Visual Studio .net-i konsooli rakendus, vajutage klahvi F5 Visual Studio rakenduse käivitamiseks. Rakendus saadab teate, et teie seadme või emulaator ülemise olekualal kuvatakse.


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Android SDK mobiilsideseadmete teenused]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu tõuketeatised Android SDK]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[Azure'i klassikaline portaal]: https://manage.windowsazure.com/
[Baidu portaal]: http://www.baidu.com/
