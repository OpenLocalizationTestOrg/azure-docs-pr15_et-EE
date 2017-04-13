<properties 
    pageTitle="NoSQL Python näited DocumentDB | Microsoft Azure'i" 
    description="Otsimine NoSQL Python näited github DocumentDB, kaasa arvatud CRUD JSON dokumentide NoSQL andmebaaside tavalised toimingud." 
    keywords="Python näited"
    services="documentdb" 
    authors="moderakh" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter="python"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/18/2016" 
    ms.author="moderakh"/>


# <a name="documentdb-python-examples"></a>DocumentDB Python näited

> [AZURE.SELECTOR]
- [.Net-i näited](documentdb-dotnet-samples.md)
- [Node.js näited](documentdb-nodejs-samples.md)
- [Python näited](documentdb-python-samples.md)
- [Azure'i kood proovi Galerii](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Valimi lahendusi, mis sooritavad CRUD toimingud ja muud tavalised toimingud Azure'i DocumentDB ressursid on kaasatud [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) GitHub hoidla. Selles artiklis on toodud:

- Tööülesannete iga Python näide projekti failide linke. 
- Linkide seotud API viide sisu.

**Eeltingimused**

1. Teil on vaja Azure'i konto kasutamiseks need Python näited:
    - Saate [avada Azure'i konto tasuta](https://azure.microsoft.com/pricing/free-trial/): saate krediiti abil saate proovida makstud Azure'i teenuste ja isegi juhul, kui nad kasutada kuni hoiate konto ja kasutage tasuta Azure teenused, nt veebisaidid. Krediitkaardi kunagi tuleb tasuda, kui te just teiega sätete muutmine ja paluge võetakse.
   - Saate [aktiveerida Visual Studio abonendi eelised](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): teie Visual Studio tellimuse annab teile krediiti iga kuu makstud Azure'i teenuste kasutatavad.
2. Samuti vajate [Python SDK](documentdb-sdk-python.md). 

    > [AZURE.NOTE] Iga valim on suletud, siis häälestab ise ja puhastada pärast ise. Sellisel kujul näidised probleemi mitme kõned [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Iga kord, kui seda tehakse tellimust, saadetakse teile arve kasutamise kohta jõudluse taseme loodava kogumise 1 tund. 

## <a name="database-examples"></a>Andmebaasi näited

[Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) faili [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) projekti näitab, kuidas teha järgmisi toiminguid.

Ülesanne | API viide
--- | ---
[Andmebaasi loomine](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) | [document_client. CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Päringu konto andmebaasi jaoks](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) | [document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Lugege andmebaasi ID](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) | [document_client. ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Loendi andmebaaside konto jaoks](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) | [document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Andmebaasi kustutamine](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) | [document_client. DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)

## <a name="collection-examples"></a>Saidikogumi näited 

[Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) faili [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) projekti näitab, kuidas teha järgmisi toiminguid.

Ülesanne | API viide
--- | ---
[Kogumi loomine](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) | [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Loendi kõigi saidikogumite andmebaasi lugemine](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) | [document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Kogumi ID hankimine](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) | [document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Jõudluse taseme kogumi hankimine](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) | [DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Jõudluse taseme kogumi muutmine](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) | [document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Kollektsiooni kustutamine](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) | [document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
