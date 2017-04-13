<properties
    pageTitle="Veebi-API taustväärtus API haldus ja Azure Active Directory kaitsmise kohta"
    description="Saate teada, kuidas kaitsta veebi-API taustväärtus API haldus ja Azure Active Directory." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Veebi-API taustväärtus API haldus ja Azure Active Directory kaitsmise kohta

Järgmises videos näitab, kuidas koostada veebi-API taustväärtus ja kaitsmine OAuth 2.0 protokolli API haldus ja Azure Active Directory abil.  Selles artiklis antakse ülevaade ja Lisateavet video juhiseid. See 24 minutid video näitab, kuidas soovite:

-   Veebi-API taustväärtus koostamine ja kinnitage AAD – alates 1:30
-   API importimine API haldus – alates 7:10
-   Arendaja portaali helistamiseks API - alates 9:09 konfigureerimine
-   Helistamiseks API - alates 18:08 töölauarakendus konfigureerimine
-   JWT valideerimine poliitika kaitseme taotlused – alates 20:47 konfigureerimine

>[AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

## <a name="create-an-azure-ad-directory"></a>Mõni Azure AD kataloogi loomine

Kasutades Azure Active Directory, peate esmalt on tagatud turvamiseks veebi-API-ga on on AAD rentnik. Selles videos kasutatakse rentniku nimega **APIMDemo** . Rentnikku AAD loomiseks [Azure klassikaline portaali](https://manage.windowsazure.com) sisselogimine ja klõpsake nuppu **Uus**->**Rakenduse teenuste**->**Active Directory**->**Directory**->**Kohandatud loomine**. 

![Azure Active Directory][api-management-create-aad-menu]

Selles näites luuakse vaikimisi domeeni nimega **DemoAPIM.onmicrosoft.com** **APIMDemo** kataloogi. See kataloog kasutatakse kogu video.

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Azure Active Directory tagatud teenuse Veebiteenuste loomine

Selles etapis tuleb veebi-API taustväärtus abil loodud Visual Studio 2013. Selles etapis tuleb video algavad numbriga 1:30. Veebiteenuste loomiseks valige kirjutamata projekti Visual Studios **fail**->**Uus**->**projekti**, ja valige loendist **veebi** Mallid **ASP.net-i veebirakenduse** . See video nimega projekti **APIMAADDemo**. Klõpsake nuppu **OK** , et luua projekt. 

![Visual Studio][api-management-new-web-app]

Klõpsake **Veebi-API** alates selle **loendi malli valimine** Veebiteenuste projekti loomise kohta. Azure'i Directory autentimise konfigureerimine klõpsake nuppu **Muuda autentimist**.

![Uue projekti][api-management-new-project]

Klõpsake **Ettevõtte kontod**ja määrake oma AAD rentniku **domeeni** . Selles näites on selle domeeni **DemoAPIM.onmicrosoft.com**. Kataloogi domeeni saab kataloogi vahekaardil **Domeenid** .

![Domeenide][api-management-aad-domains]

Soovitud sätteid konfigureerida dialoogiboksis **Muuda autentimist** ja klõpsake nuppu **OK**.

![Muuda autentimine][api-management-change-authentication]

Nupu **OK** klõpsamisel Visual Studio proovib Azure AD kataloogi rakenduse registreerida ja võidakse teil paluda Visual Studio ja logige sisse. Logige sisse oma kataloogi haldus kontoga.

![Visual Studio sisse logida][api-management-sign-in-vidual-studio]

Kui soovite konfigureerida selle projekti Host **pilveteenuses** on Azure Web API märkige ruut ja seejärel klõpsake nuppu **OK**.

![Uue projekti][api-management-new-project-cloud]

Võidakse teilt Azure'i sisse logida ja seejärel saate konfigureerida Web App.

![Konfigureerimine][api-management-configure-web-app]

Selles näites on määratud uue **rakenduse teenusleping** nimega **APIMAADDemo** .

Klõpsake nuppu **OK** , et veebirakenduse konfigureerimine ja projekti loomine.

## <a name="add-the-code-to-the-web-api-project"></a>Project Web API koodi lisamine

Järgmiseks video lisab kood project Web API-ga. Selles etapis tuleb algab 4:35.

Selles näites Web API rakendatakse mudeli ja saanud selle domeenikontrolleri abil lihtsa kalkulaator teenuse. Mudeli teenuse lisamiseks paremklõpsake **Solution** Exploreris **mudelite** ja valige **Lisa**, **klassi**. Ainekursuse nime `CalcInput` ja klõpsake nuppu **Lisa**.

Lisage järgmine `using` lause ülaosas olevat `CalcInput.cs` faili.

    using Newtonsoft.Json;

 Asendage loodud klassi järgmine kood.

    public class CalcInput
    {
        [JsonProperty(PropertyName = "a")]
        public int a;

        [JsonProperty(PropertyName = "b")]
        public int b;
    }

Paremklõpsake **Solution** Exploreris **kontrollerid** ja valige käsk **Lisa**->**kontrolleril**. Valige **Web API 2 kontrolleril - Tühjenda** ja klõpsake nuppu **Lisa**. Tippige **CalcController** domeenikontrolleri nimi ja klõpsake nuppu **Lisa**.

![Selle domeenikontrolleri lisamine][api-management-add-controller]

Lisage järgmine `using` lause ülaosas olevat `CalcController.cs` faili.

    using System.IO;
    using System.Web;
    using APIMAADDemo.Models;

Asendage loodud kontrolleril klassi järgmine kood. Järgmine kood rakendab selle `Add`, `Subtract`, `Multiply`, ja `Divide` tavaline kalkulaator API toimingud.

    [Authorize]
    public class CalcController : ApiController
    {
        [Route("api/add")]
        [HttpGet]
        public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/sub")]
        [HttpGet]
        public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/mul")]
        [HttpGet]
        public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/div")]
        [HttpGet]
        public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }
    }

Vajutage klahvi **F6** koostamine ja veenduge, et lahendus.

## <a name="publish-the-project-to-azure"></a>Azure'i projekti avaldamine

Selles etapis tuleb Visual Studio projekti on avaldatud Azure. Selles etapis tuleb 5:45 video algab.

Projekti avaldamiseks Azure, paremklõpsake **APIMAADDemo** projekti Visual Studio ja valige käsk **Avalda**. Jätke vaikesätted dialoogiboksi **Veebis avaldamine** ja klõpsake nuppu **Avalda**.

![Veebis avaldamine][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>Azure AD kirjutamata teenuserakenduse õiguste andmine

Konfigureerimine ja avaldamise käigus Veebiteenuste projekti kataloogis Azure AD luuakse uus rakendus teenuse taustväärtus. Selles etapis tuleb video, alustades 6:13, veebi-API kirjutamata antakse õigused.

![Rakenduse][api-management-aad-backend-app]

Klõpsake rakenduse konfigureerimiseks vajalike õiguste nimi. Liikuge menüü **konfigureerimine** ja liikuge kerides jaotiseni **muude rakenduste õigused** . Klõpsake **Rakenduse õigused** ripploendis kõrval **Windows** **Azure Active Directory**, märkige ruut **directory andmete lugemine**ja klõpsake nuppu **Salvesta**.

![Õiguste lisamine][api-management-aad-add-permissions]

>[AZURE.NOTE] Kui **Windows** **Azure Active Directory** õiguste muudes rakendustes pole loendis, klõpsake käsku **Lisa rakendus** ja lisage see loendist.

Märkige üles **Rakendus Id URI** kasutamiseks järgmise sammuna Azure AD Rakenduse konfigureerimisel API arendaja haldusportaali.

![Rakenduse Id URI][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>Veebi-API importimine API haldus

API-d on konfigureeritud API Publisheri portaalis Azure klassikaline portaali kaudu avatavas. Publisheri portaali jõuda nuppu **Halda** API halduse teenust Azure klassikaline portaalis. Kui te pole veel loonud API halduse teenuse eksemplar, teemast [loomine eksemplari API halduse teenuse][] [haldamine oma esimese API][] õpetuses.

![Publisheri portaal][api-management-management-console]

Toimingud võivad olla [käsitsi lisatud API-d](api-management-howto-add-operations.md)või saab importida. Selles videos imporditakse ärplema vormingus alates 6:40 toimingud.

Looge fail nimega `calcapi.json` järgmised sisu ja salvestage see oma arvutisse. Tagada, et selle `host` atribuut points oma veebi-API taustväärtus. Selles näites `"host": "apimaaddemo.azurewebsites.net"` kasutatakse.

{"ärplema": "2.0", "teave": {"pealkiri": "Kalkulaator", "kirjeldus": "Arithmetics http kaudu!", "versioon": "1.0"}, "host": "apimaaddemo.azurewebsites.net", "basePath": "/ api", "skeemid": ["http"] "teed": {"/ lisada? on = {} ja b = {b}": {"get": {"kirjeldus": "Vastab summa kahe arvude.", "operationId": "Lisa kahe täisarvude", "parameetrid": [{"nimi": "a", "on": "päringu", "kirjeldus": "esimese operand. Vaikeväärtus on <code>51</code>. ","nõutavad": tõene,"vaikimisi":"51","kellaajavälju": ["51"]}, {"nimi":"b","on":"päringu","kirjeldus":"teine operand. Vaikeväärtus on <code>49</code>. ","nõutavad": tõene,"vaikimisi":"49","kellaajavälju": ["49"]}],"vastuste": {}}}," / sub?a = {a} & b = {b} ": {"get": {"kirjeldus":"Vastab vahe kahe arvude.","operationId":"Lahutamise kahe täisarvude","parameetrid": [{"nimi":"a","on":"päringu","kirjeldus":"esimese operand. Vaikeväärtus on <code>100</code>. ","nõutavad": tõene,"vaikimisi":"100","kellaajavälju": ["100"]}, {"nimi":"b","on":"päringu","kirjeldus":"teine operand. Vaikeväärtus on <code>50</code>. ","nõutavad": tõene,"vaikimisi":"50","kellaajavälju": ["50"]}],"vastuste": {}}}," / div?a = {a} & b = {b} ": {"get": {"kirjeldus":"Vastab jagatis kaks arvude.","operationId":"Divide kaks täisarvude","parameetrid": [{"nimi":"a","on":"päringu","kirjeldus":"esimese operand. Vaikeväärtus on <code>100</code>. ","nõutavad": tõene,"vaikimisi":"100","kellaajavälju": ["100"]}, {"nimi":"b","on":"päringu","kirjeldus":"teine operand. Vaikeväärtus on <code>20</code>. ","nõutavad": tõene,"vaikimisi":"20","kellaajavälju": ["20"]}],"vastuste": {}}}," / mul?a = {a} & b = {b} ": {"get": {"kirjeldus":"Vastab korrutis kahe arvude.","operationId":"Korruta kahe murdosa","parameetrid": [{"nimi":"a","on":"päringu","kirjeldus":"esimese operand. Vaikeväärtus on <code>20</code>. ","nõutavad": tõene,"vaikimisi":"20","kellaajavälju": ["20"]}, {"nimi":"b","on":"päringu","kirjeldus":"teine operand. Vaikeväärtus on <code>5</code>. ","nõutavad": tõene,"vaikimisi":"5","kellaajavälju": ["5"]}],"vastuste": {}}}}}

Importimiseks kalkulaator API, klõpsake vasakul menüüst **API halduse** **API-de** ja klõpsake **Importimine API -ga**.

![API importimise nupp][api-management-import-api]

Järgmiste toimingute konfigureerimiseks kalkulaator API.

1. Klõpsake valikut **failist**, liikuge sirvides soovitud `calculator.json` salvestatud fail ja klõpsake suvandinuppu **ärplema** .
2. Tippige **calc** **Web API URL-i järelliite** tekstiväli.
3. Klõpsake väljal **tooted (valikuline)** ja valige **Starter**.
4. Klõpsake nuppu **Salvesta** importimine API.

![Lisage uus API][api-management-import-new-api]

Pärast importimist API API kokkuvõtte leht kuvatakse Publisheri portaalis.

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>Kõne API edutult portaalist arendaja

Selles etapis API imporditud API haldus, kuid ei saa veel nimetada edukalt portaalist arendaja Kuna kirjutamata teenus on kaitstud Azure AD autentimist. Seda näitab videos alates 7:40 täitke järgmised juhised.

Klõpsake paremas servas Publisheri portaali **arendaja portaalis** .

![Portaali arendaja][api-management-developer-portal-menu]

**API-d** ja seejärel klõpsake **kalkulaator** API.

![Portaali arendaja][api-management-dev-portal-apis]

Klõpsake **seda proovida**.

![Proovige järele][api-management-dev-portal-try-it]

Klõpsake nuppu **saada** ja pange tähele **401 volitamata**vastuse olek.

![Saatmine][api-management-dev-portal-send-401]

Taotluse puudub, kuna taustväärtus API on kaitstud Azure Active Directory. Enne edukalt helistamiseks API arendaja portaali tuleb konfigureerida lubada arendajate OAuthi 2.0 abil. Selle protsessi on kirjeldatud järgmistes lõikudes.

## <a name="register-the-developer-portal-as-an-aad-application"></a>Arendaja portaali registreerida AAD rakendusena

Esimene samm konfigureerimise arendajate OAuthi 2.0 abil lubada portaali arendaja on registreerida arendaja portaali AAD rakendusena. Seda näitab alates 8:27 video.

Liikuge Azure AD rentniku esimesel etapil seda videot selles näites **APIMDemo** kaudu ja liikuge **rakendused** vahekaardil.

![Uus rakendus][api-management-aad-new-application-devportal]

Klõpsake nuppu **Lisa** uus Azure Active Directory rakenduse loomiseks ja valige **oma ettevõttes areneb rakenduse lisamine**.

![Uus rakendus][api-management-new-aad-application-menu]

Valige **veebirakenduse ja/või veebi-API**, sisestage nimi ja järgmise noolenuppu. Selles näites kasutatakse **APIMDeveloperPortal** .

![Uus rakendus][api-management-aad-new-application-devportal-1]

**Sisselogimise URL** sisestage URL-i API halduse teenust ja lisa `/signin`. Selles näites kasutatakse **https://contoso5.portal.azure-api.net/signin **.

**Rakenduse Id URL** sisestage URL-i API halduse teenust ja mõned kordumatud märgid. Need võivad olla mis tahes soovitud sümbolid ja selles näites kasutatakse **https://contoso5.portal.azure-api.net/dp** . Kui soovitud **rakenduse atribuudid** on konfigureeritud, klõpsake nuppu märge rakenduse loomiseks.

![Uus rakendus][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>API halduse OAuth 2.0 loa serveris konfigureerimine

Järgmiseks on konfigureerimine OAuth 2.0 loa serveris API haldus. See toiming on näidatud 9:43 alates video.

Klõpsake vasakul menüüst API haldus **turvalisuse** , klõpsake **OAuth 2.0**ja seejärel klõpsake käsku **Lisa autoriseerimine** server.

![Luba serveri lisamine][api-management-add-authorization-server]

Sisestage **nimi** ja **Kirjeldus** väljadele nimi ja soovi korral ka kirjeldus. Neid välju kasutatakse tuvastamiseks OAuth 2.0 autoriseerimine serveri eksemplari API halduse sees. Selles näites kasutatakse **autoriseerimine server demo** . Hiljem kui määrate OAuth 2.0 serveriga autentimise API kasutada, valite selle nime.

**Kliendi registreerimise lehe URL** Sisestage näiteks kohatäite väärtus `http://localhost`.  **Kliendi registreerimise lehe URL-i** suunab lehele, millega kasutajad saavad luua ja OAuth 2.0 kasutajahalduse kontod toetavad teenusepakkujad oma kontode konfigureerimine. Selles näites kasutajate loomine ja nii, et kasutatakse kohatäite oma kontode konfigureerimine.

![Luba serveri lisamine][api-management-add-authorization-server-1]

Järgmisena Määrake **autoriseerimine lõpp-punkti URL-i** ja **sümboolne lõpp-punkti URL-i**.

![Autoriseerimine server][api-management-add-authorization-server-1a]

Need väärtused saab tuua AAD rakenduse arendaja portaali loodud lehelt **Rakenduse lõpp-punktid** . Lõpp-punktid liikuge juurdepääsu **konfigureerimine** sakki AAD rakendus ja klõpsake nuppu **Kuva lõpp-punktid**.

![Rakenduse][api-management-aad-devportal-application]

![Vaate lõpp-punktid][api-management-aad-view-endpoints]

**Lõpp-punkti OAuth 2.0 autoriseerimine** kopeerige ja kleepige **URL-i autoriseerimine lõpp-punkti** tekstiväli.

![Luba serveri lisamine][api-management-add-authorization-server-2]

**OAuth 2.0 Turbeloa lõpp-punkti** kopeerige ja kleepige see **Luba lõpp-punkti URL-i** tekstiväli.

![Luba serveri lisamine][api-management-add-authorization-server-2a]

Lisaks Turbeloa lõpp-punkti kleepida, lisada on täiendavad keha parameeter nimega **ressursside** ja väärtuse kasutada **Rakenduse Id URI** rakendusest AAD taustväärtus teenus, mis on loodud, millal ilmus Visual Studio projekti.

![Rakenduse Id URI][api-management-aad-sso-uri]

Järgmine, määrake kliendi mandaat. Need on soovitud juurdepääsu, sellisel juhul kirjutamata service ressursi korral kasutatav mandaat.

![Kliendi identimisteave][api-management-client-credentials]

**Kliendi Id**saamiseks liikuge AAD taotluse kirjutamata teenuse kohta vahekaarti **konfigureerimine** ja kopeerige **Kliendi Id**.

Saada **Kliendi salajane** klõpsake **Valige kestus** drop-down **klahvid** jaotises ja määrake intervalli. Selles näites kasutatakse 1 aasta.

![Kliendi ID][api-management-aad-client-id]

Klõpsake nuppu **Salvesta** salvestada konfiguratsiooni ja kuvamiseks klahvi. 

>[AZURE.IMPORTANT] Märkige üles see võti. Kui Azure Active Directory konfiguratsiooni akna sulgemiseks klahvi ei saa kuvada uuesti.

Võti kopeerimine lõikelauale, aktiveerige Publisheri portaali, kleepida võti **Kliendi salajane** tekstiväli ja klõpsake nuppu **Salvesta**.

![Luba serveri lisamine][api-management-add-authorization-server-3]

Kohe pärast kliendi identimisteave on on kood loa andmine. Kopeerige see autoriseerimine kood ja uuesti aktiveerimiseks Azure AD arendaja portaali rakenduse konfigureerimine lehe ja kleebi loa andmine **Vasta URL** väljale, ja klõpsake nuppu **Salvesta** uuesti.

![Vastus URL-i][api-management-aad-reply-url]

Järgmiseks on arendaja portaali AAD rakenduse õiguste konfigureerimine. Klõpsake **Rakenduse õiguste** ja märkige ruut **Loe kataloog andmete**jaoks. Klõpsake nuppu **Salvesta** selle muudatuse salvestamiseks ja seejärel nuppu **Lisa rakendus**.

![Õiguste lisamine][api-management-add-devportal-permissions]

Klõpsake ikooni Otsi, **APIM** tipite väljale algab valige **APIMAADDemo**ja klõpsake märkeruutu salvestada.

![Õiguste lisamine][api-management-aad-add-app-permissions]

Klõpsake **APIMAADDemo** **Delegeeritud õiguste** ja **Accessi APIMAADDemo**märkeruut ja klõpsake nuppu **Salvesta**. See võimaldab arendaja portaali rakenduse teenuse taustväärtus.

![Õiguste lisamine][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>Luba OAuth 2.0 Kasutaja autoriseerimine API kalkulaator

Nüüd, kui OAuth 2.0 server on konfigureeritud, saate selle määrata oma API turbesätete. Selles etapis tuleb näitab videos alates 14:30.

Klõpsake vasakpoolses menüüs **API-d** , ja klõpsake nuppu **kalkulaator** vaadata ja selle sätete konfigureerimine.

![API kalkulaator][api-management-calc-api]

Liikuge vahekaardil **Turvalisus** , märkige ruut **OAuth 2.0** , valige **Luba serveri** ripploendist soovitud autoriseerimine server ja klõpsake nuppu **Salvesta**.

![API kalkulaator][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>Kõne edukalt kalkulaator API portaalist arendaja

Nüüd, kui OAuth 2.0 autoriseerimine konfigureerimist API, tegevuse saab edukalt helistada Arenduskeskuse linki. Selles etapis tuleb näitab videos alates 15:00.

Tagasi liikuda kalkulaator teenuse arendaja portaalis toimingu **Lisa kaks täisarvude** ja klõpsake **seda proovida**. Märkus uue üksuse **Luba** jaotise vastav autoriseerimine server te just lisasite.

![API kalkulaator][api-management-calc-authorization-server]

Valige **Luba kood** ripploendist autoriseerimine ja sisestage mandaat konto, mida soovite kasutada. Kui teil on juba sisse logitud ei võidakse teilt kontoga.

![API kalkulaator][api-management-devportal-authorization-code]

Klõpsake nuppu **saada** ja pange tähele **vastuse olek** **200 OK** ja nende tulemused toimingu vastuse sisu.

![API kalkulaator][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>Helistamiseks API töölauarakendus konfigureerimine

Järgmise toimingu video 16:30 algab ja konfigureerib helistamiseks API lihtsa töölauarakendus. Esimene samm on registreerida töölauarakenduses Azure AD ning sellele juurdepääsu kataloogiga ja kirjutamata teenusega. Tagasi: on tutvustava helistamiseks kalkulaator API toimingu töölauarakenduses.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>JWT valideerimine poliitika kaitseme taotlusi konfigureerimine

Lõplik protseduuri video 20:48 algab ja näidatakse, kuidas eelnevalt lubada taotluste kontrollimine iga sissetuleva taotluse Accessi sõned [Valideerimiseks JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) poliitika abil. Kui kutse on kinnitatud, kinnitage JWT poliitika, kutse on blokeerinud API haldus ja on edasi mööda soovitud taustväärtus.

    <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
        <openid-config url="https://login.windows.net/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
        <required-claims>
            <claim name="aud">
                <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
            </claim>
        </required-claims>
    </validate-jwt>

Üks tutvustamise konfigureerimine ja kasutamine selle poliitika, lugege teemat [Cloud kataks episood 177: rohkem API haldusfunktsioonid](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ja edasikerimine 13:50. 18:50 tutvustamise toimingu helistades arendaja portaali ja ilma nõutav luba luba 15:00, et näha millised poliitikad soovite rakendada poliitika redaktoris konfigureeritud ja seejärel edasi.

## <a name="next-steps"></a>Järgmised sammud
-   Vaadake veel [videod](https://azure.microsoft.com/documentation/videos/index/?services=api-management) API haldustoimingute kohta.
-   Muud võimalused kirjutamata teenust secure kohta leiate teemast [vastastikune serdi autentimist](api-management-howto-mutual-certificates.md) ja [ühendamine VPN- või ExpressRoute kaudu](api-management-howto-setup-vpn.md).

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[API halduse teenuse eksemplari loomine]: api-management-get-started.md#create-service-instance
[Oma esimese API haldamine]: api-management-get-started.md
