<properties
    pageTitle="Alustamine API rakendused ja ASP.net-i rakenduse teenus | Microsoft Azure'i"
    description="Saate teada, kuidas luua, juurutada ja tarbimine rakenduse ASP.net-i API teenuses Azure rakenduse Visual Studio 2015 abil."
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
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="rachelap"/>

# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>API rakendused, ASP.net-i ja ärplema teenuses Azure rakenduse kasutamise alustamine

[AZURE.INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

See on esimene rida õpetusi, mis näitab, kuidas kasutada Azure'i rakendust Service funktsioone, mis on abiks arendamise ja majutuse rahulik API.  Selles õpetuses käsitletakse tugi API metaandmete ärplema vormingus.

Saate teada:

* Kuidas luua ja juurutada [API rakendused](app-service-api-apps-why-best-platform.md) teenuses Azure rakenduse Visual Studio 2015 sisse ehitatud tööriistade abil.
* Kuidas abil Swashbuckle Nugeti paketi loomiseks dünaamiliselt ärplema API metaandmete API discovery automatiseerida.
* Kuidas kasutada metaandmete ärplema API automaatselt genereerida kliendi koodi API rakenduse.

## <a name="sample-application-overview"></a>Valimi rakenduse ülevaade

Selles õppetükis saate koostööd lihtne ülesannete loendi valimi rakenduse. Taotlus on ühe lehe rakenduse (SPA) vastendamisel, mõne teise eesnime taseme ASP.net-i veebi-API ja mõne ASP.net-i veebi-API andmete taseme.

![API rakenduste valimi rakenduse skeem](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Siin on [AngularJS](https://angularjs.org/) esiosa kuvatõmmis.

![Kuulake API rakendused rakendus Ülesandeloend](./media/app-service-api-dotnet-get-started/todospa.png)

Visual Studio lahendus sisaldab kolme projekti.

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** - esiosa: AngularJS SPA, mis nõuab keskmisel taseme.

* **ToDoListAPI** - keskmisel taseme: ASP.net-i veebi-API projekti, mis nõuab andmete kiht ülesandeloendi üksuste CRUD toiminguid.

* **ToDoListDataAPI** - andmete taseme: ASP.net-i veebi-API projekti, mis CRUD tehete ülesandeloendi üksuste kohta.

Kolme esimese arhitektuur on üks mitme arhitektuurides, mida saate rakendada, kasutades API rakendused ja tutvustamise otstarbeks siin kasutatud. Iga taseme kood on sama lihtne kui võimalik, et näidata API Apps uusimaid funktsioone; näiteks andmete taseme kasutab serveri mälu asemel andmebaasi selle püsimine süsteem.

Lõpuleviimise selles õpetuses, siis on teil kaks Web API projekte üles ja töötab rakendustes rakenduse teenuse API pilveteenuses.

Järgmise õppeteema sarja kasutab SPA ots pilveteenusesse.

## <a name="prerequisites"></a>Eeltingimused

* ASP.net-i veebi-API - kuueosalisest juhiseid Oletame, et teil on põhiteadmisi töötamine Visual Studio ASP.net-i [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) .

* Azure'i konto - saate [avada Azure'i konto tasuta](/pricing/free-trial/?WT.mc_id=A261C142F) või [aktiveerimine Visual Studio abonendi eelised](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

    Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751). Olemas, saate kohe luua lühiajaline starter rakenduse App teenuses – **nõutav krediitkaarti**ja kohustusi.

* Visual Studio 2015 koos [Azure SDK.NET](https://azure.microsoft.com/downloads/archive-net-downloads/) - The SDK installib Visual Studio 2015 automaatselt, kui te pole seda veel.

    * Visual Studio, klõpsake nuppu Spikker -> Teave Microsoft Visual Studio ja veenduge, et "Azure'i rakendust Service tööriistad v2.9.1" või suurem on installitud.

    ![Azure'i rakenduse tööriistad vesion](./media/app-service-api-dotnet-get-started/apiversion.png)

    >[AZURE.NOTE] Sõltuvalt sellest, mitu SDK sõltuvused on juba teie arvutis, SDK installimine võib võtta kaua aega, minutid pool tundi või rohkem.

## <a name="download-the-sample-application"></a>Laadige rakendus näidis

1. Laadige alla [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) hoidla.

    Saate nuppu **Laadi alla ZIP** või klooni hoidla oma kohalikus arvutis.

2. Avage ToDoList lahenduse Visual Studio 2015 või 2013.
   1. Peate iga lahenduse usaldada.
        ![Turbehoiatus](./media/app-service-api-dotnet-get-started/securitywarning.png)

3. Lahendus (CTRL + SHIFT + B) taastamine Nugeti pakettide koostamine.

    Kui soovite näha rakenduse töös enne juurutamist, saate seda kohalikult käivitada. Veenduge, et ToDoListDataAPI on projekti käivitamine ja käivitage lahendus. Mida peaks oodata HTTP 403 viga brauseris.

## <a name="use-swagger-api-metadata-and-ui"></a>Kasutage ärplema API metaandmete ja kasutajaliides

Azure'i rakendust Service sisse ehitatud [ärplema](http://swagger.io/) 2.0 API metaandmete tugi. Iga API rakenduse saate määrata mõne URL-i lõpp-punkti, mis tagastab metaandmete API ärplema JSON-vormingus. Tagastatud selle lõpp-punkti metaandmeid saab luua kliendi kood.

ASP.net-i veebi-API mõnda projekti dünaamiliselt genereerida ärplema metaandmete [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) Nugeti paketi abil. Swashbuckle Nugeti pakett on juba installitud allalaaditud ToDoListDataAPI ja ToDoListAPI projektides.

Selle õpetuse jaotises loodud ärplema 2.0 metaandmete vaadata ja seejärel saate proovida test kasutajaliides, mis põhineb ärplema metaandmete.

1. Määrata käivitus projekt ToDoListDataAPI projekt (**mitte** ToDoListAPI projekt).

    ![Kui käivitus projekti määramine ToDoDataAPI](./media/app-service-api-dotnet-get-started/startupproject.png)

2. Vajutage klahvi F5 või klõpsake **silumine > Käivita silumine** projekti käivitamiseks silumine režiimis.

    Brauseris avatakse ja kuvatakse lehe HTTP 403 tõrge.

3. Lisage oma brauseri aadressiribale `swagger/docs/v1` lõppu rea- ja seejärel vajutage tagasi. (URL on `http://localhost:45914/swagger/docs/v1`.)

    See on vaike-URL, mis kasutavad Swashbuckle ärplema 2.0 JSON metaandmete jaoks API tagastamiseks.

    Kui kasutate Internet Explorerit, brauseri palub teil *v1.json* faili alla laadida.

    ![IE JSON metaandmete allalaadimine](./media/app-service-api-dotnet-get-started/iev1json.png)

    Kui kasutate Chrome'i, Firefoxi või serva, brauseris kuvatakse selle JSON brauseriaknas. Eri brauserite toime JSON teisiti, ja oma brauseriakna ei saa otsida täpselt nagu näide.

    ![JSON metaandmete Chrome'is](./media/app-service-api-dotnet-get-started/chromev1json.png)

    Järgmises näites kuvatakse esimene jaotis ärplema metaandmete API, määratluse toomine meetodi abil. Selle metaandmete on, mis juhib ärplema UI, mida kasutada järgmist ja kasutate selle õpetuse hiljem jaotises automaatselt genereerida kliendi koodi.

        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },

4. Sulgege brauser ja Visual Studio silumine lõpetada.

5. Projectis ToDoListDataAPI **Solution**Exploreris *App_Start\SwaggerConfig.cs* faili avamisel Kerige allapoole suvandini rea 174 ja kommenteerige välja järgmine kood.

        /*
            })
        .EnableSwaggerUi(c =>
            {
        */

    *SwaggerConfig.cs* fail on loodud projekti Swashbuckle paketi installimisel. Faili pakub mitmel viisil konfigureerida Swashbuckle.

    Koodi, mida te olete uncommented võimaldab ärplema UI, mida kasutada järgmist. Kui loote Veebiteenuste projekti API rakenduse project malli abil, järgmine kood on kommenteerinud vaikimisi turvalisuse huvides.

6. Projekti uuesti käivitada.

7. Lisage oma brauseri aadressiribale `swagger` lõppu rea- ja seejärel vajutage tagasi. (URL on `http://localhost:45914/swagger`.)

8. Kasutajaliidese ärplema lehe kuvamisel klõpsake **ToDoList** kuvamiseks viise.

    ![Ärplema UI saadaval meetodid](./media/app-service-api-dotnet-get-started/methods.png)

9. Klõpsake loendis nuppu esimese **saada** .

10. Sisestage jaotises **Parameetrid** tärni, kui väärtus on `owner` parameeter ja klõpsake seejärel nuppu **proovida**.

    Hiljem õpetused autentimise lisamisel annab keskmisel taseme tegelik kasutaja ID, kui soovite andmete esimese taseme. Nüüd, kõik tööülesanded on tärn nende omanik ID samal ajal, kui rakendus töötab ilma autentimine on lubatud.

    ![Proovi ärplema kasutajaliides](./media/app-service-api-dotnet-get-started/gettryitout1.png)

    Ärplema UI kõned ToDoList Get meetodit ja kuvab vastuse koodi ja JSON tulemusi.

    ![Ärplema UI proovi tulemused](./media/app-service-api-dotnet-get-started/gettryitout.png)

11. Klõpsake nuppu **Postita**ja seejärel märkige ruut jaotises **Mudeli skeemi**.

    Klõpsates mudeli skeemile prefills sisestamise kasti, kus saate määrata parameetri väärtus postituse meetod. (Kui see ei tööta Internet Exploreris, kasutada mõnda muud brauserit või käsitsi parameetri väärtuse sisestamine järgmise juhise juurde.)  

    ![Ärplema UI proovi postituse](./media/app-service-api-dotnet-get-started/post.png)

12. Muuta JSON, klõpsake selle `todo` parameetri sisestusmeetodi välja nii, et see näeb välja nagu järgmises näites või asendada oma tekstiga kirjeldus:

        {
          "ID": 2,
          "Description": "buy the dog a toy",
          "Owner": "*"
        }

13. Klõpsake **proovida**.

    ToDoList API tagastab HTTP 204 vastuse koodi, mis näitab edu.

14. Klõpsake esimest nuppu **saada** ja seejärel selle lehe jaotises nuppu **proovida** .

    Hangi meetod vastus sisaldab nüüd uue üksuse.

15. Valikuline: Proovige ka panna, kustutamine ja saada ID viisil.

16. Sulgege brauser ja Visual Studio silumine lõpetada.

ASP.net-i veebi-API projekte swashbuckle töötab. Kui soovite lisada projekti ärplema metaandmete genereerimine, installige lihtsalt Swashbuckle pakett.

>[AZURE.NOTE] Ärplema metaandmete sisaldab Ainuidentifikaator iga toimingu API. Vaikimisi võib Swashbuckle luua dubleeritud ärplema toimingu ID oma Web API kontrolleril meetodid. See juhtub siis, kui kontrolleril on ülekoormatud HTTP meetodid, `Get()` ja `Get(id)`. Ülekoormuse reageerimine kohta leiate teemast [kohandamine Swashbuckle genereeritud API määratlusi](app-service-api-dotnet-swashbuckle-customize.md). Kui loote Veebiteenuste projekti Visual Studios Azure'i API rakenduse malli abil, lisatakse kood, mis loob toiming kordumatu ID-d automaatselt *SwaggerConfig.cs* faili.  

## <a id="createapiapp"></a>Azure API rakenduse loomine ja koodi juurutamiseks

Selles jaotises saate kasutada Azure tööriistu, mis on integreeritud Visual Studio **Web avaldamine** viisardi Azure uue API rakenduse loomiseks. Seejärel juurutada ToDoListDataAPI projekti API uus rakendus ja kõne API käivitades ärplema UI.

1. Paremklõpsake ToDoListDataAPI projekti **Solution Exploreris**, ja seejärel klõpsake nuppu **Avalda**.

    ![Klõpsake nuppu Avalda Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu.png)

2.  Klõpsake **Veebis avaldamine** viisardi juhises **profiili** **Microsoft Azure'i rakendust Service**.

    ![Klõpsake rakenduse Azure teenuse avaldamine veebis](./media/app-service-api-dotnet-get-started/selectappservice.png)

3. Azure'i kontosse sisse logida, kui te pole seda juba teinud või värskendada oma kasutajanimi ja parool, kui need on aegunud.

4. Dialoogiboksis rakenduse teenuse valige Azure'i **tellimus** , mida soovite kasutada, ja seejärel nuppu **Uus**.

    ![Klõpsake nuppu Uus rakendus teenuse dialoogiboksis](./media/app-service-api-dotnet-get-started/clicknew.png)

    Kuvatakse dialoogiboks **Loomine rakenduse teenuse** **Hosting** vahekaarti.

    Kuna veebi-API projekt, mis on installitud Swashbuckle juurutamist Visual Studio eeldab, et soovite luua rakenduse API-ga. See on pealkiri **API rakenduse nime** järgi ja asjaolu, et ripploendist **Muuda tüüpi** on seatud **API rakenduse**märgitud.

    ![Rakenduse teenuse dialoogiboksis rakenduse tüüp](./media/app-service-api-dotnet-get-started/apptype.png)

5. Sisestage **API rakenduse nimi** on kordumatu *azurewebsites.net* domeeni. Saate aktsepteerida vaikenime, mis pakub Visual Studio.

    Kui sisestate nime, mille keegi teine on juba kasutanud, kuvatakse punane hüüumärk paremale.

    URL-i API rakenduse saab `{API app name}.azurewebsites.net`.

6. **Ressursirühm** ripploendis nuppu **Uus**ja sisestage "ToDoListGroup" või mõne muu nimi, soovi korral.

    Ressursirühma on Azure ressursse nagu API rakendused, andmebaasid, VMs, jne. Selles õpetuses on parim, kuna mis hõlbustab kustutada sammhaaval Azure ressursse, juhend on loodud uue ressursirühma loomiseks.

    See ruut saate valida olemasoleva [ressursirühm](../azure-resource-manager/resource-group-overview.md) või luua uue tippige nimi, mis erineb mis tahes olemasolevaid ressursirühm teie tellimus.

7. **Rakenduse teenuse leping** rippmenüüst kõrval nuppu **Uus** .

    Kuvatõmmise näitab Näidisväärtuste **API rakenduse nime**, **tellimus**ja **Ressursirühm** --oma väärtused teistsugused.

    ![Rakenduse teenuse dialoogiboksi loomine](./media/app-service-api-dotnet-get-started/createas.png)

    Järgmistes juhistes loote uue ressursirühma rakenduse teenusleping. Mõni rakendus teenusleping määrab Arvuta ressursse, mis töötab teie API rakenduse. Näiteks kui valite tasuta taseme, API rakenduse töötab ühiskasutusega VMs ajal mõned makstud astme töötab see sihtotstarbeline VMs. Rakenduse teenuse lepingute kohta leiate teemast [rakenduse teenuse lepingute ülevaade](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

8. Dialoogiboksis **Konfigureerida rakendus teenuse kavandamine** sisestage "ToDoListPlan" või mõne muu nime, kui eelistate.

9. Valige ripploendist **asukoht** asukoht, mis on teile.

    See säte määrab, mis töötavad rakenduse Azure andmekeskuse. Valige asukoht teie lähedal [latentsus](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)minimeerimiseks.

10. Klõpsake **suurus** rippmenüü **tasuta**.

    Selles õpetuses annab tasuta hinnakirjad taseme piisavalt jõudlust.

11. Klõpsake dialoogiboksis **Konfigureerida rakendus teenuse leping** nuppu **OK**.

    ![Klõpsake nuppu OK ning konfigureerimine rakenduse teenusleping](./media/app-service-api-dotnet-get-started/configasp.png)

12. Klõpsake dialoogiboksis **Rakenduse teenuse loomine** nuppu **Loo**.

    ![Klõpsake nuppu Loo loomine rakenduse teenuse dialoogiboksis](./media/app-service-api-dotnet-get-started/clickcreate.png)

    Visual Studio loob API rakendus ja avalda profiili, mis on kõik nõutavad API rakenduse sätted. Seejärel avab **Veebis avaldamine** viisardi, mida saate kasutada projekti juurutamine.

    **Veebis avaldamine** viisardi avatakse vahekaardil **ühendus** (vt allpool).

    Vahekaardil **ühendus** **serveri** ja **saidi nimi** sätted käsk API rakenduse. **Kasutajanimi** ja **parool** on juurutamise identimisteavet, mida Azure'i loob teie jaoks. Pärast juurutuse Visual Studio avatakse brauseris **Sihtkoha URL** (see on ainult otstarvet **Sihtkoha**URL-i).  

13. Klõpsake nuppu **edasi**.

    ![Klõpsake nuppu edasi ühenduse menüüs avalda veebis](./media/app-service-api-dotnet-get-started/connnext.png)

    Järgmise vahekaardi on vahekaart **sätted** (vt allpool). Siin saate muuta juurutamiseks silumine Koosta [Kaug](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)silumine vahekaarti Koosta konfigureerimine. Menüü pakub ka mitu **Faili Avaldamissuvandid**:

    * Täiendavate sihtkohta failide eemaldamine
    * Precompile ajal avaldamine
    * App_Data kausta failide välistamine

    Selle õpetuse te ei pea ühte järgmistest. Üksikasjalikumad selgitused, mida nad teevad, lugege teemat [kohta: juurutamine on Web projekti abil ühe klõpsuga avaldamine Visual Studios](https://msdn.microsoft.com/library/dd465337.aspx).

14. Klõpsake nuppu **edasi**.

    ![Klõpsake nuppu edasi sätted vahekaardil veebis avaldamine](./media/app-service-api-dotnet-get-started/settingsnext.png)

    Järgmine on vahekaarti **Eelvaade** (vt allpool), mis annab võimalust näha, millised failid kavatsete projekti API rakendusse kopeerida. Kui juurutamist projekti API rakendus, mida te juba juurutatud varasemas versioonis, kopeeritakse ainult muudetud failid. Kui soovite näha loendit mis kopeeritakse, saate klõpsata nuppu **Käivita eelvaade** .

15. Klõpsake nuppu **Avalda**.

    ![Klõpsake nuppu Avalda eelvaade menüüs avalda veebis](./media/app-service-api-dotnet-get-started/clickpublish.png)

    Uue API rakenduse juurutamine Visual Studio ToDoListDataAPI projekt. **Väljundi** aknas logib eduka juurutamise ja URL-i API rakenduse avada brauseriaknas kuvatakse leht "loodud".

    ![Juurutamise väljundi aknas](./media/app-service-api-dotnet-get-started/deploymentoutput.png)

    ![Uue API rakenduse loodud lehe](./media/app-service-api-dotnet-get-started/appcreated.png)

16. Lisada "ärplema" URL brauseri aadressiribale ja vajutage sisestusklahvi Enter. (URL on `http://{apiappname}.azurewebsites.net/swagger`.)

    Brauseris kuvatakse sama ärplema kasutajaliides, mis teile kuvati varasemas versioonis, kuid nüüd töötab pilveteenuses. Get meetodit proovida ja näete, et olete naasmiseks vaikimisi 2 ülesandeloendi üksuste. Kohalikus arvutis mälu salvestatud varem tehtud muudatused.

17. Avage [Azure'i portaalis](https://portal.azure.com/).

    Azure portaali on kasutajaliides web Azure ressursside API rakenduste haldamine.

18. Klõpsake **rohkem teenuseid > rakenduse teenuste**.

    ![Liikuge rakenduse teenused](./media/app-service-api-dotnet-get-started/browseas.png)

19. **Rakenduse teenuste** labale otsimine ja klõpsake nuppu Uus API rakendus. (Azure'i portaalis on avatud õige windows nimetatakse *labad*).

    ![Rakenduse teenuste blade](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)

    Avage kaks noad. Ühe tera on API rakenduse ülevaade ja üks on pikk loendi sätted, mida saate vaadata ja muuta.

20. Leidke jaotis **API** labale **sätted** ja klõpsake nuppu **API määratlus**.

    ![API määratlus rakenduses sätted blade](./media/app-service-api-dotnet-get-started/apidefinsettings.png)

    **API määratlus** tera võimaldab teil määrata URL, mis tagastab ärplema 2.0 metaandmete JSON-vormingus. Kui Visual Studio loob API rakendus, seda määrab API määratlus URL-i vaikeväärtus Swashbuckle loodud metaandmed, mis teile kuvati varasemas versioonis, mis on rakenduse API's base URL-i pluss `/swagger/docs/v1`.

    ![URL-i API määratlus](./media/app-service-api-dotnet-get-started/apidefurl.png)

    Kui valite genereerida kliendi koodi see rakendus API, toob Visual Studio metaandmete seda URL-i.

## <a id="codegen"></a>Luua andmete taseme kliendi koodi.

Üks eeliseid ärplema integreerimine Azure API rakendused on automaatne koodi loomine. Loodud kliendi tunnid oleks lihtsam kirjutada koodi, mis nõuab API rakendus.

ToDoListAPI projekt on juba loodud kliendi koodi, kuid Järgmistes juhistes saate kustutada ja taastada, et näha, kuidas koodi loomine.

1. Visual Studio **Solution Exploreris**Projectis ToDoListAPI *ToDoListDataAPI* kausta kustutada. **Hoiatus: Kustutada ainult kaust, mitte ToDoListDataAPI projekt.**

    ![Kustuta loodud kliendi kood](./media/app-service-api-dotnet-get-started/deletecodegen.png)

    See kaust on loodud, kasutades koodi genereerimine protsessi, mida olete läbida.

2. Paremklõpsake ToDoListAPI projekt, ja seejärel klõpsake **Lisa > REST API kliendi**.

    ![Visual Studio REST API kliendi lisamine](./media/app-service-api-dotnet-get-started/codegenmenu.png)

3. Dialoogiboksis **Lisa REST API kliendi** klõpsake **Ärplema URL-i**, ja klõpsake **Azure vara valimine**.

    ![Azure'i vara valimine](./media/app-service-api-dotnet-get-started/codegenbrowse.png)

4. Dialoogiboksis **Rakenduse teenuse** laiendamine ressursirühm kasutate selles õpetuses mõeldud ja valige oma API rakendus ja seejärel klõpsake nuppu **OK**.

    ![Valige API rakenduse koodi loomine](./media/app-service-api-dotnet-get-started/codegenselect.png)

    Pange tähele, et **Lisada REST API kliendi** dialoogiboksi naastes tekstiväli on täidetud sisse API määratluse URL-i väärtus, mis teile kuvati varem portaalis.

    ![URL-i API määratlus](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)

    >[AZURE.TIP] Sisestage URL-i asemel otse läbi dialoogiboksis sirvimine on alternatiivse metaandmete saamiseks koodi loomine. Või kui soovite luua kliendi koodi enne juurutamist Azure, võib käivitage project Web API kohalikult, avage URL, mis annab ärplema JSON-fail, salvestage fail ja **Valige olemasolev ärplema metaandmete faili** suvandi.

5. Klõpsake dialoogiboksis **REST API kliendi lisamine** nuppu **OK**.

    Visual Studio loob kausta API rakenduse nime ja loob kliendi tunnid.

    ![Koodi failide jaoks loodud](./media/app-service-api-dotnet-get-started/codegenfiles.png)

6. Avage Projectis ToDoListAPI *Controllers\ToDoListController.cs* 40 rida, mis nõuab API abil loodud kliendi koodi kuvamiseks.

    Järgmised koodilõigu näitab, kuidas koodi instantiates kliendi objekti ja nõuab Get meetod.

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }

        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }

    Parameetri ehitaja saab lõpp-punkti URL-i kaudu soovitud `toDoListDataAPIURL` rakenduse säte. Fail, klõpsake selle väärtuseks on seatud kohaliku IIS-i kiire URL-i API projekti nii, et käivitada rakenduse kohalikult. Kui jätate parameetri konstruktori, on vaikimisi lõpp-punkti URL-i teie loodud kood.

7. Kliendi tunni luuakse erineva nimega oma API rakenduse nime järgi; Tippige nimi vastab, mis on loodud projektis *Controllers\ToDoListController.cs* koodi muutmine Näiteks kui teie nimi oma API rakenduse ToDoListDataAPI071316, saate muuta järgmine kood:

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

Järgmine:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-to-host-the-middle-tier"></a>Teise eesnime taseme majutada API rakenduse loomine

Varasemate teie [loodud andmete taseme API rakendus ja juurutatud selle kood](#createapiapp).  Nüüd saate kasutage sama toimingut, teise eesnime taseme API rakenduse.

1. Paremklõpsake **Solution Exploreris**teise eesnime esimese taseme ToDoListAPI project (mitte soovitud andmete taseme ToDoListDataAPI), ja seejärel klõpsake nuppu **Avalda**.

    ![Klõpsake nuppu Avalda Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu2.png)

2.  Klõpsake vahekaardi **profiil** **Veebis avaldamine** viisardi **Microsoft Azure'i rakendust Service**.

3. Klõpsake dialoogiboksis **Rakenduse teenuse** nuppu **Uus**.

4. Dialoogiboks **Loomine rakenduse teenuse** **Hosting** vahekaarti, nõustuge vaikevalikuga **API rakenduse nimi** või sisestage nimi, mis on kordumatu *azurewebsites.net* domeeni.

5. Valige Azure **tellimust** te kasutate.

6. **Ressursirühm** ripploendis valige varem loodud sama ressursirühma.

7. Valige varem loodud sama lepingu **Rakenduse teenusleping** rippmenüü. See vaikimisi selle väärtuse.

8. Klõpsake nuppu **Loo**.

    Visual Studio loob API rakenduse, loob avalda profiili selle jaoks, ja kuvab **ühenduse** etappi viisardi **Avaldamine veebis** .

9.  **Veebis avaldamine** viisardi juhises **ühenduse** nuppu **Avalda**.

    Visual Studio juurutamine ToDoListAPI projekti uus API rakendus ja avatakse brauseris URL-i API rakenduse. Kuvatakse leht "edukalt loodud".

## <a name="configure-the-middle-tier-to-call-the-data-tier"></a>Teise eesnime kiht kõne andmete taseme konfigureerimine

Kui teise eesnime esimese taseme API rakenduse nüüd helistanud, seda proovida helistada andmete taseme, mis on endiselt fail localhost URL-i. Selles jaotises saate sisestage andmed esimese taseme API rakenduse URL on keskkonna säte keskmisel taseme API rakenduse. Kui teise eesnime esimese taseme API rakenduse koodi toob andmed taseme URL-i säte, alistab keskkonna säte, mis on fail.

1. [Azure portaali](https://portal.azure.com/)ja seejärel liikuge **API rakenduse** tera API rakenduse majutada TodoListAPI (keskmisel taseme) projekti loodud.

2. Klõpsake API rakenduse **sätted** labale **rakenduse sätted**.

3. API rakenduse **Rakenduse sätted** tera, liikuge kerides jaotiseni **rakenduse sätted** ja lisage järgmised võti ja väärtus. Väärtus on avaldatud selles õpetuses esimese API rakenduse URL.

  	| **Klahv** | toDoListDataAPIURL |
  	|---|---|
  	| **Väärtus** | https://{Your andmete taseme API rakenduse nimi} .azurewebsites .net |
  	| **Näide** | https://todolistdataapi.azurewebsites.net |

4. Klõpsake nuppu **Salvesta**.

    ![Klõpsake rakenduse sätete salvestamine](./media/app-service-api-dotnet-get-started/asinportal.png)

    Kui kood töötab Azure, alistab selle väärtuse nüüd localhost URL, kus fail on.

## <a name="test"></a>Test

1. Brauseriaknas, liikuge sirvides uue keskmisel taseme API rakenduse vastloodud ToDoListAPI URL-i. Saate seal, klõpsates API rakenduse peamised blade portaali URL.

2. Lisada "ärplema" URL brauseri aadressiribale ja vajutage sisestusklahvi Enter. (URL on `http://{apiappname}.azurewebsites.net/swagger`.)

    Brauseris kuvatakse sama ärplema kasutajaliides, mis teile kuvati varem ToDoListDataAPI, kuid nüüd `owner` pole väljale toimimiseks toomine, kuna teise eesnime esimese taseme API rakenduse saadab selle väärtuse andmete taseme API rakenduse jaoks. (Kui teete õpetused autentimine, teise eesnime esimese taseme saadab tegelik kasutaja ID-d soovitud `owner` parameeter; jaoks kohe seda on raske kodeerimine tärni.)

3. Proovige Get meetodit ja muid viise, et kontrollida, et keskmisel taseme API rakenduse edukalt helistab andmete taseme API rakendus.

    ![Ärplema UI saada meetod](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>Tõrkeotsing

Juhuks, kui tekib probleem, nagu siin selles õpetuses läbida ei probleemide tõrkeotsinguks ja lahendamiseks järgmist.

* Veenduge, et kasutate [Azure SDK .net-i](http://go.microsoft.com/fwlink/?linkid=518003)uusim versioon.

* Kaks projekti nimed on sarnane (ToDoListAPI, ToDoListDataAPI). Kui pole selline, nagu on kirjeldatud juhised, kui töötate projekti, veenduge, et olete avanud õige projekt.

* Kui olete ettevõtte võrgus ja proovite võtta kasutusele Azure'i rakendust Service läbi tulemüüri, veenduge, et pordid 443 ja 8172 on avatud Web juurutamine. Kui te ei saa avada need pordid, saate kasutada muid viise juurutamise.  Lugege teemat [Deploy oma Azure'i rakendust Service rakendus](../app-service-web/web-sites-deploy.md).

* "Marsruutimiseks nimed peavad olema kordumatud" tõrgete--te võite need kui valesti projekti kogemata juurutama API rakendus ja seejärel hiljem juurutada selle õige. Probleemi lahendamiseks valige ümberkorraldamine õige projekti API rakendus ja **Avaldada Web** viisardi vahekaardil **sätted** **eemaldada Lisafailid sihtkohta**.

Pärast seda, kui teil on rakenduse ASP.net-i API teenuses Azure rakendus töötab, võite Lisateavet Visual Studio funktsioonid, mis lihtsustavad tõrkeotsing. Logimise kohta leiate teavet teemast Kaug silumine ja muud, [tõrkeotsingu Azure'i rakendust Service rakenduste Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Järgmised sammud

Näete juurutada olemasolevaid Web API projekte API rakendused, luua kliendi koodi API rakendused ja tarbimine API rakenduste .net-i kliendi kaudu. Kuvatakse selle sarja järgmise õppeteema [CORS kasutamine klientide JavaScripti API rakenduste kasutamise](app-service-api-cors-consume-javascript.md)kohta.

Kliendi koodi loomine kohta lisateabe saamiseks vt GitHub.com [Azure/AutoRest](https://github.com/azure/autorest) hoidla. Probleemid, mis on loodud kliendi abi, avage [probleemi AutoRest hoidlas](https://github.com/azure/autorest/issues).

Kui soovite luua uue API rakenduse projektide nullist, **Azure'i API rakenduse** malli kasutamine.

![Visual Studio API rakendusemall](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

**Azure'i API rakenduse** project malli valimine soovitud **tühja** ASP.net-i 4.5.2 võrdub malli, lisada Veebiteenuste tugi märkeruutu ja Swashbuckle Nugeti paketi installimisel. Lisaks lisab Mall mõned Swashbuckle konfigureerimiskoodi vältimiseks loomine dubleeritud ärplema toimingu ID-d. Kui olete loonud mõne API rakenduse projekti, saate juurutamist API rakenduse teile kuvati selles õpetuses samal viisil.
