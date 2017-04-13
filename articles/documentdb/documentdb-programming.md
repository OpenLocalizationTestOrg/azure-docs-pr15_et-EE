<properties 
    pageTitle="DocumentDB kavandamine: Salvestatud toimingute ja andmebaasi päästikute UDF-ID | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada DocumentDB kirjutada JavaScript salvestatud toimingute, andmebaasi päästikute ja kasutaja määratletud funktsioonid (UDF). Saada andmebaasi programing näpunäiteid ja palju muud." 
    keywords="Päästikute, salvestatud protseduur, salvestatud protseduur, andmebaasiprogrammi, sproc, documentdb, Azure'i, Microsoft Azure'i andmebaas"
    services="documentdb" 
    documentationCenter="" 
    authors="aliuy" 
    manager="jhubbard" 
    editor="mimig"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="andrl"/>

# <a name="documentdb-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>DocumentDB serveripoolne kavandamine: Salvestatud toimingute ja andmebaasi päästikute UDF-ID

Siit saate teada, kuidas Azure'i DocumentDB keele integreeritud, selgituseks täitmise JavaScripti võimaldab arendajatel kirjutada **Salvestatud toimingute** **päästikute** ja **kasutaja määratletud funktsioonid (UDF)** algupäraselt JavaScripti. See võimaldab teil andmebaasi programmi rakenduse loogika, mida saab saata ja täide otse andmebaasi salvestusruumi sektsioonid kirjutamine 

Soovitame alustamine, jälgides järgmisest videost, kus Andrew Liu on lühiülevaade DocumentDB's serveripoolne andmebaasi programmeerimise mudel. 

> [AZURE.VIDEO azure-demo-a-quick-intro-to-azure-documentdbs-server-side-javascript]

Seejärel tagastavad selle artikli kui õpite vastused järgmistele küsimustele.  

- Kuidas kirjutada on salvestatud protseduur, päästik või UDF-i, JavaScripti?
- Kuidas DocumentDB tagada HAPPE?
- Kuidas toimivad tehingud DocumentDB?
- Mis on käivitab enne ja pärast käivitab ja kuidas kirjutada üks?
- Kuidas registreerida ja käivitada salvestatud protseduur, päästik või UDF rahulik viisil HTTP kaudu?
- DocumentDB SDK-d on saadaval, luua ja käivitada salvestatud toimingute ja käivitab UDF-ID?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Salvestatud protseduur ja UDF kavandamine, Sissejuhatus

Seda moodust *"* JavaScripti tänapäevane päeva T-SQL-i" vabastab rakenduste arendajatele keerulisi tüüp süsteemi vastuolusid ja objekti relatsiooniline vastenduse tehnoloogiate kaudu. See on ka arvu sisemine eeliseid, mida saab kasutada, et luua rikkalikke rakendusi.  

-   **Üksikasjalik loogika:** JavaScripti programmeerimiskeele, kõrge nimega pakub rikkaliku ja tuttava kasutajaliidese väljendada äriloogika. Keerukate järjestus toimingute lähemale andmetele, saate seda teha.

-   **Aatomi tehingud:** DocumentDB tagab, et andmebaasi toiminguid sees ühe salvestatud toimingu või päästik on aatomi. See võimaldab rakenduse ühendamine seotud toiminguid ühe paketina, et need kõik õnnestub või ükski neist ei õnnestu. 

-   **Jõudluse:** Asjaolu, et JSON vastendatakse otseselt süsteemile JavaScripti keele tüüp on ka DocumentDB talletusruumi põhiühik võimaldab Optimeerimised nagu rongile realiseerumisele JSON dokumentide puhvri pargi ja muutes need kättesaadavaks tellitavate täitmiskood arvu. On veel jõudluse kasu saatmise äriloogika andmebaasi.
    -   Partiide-arendajad saate toiminguid nagu lisab rühmitada ja esitage need mitmekaupa. Võrgu liikluse latentsus maksumus ja selle poe elemente, et luua eraldi tehingud on oluliselt vähendada. 
    -   Enne koostamine-DocumentDB precompiles salvestatud toimingute, päästikute ja kasutaja määratletud funktsioonid (UDF) vältimiseks JavaScripti koostamise maksumus iga kutsumise jaoks. Selle pea kohal koostamise bait kood üksikasjalik loogika on vastate minimaalse väärtuse.
    -   Järjestuse – paljude toimingute vajate side-mõju ("päästik"), mis hõlmab potentsiaalselt ühe või mitu Store'i teisene toiminguid teha. Atomicity, peale seda on veel kiire, kui server. 
-   **Kapseldus:** Salvestatud toimingute saab rühmitamiseks äriloogika ühes kohas. See on kaks eelised:
    -   See lisatakse mõne võtmiseks kiht peal lähteandmed, mis võimaldab andmete arhitektid töötada nende rakenduste andmetest sõltumatult. See on eriti kasulik, kui andmed on skeemi vähem rabe oletused, mis võib-olla küpsetatud rakendusse, kui neil on andmed otse tegelema tõttu.  
    -   See võtmiseks võimaldab suurettevõtete nende andmete turvaliseks täiustades skriptide kaudu juurdepääs.  

Loomine ja täitmise andmebaasi päästikute, salvestatud protseduur ja kohandatud päringu tehtemärke on toetatud [REST API -ga](https://msdn.microsoft.com/library/azure/dn781481.aspx), [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)ja .net-i, Node.js ja JavaScripti paljude platvormide [Kliendi SDK-d](documentdb-sdk-dotnet.md) .

**Selle õpetuse kasutab [Node.js SDK K lubadusi](http://azure.github.io/azure-documentdb-node-q/) ** illustreerimiseks valemisüntaksit ja kasutamist salvestatud toimingute ja käivitab UDF-ID.   

## <a name="stored-procedures"></a>Salvestatud toimingute

### <a name="example-write-a-simple-stored-procedure"></a>Näide: Kirjutamine lihtsa salvestatud protseduur 
Alustame lihtsa salvestatud toiming, mis tagastab "Tere, maailm" vastuse.

    var helloWorldStoredProc = {
        id: "helloWorld",
        body: function () {
            var context = getContext();
            var response = context.getResponse();
    
            response.setBody("Hello, World");
        }
    }


Salvestatud toimingute kohta saidikogumi registreeritud, ja mis tahes dokumendi või manuse kohal selle saidikogumi töötavad. Järgmised koodilõigu näitab, kuidas registreerida helloWorld salvestatud protseduur kogumi. 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Kui salvestatud protseduur on registreeritud, saame käivitada selle kogumise vastu ja tulemuste tagasi kliendi lugeda. 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


Konteksti objekti pakub juurdepääsu kõik toimingud, mida saab teha DocumentDB salvestusruumi ning koosolekukutsete ja kutsele vastamise objektide juurdepääsu. Sel juhul kasutatakse vastuse objekti seadmiseks keha vastust, mis saadeti kliendile. Rohkem üksikasju, vaadake [DocumentDB JavaScripti serveri SDK dokumentatsiooni](http://azure.github.io/azure-documentdb-js-server/).  

Andke meile selle näite laiendada ja lisada rohkem andmebaasi salvestatud protseduur seotud funktsioone. Salvestatud toimingute saate luua, värskendada, lugemine, päringu ja dokumendid ja manused sees selle saidikogumi kustutamine.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Näide: Kirjutama salvestatud dokumendi loomine 
Järgmise koodilõigu näitab, kuidas kasutada kontekstis objekti suhelda DocumentDB ressursid.

    var createDocumentStoredProc = {
        id: "createMyDocument",
        body: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


Salvestatud toiming võtab Sisestuskeel documentToCreate praeguse saidikogumi luua dokumendi kehas. Kõigi selliste toimingute on asünkroonne ja JavaScripti funktsioon kontekstiatribuuti sõltuvad. Funktsiooni tagasihelistamise on kaks parameetrit, üks tõrge objekti toiming nurjub juhul ja üks loodud objekti. Sees tagasihelistamise, kasutajad saate lahendanud erandit või põhjustada tõrke. Juhul, kui ei esitata pakkumist ja on tõrge, põhjustab DocumentDB käitusaja tõrge.   

Ülaltoodud näites on tagasihelistamise põhjustab viga, kui see toiming nurjus. Muul juhul määrab loodud dokumendi ID-d kliendi vastuse kehana. Siin on, kuidas see salvestatud protseduur käivitatakse sisendparameetrid.

    // register the stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
    
            // run stored procedure to create a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "The Hitchhiker’s Guide to the Galaxy",
                author: "Douglas Adams"
            };
    
            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });

    
Pange tähele, et see salvestatud protseduur muuta massiivi dokumendi asutuste sisendina ja luua need kõik sama salvestatud protseduur täitmise asemel mitme võrgu taotlusi luua iga ükshaaval. See saab rakendada tõhusa hulgi importija DocumentDB (arutatakse hiljem selles õpetuses).   

Käesolevas näites kirjeldatud näidanud salvestatud toimingute kasutamise kohta. Oleme allpool olevat õpetuse hõlmab päästikute ja kasutaja määratletud funktsioonid (UDF).

## <a name="database-program-transactions"></a>Programmi andmebaasitoiminguid
Tehingu tüüpilised andmebaasi defineeritakse kui üks loogiline üksuse töö toimingute jada. Iga tehingu pakub **HAPPE tagab**. HAPE on tuntud lühend, mis tähistab nelja atribuudid - Atomicity, järjepidevuse, eraldamise ja kestvus.  

Lühidalt öeldes atomicity tagab, et kõik tööd teha sees tehingu käsitletakse ühe üksuse kui kas kõik on kinnitatud või mitte keegi. Vormindusühtsuse kontrollib, kas andmed on alati hea sisemine olekus tehingud üle. Eraldamise tagab, et kahe tehinguid varjutaks üksteisele-üldiselt, kõige trükikojas süsteemidest eraldamise mitu taset, mida saab kasutada vastavalt vajadustele rakenduse. Kestvus tagab iga muudatust, mis on kinnitatud andmebaasi alati on Esita.   

DocumentDB, JavaScripti on majutatud sama mäluruumi andmebaas nimega. Seega taotlused jooksul salvestatud toimingute ja käivitab käivitada andmebaasi seansi ulatust. See võimaldab DocumentDB tagada HAPPE kõik toimingud, mis on osa ühe salvestatud protseduur/Käivita. Võtke arvesse järgmist salvestatud protseduur määratlus.

    // JavaScript source code
    var exchangeItemsSproc = {
        name: "exchangeItems",
        body: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            var player1Document, player2Document;
    
            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);
    
                    if (documents.length != 1) throw "Unable to find both names";
                    player1Document = documents[0];
    
                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable to find both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable to read player details, abort ";
                });
    
            if (!accept) throw "Unable to read player details, abort ";
    
            // swap the two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;
    
                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable to update player 1, abort ";
    
                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable to update player 2, abort"
                            });
    
                        if (!accept2) throw "Unable to update player 2, abort";
                    });
    
                if (!accept) throw "Unable to update player 1, abort";
            }
        }
    }
    
    // register the stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

See salvestatud protseduur kasutab tehingud mängimine rakenduses trade üksuste vahel kaks ühekordne. Salvestatud protseduur avaldab iga vastav Playeri ID möödunud argumendina kaks dokumentide lugemine. Kui mõlemad dokumendid mängija ei leita, siis salvestatud protseduur värskendab dokumendid, nende üksuste vahetamine. Kui teel on tekkinud vigu, põhjustab JavaScript erand, et käsk peidetult tehingu.

Kui saidikogumi salvestatud protseduur on registreeritud suhtes, mis on ühe-sektsiooni kogum, siis tehingu on rakendatud kõik docuemnts, kogumis. Kui selle saidikogumi on liigendatud, siis salvestatud toimingute täidetakse tehingu ulatus ühe sektsiooni võti. Iga salvestatud protseduur täitmise siis hulka partition võtmeväärtuse vastav tehingu tuleb käitada jaotises ulatus. Lisateavet leiate teemast [DocumentDB eraldamine](documentdb-partition-data.md).

### <a name="commit-and-rollback"></a>Kinnita ja tagasipööramine
Tehingud sügavalt ja algupäraselt integreeritakse DocumentDB's JavaScript programmeerimise mudel. Funktsiooni JavaScripti sees kõik toimingud automaatselt lehviga jaotises ühe toimingu. Kui JavaScripti ilma erandite on lõpule jõudnud, andmebaasi toimingud on kinnitatud. "ALUSTAGE tehing" ja "Kinnita tehing" laused relatsioonandmebaasidest on kaudselt DocumentDB.  
 
Kui mõni erand, mis on levitatud skripti, DocumentDB's JavaScripti käitusaja pöörab tagasi kogu tehingu. Nagu varem näites, korrutamine erandi võrdub tõhus DocumentDB "TAGASIPÖÖRAMINE tehingu".
 
### <a name="data-consistency"></a>Andmete järjepidevus
Salvestatud toimingute ja käivitab on alati täita DocumentDB saidikogumi esmane koopia. See tagab, et loeb sees salvestatud toimingute pakkumise tugev järjepidevuse. Päringute kasutaja määratletud funktsioonide abil saab teostada esmast või mõnda teise koopia, kuid me tagada vastavad nõutud järjepidevuse taset, valides sobiv koopia.

## <a name="bounded-execution"></a>Piiratud täitmise
Kõik DocumentDB toimingud peate täitma jooksul määratud server taotleda ajalõpu kestus. See piirang kehtib ka JavaScripti funktsioone (salvestatud toimingute, päästikute ja kasutaja määratletud funktsioonid). Kui toiming ei viida tähtaja, tehing on tagasi pöörata. JavaScripti funktsioone peab lõpule aja jooksul või rakendada jätku vastavalt mudeli paketi/jätkata täitmise.  

Salvestatud toimingute ja käivitab ajalimiite käsitlema lihtsustamiseks tagastada kõik funktsioonid jaotises Saidikogumi objekti (loomine, lugemine, asendamine ja Kustuta dokumendid ja manused) jaoks loogikaväärtus, mis tähistab seda, kas see toiming on lõpule. Kui selle väärtus on false, on märge, et tähtaja on aegumas ja, et täitmise protseduur peab kokkuvõte.  Kui salvestatud protseduur lõpetab kellaaja ja järjekorda tulemust, mis tahes lõpuleviimiseks on tagatud toimingud järjekorras enne esimest kasutajalitsentsi poe toimingut.  

JavaScripti funktsioonid on ka piiratud ressursside tarbimine. DocumentDB jätab ühe saidikogumi ettevalmistatud suurusest andmebaasi konto. Läbilaskevõime on väljendatud CPU, mälu ja IO tarbimine nimetatakse taotluse üksused või RUs normaliseeritud üksus. JavaScripti funktsioone saab kasutada suure hulga RUs kiiresti üles ja võidakse kuvada määr piiratud, kui selle saidikogumi piirangu. Ressursi intensiivse salvestatud toimingute võib karantiini ka andmebaasi lihtsad toimingud kättesaadavuse tagamiseks.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Näide: Hulgiredigeerimiseks andmete importimise andmebaasiprogrammi vahel.
Allpool on kujutatud salvestatud protseduur, mis on kirjutatud hulgi-importimine dokumentide kogumi. Pange tähele, kuidas käsitleb salvestatud protseduur piiratud täitmise, märkides ruudu on kahendväärtus tagastada väärtuse createDocument, ja kasutab jälgida ja jätkake edenemise üle pakettidena lisatakse iga kutsumise salvestatud protseduur dokumentide arv.

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();
    
        // The count of imported docs, also used as current doc index.
        var count = 0;
    
        // Validate input.
        if (!docs) throw new Error("The array is undefined or null.");
    
        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }
    
        // Call the create API to create a document.
        tryCreate(docs[count], callback);
    
        // Note that there are 2 exit conditions:
        // 1) The createDocument request was not accepted. 
        //    In this case the callback will not be called, we just call setBody and we are done.
        // 2) The callback was called docs.length times.
        //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);
    
            // If the request was accepted, callback will be called.
            // Otherwise report current count back to the client, 
            // which will call the script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }
    
        // This is called when collection.createDocument is done in order to process the result.
        function callback(err, doc, options) {
            if (err) throw err;
    
            // One more document has been inserted, increment the count.
            count++;
    
            if (count >= docsLength) {
                // If we created all documents, we are done. Just set the response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a id="trigger"></a>Andmebaasi päästikute
### <a name="database-pre-triggers"></a>Andmebaasi eelse päästikute
DocumentDB pakub käivitab, mis on täide või käivitab toimingu dokumendiga. Näiteks saate määrata eelse päästik, kui loote dokumendi – see eelse päästik käivitub enne seda, kui dokument on loodud. Järgmine on näide sellest, kuidas eelse päästikute saab kinnitada, et on loodud dokumendi atribuudid.

    var validateDocumentContentsTrigger = {
        name: "validateDocumentContents",
        body: function validate() {
            var context = getContext();
            var request = context.getRequest();
    
            // document to be created in the current operation
            var documentToCreate = request.getBody();
    
            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }
    
            // update the document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


Ja vastavate Node.js kliendipoolne registreerimise koodi käivitada.

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };
    
            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };
    
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


Enne päästikute ei tohi olla sisendparameetrid. Taotluse objekti saab töödelda seostatud toimingu taotluse sõnum. Siin eelse päästik käitatakse dokumendi loomise ja taotluse sõnumi keha sisaldab dokumenti, luua JSON-vormingus.   

Päästikute registreerimisel kasutajad saate määrata toimingud, mida võite alustada. See käivitab loodi TriggerOperation.Create, mis tähendab, et järgmist ei ole lubatud.

    var options = { preTriggerInclude: "validateDocumentContents" };
    
    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });
    
    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Pärast andmebaasi päästikute
Pärast päästikute, eelse päästikute, nagu on seostatud toimingu dokumendiga ja ei võta sisendparameetrid. Need **pärast** selle toimingu lõpuleviimist käivitage ja vastuse sõnum, mis saadetakse kliendi juurde.   

Järgmises näites kuvatakse pärast päästikute toiming:

    var updateMetadataTrigger = {
        name: "updateMetadata",
        body: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            // document that was created
            var createdDocument = response.getBody();
    
            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable to update metadata, abort";
     
            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable to find metadata document';
                         
                         var metadataDocument = documents[0];
                         
                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable to update metadata, abort";
                               });
                         if(!accept) throw "Unable to update metadata, abort";
                         return;                    
            }                                                                                           
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


Käivitab saab registreerida, nagu on näidatud järgmises näites.

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "The Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };
    
            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };
        
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


See käivitab metaandmete dokumendi päringut ja värskendab see äsja loodud dokumendi üksikasjad.  

Asi, mis on oluline märkida on **selgituseks** täitmise päästikute DocumentDB sisse. See pärast päästik käivitatakse osana sama tehingu algse dokumendi loomine. Seetõttu, kui me põhjustada erandi pärast päästik (ütlevad, kui me ei saa värskendada metaandmete dokument) kaudu, kogu tehingu ei õnnestu ja tagasi pöörata. Dokument luuakse ja erandi tagastatakse.  

##<a id="udf"></a>Kasutaja määratletud funktsioonid
Kasutaja määratletud funktsioonid (UDF) kasutatakse laiendada keele grammatika DocumentDB SQL-i päringu ja rakendada kohandatud äriloogika. Ta saab ainult helistada päringute sees. Neid ei saa kasutada kontekstis objektile ja on mõeldud kasutamiseks ainult Arvuta JavaScripti. Seetõttu saab kasutada UDF-ID DocumentDB teenuse teisene koopiad.  
 
Järgmises näites UDF, arvutada tulumaksu aluseks jaoks eri tulu sulgudes loob ja kasutab seda sees päringu leidmiseks kõik inimesed, kes makstav maksud rohkem kui 20 000 €.

    var taxUdf = {
        name: "tax",
        body: function tax(income) {
    
            if(income == undefined) 
                throw 'no input';
    
            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


Seejärel saate kasutada UDF päringute nagu järgmises näites:

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);
    
            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>JavaScripti keel integreeritud päringu API-ga
Lisaks väljaandja päringute abil DocumentDB's SQL-i grammatika, serveripoolne SDK abil saate teostada optimeeritud päringute Fluenti JavaScripti kasutajaliidese ilma teadmisi SQL-i abil. Päringu JavaScripti API võimaldab teil koostada programmiliselt päringute, läbides põhiliste funktsioonide chainable funktsioon sisse helistab süntaks, mis on tuttav ECMAScript5 on massiiv parketilipid ja populaarsed JavaScripti teekide nagu lodash. Päringute on sõeluda JavaScripti käitusaja käivitatakse DocumentDB's indeksite abil.

> [AZURE.NOTE]`__` (kahekordne allkriips) on pseudonüümi `getContext().getCollection()`.
> <br/>
> Teisisõnu, saate kasutada `__` või `getContext().getCollection()` juurde pääseda päringu JavaScripti API-ga.

Toetatud on järgmised.
<ul>
<li>
<b>Chain() …. väärtuse ([tagasihelistamise] [, suvandid])</b>
<ul>
<li>
Algab value() aheldatud telefonil, mis tuleb lõpetada.
</li>
</ul>
</li>
<li>
<b>filter (predicateFunction [, suvandid] [, tagasihelistamise])</b>
<ul>
<li>
Filtreerib sisend põhiliste funktsiooni, mis tagastab tõene/väär, et filtreeritakse ja väljaregistreerimise Sisestuskeel dokumentide tulemuseks määramine. See toimib sarnaselt WHERE-klausli SQL-i.
</li>
</ul>
</li>
<li>
<b>kaardi (transformationFunction [, suvandid] [, tagasihelistamise])</b>
<ul>
<li>
Rakendab projektsiooni, mis on antud teisendus funktsioon, mis kaartide Sisestuskeel tooteartiklite JavaScripti objekti või väärtuse. See toimib sarnaselt SELECT-klauslis SQL-i.
</li>
</ul>
</li>
<li>
<b>sikutama ([propertyName] [, suvandid] [, tagasihelistamise])</b>
<ul>
<li>
See on otsetee kaart, mis ekstraktib Sisestuskeel tooteartiklite ühe atribuudi väärtust.
</li>
</ul>
</li>
<li>
<b>Ühenda ([isShallow] [, suvandid] [, tagasihelistamise])</b>
<ul>
<li>
Ühendab ja lömastab massiivid ühe massiivi iga Sisestuskeel üksuse kaudu. See toimib sarnaselt SelectMany LINQ.
</li>
</ul>
</li>
<li>
<b>sortBy ([predikaat] [, suvandid] [, tagasihelistamise])</b>
<ul>
<li>
Uus komplekt dokumentide aedvili järgi sortida tõusvas järjestuses, kasutades antud predikaat Sisestuskeel dokumendi voo dokumentidele. See toimib sarnaselt klausel ORDER BY, milles SQL-i.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([predikaat] [, suvandid] [, tagasihelistamise])</b>
<ul>
<li>
Uus komplekt dokumentide aedvili järgi sortida laskuvas järjestuses, kasutades antud predikaat voos Sisestuskeel dokumendi dokumendid. See toimib sarnaselt SQL klausel ORDER BY x DESC.
</li>
</ul>
</li>
</ul>


Kui predikaat ja/või Vaateselektori funktsioonide sees, järgmised JavaScripti importida saada automaatselt optimeeritud käitamist DocumentDB indeksite:

* Lihtne tehtemärgid: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Literaalide, sh literaalstringide objekti: {}
* var saatja

Järgmised JavaScripti importida saada optimeeritud DocumentDB indeksite:

* Kontrollivad (nt if, samal ajal)
* Funktsiooni kõned

Lisateabe saamiseks lugege meie [Serveripoolne JSDocs](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Näide: Kirjutama salvestatud päringu JavaScripti API kasutamine

Järgmine kood näidis on kujutatud, kuidas päringu JavaScripti API saab kasutada salvestatud protseduur kontekstis. Salvestatud protseduur lisab dokumendi, on parameetriga antud ja värskendused on metaandmete dokumendi, kasutades funktsiooni `__.filter()` meetod minSize, maxSize ja totalSize põhineb Sisestuskeel dokumendi atribuudi suurus.

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent to our callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

            // The metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all the rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace the metadata document in the store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
              //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
              //       We have to take care of that on the client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-to-javascript-cheat-sheet"></a>SQL-i, JavaScripti täieliku lehele
Järgnevas tabelis on toodud erinevad SQL-i ja vastavate JavaScripti päringute.

Kui SQL-päringud, kus dokument atribuudi klahvid (nt `doc.id`) on tõstutundlikud.

<br/>
<table border="1" width="100%">
<colgroup>
<col span="1" style="width: 40%;">
<col span="1" style="width: 40%;">
<col span="1" style="width: 20%;">
</colgroup>
<tbody>
<tr>
<th>SQL-I</th>
<th>Päringu JavaScripti API-ga</th>
<th>Üksikasjad</th>
</tr>
<tr>
<td>
<pre>
Valige *: docs
</pre>
</td>
<td>
<pre>
{saatja dokumendi;} __.map(function(doc));
</pre>
</td>
<td>Tulemuste kõik dokumendid (nummerdatud koos jätkamiseks luba), nagu on.</td>
</tr>
<tr>
<td>
<pre>
Valige docs.id, docs.message nimega sõnum, docs.actions saatja dokumendid
</pre>
</td>
<td>
<pre>
__.map(function(doc) {tagastada {id: doc.id, sõnum: doc.message, toimingud: doc.actions};});
</pre>
</td>
<td>Projektide id, sõnumi (määrata pseudonüümid sõnum) ja toiming: kõik dokumendid.</td>
</tr>
<tr>
<td>
<pre>
Valige * dokumendid, kus docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.filter(function(doc) {tagastada doc.id === "X998_Y998;"});
</pre>
</td>
<td>Dokumendid, millel on predikaat päringuid: id = "X998_Y998".</td>
</tr>
<tr>
<td>
<pre>
Valige *: docs kus ARRAY_CONTAINS (dokumendid. Sildid, 123)
</pre>
</td>
<td>
<pre>
__.filter(function(x) {tagastada x.Tags & & x.Tags.indexOf(123) >-; 1});
</pre>
</td>
<td>Päringuid dokumente, mis on atribuut sildid ja sildid on massiiv, mis sisaldab väärtust 123.</td>
</tr>
<tr>
<td>
<pre>
Valige docs.id, docs.message nimega sõnum saatja dokumentide WHERE docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.Chain() .filter(function(doc) {tagastada doc.id === "X998_Y998;"}) .map(function(doc) {tagastada {id: doc.id, sõnum: doc.message};}) .value();
</pre>
</td>
<td>Dokumentide korral päringuid id = "X998_Y998" ning seejärel projekte ID-d ja sõnumi (määrata pseudonüümid sõnum).</td>
</tr>
<tr>
<td>
<pre>
Valige väärtus sildi saatja dokumentide Liitu sildistamine dokumentides. Siltide ORDER BY docs._ts
</pre>
</td>
<td>
<pre>
__.Chain() .filter(function(doc) {saatja dokumenti. Siltide & & Array.isArray (doc. Sildid); }) .sortBy(function(doc) {saatja doc._ts;}) .pluck("tags") .flatten() .value()
</pre>
</td>
<td>Dokumendid, mis on massiiv atribuudi, sildid, filtrid ja sordib tulemuseks dokumentide _ts ajatempli süsteemi atribuut ja seejärel projektide + lömastab siltide massiiv.</td>
</tr>
</tbody>
</table>

## <a name="runtime-support"></a>Käitusaja tugi
[DocumentDB JavaScripti serveripoolne SDK](http://azure.github.io/azure-documentdb-js-server/) toetab kõige mainstream JavaScripti keele funktsioone nagu standardne [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)järgi.

### <a name="security"></a>Turvalisus
JavaScripti salvestatud toimingute ja käivitab on liivakastikoodi, nii et ühe skripti mõju lekkima teise ilma hetktõmmise tehingu eraldamise andmebaasi tasemel. Käitusaja keskkonnas on ühendatud, kuid pärast iga katse konteksti puhastada. Seega nad on tagatud ohutu, igal pool avaldavad üksteisest.

### <a name="pre-compilation"></a>Enne koostamine
Salvestatud toimingute, päästikute ja UDF-ID on peidetult precompiled bait kood vormingusse koostamise kulude vältimiseks iga skripti kutsumise ajal. See tagab manamine, Salvestatud toimingute on kiire ja on väike jalajälg.

## <a name="client-sdk-support"></a>SDK klienditugi
Lisaks [Node.js](documentdb-sdk-node.md) klient, toetab DocumentDB [.NET](documentdb-sdk-dotnet.md), [Java](documentdb-sdk-java.md), [JavaScript](http://azure.github.io/azure-documentdb-js/)ja [Python SDK-d](documentdb-sdk-python.md). Salvestatud toimingute, päästikute ja UDF-ID saab luua ja täidetud nende SDK-d ka kasutamine. Järgmises näites kirjeldatakse, kuidas luua ja käivitada talletatud protseduuri kasutades .net-i klient. Pange tähele, kuidas .NET tüüpi salvestatud protseduur nimega JSON läks ja ettelugemist.

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
            }"
    };
    
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


See näide näitab, kuidas [.NET SDK](https://msdn.microsoft.com/library/azure/dn948556.aspx) abil saate luua eelse päästik ja luua dokumendi päästik, mis on lubatud. 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };
    
    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


Ja järgmises näites on kujutatud kasutaja määratletud funktsiooni (UDF) loomine ja kasutamine [DocumentDB SQL-päringu](documentdb-sql-query.md).

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };
    
    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>REST API-GA
Kõik DocumentDB toiminguid teha rahulik viisil. Salvestatud toimingute, päästikute ja kasutaja määratletud funktsioonid saab registreerida jaotises kogumi, kasutades HTTP POST. Järgmine on näide sellest, kuidas registreerida salvestatud protseduur:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    
    
    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


Salvestatud protseduur on registreeritud täites postituse taotluse vastu URI dbs/testdb/colls/testColl/sprocs koos sisaldav salvestatud protseduur loomiseks keha. Päästikute ja UDF-ID saab registreerida samamoodi väljastanud POSTITUSES /triggers ja /udfs vastavalt.
See salvestatud protseduur saavad seejärel teostada väljastanud postituse taotluse vastu selle ressursi link.

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT
    
    
    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Siin on salvestatud protseduur sisendi edasi koosolekukutse kehasse. Pange tähele, et sisend on möödas JSON massiivi parameetrid. Salvestatud protseduur võtab esimese sisend dokumendina, mis on vastuse keha. Saame vastus on järgmine:

    HTTP/1.1 200 OK
     
    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


Päästikute erinevalt salvestatud toimingute, ei saa otse käivitada. Selle asemel teostavad neid dokumendi toimingu käigus. Me ei saa määrata päästikute käivitamiseks taotlusega, kasutades HTTP päised. Järgmises taotluse dokumendi loomine.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger
    
    
    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


Siin on x-ms-documentdb-pre-trigger-include päises määratud eelse päästik taotluse töötada. Mis tahes järgmised päästikute esitatakse vastavalt x-ms-documentdb-post-trigger-include päis. Pange tähele, et mõlemad enne ja pärast päästikute saab määrata antud taotlus.

## <a name="sample-code"></a>Proovi kood

Meie [Github hoidla](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples)leiate veel serveripoolse koodi näiteid (sh [- kustutamiseks](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js)ja [värskendada](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)).

Kas soovite oma võimas salvestatud protseduur ühiskasutusse anda? Palun saatke meile taotlust pull! 

## <a name="next-steps"></a>Järgmised sammud

Kui teil on ühe või mitme salvestatud toimingute, päästikute ja kasutaja määratletud funktsioonid, mis on loodud, saate laadida ja vaadata neid skripti Exploreriga Azure'i portaalis. Lisateabe saamiseks lugege teemat [Vaade salvestatud toimingute, päästikute, ja kasutaja määratletud funktsioonid DocumentDB skripti Exploreriga](documentdb-view-scripts.md).

Võite leida järgmistest viiteid ja ressurssidest kasulik oma tee Lisateavet DocumentDB serveripoolne programmeerimise:

- [Azure'i DocumentDB SDK-d](https://msdn.microsoft.com/library/azure/dn781482.aspx)
- [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
- [JSON](http://www.json.org/) 
- [JavaScripti ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
- [JavaScripti – süsteemi JSON tüüp](http://www.json.org/js.html) 
- [Portable turvalise andmebaasi laiendatavus](http://dl.acm.org/citation.cfm?id=276339) 
- [Teenuse rakendusse andmebaasi arhitektuur](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
- [Microsoft SQL serveri .NET Runtime'i majutusteenuse](http://dl.acm.org/citation.cfm?id=1007669)
