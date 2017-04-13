<properties
    pageTitle="Mobiilirakenduste Node.js kirjutamata serveri SDK töötamise | Azure'i rakendust Service"
    description="Saate teada, kuidas töötada Node.js kirjutamata server SDK Azure'i rakenduse teenuse mobiilirakenduste kohta."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Kuidas kasutada Azure Mobile'i rakendused Node.js SDK

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Sellest artiklist leiate üksikasjalikku teavet ja näiteid, mis näitab, kuidas töötada Node.js kirjutamata Azure rakenduse teenuse Mobile'i rakendused.

## <a name="Introduction"></a>Sissejuhatus

Azure'i rakenduse teenuse mobiilirakenduste pakub võimalust veebirakenduse mobile optimeeritud andmepääsu Web API lisada.  ASP.net-i ja Node.js veebirakenduste jaoks on esitatud Azure rakenduse teenuse Mobile'i rakendused SDK.  SDK sisaldab järgmisi toiminguid:

- Juurdepääs andmetele tabeli toiminguid (vt, lisamine, värskendamine, kustutamine)
- Kohandatud API toimingud

Mõlemad toimingud ette autentimist üle kõik identiteedipakkujad Azure'i rakendust Service, sh social identiteedipakkujad nagu Facebooki, Twitteri, Google ja Microsoft Azure Active Directory enterprise identiteedi jaoks lubatud.

Vaadake näiteid iga [näidised directory github]puhul kasutada.

## <a name="supported-platforms"></a>Toetatud platvormid

Azure'i Mobile'i rakendused sõlm SDK toetab LTS väljaanne sõlm ja uuemad versioonid.  Nagu kirjalikult LTS uusim versioon on sõlm v4.5.0.  Muude versioonide sõlm võivad töötada, kuid ei toetata.

Azure'i Mobile'i rakendused sõlm SDK toetab kahte andmebaasidraiverid - sõlm-mssql draiver toetab SQL Azure'i ja SQL serveri kohaliku eksemplari.  Sqlite3 draiver toetab ainult ühe eksemplari SQLite andmebaasid.

### <a name="howto-cmdline-basicapp"></a>Kuidas: loomine on lihtne Node.js kirjutamata käsureal

Iga Azure'i rakenduse teenuse Mobile'i rakendus Node.js kirjutamata käivitatakse ExpressJS rakendusena.  ExpressJS on kõige populaarsemate web teenuse raames saadaval Node.js.  Saate luua basic [kiire] rakenduse järgmiselt:

1. Käsu või PowerShelli aknas, looge oma projekti kaust.

        mkdir basicapp

2. Käivitage npm käivitamise struktuuri paketi käivitamine.

        cd basicapp
        npm init

    Käsk npm käivitamise küsib kogumi küsimused projekti lähtestada.  Näide väljund vt

    ![Npm käivitamise väljund][0]

3. Installige kiire ja azure mobiilirakenduste teekide npm hoidla.

        npm install --save express azure-mobile-apps

4. Põhilised mobiiltelefonide server kasutusele app.js faili loomine.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

See rakendus loob mobile optimeeritud WebAPI ühe lõpp-punkti (`/tables/TodoItem`), mis pakub autentimata juurdepääsu aluseks oleva SQL-i andmesalve, dünaamiline skeemi abil.  Sobib pärast kliendi Raamatukogu kiire käivitamise:

- [Androidi kliendi Kiirjuhend]
- [Apache Cordova kliendi Kiirjuhend]
- [iOS-i kliendi Kiirjuhend]
- [Windowsi poe kliendi Kiirjuhend]
- [Xamarin.iOS kliendi Kiirjuhend]
- [Xamarin.Android kliendi Kiirjuhend]
- [Xamarin.Forms kliendi Kiirjuhend]

Leiate selle lihtsa rakenduse [github basicapp proovi]kood.

### <a name="howto-vs2015-basicapp"></a>Kuidas: loomine sõlm kirjutamata Visual Studio 2015

Visual Studio 2015 jaoks on vaja töötada jooksul IDE Node.js rakendusi laiendamine.  Installige alustamiseks [Node.js tööriistade 1.1 Visual Studio].  Kui Node.js Tools for Visual Studio on installitud, looge rakenduse Express 4.x.

1. Avage dialoogiboks **Uue projekti** ( **failist** > **Uus** > **projekti...**).

2. Laiendage **Mallid** > **JavaScripti** > **Node.js**.

3. Valige **tavaline Azure Node.js Express 4 rakendus**.

4. Täitke projekti nime.  Klõpsake nuppu *OK*.

    ![Visual Studio 2015 uue projekti][1]

5. Paremklõpsake sõlme **npm** ja valige **installida uue... npm paketid**.

6. Võib-olla peate värskendama npm kataloogi esimese Node.js rakenduse loomise kohta.  Vajadusel, klõpsake nuppu **Värskenda** .

7. Sisestage otsinguväljale _Azure'i mobiilirakenduste_ .  **Azure'i-mobiilirakenduste 2.0.0** paketi, klõpsake käsku **Paketi installimine**.

    ![Uue npm pakettide installimine][2]

8. Klõpsake nuppu **Sule**.

9. Avage fail _app.js_ tugi Azure Mobile'i rakendused SDK lisada.  Rida 6 at teegi allosas nõuavad laused, lisage järgmine kood:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    Ligikaudu rida 27 pärast muude app.use laused, lisage järgmine kood:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Salvestage fail.

10. Käivitage rakendus kohalikult (API on kätte http://localhost:3000) või Azure avaldada.

### <a name="create-node-backend-portal"></a>Kuidas: Node.js kirjutamata, Azure portaali loomine

Saate luua mobiilirakenduse kirjutamata paremale [Azure portaali]. Saate täitke järgmised juhised või luua kliendi ja serveri koos järgides õppeteema [mobiilse rakenduse loomine](app-service-mobile-ios-get-started.md) . Õpetuse juhistest lihtsustatud versioon sisaldab ja on parim tõendada mõistet projektide.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Uuesti _alustada_ tera, klõpsake jaotises **looge uus tabel API**, valige **Node.js** oma **Taustväärtus keel**. Märkige ruut "**nõus, et see kirjutab kogu saidi sisu.**" ja seejärel klõpsake nuppu **Loo TodoItem tabel**.

### <a name="download-quickstart"></a>Kuidas: Node.js kirjutamata Kiirjuhend Koodiprojekti abil Git allalaadimine

Node.js Mobile'i rakendus kirjutamata loomisel portaalis **Lühijuhend** blade Node.js projekti on teie jaoks loodud ja juurutatud saidile. Saate lisada tabeleid ja API-d ja redigeerida koodi failid Node.js taustväärtus portaalis. Erinevate juurutamise tööriistade abil allalaadimise taustväärtus projekt, nii et saate lisada või muuta tabelite ja API-d ning seejärel uuesti projekti avaldada. Lisateavet leiate [Azure'i rakenduse teenuse juurutusjuhend]. järgmist toimingut kasutatakse Git hoidla Kiirjuhend projekti koodi alla laadida.

1. Installige Git, kui te pole seda juba teinud. Git installimiseks vaja juhiseid erinevad operatsioonisüsteemid. Vaadake [Installimist Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) operatsioonisüsteemi kohased jagamisel ja installimise juhiseid.

2. Järgige [rakenduse teenuse rakenduse hoidla lubamine](../app-service-web/app-service-deploy-local-git.md#Step3) lubamiseks Git hoidla taustväärtus saidi, teha märkuse juurutamise kasutajanimi ja parool.

3. Teie Mobile'i rakendus kirjutamata labale märkige **Git klooni URL-i** sätte.

4. Käivitada soovitud `git clone` käsk Git klooni URL-i, sisestage parool, kui see on nõutav, nagu järgmises näites:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

5. Liikuge sirvides kohalikku kausta, mis eelmises näites on /todolist, ja pöörake tähelepanu sellele, projekti faile alla laadida. Otsige üles soovitud `todoitem.json` faili selle `/tables` kataloogi.  See fail määratleb tabeli õigused.  Leiate ka selle `todoitem.js` fail samas kaustas, mis määratleb selle CRUD-toimingut skriptid tabeli.

6. Pärast muudatuste tegemist projekti faile, käivitada lisamine Kinnita ja seejärel laadida saidi järgmised käsud:

        $ git commit -m "updated the table script"
        $ git push origin master

    Uute failide lisamisel projekt, peate esmalt käivitada soovitud `git add .` käsk.

Saidi on uuesti iga kord, kui uus komplekt võtab on lükata saidile.

### <a name="howto-publish-to-azure"></a>Kuidas: Azure'i oma Node.js taustväärtus avaldamine

Microsoft Azure'i pakub palju vahendid avaldada oma Azure'i rakenduse teenuse Mobile'i rakendused Node.js kirjutamata Azure'i teenus.  Nendeks, kasutades integreeritud Visual Studio juurutamise tööriistad, käsurea tööriistad ja pidev Juurutussuvandid andmeallika juhtelemendi põhjal.  Selle teema kohta leiate lisateavet teemast [Azure rakenduse teenuse juurutusjuhend].

Azure'i rakenduse teenus on teatud soovitused Node.js rakendus, mis teil peaks läbi vaadata enne juurutamist.

- Kuidas [määrata sõlm versioon]
- Kuidas [kasutada sõlm moodulid]

### <a name="howto-enable-homepage"></a>Kuidas: rakenduse avalehel lubamine

Paljud rakendused on web ja mobiilirakendused kombinatsiooni ja ExpressJS raames võimaldab teil ühendada kaks aspekte.  Mõnikord, võite rakendada ainult mobile liides.  See on kasulik näha sihtleht, et tagada teenuse rakendus on tööks.  Saate anda oma avalehe või lubada ajutist avalehel.  Kasutage ajutist avalehel lubamiseks väärtustada Azure'i mobiilirakenduste kohta järgmist:

    var mobile = azureMobileApps({ homePage: true });

Kui soovite see suvand on saadaval ainult kohalikult väljatöötamisel, saate seda sätet oma `azureMobile.js` faili.

## <a name="TableOperations"></a>Tabeli toimingud 

Azure'i mobiilirakenduste Node.js Server SDK pakub menetlustele esitamist talletatud Azure SQL andmebaasis on WebAPI andmetabelid.  Viis toimingud on saadaval.

| Toiming | Kirjeldus |
| --------- | ----------- |
| /Tables/_tablename_ hankimine | Tabeli kõigi kirjete hankimine |
| /Tables/_tablename_/:id hankimine | Kindlale kirjele tabelis hankimine |
| POSTITUSE /tables/_tablename_ | Kirje loomine tabelis |
| PAIK /tables/_tablename_/:id | Kirje tabeli värskendamine |
| /Tables/_tablename_/:id kustutamine | Tabeli kirje kustutamine |

See WebAPI [OData] toetab ja laiendab tabeli skeemi toetamiseks [ühenduseta sünkroonimine].

### <a name="howto-dynamicschema"></a>Kuidas: tabelite abil dünaamiline skeemi määratlemine

Enne tabeli saab kasutada, tuleb see määratleda.  Tabeleid saab määratleda staatiline skeem (kus arendaja määratleb veergude skeemiga jooksul) või dünaamiliselt (kui SDK kontrollib sissetulevad taotlused põhineva skeemi). Lisaks saate lisada JavaScripti koodi määratluse arendaja kindlate aspektide soovitud WebAPI määrata.

Hea tava, tuleks määratleda iga tabeli Javascript faili kataloogis tabelid ja seejärel tables.import() meetodi abil importida tabelid.  Ulatub rakenduse basic, tuleks kohandada app.js faili:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Tabeli määratlus. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Tabelite kasutamine dünaamiline skeemi vaikimisi.  Dünaamiliste skeemi globaalselt väljalülitamiseks rakenduse sätte **MS_DynamicSchema** väärtuseks false Azure portaali.

Täieliku näite leiate [todo valimi github].

### <a name="howto-staticschema"></a>Kuidas: tabelite abil skeemi staatilise määratlemine

Saate määratleda konkreetselt esitamist kaudu soovitud WebAPI veerud.  Azure'i mobiilirakenduste Node.js SDK lisab automaatselt lisaveergudest, mis on nõutav loend, mis pakub ühenduseta sünkroonimise.  Näiteks Kiirjuhend klientrakendustes nõuda kahe veeruga tabeli: teksti (stringi) ja täitke (kahendmuutuja).  
Tabeli saab määratleda tabeli määratlus JavaScript faili (asub kataloogis tabelid) järgmiselt:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Kui määratlete tabelite staatiliselt, siis peate ka helistada tables.initialize() meetodi abil luua andmebaasi skeemi käivitamisel.  Tables.initialize() meetod tagastab [lubadus] , et veebiteenuse ei pakuta taotlusi enne andmebaasi on lähtestatud.

### <a name="howto-sqlexpress-setup"></a>Kuidas: kasutada SQL Expressi nimega arengu andmesalve oma kohalikus arvutis

Azure'i Mobile'i rakendused The AzureMobile rakenduste sõlm SDK pakub andmete serveeritakse välja kasti kolm võimalust: SDK pakub andmete serveeritakse välja kasti kolm võimalust:

- **Mälu** draiveri abil kujutavad endast püsiv näide
- Andma SQL Expressi andmesalve arengu **mssql** draiveri abil
- **Mssql** draiveri abil saate sisestada ka Azure'i SQL-andmebaasi andmesalve tootmiseks

Azure'i Mobile'i rakendused Node.js SDK kasutab [mssql Node.js paketi] luua ja kasutada SQL Express ja SQL-andmebaasi ühendus.  Selle paketi jaoks on vaja TCP-ühenduste lubamine SQL Expressi eksemplari.

> [AZURE.TIP]Mälu draiver ei paku vahendid testimine täielik kogum.  Kui soovite oma kirjutamata kohalikult testimiseks, soovitame kasutada SQL Expressi andmesalve ja mssql draiver.

1. Laadige alla ja installige [Microsoft SQL Server 2014 Express].  Veenduge, et installite SQL Server 2014 Expressi tööriistad Edition.  Kui vajate konkreetselt 64-bitine tugi, 32-bitine versioon tarbib vähem mälu käivitamisel.

2. Käivitage SQL serveri 2014 konfigureerimishaldur.

  1. Laiendage **SQL serveri võrgukonfiguratsioon** puu vasakpoolses menüüs.
  2. Klõpsake **SQLEXPRESSIS Protokollid**.
  3. Paremklõpsake **TCP/IP** ja valige **Luba**.  Klõpsake hüpikakna dialoogiboksis nuppu **OK** .
  4. Paremklõpsake **TCP/IP** ja valige **Atribuudid**.
  5. Klõpsake vahekaarti **IP-aadressid** .
  6. Otsige üles sõlm **IPAll** .  Sisestage väljale **TCP Port** **1433**.

         ![Configure SQL Express for TCP/IP][3]

  7. Klõpsake nuppu **OK**.  Klõpsake hüpikakna dialoogiboksis nuppu **OK** .
  8. Puu vasakpoolses menüüs nuppu **SQL Serveri teenuseid** .
  9. Paremklõpsake **SQL serveri (SQLEXPRESSIS)** ja valige **taaskäivitamine**
  10. SQL serveri 2014 konfigureerimishaldur sulgemine

3. Käivitage SQL Server Management 2014 Studio ja ühenduse loomine kohaliku SQL Expressi eksemplari

  1. Paremklõpsake objekti Exploreri oma eksemplar ja valige **Atribuudid**
  2. Valige vahekaart **Turve** .
  3. Veenduge, et **SQL serveri ja Windowsi autentimisrežiimis** on valitud
  4. Klõpsake nuppu **OK**

        ![SQL-i kiire autentimise konfigureerimine][4]

  5. Laiendage **Turvalisus** > Exploreris objekt**logimine**
  6. Paremklõpsake **sisselogimise** ja valige **Uus Logi sisse...**
  7. Sisestage kasutajanimi.  Valige **SQL serveri autentimine**.  Sisestage parool, seejärel sisestage sama parooli **kinnitus**parool.  Parooli peab vastama Windows keerukuse nõuetele.
  8. Klõpsake nuppu **OK**

        ![SQL-i kiire uue kasutaja lisamine][5]

  9. Paremklõpsake oma uue Logi sisse ja valige **Atribuudid**
  10. Valige lehe **Serverirollid**
  11. Märkige ruut **dbcreator** serveri roll
  12. Klõpsake nuppu **OK**
  13. Sulgege SQL Server 2015 Management Studio

Veenduge, et kasutajanimi ja parool, mille valisite sätte lindistamist.  Võimalik, et peate täiendavate serverirollide või vastavalt oma vajadustele kindla andmebaasi õiguste määramine.

Node.js rakendus tuvastab **SQLCONNSTR_MS_TableConnectionString** keskkonna muutuja jaoks selle andmebaasi ühendusstring.  Saate määrata muutuja teie keskkonnas.  Näiteks saate määrata muutuja keskkonna PowerShelli abil:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Juurdepääs andmebaasile läbi TCP/IP ja sisestage kasutajanimi ja parool ühenduse.

### <a name="howto-config-localdev"></a>Kuidas: projekti arengu konfigureerimine

Azure'i mobiilirakenduste loeb JavaScripti fail nimega _azureMobile.js_ kohaliku failisüsteemi kaudu.  Kasutage seda faili konfigureerida Azure Mobile'i rakendused SDK valmistamisel – kasutage selle asemel rakenduse sätete [Azure portaali] .  _AzureMobile.js_ faili eksportida konfiguratsiooni objekti.  On kõige levinum sätted.

- Andmebaasi sätted
- Diagnostikalogimise sätted
- Alternatiivsete CORS sätted

Näiteks _azureMobile.js_ faili eelnev andmebaasi sätete rakendamiseks järgmiselt:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Soovitame lisada _azureMobile.js_ _.gitignore_ faili (või muude lähtekoodi jälgimine ignoreerib fail) vältimiseks paroolid on salvestatud pilveteenuses.  Alati tootmise sätete konfigureerimine jaotises rakenduse sätted [Azure portaali].

### <a name="howto-appsettings"></a>Kuidas: Teie Mobile'i rakendus rakenduse sätete konfigureerimine

Enamik _azureMobile.js_ faili sätted on samaväärne säte rakenduse [Azure portaali].  Järgmises loendis abil saate oma rakenduse rakenduse sätete konfigureerimine.

| Rakenduse säte                 | _azureMobile.js_ säte  | Kirjeldus                               | Kehtivad väärtused                                |
| :-------------------------- | :------------------------ | :---------------------------------------- | :------------------------------------------ |
| **MS_MobileAppName**        | Nimi                      | Rakenduse nimi                       | string                                      |
| **MS_MobileLoggingLevel**   | Logging.Level             | Logige sõnumite minimaalne Logi tase      | tõrge, hoiatus, teave, Paljusõnaline, silumine rumal |
| **MS_DebugMode**            | silumine                     | Luba või Keela silumine režiim              | True, false                                 |
| **MS_TableSchema**          | Data.Schema               | SQL-i tabelite skeemi vaikenime        | string (vaikimisi: dbo)                       |
| **MS_DynamicSchema**        | data.dynamicSchema        | Luba või Keela silumine režiim              | True, false                                 |
| **MS_DisableVersionHeader** | versioonis (määratlemata)| Keelab päise X-ZUMO serveri versioon | True, false                                 |
| **MS_SkipVersionCheck**     | skipversioncheck          | Keelab kliendi API versiooni kontroll     | True, false                                 |

Rakenduse mõne sätte seadmiseks tehke järgmist.

1. [Azure'i portaali]sisse logida.
2. Valige **kõik ressursid** või **Rakenduse teenused** klõpsake nuppu Mobile rakenduse nimi.
3. Sätete tera avaneb vaikimisi. Kui seda pole, klõpsake nuppu **sätted**.
4. Klõpsake menüüs üldine **rakenduse sätted** .
5. Liikuge kerides jaotisse rakenduse sätted.
6. Kui rakenduse seade juba olemas, klõpsake rakenduse sätte redigeerimiseks väärtus väärtus.
7. Kui teie rakendus säte olemas, sisestage väljadele võti ja väljale väärtus väärtus rakenduse säte.
8. Kui olete lõpule jõudnud, klõpsake nuppu **Salvesta**.

Enamik rakenduse sätete muutmise jaoks on vaja teenuse uuesti.

### <a name="howto-use-sqlazure"></a>Kuidas: kasutada SQL-andmebaasi tootmise andmete salvestamiseks

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Azure'i SQL-andmebaasi kasutamine andmete poe on identne üle kõik Azure'i rakendust Service rakenduste andmetüübid. Kui te pole seda veel teinud, tehke Mobile'i rakendus taustväärtus loomiseks.

1. [Azure'i portaali]sisse logida.

2. Top akna vasakule, klõpsake nuppu **+ Uus** > **Web + Mobile** > **Mobile'i rakendus**, seejärel sisestage oma mobiilirakenduse kirjutamata nimi.

3. Tippige väljale **Ressursirühm** oma rakendusena sama nimi.

4. Vaikimisi rakenduse teenusepakett on märgitud.  Kui soovite muuta oma rakenduse teenusleping, saate seda teha, klõpsates nuppu rakenduse teenusleping > **+ Loo uus**.  Sisestage nimi uue rakenduse teenusleping ja valige soovitud asukoht.  Klõpsake hinnakirjad taseme ja valige sobiv hinnakirjad taseme, teenuse kohta. Valige **Kuva kõik** Kuva rohkem suvandeid hinnakirjad, nt **tasuta** ja **ühiskasutuses**.  Kui olete valinud hinnakirjad taseme, klõpsake nuppu **Vali** .  Olles tagasi kohas **rakenduse teenusleping** tera, klõpsake nuppu **OK**.

5. Klõpsake nuppu **Loo**. Ettevalmistamise mobiilirakenduse kirjutamata võib kuluda paar minutit.  Kui kirjutamata Mobile'i rakendus on ette valmistatud, avaneb portaali **sätted** höövlitera Mobile'i rakendus taustväärtus.

Kui kirjutamata Mobile'i rakendus on loodud, saate valida olemasoleva SQL-i andmebaasiga ühenduse loomine oma mobiilirakenduse kirjutamata või luua uue SQL-i andmebaasi.  Selles jaotises loome SQL-andmebaasi.

> [AZURE.NOTE]Kui teil on juba andmebaasi mobiilirakenduse kirjutamata samasse asukohta, saate selle asemel valida **olemasoleva andmebaasi kasutamine** ja valige selle andmebaasi. Mõnda muusse andmebaasi kasutamine ei ole soovitatav kõrgema latentsused tõttu.

6. Uue mobiilirakenduse kirjutamata, klõpsake nuppu **sätted** > **Mobile'i rakendus** > **andmete** > **+ Lisa**.

7. Valige **Lisa andmeühenduse** labale **SQL-andmebaasi – nõutav sätete konfigureerimine** > **Loo uus andmebaas**.  Sisestage väljale **nimi** uue andmebaasi nimi.

8. Klõpsake käsku **Server**.  **Uue serveri** tera, Sisestage kordumatu serveri nimi väljale **Serveri nimi** ja sobiva **serveri administraatori kasutajanime** ja **parooli**.  Veenduge, et **serveri juurdepääsu luba azure teenused** on märgitud.  Klõpsake nuppu **OK**.

    ![SQL Azure'i andmebaasi loomine][6]

9. Enne **uue andmebaasi** , klõpsake nuppu **OK**.

10. Uuesti sisse **Lisa andmeühenduse** tera, valige **ühendusstring**, sisestage kasutajanimi ja parool, mida andmebaasi loomisel sisestatud.  Kui kasutate olemasoleva andmebaasiga, sisestage selle andmebaasi sisselogimise.  Kui sisestanud, klõpsake nuppu **OK**.

11. **Andmeühenduse lisamine** enne uuesti, klõpsake nuppu **OK** , et luua andmebaasi.

<!--- END OF ALTERNATE INCLUDE -->

Selle andmebaasi loomiseks võib kuluda mõni minut.  Kasutage **teatiste** ala juurutamise jälgimiseks.  Edu kuni andmebaas on edukalt juurutatud.  Pärast edukalt juurutatud, luuakse ühendusstringi SQL-andmebaasi eksemplari sisse oma mobiilse taustväärtus rakenduse sätted.  Saate vaadata selle rakenduse sätte **sätted** > **rakenduse sätted** > **ühendusstringi**.

### <a name="howto-tables-auth"></a>Kuidas: nõuda autentimist tabelite kohta

Kui soovite kasutada rakenduse autentimise tabelite lõpp-punkti, tuleb konfigureerida rakendus autentimise [Azure'i portaalis] esmalt.  Azure'i rakenduse teenuse autentimise konfigureerimise kohta lisateabe saamiseks vaadata kavatsete kasutada identiteedipakkuja konfiguratsiooni juhend:

- [Azure Active Directory autentimise konfigureerimine]
- [Facebooki autentimise konfigureerimine]
- [Google autentimise konfigureerimine]
- [Kuidas konfigureerida Microsoft Authentication]
- [Twitteri autentimise konfigureerimine]

Igas tabelis oleks juurdepääs atribuut, mida saab kasutada tabeli juurdepääsu piiramiseks.  Järgmises näites kuvatakse staatiliselt määratletud tabel autentimise nõutav.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Atribuudi Accessi saate teha ühte kolmest väärtusest

  - *anonüümse* näitab, et klientrakenduse on lubanud ilma autentimise andmete lugemine
  - *autenditud* näitab, et klientrakenduse peate saatma taotluse luba kehtiv autentimine
  - *keelatud* näitab, et selles tabelis on praegu keelatud

Kui access atribuut on määramata, autentimata juurdepääs on lubatud.

### <a name="howto-tables-getidentity"></a>Kuidas: tabelite taotluste autentimise kasutamine

Saate häälestada erinevad nõuded, mis on nõutav, kui autentimine on häälestatud.  Nende nõuete ei ole tavaliselt kättesaadav kaudu soovitud `context.user` objekti.  Siiski ta saab tuua, kasutades funktsiooni `context.user.getIdentity()` meetod.  Funktsiooni `getIdentity()` meetod tagastab lubadus, mille lahenduseks objekti.  Objekti te ei sisesta autentimise meetodit (Facebooki, google, Twitteri, microsoftaccount või aad).

Näiteks kui häälestate Microsofti Account autentimis- ja taotluse taotlemine e-posti aadressid, saate lisada meiliaadressi kirje järgmises tabelis kontrolleril abil:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Vaadata, millised nõuded on saadaval, kasutage kuvamiseks veebibrauseris soovitud `/.auth/me` saidi lõpp-punkti.

### <a name="howto-tables-disabled"></a>Kuidas: keelamine juurdepääsu teatud tabeli toimingud

Lisaks kuvataks tabel, saab atribuudi juurdepääsu piiramiseks tegevuste.  Leidub neli toimingud:

  - *Lugege* on rahulik saada tabeli toiming
  - Rahulik postituse toimingu tabeli *lisamine* on
  - *Värskendage* on tabeli rahulik paik toiming
  - *kustutamine* on tabeli rahulik kustutamine toimingut

Näiteks võite soovida kirjutuskaitstud autentimata tabel:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Kuidas: päringut, mida kasutatakse tabeli toimingutega reguleerimine

Tabeli toimingute üldine tingimus on piiratud vaade andmed.  Näiteks saate esitada tabeli, mis on sildistatud autenditud kasutaja ID-ga nii, et ainult saate lugeda või värskendada oma kirjeid.  Järgmine tabel määratlus pakub seda funktsiooni.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Toimingud, mis tavaliselt päringut on päringu atribuut, mida saate kohandada, kus koos klausel. Päringu atribuut on [QueryJS] objekti, mis on päring OData teisendamiseks midagi, mida saate protsessi andmete taustväärtus.  Lihtne võrdse juhtudel (nt eelnev üks) saab kasutada kaardi. Saate lisada ka teatud SQL-klauslitest:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Kuidas: konfigureerimine pehmete Kustuta tabel

Pehmete Kustuta tegelikult ei kustutata kirjeid.  Selle asemel see tähistab neid kustutada andmebaasi jooksul kustutatud veeru seadmisega tõene.  Kui Mobile kliendi SDK kasutab IncludeDeleted(), eemaldab Azure'i Mobile'i rakendused SDK pehmete kustutatud kirjete tulemustest.  Konfigureerige pehmete Kustuta tabel, seadke soovitud `softDelete` definitsioonifail tabeli atribuudi:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Mida tuleks luua puhastamine kirjete – kas klientrakendusega WebJob, Azure funktsioon või kohandatud API kaudu.

### <a name="howto-tables-seeding"></a>Kuidas: seeme andmebaasi andmetega

Kui loote uue rakenduse, soovi korral võite seeme tabeli andmetega.  Seda saab teha tabeli määratlus JavaScripti failis järgmiselt:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Andmete külv ainult teha, kui tabel on loodud Azure'i Mobile'i rakendused SDK.  Kui tabel on juba olemas andmebaasi sees, süstitakse tabeli andmed.  Kui dünaamiline skeemi on sisse lülitatud, siis skeemiga on tuletatud seemnetega andmetest.

Soovitame, et helistate konkreetselt funktsiooni `tables.initialize()` meetod teenuse käivitub tabeli loomiseks.

### <a name="Swagger"></a>Kuidas: ärplema toe lubamine

Azure'i rakenduse teenuse mobiilirakenduste kaasas sisemised [ärplema] tugi.  Ärplema toe lubamiseks installige esmalt ärplema-ui sõltuv.

    npm install --save swagger-ui

Kui installitud, saate lubada Azure mobiilirakenduste ehitaja ärplema tugi.

    var mobile = azureMobileApps({ swagger: true });

Saate ilmselt ainult soovite arengu väljaanded ärplema toe lubamine.  Saate seda teha, kasutades funktsiooni `NODE_ENV` rakenduse säte:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Http://_yoursite_.azurewebsites.net/swagger asub ärplema lõpp-punkti.  Ärplema Kasutajaliidese kaudu pääsete juurde selle `/swagger/ui` lõpp-punkti.  Kui valite nõuab autentimist üle kogu rakenduse, ärplema annab tulemiks veaväärtuse.  Parimate tulemuste saamiseks valige lubada autentimata taotlused kaudu Azure'i rakenduse teenuse autentimise / loa sätted, seejärel kontrolli autentimise abil soovitud `table.access` atribuut.

Saate lisada ka ärplema võimalus oma `azureMobile.js` faili, kui soovite ainult ärplema tugi, kohalik väljatöötamisel.

## <a name="a-namepushpush-notifications"></a><a name="push">Tõuketeatised

Mobiilirakenduste integreerub Azure'i teatis jaoturi selleks, et saaksite saata suunatud Tõuketeatiste miljoneid seadmeid üle kõigi suuremate platvormid. Teatis jaoturi abil saate saata Tõuketeatiste iOS-i, Androidi ja Windowsi seadmed. Lisateavet kõik, mida saate teha teatise jaoturi leiate teemast [Teatise jaoturi ülevaade](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Kuidas: Tõuketeatiste saatmine

Järgmine kood näitab, kuidas kasutada tõuketeatised objekti leviedastuse tõuketeatised teatise saatmine registreeritud iOS-seadmete:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Luues malli tõuketeatised registreerimise klient, saate malli tõuketeatised sõnumeid saata selle asemel kõik toetatud platvormid seadmed. Järgmine kood näitab, kuidas malli teatise saatmine:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


###<a name="push-user"></a>Kuidas: saada Tõuketeatiste autenditud kasutaja siltide kasutamine

Kui autenditud kasutaja registreerib Tõuketeatiste, lisatakse automaatselt kasutaja ID sildi registreerimise. Selle sildiga abil saate saata Tõuketeatiste registreeritud mõne kindla kasutaja kõigis seadmetes. Järgmine kood saab taotluse kasutaja SID ja saadab malli tõuketeatised teate, et iga seadme registreerimine kasutaja jaoks.

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Registreerumisel Tõuketeatiste autenditud klient, veenduge, et autentimine on lõpule viidud enne, kui proovite registreerimise.

## <a name="CustomAPI"></a>Kohandatud API-d

###  <a name="howto-customapi-basic"></a>Kuidas: kohandatud API määratlemine


Lisaks andmete juurdepääs API /tables lõpp-punkti kaudu, Azure'i mobiilirakenduste kohta saate sisestada kohandatud API ulatus.  Kohandatud API-d on määratletud sarnaselt tabeli määratlused ja pääsete juurde kõik samad vahendid, sh autentimine.

Kui soovite kasutada rakenduse autentimise kohandatud API-ga, tuleb konfigureerida rakendus autentimise [Azure'i portaalis] esmalt.  Azure'i rakenduse teenuse autentimise konfigureerimise kohta lisateabe saamiseks vaadata kavatsete kasutada identiteedipakkuja konfiguratsiooni juhend:

- [Azure Active Directory autentimise konfigureerimine]
- [Facebooki autentimise konfigureerimine]
- [Google autentimise konfigureerimine]
- [Kuidas konfigureerida Microsoft Authentication]
- [Twitteri autentimise konfigureerimine]

Kohandatud API-d on määratletud palju samamoodi nagu tabelite API-ga.

1. Mõne **api** kataloogi loomine
2. Looge API definitsioonifail JavaScripti **api** kataloogis.
3. Impordi meetodi abil saate importida **api** kataloogi.

Siin on prototüüp api määratlus kasutasime varem basic-rakenduse valimi põhjal.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Vaatame API, mis tagastab serveri kuupäeva _Date.now()_ meetodi kasutamise näide.  Siin on api/date.js faili:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Iga parameetri on üks standard rahulik verbide - toomine, postituse, paik või Kustuta.  Meetod on standardne [ExpressJS vahevara] funktsiooni, mis saadab nõutav väljundi.

### <a name="howto-customapi-auth"></a>Kuidas: nõua juurdepääsuks kohandatud API autentimine

Azure'i Mobile'i rakendused SDK rakendab autentimise tabelite lõpp-punkti ja kohandatud API samal viisil.  Autentimine väljatöötatud eelmises jaotises API, lisage soovitud **Accessi** atribuut:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Saate määrata autentimise teatud toimingute kohta:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Samamoodi, mida kasutatakse tabeli lõpp-punkti tuleb kasutada kohandatud API-d, mis nõuab autentimist.

### <a name="howto-customapi-auth"></a>Kuidas: toime suure faili üleslaadimine

Azure'i Mobile'i rakendused SDK kasutab [keha-parseri vahevara](https://github.com/expressjs/body-parser) aktsepteerimiseks ja dekodeerida oma sisu.  Saate konfigureerida eelnevalt keha-parseri suuremat faili lisatud kinnitamiseks.

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Fail on base-64-kodeeritud enne edastamist.  Suurendab mahtu tegeliku üles (ja seega suuruse te peate konto).

### <a name="howto-customapi-sql"></a>Kuidas: käivitada kohandatud SQL-lauseid

Azure'i Mobile'i rakendused SDK võimaldab juurdepääsu kogu konteksti kaudu taotluse objekti, mis võimaldab teil käivitada parameetriga määratletud andmepakkuja SQL-lauseid hõlpsasti:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Silumine lihtne tabelid ja lihtne API-d

### <a name="howto-diagnostic-logs"></a>Kuidas: silumine, diagnoosimine ja Azure mobiilirakenduste tõrkeotsing

Azure'i rakenduse teenus pakub paljusid silumine ja tõrkeotsingu tehnikate Node.js rakenduste.
Vaadake järgmisi artikleid, et alustada oma Node.js Mobile taustväärtus tõrkeotsing:

- [Azure'i rakenduse teenuse jälgimine]
- [Azure'i rakenduse teenuses diagnostikalogimise lubamiseks]
- [Rakenduse Azure teenuse Visual Studio tõrkeotsing]

Mitmesuguseid diagnostika log tööriistad Node.js rakenduste juurde.  Sees, kasutab Azure Mobile'i rakendused Node.js SDK [Winston] diagnostikalogimine.  Logimine on lubatud automaatselt, võimaldades silumine režiimis või määrata **MS_DebugMode** rakenduse säte True [Azure portaali]. Loodud logide kuvatakse [Azure portaali]Diagnostikalogid.

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Kuidas: Azure'i portaalis lihtne tabeliga töötamine

Lihtne tabelite portaalis teile loomine ja nendega töötamine tabelite otse portaalist. Saate redigeerida ka tabeli toimingute abil rakenduse teenuse redaktor.

Kui klõpsate kirjutamata saidisätetes **lihtne tabelid** , saate lisada, muuta või tabeli kustutamine. Näete tabeli andmed.

![Lihtne tabeliga töötamine](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Järgmised käsud on saadaval käsuribal tabeli.

+ **Muuda õigusi** – Muuda õigusi lugeda, lisada, värskendada ja kustutada tabeli toimingud. 
  Suvandid on anonüümse juurdepääsu, nõuab autentimist või toimingu kõigi juurdepääsu lubamiseks. 
+ **Skripti redigeerimine** - skriptifail tabeli avatakse rakenduse teenuse redaktor.
+ **Haldamine skeemi** - lisada või kustutada veerge või tabeli registrit muuta.
+ **Tühjenda tabel** - kärbib olemasolevasse tabelisse kõik andmed ridade kustutamine, kuid jättes skeemiga muutmata.
+ **Kustuta read** – Kustuta üksikute andmeread.
+ **Logide streaming vaade** – loob ühenduse streaming Logi teenuse saidile.

###<a name="work-easy-apis"></a>Kuidas: Azure'i portaalis lihtne API-de töötamine

Lihtne API-de portaalis teile loomine ja nendega töötada kohandatud API-de otse portaalist. Saate redigeerida API skriptide abil rakenduse teenuse redaktor.

Kui klõpsate kirjutamata saidisätetes **Lihtne API -d** , saate lisada, muuta või kohandatud API lõpp-punkti kustutamine.

![Lihtne API-de töötamine](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

Portaalis saate muuta juurdepääsuõigusi antud HTTP toimingu, redigeerida API skriptifail rakenduse teenuse redaktor või vaadata streaming logid.

###<a name="online-editor"></a>Kuidas: rakenduse teenuse redaktor-koodi redigeerimine

Azure portaali saate redigeerida oma Node.js kirjutamata skripti faile rakenduse teenuse redaktor ilma projekti kohalikku arvutisse allalaadimiseks. Skripti faile veebis redaktoris redigeerimiseks tehke järgmist.

1. Mobiilirakenduse kirjutamata tera, valige **Kõik sätted** > kas **lihtne tabelite** või **Lihtne API -d**, tabeli või API, klõpsake käsku **Redigeeri skripti**. Skripti faili avamisel rakenduse teenuse redaktor.

    ![Rakenduse teenuse redaktor](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)

2. Tehke soovitud muudatused koodi faili online redaktoris. Tippimise ajal salvestatakse muudatused automaatselt.


<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Androidi kliendi Kiirjuhend]: app-service-mobile-android-get-started.md
[Apache Cordova kliendi Kiirjuhend]: app-service-mobile-cordova-get-started.md
[iOS-i kliendi Kiirjuhend]: app-service-mobile-ios-get-started.md
[Xamarin.iOS kliendi Kiirjuhend]: app-service-mobile-xamarin-ios-get-started.md
[Xamarin.Android kliendi Kiirjuhend]: app-service-mobile-xamarin-android-get-started.md
[Xamarin.Forms kliendi Kiirjuhend]: app-service-mobile-xamarin-forms-get-started.md
[Windowsi poe kliendi Kiirjuhend]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[ühenduseta sünkroonimine]: app-service-mobile-offline-data-sync.md
[Azure Active Directory autentimise konfigureerimine]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebooki autentimise konfigureerimine]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google autentimise konfigureerimine]: app-service-mobile-how-to-configure-google-authentication.md
[Kuidas konfigureerida Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitteri autentimise konfigureerimine]: app-service-mobile-how-to-configure-twitter-authentication.md
[Azure'i rakendust Service juurutusjuhend]: ../app-service-web/web-sites-deploy.md
[Azure'i rakenduse teenuse jälgimine]: ../app-service-web/web-sites-monitor.md
[Azure'i rakenduse teenuses diagnostikalogimise lubamiseks]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Rakenduse Azure teenuse Visual Studio tõrkeotsing]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[Määrake sõlm versioon]: ../nodejs-specify-node-version-azure-apps.md
[Kasutage sõlm moodulid]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Ärplema]: http://swagger.io/

[Azure'i portaal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Lubadus]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[github basicapp näidis]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[github Todo näidis]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[näidised directory github]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Visual Studio Node.js Tööriistad 1.1]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[MSSQL Node.js pakett]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS vahevara]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
