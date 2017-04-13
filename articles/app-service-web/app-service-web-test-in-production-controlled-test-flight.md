<properties
    pageTitle="Flighting (beeta) teenuses Azure rakenduse juurutamine"
    description="Saate teada, kuidas flight uued funktsioonid rakenduse või beeta testi oma värskenduste õppeteema – lõpuni. Koondab rakenduse teenuse funktsioone, nagu pidev avaldamine, slots, liikluse marsruutimist ja rakenduse ülevaated integreerimine."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/02/2016"
    ms.author="cephalin"/>
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Flighting (beeta) teenuses Azure rakenduse juurutamine

Selle õpetuse näidatakse, kuidas teha *flighting juurutuste* erinevaid võimalusi [Azure'i rakendust Service](http://go.microsoft.com/fwlink/?LinkId=529714) ja [Azure rakenduse ülevaated](/services/application-insights/)integreerimise abil. 

*Flighting* on juurutamise protsess, mis kinnitab teatud reaal klientide muutmine või uute funktsioonide ja on suur katsetamine tootmise stsenaariumi. See on sarnane beeta testimine ja mõnikord nimetatakse "kontrollitud testi flight". Selle võimaluse abil paljude suurettevõtete web kohalolek ennetähtaegse valideerimine saamiseks nende rakenduste värskenduste praktikas [dünaamilised](https://en.wikipedia.org/wiki/Agile_software_development)arenduse. Azure'i rakendust Service võimaldab testi valmistamisel integreerida pidev avaldamine ja rakenduse ülevaated rakendada sama DevOps stsenaariumi. Seda moodust eelised on järgmised.

- **Gain tootmisse reaal tagasiside _enne_ värskendused on vabastatud** - parem kui tagasiside saamine, kui vabastate ainus kogub tagasiside enne. Saate otse kasutaja liikluse ja muud käitumised värskendatakse juba sisse toote elutsükli soovite testida.
- **Tõsta [pidev testi põhinev arengu (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development) ** - testi valmistamisel integreerimist pidev integratsioon ja seadmeid rakenduse ülevaated, kasutaja valideerimine juhtub alguses ja automaatselt teie toote elutsükli. See aitab aega investeeringud testi käsitsi täitmise vähendada.
- **Optimeerimine töövoo testimiseks** – automatiseerimine katse pidev jälgimise mõõteaparatuuri, millel saab potentsiaalselt täita eesmärke mitmesugused katsed ühe protsessi, nt [integreerimine](https://en.wikipedia.org/wiki/Integration_testing), [regressiooni](https://en.wikipedia.org/wiki/Regression_testing), [kasutatavuse](https://en.wikipedia.org/wiki/Usability_testing), hõlbustusfunktsioonide, lokaliseerimine, [jõudluse](https://en.wikipedia.org/wiki/Software_performance_testing), [Turve](https://en.wikipedia.org/wiki/Security_testing)ja [aktsepteerimise](https://en.wikipedia.org/wiki/Acceptance_testing).

Flighting juurutamise pole on reaalajas liikluse praktiliselt marsruutimist. Juurutuse, mida soovite nii kiiresti kui võimalik saada ülevaade, olgu see siis soovitud ootamatu viga, jõudluse vähenemine, kasutaja kogemus probleemide. Pidage meeles, et teil on tegemist real kliendid. Et seda teha right, tuleb teil teha kindel, et olete seadistanud flighting juurutamise koguda kõik andmed, mida vajate selleks, et teie järgmise juhise jaoks otsus. Selle õpetuse näidatakse, kuidas koguda andmeid rakenduse ülevaated, kuid saate kasutada uue reliikvia või muid tehnoloogiaid, mis sobib teie stsenaariumi. 

## <a name="what-you-will-do"></a>Mida te teete

Selles õppetükis saate teada, kuidas tuua koos, et testida rakenduse App teenuse valmistamisel järgmistel juhtudel:

- Rakenduse beeta- [liikluse marsruutimiseks tootmise](app-service-web-test-in-production-get-start.md)
- [Dokumendi rakenduse](../application-insights/app-insights-web-track-usage.md) mõõdikute kasu saada
- Pidevalt Juurutage beeta rakendus ja jälgida reaalajas rakenduse mõõdikud
- Võrrelge mõõdikute tootmise rakendus ja beeta rakenduse näha, kuidas koodi muudatusi tõlkida tulemuste vahel

## <a name="what-you-will-need"></a>Mida on vaja

-   Azure'i konto
-   [GitHub](https://github.com/) konto
- Visual Studio 2015 – saate alla laadida [ühenduse edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).
-   Git Shell (paigaldatud [GitHub Windowsi jaoks](https://windows.github.com/)) – nii saate käivitada nii Git ja PowerShelli käske sama seansi
-   Viimane [Azure PowerShelli](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bittide
-   Põhiteadmisi järgmist:
    -   [Azure'i ressursihaldur](../azure-resource-manager/resource-group-overview.md) malli juurutamise (vt [Deploy keerukate rakenduses ootuspäraselt Azure](app-service-deploy-complex-application-predictably.md))
    -   [Git](http://git-scm.com/documentation)
    -   [PowerShelli](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Peate selle õpetuse lõpuleviimiseks Azure'i konto.
> + Saate [avada Azure'i konto tasuta](/pricing/free-trial/) – saate krediiti abil saate proovida makstud Azure'i teenuste ja isegi juhul, kui neid kasutatakse hoiate konto ja kasutamine tasuta Azure teenuseid, näiteks Web Apps.
> + Saate [aktiveerida Visual Studio abonendi kasu](/pricing/member-offers/msdn-benefits-details/) – teie Visual Studio tellimuse annab teile krediiti iga kuu makstud Azure'i teenuste kasutatavad.
>
> Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="set-up-your-production-web-app"></a>Oma tootmise web appi häälestamine

>[AZURE.NOTE] Selles õpetuses kasutatud skript automaatselt konfigureerida pidev avaldamise kaudu oma GitHub hoidla. Selleks, et GitHub mandaat on juba salvestatud Azure, muidu skriptitud juurutamise nurjub, kui proovite veebirakenduste andmeallika juhtelemendi sätete konfigureerimine.
>
>Azure GitHub mandaadi talletamiseks luua veebirakenduse [Azure portaali](https://portal.azure.com/) ja [konfigureerida GitHub juurutuse](app-service-continuous-deployment.md#Step7). Ainult peate selleks üks kord.

Tüüpilised DevOps stsenaariumi puhul on rakendus, mis töötab reaalajas Azure ja soovite seda muuta pidev avaldamise kaudu. Selle stsenaariumi korral saavad võtta kasutusele tootmise malli, mida teil on välja töötatud ja testida.

1.  Saate luua oma kahvel [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) hoidla. Toidulauani loomise kohta leiate teavet teemast [kahvel Repo](https://help.github.com/articles/fork-a-repo/). Kui teie kahvel on loodud, näete brauseris.

    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Avage Git Shell seanss. Kui teil pole veel Git Shell, installige [Windowsi GitHub](https://windows.github.com/) .
3.  Luua kohaliku klooni oma kahvel, käitades järgmine käsk:

        git clone https://github.com/<your_fork>/ToDoApp.git

4.  Kui olete oma kohaliku klooni, liikuge * &lt;repository_root >*\ARMTemplates ja Käivita selle deploy.ps1 skript ning kordumatu järelliide, nagu allpool näidatud:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix>

4.  Vastava viiba kuvamisel tippige soovitud kasutajanimi ja parool andmebaasi juurdepääsu. Pidage meeles andmebaasi mandaat, sest peate need uuesti määrata ressursirühma värskendamisel.

    Peaksite nägema ettevalmistamise edenemist erinevate Azure ressursse. Kui juurutamise lõpule jõudnud, kuvatakse skripti käivitage rakendus brauseris ja teile sõbralik piiks.
    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)

6.  Tagasi Git Shell seansi käivitamine:

        .\swap –Name ToDoApp<your_suffix>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)

7.  Kui skripti lõpetab, minge tagasi liikuge sirvides soovitud frontend aadress (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) kuvamiseks valmistamisel rakendus.
5.  [Azure portaali](https://portal.azure.com/) sisse logida ja heita pilk, mis on loodud.

    Peaks oskama vt sama ressursi jaotises üks kahest veebirakenduste soovitud `Api` nime järelliide. Kui vaatate Ressursivaade rühma, kuvatakse ka SQL-andmebaasi ja server, rakenduse teenusleping ja lavastus slots web apps. Sirvige erinevate ressursside ja võrdleb neid * &lt;repository_root >*\ARMTemplates\ProdAndStage.json näha, kuidas need on konfigureeritud mall.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

Olete seadistanud tootmise rakendus.  Nüüd Oletagem, et saada tagasisidet, et kasutatavuse on kehva rakenduse. Et saaksite otsustada, kui soovite uurida. Kui kavatsete dokumendi rakenduse tagasiside andmiseks.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Uurige: Vahend oma kliendi rakenduse jälgimise mõõdikute

5. Avatud * &lt;repository_root >*\src\MultiChannelToDo.sln Visual Studios.
6. Taastada, paremklõpsates lahenduse Nuget-paketid > **Halda Nugeti pakettide lahenduse** > **taastamine**.
6. Paremklõpsake **MultiChannelToDo.Web** > **Lisamine rakenduse ülevaateid telemeetria** > **Sätete konfigureerimine** > Muuda ressursirühma ToDoApp*&lt;your_suffix >* > **Rakenduse ülevaated lisamine projekti**.
7. Avage Azure'i portaalis tera **MultiChannelToDo.Web** rakenduse ülevaate ressursi. **Rakenduse seisundi** osas, klõpsake **saate teada, kuidas brauseris lehe laadimise andmete kogumise** > Kopeeri kood.
7. Kopeeritud JS instrumentation koodi lisamisel * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml just enne `<heading>` silt. Peaks olema kordumatu instrumentation võti oma rakenduse ülevaate ressursi.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>

11. Saada kohandatud sündmused rakenduse ülevaated hiire klõpsuga, lisades sisu lõppu järgmine kood:

        <script>
            $(document.body).find("*").click(function(event) {

                appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
            });
        </script>

    See JavaScripti koodilõigu saadab kohandatud sündmuse rakenduse ülevaated iga kord, kui kasutaja klõpsab suvalist web Appis.

12. Git Shellis Kinnita ja vajutage muudatuste toidulauani GitHub. Siis oodake kliendid värskendage brauserit.

        git add -A :/
        git commit -m "add AI configuration for client app"
        git push origin master

6.  Vaheta tootmisse juurutatud rakenduse muudatusi:

        .\swap –Name ToDoApp<your_suffix>

13. Liikuge sirvides rakenduse ülevaated ressursside konfigureeritud. Klõpsake nuppu kohandatud sündmused.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

    Kui te ei näe jaoks kohandatud sündmused mõõdikute, oodake paar minutit ja klõpsake nuppu **Värskenda**.

Oletame, et näete diagrammi nagu allpool:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

Ja sündmuse ruudustiku selle all.

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

ToDoApp rakenduse koodi vastavalt **nuppu** sündmus vastab nupp Edasta ja **sisestatud** sündmuse vastab siis tekstivälja. Nii palju, mida oleks. Siiski tundub, et on hea hulk hiireklõpsuga ja väga mõne klõpsuga ülesandeloendi üksuste ( **LI** sündmused).

Selle vormi saate oma juhul, mis on mõnel kasutajal segi Kasutajaliidese mille osa on klõpsata ja sellepärast, et kursor on laadiatribuudid tekstivaliku valikuriistana, loendiüksusi ja nende ikoonid, kui.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

See võib olla kunstlik näiteks. Siiski ei kavatse teha rakenduse paraneb ja tehke flighting juurutamise saada tagasisidet kasutatavuse reaalajas kliendid.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>Dokumendi jälgimise mõõdikute oma serveri rakenduse
See on mõne tangensi, kuna seda stsenaariumi, mis on näidatud selles õpetuses tegeleb ainult kliendi rakendus. Siiski jaoks täiendavalt saate häälestab serveripoolne rakendus.

6. Paremklõpsake **MultiChannelToDo** > **Lisamine rakenduse ülevaateid telemeetria** > **Sätete konfigureerimine** > Muuda ressursirühma ToDoApp*&lt;your_suffix >* > **Rakenduse ülevaated lisamine projekti**.
12. Git Shellis Kinnita ja vajutage muudatuste toidulauani GitHub. Siis oodake kliendid värskendage brauserit.

        git add -A :/
        git commit -m "add AI configuration for server app"
        git push origin master

6.  Vaheta tootmisse juurutatud rakenduse muudatusi:

        .\swap –Name ToDoApp<your_suffix>

See on õige!

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a>Uurige: Oma kliendi rakenduse mõõdikute pesa kohased siltide lisamine

Selles jaotises tuleb konfigureerida erinevate juurutamise teenindusaegade pesa kohased telemeetria saatmiseks sama rakenduse ülevaated ressurss. Sellisel viisil saate võrrelda saate hõlpsasti vaadata rakenduse muudatuste mõju koguda andmeid liikluse eri teenindusaegadest (juurutamise keskkondades) vahel. Samal ajal saate eraldada tootmise liikluse ülejäänud nii, et saate jälgida oma tootmise rakenduse vastavalt vajadusele jätkata.

Kuna te olete andmete kogumine kliendi käitumine, siis [Lisage telemeetria initializer oma JavaScripti koodi](../application-insights/app-insights-api-custom-events-metrics.md#js-initializer) index.cshtml. Kui soovite serveripoolne jõudlust testida, näiteks saate teha ka sarnaselt serveri koodi (vt teemat [Rakenduse ülevaateid API kohandatud sündmused ja mõõdikute](../application-insights/app-insights-api-custom-events-metrics.md).

1. Esmalt lisage koodi koolitusseminaride kahe `//` kommentaaride all JavaScript blokeerida, et te lisatud soovitud `<heading>` sildistamine varasemas versioonis.

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    Initializer järgmine kood tekitab soovitud `appInsights` objekti lisamiseks soovitud kohandatud atribuuti nimega `Environment` iga tükk telemeetria saadetakse.

2. Järgmisena Lisage kohandatud atribuut [pesa säte](web-sites-staged-publishing.md#AboutConfiguration) Azure veebirakenduse jaoks. Selle tegemiseks käivitage järgmised käsud oma Git Shell seansi jooksul.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    Projekti Web.config juba määratleb selle `environment` rakenduse säte. See säte, kui rakenduse kohalik, test oma mõõdikute on olema kodeeritud `VS Debugger`. Juhul, kui vajutada muudatuste Azure, Azure on otsimine ja kasutamine on `environment` rakenduse web appi konfiguratsiooni asemel hoopis seda sätet ja oma mõõdikute sildistatud sisuga `Production`.

3. Kinnita push kood muudatuste toidulauani github ja oodake kasutajatele uus rakendus (vaja värskendage brauserit). Kulub umbes 15 minutit uue atribuudi kuvamiseks klõpsake oma rakenduse ülevaated `MultiChannelToDo.Web` ressursi.

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master

4. Nüüd avage uuesti tera **kohandatud sündmused** ja filtreerida mõõdikud `Environment=Production`. Nüüd peaks olema näeksid kõik uue kohandatud sündmused tootmise pesa selle filtriga.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)

5. Umbes praeguse mõõdikute Exploreri sätete salvestamiseks nuppu **Lemmikud** **kohandatud sündmused: tootmise**. Te saate hõlpsasti vaheldumisi selle ja juurutamise pesa vaade hiljem.

    > [AZURE.TIP] Veelgi võimsam analytics, kaaluge [integreerimise oma Power BI rakenduse ülevaated ressurss](../application-insights/app-insights-export-power-bi.md).

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a>Oma serveri rakenduse mõõdikute pesa kohased siltide lisamine
Klõpsake uuesti jaoks täiendavalt saate häälestab serveripoolse rakenduse. Erinevalt JavaScript mõõteseadmetega kliendi rakendus, on pesa kohased sildid serveri rakendus kinnitatakse .net-i kood.

1. Avatud * &lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs. Lisada koodi blokeering all, vaid enne selle lõpetamist nimeruum looksulg.

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }

2. Vigade soovitud nime eraldusvõime, lisades selle `using` laused all alguseni faili:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;

3. Lisada koodi all alguses on `Application_Start()` meetod:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());

3. Kinnita push kood muudatuste toidulauani github ja oodake kasutajatele uus rakendus (vaja värskendage brauserit). Kulub umbes 15 minutit uue atribuudi kuvamiseks klõpsake oma rakenduse ülevaated `MultiChannelToDo` ressursi.

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>Update: Häälestamine oma beeta haru

2. Avatud * &lt;repository_root >*\ARMTemplates\ProdAndStagetest.json ja leida soovitud `appsettings` ressursid (otsida `"name": "appsettings"`). On neist, üks iga pesa 4. 

2. Iga `appsettings` ressurss, lisada mõne `"environment": "[parameters('slotName')]"` säte rakenduse lõppu on `properties` massiiv. Ärge unustage lõpetamiseks komaga eelmisele reale.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)
    
    Äsja lisatud funktsiooni `environment` säte rakenduse kõik teenindusaegade malli.
    
2. Sama fail, otsige üles soovitud `slotconfignames` ressursid (otsida `"name": "slotconfignames"`). Seal on neid, üks iga rakenduse 2.

2. Iga `slotconfignames` ressurss, lisada `"environment"` lõppu on `appSettingNames` massiiv. Ärge unustage lõpetamiseks komaga eelmisele reale.

    Ainult tegite selle `environment` säte kinni selle vastav juurutamine pesa nii rakendused rakendus.  

3. Seansi Git Shell käivitage järgmised käsud sama ressursi rühma järelliide, mida kasutasite enne.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta

4. Kui küsitakse, määrake sama SQL-i andmebaasi mandaat nagu enne. Kui teilt küsitakse, kas värskendamine ressursirühma, tippige `Y`, siis `ENTER`.

    Kui skripti lõppemist, säilitatakse kõik teie algne ressursirühm ressursse, kuid uue pesa nimega "beta" on loodud selle sama konfiguratsiooni nimega "Lavastus" pesa, mis on loodud alguses.

    >[AZURE.NOTE] Selle meetodi luua erinevate juurutamise keskkonnas erineb [dünaamilised tarkvara](app-service-agile-software-development.md)arendamiseks Azure'i rakenduse teenusega meetodit. Siin saate luua juurutamise keskkonnas juurutamise pilud, kus kuna seal loote ressursirühma juurutamise keskkonnas. Haldamise juurutamine keskkonnas ressursi rühmade abil saate salvestada tootmiskeskkonda off-piiranguid arendajatele, kuid ei ole lihtne teha valmistamisel, mida saate teha hõlpsalt teenindusaegade testimine.

Soovi korral saate ka luua alfa rakendus töötab

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

Selles õpetuses mõeldud lihtsalt kasutate rakenduse beeta.

## <a name="update-push-your-updates-to-the-beta-app"></a>Update: Push värskenduste beeta rakendusse

Tagasi, et teie rakendus, mida soovite parandada.

1. Veenduge, et olete nüüd teie beeta haru

        git checkout beta

2. Rakenduses * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, leida soovitud `<li>` sildistamine ja lisage soovitud `style="cursor:pointer"` atribuut, nagu allpool näidatud.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)

3. Kinnita ja tõuketeatised Azure.

4. Veenduge, et muudatus kohe kajastub beeta pesa navigeerimise http://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/. Kui te ei näe muutmine veel, värskendage brauseriakent uue JavaScripti koodi.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Nüüd, kui teil on opsüsteemi beeta pesa muudatuse, olete valmis flighting juurutamise sooritamiseks.

## <a name="validate-route-traffic-to-the-beta-app"></a>Kinnitage: Marsruutimiseks liikluse beeta rakendusse

Selles jaotises kuvatakse marsruutida liikluse beeta rakendus. Tutvustamise selgust, sest kavatsete selle osa kasutaja liikluse marsruutimiseks. Teatud olukord sõltub tegelikult soovite marsruutida liikluse. Näiteks kui teie sait on veebisaidil microsoft.com skaala, siis peate alla ühe protsendi oma liikluse selleks, et saada kasulikke andmeid.

1. Seansi Git Shell käivitage järgmised käsud pool tootmise liikluse marsruutimiseks beeta pesa.

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

  Funktsiooni `ReroutePercentage=50` atribuut määrab, et 50% tootmise liikluse suunatakse beeta rakenduse URL-i (määratud funktsiooni `ActionHostName` atribuudi).

2. Nüüd liikuda http://ToDoApp*&lt;your_suffix >*. azurewebsites.net. Nüüd peaks 50% liikluse ümber beeta pesa.

3. Oma rakenduse ülevaated koodiressursi, filtreerimine mõõdikud keskkonna = "beta".

    > [AZURE.NOTE] Kui salvestate selle filtreeritud vaate teine lemmik, siis saate hõlpsalt pöörata argumendil Exploreri vaadete toote ja beeta vaadete vahel.

Oletame, et rakenduse ülevaated näete umbes järgmine:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Mitte ainult see on kuvatud kohta on palju lisaklõpsu selle `<li>` sildid, kuid tundub, et lisandus hiireklõpsuga klõpsake `<li>` sildid. Seejärel saate sõlmida, et inimesed on avastanud uue `<li>` sildid on klõpsata ja on nüüd kõik eelnevalt täidetud tööülesannete rakenduse tühjendada.

Flighting juurutamise andmete põhjal saate otsustada, et oma uut UI on valmis tootmist.

## <a name="go-live-move-your-new-code-into-production"></a>Avage reaalajas: tootmise oma uue koodi viimine

Nüüd olete valmis oma update teisaldamiseks tootmise. Mis on suur, et nüüd teate, et teie värskendus on juba valideeritud _enne_ selle on lükata tootmisele. Nüüd saate kindlalt juurutamist. Kuna tegite AngularJS kliendi rakenduse värskendus, vaid kinnitatud kliendipoolne kood. Kui teil muudatuste tegemiseks tagaandmebaas Web API app, võib samamoodi ja hõlpsasti valideerimiseks muudatuste.

1. Git Shellis eemaldada liikluse marsruutimise reeglite, käivitage järgmine käsk:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()

2. Käivitage Git käsud:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master

2. Oodake mõni minut uue koodi vahekausta pesa kasutusele võtta ja seejärel käivitage http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net uue update soojendada vahekausta pesa kinnitamiseks. Pidage meeles, et selle rakenduse vahekausta pesa on seotud teie kahvel juhtslaidi haru.

3. Nüüd Vaheta tootmisse vahekausta pesa

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Kokkuvõte ##

Azure'i rakendust Service lihtne väike-keskmise suurusega ettevõtete Testige oma kliendi-ühendusega rakenduste midagi valmistamisel, mis traditsiooniliselt tehtud suur ettevõtetes. Loodetavasti selles õpetuses on andnud teile teadmisi, mida vajate koondada rakenduse teenuse ja rakenduse ülevaated flighting juurutada ja isegi muu testi-sisse-peamised, teha oma DevOps geograafiline asukoht. 

## <a name="more-resources"></a>Veel ressursse ##

-   [Dünaamilised tarkvara arendamine Azure'i rakenduse teenusega](app-service-agile-software-development.md)
-   [Lavastus keskkonnas web Appsi teenuses Azure rakenduse häälestamine](web-sites-staged-publishing.md)
-   [Keerukate rakenduses ootuspäraselt Azure juurutamine](app-service-deploy-complex-application-predictably.md)
-   [Azure'i ressursihaldur mallide koostamine](../resource-group-authoring-templates.md)
-   [JSONLint - JSON süntaksi](http://jsonlint.com/)
-   [Git harude – lihtsa harude ja ühendamine](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [Azure'i PowerShelli](../powershell-install-configure.md)
-   [Projekti Kudu viki](https://github.com/projectkudu/kudu/wiki)
