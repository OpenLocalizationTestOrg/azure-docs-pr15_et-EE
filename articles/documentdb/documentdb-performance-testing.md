<properties 
    pageTitle="DocumentDB skaala ja jõudluse testimine | Microsoft Azure'i" 
    description="Saate teada, kuidas teha skaala ja jõudluse Azure'i DocumentDB testimine"
    keywords="jõudluse testimine"
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="performance-and-scale-testing-with-azure-documentdb"></a>Jõudlus ja skaala Azure'i DocumentDB testimine
Jõudlus ja katsetada on oluline samm rakenduste arendamise. Rakendustele, andmebaasi taseme on oluline mõju üldine jõudlus ja skaleeritavus ja seetõttu on kriitiline osa jõudluse testimine. [Azure'i DocumentDB](https://azure.microsoft.com/services/documentdb/) on ehitatud elastne skaala ja hõlpsalt prognoosida ja seetõttu suur mahutamiseks rakendusi, mis on vaja suure jõudlusega andmebaasi taseme. 

Selles artiklis on arendajatele rakendamiseks jõudluse test komplektide oma DocumentDB töökoormus või hindamisel DocumentDB suure jõudlusega rakenduse stsenaariumid viide. See keskendub isoleeritakse jõudluse testimine andmebaasi, kuid sisaldab ka head tavad tootmise rakendused.

Pärast selle artikli lugemist on võimalik vastata järgmistele küsimustele:   

- Kust leian valimi .NET klientrakendusega Azure'i DocumentDB jõudluse testimiseks? 
- Kuidas ma saavutada kõrge läbilaskevõime taset Azure'i DocumentDB minu klientrakendusega?

Kood alustada laadige projekti [DocumentDB jõudluse testimine valimi](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)põhjal. 

> [AZURE.NOTE] Selle rakenduse eesmärk on näidata head tavad ekstraktimiseks väheste klientarvutite ja parema jõudluse DocumentDB välja. Ei tehtud näitamaks, et teenus, kus saate mastaapimiseks limitlessly tipp võimsus.

Kui otsite kliendipoolne konfiguratsiooni suvandid DocumentDB jõudluse parandamiseks, lugege teemat [DocumentDB jõudluse näpunäited](documentdb-performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Käivitage rakendus testimine jõudlus
Kiireim viis alustamiseks on kompileerida ja käivitada .NET valimi all, järgides allolevaid juhiseid. Samuti saate vaadata lähtekoodi ja rakendada sarnaseid konfiguratsioone oma klientrakendustele.

**Samm 1:** Projekti saate alla laadida [DocumentDB jõudluse testimine valimi](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)või kahvli Github hoidla.

**Samm 2:** EndpointUrl, AuthorizationKey, CollectionThroughput ja DocumentTemplate (valikuline) App.config sätete muutmine.

> [AZURE.NOTE] Enne ettevalmistamise saidikogumid kõrge läbilaskevõime, vaadake [Lehe hinnad](https://azure.microsoft.com/pricing/details/documentdb/) kulude kogumise kohta. DocumentDB arved salvestusruumi ja läbilaskevõime iseseisvalt üks kord tunnis, nii saate salvestada kulud kustutades või vähendada oma DocumentDB kogumite läbilaskevõime pärast testimine.

**Samm 3:** Kompileerida ja käivitage rakendus konsooli käsurea kaudu. Peaksite nägema väljundi umbes selline:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Samm 4 (vajadusel):** Funktsiooni läbilaskevõime teatatud (RU/s) tööriista peaks olema sama või suurem kui ettevalmistatud läbilaskevõime kogumist. Kui ei, suurendades DegreeOfParallelism sammhaaval võib aidata teil jõuda limiit. Kui läbilaskevõime teie kliendi rakenduse kordi, käivitades rakenduse samas või mõnes muus masinad mitme eksemplari aitab teil jõuda ettevalmistatud limiit üle erinevate aknad. Kui vajate abi selle toiminguga, palun kirjutage e-posti askdocdb@microsoft.com või tugiteenuste Piletite täita.

Kui teil on rakendus töötab, võite proovida erinevaid [indekseerimise poliitikate](documentdb-indexing-policies.md) ja [järjepidevuse tasemed](documentdb-consistency-levels.md) mõista nende mõju läbilaskevõime ja latentsusaeg. Samuti saate vaadata lähtekoodi ja sarnase konfiguratsioone test komplektid või mängufilmide rakendada.

## <a name="next-steps"></a>Järgmised sammud
Selles artiklis me vaevama kuidas saab teha jõudlus ja skaala testimine DocumentDB .net-i konsooli rakenduse kasutamine. Allolevate linkide DocumentDB töötamise kohta lisateabe saamiseks vaadake.

* [DocumentDB jõudluse testimise näidis](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Kliendi konfiguratsiooni suvandid DocumentDB jõudluse parandamiseks](documentdb-performance-tips.md)
* [Serveripoolne eraldamine DocumentDB sisse](documentdb-partition-data.md)
* [DocumentDB saidikogumid ja jõudluse tasemed](documentdb-performance-levels.md)
* [MSDN-i DocumentDB .NET SDK dokumentatsioon](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [DocumentDB .net-i näited](https://github.com/Azure/azure-documentdb-net)
* [Jõudluse näpunäiteid DocumentDB ajaveeb](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)
