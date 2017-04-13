<properties
    pageTitle="Azure'i SDK integratsiooni Mobile kaasamine iOS-i | Microsoft Azure'i"
    description="Uusimate värskenduste ja toimingute iOS-i SDK Azure Mobile kaasamine"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-on-ios"></a>Kuidas integreerida kaasamine iOS-i kohta

> [AZURE.SELECTOR]
- [Windowsi universaalne](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlighti](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS-i](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Siin kirjeldatakse lihtsaim viis suhete 's analüüsi- ja jälgida oma iOS-i rakenduse funktsioonide aktiveerimine.

Kaasamine SDK nõuab iOS6 + ja Xcode 8: juurutamise target rakenduse peab olema vähemalt iOS 6.

> [AZURE.NOTE]
> Kui te tegelikult sõltuvad XCode 7 võib kasutada [iOS-i kaasamine SDK v3.2.4](https://aka.ms/r6oouh). On teadaolev viga REACHi moodul eelmise versiooni ning töötab iOS 10-seadmetes lugege teemat [REACHi mooduli integreerimise](mobile-engagement-ios-integrate-engagement-reach.md) jaoks rohkem üksikasju. Kui otsustate kasutada SDK v3.2.4, siis lihtsalt vahele jätta selle `UserNotifications.framework` importida järgmise juhise juurde.

Järgmised toimingud on piisavalt arvutada statistikat kõik kasutajad, seansid, tegevused, jookseb ja Technicals vaja logid aruande aktiveerimiseks. Aruande arvutada teiste statistikute reguleerimist sündmusi, tõrgete ja tööd, nagu on vaja logid tuleb teha käsitsi kaasamine API abil (vt [Täpsemalt Mobile kaasamine sildistamine API iOS-i rakenduse kasutamise kohta](mobile-engagement-ios-use-engagement-api.md) , kuna statistika on sõltuva.

##<a name="embed-the-engagement-sdk-into-your-ios-project"></a>IOS-i projekti kaasamine SDK embed

- Laadige alla [siin](http://aka.ms/qk2rnj)iOS-i SDK.

- Lisada kaasamine SDK iOS-i projekti: Xcode, projekti paremklõps ja valige **"Faile lisada …"** ja valige soovitud `EngagementSDK` kausta.

- Kaasamine nõuab täiendavate raamistiku töötamiseks: rakenduses project explorer oma projekti paani avamine ja valige õige eesmärk. Seejärel avage vahekaart **"Koostada etapist"** ja **"Link kahendarvu koos teekide"** menüüs lisada need raamistikku:

    -   `UserNotifications.framework`-link, mis seadmine`Optional`
    -   `AdSupport.framework`-link, mis seadmine`Optional`
    -   `SystemConfiguration.framework`
    -   `CoreTelephony.framework`
    -   `CFNetwork.framework`
    -   `CoreLocation.framework`
    -   `libxml2.dylib`

> [AZURE.NOTE] AdSupport raames saab eemaldada. Kaasamine peab sellega soovitud IDFA kogumiseks. Siiski saate keelatud IDFA saidikogumi \<iOS-i sdk-kaasamine idfa\> täitmiseks Apple'i uue poliitika kohta selle ID-ga.

##<a name="initialize-the-engagement-sdk"></a>Kaasamine SDK lähtestamine

Peate muutma oma rakenduse volitatud esindaja:

-   Rakendamine faili ülaosas importida agent allikaid.

        [...]
        #import "EngagementAgent.h"

-   Lähtestada kaasamine meetodiga '**applicationDidFinishLaunching:**"või"**rakendus: didFinishLaunchingWithOptions:**":

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
          [...]
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
          [...]
        }

##<a name="basic-reporting"></a>Tavaline teatamine

### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Soovitatav meetod: ülekoormuse oma `UIViewController` tunnid

Selleks, et aktiveerida kõik logid nõutud kaasamine arvutada kasutajaid, seansid, tegevused, jookseb ja tehniline statistika aruande, vaid saate kõik teie `UIViewController` alamtäpi tunnid pärivad selle `EngagementViewController` tunnid (sama reegel jaoks `UITableViewController`  - \> `EngagementTableViewController`).

**Ilma kaasamine:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**Kus lingid:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Alternatiivne meetod: kõne `startActivity()` käsitsi

Kui te ei saa või ei soovi ülekoormuse oma `UIViewController` tunnid, selle asemel saate alustada oma tegevuste helistades `EngagementAgent`'s meetodite otse.

> [AZURE.IMPORTANT] IOS-i SDK kõnede automaatselt selle `endActivity()` meetod, kui rakendus on suletud. Seega on *väga* soovitatav helistamiseks on `startActivity` iga kord, kui tegevuse kasutaja muutmine ja *mitte kunagi* kõne meetod on `endActivity` meetod, kuna selle meetodi jõustab praeguse seansi tuleb lõpetada.

##<a name="location-reporting"></a>Asukoha teatamine

Apple'i Teenusetingimused ei võimalda rakendusi kasutada asukoha jälgimine statistika eesmärgil. Seega on soovitatav asukoht aruanded lubama ainult siis, kui rakenduse kasutada ka asukoha jälgimise mõnel muul põhjusel.

Alustades iOS 8, peate sisestama kuidas teie rakendus kasutab seadmisega stringi võti [NSLocationWhenInUseUsageDescription] või [NSLocationAlwaysUsageDescription] oma rakenduse Info.plist faili asukoha kirjeldus. Kui soovite aruande asukoht kaasamine tausta, lisada võti NSLocationAlwaysUsageDescription. Kõigil muudel juhtudel lisada võti NSLocationWhenInUseUsageDescription.

### <a name="lazy-area-location-reporting"></a>Rongile ala asukoha teatamine

Rongile ala asukoht aruandlus võimaldab aruande riik, piirkond ja seotud seadmete asukoht. Seda tüüpi asukoha teatamise kasutab ainult võrgukohtades (vastavalt lahtri ID või WiFi-ühendus). Kõige kord seansi esitatakse seadmes alale. GPS ei kasutata ja seega seda tüüpi aruande asukoht on väga vähe (ei tähenda, et ei) mõju kohta.

Teatatud alade kasutatakse kasutajaid, seansid, sündmused ja vigu geograafilised statistikat arvutada. Ta saab kasutada ka kriteeriumi REACHi kampaaniat. Viimase teadaolevad ala teatatud seadme saab tuua tänu [Seadme API].

Rongile ala asukoht aruandluse lubamiseks lisage järgmine rida vormindatud kaasamine agent:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Reaalajas asukoha teatamine

Reaalajas asukoha teatamine võimaldab aruande laiuskraad ja pikkuskraad seotud seadmed. Vaikimisi seda tüüpi asukoha teatamise kasutab ainult võrgukohtades (vastavalt lahtri ID või WiFi-ühendus) ja aruandluse ainult on aktiivne, kui rakendus töötab esiplaanil (nt käigus seansi).

Reaalajas asukohad on *pole* kasutatud arvutada statistikat. Nende ainus eesmärk on luba kasutada reaalajas geo hoides \<REACHi-sihtrühma-geofencing\> kriteeriumi REACHi kampaaniat.

Reaalajas asukoha aruandluse lubamiseks lisage järgmine rida vormindatud kaasamine agent:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>GPS põhineva aruandlus

Vaikimisi reaalajas asukoha teatamise kasutab ainult vastavalt võrgukohtades. Lisage vastavalt GPS asukohad (mis on palju Täpsem) kasutamise lubamine

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Tausta teatamine

Vaikimisi reaalajas asukoha teatamine on aktiivne ainult kui rakendus töötab esiplaanil (nt käigus seansi). Taustal ka aruandluse lubamiseks lisamiseks tehke järgmist.

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [AZURE.NOTE] Kui rakendus töötab taustal, ainult võrgus vastavalt asukohad esitatakse ka siis, kui olete lubanud GPS.

See funktsioon rakendamine helistab [startMonitoringSignificantLocationChanges] , kui rakenduse läheb tausta. Pange tähele, et see automaatselt uuesti rakenduse tagaplaanile kui saabub asukoht uus sündmus.

##<a name="advanced-reporting"></a>Täpsem aruandlus

Soovi korral võite kui soovite teatada rakenduse teatud sündmused, tõrgete ja -tööde haldamine, peate kasutama kaasamine API meetodid kaudu soovitud `EngagementAgent` klassi. Selle rühma objekti saab tuua helistades on `[EngagementAgent shared]` staatiline meetod.

Kaasamine API võimaldab kasutada kõiki suhete 's täpsemalt võimaluste ja on üksikasjalikult kirjeldatud juhised kaasamine API kasutamine iOS-i (samuti dokumentatsioon, on `EngagementAgent` klassi).

##<a name="disable-idfa-collection"></a>IDFA saidikogumi keelamine

Vaikimisi kasutab kaasamine [IDFA] kordumatult tuvastada kasutaja. Aga kui te ei kasuta reklaami mujal rakenduses, võib teie tagasi lükatud App Store läbivaatus protsessi. IDFA saidikogumi saab selle keelata, lisades preprocessor makro `ENGAGEMENT_DISABLE_IDFA` pch faili (või selle `Build Settings` rakenduse). See tagab, et on viiteid `ASIdentifierManager`, `advertisingIdentifier` või `isAdvertisingTrackingEnabled` rakenduse koostamine.

Integratsioon **prefix.pch** faili:

    #define ENGAGEMENT_DISABLE_IDFA
    ...

Saate kontrollida, et IDFA saidikogumi on õigesti keelatud rakenduse, märkides ruudu kaasamine testi logid. Vt integreerimine testida\<ios-sdk-kaasamine-testi-idfa\> dokumentatsiooni kohta lisateabe saamiseks.

##<a name="disable-log-reporting"></a>Keela log teatamine

### <a name="using-a-method-call"></a>Kõne meetodi abil

Kui soovite kaasamine peatamiseks logid saata, saate helistada.

    [[EngagementAgent shared] setEnabled:NO];

See kõne on püsiv: kasutab `NSUserDefaults` teabe talletamiseks.

Log teatamise uuesti sama funktsiooni abil saate lubada `YES`.

### <a name="integration-in-your-settings-bundle"></a>Teie sätted kogumis integreerimine

Asemel nõuab seda funktsiooni, saate ka integreerida see säte sisse otse oma praegust `Settings.bundle` faili. Stringi `engagement_agent_enabled` tuleb kasutada mõnda eelistused identifikaator ja see peab olema seotud tumblernupu vahetamine (`PSToggleSwitchSpecifier`).

Järgmises näites `Settings.bundle` näitab, kuidas seda rakendada:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[Seadme API]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
