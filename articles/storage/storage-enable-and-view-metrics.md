<properties
    pageTitle="Talletusruumi mõõdikute Azure'i portaalis lubamine | Microsoft Azure'i"
    description="Kuidas lubada talletusruumi mõõdikute bloobimälu, järjekorda, tabeli või faili teenuste jaoks"
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a>Azure'i talletusruumi mõõdikute lubamine ja mõõdikute andmete vaatamine

[AZURE.INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Ülevaade

Vaikimisi on lubatud talletusruumi mõõdikute salvestusruumi teenused. Saate lubada jälgimise [Azure portaali](https://portal.azure.com) või Windows PowerShelli kaudu või programmiliselt kaudu salvestusruumi kliendi teek.

Kui talletusruumi mõõdikute lubamiseks peate valima säilitatakse andmed: selle aja määratleb kaua Storage teenus hoiab mõõdikute ja kulud, saate ruumi nõutav talletada. Tavaliselt, peaksite kasutama lühemaks säilitusperiood minute mõõdikute kui kord tunnis mõõdikute jaoks vaja minute mõõdikute märgatavat lisaruumi tõttu. Valida mõne säilitusperiood nii, et teil on piisavalt aega andmete analüüsimiseks ja mis tahes mõõdikute, mida soovite säilitada vallasrežiimis analüüsiks või aruandluse alla laadida. Pidage meeles, et teil on ka toimub konto salvestusruumi mõõdikute andmete allalaadimise.

## <a name="how-to-enable-metrics-using-the-azure-portal"></a>Kuidas lubada mõõdikute Azure'i portaalis

Mõõdikute [Azure portaali](https://portal.azure.com)lubamiseks tehke järgmist

1. Liikuge oma konto salvestusruumi.
1. Avage **sätted** tera ja valige **diagnostika**.
1. Veenduge, **et oleku **kohta**** .
1. Valige mõõdikute teenuste, mida soovite jälgida.
2. Määrake säilituspoliitika tähistamiseks kaua mõõdikute säilitamise ja logige andmed.

Pange tähele, et [Azure portaali](https://portal.azure.com) pole praegu võimaldavad minute mõõdikute salvestusruumi konto konfigureerimine tuleb lubada minute mõõdikute PowerShelli kaudu või programmiliselt.

## <a name="how-to-enable-metrics-using-powershell"></a>Kuidas lubada mõõdikute PowerShelli abil

Oma kohalikus arvutis PowerShelli abil saate konfigureerimine talletusruumi mõõdikute konto salvestusruumi Azure PowerShelli cmdlet-käsk Get-AzureStorageServiceMetricsProperty tuua kehtivad sätted ja cmdlet-käsu Set-AzureStorageServiceMetricsProperty abil praeguse sätete muutmine.

Järgmiste parameetrite juhtida talletusruumi mõõdikute cmdlet-käskude kasutamine

- MetricsType: võimalikud väärtused on tunni ja minutiga.

- ServiceType: võimalikud väärtused on bloobimälu, järjekord ja tabel.

- MetricsLevel: võimalikud väärtused on pole, teenuse ja ServiceAndApi.

Näiteks järgmine käsk lülitub sisse minute mõõdikute bloobimälu teenuse konto vaikimisi salvestusruumi koos säilitamine viis päeva jooksul määramine:

`Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`

Järgmine käsk toob praeguse kord tunnis mõõdikute taseme ja säilituspoliitikate päeva bloobimälu teenuse konto vaikimisi salvestusruumi:

`Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob`

Azure PowerShelli cmdletid töötada Azure tellimuse ja valimine vaikimisi salvestusruumi konto, mida soovite kasutada, leiate teavet selle kohta, kuidas konfigureerida: [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Kuidas lubada talletusruumi mõõdikute programmiliselt

Järgmised C# koodilõigu näitab, kuidas lubada mõõdikute ja logimine bloobimälu teenuse salvestusruumi kliendi teek .net-i jaoks:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Enable Storage Analytics logging and set retention policy to 10 days.
    ServiceProperties properties = new ServiceProperties();
    properties.Logging.LoggingOperations = LoggingOperations.All;
    properties.Logging.RetentionDays = 10;
    properties.Logging.Version = "1.0";

    // Configure service properties for metrics. Both metrics and logging must be set at the same time.
    properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.HourMetrics.RetentionDays = 10;
    properties.HourMetrics.Version = "1.0";

    properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.MinuteMetrics.RetentionDays = 10;
    properties.MinuteMetrics.Version = "1.0";

    // Set the default service version to be used for anonymous requests.
    properties.DefaultServiceVersion = "2015-04-05";

    // Set the service properties.
    blobClient.SetServiceProperties(properties);


## <a name="viewing-storage-metrics"></a>Talletusruumi mõõdikute vaatamine

Kui olete salvestusruumi Analytics mõõdikute konto salvestusruumi jälgimiseks, kirjete salvestusruumi Analytics mõõdikud tuntud tabelid, oma konto salvestusruumi. Saate konfigureerida diagrammide vaatamiseks kord tunnis mõõdikute [Azure portaali](https://portal.azure.com).

1. Liikuge [Azure portaali](https://portal.azure.com)konto salvestusruumi.
2. Klõpsake jaotises **jälgimine** lisada uue diagrammi **Lisamine paanid** . **Paani Galerii**, valige meetermõõdustik, mida soovite vaadata, ja lohistage see jaotisse **jälgimine** .
3. Millised mõõdikute kuvatakse diagrammi redigeerimiseks klõpsake linki **Redigeeri** . Saate lisada või eemaldada üksikuid mõõdikute valides või nende tühistamine.
4. Kui olete mõõdikute redigeerimise lõpetanud, klõpsake nuppu **Salvesta** .

Kui soovite alla laadida pikaajalise Storage mõõdikute või analüüsida neid kohalikult, peate:

- Kasutage tööriista, mis on need tabelid teadlik ja võimaldab teil vaadata ja neid alla laadida.
- Kirjutage kohandatud rakendus või skripti lugeda ja talletada tabelid.

Paljud muude tootjate salvestusruumi sirvimise tööriistad on need tabelid teadlikud ja saate neid vaadata otse.
Saadaolevad tööriistad loendi leiate [Azure'i salvestusruumi maadeavastajad](storage-explorers.md) .

> [AZURE.NOTE] Alates versioonist 0.8.0 [Microsoft Azure'i salvestusruumi Explorer] (http://storageexplorer.com/), saate nüüd on võimalik vaadata ja alla laadida analytics mõõdikute tabelid.

Juurdepääsuks analytics tabelid programmiliselt, Pange tähele, et analytics tabeleid ei kuvata, kui kõigi tabelite loendi teie salvestusruumi konto. Saate neile juurde pääseda otse nime alusel või päringu tabelinimede .net-i kliendi teek [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) kasutamine.

### <a name="hourly-metrics"></a>Kord tunnis mõõdikud
- $MetricsHourPrimaryTransactionsBlob
- $MetricsHourPrimaryTransactionsTable
- $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Minute mõõdikud
- $MetricsMinutePrimaryTransactionsBlob
- $MetricsMinutePrimaryTransactionsTable
- $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Võimsus
- $MetricsCapacityBlob

Nende tabelite veebisaidil [Salvestusruumi Analytics mõõdikute tabeli skeemi](https://msdn.microsoft.com/library/azure/hh343264.aspx)leiate üksikasjadega skeemid. Valimi read allpool kuvatakse veerud, mis on saadaval ainult alamhulga, kuid kirjeldavad mõned olulised funktsioonid talletusruumi mõõdikute salvestab need mõõdikud viisi:

| PartitionKey  |       RowKey       |                    Ajatempli | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Kättesaadavus | AverageE2ELatency | AverageServerLatency | PercentSuccess |
|---------------|:------------------:|-----------------------------:|---------------|-----------------------|--------------|-------------|--------------|-------------------|----------------------|----------------|
| 20140522T1100 |      kasutaja; Kõik      | 2014-05-22T11:01:16.7650250Z | 7             | 7                     | 4003         | 46801       | 100          | 104.4286          | 6.857143             | 100            |
| 20140522T1100 | kasutaja; QueryEntities | 2014-05-22T11:01:16.7640250Z | 5             | 5                     | 2694         | 45951       | 100          | 143.8             | 7,8                  | 100            |
| 20140522T1100 |  kasutaja; QueryEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 538          | 633         | 100          | 3                 | 3                    | 100            |
| 20140522T1100 | kasutaja; UpdateEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 771          | 217         | 100          | 9                 | 6                    | 100               |

Selles näites minute mõõdikute andmete kasutab partition võti minute eraldusvõimega aega. Rea võti tuvastab tüüpi teavet, mis on talletatud rida ja see koosneb kahe andmeühikuga teabe, juurdepääsu tüüp ja taotluse tüüp.

- Accessi tüüp on kasutaja või süsteemi, kui kasutaja viitab kõik kasutaja taotlused salvestusruumi teenusega, ja süsteemi salvestusruumi Analytics taotlusi.

- Taotluse tüüp on kõik, mille puhul on kokkuvõte rida või see tuvastab teatud API, nt QueryEntity või UpdateEntity.


Ülaltoodud näidisandmed kuvatakse kõigi kirjete (alates 11:00 AM) ühe minuti, nii, et QueryEntities taotluste arv pluss QueryEntity arvu taotleb pluss arvu UpdateEntity taotleb lisada kuni seitse, mis on kuvatud kasutaja: kõik rida. Samuti võite saada keskmise-lõpuni latentsuse 104.4286 kasutaja: kõik reale arvutades ((143.8 * 5) + 3 + 9) / 7.

Kaaluge häälestamise [Azure portaali](https://portal.azure.com) lehe jälgimine teatiste nii, et talletusruumi mõõdikute automaatselt teavitada, saate olulisi muudatusi salvestusruumi teenuste toimimist. Kui kasutate salvestusruumi Exploreri tööriista allalaadimiseks mõõdikute andmed eraldatud vormingus, saate Microsoft Exceli andmete analüüsimiseks. Leiate ajaveebipostitusest [Microsoft Azure'i salvestusruumi maadeavastajad](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) loendi saadaolevat Exploreris tööriistad.



## <a name="accessing-metrics-data-programmatically"></a>Mõõdikute andmetele programmiliselt juurde pääseda

Järgmine loend sisaldab valimi C# koodi, mis avab minute mõõdikute vahemiku minutid, mille jaoks ja kuvab tulemused konsooli aken. Azure'i salvestusruumi teegi versioon 4, mis sisaldab CloudAnalyticsClient klassi, mis lihtsustab juurdepääs mõõdikute tabelite salvestusruumi kasutab.

    private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
    {
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
    Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
    var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
    var t = analyticsClient.GetMinuteMetricsTable(service);
    var opContext = new OperationContext();
    var query =
    from entity in metricsQuery
    // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
    // because they are calculated fields in the MetricsEntity class.
    // The PartitionKey identifies the DataTime of the metrics.
    where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0
    select entity;

    // Filter on "user" transactions after fetching the metrics from Table Storage.
    // (StartsWith is not supported using LINQ with Azure table storage)
    var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
    var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
    Console.WriteLine(resultString);
    }
    }

    private static string MetricsString(MetricsEntity entity, OperationContext opContext)
    {
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;

    }




## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Millised kulud teil tekkida, kui lubate talletusruumi mõõdikute?

Kirjutage taotlusi luua tabeli üksuste jaoks mõõdikute eest tuleb tasuda ühtsete rakendatav Azure Storage toiminguid.

Lugemis- ja Kustuta taotlused kliendi mõõdikute andmeid on ka standardse määrade arveldatavate. Kui olete konfigureerinud andmete säilituspoliitika, on ei lisandu kui Azure Storage kustutab vana mõõdikute andmeid. Juhul, kui analytics andmete kustutamiseks teie konto on eest kustutada toimingute.

Mida mõõdikud tabelid on ka arveldatavate: saate hinnata, võimsuse kasutatakse mõõdikute andmete salvestamiseks järgmist:

- Kui iga tunni teenus kasutab iga API iga teenus, siis ligikaudu 148KB andmed talletatakse tunnis mõõdikute tehingu tabelite kui teil on lubatud nii teenuse ja API taseme kokkuvõte.

- Kui iga tunni teenus kasutab iga API iga teenus, siis ligikaudu 12KB andmeid talletatakse tunnis mõõdikute tehingu tabelite kui teil on lubatud ainult teenusetaseme kokkuvõte.

- Plekid võimsus tabelil on kaks rida lisatakse iga päev (kui kasutaja on liitunud logide): See tähendab, et iga päev selle tabeli suurus suurendab kuni umbes 300 baiti.

## <a name="next-steps"></a>Edasi-järgmiselt:
[Salvestusruumi logimine ja juurdepääs logiandmed lubamine](https://msdn.microsoft.com/library/dn782840.aspx)
