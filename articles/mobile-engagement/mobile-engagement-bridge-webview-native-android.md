<properties 
    pageTitle="Bridge Androidi WebView kohalikke Mobile kaasamine Android SDK" 
    description="Kirjeldab, kuidas luua mõne seose JavaScripti ja kohalikke Mobile kaasamine Android SDK WebView vahel"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Bridge Androidi WebView kohalikke Mobile kaasamine Android SDK

> [AZURE.SELECTOR]
- [Androidi Bridge](mobile-engagement-bridge-webview-native-android.md)
- [iOS-i Bridge](mobile-engagement-bridge-webview-native-ios.md)

Mõned mobiilirakendused on loodud, kui ise rakendus on välja töötatud, kasutades kohalikke Android arendamiseks, kuid mõne või kõigi ekraanid renderdamise sees on Androidi WebView rakendusena hübriid. Saate endiselt kasutada Mobile kaasamine Android SDK sellise rakendustes ja selles õpetuses kirjeldatakse, kuidas minna see. Proovi kood allpool põhineb Androidi dokumentatsiooni [siin](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). See kirjeldab, kuidas seda dokumenteeritud moodust saab rakendada sama Mobile kaasamine Android SDK levinumaid meetodid nii, et Webview hübriid rakenduse kaudu saate algatada taotluste jälgida sündmusi, töö, tõrkeid, rakenduse-teave ajal toru need meie Android SDK kaudu. 

1. Kõigepealt peate veenduge, et olete läinud meie [õpetuse alustamine](mobile-engagement-android-get-started.md) integreerida Mobile kaasamine Android SDK hübriid rakenduse kaudu. Kui te seda oma `OnCreate` meetod näeb välja järgmine.  
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }

2. Nüüd veenduge, et hübriid rakenduse on WebView ekraani seda. See kood on sarnane järgmine kus me laadimise kohaliku HTML-vormingus faili **Sample.html** Webview rakenduses on `onCreate` ekraani meetodit. 

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }

        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }

3. Nüüd looge bridge fail nimega **WebAppInterface** , mis loob ümbrisesse üle mõned sagedamini kasutatavad Mobile kaasamine Android SDK meetodid abil soovitud `@JavascriptInterface` kirjeldatud [Androidi dokumentatsiooni](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):

        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
        
        import com.microsoft.azure.engagement.EngagementAgent;
        
        import org.json.JSONArray;
        import org.json.JSONObject;
        
        public class WebAppInterface {
            Context mContext;
        
            /** Instantiate the interface and set the context */
            WebAppInterface(Context c) {
                mContext = c;
            }
        
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
        
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
        
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
        
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  

4. Kui oleme ülaltoodud silla faili loonud, tuleb tagada, et see on meie Webview seotud. See juhtub, tuleb teil muuta oma `SetWebview` meetod, nii et see näeb välja järgmine:

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }

5. Ülaltoodud koodilõigu, saame nimega `addJavascriptInterface` meie bridge klassi seostada meie Webview ja ka loodud **EngagementJs** meetodite helistamis bridge fail nimega pidet. 

6. Nüüd luua järgmine fail nimega **Sample.html** projektis **varad** mis laaditakse soovitud Webview ja kus me kutsume meetodid bridge faili kaustas.

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
                      log('window.onerror: ' + err);
                    }
        
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with the Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
        
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
        
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
        
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
        
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
        
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
        
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
        
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>

8. Võtke arvesse järgmist ülaltoodud HTML-faili kohta.

    -   See sisaldab sisend lahtrid, kuhu saate sisestada andmed, et teie sündmus, töö, viga, AppInfo kasutada. Kui klõpsate nuppu kõrval, kõne tehakse JavaScripti, mis nõuab lõpuks meetodite bridge faili selle kaasamine Mobile Androidi SDK kõne edasi. 
    -   Meil on sildistamine mõned staatilise täiendav teave sündmusi, töö ja isegi tõrkeid, näitamaks, kuidas seda saab teha. See täiendav teave saadetakse nagu JSON on string, mis, kui saate otsida soovitud `WebAppInterface` faili, on sõeluda ja pane Androidi `Bundle` ja on möödas koos saatmine töö, sündmuste tõrkeid. 
    -   Mobile kaasamine tööd on käivitati nime teie määratud Sisestuskeel väljale Käivita 10 sekundit ja sulgeda. 
    -   Sildi või Mobile kaasamine appinfo on möödunud customer_name' staatilise võti ja sisestatud sisend väärtusena sildi jaoks soovitud väärtus. 
 
9. Käivitage rakendus ja te näete järgmist. Nüüd pakkuda mõned testi sündmuse umbes järgmist nime ja selle all olevat nuppu **saada** . 

    ![][1]

10. Nüüd kui lähete teie rakendus ja vaadake jaotist **-> sündmuste üksikasjad**vahekaardil **kuvar** , kuvatakse see sündmus staatilise rakenduse teave, mida saadame koos kuvataks. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
