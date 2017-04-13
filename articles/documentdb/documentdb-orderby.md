<properties 
    pageTitle="Järjestusalus DocumentDB andmete sortimine | Microsoft Azure'i" 
    description="Siit saate teada, ORDER BY DocumentDB päringute LINQ ja SQL-i kasutamine ja ORDER BY päringuid indekseerimise poliitika määramine." 
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="arramac"/>

# <a name="sorting-documentdb-data-using-order-by"></a>DocumentDB andmete abil järjestuses sortimine
Microsoft Azure'i DocumentDB toetab JSON dokumentide SQL-i päringu dokumentide. Päringutulemite tellida klausel ORDER BY kasutamine päringu SQL-lauseid.

Pärast selle artikli lugemist on teil saama vastavad järgmistele küsimustele. 

- Kuidas teha päringu abil Järjestusalus?
- Kuidas seadistada indekseerimise poliitika Järjestusalus jaoks?
- Mis on tulemas järgmine?

[Näidiseid](#samples) ja [FAQ](#faq) on olemas.

Täieliku viite SQL-päringud, vt [DocumentDB päringu õpetuse](documentdb-sql-query.md).

## <a name="how-to-query-with-order-by"></a>Kuidas päringut tellimusega järgi
Nagu ANSI-SQL-i, saate nüüd kaasata valikuline klausel Order By SQL-lausete kui päringu DocumentDB. Klausel võib sisaldada valikuline ASC/LASKUV argument täpsustamiseks, kus tulemid tuuakse. 

### <a name="ordering-using-sql"></a>Tellimine SQL-i abil
Näiteks siin on päring toomiseks ülemine 10 raamatud laskuvas järjestuses oma tiitlid. 

    SELECT TOP 10 * 
    FROM Books 
    ORDER BY Books.Title DESC

### <a name="ordering-using-sql-with-filtering"></a>SQL-i abil filtreerimise tellimine
Iga pesastatud vara dokumendid, nagu Books.ShippingDetails.Weight abil saate tellida ja täiendavad filtrid saate määrata WHERE-klausel Order By koos nagu järgmises näites:

    SELECT * 
    FROM Books 
    WHERE Books.SalePrice > 4000
    ORDER BY Books.ShippingDetails.Weight

### <a name="ordering-using-the-linq-provider-for-net"></a>Kasutades .net-i pakkuja LINQ tellimine
Kasutades .NET SDK versioon 1.2.0 ja kõrgem, samuti saate OrderBy() või OrderByDescending() klausel sees LINQ päringute, nagu järgmises näites:

    foreach (Book book in client.CreateDocumentQuery<Book>(UriFactory.CreateDocumentCollectionUri("db", "books"))
        .OrderBy(b => b.PublishTimestamp)
        .Take(100))
    {
        // Iterate through books
    }

DocumentDB toetab tellimine ühe arv, string või kahendmuutuja atribuudi päring, Tulekul päringu andmetüüpidega. Lugege [mis peagi järgmise](#Whats_coming_next) rohkem üksikasju.

## <a name="configure-an-indexing-policy-for-order-by"></a>Jaoks Järjestusalus poliitika indekseerimise konfigureerimine

Võta tagasi, et DocumentDB toetab kahte tüüpi registrid (räsi ja vahemiku), saate määrata teatud teed/atribuudid, andmetüübid (stringide/arvud) ja muu väärtused (täpsus või fikseeritud arvutustäpsuse väärtust). Kuna DocumentDB kasutab vaikimisi indekseerimine räsi, peate looma uue saidikogumi kohandatud indekseerimise poliitika koos vahemiku arvud, stringid või mõlemad, Järjestusalus kasutamiseks. 

>[AZURE.NOTE] Tekstistring vahemiku registrid võeti kasutusele 7 juuli 2015 versioon 2015-06-03 REST API-ga. Selleks, et luua poliitikate jaoks Järjestusalus vastu stringid, peate kasutama SDK versiooni 1.2.0 .NET SDK või versiooni 1.1.0 Python, Node.js või Java SDK.
>
>Varasema versiooni 2015-06-03 REST API-ga, oli saidikogumi indekseerimise vaikepoliitika räsi nii stringide ja arvud. See on muudetud räsi stringide ja vahemiku arvude jaoks. 

Lisateabe saamiseks vt [DocumentDB indekseerimise reegleid](documentdb-indexing-policies.md).

### <a name="indexing-for-order-by-against-all-properties"></a>Indekseerimine Järjestusalus vastu kõik atribuudid
Siin on kuidas saate luua kogumi "Kõik vahemiku" indekseerimine Järjestusalus vastu mis tahes/kõik arvulise või tekstistringi atribuudid, mis kuvatakse JSON dokumentideks sees. Siin me alistada vaikimisi index tüüp stringi väärtuste vahemiku ja täpsus (-1).
                   
    DocumentCollection books = new DocumentCollection();
    books.Id = "books";
    books.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), books);  

>[AZURE.NOTE] Pange tähele, et Järjestusalus ainult tagasi tulemused on RangeIndex indekseeritud andmetüübid (stringi ja arv). Näiteks kui teil on vaikimisi indekseerimine poliitika, mis on RangeIndex ainult arve, on Order By vastu tee, mille stringiväärtust tagastab pole üldse dokumente.

### <a name="indexing-for-order-by-for-a-single-property"></a>Ühe atribuudi jaoks Järjestusalus indekseerimine
Siin on, kuidas saate luua kogumi indekseerimine Järjestusalus vastu lihtsalt tiitliatribuut, mis on string. On kaks teed, üks tiitliatribuut ("/ tiitel /?") vahemiku indekseerimine ja teine iga muude atribuudi vaikeväärtus indekseerimise kava, mis on räsi stringide ja vahemik, arvude jaoks.                    
    
    booksCollection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 } } 
            });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), booksCollection);  


## <a name="samples"></a>Näidised
Heitke pilk selle [Github näidised projekti](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) , mis näitab, kuidas kasutada Order By, sh loomise indekseerimine poliitikate ja Piipar Järjestusalus abil. Soovitame teil pull taotlusi panuse, millest võib olla kasu muude DocumentDB arendajate näidised on avatud allikas. Lugege juhised selle kohta, kuidas täiendada [haldaja juhised](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md) .  

## <a name="faq"></a>KKK

**Mis on oodatud taotlemine üksus (RU) tarbimine Järjestusalus päringud?**

Kuna Järjestusalus kasutab DocumentDB registri otsingud, taotluse ühikute tarbitud Järjestusalus päringute juhul sarnane võrdväärse päringute ilma Order By. Nagu mis tahes muud toimingut DocumentDB, sõltub taotluse ühikute dokumentide suurused ja kujundite kui ka päringu keerukusest. 


**Mis on oodatud indekseerimine kohal Järjestusalus jaoks?**

Indekseerimise salvestusruumi pea kohal oleks proportsionaalne atribuutide arv. Must stsenaarium, index pea kohal saab andmete 100%. Ei erine läbilaskevõime (taotlemine ühikud) pea kohal, valik, et indekseerimine ja vaikimisi räsi indekseerimine.

**Kuidas saan oma olemasolevad andmed DocumentDB Order By abil päringu?**

Päringutulemite kasutamine Järjestusalus sortida, et peate muutma indekseerimise poliitika kogumise kasutada vahemiku index tüüp vastu atribuudi sortimiseks kasutatud. Vaadake [indekseerimise poliitika muutmine](documentdb-indexing-policies.md#modifying-the-indexing-policy-of-a-collection). 

**Mis on seotud Järjestusalus?**

Järjestusalus saab määrata ainult atribuudi, kas arv vastu või kui see on vahemikus stringi indekseeritud täpsus (-1).

Te ei saa teha järgmist:
 
- Järjestusalus sisemine stringi omadustega nagu ID-d, _rid ja _self (tulekul).
- Järjestusalus omadustega saadud tulemus on sisene-dokumendi ühendamine (tulekul).
- Järjestusalus mitme atribuudid (tulekul).
- Järjestusalus päringutega andmebaase, saidikogumite, kasutajad, õigused või manused (tulekul).
- Järjestusalus arvutatud omadused, nt UDF/built-in funktsioon or Avaldise tulemus.

Järjestusalus praegu puhul ei toetata rist-partition päringute kasutamisel päringu Exploreri Azure'i portaalis.

## <a name="troubleshooting"></a>Tõrkeotsing

Kui saate tõrketeate, et Järjestusalus ei toetata, veenduge, et kasutate versiooni [Tarkvaraarenduskomplektist](documentdb-sdk-dotnet.md) , mis toetab Order By. 

## <a name="next-steps"></a>Järgmised sammud

Kahvli [Github näidised projekti](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) ja andmete alustada! 

## <a name="references"></a>Viited
* [DocumentDB päringu viide](documentdb-sql-query.md)
* [DocumentDB indekseerimine poliitika viide](documentdb-indexing-policies.md)
* [DocumentDB SQL-i viide](https://msdn.microsoft.com/library/azure/dn782250.aspx)
* [DocumentDB Order By näidised](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)
 

