<properties
    pageTitle="CORS toetamiseks rakenduse teenuse | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada CORS tugi Azure Azure'i rakendust Service."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/27/2016"
    ms.author="rachelap"/>

# <a name="consume-an-api-app-from-javascript-using-cors"></a>Kasutades CORS JavaScript API rakenduse kasutamine

Rakenduse teenus pakub [Rist Origin ressursside ühiskasutus (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), mis võimaldab helistada domeenide API-d, mis on majutatud API rakendustes JavaScripti tugi. Rakenduse teenuse abil saate ilma koodi kirjutamata oma API CORS juurdepääsu oma API konfigureerimine.

Selles artiklis on kaheosaline:

* [Kuidas konfigureerida CORS](#corsconfig) jaotises selgitatakse üldiselt konfigureerimine CORS API rakenduse, veebirakenduse, või mobiilirakenduse kaudu. Sama kehtib kõigi raamistik, mida toetavad rakendust Service, sh .net-i, Node.js ja Java. 

* Alustades [jätkuva töötamise alustamine .NET õpetused](#tutorialstart) jaotis, artikkel on õpetuse, mis demonstreerib CORS toetavad tuginedes, mida tegite [esimese API Apps kasutamise alustamine õpetuse](app-service-api-dotnet-get-started.md). 

## <a id="corsconfig"></a>Azure'i rakendust Service CORS konfigureerimine

Azure'i portaalis või [Azure ressursihaldur](../azure-resource-manager/resource-group-overview.md) tööriistade abil saate konfigureerida CORS.

#### <a name="configure-cors-in-the-azure-portal"></a>Azure'i portaalis CORS konfigureerimine

8. Minge brauseris [Azure portaali](https://portal.azure.com/).

2. Klõpsake **Rakenduse teenuste**ja klõpsake API rakenduse nimi.

    ![Valige API rakenduse portaalis](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. Paremal **API rakenduse** tera avanevas **sätete** tera, otsige üles jaotis **API** , ja klõpsake **CORS**.

    ![Valige sätted blade CORS](./media/app-service-api-cors-consume-javascript/clicksettings.png)

11. Teksti, sisestage väljale URL-i või URL-id, mida soovite lubada JavaScripti kõned pärit.


    Näiteks kui olete juurutanud JavaScripti rakenduse web appi, nimega todolistangular, sisestage "https://todolistangular.azurewebsites.net". Teise võimalusena võite sisestada tärn (*) määramaks, et aktsepteeritakse kõik origin domeenid.


13. Klõpsake nuppu **Salvesta**.

    ![Klõpsake nuppu Salvesta](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Pärast nupu **Salvesta**klõpsamist aktsepteerib API rakenduse JavaScripti kõned määratud URL-ist.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>Azure'i ressursihaldur tööriistadega CORS konfigureerimine

Saate ka konfigureerida CORS API rakenduse käsurea tööriistu, nt [Azure PowerShelli](../powershell-install-configure.md) ja [Azure CLI](../xplat-cli-install.md) [Azure'i ressursihaldur mallide](../resource-group-authoring-templates.md) abil. 

Azure'i ressursihaldur Mall, mis määrab atribuudi CORS näide [selle õpetuse valimi rakenduse hoidla azuredeploy.json faili](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json)avada. Otsige üles jaotis malli, mis näeb välja nagu järgmises näites:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a id="tutorialstart"></a>Jätkuva .NET õpetuse töötamise alustamine

Kui teil on pärast Node.js või Java töötamise alustamine sarja API rakendused, on lõpule viidud saamisega alustamine sarja. Minge [järgmised toimingud](#next-steps) jaotise leidmiseks soovitused edasi õppida API rakenduste kohta.

Käesoleva artikli ülejäänud .NET töötamise alustamine sarja jätkamise ja eeldab edukalt lõpule viinud [esimese õpetuse](app-service-api-dotnet-get-started.md).

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a>Uue veebirakenduse ToDoListAngular projekti juurutamiseks

[Esimese õpetuse](app-service-api-dotnet-get-started.md)loodud keskmisel taseme API rakenduse ja andmete taseme API rakenduse. Selles õpetuses loote ühe lehe rakenduse (SPA) web appi kõned keskmisel taseme API rakenduse. Saate töötada Spa tuleb lubada CORS keskmisel taseme API rakenduse. 

[ToDoList proovi taotluse](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list)ToDoListAngular projekt on lihtne AngularJS klient, mis nõuab teise eesnime esimese taseme ToDoListAPI Veebiteenuste projekti. JavaScripti koodi *app/scripts/todoListSvc.js* faili nõuab API abil AngularJS HTTP pakkuja. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 
        
            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a>Projekti ToDoListAngular uue veebirakenduse loomine

Uue rakenduse teenuse veebirakenduse loomine ja juurutamine projekti selle protseduuri on sarnane kuvatud [loomine](app-service-api-dotnet-get-started.md#createapiapp)ja selle sarja esimene õpetuses API rakenduse juurutamine. Ainus erinevus on tippige rakenduse **Web Appi** asemel **API rakenduse**.  Kuvatõmmised on dialoogid, vaadake teemat 

1. Paremklõpsake ToDoListAngular projekti **Solution Exploreris**, ja seejärel klõpsake nuppu **Avalda**.

3.  Klõpsake vahekaardi **profiil** **Veebis avaldamine** viisardi **Microsoft Azure'i rakendust Service**.

5. Klõpsake dialoogiboksis **Rakenduse teenuse** nuppu **Uus**.

3. Sisestage dialoogiboksi **Loomine rakenduse teenuse** **Hosting** menüü **Web Appi nimi** , mis on kordumatu *azurewebsites.net* domeeni. 

5. Valige Azure'i **tellimus** , mida soovite töötada.

6. Valige ripploendist **Ressursirühm** varem loodud sama ressursirühma.

4. Valige ripploendist **Rakenduse teenusleping** sama lepingu varem loodud. 

7. Klõpsake nuppu **Loo**.

    Visual Studio loob veebirakenduse, loob avalda profiili selle jaoks, ja kuvab **ühenduse** etappi viisardi **Avaldamine veebis** .

    Ärge veel klõpsake nuppu **Avalda** . Saate konfigureerida järgmises jaotises uue veebirakenduse helistamiseks keskmisel taseme API rakendust teenuses rakendus töötab. 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a>Teise eesnime, teise URL-i web appi sätete määramine

1. [Azure portaali](https://portal.azure.com/), ja seejärel liikuge web app loodud majutada TodoListAngular (esiosa) project **Web App** tera.

2. Klõpsake **Sätted > rakenduse sätted**.

3. Lisage jaotises **rakenduse sätted** järgmised võti ja väärtus.

  	|Klahv|Väärtus|Näide
  	|---|---|---|
  	|toDoListAPIURL|https://{Your keskel taseme API rakenduse nimi} .azurewebsites .net|https://todolistapi0121.azurewebsites.net|

4. Klõpsake nuppu **Salvesta**.

    Kui kood töötab Azure, alistab see väärtus localhost URL, kus *fail* on. 

    Koodi, mis toob sätte väärtus on *index.cshtml*:

        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>

    Koodi *todoListSvc.js* kasutab seda sätet.

        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a>Juurutage uus web appi ToDoListAngular web projekt

*  Visual Studio **Veebis avaldamine** viisardi juhises **ühenduse** nuppu **Avalda**.

    Visual Studio juurutamine ToDoListAngular projekti uue veebirakenduse ja avatakse brauseris veebirakenduse URL. 

### <a name="test-the-application-without-cors-enabled"></a>Rakenduse testimine ilma CORS lubatud 

2. Avage brauseris Arendaja tööriistad konsooli aken.

3. Kuvab AngularJS UI brauseriaknas linki **Loendite** .

    Kõne teise eesnime esimese taseme API rakenduse proovib JavaScripti koodi, kuid kõne nurjub, kuna esiosa töötab domeenil kui tagasi. Arendaja tööriistad konsooli brauseriaknas kuvatakse rist-origin tõrketeade.

    ![Rist-origin tõrketeade](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a>Teise eesnime taseme API rakenduse CORS konfigureerimine

Selles jaotises saate konfigureerida CORS sätet Azure teise eesnime esimese taseme ToDoListAPI API rakenduse. See säte võimaldab keskmisel taseme API rakenduse abil loodud ToDoListAngular project web Appis JavaScripti kõnesid vastu võtta.

8. Avage brauseris, [Azure portaali](https://portal.azure.com/).

2. Klõpsake **Rakenduse teenused**ja klõpsake ToDoListAPI (keskmisel taseme) API rakendus.

    ![Valige API rakenduse portaalis](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. Paremal **API rakenduse** tera avanevas **sätete** tera, otsige üles jaotis **API** , ja klõpsake **CORS**.

    ![Valige CORS portaalis](./media/app-service-api-cors-consume-javascript/clicksettings.png)

12. Tekstivälja, sisestage URL-i ToDoListAngular (esiosa) web appi. Näiteks kui olete juurutanud ToDoListAngular project web appi, nimega todolistangular0121, kõnede URL-i lubamine `https://todolistangular0121.azurewebsites.net`.

    Teise võimalusena võite sisestada määramaks, et kõik origin domeenide aktsepteeritakse tärn (*).

13. Klõpsake nuppu **Salvesta**.

    ![Klõpsake nuppu Salvesta](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Pärast nupu **Salvesta**klõpsamist aktsepteerib API rakenduse JavaScripti kõned määratud URL. Klõpsake selle kuvatõmmis aktsepteerib ToDoListAPI0223 API rakenduse JavaScripti kliendi kõnede ToDoListAngular web Appist.

### <a name="test-the-application-with-cors-enabled"></a>Testige rakenduse CORS lubatud

* Avage brauseris HTTPS URL-i veebirakenduse. 

    Seekord rakendus võimaldab teil vaatamine, lisamine, redigeerimine ja ülesandeloendi üksuste kustutamine. 

    ![Lehe näidis rakenduse Ülesandeloend](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>Rakenduse teenuse CORS võrreldes Web API CORS

Veebi-API projekti, saate installida [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) Nugeti pakett määramiseks koodi millist domeeni oma API aktsepteerib JavaScripti helistab kaudu.
 
Web API CORS tugi on paindlikumad rakenduse teenuse CORS tugi. Näiteks koodi saate määrata eri aktsepteeritud päritolu erinevate meetodite, ajal rakenduse teenuse CORS määrate aktsepteeritud päritolu kõigi API rakenduse meetodite kogumit.

> [AZURE.NOTE] Ei, proovige kasutada nii veebi-API CORS ja rakenduse teenuse CORS API rakenduses. Rakenduse teenuse CORS on ülimuslik ja veebi-API CORS ei mõjuta. Näiteks kui ühe origin domeeni rakenduse teenuses lubamine ja lubamine Web API-koodi origin domeenides, aktsepteerib Azure'i API rakenduse ainult kõned Azure määratud domeeni.

### <a name="how-to-enable-cors-in-web-api-code"></a>Kuidas lubada CORS Web API-koodi

Järgmised toimingud summarize protsessi Web API CORS toe lubamine. Lisateabe saamiseks vt [Lubamine rist-Origin taotlusi ASP.net-i veebi-API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

1. Installige Veebiteenuste Projectis [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) Nugeti pakett.

1. Kaasa on `config.EnableCors()` rida koodi **registreerida** meetodi **WebApiConfig** ainekursust, nagu järgmises näites. 

        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
                
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
    
                // Web API routes
                config.MapHttpAttributeRoutes();
    
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }

1. Veebi-API kontrolleril, lisage on `using` -lause on `System.Web.Http.Cors` nimeruumi, ja lisage soovitud `EnableCors` atribuut kontrolleril klassi või üksikud meetme meetodite. Järgmises näites, kehtib kogu domeenikontrolleri CORS tugi.

        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController
 
## <a name="using-azure-api-management-with-api-apps"></a>Azure'i API haldusega API rakendustega

Kui kasutate Azure API Management API rakendusega, konfigureerida CORS API halduse asemel API rakenduse. Lisateavet järgmistest teemadest:

* [Azure'i API halduse ülevaade (video: CORS algab 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [Rist poliitika API haldus](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)
 
## <a name="troubleshooting"></a>Tõrkeotsing

Juhuks, kui tekib probleem läbimise selles õpetuses, siis siin on mõned tõrkeotsingu ideid.

* Veenduge, et kasutate [Azure SDK Visual Studio 2015 .net-i](http://go.microsoft.com/fwlink/?linkid=518003)uusim versioon.

* Veenduge, et sisestasite `https` CORS säte, ja veenduge, et kasutate `https` veebi rakenduse käivitamiseks.

* Veenduge, kas sisestasite CORS säte keskmisel taseme API rakenduse, mitte veebi rakendus.

* Kui olete konfigureerinud CORS, nii rakenduse koodi ja Azure rakenduse teenust, võtke arvesse rakenduse teenuse CORS säte alistab kõik teete rakenduse koodi. 

Visual Studio funktsioonid, mis lihtsustada tõrkeotsingu kohta lisateabe saamiseks lugege teemat [tõrkeotsing Azure'i rakendust Service rakenduste Visual Studios](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Järgmised sammud 

Selles artiklis on kuvatud rakenduse teenuse CORS toe lubamine, et kliendi JavaScripti koodi saate helistada API domeenil. API rakenduste kohta lisateabe saamiseks vt [Sissejuhatus rakenduse teenuses autentimist](../app-service/app-service-authentication-overview.md)ja [Kasutaja autentimise API rakenduste](app-service-api-dotnet-user-principal-auth.md) õpetuse minge.
