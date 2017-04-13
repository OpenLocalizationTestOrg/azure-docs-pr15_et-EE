<properties
    pageTitle="Azure'i rakendust Service - Node.js versioonile mobiilsideseadmete teenuste"
    description="Saate teada, kuidas hõlpsasti uuendada oma Mobile teenuste rakenduse App teenuse mobiilirakenduse"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>Uuendada oma olemasoleva Node.js Azure mobiilsideteenuse rakenduse teenus

Rakenduse teenuse Mobile on uus võimalus koostada mobiilirakenduste kaudu Microsoft Azure'i. Lisateavet leiate teemast [millised mobiilirakenduste?].

Selles teemas kirjeldatakse, kuidas võtta kasutusele Azure Mobile teenused olemasoleva Node.js kirjutamata rakenduse uus rakendus teenuse Mobile'i rakendused. Kui teete seda uuendada, olemasoleva Mobile teenuste rakenduse saate jätkata.  Kui teil on vaja uuendada Node.js kirjutamata rakenduse viidata [üleminekut .NET Mobile teenuste](./app-service-mobile-net-upgrading-from-mobile-services.md).

Mobiilse taustväärtus uuendamisel Azure'i rakendust Service on juurdepääs kõik rakenduse teenuse funktsioonid ja vastavalt [rakenduse teenuse hinnad], Mobile teenuste hinnad on arve.

## <a name="migrate-vs-upgrade"></a>Migreerimine ja täiendamine

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Soovitatav on selle [migreerimine](app-service-mobile-migrating-from-mobile-services.md) enne uuendamist läbi. Sellisel viisil mõlemad versioonid rakenduse sellele sama rakenduse teenuse kavandamine ja tekkida täiendava tasuta.

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Mobile'i rakendused Node.js serveri SDK täiustused

Täiendamine versiooniks uue [Mobile'i rakendused SDK](https://www.npmjs.com/package/azure-mobile-apps) pakub palju parandusi, sh:

- [Kiire framework](http://expressjs.com/en/index.html), uue sõlm SDK on kerge ja loodud kursis uus sõlm versioonid, kui nad tulevad välja. Saate kohandada rakenduse käitumist Express vahevara abil.

- Mobile teenuste SDK võrreldes olulisi jõudlustäiustusi.

- Veebisaidi saate majutada nüüd koos oma mobiilsideseadmete kirjutamata; Samuti on lihtne Azure Mobile SDK lisada mis tahes olemasolevat express.v4 rakendust.

- Mitu platvormi ja ehitatud, Mobile'i rakendused SDK saab väljatöötatud ja käivitada kohalikult Windows, Linux ja OSX platvormide. Nüüd on lihtne kasutada levinud sõlm meetodid nagu töötab [Mocha](https://mochajs.org/) kontrollib enne juurutamist.

## <a name="overview"></a>Tavaline versioonitäienduse ülevaade

Abi üleminekut Node.js kirjutamata, Azure'i rakendust Service on andnud ühilduvus paketi.  Pärast uuele versioonile, peate niew saidi, uue rakenduse teenuse saidile juurutatud.

Mobile Services kliendi SDK-d on **Uus mobiilirakenduste server SDK ühildub** . Jätkumise oma rakenduse pakkumiseks peaks muudatuste avaldamine pole praegu serveeritakse avaldatud kliendid saidil. Selle asemel loote uue mobiilirakenduse, mis toimib duplikaat. Selle rakenduse saate paigutada samale rakenduse teenusleping proovima täiendavaid rahalisi kulusid.

Siis on rakenduse kaks versiooni: üks, mis jääb samaks, ja pakutakse avaldatud rakendused looduslike, ja teise, mida saate siis täiendada ja target uue klientrakenduse väljaandes. Saate teisaldada ja testige oma tempos oma koodi, kuid veenduge, et kõik teete veaparandused saada rakendatud nii. Kui tunnete, et soovitud arvu looduses rakendused on värskendatud üle uusimale versioonile klient, saate kustutada algse migreeritud rakenduse kui soovite. See ei kandma kõik täiendavad rahaliste kulud, kui majutatud sama rakenduse teenuse lepingu oma Mobile'i rakendus.

Täieliku ülevaate versiooniuuenduse jaoks on järgmine:

1. Laadige oma olemasoleva (migreeritud) Azure mobiilsideteenuse.
2. Azure'i mobiilirakenduse abil ühilduvust paketi projekti teisendada.
3. Vea erinevusi (nt autentimise sätted).
4. Uue rakenduse teenuse juurutada teisendatud Azure'i Mobile'i rakendus projekti.
4. Uue mobiilirakenduse kasutavate klientrakenduse uue versiooni.
5. (Valikuline) Teie algne migreeritud mobiilsideteenuse rakenduse kustutamine.

Kustutamise võib tekkida siis, kui te ei näe mis tahes liikluse oma algse migreeritud mobiilsideteenuse.

## <a name="install-npm-package"></a>Installimine on eeltingimused

Installi [sõlme] oma kohalikus arvutis.  Lisaks peaksite installima ühilduvus pakett.  Pärast sõlm on installitud, saate saate uue cmd või PowerShelli käsuviibas käivitage järgmine käsk:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Hankige oma skriptide Azure Mobile teenused

- [Azure'i portaali]sisse logida.
- **Kõik ressursid** või **Rakenduse teenuste**kasutamisel leida Mobile Services saidile.
- Saidil, klõpsake nuppu **Tööriistad** -> **Kudu** -> Kudu saidi avamiseks**avage** .
- Klõpsake **Konsooli silumine** -> **PowerShelli** konsooli silumine avamiseks.
- Liikuge `site/wwwroot/App_Data/config` klõpsates iga directory omakorda
- Klõpsake allalaadimise ikooni kõrval olevat `scripts` kataloogi.

Laadige alla ZIP-vormingus skriptide.  Looge uus kaust teie kohalikus arvutis ja lahti selle `scripts.ZIP` faili kataloogi raames.  See loob mõne `scripts` kataloogi.

## <a name="scaffold-app"></a>Uue Azure mobiilirakenduste kirjutamata tellingud

Kataloogist, mis sisaldab kataloogi skriptide käivitage järgmine käsk:

```scaffold-mobile-app scripts out```

See loob scaffolded Azure mobiilirakenduste kirjutamata sisse selle `out` kataloogi.  Kuigi ei pea, on hea mõte kasutada funktsiooni `out` kataloog source code andmebaasi oma valik.

## <a name="deploy-ama-app"></a>Oma Azure'i mobiilirakenduste taustväärtus juurutamine

Juurutamisel, peate tegema järgmist:

1. Looge uus Mobile'i rakendus [Azure portaali].
2. Käivitage selle `createViews.sql` ühendatud andmebaasi skripti.
3. Link andmebaas, mis on lingitud teie mobiiliteenuse uue rakenduse teenust.
4. Link uus rakendus teenuse mis tahes muud ressursid (nt teatis jaoturi).
5. Uue saidi juurutada genereeritud koodi.

### <a name="create-a-new-mobile-app"></a>Looge uus Mobile'i rakendus

1. [Azure'i portaali]sisse logida.

2. Valige **+ Uus** > **Web + Mobile** > **Mobile'i rakendus**, seejärel sisestage oma Mobile'i rakendus kirjutamata nimi.

3. **Ressursirühm**, valige ressursi olemasolevasse rühma või luua uue (kasutades sama nime panemine.) 
 
    Saate valida mõne muu rakenduse teenusleping või looge uus. Rakenduse teenuste kohta lisateavet lepingud ja kuidas luua uue lepingu eri hinnad taseme ja soovitud asukohta, leiate [Azure'i rakendust Service lepingute põhjalik ülevaade](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. **Rakenduse teenusleping**vaikimisi plaani ( [Standard taseme](https://azure.microsoft.com/pricing/details/app-service/)) on märgitud. Võite valida ka mõne muu lepingu või [looge uus](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Rakenduse teenusleping sätted kindlaks [asukoht, funktsioonide maksumus ja arvutada ressursid](https://azure.microsoft.com/pricing/details/app-service/) oma rakendusega seotud. 

    Kui otsustate leping, klõpsake nuppu **Loo**. See loob Mobile'i rakendus taustväärtus. 


### <a name="run-createviewssql"></a>Käivitage CreateViews.SQL

Scaffolded rakendus sisaldab faili `createViews.sql`.  See skript tuleb käivitada vastu Sihtandmebaas.  Target (sihtkoht) andmebaasi ühendusstringi aadressil oma migreeritud mobiilsideteenuse keelest **sätted** jaotises **Ühendusstringi**.  See on nime `MS_TableConnectionString`.

SQL Server Management Studio või Visual Studio skripti kaudu käivitada.

### <a name="link-the-database-to-your-app-service"></a>Link rakenduse teenust andmebaas

Olemasoleva andmebaasi linkimine rakenduse teenust.

- [Azure portaali], avage rakendus teenust.
- Valige **Kõik sätted** -> **andmeühendusi**.
- Klõpsake nuppu + **Lisa**.
- Valige rippmenüüst, **SQL-andmebaas**
- Jaotises **SQL-andmebaasi**valida olemasoleva andmebaasi, seejärel **Valige**.
- **Ühendusstringi**, sisestage andmebaasi kasutajanimi ja parool ja seejärel klõpsake nuppu **OK**.
- **Lisa andmeühenduste** tera, klõpsake nuppu **OK**.

Kasutajanime ja parooli leiate vaatate target andmebaasi ühendusstringi oma migreeritud mobiilsideteenuse.


### <a name="set-up-authentication"></a>Häälestamine autentimine

Azure'i mobiilirakenduste kohta saate konfigureerida Azure Active Directory, Facebooki, Google, Microsoft ja Twitteri autentimise teenuses.  Kohandatud autentimine on vaja eraldi välja töötada.  Viidata [Autentimise põhimõtet] dokumentatsiooni ja [Autentimise Kiirjuhend] dokumentatsioonist.  

## <a name="updating-clients"></a>Mobiilikliendid värskendamine

Kui teil on mõni funktsionaalseid mobiilirakenduse kirjutamata, saate töötada oma kliendi rakendus, mis kasutab seda uue versiooni. Mobiilirakenduste sisaldab ka uus versioon, mille kliendi SDK-d ja sarnaselt serveri uuele versioonile üle, peate kogu viiteid Mobile teenuste SDK-d eemaldage enne installimist Mobile'i rakenduste versioonid.

Üks peamisi muudatusi vahelised erinevused on puhul on ehitajatel pole enam vaja taotluse võti. Saate nüüd lihtsalt läheb Mobile rakenduse URL. Näiteks klõpsake .net-i kliendid, on `MobileServiceClient` ehitaja on nüüd:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Saate lugeda installimist uus SDK-d ja uue struktuur kaudu linkidest kasutamise kohta:

- [Android 2.2 või uuem versioon](app-service-mobile-android-how-to-use-client-library.md)
- [iOS-i versioon 3.0.0 või uuem versioon](app-service-mobile-ios-how-to-use-client-library.md)
- [.Net-i (Windows/Xamarin) versioon 2.0.0 või uuem versioon](app-service-mobile-dotnet-how-to-use-client-library.md)
- [Apache Cordova 2.0 või uuem versioon](app-service-mobile-cordova-how-to-use-client-library.md)

Kui rakenduse Tõuketeatiste kasutada, märkige kindla registreerimise juhiseid iga platvormi, kui seal on olemas mõned muudatused ka.

Kui olete valmis uue klientrakenduse versiooni, proovi uuele versioonile üle viidud serveri projekti suhtes. Pärast kontrollimine, et see toimib, saate vabastada kliendid oma rakenduse uus versioon. Lõpuks, kui klientide olnud võimalus neid värskendusi, saate kustutada rakenduse versiooni Mobile teenused. Selles etapis täielikult versiooniks rakenduse teenuse mobiilirakenduse abil uusima mobiilirakenduste serveri SDK.

<!-- URLs. -->

[Azure'i portaal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Mis on mobiilirakenduste kohta?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Rakenduse teenuse hinnad]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Autentimise mõisted]: ../app-service/app-service-authentication-overview.md
[Autentimise Kiirjuhend]: app-service-mobile-auth.md

[Azure'i portaal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
