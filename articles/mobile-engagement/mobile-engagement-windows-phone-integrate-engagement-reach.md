<properties 
    pageTitle="Windows Phone Silverlighti REACHi SDK integreerimine" 
    description="Kuidas integreerida Azure mobiilsideseadmete kaasamine REACHi Windows Phone Silverlighti rakendused"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone Silverlighti REACHi SDK integreerimine

Peate järgima kirjeldatud [Windows Phone Silverlighti kaasamine SDK integreerimine](mobile-engagement-windows-phone-integrate-engagement.md) enne järgmise juhendi integreerimine korras.

##<a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>Windows Phone Silverlighti projekti kaasamine saavutamiseks SDK embed

Te ei ole midagi lisada. `EngagementReach`viited ja ressursid on juba projektis.

> [AZURE.TIP]  Saate kohandada pilte, mis asub selle `Resources` kausta projekti, eriti kaubamärgi ikooni (seda vaikekäitumist ikooni Engagement).

##<a name="add-the-capabilities"></a>Võimaluste lisamine

Kaasamine saavutamiseks SDK peab mõned täiendavad võimalused.

Avage oma `WMAppManifest.xml` faili ja veenduge, et deklareeritud järgmisi funktsioone:

-   `ID_CAP_PUSH_NOTIFICATION`
-   `ID_CAP_WEBBROWSERCOMPONENT`

Esimene kasutab MPNS teenuse töölauateatise kuvamise lubamiseks. Teine kasutatakse brauseri tööülesande embed SDK.

Redigeerimine on `WMAppManifest.xml` faili ja lisada sees on `<Capabilities />` silt:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

##<a name="enable-the-microsoft-push-notification-service"></a>Luba selle Microsofti Lükketeatiste teenus

**Microsoft Push teavitusteenuse** (nimetatakse MPNS) kasutamiseks oma `WMAppManifest.xml` faili peab olema ka `<App />` silt on `Publisher` atribuut määratud projekti nime.

##<a name="initialize-the-engagement-reach-sdk"></a>Kaasamine REACHi SDK lähtestamine

### <a name="engagement-configuration"></a>Kaasamine konfigureerimine

Kaasamine konfigureerimine on tsentraliseeritud soovitud `Resources\EngagementConfiguration.xml` projekti fail.

Selles failis REACHi konfiguratsiooni redigeerimiseks tehke järgmist.

-   *Valikuline*märkida, kas kohalikke tõuketeatised (MPNS) on aktiveeritud või mitte vahel `<enableNativePush>` ja `</enableNativePush>` sildid, (`true` vaikimisi).
-   *Valikuline*vahel tõuketeatised kanali nimi `<channelName>` ja `</channelName>` sildid, sisestage sama, et rakenduse praegu võib kasutada või tühjaks jätta.

Kui soovite määrata selle käitusajal, saate helistada enne kaasamine agent lähtestamine järgmisel viisil:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */
    
    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [AZURE.TIP] Saate määrata MPNS tõuketeatised kanali oma rakenduse nimi. Vaikimisi loob kaasamine põhjal appId nimi. Teil pole vaja määrata nime ise, välja arvatud juhul, kui kavatsete kasutada tõuketeatised kanali väljaspool allikaid.

### <a name="engagement-initialization"></a>Kaasamine lähtestamine

Muutke soovitud `App.xaml.cs`:

-   Lisada oma `using` laused:

        using Microsoft.Azure.Engagement;

-   Lisa `EngagementReach.Instance.Init` kohe pärast `EngagementAgent.Instance.Init` sisse `Application_Launching` :

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

-   Lisa `EngagementReach.Instance.OnActivated` sisse selle `Application_Activated` meetod:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

> [AZURE.IMPORTANT] Funktsiooni `EngagementReach.Instance.Init` sihtotstarbeline teemas töötab. Teil pole seda ise teha.

##<a name="app-store-submission-considerations"></a>App store esitamise kaalutlused

Microsoft paneb mõned reeglid selle Tõuketeatiste kasutamisel:

Jaotise Microsoft [Rakenduste poliitikad] dokumentatsioon, 2.9:

1) Peate paluma, et aktsepteerida tõuketeatisi vastuvõtmiseks kasutaja. Seejärel lisage oma sätted võimalus keelata push teatised.

Objekti EngagementReach pakub kahte meetodit haldamiseks ning valida-/ valida väljaregistreerimine, `EnableNativePush()` ja `DisableNativePush()`. Võite, näiteks luua suvandi sätete abil soovitud asendisse MPNS lubamiseks või keelamiseks.

Saate valida ka inaktiveerida MPNS kaudu kaasamine konfiguratsiooni\<windows phone-sdk-REACHi-konfiguratsiooni\>.

> 2.9.1) rakenduse peate esmalt Kirjeldage teatised esitatakse ja **kasutaja kiire luba (osalemine)**, ja **peab võimaldama süsteem, mille kaudu saate kasutaja loobuda vastuvõtmine Tõuketeatiste**. Kõik teatised, kasutades Microsoft Push teavitusteenuse peab vastama kasutajale anda kirjeldus ja järgima kõiki kohaldatavaid [Rakenduste poliitikad]  [ Content Policies] ja [Rakenduse teatud tüüpi lisanõuded].

2) Ärge kasutage liiga palju tõuketeatisi. Kaasamine hakkama teile teatised.

> 2.9.2) rakendus ja selle kasutamine selle Microsofti Lükketeatiste teenus pole liiga võrgu läbilaskevõime või Microsoft tõuketeatiseteenus ribalaius või muul viisil asjatult koormata Windows Phone või muu Microsoft seadme või teenust liigne Tõuketeatiste, mis on määratud Microsoft oma äranägemisel, ja peab kahjustada ega varjutaks mis tahes Microsofti võrkude serverites või mis tahes kolmanda osapoole serverites või võrguga ühendatud Microsoft tõuketeatiseteenus.

3) Toetuvad MPNS criticals teabe saatmiseks. Kaasamine kasutab MPNS, et see reegel kehtib ka kampaaniat, mis on loodud sees kaasamine ees.

> 2.9.3) Microsoft Push teatise teenus ei pruugi olla kasutada saata teatised, mis on ülesande kriitiline või muul viisil võib mõjutada elu või surma, sealhulgas, kuid teadetest piirangule seotud mobiilsideseadet või tingimus. MICROSOFT SARNASE MIS TAHES GARANTII, ET KASUTADA MICROSOFTI LÜKKETEATISTE TEENUS VÕI KOHALETOIMETAMISE MICROSOFT TÕUKETEATISTE TEATISE TEENUS ON KATKESTUSTETA, TÕRGE TASUTA VÕI MUUL VIISIL TAGADA TEKKIDA REAALAJAS ALUSEL.

**Me ei saa tagada rakenduse läheb kinnitamise protsessi, kui te pole neid soovitusi.**

##<a name="handle-data-push-optional"></a>Täitepide andmete push (valikuline)

Kui soovite oma rakenduse REACHi andmete sunnib vastu võtta, teil rakendada EngagementReach klassi sündmustest.

    EngagementReach.Instance.DataPushStringReceived += (body) =>
    {
       Debug.WriteLine("String data push message received: " + body);
       return true;
    };
    
    EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
    {
       Debug.WriteLine("Base64 data push message received: " + encodedBody);
       // Do something useful with decodedBody like updating an image view
       return true;
    };

Näete, et iga meetodi tagasihelistamise tagastas on kahendväärtus. Kaasamine saadab tagasiside andmiseks oma tagaandmebaas pärast andmete tõuketeatised. Kui soovitud tagasihelistamise tagastab väärtuse false, on `exit` on saada tagasisidet. Muul juhul oleks `action`. Kui pole tagasihelistamise on seatud sündmusi, on `drop` tagasiside tagastatakse allikaid.

> [AZURE.WARNING] Kaasamine ei ole võimalik saada hulgidiagrammid tagasiside eest andmete tõuketeatised. Kui soovite määrata mitu käitleja sündmuse, võtke arvesse, et tagasiside vastavad viimasele üks saadetakse. Sel juhul soovitatav alati tagastab sama väärtuse, klõpsake selle ees vältida segadust tagasiside.

##<a name="customize-ui-optional"></a>Kohandamine Kasutajaliidese (valikuline)

### <a name="first-step"></a>Esimene samm

Me võimaldavad kohandada REACHi UI.

Selleks on vaja luua alamklass, on `EngagementReachHandler` klassi.

**Proovi kood:**

    using Microsoft.Azure.Engagement;
    
    namespace Example
    {
       internal class ExampleReachHandler : EngagementReachHandler
       {
          // Override EngagementReachHandler methods depending on your needs
       }
    }

Määrake sisu on `EngagementReach.Instance.Handler` välja koos oma kohandatud objekti oma `App.xaml.cs` klassi sees on `Application_Launching` meetod.

**Proovi kood:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [AZURE.NOTE] Vaikimisi kasutab kaasamine enda rakendamine `EngagementReachHandler`. Teil pole vaja luua oma ja kui teete, pole teil alistada iga meetod. Vaikekäitumine on lingid base objekti valimiseks.

### <a name="layouts"></a>Paigutused

Vaikimisi kasutab REACHi manustatud ressursse dll teatised ja lehtede kuvamiseks.

Siiski saate otsustada, kas kasutada oma ressursse vastavalt oma tootemargile nende komponentide.

Saate alistada `EngagementReachHandler` meetodite kaasamine kasutada paigutuste öelda teie alamklassi:

**Proovi kood:**

    // In your subclass of EngagementReachHandler
    
    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetPollUri()
    {
       // return the path of your own xaml
    }
    
    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [AZURE.TIP] Funktsiooni `CreateNotification` meetod võib tagastada null. Teate ei kuvata ja REACHi turunduskampaania kõrvaldatakse.

Lihtsustada oma paigutuse rakendamist, pakume oma XAML-i, mis võib olla teie koodi alusena. Need asuvad lingid SDK arhiivi (/ src/REACHi /).

> [AZURE.WARNING] Pakume allikad on täpne sama need, mida me kasutame. Nii, et kui soovite muuta need otse, ärge unustage muuta nimeruumi ja nimi.

### <a name="notification-position"></a>Teatis asukohta

Vaikimisi kuvatakse teatis – rakenduse alumises vasakus servas rakendus. Saate muuta seda käitumist alistamine selle `GetNotificationPosition` meetod on `EngagementReachHandler` objekti.

    // In your subclass of EngagementReachHandler
    
    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

Praegu saate valida soovitud `BOTTOM` (vaikeväärtus) ja `TOP` positsioonide.

### <a name="launch-message"></a>Käivitage sõnum

Kui kasutaja klõpsab süsteemi teatis (töölauateatis), kaasamine käivitab rakenduse, laadimine tõuketeatised sõnumite sisu ja kuvada vastava turunduskampaania leht.

Rakenduse käivitamine ja kuvatava lehe (olenevalt teie võrgu kiiruse) vahel on viivitus.

Tähistamiseks kasutajale midagi on laadimise, peate esitama visuaalse teabe, nt edenemise riba või edenemise näidik. Kaasamine ei oska, et ise, kuid pakub mõne käitleja teie eest.

Tehke soovitud tagasihelistamise rakendada.

    /* The application has launched and the content is loading.
     * You should display an indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };
    
    /* The application has finished loading the content and the page
     * is about to be displayed.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };
    
    /* The content has been loaded, but an error has occurred.
     * You can provide an information to the user.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

Saate määrata selle tagasihelistamise oma `Application_Launching` meetodit oma `App.xaml.cs` parim enne faili selle `EngagementReach.Instance.Init()` kõne.

> [AZURE.TIP] Iga sündmuseohjuri nimetatakse UI lõime. Te ei pea muretsema, MessageBox või midagi seotud Kasutajaliidese kasutamisel.

[Rakenduste poliitikad]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[Lisanõuded konkreetse rakenduse tüübid]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx
 
