<properties 
    pageTitle="Azure'i rakenduse teenuses Azure Redis vahemälu koos seansi olek" 
    description="Saate teada, kuidas kasutada Azure vahemälu teenuse toetamiseks ASP.net-i seansi olek vahemällu." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="none"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="06/27/2016" 
    ms.author="riande"/>


# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Azure'i rakenduse teenuses Azure Redis vahemälu koos seansi olek


See teema selgitab, kuidas kasutada Azure Redis vahemälu teenuse seansi olek.

Kui teie ASP.net-i veebirakenduse kasutab seansi olek, peate konfigureerimine välise seansi oleku pakkuja (Redis vahemälu teenuse või SQL serveri seansi oleku pakkuja). Kui kasutate seansi olek ning te ei kasuta välise teenusepakkuja, mida on piiratud ühe astme oma veebirakenduse. Redis vahemälu teenus on kiireim ja lihtsaim lubamiseks.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a id="createcache"></a>Vahemälu loomine
Järgige [neid juhiseid](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) vahemälu luua.

##<a id="configureproject"></a>Oma veebirakenduse RedisSessionStateProvider Nugeti pakett lisamine
Installimine on Nugeti `RedisSessionStateProvider` paketi.  Järgmise käsu abil saate installida paketi halduri konsooli (**Tööriistad** > **Nugeti Package Manager** > **Package Manager konsooli**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
**Tööriistade**kaudu installimiseks > **Nugeti Package Manager** > **Haldamine kullatükk pakettide lahenduse**otsimine `RedisSessionStateProvider`.

Lisateabe saamiseks vt [Nugeti RedisSessionStateProvider lehe](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ ) ja [vahemälu kliendi konfigureerimine](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

##<a id="configurewebconfig"></a>Fail muutmine
Lisaks vahemälu komplekti viited tegemist Nugeti pakett lisab konts kirjed *fail* . 

1. Avage *web.config* ja otsige üles soovitud **sessionState** element.

1. Sisestage soovitud väärtused `host`, `accessKey`, `port` (SSL-i pordi tuleks 6380), ja seadke `SSL` et `true`. Need väärtused saab teie vahemälu eksemplari keelest [Azure portaali](http://go.microsoft.com/fwlink/?LinkId=529715) . Lisateavet leiate teemast [ühenduse loomine vahemällu](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). Pange tähele, et pordi SSL on keelatud vaikimisi uue vahemälu. Pordi SSL lubamise kohta leiate lisateavet [konfigureerimine Azure'i Redis vahemälu vahemälu](https://msdn.microsoft.com/library/azure/dn793612.aspx) teema jaotisest [Juurdepääs pordid](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) . Järgmised märgistus kuvatakse *fail, spetsiaalselt muudatused *port*, *host*, accessKey* muudatused*, ja *SSL-i *.

          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;


##<a id="usesessionobject"></a>Kasutage seansi objekti kood
Viimane toiming on ASP.net-i koodi seansi objekti kasutama hakata. Saate lisada objektide seansi olek **Session.Add** meetodi abil. See meetod kasutab võti ja väärtuse paarideks talletamiseks üksuste vahemälus seansi olek.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

Järgmine kood toob selle väärtuse seansi olek.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

Samuti saate oma web Appis Redis vahemälu vahemälu objektidele. Lisateavet leiate teemast [MVC filmi rakenduse Azure'i Redis vahemälu 15 minutit](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
ASP.net-i seansi olek kasutamise kohta leiate lisateavet teemast [ASP.net-i seansi olek ülevaade][].

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="whats-changed"></a>Mis on muutunud
* Muuda juhend veebisaitide rakenduse teenusega leiate: [Azure'i rakendust Service ja selle mõju olemasoleva Azure'i teenused](http://go.microsoft.com/fwlink/?LinkId=529714)

  *[Rick Anderson](https://twitter.com/RickAndMSFT) järgi*
  
  [installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
  [ASP.net-i seansi olek ülevaade]: http://msdn.microsoft.com/library/ms178581.aspx

  [NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 
