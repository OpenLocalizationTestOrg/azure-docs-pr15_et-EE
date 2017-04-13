<properties 
    pageTitle="Mis on Azure WebJobs SDK" 
    description="Azure'i WebJobs SDK tutvustus. Selgitatakse, mis SDK on tüüpilised stsenaariumid, see on kasulik, ja proovi kood." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="what-is-the-azure-webjobs-sdk"></a>Mis on Azure WebJobs SDK

## <a id="overview"></a>Ülevaade

Selles artiklis selgitatakse, mis on WebJobs SDK, vaatab läbi mõne levinud stsenaariumi, see on kasulik ja antakse ülevaade sellest, kuidas seda kasutada koodi.

[WebJobs](websites-webjobs-resources.md) funktsioon Azure'i rakenduse teenus, mis võimaldab käivitada programmi või skripti, veebirakenduse, API rakenduse või mobiilirakenduses samas kontekstis. [WebJobs SDK](websites-webjobs-resources.md) eesmärk lihtsustada koodi kirjutamise levinud toiminguid, mida on WebJob saate teha, näiteks pilditöötluse, järjekorda töötlemine RSS-i koondamine, faili, ja hooldus e-kirjade saatmine. WebJobs SDK on valmisfunktsioone Azure Storage ja teenuse siini töötamiseks, ajastamist ja tõrgete ja jaoks mitme levinud stsenaariumi. Lisaks on eesmärk olema laiendatav. [WebJobs SDK on avatud](https://github.com/Azure/azure-webjobs-sdk/), kuid on ka [avage allikas hoidla laiendid](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

WebJobs SDK sisaldab järgmised komponendid.

* **NuGet-paketid**. Saate lisada Visual Studio konsooli rakendus projekti NuGet-paketid kasutatavat oma koodi kaunistamine oma meetodite WebJobs SDK atribuutidega raamistik.
  
* **Armatuurlaua**. Osa WebJobs SDK on kaasatud Azure'i rakendust Service ja antakse rikkaliku jälgimine ja diagnostika NuGet-paketid kasutavad programmid. Teil pole nende diagnostika- ja funktsioonide kasutamiseks koodi kirjutamist.

## <a id="scenarios"></a>Stsenaariumid

Siin on mõned tüüpilised stsenaariumid, saate kergemini toime Azure'i WebJobs SDK.

* Piltide töötlemine ja muud Protsessori jaoks mahukat tööd. Veebisaitide ühine joon on võimalus üles laadida pilte või videoid. Sageli soovite töödelda sisu, kui see on üles laaditud, kuid te ei soovi muuta kasutaja, oodake, kuni te seda teha.

* Töötlemine. Levinud viis web frontend suhelda kirjutamata teenus on kasutada järjekorrad. Kui veebisaidil on vaja teha tööd, see sunnib sõnumi peale järjekorda. Taustväärtus teenuse tõmbab sõnumeid kuhjuda ja ei tööta. Võite kasutada järjekorrad pilditöötluse: näiteks pärast kasutaja lisatud failide arvu, panna nimesid järjekorda sõnumi töötlemiseks taustväärtus kätte. Või võite kasutada järjekorrad saidi tundlikkuse parandamiseks. Näiteks mitte kirjalikult otse SQL-andmebaasiga, kirjutada järjekorda, paluge kasutajat, olete valmis, ja andke teenuse pidet latentsusajaga relatsiooniline tagaandmebaas töötada. Näiteks järjekorda töötlemise protsessi pilt, vt [õpetuse WebJobs SDK alustamine](websites-dotnet-webjobs-sdk-get-started.md).

* RSS-i koondamine. Kui teil on sait, mis säilitab RSS-kanalite loendi, mida võiks tõmmata kõikide kanalite tausta protsessi artiklid.

* Faili hoolduse, nagu liitmise või puhastamine logifailid.  Võib olla mitu saitide või jaoks eraldi aeg kestab, mida soovite ühendada, et analüüsi töö käitamist on loodud logifailide. Või võiksite ajastamise juhtida nädala puhastamiseks vana logifailid.

* Sissepääsu Azure tabelitesse. Võimalik on talletatud failide ja plekid ning soovite sõeluda neid ja salvestab andmed tabeli. Funktsiooni sissepääsu võib kirjutamine paljude ridadega (miljoneid mõnel juhul) ja WebJobs SDK võimaldab rakendada see funktsioon on lihtne. SDK pakub reaalajas jälgimine edenemise näitajate nt kirjutada tabeli ridade arv.

* Muud pikaajalisi ülesanded, mida soovite käivitada tausta teemas, näiteks [e-kirjade saatmine](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure). 

* Kõik toimingud, mida soovite käivitada ajakava, näiteks varundamise toimingut iga öösel.

Paljud need stsenaariumid soovite mastaapimiseks web appi käitamist mitmes VMs, mis kestab korraga mitme WebJobs. Mõnel juhul see võib põhjustada andmete töödelda mitu korda, kuid see ei ole probleem, kui kasutate sisseehitatud järjekorda, bloobimälu ja teenuse siini päästikute WebJobs SDK. SDK tagab, et teie funktsioonide töödeldakse iga sõnumi või bloobimälu ainult üks kord.

WebJobs SDK ka lihtsustab levinud tõrketöötluse stsenaariumid käsitlema. Saate häälestada teatisi saada teatisi funktsiooni nurjub, kui ajalõpud saate määrata, et funktsiooni automaatselt tühistatakse, kui see pole määratud aja jooksul lõpule viia.

## <a id="code"></a>Koodinäiteid

Tüüpilised tööülesandeid töötamine Azure Storage käsitlemise kood on lihtne. Konsooli rakenduses `Main` loomist meetod on `JobHost` objekt, mille kõned meetodite kirjutamise koordinaadid. WebJobs SDK raames teab, kui teie meetodite helistada ja mis parameetrite väärtused kasutada põhjal WebJobs SDK saate kasutada neid atribuute. SDK pakub *päästikute* , et määrata tingimused põhjustada nimetatakse funktsiooni ja saate määrata, kuidas saada teavet ja sealt meetod parameetrite *sideained* .

Atribuut [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) põhjustab näiteks kui järjekorda sõnum ning kui sõnumi vorming on JSON baitide massiivis või kohandatud tüübi, sõnum on automaatselt deserialized nimetatakse funktsiooni. Atribuut [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) käivitab protsessi iga kord, kui luuakse uus bloobimälu Azure Storage konto.

Siin on lihtne programm, mis küsitlused järjekorda ja loob bloobimälu, iga järjekorda vastuvõetud sõnumid:

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

Funktsiooni `JobHost` objekt on ümbris tausta funktsioonide kogum. Funktsiooni `JobHost` objekti jälgib sündmused, mis käivitab neid kellad, funktsioonid ja käivitab funktsioonide, kui päästik sündmused. Saate kõne on `JobHost` Määrake, kas container protsessi käivitamiseks praeguse teema või tausta sõnumilõime meetod. Näites kuvatakse `RunAndBlock` meetod töötab protsessi pidevalt praeguse lõime.

Kuna selle `ProcessQueueMessage` meetod selles näites on mõne `QueueTrigger` atribuut, käivitab see funktsioon on vastuvõtu järjekorda uue sõnumi. Funktsiooni `JobHost` objekti jälgimine uute sõnumite järjekord määratud järjekorda ("webjobsqueue" selles näites) ja kui tuvastatakse, on see helistab `ProcessQueueMessage`. 

Selle `QueueTrigger` atribuut seob selle `inputText` järjekorda sõnumi väärtuse-parameeter. Ja `Blob` seob atribuut on `TextWriter` objekti lisamine bloobimälu nimega "blobname" nimega "ümbrisenimi" nõus.  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

Funktsioon kasutab neid parameetreid siis kirjutamise järjekorda sõnumi väärtus on bloobimälu:

        writer.WriteLine(inputText);

WebJobs SDK päästik ja sideaine funktsioonid oluliselt lihtsustada koodi tuleb kirjutada. Nõutav töötlemiseks järjekorrad, plekid või failide või algatada toimingud, madala koodi tehakse teile WebJobs SDK raames. Näiteks raames loob järjekorrad, mida pole veel, avatakse järjekorda, loeb järjekorda sõnumeid, kustutab järjekorda sõnumeid, kui töötlemine on lõpule viidud, loob bloobimälu ümbriste, mida pole veel kirjutab plekid jne.

Koodi järgmises näites kuvab erinevate päästikute ühe WebJob: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, ja `ErrorTrigger`. 

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

        public static void ErrorMonitor(
        [ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
        [SendGrid(
            To = "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors to the Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a id="schedule"></a>Plaanimine

Funktsiooni `TimerTrigger` atribuut võimaldab päästik funktsioonide käitamiseks ajakava. Kogu Azure'i või ajakava üksikute funktsioonid on WebJob WebJobs SDK abil saate ajastada on WebJob `TimerTrigger`. Siin on näide kood.

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

Veel proovi kood, leiate Azure'i webjobs-sdk laiendid hoidla [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) GitHub.com.

## <a name="extensibility"></a>Laiendatavus

Te ei pea sisseehitatud funktsioonid--WebJobs SDK võimaldab kirjutage kohandatud päästikute ja sideaineid.  Näiteks saate kirjutada päästikute vahemälu sündmused ja perioodiliste ajakava. [Avage allikas hoidla](https://github.com/Azure/azure-webjobs-sdk-extensions) on sisaldab [üksikasjaliku juhendi WebJobs SDK laiendatavuse](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) ja proovi kood aitavad teil alustada oma päästikute ja sideained.

## <a id="workerrole"></a>Väljaspool WebJobs WebJobs SDK abil

Programmi, mis kasutab funktsiooni WebJobs SDK on standardne konsooli rakendus ja käitamise pool – see ei sisalda mõnda WebJob käitada. Arvuti arengu testida programmi kohapeal ja valmistamisel käivitada selle pilveteenuses töötaja roll või teenus Windows nendes keskkondades soovi. 

Juhtpaneel on ainult saadaval Azure'i rakendust Service veebirakenduse laiendamine. Kui soovite mõne WebJob väljaspool käivitamiseks ja siiski kasutada armatuurlaud, saate konfigureerida veebirakenduse kasutada sama salvestusruumi konto oma WebJobs SDK armatuurlaua ühendusstring viitavat ning et web appi WebJobs armatuurlaua seejärel kuvatakse andmed oma programm, kus töötab kuhugi mujale funktsioon täitmise kohta. Saate armatuurlaua URL-i https://*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions abil. Lisateabe saamiseks vaadake [armatuurlaua arengu WebJobs SDK saamine](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), kuid Pange tähele, et ajaveebipostituse mõnda vana ühenduse stringi nimi. 

## <a id="nostorage"></a>Armatuurlaua funktsioonid

WebJobs SDK pakub mitmeid eeliseid, isegi siis, kui te ei kasuta WebJobs SDK päästikute või sideained.

* Saate kasutada funktsioone armatuurlaualt.
* Saate kordus funktsioonide armatuurlaualt.
* Saate vaadata logid armatuurlaual seotud kindla WebJob (logid, kirjutada, kasutades Console.Out, Console.Error, Jälita jne) või lingitud konkreetne funktsioon kutsumise, mida need (logid kirjutada, kasutades mõnda `TextWriter` objekt, mille SDK edastab funktsioonile parameetrina). 

Lisateabe saamiseks lugege teemat [kuidas rakendada käsitsi funktsiooni](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) ja [Kuidas kirjutada logid](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>Järgmised sammud

WebJobs SDK kohta leiate lisateavet teemast [Azure WebJobs Soovitatavad ressursid](http://go.microsoft.com/fwlink/?linkid=390226).

Viimane täiustused, mida soovite WebJobs SDK kohta leiate teavet teemast [Väljalaskemärkmed](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).
 
