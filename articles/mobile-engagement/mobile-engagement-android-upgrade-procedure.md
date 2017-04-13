<properties 
    pageTitle="Azure'i mobiilsideseadmete kaasamine Android SDK integreerimine" 
    description="Uusimad värskendused ja toiminguid Android SDK Azure Mobile kaasamine"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="upgrade-procedures"></a>Toimingute täiendamine

Kui teil juba on integreeritud vanemat versiooni meie SDK rakenduse, tuleb SDK täiendamisel, võtke arvesse järgmist.

Peate järgima mitu korda, kui teil on vastamata SDK erinevates versioonides. Näiteks kui migreerite 1.4.0 1.6.0 tuleb esmalt kasutage toimingut ", et 1.5.0 1.4.0" siis korras ", et 1.6.0 1.5.0".

Mis tahes versiooni täiendamist, peate asendama selle `mobile-engagement-VERSION.jar` uuega.

##<a name="from-420-to-421"></a>Et 4.2.1 4.2.0 kaudu

Tegelikult teha seda toimingut iga versiooni SDK, kui saate integreerida REACHi on turvalisuse parandamine.

Nüüd lisage `exported="false"` kõik REACHi tegevustega.

REACHi peaks nüüd välja nägema järgmine oma `AndroidManifest.xml`:

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

##<a name="from-400-to-410"></a>Et 4.1.0 4.0.0 kaudu

SDK nüüd pidet uus õiguste mudel Androidi m.

Kui kasutate asukoha funktsioonid või üldpildiga teatised lugege [seda jaotist](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

Lisaks uute õiguste mudel, toetame nüüd konfigureerimine asukoht funktsioonid käitusajal.
Meil on ikka ühildub manifest parameetrid asukoha jaoks, kuid see on nüüd aegunud. Käitusaja konfigureerimine kasutama eemaldada järgnevalt kaudu oma ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

ja lugege [järgmist värskendatud](mobile-engagement-android-integrate-engagement.md#location-reporting) käitusaja konfiguratsiooni asemel kasutada.

##<a name="from-300-to-400"></a>Kui soovite 4.0.0 3.0.0

### <a name="native-push"></a>Kohalikke tõuketeatised

Kohalikke tõuketeatised (GCM/ADM) nüüd kasutatakse ka jaoks rakenduste teatised, mis tahes tüüpi tõuketeatised turunduskampaania kohalikke tõuketeatised identimisteave tuleb konfigureerida.

[Kui see pole veel palun toimige järgmiselt.](mobile-engagement-android-integrate-engagement-reach.md#native-push)

### <a name="androidmanifestxml"></a>AndroidManifest.xml

REACHi integreerimine on muudetud ``AndroidManifest.xml``.

Asendage see:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Järgi

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>
    <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
      <intent-filter>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
      </intent-filter>
    </receiver>

On võimalik, et laadimise ekraani nüüd teate (koos teksti/veebi sisu) või küsitluse klõpsamisel.
Teil seda nende kampaaniat 4.0.0 tööle lisada.

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Ressursid

Manustamine uue `res/layout/engagement_loading.xml` fail oma projekti.

##<a name="from-240-to-300"></a>Et 3.0.0 2.4.0 kaudu

Järgmine kirjeldatakse, kuidas migreerida mõne SDK integreerimine Capptain teenus Capptain SAS tootja Azure Mobile kaasamine rakendusse. Kui migreerite varasemast versioonist, pöörduge Capptain veebisaidi migreerimine 2.4.0 esmalt ja seejärel rakendage järgmist.

>[AZURE.IMPORTANT] Capptain ja Mobile kaasamine pole sama teenuste ja allpool toodud juhiseid tõstetakse esile ainult kuidas saate migreerida kliendi rakendus. Migreerimine rakenduse SDK on siirata andmete Capptain serverid Mobile kaasamine serverid.

### <a name="jar-file"></a>JAR faili

Asendage `capptain.jar` , `mobile-engagement-VERSION.jar` sisse oma `libs` kausta.

### <a name="resource-files"></a>Ressursi failid

Iga ressursifail, mida me esitatud (eesliide `capptain_`) on asendada uute (eesliide `engagement_`).

Kui teil on kohandatud need failid, tuleb uuesti rakendada uusi faile, mis **on ümber ka kõik identifikaatorid ressursi failid**oma kohandamine.

### <a name="application-id"></a>Rakenduse ID

Nüüd kasutab kaasamine ühendusstringi SDK identifikaatorite, nt rakenduse identifikaatori konfigureerimine.

Peate kasutama `EngagementAgent.init` meetod oma käivitit tegevuse umbes järgmine:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Ühendusstringi rakenduse jaoks kuvatakse Azure portaali.

Eemaldage kõik kõne `CapptainAgent.configure` nimega `EngagementAgent.init` asendab selle meetodi.

Funktsiooni `appId` saab enam konfigureerida abil `AndroidManifest.xml`.

Eemaldage see jaotis kaudu oma `AndroidManifest.xml` kui teil on see:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Java API

Mis tahes Java klassi meie SDK iga kõne on ümber nimetada; näiteks `CapptainAgent.getInstance(this)` nimeks tuleb `EngagementAgent.getInstance(this)`, `extends CapptainActivity` nimeks tuleb `extends EngagementActivity` jne...

Kui on ühendatud vaikimisi agent eelistused faile, vaikimisi faili nimi on nüüd `engagement.agent` ja oluline on `engagement:agent`.

Web teadaannete loomisel JavaScripti sideaine on nüüd `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Juhtunud on palju muudatusi, teenuse jagatakse enam ja palju vastuvõtjad pole enam eksporditavad.

Deklaratsioon teenuse on nüüd lihtsam; Eemaldage tahtlikult filtri ja kõik metaandmete sees ja lisage `exportable=false`.

Lisaks kõik ümber kasutama allikaid.

See näeb nüüd:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Kui soovite lubada testi logid, metaandmeid-rakenduse sildi teisaldati ja uus nimi.

            <application>
            
              <meta-data android:name="engagement:log:test" android:value="true" />
            
              <service/>
            
            </application>

Kõik muud metaandmete on lihtsalt ümber, siin on täielik (kursuse Nimeta ainult need, mida kasutate).

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>
            
            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Google Play ja SmartAd jälgimine on eemaldatud SDK tuleb lihtsalt eemaldada asenduseta:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

REACHi tegevused on nüüd deklareeritud umbes järgmine:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            
Kui teil on kohandatud REACHi tegevused, peate muutma ainult ühte vastavaks tahtlikult toimingud `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` või `com.microsoft.azure.engagement.reach.intent.action.POLL`.

On ümber nimetatud leviedastuse vastuvõtjad pluss nüüd lisada `exported=false`. Siin on täielik vastuvõtjad uue täpsustades, (kursuse Nimeta ainult need, mida kasutada):

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Jälitamise vastuvõtja on eemaldatud, et saaksite selle jaotise eemaldada:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Pange tähele, et teie rakendamine leviedastuse vastuvõtja **EngagementMessageReceiver** deklareerimist on muudetud on `AndroidManifest.xml`. Selle põhjuseks on eemaldatud API meilisõnumeid saata ega eemaldada suvalise XMPP sõnumid suvalise XMPP üksused ja API-seadmete vahel sõnumite saatmiseks ja vastuvõtmiseks. Seega on teil ka järgmised kontekstiatribuuti kustutamiseks oma **EngagementMessageReceiver** rakendamist.

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

ja

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

Kustutage kõik kõne **EngagementAgent** jaoks:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

ja

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard

Proguard konfiguratsiooni võivad mõjutada rebranding, reegleid on nüüd otsivad, nt:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }
            
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }
 
