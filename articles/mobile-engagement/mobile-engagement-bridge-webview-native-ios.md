<properties 
    pageTitle="IOS-i WebView kohalikke Mobile kaasamine iOS SDK ületada" 
    description="Kirjeldab, kuidas luua mõne seose JavaScripti ja kohalikke Mobile kaasamine iOS SDK WebView vahel"      
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
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a>IOS-i WebView kohalikke Mobile kaasamine iOS SDK ületada

> [AZURE.SELECTOR]
- [Androidi Bridge](mobile-engagement-bridge-webview-native-android.md)
- [iOS-i Bridge](mobile-engagement-bridge-webview-native-ios.md)

Mõned mobiilirakendused on loodud hübriid rakendusena kui ise rakendus on välja töötatud, kasutades kohalikke iOS-i eesmärk-C arengu, kuid mõne või kõigi ekraanid renderdamise iOS WebView sees. Saate siiski kasutada selliseid rakendustes SDK Mobile kaasamine iOS-i mobiilirakendustes ja selles õpetuses kirjeldatakse, kuidas minna see. 

On selleks kuigi mõlemad on dokumentideta kaks võimalust:

- Esmalt üks on kirjeldatud seda [linki](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) , mis hõlmab registreerimisel on `UIWebViewDelegate` veebivaade ja saagi-ja-kohe-Loobu asukoha muutmine, JavaScript valmis. 
- Teine üks põhineb selle [WWDC 2013 seansi](https://developer.apple.com/videos/play/wwdc2013/615)lähenemisviisi, mis on puhtam, kui esimene ja järgime selle juhendi. Pange tähele, et see lähenemine toimib ainult iOS7 ja kohal. 

Järgige alltoodud juhiseid for iOS ületada näidis:

1. Kõigepealt peate veenduge, et olete läinud meie [õpetuse alustamine](mobile-engagement-ios-get-started.md) integreerida Mobile kaasamine iOS SDK hübriid rakenduse kaudu. Soovi korral saate lubada testi logimine järgmiselt nii, et te näete SDK meetodite me käivitamine need on webview kaudu. 
    
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
            [EngagementAgent setTestLogEnabled:YES];
           ....
        }

2. Nüüd veenduge, et hübriid rakenduse on webview ekraani seda. Saate lisada selle funktsiooni `Main.storyboard` rakenduse. 

3. Selle webview seostada oma **ViewController** , klõpsates ja lohistades soovitud webview vaade kontrolleril stseeni abil soovitud `ViewController.h` redigeerimine Kuva, asetades selle just allpool on `@interface` joon. 

4. Kui te ei tee seda, avaneb dialoogiboks küsiks nimi. Sisestage nimi kujul **webView**. Teie `ViewController.h` fail peaks välja nägema umbes järgmine:

        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
        
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
        
        @end

5. Uuendame soovitud `ViewController.m` faili hiljem, kuid kõigepealt loome silla faili, mis loob ümbrisesse üle mõned sagedamini kasutatavad Mobile kaasamine iOS SDK meetodid. Looge uus päise fail nimega **EngagementJsExports.h** , mis kasutab funktsiooni `JSExport` süsteem kirjeldatud eelnimetatud [seansi](https://developer.apple.com/videos/play/wwdc2013/615) esitamist kohalikke iOS-i meetodid. 

        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
        
        @protocol EngagementJsExports <JSExport>
        
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
        
        @end

        @interface EngagementJs : NSObject <EngagementJsExports>

        @end

6. Järgmiseks loome silla faili teine osa. Looge fail nimega **EngagementJsExports.m** , mis sisaldab loomise tegelik ümbriste helistades Mobile kaasamine iOS-i SDK meetodite rakendamist. Ka Pange tähele, et saaksime on sõelumine soovitud `extras` on möödunud webview JavaScripti ja paneb, et sisse on `NSMutableDictionary` objekti edastatava kaasamine SDK meetodi kutsed.  

        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
        
        @implementation EngagementJs
        
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
        
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
        
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
        
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
        
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
        
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
           
           return extras;
        }
        
        @end

5. Nüüd **ViewController.m** naasta ja värskendage seda järgmine kood: 
        
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
        
        @interface ViewController ()
        
        @end
        
        @implementation ViewController
        
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
        
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
           
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
        
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
           
           context[@"EngagementJs"] = [EngagementJs class];
        }
        
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
        
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
        
        @end

6. Võtke arvesse järgmist **ViewController.m** faili kohta.

    - Klõpsake soovitud `loadWebView` meetod me laadimise kohaliku HTML-fail nimega **LocalPage.html** kelle vaatame järgmine kood. 
    - Rakenduses selle `webViewDidFinishLoad` meetod on haardeseadised on `JsContext` ja meie ümbris klassi seostada seda. See võimaldab helistades meie ümbris SDK meetodite abil kaudu soovitud webView **EngagementJs** pidet. 

7. Looge fail nimega **LocalPage.html** järgmine kood:

        <!doctype html>
        <html>
           <head>
               <style type='text/css'>
                   html { font-family:Helvetica; color:#222; }
                   h1 { color:steelblue; font-size:22px; margin-top:16px; }
                   h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
               </style>
               
               <script type="text/javascript">
               
               window.onerror = function(err)
               {
                   alert('window.onerror: ' + err);
               }
               
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with the Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
                   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
                           
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
        
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
                           
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
               
           </head>
           <body>
               <h1>Bridge Tester</h1>
               
               <div id='engagement'>
                   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
                   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
                   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
                   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
               
               </div>
           </body>
        </html>

8. Võtke arvesse järgmist ülaltoodud HTML-faili kohta.

    -   See sisaldab sisend lahtrid, kuhu saate sisestada andmed, et teie sündmus, töö, viga, AppInfo kasutada. Kui klõpsate nuppu kõrval, kõne tehakse JavaScripti, mis nõuab lõpuks meetodite bridge faili selle Mobile kaasamine iOS SDK kõne edasi. 
    -   Meil on sildistamine mõned staatilise täiendav teave sündmusi, töö ja isegi tõrgete näidata, kuidas seda saab teha. See täiendav teave saadetakse nagu JSON on string, mis, kui saate otsida soovitud `EngagementJsExports.m` faili, on sõeluda ja koos töö, sündmuste tõrgete saatmine on möödas. 
    -   Mobile kaasamine tööd on käivitati nime teie määratud Sisestuskeel väljale Käivita 10 sekundit ja sulgeda. 
    -   Sildi või Mobile kaasamine appinfo on möödunud customer_name' staatilise võti ja sisestatud sisend väärtusena sildi jaoks soovitud väärtus. 
 
9. Käivitage rakendus ja te näete järgmist. Nüüd pakkuda mõned testi sündmuse umbes järgmist nime ja klõpsake selle kõrval nuppu **saada** . 

    ![][1]

10. Nüüd kui lähete teie rakendus ja vaadake jaotist **-> sündmuste üksikasjad**vahekaardil **kuvar** , kuvatakse see sündmus staatilise rakenduse teave, mida saadame koos kuvataks. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
