<properties 
    pageTitle="DocumentDB andmebaasi küsimused - korduma kippuvad küsimused | Microsoft Azure'i" 
    description="Siit leiate vastused korduma kippuvatele küsimustele Azure'i DocumentDB NoSQL dokumendi andmebaasi teenuse JSON. Andmebaasi küsimustele võimsus, jõudlus tasemed ja skaleerimist kohta." 
    keywords="Korduma kippuvad küsimused, documentdb, Azure'i, Microsoft Azure'i andmebaasi küsimustele"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="mimig"/>


#<a name="frequently-asked-questions-about-documentdb"></a>Korduma kippuvad küsimused DocumentDB kohta

## <a name="database-questions-about-microsoft-azure-documentdb-fundamentals"></a>Andmebaasi Microsoft Azure'i DocumentDB põhikomponentide seotud küsimused

### <a name="what-is-microsoft-azure-documentdb"></a>Mis on Microsoft Azure'i DocumentDB? 
Microsoft Azure'i DocumentDB on on väga kiiresti, planeedi-skaala NoSQL dokumendi andmebaasi-kui-a-teenus, mis pakub rikkaliku päringute üle skeemi tasuta andmed, aitab pakkuda konfigureeritav ja usaldusväärne jõudlust ja võimaldab kiire arengu, kõik toetavad power hallatavate platvormi kaudu ja jõuavad Microsoft Azure. DocumentDB on õige web, mobiil, mängimine ja asjade rakendused, kui prognoositavad läbilaskevõime, kõrge-saadavus, madal latentsus ja skeemi tasuta andmemudeli on võti nõuetele. DocumentDB pakub skeemi paindlikkust ja indekseerimise kaudu kohalikke JSON andmemudeli RTF-vormingus ja sisaldab mitme dokumendi selgituseks tugi integreeritud JavaScript.  
  
Veel andmebaasi küsimused, vastused ja juurutamine ja selle teenuse abil juhised leiate [DocumentDB dokumentatsiooni leht](https://azure.microsoft.com/documentation/services/documentdb/).

### <a name="what-kind-of-database-is-documentdb"></a>Millist liiki andmebaas on DocumentDB?
DocumentDB on NoSQL dokumendi saamine andmebaasi, mis salvestab andmed JSON-vormingus.  DocumentDB toetab pesastatud, enda contained andmete struktuurid, mida saate kasutataks rikkaliku DocumentDB [SQL-päringu grammatika](documentdb-sql-query.md)kaudu. DocumentDB pakub suure jõudlusega selgituseks töötlemine serveripoolne JavaScripti kaudu [Salvestatud toimingute, päästikute, ja kasutaja määratletud funktsioonid](documentdb-programming.md). Andmebaasi toetab samuti arendaja Timmitavad järjepidevuse tasemed seotud [jõudluse tasemed](documentdb-performance-levels.md).
 
### <a name="do-documentdb-databases-have-tables-like-a-relational-database-rdbms"></a>Kas DocumentDB andmebaasid on tabelid, nt relatsiooniandmebaasist (RDBMS)?
Ei, DocumentDB salvestab andmed JSON dokumentide kogumid.  DocumentDB ressursside kohta leiate teavet teemast [DocumentDB ressursi mudel ja põhimõtet](documentdb-resources.md). Kuidas NoSQL lahendusi, nt DocumentDB erineb relatsiooniline lahenduste kohta leiate lisateavet teemast [NoSQL vs SQL-i](documentdb-nosql-vs-sql.md).

### <a name="do-documentdb-databases-support-schema-free-data"></a>Kas DocumentDB andmebaaside toetavad skeemi tasuta andmeid?
Jah, DocumentDB võimaldab rakenduste suvalise JSON dokumente ilma skeemi definitsiooni või näpunäiteid salvestada. Andmed on päringu DocumentDB SQL-päringu kasutajaliidese jaoks kohe saadaval.   

### <a name="does-documentdb-support-acid-transactions"></a>Kas DocumentDB toetab HAPPE tehingud?
Jah, toetab DocumentDB rist-dokumendi tehingud väljendatud hinnaks dollarites JavaScripti salvestatud toimingute ja käivitab. Tehingute on rakendatud üks sektsiooni sees iga saidikogumi ja koos HAPPE semantikat nagu kõik teostamine või midagi muud samaaegselt täidesaatva kood ja kasutajale taotlused eraldatud.  Kui erandid visatakse server küljel täitmise JavaScripti rakenduse koodi abil, on kogu tehing tagasi pöörata. Tehingud kohta leiate lisateavet teemast [andmebaasi programm tehingud](documentdb-programming.md#database-program-transactions).

### <a name="what-are-the-typical-use-cases-for-documentdb"></a>Millised on tüüpilised kasutamine karpide DocumentDB?  
DocumentDB on hea valik Uus web, mobiil, mängimine ja asjade rakendused, kus automaatse skaala prognoositavad jõudluse tagamiseks kiire järjestuse millisekundilist vastuse korda ja päringu võimalus üle skeemi tasuta andmed on oluline. DocumentDB ärisündmuste kiire areng ja pidev iteratsiooni, rakenduse andmemudelid toetavad. Rakendused, mida kasutaja loodud sisu ja andmete haldamine on [levinud kasutamine karpide DocumentDB](documentdb-use-cases.md).  

### <a name="how-does-documentdb-offer-predictable-performance"></a>Kuidas DocumentDB pakkuda prognoositavad jõudlus?
[Taotluse üksus](documentdb-request-units.md) on rakenduses DocumentDB läbilaskevõime mõõtmine. 1 RU vastab 1KB dokumendi toomine jõudlus. Iga toiming DocumentDB, sh loeb, kirjutab, SQL-päringud ja salvestatud protseduur täitmised on tarkadeks RU väärtus, mis põhineb läbilaskevõime, toimingu lõpuleviimiseks nõutav. Selle asemel CPU, IO ja mälu ja kuidas need iga oma rakenduse läbilaskevõime mõju mõelda, saate mõelda ühe RU mõõt.

Iga saidikogumi DocumentDB saate reserveeritud ettevalmistatud läbilaskevõime RUs jõudluse sekundis osas. Rakenduste mis tahes skaala, saate võrrelda üksikuid taotlusi mõõta oma RU väärtuste ja sätte saidikogumid käsitlema taotluse üksuste kogusumma üle kõik kutsed. Saate skaala kuni või kui teie rakendus areneb edasi vajab teie saidikogumi läbilaskevõime mahtu. Lisateavet taotluse üksused ja abi saamiseks oma saidikogumi määratlemine vajab, lugege [haldamine jõudlus ja võimsus](documentdb-manage.md) ja proovige [läbilaskevõime kalkulaator](https://www.documentdb.com/capacityplanner). 

### <a name="is-documentdb-hipaa-compliant"></a>DocumentDB HIPAA vastab?
Jah, DocumentDB on HIPAA nõuetele. Luuakse HIPAA nõuete kasutamiseks, avaldamine, ja kaitsmine seisund eraldi tuvastamist võimaldav teave. Lisateavet leiate teemast [Microsoft Usalduskeskus](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-documentdb"></a>Mis on salvestuslimiitide, DocumentDB? 
Ei ole teoreetiline piiratud hulk andmeid, mida saab salvestada kogumi DocumentDB. Kui soovite üle 250 GB ühe saidikogumi andmete talletamiseks, siis on teie konto limiidist suurem [pöörduge klienditoe poole](documentdb-increase-limits.md) , et. 

### <a name="what-are-the-throughput-limits-of-documentdb"></a>Mis on DocumentDB läbilaskevõime piirid? 
Ei ole teoreetiline piiratud läbilaskevõime DocumentDB, klõpsake kogumi toetavad kogusumma kui teie töökoormus saab jagada umbes ühtlaselt piisavalt suure hulga partition võtmed. Kui soovite üle 250 000 taotluse üksuste/teise kohta või saidikogumi konto, palun [pöörduge klienditoe poole](documentdb-increase-limits.md) , et on teie konto limiidist suurendada. 

### <a name="how-much-does-microsoft-azure-documentdb-cost"></a>Kui palju maksab Microsoft Azure'i DocumentDB?
Vaadake [DocumentDB hinnakirjad üksikasjade](https://azure.microsoft.com/pricing/details/documentdb/) lehel üksikasjad. DocumentDB kasutamise eest on määratud arv saidikogumid kasutusel tundide kogumite olid võrgus, ja tarbitud salvestusruumi ja ettevalmistatud läbilaskevõime iga saidikogumi jaoks. 

### <a name="is-there-a-free-account-available"></a>On tasuta konto jaoks saadaval?
Kui olete kasutanud Azure, saate kasutajaks [Azure'i tasuta konto](https://azure.microsoft.com/free/), mis annab teile 30 päeva ja $200 proovida Azure teenuseid. Või kui teil on tellimus Visual Studios, teil on õigus saada [150 $ tasuta Azure autorid kuus](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) kasutada iga Azure'i teenus.  

### <a name="how-can-i-get-additional-help-with-documentdb"></a>Kuidas saan DocumentDB täiendavat abi?
Kui vajate abi, jõuda meile sisse [Virnas ületäitumise](http://stackoverflow.com/questions/tagged/azure-documentdb), [Azure'i DocumentDB MSDN-i arendaja Foorumid](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB)või [1:1 vestelda DocumentDB engineering meeskonnatöö](http://www.askdocdb.com/)plaanimine. Uudised DocumentDB ja uute funktsioonide kursis püsida, järgige meile [Twitteri](https://twitter.com/DocumentDB).

## <a name="set-up-microsoft-azure-documentdb"></a>Microsoft Azure'i DocumentDB häälestamine

### <a name="how-do-i-sign-up-for-microsoft-azure-documentdb"></a>Kuidas sisse logida Microsoft Azure'i DocumentDB jaoks?
Microsoft Azure'i DocumentDB on [Azure portaali][azure-portal].  Kõigepealt peab kasutajaks Microsoft Azure'i tellimus.  Pärast registreerumist Microsoft Azure'i tellimus, saate lisada Azure tellimuse DocumentDB konto. Juhised DocumentDB konto lisamise kohta leiate teemast [loomine DocumentDB andmebaasi konto](documentdb-create-account.md).   

### <a name="what-is-a-master-key"></a>Mis on lahti?
Juhtslaidi klahvi on Turbeloa juurdepääsu konto kõik ressursid. Võti üksikisikute on lugeda ja kirjutada Accessi andmebaasi konto kõigi ressursside. Olge ettevaatlik levitamise juhtslaidi võtmed. Primaarvõtme juhtslaidi ja teisese juhtslaidi võtme on saadaval [Azure portaali] **klahvid **tera[azure-portal]. Klahvid kohta leiate lisateavet teemast [Vaade, kopeerimine ja kiirklahvide uuesti luua](documentdb-manage-account.md#keys).

### <a name="how-do-i-create-a-database"></a>Kuidas luua andmebaasi?
Saate luua [Azure portaali]() abil, nagu on kirjeldatud [DocumentDB andmebaasi loomine](documentdb-create-database.md), [DocumentDB SDK-d](documentdb-sdk-dotnet.md)või [REST API-de](https://msdn.microsoft.com/library/azure/dn781481.aspx)andmebaasid.  

### <a name="what-is-a-collection"></a>Mis on kogumi?
Kogumi on JSON dokumendid ja seotud JavaScripti rakenduse loogika ümbris. Kogumi on tasustatav üksus, kus [maksumus](documentdb-performance-levels.md) määratakse soovitud läbilaskevõime ja storaged kasutada. Saidikogumite saab ühendada ühe või mitme sektsioonid/serveri ja saab skaala toime praktiliselt piiramatu salvestusruum või läbilaskevõime mahtu.

Saidikogumite on ka arvelduse üksused DocumentDB jaoks. Iga saidikogumi arve esitatud tunni põhjal ettevalmistatud läbilaskevõime ja kasutatud salvestusruumi. Lisateavet leiate teemast [DocumentDB hinnad](https://azure.microsoft.com/pricing/details/documentdb/).  

### <a name="how-do-i-set-up-users-and-permissions"></a>Kuidas häälestada kasutajad ja õigused?
Saate luua kasutajad ja õigused, kasutades ühte [DocumentDB SDK-d](documentdb-sdk-dotnet.md) või [REST API-de](https://msdn.microsoft.com/library/azure/dn781481.aspx)kaudu.   

## <a name="database-questions-about-developing-against-microsoft-azure-documentdb"></a>Andmebaasi arendamise vastu Microsoft Azure'i DocumentDB seotud küsimused

### <a name="how-to-do-i-start-developing-against-documentdb"></a>Kuidas teha käivitada arendamise DocumentDB vastu?
[SDK-d](documentdb-sdk-dotnet.md) on saadaval .NET, Python, Node.js, JavaScript ja Java.  Arendajad saate kasutada ka [Rahulik HTTP API -de](https://msdn.microsoft.com/library/azure/dn781481.aspx) suhelda DocumentDB ressursid mitmesuguseid platvormid ja keeled. 

Näidised DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md)ja [Python](documentdb-python-samples.md) SDK-d on saadaval github.

### <a name="does-documentdb-support-sql"></a>DocumentDB ei toeta SQL-i?
DocumentDB SQL-päringu keele on on toetatud SQL-i funktsioonide täiustatud alamhulga. DocumentDB SQL-päringu keele sisaldab RTF-vormingus hierarhilise ja seoselisi tehtemärke ja laiendatavuse kaudu JavaScript põhineb kasutaja määratletud funktsioonid (UDF). JSON grammatika võimaldab modelleerimine JSON dokumentide nimega õunapuude, mille sildid nimega puu sõlmed, mida kasutavad nii DocumentDB automaatse indekseerimise tehnika kui ka SQL-päringu keel, DocumentDB.  Üksikasjad, kuidas kasutada SQL-i grammatika, lugege teemadest [Päringu DocumentDB] [ query] artikkel.

### <a name="what-are-the-data-types-supported-by-documentdb"></a>Mis on DocumentDB toetatud andmetüübid?
Toetatud DocumentDB lihtsad andmetüübid on sama, mis JSON. JSON on lihtne tüüp süsteem, mis koosneb stringide, arvude (IEEE754 kahekordsete täpsus) ja loogikaväärtusi - väärtuse true, false ja tühiväärtusi.  Näiteks kuupäeva ja kellaaja, Guid, Int64 ja geomeetria keerukamaid andmetüübid saate esindatud nii JSON ja DocumentDB {} tehtemärk ja massiivi [tehtemärgi abil pesastatud objektide loomise kaudu. 

### <a name="how-does-documentdb-provide-concurrency"></a>Kuidas DocumentDB anda kokkulangevus?
DocumentDB toetab loodetav kokkulangevus juhtelemendi (OCC) HTTP üksuse sildid või etags kaudu. Iga DocumentDB ressursi on mõni etag ja selle etag on seatud serveris iga kord, kui dokumenti on värskendatud. Etag päise- ja praegune väärtus on kaasatud vastuse kõigile sõnumitele. Etags saab kasutada If-Match päisega serverit otsustada, kui ressursi tuleks värskendada. If-Match väärtus etag-väärtus, et kontrollida. Kui etag väärtus vastab serveri etag väärtus, värskendatakse selle ressursi. Kui soovitud etag on aegunud, server hülgab koostöös on "HTTP 412 eelduseks tõrge" vastuse koodi. Seejärel peab klient refetch ressursi omandada ressursi etag praegust väärtust. Lisaks saab kasutada If-pole-Match päisega etags, määratlemaks, kas ressursi uuesti toomiseks on vaja. 

Loodetav kokkulangevus .net-i kasutada, kasutage [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) klassi. .Net-i valimi, vt [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) github valimis DocumentManagement.

### <a name="how-do-i-perform-transactions-in-documentdb"></a>Kuidas toimingute tegemise DocumentDB
DocumentDB toetab keele integreeritud tehingud JavaScripti salvestatud toimingute ja käivitab kaudu. Kõik andmebaasi toimingud sees skriptide täidetakse hetktõmmise eraldamise veebirakendust kogumi, kui see on ühe-sektsiooni või dokumente sama sektsiooni võtmeväärtuse kogumi, kui selle saidikogumi on liigendatud punkti. Dokumendiversioonide (ETags) hetktõmmise tehtud tehingu alguses ja kinnitatud ainult siis, kui skripti õnnestus. Kui JavaScripti põhjustab viga, tehing on tagasi pöörata. Üksikasjalikumat teavet teemast [DocumentDB serveripoolne programmeerimisega](documentdb-programming.md) .

### <a name="how-can-i-bulk-insert-documents-into-documentdb"></a>Kuidas saab sisse DocumentDB lisa dokumendid hulgiredigeerimiseks? 
On kolm võimalust üheks DocumentDB dokumentide lisa hulgi.

- Andmete Migreerimistööriista, nagu on kirjeldatud [DocumentDB impordi andmed](documentdb-import-data.md).
- Dokumendi Explorer Azure'i portaalis, nagu on kirjeldatud [Lisa hulgi dokumentide dokumendi Exploreriga](documentdb-view-json-document-explorer.md#BulkAdd).
- Salvestatud toimingute, nagu on kirjeldatud [DocumentDB serveripoolne programmeerimisega](documentdb-programming.md).

### <a name="does-documentdb-support-resource-link-caching"></a>Kas DocumentDB toetab ressursside link vahemällu?
Jah, kuna DocumentDB on rahulik teenus, ressursside lingid on püsiv ja saab kopeerida. DocumentDB kliendid saate määrata mõne "If-pole-Match" päise loeb dokumendi või saidikogumi ja Värskenda oma kohaliku kopeeritakse ainult siis, kui serveri versioon on muuta nagu mis tahes ressursside suhtes. 

### <a name="is-a-local-instance-of-documentdb-available"></a>Kohaliku eksemplari DocumentDB on saadaval?
Praegu kohaliku eksemplari DocumentDB pole saadaval. Saate jälgida olekut kohaliku emulaator ja hääletada selle [tagasiside Foorum](https://feedback.azure.com/forums/263030-documentdb/suggestions/6328798-standalone-local-instance).


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 
