<properties 
    pageTitle="Kasutades .NET Media Services kontoga ühenduse" 
    description="See teema näitab, kuidas Media Servicesi žestid .NET ühendamiseks." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a>Kasutades .net-i Media Services SDK Media Services kontoga ühenduse

> [AZURE.SELECTOR]
- [ÜLEJÄÄNUD](media-services-rest-connect-programmatically.md)
- [.NET-I](media-services-dotnet-connect-programmatically.md)


Selles teemas kirjeldatakse, kuidas hankida Microsoft Azure Media Servicesi programmiline ühendus on kavandamise Media Services SDK .net-i jaoks.


## <a name="connecting-to-media-services"></a>Ühenduse Media Services

Ühenduse Media Servicesi programmiliselt, te peate varem häälestatud konto Azure, konfigureeritud Media Servicesi konto, ning seejärel nuppu Häälesta Media Services SDK arengu projekti Visual Studio .net-i jaoks. Lisateabe saamiseks lugege teemat häälestamine arengu Media Services SDK .net-i jaoks.

Media Services konto häälestamine protsessi lõpus hankisite nõutav ühendus järgmised väärtused. Nende abil saate teha Media Servicesi programmiline ühendused.

- Media Servicesi konto nimi.

- Media Servicesi konto võti.

Nende väärtuste leidmiseks minge Azure'i haldamine portaalis, valige oma meediumi teenusekonto ja portaali akna allservas "**Klahve haldamine**" ikooni. Iga tekstivälja kõrval olevat ikooni klõpsamisel kopeerib väärtuse süsteemi lõikelauale.


## <a name="creating-a-cloudmediacontext-instance"></a>CloudMediaContext eksemplari loomine

Media Servicesi suhtes programmeerimise alustamiseks peate looma **CloudMediaContext** eksemplari, mis tähistab Serveri konteksti. **CloudMediaContext** sisaldab viiteid tööde haldamine, varad, failid, juurdepääsupoliitikaid ja lokaatorid olulised saidikogumid.

>[AZURE.NOTE] Klassi **CloudMediaContext** ei ole jutulõnga ohutu. Peaksite looma uue CloudMediaContext, teema või toimingute kohta.


CloudMediaContext on viis ehitaja ülekoormuse. Soovitatav on kasutada ehitajatel, mis võtavad **MediaServicesCredentials** parameetrina. Lisateavet **Diagrammide korduvkasutamine juurdepääsu juhtimine teenuse sõned** järgneva. 

Järgmises näites kasutatakse avaliku CloudMediaContext(MediaServicesCredentials credentials) ehitaja:

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Diagrammide korduvkasutamine juurdepääsu juhtimine teenuse sõned

Selles jaotises kirjeldatakse lepingute uuesti kasutada juurdepääsu juhtimine teenuse sõned CloudMediaContext ehitajatel, mis võtavad MediaServicesCredentials parameetrina abil.


[Azure Active Directory juurdepääsu reguleerimine](https://msdn.microsoft.com/library/hh147631.aspx) (nimetatakse ka juurdepääsu juhtimine teenuse või ACS) on pilvepõhist teenust, mida on lihtne viis autentimist ja lubada kasutajatel juurde pääseda veebirakendustega. Microsoft Azure Media Servicesi kontrollib juurdepääsu oma teenuste küll OAuthi protokolli, mis nõuab mõne ACS luba. Media Servicesi saab ACS sõned loa serveris.

Media Services SDK koos töötades saate käsitle märkide Kuna SDK kood juhtide jaoks neile. Siiski lastes täielikult hallata ACS sõned SDK viib mittevajalike Turbeloa taotlused. Paluda sõned võtab aega ja tarbib kliendi ja serveri ressursse. Lisaks ACS server ahendab taotlused kui määr on liiga suur. On 30 taotlusi teise kohta leiate teemast [ACS teenusepiirangud](https://msdn.microsoft.com/library/gg185909.aspx) rohkem üksikasju.

Alustades Media Services SDK versiooni 3.0.0.0, saate kasutada ACS sõned. ACS märkide vahel mitme kontekstides ühiskasutuse lubamine **CloudMediaContext** ehitajatel, mis võtavad **MediaServicesCredentials** parameetrina. Klassi MediaServicesCredentials kapseloi Media Servicesi mandaat. Kui mõni ACS luba on saadaval ja selle aegumise aeg on teada, saate luua uue eksemplari MediaServicesCredentials luba ja andke seda CloudMediaContext ehitaja. Pange tähele, et Media Services SDK automaatselt värskendab sõned iga kord, kui need aeguvad. Nagu on näidatud allpool toodud näidetes on kaks võimalust ACS märkide uuesti kasutada.

- Saate vahemälu **MediaServicesCredentials** objekti (nt staatilise klassi muutuja) mälu. Seejärel edastama vahemällu talletatud objekti CloudMediaContext ehitaja. MediaServicesCredentials objekt sisaldab ACS luba, mida saab uuesti kasutada, kui see on kehtiv. Kui luba ei sobi, värskendatakse selle Media Services SDK MediaServicesCredentials ehitaja antud mandaadi abil.

    Pange tähele, et **MediaServicesCredentials** objekti saab pärast selle RefreshToken nimetatakse kehtiv luba. **CloudMediaContext** kõned ehitaja **RefreshToken** meetod. Kui kavatsete laiendatud Turbeloa väärtused salvestada, veenduge, et kontrollida, kas TokenExpiration väärtus on lubatud enne salvestamist Turbeloa andmed. Kui see ei sobi, helistage RefreshToken enne vahemällu.

        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        
        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }
        
        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

- Te saate ka vahemälu AccessToken stringi ja TokenExpiration väärtused. Väärtused hiljem saab luua uue MediaServicesCredentials objekti Turbeloa vahemällu talletatud andmetega.  See on eriti kasulik stsenaariumid, kus luba saab turvaliselt jagada mitme protsesside või arvutite vahel.

    Järgmised Koodilõigud helistage SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage ja UpdateTokenDataInExternalStorageIfNeeded meetodid, mis on määratletud selles näites. Saate määratleda talletada, tuua ja laiendatud Turbeloa andmete värskendamine järgmised võimalused. 

        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
        
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
        
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
        
    Salvestatud Turbeloa väärtused abil saate luua MediaServicesCredentials.


        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;
        
        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);
        
        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };
        
        CloudMediaContext context2 = new CloudMediaContext(credentials);

    Värskendage Turbeloa Kopeeri juhuks, kui luba värskendati Media Services SDK. 
    
        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }
        

- Kui teil on mitu Media Servicesi kontot (näiteks laadi ühiskasutuse taustvärvi või Geo-jaotuse) te saate vahemälu MediaServicesCredentials objektid, kasutades System.Collections.Concurrent.ConcurrentDictionary kogumise (ConcurrentDictionary saidikogumi tähistab kogumi jutulõnga ohutu /-väärtuse paarideks, millele pääseb samaaegselt mitu Teemad). Seejärel saate GetOrAdd meetodit vahemällu talletatud identimisteabe. 

        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();
        

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;
        
            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));
        
            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);
        
            return cloudMediaContext;
        }
        
## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a>Põhja-Hiina piirkonnas Media Servicesi kontoga ühenduse

Kui teie konto asub piirkonnas Põhja-Hiinas, kasutage järgmist ehitaja.

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Näiteks:


    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>Salvestamise ühenduse väärtuste konfigureerimine

See on väga soovitatav tava konfiguratsiooni ühenduse väärtused, näiteks konto nimi ja parool, eriti tundliku väärtuste talletamiseks. Samuti on soovitatav tundliku konfiguratsiooni andmete krüptimiseks. Funktsiooni Windows krüptimine süsteemi (EFS) abil saate krüptida kogu konfiguratsioonifail. Faili EFS lubamiseks paremklõpsake faili, valige **Atribuudid**ja vahekaardil **Täpsemalt** krüptimine. Samuti võite luua kohandatud lahenduse valitud osade konfigureerimise faili krüptimise abil kaitstud konfigureerimine. Vt [krüptimise konfiguratsiooni teabe abil kaitstud konfigureerimine](https://msdn.microsoft.com/library/53tyfkaw.aspx).

Järgmine App.config fail sisaldab väärtusi, nõutav ühendus. Väärtused on <appSettings> element on nõutav väärtused, mis teil Media Servicesi konto häälestamise käigus.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


Konfiguratsiooni ühenduse väärtuste toomiseks saate kasutada **ConfigurationManager** klassi ja seejärel määrata väärtused väljad koodi:
    
    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
