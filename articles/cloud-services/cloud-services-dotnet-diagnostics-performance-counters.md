<properties
   pageTitle="Jõudluse hinnale kasutamine Azure diagnostika | Microsoft Azure'i"
   description="Jõudluse hinnale Azure pilveteenustega või virtuaalse masina abil saate otsida kitsaskohtade ja jõudluse häälestamine."
   services="cloud-services"
   documentationCenter=".net"
   authors="rboucher"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/29/2016"
   ms.author="robb" />

# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Loomine ja kasutamine jõudluse hinnale Azure rakenduses

Selles artiklis kirjeldatakse eelised ja kuidas panna jõudluse hinnale Azure rakenduse. Neid saab kasutada andmete kogumise, kitsaskohtade leidmine ja süsteemi ja rakenduste jõudluse häälestamine.

Jõudluse hinnale saadaval Windows Server, IIS-i ja ASP.net-i, saate ka kogutud ja Azure web rollid, töötaja rollid ja Virtuaalmasinates seisundi määramiseks kasutada. Saate luua ja kasutada kohandatud jõudluse hinnale.  

Saate uurida jõudluse andmete
1. Otse rakenduse hostis jõudluse kuvari tööriistaga kaugtöölaua abil
2. Kasutades System Center Operations Manager Azure'i halduspaketi
3. Muud jälgimisega seotud tööriistu juurdepääs diagnostikateavet üle Azure salvestusruumi. Lisateabe saamiseks vaadake [talletamine ja Azure Storage diagnostika andmete kuvamine](https://msdn.microsoft.com/library/azure/hh411534.aspx) .  

[Azure'i klassikaline portaali](http://manage.azure.com/)rakenduse täitmise kontrollimise kohta leiate lisateavet teemast [kuvari pilveteenustega](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

Täiendavad põhjalikumat on logimine ja strateegia jälitamine ja diagnostika- ja teiste meetodite abil abil tõrkeotsing ja optimeerida Azure rakenduste loomise juhised leiate teemast [Tõrkeotsing head tavad Azure'i rakenduste arendamise](https://msdn.microsoft.com/library/azure/hh771389.aspx).


## <a name="enable-performance-counter-monitoring"></a>Luba jõudluse counter jälgimine

Jõudluse hinnale on vaikimisi lubatud. Rakenduse või käivitus tööülesande muutma vaikimisi diagnostika agendi konfiguratsiooni kaasamiseks soovitud konkreetsete tulemuslikkuse väidab, et soovite jälgida iga rolli eksemplari.

### <a name="performance-counters-available-for-microsoft-azure"></a>Jõudluse hinnale saadaval Microsoft Azure

Azure'i pakub Windows Server, IIS-i ja ASP.net-i virnas alamhulga jõudluse hinnale saadaval. Järgmises tabelis on loetletud mõned jõudluse hinnale erilist huvi Azure rakendused.

|Counter kategooria: Objekti (eksemplari)|Counter nimi      |Viide|
|---|---|---|
|.NET CLR-i erandid (_globaalne_)|# Exceps visati sekundis   |Erandi jõudluse hinnale|
|.NET CLR mälu (_globaalne_)    |% Kellaaja c            |Mälu jõudluse hinnale|
|ASP.NET-I                      |Rakenduse taaskäivitamist    |Jõudluse hinnale ASP.net-i jaoks|
|ASP.NET-I                      |Päringu täitmise  |Jõudluse hinnale ASP.net-i jaoks|
|ASP.NET-I                      |Katkestatud taotlused   |Jõudluse hinnale ASP.net-i jaoks|
|ASP.NET-I                      |Töötaja protsessi taaskäivitamist |Jõudluse hinnale ASP.net-i jaoks|
|ASP.net-i rakendused (__kogusumma__)|Taotlusi kokku        |Jõudluse hinnale ASP.net-i jaoks|
|ASP.net-i rakendused (__kogusumma__)|Taotlusi sekundis          |Jõudluse hinnale ASP.net-i jaoks|
|ASP.net-i v4.0.30319           |Päringu täitmise  |Jõudluse hinnale ASP.net-i jaoks|
|ASP.net-i v4.0.30319           |Päringu ootamine       |Jõudluse hinnale ASP.net-i jaoks|
|ASP.net-i v4.0.30319           |Taotlused jooksva        |Jõudluse hinnale ASP.net-i jaoks|
|ASP.net-i v4.0.30319           |Taotluse järjekorda.         |Jõudluse hinnale ASP.net-i jaoks|
|ASP.net-i v4.0.30319           |Tagasi lükatud taotlusi       |Jõudluse hinnale ASP.net-i jaoks|
|Mälu                       |Saadaval megabaiti        |Mälu jõudluse hinnale|
|Mälu                       |Kinnitatud baiti         |Mälu jõudluse hinnale|
|Processor(_Total)            |% Protsessori aeg        |Jõudluse hinnale ASP.net-i jaoks|
|TCPv4                        |Ühenduse tõrked     |TCP objekt|
|TCPv4                        |Ühendused, mis on loodud |TCP objekt|
|TCPv4                        |Ühenduste lähtestamine       |TCP objekt|
|TCPv4                        |Segmente saatmist/sec       |TCP objekt|
|Võrgu Interface(*)         |Baiti saanud sekundis      |Võrgu kasutajaliidese objekt|
|Võrgu Interface(*)         |Baiti saadetud sekundis          |Võrgu kasutajaliidese objekt|
|Võrgu kasutajaliidese (Microsoft Virtual masina siini võrgu adapterit _2)|Baiti saanud sekundis|Võrgu kasutajaliidese objekt|
|Võrgu kasutajaliidese (Microsoft Virtual masina siini võrgu adapterit _2)|Baiti saadetud sekundis|Võrgu kasutajaliidese objekt|
|Võrgu kasutajaliidese (Microsoft Virtual masina siini võrgu adapterit _2)|Kogusumma baiti sekundis|Võrgu kasutajaliidese objekt|

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Luua ja lisada kohandatud jõudluse hinnale rakenduse

Azure'i toetab luua ja muuta kohandatud jõudluse hinnale web rollid ja töötaja rollid. Jälgimiseks ja rakendusele vastav käitumine võib kasutada funktsiooni hinnale. Saate luua ja kustutada kohandatud jõudluse counter kategooriad ja tunnused käivitus tööülesande, web rolli või töötaja rolli laiendatud õigustega.

>[AZURE.NOTE] Kood, mis muudab kohandatud jõudluse hinnale on tõstetud käivitamiseks õigused. Kui kood on web rolli või töötaja rolli, roll peab sisaldama silti <Runtime executionContext="elevated" /> ServiceDefinition.csdef faili rolli korralikult.

Saate saata kohandatud jõudluse andmete kasutamise diagnostika agent Azure salvestusruumi.

Funktsiooni jõudlust standard andmete luuakse Azure protsessid. Kohandatud jõudluse andmete peab loodud rolli või töötaja rolli veebirakenduses. Teemast [Jõudluse Counter tüübid](https://msdn.microsoft.com/library/z573042h.aspx) andmeid saab salvestada kohandatud jõudluse hinnale tüüpi teavet. Vaadake [PerformanceCounters valimi](http://code.msdn.microsoft.com/azure/) näide, mis loob ja määrab kohandatud jõudluse andmete web roll.

## <a name="store-and-view-performance-counter-data"></a>Jõudluse andmete salvestamine ja kuvamine

Azure'i vahemälu jõudluse andmete koos muude diagnostikateave. Andmed on saadaval jälgida töötamise rolli eksemplari kaugtöölaua juurdepääs abil saate vaadata näiteks jõudluse jälgimine. Püsima andmed väljaspool rolli eksemplari, peab diagnostika agent andmete edastamine Azure salvestusruumi. Soovitud andmete vahemällu talletatud jõudluse mahupiirangu saab konfigureerida diagnostika agent või see võib olla konfigureeritud ühiskasutuses tähtaeg on Diagnostikaandmete osa. Puhvri suurus määramise kohta leiate lisateavet teemast [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) ja [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Leiate ülevaate häälestamise diagnostika agent andmete edastamiseks salvestusruumi konto [poe ja Azure Storage diagnostika andmete kuvamine](https://msdn.microsoft.com/library/azure/hh411534.aspx) .

Iga konfigureeritud jõudluse counter eksemplar on salvestatud määratud valimite määra ja valimisse andmed on üle salvestusruumi konto ajastatud edastamine taotluse või taotluse nõudmisel edastamine. Automaatse üleandmise võib ajastatud nii tihti kui ühe korra minutis. Jõudluse andmete üle diagnostika agent talletatakse tabeli WADPerformanceCountersTable, salvestusruumi konto. Selles tabelis võib juurde ja standard Azure storage API meetodid esitatakse selle kohta päring. Lugege teemat [Microsoft Azure'i PerformanceCounters valimi](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) näitena päringu- ja jõudluse andmete WADPerformanceCountersTable tabeli kuvamine.

>[AZURE.NOTE] Sõltuvalt diagnostika agent edastamine sagedus ja järjekorra latentsus, võib selle viimase jõudluse andmete salvestusruumi konto minuti aegunud.

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Jõudluse hinnale diagnostika konfiguratsioonifail kasutamise lubamine

Järgmiste toimingute abil oma Azure rakenduse jõudluse hinnale.

## <a name="prerequisites"></a>Eeltingimused

Selles jaotises eeldab, et imporditud diagnostika kuvari rakenduse ja diagnostika konfiguratsioonifail lisatakse teie Visual Studio lahendus (diagnostics.wadcfg SDK 2.4 ja allpool või diagnostics.wadcfgx SDK 2,5 ja rohkem). Vaadake juhiseid 1 ja 2 [Lubada diagnostika Azure'i pilveteenustega ja Virtuaalmasinates](./cloud-services-dotnet-diagnostics.md)) lisateabe saamiseks.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Samm 1: Koguda ja jõudluse hinnale andmete talletamine

Kui olete lisanud oma Visual Studio lahendus diagnostika faili saate konfigureerida saidikogumi ja jõudluse andmete säilitamine Azure rakenduses. Seda tehakse, lisades jõudluse hinnale diagnostika fail. Diagnostika andmed, sh jõudluse hinnale, kogutakse esmalt eksemplari. Seejärel püsis andmete WADPerformanceCountersTable tabelisse teenuses Azure tabeli nii, et peate ka rakenduse salvestusruumi konto määramine. Kui olete rakenduse kohalik katsetamine emulaator arvutada, saate ka talletada diagnostika andmete kohalikult salvestusruumi emulaator. Enne diagnostika andmete talletamiseks peate esmalt [Azure klassikaline portaali](http://manage.windowsazure.com/) ja salvestusruumi konto loomine. Hea tava on leidmiseks konto salvestusruumi sama geograafilise asukoha nimega Azure rakenduse maksmise välise läbilaskevõime kulude vältimiseks ja vähendamiseks latentsus.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Jõudluse hinnale diagnostika-faili lisamine

On palju hinnale saate kasutada. Järgmises näites on kujutatud mitu jõudluse väidab, et on soovitatav web ja töötaja rolli jälgimiseks.

Avage fail diagnostika (diagnostics.wadcfg SDK 2.4 ja allpool või diagnostics.wadcfgx SDK 2,5 ja kohal) ja lisage järgmine DiagnosticMonitorConfiguration elementi:

```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
       <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
    </PerformanceCounters>
```

BufferQuotaInMB atribuut, mis määrab maksimaalse faili süsteemi salvestusruumi, mis on saadaval andmetüüp saidikogumi (Azure'i logid, IIS-i logid). Vaikimisi on 0. Kvoodi jõudmisel vanimast andmed on kustutatud uute andmete lisamise. BufferQuotaInMB atribuutide summa peab olema suurem kui OverallQuotaInMB atribuudi väärtust. Täpsema teabe määramise, kui palju salvestusruumi on vaja diagnostika andmete kogumine, [Tõrkeotsingu head tavad Azure'i rakenduste arendamise](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx)jaotisest häälestamise WAD.

Atribuut scheduledTransferPeriod, mis määrab ajastatud andmete edastamine vaheline intervall, ülespoole lähima minutid. Järgmistes näidetes seatakse see PT30M (30 minuti). Edastamine ajavahemiku, väike väärtuseks 1 minut, nt avaldab negatiivset mõju teie taotlus jõudluse valmistamisel, kuid võib olla kasulik näha diagnostika töötamise kiiresti, kui teil on testimine. Ajastatud edastamine peaks olema piisavalt väike, et tagada, et Diagnostikaandmete ei kirjutata üle eksemplari, kuid piisavalt suur, et see ei mõjuta rakenduse jõudlus.

Atribuut counterSpecifier määrab jõudlust on vastuolus koguda. Proovid atribuut määrab määr, kus jõudluse näidiku peaks proov, sellisel juhul 30 sekundit.

Kui olete lisanud soovitud jõudluse väidab, et soovite koguda, salvestage muudatused diagnostika-faili. Järgmiseks peate määrama salvestusruumi konto, mille diagnostika andmed jätkunud.

### <a name="specify-the-storage-account"></a>Salvestusruumi konto määramine

Püsivad diagnostika teabe Azure Storage kontoga, peate määrama ühendusstringi teenuse konfigureerimine (ServiceConfiguration.cscfg) faili.

Azure'i SDK 2,5 salvestusruumi konto saab määrata diagnostics.wadcfgx faili.

>[AZURE.NOTE] Need juhised kehtivad ainult Azure SDK 2,4 ja all. Azure'i SDK 2,5 salvestusruumi konto saab määrata diagnostics.wadcfgx faili.

Ühenduse stringid seadmiseks tehke järgmist.

1. Avage ServiceConfiguration.Cloud.cscfg faili, kasutades oma lemmik tekstiredaktoris ja oma Storage ühendusstring. *Account_name* ja *AccountKey* väärtused leiate Azure'i klassikaline portaali salvestusruumi konto armatuurlaua klahvid haldamine jaotises.

    ```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Salvestage fail ServiceConfiguration.Cloud.cscfg.

3. Avage fail ServiceConfiguration.Local.cscfg ja veenduge, et UseDevelopmentStorage on seatud väärtusele tõene.

    ```
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
Nüüd, kui ühendus stringid on määratud, püsima rakenduse rakenduse juurutamisel diagnostika andmete salvestusruumi kontole.
4. Salvestamine ja koostada projekti, siis rakenduse juurutamine.

## <a name="step-2-optional-create-custom-performance-counters"></a>Samm 2: (Valikuline) Loo kohandatud jõudluse hinnale

Lisaks eelmääratletud jõudluse hinnale, saate lisada oma kohandatud jõudluse hinnale jälgida veebis või töötaja rollid. Jälgida ja jälgida rakenduse kohased käitumist ja saab loodud või kustutatud käivitus tööülesande, web rolli või töötaja rolli laiendatud õigustega võib kasutada kohandatud jõudluse hinnale.

Azure'i diagnostika agent värskendab jõudluse counter konfiguratsiooni failist .wadcfg ühe minuti järel.  Kui loote kohandatud jõudluse hinnale OnStart meetodit ja tööülesannete käivitus kuluda rohkem kui üks minut käivitada, oma kohandatud jõudluse hinnale pole on loodud kui Azure diagnostika agent proovib laadida.  Sel juhul näete, et Azure'i diagnostika õigesti sisaldab diagnostika kõik andmed, välja arvatud teie kohandatud jõudluse hinnale.  Selle probleemi lahendamiseks letid käivitus ülesande või teisaldada mõne käivitus tööülesande töö OnStart meetodil jõudluse hinnale loomise järel.

Järgmiste toimingute loomine on lihtne kohandatud jõudluse vastuollu nimega "\MyCustomCounterCategory\MyButton1Counter":

1. Avage teenus määratluse faili (CSDEF) rakenduse.
2. Käitusaja element on WebRole või WorkerRole elemendi luba Andmetäite administraatoriõigustega lisamiseks tehke järgmist.

    ```
    <runtime executioncontext="elevated"/>
    ```
3. Salvestage fail.
4. Avage fail diagnostika (diagnostics.wadcfg SDK 2.4 ja allpool või diagnostics.wadcfgx SDK 2,5 ja kohal) ja lisage järgmine tekst on DiagnosticMonitorConfiguration 

    ```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
     <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Salvestage fail.
6. Luua kohandatud Jõudluseloenduri kategooria OnStart viis oma rolli enne base kasutada. OnStart. Järgmises näites C# loob kohandatud kategooria, kui see pole juba olemas:

    ```
    public override bool OnStart()
    {
    if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
    {
       CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

       // add a counter tracking user button1 clicks
       CounterCreationData operationTotal1 = new CounterCreationData();
       operationTotal1.CounterName = "MyButton1Counter";
       operationTotal1.CounterHelp = "My Custom Counter for Button1";
       operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
       counterCollection.Add(operationTotal1);

       PerformanceCounterCategory.Create(
         "MyCustomCounterCategory",
         "My Custom Counter Category",
         PerformanceCounterCategoryType.SingleInstance, counterCollection);

       Trace.WriteLine("Custom counter category created.");
    }
    else{
       Trace.WriteLine("Custom counter category already exists.");
    }

    return base.OnStart();
    }
    ```
7. Värskendage hinnale oma rakenduses. Järgmises näites värskendab vastu kohandatud jõudluse Button1_Click sündmuste kohta:

    ```
    protected void Button1_Click(object sender, EventArgs e)
        {
         PerformanceCounter button1Counter = new PerformanceCounter(
           "MyCustomCounterCategory",
           "MyButton1Counter",
           string.Empty,
           false);
         button1Counter.Increment();
        this.Button1.Text = "Button 1 count: " +
           button1Counter.RawValue.ToString();
        }
    ```
8. Salvestage fail.  

Kohandatud jõudluse andmete kogub nüüd Azure diagnostika kuvar.

## <a name="step-3-query-performance-counter-data"></a>Samm 3: Päringu jõudluse andmete

Kui teie taotlus on juurutatud ja töötab diagnostika kuvari alustab kogumiseks jõudluse hinnale ja püsib Azure mäluruumi andmeid. Näiteks Server Explorer Visual Studios, [Azure'i salvestusruumi Explorer](http://azurestorageexplorer.codeplex.com/)või Cerebrata [Azure diagnostika Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) abil saate vaadata hinnale jõudlusandmeid WADPerformanceCountersTable tabelis. Tabeli teenuse [C#](../storage/storage-dotnet-how-to-use-tables.d), [Java](../storage/storage-java-how-to-use-table-storage.md), [Node.js](../storage/storage-nodejs-how-to-use-table-storage.md), [Python](../storage/storage-python-how-to-use-table-storage.md), [Ruby](../storage/storage-ruby-how-to-use-table-storage.md)või [PHP](../storage/storage-php-how-to-use-table-storage.md)abil saab ka programatically päringud.

Järgmises näites C# lihtne päring WADPerformanceCountersTable tabel näitab ja salvestab diagnostika andmed CSV-faili. Kui jõudluse hinnale on salvestatud CSV-faili abil saate Microsoft Excelis või mõnda muud tööriista Graafikavidinate võimaluste nende andmete visualiseerimiseks. Ärge unustage lisada viide Microsoft.WindowsAzure.Storage.dll, mis on kaasatud Azure'i SDK .net-i oktoober 2012 ja uuemad versioonid. Selle komplekti on installitud % programmi Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ kataloogi.

```
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ...

    // Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
    // you store your connection string using the Microsoft Azure service configuration
    // system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
    // to retrieve your storage connection string.  If you're not using Cloud Services, it's
    // recommended that you store the connection string in your web.config or app.config file.
    // Use the ConfigurationManager type to retrieve your storage connection string.

    string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
    //string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

    // Get a reference to the storage account using the connection string.  You can also use the development
    // storage account (Storage Emulator) for local debugging.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    //CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
    CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

    // Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
    TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
       .Where(
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
          TableOperators.And,
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
          TableOperators.And,
          TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
          )
       )
    );

    // Execute the table query.
    IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

    // Process the query results and build a CSV file.
    StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

    foreach (PerformanceCountersEntity entity in result)
    {
       sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
          + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
    }

    StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
    sw.Write(sb.ToString());
    sw.Close();
```

Üksuste vastendamine C# objektid, kasutades kohandatud klassi, mis on saadud **TableEntity**. Järgmine kood määratleb üksuse ainekursuse, **WADPerformanceCountersTable** tabeli vastu jõudluse tähistava.


    public class PerformanceCountersEntity : TableEntity
    {
       public long EventTickCount { get; set; }
       public string DeploymentId { get; set; }
       public string Role { get; set; }
       public string RoleInstance { get; set; }
       public string CounterName { get; set; }
       public double CounterValue { get; set; }
    }


## <a name="next-steps"></a>Järgmised sammud
[Kuva täiendavad artiklid Azure'i diagnostika] (.. / azure-diagnostics.md)
