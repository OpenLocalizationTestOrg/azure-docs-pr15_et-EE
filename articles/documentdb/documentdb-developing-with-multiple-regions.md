<properties
   pageTitle="Mitme regioonide DocumentDB koos arendamise | Microsoft Azure'i"
   description="Saate teada, kuidas pääseda Azure'i DocumentDB täielikult hallatud NoSQL andmebaasi teenuse andmete mitme piirkonna."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="kipandya"/>
   
# <a name="developing-with-multi-region-documentdb-accounts"></a>Mitme piirkonna DocumentDB kontode arendamise

> [AZURE.NOTE] Globaalse jaotuse andmebaase DocumentDB on üldiselt kättesaadav ja mis tahes vastloodud DocumentDB kontode jaoks automaatselt lubatud. Töötame kõik olemasolevad kontodega globaalne jagamise lubamiseks, kuid vahepeal kui soovite, et teie kontol lubatud levitamist palun [pöörduge klienditoe poole](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ja me tuleb lubada teile nüüd.

Selleks, et ära [globaalse](documentdb-distribute-data-globally.md)jaotuse klientrakendustes saate määrata järjestatud eelistused loendis piirkondade kasutatava dokumendi toimingute tegemiseks. Seda saab teha, seades ühenduse poliitika. Azure'i DocumentDB konto konfigureerimine, piirkondlike kättesaadavus ja eelistused loendis määratud põhineva, valitakse kõige optimaalse lõpp-punkti sooritamiseks kirjutada ja lugeda toimingute SDK. 

Selles eelistused loendis on määratud käivitamisel ühenduse DocumentDB kliendi SDK-d. Funktsiooni SDK-d aktsepteerida valikuline parameeter "PreferredLocations" mis on Azure piirkondade järjestatud loendi.

SDK saadab automaatselt kõik kirjutab praeguse kirjutamine piirkond. 

Kõik loeb saadetakse PreferredLocations loendi esimese saadaval alale. Taotluse nurjumisel kliendi kuvatakse järgmine redigeeritav ala loendis allapoole nurjuda jne. 

Kliendi SDK-d proovib ainult lugeda piirkondade määratletud PreferredLocations. Näiteks kui andmebaasi kontot ei kolme piirkondades saadaval, kuid kliendi ainult määratledes kahe-kirjutamine piirkondade PreferredLocations, siis pole loeb pakutakse kirjutamine piirkonnast, isegi kui tegemist on Tõrkesiirde.

Rakenduse saate praeguse kirjutamine lõpp-punkti kinnitamine ja lõpp-punkti valitud SDK, märkides ruudu kaks atribuudid WriteEndpoint ja ReadEndpoint saadaval SDK versioon 1.8 ja üle lugeda. 

Kui PreferredLocations atribuut on seatud ei, kõik kutsed kella kirjutamine praeguse ala. 


## <a name="net-sdk"></a>.NET SDK
SDK saab koodi muutmata. Sel juhul SDK automaatselt suunab nii loeb ja kirjutab kirjutamine praeguse ala. 

Versioonis 1.8 ja hiljem .NET SDK DocumentClient ehitaja ConnectionPolicy parameeter on Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations atribuudi. Atribuut on tippige saidikogumi `<string>` ja peaks sisaldama piirkond nimede loend. String väärtused on vormindatud regiooni nimi veeru [Azure'i piirkondade] kohta [ regions] lehel ilma tühikuteta enne või pärast esimese ja viimase märgi vastavalt.

Praeguse kirjutamine ja lugege lõpp-punktid on saadaval DocumentClient.WriteEndpoint ja DocumentClient.ReadEndpoint vastavalt.

> [AZURE.NOTE] Vanade konstantide peaks ei loeta URL-ide lõpp-punktid. Teenuse võivad need igal ajal värskendada. SDK tegeleb selle muudatuse automaatselt.

    // Getting endpoints from application settings or other configuration location
    Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
    string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

    //Setting read region selection preference 
    connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
    connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
    connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

    // initialize connection
    DocumentClient docClient = new DocumentClient(
        accountEndPoint,
        accountKey,
        connectionPolicy);

    // connect to DocDB 
    await docClient.OpenAsync().ConfigureAwait(false);


## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript ja Python SDK-d
SDK saab koodi muutmata. Sel juhul SDK automaatselt otse nii loeb ja kirjutab praeguse kirjutamine piirkond. 

Versioonis 1.8 ja uuemad versioonid iga SDK ConnectionPolicy parameeter DocumentClient ehitaja uue atribuudi nimetatakse DocumentClient.ConnectionPolicy.PreferredLocations. See parameeter on stringide massiiv, mis viib piirkond nimede loend. Nimede vormindatud regiooni nimi veeru [Azure'i piirkondade] kohta [ regions] lehe. Samuti saate kasutada eelmääratletud konstantide mugavuse objekti AzureDocuments.Regions

Praeguse kirjutamine ja lugege lõpp-punktid on saadaval DocumentClient.getWriteEndpoint ja DocumentClient.getReadEndpoint vastavalt.

> [AZURE.NOTE] Vanade konstantide peaks ei loeta URL-ide lõpp-punktid. Teenuse võivad need igal ajal värskendada. SDK toime selle muudatuse automaatselt.

Allpool on NodeJS/JavaScripti koodi näide. Python ja Java jälgib sama muster.

    // Creating a ConnectionPolicy object
    var connectionPolicy = new DocumentBase.ConnectionPolicy();
    
    // Setting read region selection preference, in the following order -
    // 1 - West US
    // 2 - East US
    // 3 - North Europe
    connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];
    
    // initialize the connection
    var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);


## <a name="rest"></a>ÜLEJÄÄNUD 
Pärast andmebaasi konto on tehtud mitme piirkondades saadaval, kliendid päringu selle kättesaadavus täites GET-päringu kohta järgmised URI.

    https://{databaseaccount}.documents.azure.com/

Teenus tagastab piirkondade ja nende vastavate DocumentDB lõpp-punkti URI-d on koopiate loendi. Praeguse kirjutamine ala on toodud vastus. Kliendi saate valida sobiv lõpp-punkti jaoks Kõikide täiendavate REST API taotlusi järgmiselt.

Näide vastus

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


-   Kõik panna, postitus ja Kustuta taotlused peavad minge URI näidatud kirjutamine
-   Kliendi poolt valitud mis tahes lõpp-punkti minna kõik saab ja muud kirjutuskaitstud taotlusi (nt päringud)

Kirjutage kirjutuskaitstud piirkondadele nurjuvad HTTP tõrkekood 403 ("keelatud").

Kui kirjutamine piirkond muutub pärast kliendi algne discovery etapp, nurjub järgmise kirjutab kirjutamine eelmisele alale HTTP tõrkekood 403 ("keelatud"). Kliendi peaksin siis jälle, et saada värskendatud kirjutamine piirkond piirkondade loendiga.

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet jaotavad andmete globaalselt DocumentDB järgmistest artiklitest:

- [Andmete globaalselt DocumentDB levitamine](documentdb-distribute-data-globally.md)
- [Vormindusühtsuse tasemed](documentdb-consistency-levels.md)
- [Mitme piirkondade läbilaskevõime tööpõhimõtted](documentdb-manage.md#how-throughput-works-with-multiple-regions)
- [Azure'i portaalis piirkondade lisamine](documentdb-portal-global-replication.md)

[regions]: https://azure.microsoft.com/regions/ 
