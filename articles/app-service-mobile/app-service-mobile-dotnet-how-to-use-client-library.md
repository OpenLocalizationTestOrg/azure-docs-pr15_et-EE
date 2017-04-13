<properties
    pageTitle="Töötamine rakenduse teenuse mobiilirakenduste hallatavate kliendi teek (Windowsi | Xamarin) | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada .net-i kliendi Azure'i rakenduse teenuse mobiilirakenduste Windows ja Xamarin rakendused."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>Kuidas kasutada hallatavate kliendi Azure'i mobiilirakenduste kohta

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

##<a name="overview"></a>Ülevaade

Sellest juhendist näitab, kuidas teha levinud stsenaariumi klientide teegi kasutamise Azure'i teenus Mobile'i rakendused Windowsi meilirakendus ja Xamarin rakendused. Kui olete kasutanud mobiilirakenduste, võiksite kaaluda esmalt lõpuleviimine [Azure'i mobiilirakenduste Kiirjuhend] [ 1] õpetuse. Sellest juhendist me keskenduda kliendipoolne hallatavate SDK. Lisateavet serveripoolne SDK-d mobiilirakenduste, dokumentatsioonist [.net-i Server SDK] [ 2] või [Node.js Server SDK][3].

## <a name="reference-documentation"></a>Dokumentides

Viide dokumentatsioonist kliendi SDK asub siin: [Azure'i Mobile'i rakendused .net-i kliendi viide][4].
Leiate ka mitu kliendi proovi [Azure-näidised GitHub hoidla][5].

## <a name="supported-platforms"></a>Toetatud platvormid

.NET platvorm toetab järgmiste platvormide.

* Xamarin Androidi väljalasete jaoks API 19 kuni 24 (KitKat pähklimass kaudu)
* IOS-i Xamarin väljastab iOS 8.0 ja uuemad versioonid
* Universaalne Windowsi platvormi
* Windows Phone 8.1
* Windows Phone 8.0, välja arvatud Silverlighti rakendused

"Serveri rahavoogude" autentimise kasutab WebView esitatud UI.  Kui seade ei ole võimalik esitada WebView UI, siis muid viise autentimine on vaja.  See SDK sobib seega ei Vaata-tüüpi või sarnaselt piiratud seadmetes.

##<a name="setup"></a>Häälestamise ja eeltingimused

Me oletame, et teil on juba loodud ja avaldatud mobiilirakenduse kirjutamata projekti, mis sisaldab vähemalt üks tabel.  Selles teemas koodile tabeli nimi on `TodoItem` ja see sisaldab järgmisi veerge: `Id`, `Text`, ja `Complete`. Selles tabelis on loodud, kui täidate Azure'i mobiilirakenduste Kiirjuhend sama tabel.

Vastavate tipitud kliendipoolne tüüp C# on järgmised klassi.

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    }

[JsonPropertyAttribute] [ 6] võimaldab määratleda *PropertyName* vastenduse kliendi tüüp ja tabeli vahel.

Saate teada, kuidas oma mobiilirakenduste kirjutamata tabelite loomine, lugege teemat [.net-i Server SDK teema] teave[ 7] või [Node.js Server SDK teema][8]. Kui olete loonud oma mobiilirakenduse kirjutamata Azure portaali kaudu on Kiirjuhend, saate kasutada ka **lihtne tabelite** säte [Azure portaali].

###<a name="how-to-install-the-managed-client-sdk-package"></a>Tehke järgmist: installige klientide SDK pakett

Üht järgmistest meetoditest mobiilirakenduste kaudu [Nugeti]hallatavate kliendi SDK paketi installimiseks kasutada[9]:

+ **Visual Studio** Paremklõpsake oma projekti, klõpsake nuppu **Haldamine Nugeti pakettide**, otsige soovitud `Microsoft.Azure.Mobile.Client` paketti ja seejärel klõpsake nuppu **Installi**.

+ **Xamarin Studio** Paremklõpsake oma projekti, klõpsake nuppu **Lisa** > **Lisada Nugeti pakettide**, otsige soovitud `Microsoft.Azure.Mobile.Client `paketti ja seejärel klõpsake nuppu **Lisa paketi**.

Tegevuse peamine failis meeles lisada lause **abil** :

    using Microsoft.WindowsAzure.MobileServices;

###<a name="symbolsource"></a>Kuidas: silumine sümbolid Visual Studio töötamine

Nimeruumi Microsoft.Azure.Mobile sümbolid on saadaval [SymbolSource][10].  Vaadake [juhiseid SymbolSource] [ 11] SymbolSource integreerida Visual Studio.

##<a name="create-client"></a>Mobiilirakenduste kliendi loomine

Järgmine kood tekitab [MobileServiceClient] [ 12] objekt, mida kasutatakse juurdepääsuks oma Mobile'i rakendus taustväärtus.

    var client = new MobileServiceClient("MOBILE_APP_URL");

Eelmise koodi asendamine `MOBILE_APP_URL` mobiilirakenduse kirjutamata URL, mis on leitud höövlitera oma mobiilirakenduse kirjutamata [Azure portaali]. Objekti MobileServiceClient peaks olema on singleton.

## <a name="work-with-tables"></a>Töötamine tabelitega

Järgmises jaotises üksikasjad, kuidas otsida ja tuua kirjete ja tabelis andmeid muuta.  Käsitletakse järgmisi teemasid:

* [Tabeli viite loomine](#instantiating)
* [Päringu andmete](#querying)
* [Tagastatud andmete filtreerimine](#filtering)
* [Tagastatud andmete sortimine](#sorting)
* [Tagastab andmed lehtedel](#paging)
* [Valige soovitud veerud](#selecting)
* [Otsi kirjet ID](#lookingup)
* [Untyped päringutega tegelemine](#untypedqueries)
* [Andmete lisamine](#inserting)
* [Andmete värskendamine](#updating)
* [Andmete kustutamine](#deleting)
* [Konfliktide lahendamine ja loodetav kokkulangevus](#optimisticconcurrency)
* [Windowsi kasutajaliidese sidumine](#binding)
* [Lehe suuruse muutmine](#pagesize)

###<a name="instantiating"></a>Kuidas: tabeli viite loomine

Koodi, mis kasutab või muudab seda kirjutamata tabeli andmete kutsub funktsioonid on `MobileServiceTable` objekti. Saada viide tabeli helistades [GetTable] meetod järgmiselt:

    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();

Tagastatud objekt kasutab tipitud sariväljaanne mudel. Untyped sariväljaanne mudelit, mis on samuti toetatud. Kuvatakse järgmine näide [loob viite untyped tabelisse]:

    // Get an untyped table reference
    IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");

Untyped päringute, peate määrama OData aluseks oleva päringu string.

###<a name="querying"></a>Kuidas: andmepäringuid oma Mobile'i rakendus

Selles jaotises kirjeldatakse, kuidas anda päringute kirjutamata Mobile'i rakendus, mis sisaldab järgmisi funktsioone:

- [Tagastatud andmete filtreerimine](#filtering)
- [Tagastatud andmete sortimine](#sorting)
- [Tagastab andmed lehtedel](#paging)
- [Valige soovitud veerud](#selecting)
- [Andmete ID järgi otsimine](#lookingup)

>[AZURE.NOTE]Tagastatakse kõik read takistada server põhinev leheformaadi jõustatakse.  Piipar hoiab vaikimisi taotlused suurte andmekogumite negatiivselt mõjutavad teenuse.  Rohkem kui 50 rida tagastamiseks kasutage funktsiooni `Skip` ja `Take` meetod, nagu on kirjeldatud [saatja andmete lehed].

###<a name="filtering"></a>Kuidas: filtri tagastatud andmeid

Järgmine kood näitab, kuidas filtreerida andmeid, sh on `Where` klausel päringu. Tagastab kõigi üksuste see `todoTable` nime, kelle `Complete` atribuut on võrdne `false`. [Kui] funktsioon kehtib filtreerimine predikaat päringule tabeli rea kohta.

    // This query filters out completed TodoItems and items without a timestamp.
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .ToListAsync();

Saate vaadata saadetud sõnumi kontrolli tarkvara, näiteks brauseri Arendaja tööriistad või [viiuldaja]abil soovitud taustväärtus taotluse URI. Kui vaatate taotluse URI, Pange tähele, et päringu string on muudetud.

    GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1

OData taotlus tõlgitakse SQL-päringu Server SDK:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0

Funktsioon, mis on `Where` meetod võib olla suvaline hulk tingimusi.

    // This query filters out completed TodoItems where Text isn't null
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
       .ToListAsync();

Selles näites soovite tõlkida SQL-päringu Server SDK:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0

Selle päringu ka tükeldamiseks mitmeks mitme osalaused:

    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .Where(todoItem => todoItem.Text != null)
       .ToListAsync();

Kaks võimalust on võrdväärsed ja vaheldumisi kasutada.  Endise suvand&mdash;, ühendades ühe päringus mitut predicates&mdash;kompaktsemaks ja soovitatav on.

Funktsiooni `Where` klausel toetab toimingute kohta, mida saab tõlkida OData alamhulk. Järgmised toimingud.

* Seoselisi tehtemärke (==,! =, <, < =, >, > =),
* Aritmeetilisi tehtemärke (+, -, /, *, %),
* Arvu täpsus (Math.Floor, Math.Ceiling)
* Stringifunktsioonide (pikkus, alamstringi, Asenda, IndexOf, StartsWith, EndsWith)
* Kuupäevaatribuutide (aasta, kuu, päev, tunni, minuti, teise)
* Juurdepääs objekti atribuudid ja
* Nende toimingute kombineerides avaldised.

Kui arutate, mida Server SDK toetab, võite kaaluda [OData v3 dokumentatsiooni].

###<a name="sorting"></a>Kuidas: sortimine, tagastatakse andmed

Järgmine kood illustreerib andmete sortimise kohta, sh mõne funktsiooni [OrderBy] või [OrderByDescending] päring. Tagastab see üksuste `todoTable` sorditud tõusvas järjestuses on `Text` välja.

    // Sort items in ascending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderBy(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

    // Sort items in descending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderByDescending(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

###<a name="paging"></a>Kuidas: andmeid tagastada lehtedel

Vaikimisi on taustväärtus tagastab ainult esimesed 50 rida. Tagastatud ridade arvu suurendamiseks saate [teha] meetodi. Kasutage `Take` koos taotleda kindla "lehele" kokku andmekomplekti päringu tagastatavad [Jäta] meetod. Järgmise päringu käivitamisel tagastab tabeli top kolm üksused.

    // Define a filtered query that returns the top 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

Parandatud järgmine päring ignoreerib kolm esimest tulemused ja tagastab väärtuse kolme järgmist. Selle päringu toodab teise "leht" andmed, kus kuvatakse leheküljeformaadi on kolm üksust.

    // Define a filtered query that skips the top 3 items and returns the next 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Skip(3)
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

[IncludeTotalCount] meetodit taotleb koguarv _Kõik_ kirjed, mille soovite on tagastatud, ignoreerides määratud Piipar või piiravad klauslit:

    query = query.IncludeTotalCount();

Tõeline rakenduses saate päringuid, mis on sarnane eelmises näites Piipar juhtelemendi või sarnastes UI lehtede vahel liikuda.

>[AZURE.NOTE]50-rea piirata mobiilirakenduse kirjutamata alistamiseks peab ka rakendada [EnableQueryAttribute] avaliku hankimine ja määrake Piipar käitumine. Kui rakendatud meetodit, järgmist määrab tagastada 1000 ridade maksimaalne:
>
>    [EnableQuery(MaxTop=1000)]

### <a name="selecting"></a>Kuidas: valige soovitud veerud

Saate määrata, lisades [Valige] klausel päringu tulemused kaasata atribuutide komplekti. Näiteks järgmine kood näitab, kuidas vaid ühe välja valimiseks ning valige ja vormindamine mitme välja:

    // Select one field -- just the Text
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => todoItem.Text);
    List<string> items = await query.ToListAsync();

    // Select multiple fields -- both Complete and Text info
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => string.Format("{0} -- {1}",
                        todoItem.Text.PadRight(30), todoItem.Complete ?
                        "Now complete!" : "Incomplete!"));
    List<string> items = await query.ToListAsync();

Kõik funktsioonid, mis on kirjeldatud siiani on täiendavad, nii, et soovime säilitada Aheldamise, neid. Iga aheldatud kõne mõjutab mitu päringut. Veel üks näide:

    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Where(todoItem => todoItem.Complete == false)
                    .Select(todoItem => todoItem.Text)
                    .Skip(3).
                    .Take(3);
    List<string> items = await query.ToListAsync();

### <a name="lookingup"></a>Kuidas: ID andmete otsimine

Funktsiooni [LookupAsync] saab kasutada objektide otsimine andmebaasist kindla ID-ga.

    // This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
    TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");

### <a name="untypedqueries"></a>Kuidas: käivitada untyped päringud

Päringu abil objekti untyped tabeli täitmisel konkreetselt Määrake OData päringustringi helistades [ReadAsync], nagu järgmises näites:

    // Lookup untyped data using OData
    JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");

Saate tagasi JSON väärtused, mida saate kasutada nagu atribuudi kotti. JToken ja Newtonsoft Json.NET kohta leiate lisateavet teemast [Json.NET] saidile.

### <a name="inserting"></a>Kuidas: mobiilirakenduse kirjutamata andmete lisamine

Kõigi KLIENDITÜÜPIDE peab sisaldama liige nimega **Id**, mis on vaikimisi string. See **Id** on nõutav CRUD-toiminguid ja ühenduseta sünkroonimine. Järgmine kood iseloomustab uue rea lisamiseks tabeli [InsertAsync] meetodi kasutamise kohta. Parameetri sisaldab andmeid, lisatakse .NET objektina.

    await todoTable.InsertAsync(todoItem);

Kui kohandatud ID kordumatu väärtus pole kaasatud selle `todoItem` ajal lisada, GUID on loodud server.
Saate tuua kontrollimise objekti pärast kõne tagastab käigus loodud ID-d.

Lisada untyped andmeid, võib ära Json.NET:

    JObject jo = new JObject();
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

Siin on näide, kasutades e-posti aadress kordumatu stringi ID:

    JObject jo = new JObject();
    jo.Add("id", "myemail@emaildomain.com");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

### <a name="working-with-id-values"></a>Töötamine ID väärtused

Mobiilirakenduste toetab kohandatud stringiväärtust kordumatu **id** Tabeliveeru. Stringi väärtus võimaldab kasutada kohandatud väärtusi, nt meiliaadressid või kasutajanimed koodi rakendused  Stringi ID esitama järgmised eelised:

* Ilma muuta ringi reisi andmebaas luuakse ID-d.
* Kirjete on lihtsam ühendada erinevate tabelite või andmebaasidest.
* Rakenduse loogika paremini integreerida ID väärtused.

Kui stringi ID väärtus on seatud on lisatud kirje, mobiilirakenduse kirjutamata loob kordumatu väärtus on ID. [Guid.NewGuid] meetodi abil saate luua oma ID väärtused klient või selle taustväärtus.

    JObject jo = new JObject();
    jo.Add("id", Guid.NewGuid().ToString("N"));

###<a name="modifying"></a>Kuidas: mobiilirakenduse kirjutamata andmete muutmine

Järgmine kood iseloomustab [UpdateAsync] meetodi abil värskendada olemasoleva kirje uut teavet sama ID-ga. Parameetri sisaldab andmeid värskendatakse .NET objektina.

    await todoTable.UpdateAsync(todoItem);

Untyped andmete värskendamine võib võtta ära [Json.NET] järgmiselt:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.UpdateAsync(jo);

Mõne `id` väli peab olema määratud värskendust tegemisel. Funktsiooni taustväärtus kasutab funktsiooni `id` välja rea värskendamiseks tuvastamiseks. Funktsiooni `id` välja aadressil tulemus on `InsertAsync` kõne. Mõne `ArgumentException` tõstetakse, kui proovite värskendada üksuse võimaldamata soovitud `id` väärtus.

###<a name="deleting"></a>Kuidas: Mobile'i rakendus taustväärtus andmete kustutamine

Järgmine kood iseloomustab [DeleteAsync] meetodi abil olemasoleva eksemplari kustutada. Näiteks tähistatakse selle `id` väli Sea selle `todoItem`.

    await todoTable.DeleteAsync(todoItem);

Untyped andmete kustutamiseks võib kuluda ära Json.NET järgmiselt:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    await table.DeleteAsync(jo);

Kui teete Kustuta taotluse, peab olema määratud ID-d. Muid atribuute edastatakse teenuse või teenuste ignoreeritakse. Tulemus on `DeleteAsync` kõne on tavaliselt `null`. Läbida ID aadressil tulemus on `InsertAsync` kõne. A `MobileServiceInvalidOperationException` Expression.Error üksuse kustutamine ilma katsel kuvatakse `id` välja.

###<a name="optimisticconcurrency"></a>Kuidas: kasutada loodetav kokkulangevus jaoks konfliktide lahendamine

Kahte või enamat klienti võib kirjutada muudatused sama kirje samal ajal. Konflikti avastamine, ilma kirjutaks viimase kirjutamine eelmise värskendusi. **Loodetav kokkulangevus juhtelemendi** eeldatakse, et iga tehingu saate endale ja seetõttu kasutada mis tahes ressursi lukustamine.  Enne tehingut, loodetav kokkulangevus juhtelemendi kinnitab pole tehingu andmeid muutnud. Kui andmed on muudetud, committing tehing on tagasi pöörata.

Mobiilirakenduste loodetav kokkulangevus juhtelement toetab muutuste jälitamise iga üksuse kasutamise soovitud `version` süsteemi atribuut veerg, mis on määratletud iga tabeli oma Mobile'i rakendus taustväärtus. Iga kord, kui kirje on värskendatud, mobiilirakenduste määrab selle `version` atribuudi kirje uueks väärtuseks. Värskenda iga taotluse ajal selle `version` atribuudi kirje kaasas kutse on võrreldes sama atribuudi kirje serveris. Kui versioon möödunud taotluse ei ühti selle taustväärtus ja seejärel tõstab kliendi teek on `MobileServicePreconditionFailedException<T>` erand. Kaasas erandi tüüp on taustväärtus, mis sisaldab kirjet serverid soovitud kirjet. Rakenduse saate seda teavet otsustada, kas värskendamine taotluse uuesti, kellel on õige `version` muutuste taustväärtus väärtus.

Veeru määratlemiseks klõpsake tabeli tunni jaoks soovitud `version` süsteemi atribuudi loodetav kokkulangevus lubamiseks. Näiteks:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }

        // *** Enable Optimistic Concurrency *** //
        [JsonProperty(PropertyName = "version")]
        public string Version { set; get; }
    }


Rakenduste untyped tabelite abil lubada loodetav kokkulangevus, seades selle `Version` märkige lipuga soovitud `SystemProperties` tabeli järgmiselt.

    //Enable optimistic concurrency by retrieving version
    todoTable.SystemProperties |= MobileServiceSystemProperties.Version;

Lisaks lubamine loodetav kokkulangevus, peate samuti püüdma selle `MobileServicePreconditionFailedException<T>` erand [UpdateAsync]helistamisel koodi.  Konflikti lahendamine, rakendades õige `version` värskendatud kirje ja kõne [UpdateAsync] lahendatud kirjetega. Järgmine kood kuvatakse üks kord tuvastatud kirjutamine konflikti lahendamise kohta:

    private async void UpdateToDoItem(TodoItem item)
    {
        MobileServicePreconditionFailedException<TodoItem> exception = null;

        try
        {
            //update at the remote table
            await todoTable.UpdateAsync(item);
        }
        catch (MobileServicePreconditionFailedException<TodoItem> writeException)
        {
            exception = writeException;
        }

        if (exception != null)
        {
            // Conflict detected, the item has changed since the last query
            // Resolve the conflict between the local and server item
            await ResolveConflict(item, exception.Item);
        }
    }


    private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
    {
        //Ask user to choose the resoltion between versions
        MessageDialog msgDialog = new MessageDialog(
            String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
            serverItem.Text, localItem.Text),
            "CONFLICT DETECTED - Select a resolution:");

        UICommand localBtn = new UICommand("Commit Local Text");
        UICommand ServerBtn = new UICommand("Leave Server Text");
        msgDialog.Commands.Add(localBtn);
        msgDialog.Commands.Add(ServerBtn);

        localBtn.Invoked = async (IUICommand command) =>
        {
            // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
            // catching a MobileServicePreConditionFailedException.
            localItem.Version = serverItem.Version;

            // Updating recursively here just in case another change happened while the user was making a decision
            UpdateToDoItem(localItem);
        };

        ServerBtn.Invoked = async (IUICommand command) =>
        {
            RefreshTodoItems();
        };

        await msgDialog.ShowAsync();
    }

Lisateavet leiate teemast [Ühenduseta andmete sünkroonimine Azure Mobile'i rakendused] .

###<a name="binding"></a>Kuidas: siduda mobiilirakenduste andmed Windowsi kasutajaliides

Selles jaotises kirjeldatakse kuvamise tagastatud andmeobjektid Kasutajaliidese elementidele Windowsi rakenduse kasutamine.  Järgmine näide kood seob allikas loendi lõpetamata üksuste päringu abil. [MobileServiceCollection] loob Mobile'i rakendused-arvestada sidumine saidikogumi.

    // This query filters out completed TodoItems.
    MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
        .Where(todoItem => todoItem.Complete == false)
        .ToCollectionAsync();

    // itemsControl is an IEnumerable that could be bound to a UI list control
    IEnumerable itemsControl  = items;

    // Bind this to a ListBox
    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Mõni juhtelement hallatavate pikkus toetavad liidest nimetatakse [ISupportIncrementalLoading]. Selle kasutajaliidese võimaldab taotleda lisaandmed kasutaja kerimise juhtelemendid. Selle liidese universal Windowsi rakendused kaudu [MobileServiceIncrementalLoadingCollection], mis tegeleb automaatselt juhtelementidelt kõnesid sisseehitatud toetatakse. Kasutage `MobileServiceIncrementalLoadingCollection` Windowsi rakendustes järgmiselt:

    MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
    items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Uue saidikogumi kasutamine Windows Phone 8 ja "Silverlight" rakendused, kasutage funktsiooni `ToCollection` laiend meetodite `IMobileServiceTableQuery<T>` ja `IMobileServiceTable<T>`. Andmete laadimine helistage `LoadMoreItemsAsync()`.

    MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
    await items.LoadMoreItemsAsync();

Kui kasutate saidikogumi, mis on loodud, helistades `ToCollectionAsync` või `ToCollection`, saate saidikogumi, mis saab seotud Kasutajaliidese juhtelemendid.  Selle saidikogumi on Piipar arvestada.  Kuna kogumine on andmete laadimine on võrgus, mõnikord laadimine nurjub. Sellise tõrkeid käsitlema alistada soovitud `OnException` meetod, `MobileServiceIncrementalLoadingCollection` erandid tulenevad kõned käsitlema `LoadMoreItemsAsync`.

Kaaluge võimalust, kui tabel sisaldab palju välju, kuid soovite kuvada mõned neist juhtelemendis. Teatud veergude kuvamiseks UI valimiseks võib kasutada eelmises jaotises ",[Valige soovitud veerud](#selecting)" on juhised.

###<a name="pagesize"></a>Lehe suuruse muutmine

Azure'i mobiilirakenduste tagastab vaikimisi kuni 50 üksust taotluse kohta.  Saate muuta lehe suurus klient ja server lehe maksimaalselt mahtu.  Määrake soovitud lehe suuruse suurendamiseks `PullOptions` kasutamisel `PullAsync()`:

    PullOptions pullOptions = new PullOptions
        {
            MaxPageSize = 100
        };

Eeldades, et olete teinud soovitud `PageSize` võrdne või suurem kui 100 sees server, taotluse tagastab kuni 100 üksused.

##<a name="#offlinesync"></a>Töötamine ühenduseta tabelid

Ühenduseta tabelid kasutada kohalike SQLite poe abil poe andmete kasutamiseks ühenduseta režiimis.  Kõik toimingud on valmis vastu kohaliku tabeli SQLite talletada asemel kaugserverisse pood.  Ühenduseta tabeli loomiseks esmalt ette projekti:

1. Paremklõpsake Visual Studio lahendus > **Haldamine Nugeti pakettide lahenduse...**, siis otsimine ja installige **Microsoft.Azure.Mobile.Client.SQLiteStore** Nugeti paketi kõik projektide lahendus.

2. (Valikuline) Installige Windows seadmed toetavad, üks järgmistest SQLite käitusaja paketid.

    * **Windows 8.1 käitusaja:** Installige [Windows 8.1 SQLite][3].
    * **Windows Phone 8.1:** Installige [Windows Phone 8.1 jaoks SQLite][4].
    * **Universaalne Windowsi platvormi** Installige [SQLite Universal Windows Universal][5].

3. (Valikuline). Windowsi seadmetele, klõpsake menüü **Viited** > **Lisada viide**, laiendage kaust **Windowsi** > **laiendid**ja seejärel sobiv **SQLite for Windows** SDK **Visual C++ 2013 käitusaja for Windows** SDK koos luba.
    SQLite SDK nimed iga Windowsi platvormi veidi erineda.

Enne tabeli viide saab luua, peab olema valmis kohalikku salve:

    var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
    store.DefineTable<TodoItem>();

    //Initializes the SyncContext using the default IMobileServiceSyncHandler.
    await this.client.SyncContext.InitializeAsync(store);

Poe lähtestamine tavaliselt saab teha kohe pärast kliendi on loodud.  **OfflineDbPath** tuleks kasutada kõigi platvormide, mida toetavad faili nimi.  Tee on täielik tee (ehk teisisõnu öeldes see algab on kaldkriips), siis selle tee kasutatakse.  Kui tee on täielikult kvalifitseeritud, paigutatakse faili platvormi kohased asukoht.

* IOS-i ja Androidi seadmete jaoks, on vaikimisi tee kaust "Isiklikke faile".
* Windowsi jaoks, on vaikimisi tee rakendusele vastav kaust "AppData".

Tabeli viide võib saada, kasutades funktsiooni `GetSyncTable<>` meetod:

    var table = client.GetSyncTable<TodoItem>();

Pole vaja kasutada ka ühenduseta tabeli autentida.  Peate ainult autentimiseks, kui suhtlete teenuse taustväärtus.

###<a name="syncoffline"></a>Sünkroonimine on ühenduseta tabel

Ühenduseta tabelid ei sünkroonita selle kirjutamata vaikimisi.  Sünkroonimine on tükeldatud kahe andmeühikuga.  Muudatusi saate push eraldi allalaadimise uued üksused.  Siin on tüüpilised sünkroonimine meetod.

    public async Task SyncAsync()
    {
        ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

        try
        {
            await this.client.SyncContext.PushAsync();

            await this.todoTable.PullAsync(
                //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
                //Use a different query name for each unique query in your program
                "allTodoItems",
                this.todoTable.CreateQuery());
        }
        catch (MobileServicePushFailedException exc)
        {
            if (exc.PushResult != null)
            {
                syncErrors = exc.PushResult.Errors;
            }
        }

        // Simple error/conflict handling. A real application would handle the various errors like network conditions,
        // server conflicts and others via the IMobileServiceSyncHandler.
        if (syncErrors != null)
        {
            foreach (var error in syncErrors)
            {
                if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                {
                    //Update failed, reverting to server's copy.
                    await error.CancelAndUpdateItemAsync(error.Result);
                }
                else
                {
                    // Discard local change.
                    await error.CancelAndDiscardItemAsync();
                }

                Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
            }
        }
    }

Kui esimene argument `PullAsync` on tühi, siis ei kasutata astmeline sünkroonimine.  Iga toimingu sünkroonimine laadib kõik kirjed.

SDK sooritab peidetud `PushAsync()` enne tõmbamine kirjed.

Konflikti töötlemine juhtub on `PullAsync()` meetod.  Saate tegeleda konfliktide samal viisil võrgus tabelitena.  Konflikti toodetud kui `PullAsync()` nimetatakse asemel lisamine, värskendamine või Kustuta. Kui mitme konfliktide, nad on ühendatud ühe MobileServicePushFailedException sisse.  Iga tõrge toime eraldi.

##<a name="#customapi"></a>Kohandatud API töötamine

Kohandatud API võimaldab määratleda kohandatud lõpp-punktid mis edastavad serveri funktsioonid, mis pole vastendamine lisada, värskendada, kustutada või lugeda toiming. Kohandatud API abil saate määrata rohkem kontrolli sõnumside, sh lugemine ja HTTP sõnumipäiste seadmine ja sõnumi keha vorming peale JSON määratlemine.

Kohandatud API helistate, helistades [InvokeApiAsync] meetodite klientarvutis. Näiteks järgmine rida koodi saadab POST taotlus on **completeAll** API sisse selle kirjutamata:

    var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);

Sellel vormil on tipitud meetod kõne ja nõuab, et **MarkAllResult** annaks tüüp on määratletud. Tipitud-ja untyped on toetatud.

##<a name="authentication"></a>Kasutajate autentimiseks

Mobiilirakenduste toetab autentimist ja lubatakse rakenduse kasutajad abil erinevad väliste identiteedipakkujad: Facebook, Google, Microsoft Account, Twitteri ja Azure Active Directory. Õigusi saab seada tabelites piirata juurdepääsu teatud toimingute ainult autenditud kasutajad. Autenditud kasutaja ID abil rakendada serveri skriptide autoriseerimine reeglid. Lisateabe saamiseks vt õpetuse [rakenduse lisamine autentimine].

Kahe autentimise puhul on toetatud: _Kliendi haldusega_ ja _serveri haldusega_ . Serveri haldusega voo pakub lihtsaim autentimise kogemuse, kuna see põhineb pakkuja web autentimine liides. Kliendi hallatavate voogu võimaldab põhjalikult integreerimine seadme võimaluste see sõltub teenusepakkuja kohased seadme SDK-d.

>[AZURE.NOTE] Soovitame kasutada kliendi hallatavate meilivoo tootmise rakendused.

Häälestada autentimine, peavad registreerima ühe või mitme identiteedipakkujad rakenduse.  Identiteedipakkuja loob kliendi ID ja kliendi salajane oma rakenduse.  Need väärtused siis seatakse teie taustväärtus lubamiseks Azure'i rakendust Service autentimine/autoriseerimine.  Lisateabe saamiseks järgige õpetuses [rakenduse lisamine autentimise]üksikasjalikke juhiseid.

Selles jaotises käsitletakse järgmisi teemasid:

+ [Kliendi hallatavate autentimine](#clientflow)
+ [Serveri haldusega autentimine](#serverflow)
+ [Vahemällu autentimise luba](#caching)

###<a name="clientflow"></a>Kliendi hallatavate autentimine

Rakenduse saate sõltumatult pakkujalt identiteedi ja seejärel andke tagastatud luba sisselogimise ajal koos oma taustväärtus. See kliendi vool võimaldab ühekordse sisselogimise teenuse kasutaja või kasutajate täiendavate andmete toomiseks identiteedipakkuja juures. Kliendi meilivoo autentimine on eelistatud kasutamise serveri meilivoo identiteedipakkuja SDK pakub rohkem kohalikke Kasutuskogemuse olemus ja võimaldab täiendav kohandamine.

Näited on esitatud jaoks järgmised kliendi-flow autentimise mustrid:

+ [Active Directory Authentication Library](#adal)
+ [Facebooki või Google](#client-facebook)
+ [Reaalajas SDK](#client-livesdk)

#### <a name="adal"></a>Autentida Active Directory Authentication Library kasutajad

Kliendi Azure Active Directory autentimist kasutades saate kasutada algatada kasutaja autentimisega Active Directory autentimise Raamatukogu (ADAL).

1. [Kuidas konfigureerida rakenduse teenus Active Directory login] õpetuse järgides konfigureerimine oma mobiilirakenduse kirjutamata AAD sisselogimise. Veenduge, et kohustuslik toiming omakliendi rakenduse registreerimisel.
2. Visual Studio või Xamarin Studio, avage oma projekti ja lisada selle `Microsoft.IdentityModel.CLients.ActiveDirectory` Nugeti pakett. Kui otsida, lisage väljalaske-eelse versiooni.
3. Lisage järgmine kood rakenduse vastavalt kasutate platvormi. Iga, Veenduge järgmises asendamine.

    * Asendage **Lisa-asutus – siin** rentniku, kus te ette valmistatud rakenduse nimi. Vormingu peaks olema https://login.windows.net/contoso.onmicrosoft.com. See väärtus saab kopeerida Azure Active Directory [Azure klassikaline portaalis]domeeni menüüs.
    * Asendage **Lisa-RESSURSI ID siin** kliendi ID jaoks oma mobiilirakenduse taustväärtus. Kliendi ID saate hankida mõnest portaalis jaotises **Azure Active Directory sätted** vahekaarti **Täpsemalt** .
    * Asendage **Lisa-kliendi ID siin** omakliendi rakendusest kopeeritud kliendi ID-ga.
    * Asendage **Lisa-REDIRECT URI siin** oma saidi _/.auth/login/done_ lõpp-punkti, kasutades HTTPS värviskeemi. See väärtus peab olema _https://contoso.azurewebsites.net/.auth/login/done_.

    Koodi vaja iga platvormi järgmiselt:

    **Windows:**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            while (user == null)
            {
                string message;
                try
                {
                    AuthenticationContext ac = new AuthenticationContext(authority);
                    AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                        new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                    JObject payload = new JObject();
                    payload["access_token"] = ar.AccessToken;
                    user = await App.MobileService.LoginAsync(
                        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

    **Xamarin.iOS**

        private MobileServiceUser user;
        private async Task AuthenticateAsync(UIViewController view)
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(view));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    **Xamarin.Android**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(this));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                AlertDialog.Builder builder = new AlertDialog.Builder(this);
                builder.SetMessage(ex.Message);
                builder.SetTitle("You must log in. Login Required");
                builder.Create().Show();
            }
        }
        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
        }

####<a name="client-facebook"></a>Ühekordse sisselogimise märgiks Facebookist või Google abil

Saate kliendi voogu, nagu on näidatud selles koodilõigu Facebooki või Google.

    var token = new JObject();
    // Replace access_token_value with actual value of your access token obtained
    // using the Facebook or Google SDK.
    token.Add("access_token", "access_token_value");

    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {
        while (user == null)
        {
            string message;
            try
            {
                // Change MobileServiceAuthenticationProvider.Facebook
                // to MobileServiceAuthenticationProvider.Google if using Google auth.
                user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

####<a name="client-livesdk"></a>Ühe Sisselogimine Microsofti Account SDK Live kaudu

Kasutajate autentimiseks peate oma Microsofti konto Arenduskeskus rakenduse registreerima. Saate konfigureerida üksikasjade oma Mobile'i rakendus taustväärtus. Looge Microsofti konto registreerimine ja ühendage see oma mobiilirakenduse kirjutamata, täitke juhised registris [rakenduse kasutamiseks Microsofti konto kasutajanime]. Kui teil on oma rakenduse Windowsi poe nii Windows Phone 8/Silverlighti versioon, registreerige esmalt Windowsi poe versioon.

Järgmine kood autendib Live SDK abil ja kasutab tagastatud luba oma mobiilirakenduse taustväärtus sisse logima.

    private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
    private async System.Threading.Tasks.Task AuthenticateAsync()
    {

        // Get the URL the Mobile App backend.
        var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

        // Create the authentication client for Windows Store using the service URL.
        LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
        //// Create the authentication client for Windows Phone using the client ID of the registration.
        //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

        while (session == null)
        {
            // Request the authentication token from the Live authentication service.
            // The wl.basic scope should always be requested.  Other scopes can be added
            LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
            if (result.Status == LiveConnectSessionStatus.Connected)
            {
                session = result.Session;

                // Get information about the logged-in user.
                LiveConnectClient client = new LiveConnectClient(session);
                LiveOperationResult meResult = await client.GetAsync("me");

                // Use the Microsoft account auth token to sign in to App Service.
                MobileServiceUser loginResult = await App.MobileService
                    .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

                // Display a personalized sign-in greeting.
                string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
                var message = string.Format("You are now logged in - {0}", loginResult.UserId);
                var dialog = new MessageDialog(message, title);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
            else
            {
                session = null;
                var dialog = new MessageDialog("You must log in.", "Login Required");
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }
    }

Lisateabe saamiseks vaadake [Windows Live'i SDK] dokumentatsioonist.

###<a name="serverflow"></a>Serveri haldusega autentimine

Kui olete oma identiteedipakkuja registreeritud, helistage [MobileServiceAuthenticationProvider] väärtusega teenuse pakkuja [LoginAsync] meetodi [MobileServiceClient]. Näiteks järgmine kood alustab serveri meilivoo sisselogimine Facebooki abil.

    private MobileServiceUser user;
    private async System.Threading.Tasks.Task Authenticate()
    {
        while (user == null)
        {
            string message;
            try
            {
                user = await client
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

Kui kasutate identiteedi pakkuja peale Facebooki, muuta [MobileServiceAuthenticationProvider] väärtus väärtus pakkuja.

Serveri meilivoo, haldab Azure'i rakendust Service, kuvades valitud pakkuja lehe OAuthi autentimine voogu.  Kui identiteedi pakkuja annab Azure'i rakenduse teenus loob mõne rakenduse teenuse autentimise luba. [LoginAsync] meetod annab vastuseks [MobileServiceUser], mis sisaldab nii [kasutajanimi] autenditud kasutaja [MobileServiceAuthenticationToken]märgiks JSON web (JWT). Selle märgiks saab vahemälus talletatud ja taaskasutada enne selle lõppemist. Lisateavet leiate teemast [vahemällu autentimise luba](#caching).

###<a name="caching"></a>Vahemällu autentimise luba

Mõnel juhul saate kõne sisselogimise meetodit vältida pärast esimese eduka autentimise talletades autentimise luba pakkuja.  Windowsi poest ja UWP rakendusi saate [PasswordVault] vahemälu praeguse autentimise luba pärast eduka sisselogimine, järgmiselt:

    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

    PasswordVault vault = new PasswordVault();
    vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
        client.currentUser.MobileServiceAuthenticationToken));

Kasutajanimi väärtus on salvestatud mandaat kasutajanimi ja luba on salvestatud parool. Järgmisel käivitamisel, saate **PasswordVault** vahemällu talletatud identimisteabe. Järgmises näites kasutatakse vahemälus, kui nad on leitud ja muidu avaldab kinnitamiseks uuesti soovitud taustväärtus:

    // Try to retrieve stored credentials.
    var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
    if (creds != null)
    {
        // Create the current user from the stored credentials.
        client.currentUser = new MobileServiceUser(creds.UserName);
        client.currentUser.MobileServiceAuthenticationToken =
            vault.Retrieve("Facebook", creds.UserName).Password;
    }
    else
    {
        // Regular login flow and cache the token as shown above.
    }

Kasutaja väljalogimisel Samuti peate kustutada talletatud mandaatide, järgmiselt:

    client.Logout();
    vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));

Xamarin rakenduste abil [Xamarin.Auth] API-de turvaliselt säilitada objekti **konto** identimisteave. Näiteks nende API-de abil, vt [AuthStore.cs] koodi faili [ühiskasutuse valimi ContosoMoments foto].

Kui kasutate kliendi hallatavate autentimine, saate saab ka vahemälu juurdepääsu luba saadud pakkuja Facebooki või Twitteri. Selle märgiks saab esitada taotleda uut autentimise luba kirjutamata, järgmiselt:

    var token = new JObject();
    // Replace <your_access_token_value> with actual value of your access token
    token.Add("access_token", "<your_access_token_value>");

    // Authenticate using the access token.
    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);

##<a name="pushnotifications"></a>Tõuketeatised

Järgmistes teemades kataks tõuketeatised:

* [Tõuketeatiste register](#register-for-push)
* [Windowsi poe paketi SID](#package-sid)
* [Mitu platvormi Mallid registreerima](#register-xplat)

###<a name="register-for-push"></a>Kuidas: Tõuketeatiste Register

Mobiilirakenduste kliendi võimaldab Registreeruge Tõuketeatiste Azure'i teatis jaoturi abil. Kui registreerimine, saate hankida pidet, mille saate hankida kaudu platvormi kohased Push teatise teenuse (PNS). Seejärel sisestage selle väärtuse koos sildid registreerimise loomisel. Järgmine kood registreerib teie Windowsi rakenduse jaoks Tõuketeatiste koos Windowsi teatise teenuse (WNS):

    private async void InitNotificationsAsync()
    {
        // Request a push notification channel.
        var channel =await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        // Register for notifications using the new channel.
        await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
    }

Kui teil on lükkamine WNS abil, siis peate [Windowsi poe paketi SID](#package-sid).  Windowsi rakendused, sh Kuidas registreeruda malli registreerimine, leiate [Lisa Tõuketeatiste app].

Siltide paluda kliendilt ei toetata.  Sildi taotlused kõrvaldatakse vaikselt registreerimise kaudu.
Kui soovite oma seadme registreerimiseks siltidega, luua kohandatud API, mida kasutab teatis jaoturi API registreerimise teie nimel.  [Kohandatud API kõne](#customapi) asemel on `RegisterNativeAsync()` meetod.

###<a name="package-sid"></a>Kuidas: saada Windowsi poe pakett SID

Paketi SID on vaja lubada Tõuketeatiste Windowsi poe rakenduses.  Paketi SID vastuvõtmiseks registreerida rakenduse Windowsi poest.

Saada see väärtus:

1. Visual Studio lahenduste Explorer, paremklõpsake Windowsi poe rakenduse project, klõpsake **poe** > **... Poe rakendus seostada**.
2. Viisardis **järgmise**, logige sisse oma Microsofti kontoga sisse, sisestage oma rakenduse nimi reservi **uue rakenduse nime**, klõpsake käsku **reservi**.
3. Pärast rakenduse registreerimine on loodud, valige rakenduse nime, klõpsake nuppu **Järgmine**ja klõpsake **seostada**.
4. Logige sisse oma Microsofti Account [Windows Arenduskeskus] . Klõpsake jaotises **minu rakendused**rakendus registreerimise loodud.
5. Klõpsake **rakenduse halduse** > **identiteedi**ja seejärel liikuge allapoole, et otsida teie **Paketi SID**.

Palju kasutab paketi SID kohelge seda kui URI, sel juhul peate kasutama _ms-rakendus: / /_ kava nimega. Märkige üles oma paketi SID, ühendades selle väärtuse eesliide loodud versiooni.

Xamarin rakenduste jaoks on vaja registreerida rakendus iOS või Android platvormid töötavad mõned täiendavad koodi. Lisateavet leiate teemast teie platvorm:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push)

###<a name="register-xplat"></a>Kuidas: Register tõuketeatised Mallid platvormidel teatiste saatmine

Mallide registreerimiseks kasutage funktsiooni `RegisterAsync()` meetod mallidega järgmiselt:

        JObject templates = myTemplates();
        MobileService.GetPush().RegisterAsync(channel.Uri, templates);

Mallide peaks olema `JObject` tüübid ja võib olla mitu Mallid JSON järgmises vormingus:

        public JObject myTemplates()
        {
            // single template for Windows Notification Service toast
            var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

            var templates = new JObject
            {
                ["generic-message"] = new JObject
                {
                    ["body"] = template,
                    ["headers"] = new JObject
                    {
                        ["X-WNS-Type"] = "wns/toast"
                    },
                    ["tags"] = new JArray()
                },
                ["more-templates"] = new JObject {...}
            };
            return templates;
        }

Meetod **RegisterAsync()** aktsepteerib ka teisene paanid:

        MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);

Turvalisuse registreerimise käigus eraldatakse kõik sildid kohe. Siltide lisamiseks installide või mallidele installide teemast [töötamine .NET taustväärtus serveriga SDK Azure'i mobiilirakenduste].

Saada teatisi, kasutades neid registreeritud malle, vaadake [Teatis jaoturi API -d].

##<a name="misc"></a>Mitmesugused Teemad

###<a name="errors"></a>Kuidas: vigade

Tõrke ilmnemisel on kirjutamata kliendi SDK tõstab on `MobileServiceInvalidOperationException`.  Järgmises näites on kujutatud, kuidas hallata erandi, mis tagastatakse funktsiooni kirjutamata:

    private async void InsertTodoItem(TodoItem todoItem)
    {
        // This code inserts a new TodoItem into the database. When the operation completes
        // and App Service has assigned an Id, the item is added to the CollectionView
        try
        {
            await todoTable.InsertAsync(todoItem);
            items.Add(todoItem);
        }
        catch (MobileServiceInvalidOperationException e)
        {
            // Handle error
        }
    }

Teine näide tegelemiseks tõrke tingimustega leiate [Mobile'i rakendused failide valimi]. Näide [LoggingHandler] pakub logimine volitatud esindaja sündmuseohjuri (järgmine) log taotlusi, tehtud on taustväärtus.

###<a name="headers"></a>Kuidas: kohandada päiste taotlemine

Milline olukord teie konkreetse rakenduse toetamiseks võib juhtuda kohandamine Mobile'i rakendus taustväärtus suhtlemine. Näiteks võite iga väljamineva taotluse kohandatud päise lisamine või isegi muutmiseks vastuste olek koode. Saate kasutada ka kohandatud [DelegatingHandler], nagu järgmises näites:

    public async Task CallClientWithHandler()
    {
        HttpResponseMessage[]
        MobileServiceClient client = new MobileServiceClient("AppUrl",
            new MyHandler()
            );
        IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
        var newItem = new TodoItem { Text = "Hello world", Complete = false };
        await todoTable.InsertAsync(newItem);
    }

    public class MyHandler : DelegatingHandler
    {
        protected override async Task<HttpResponseMessage>
            SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Change the request-side here based on the HttpRequestMessage
            request.Headers.Add("x-my-header", "my value");

            // Do the request
            var response = await base.SendAsync(request, cancellationToken);

            // Change the response-side here based on the HttpResponseMessage

            // Return the modified response
            return response;
        }
    }


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Rakenduse lisamiseks autentimine]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Ühenduseta sünkroonimise Azure mobiilirakendused]: app-service-mobile-offline-data-sync.md
[Tõuketeatiste lisamiseks rakenduse]: app-service-mobile-windows-store-dotnet-get-started-push.md
[Rakenduse kasutamiseks Microsofti kontole sisselogimine registreerimine]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Kuidas konfigureerida rakenduse teenus Active Directory login]: app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[loob viite untyped tabelisse]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[Tegemine]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[Valige]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[Jäta]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[Kasutajanimi]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[Kui]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Azure'i portaal]: https://portal.azure.com/
[Azure'i klassikaline portaal]: https://manage.windowsazure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Windowsi Arenduskeskus]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live'i SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Teatis jaoturi API-d]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Mobiilirakenduste failide näidis]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData v3 dokumentatsioon]: http://www.odata.org/documentation/odata-version-3-0/
[Viiuldaja]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: (https://github.com/azure-appservice-samples/ContosoMoments/blob/dev/src/Mobile/ContosoMoments/Helpers/AuthStore.cs)
[ContosoMoments foto ühiskasutuse näidis]: https://github.com/azure-appservice-samples/ContosoMoments