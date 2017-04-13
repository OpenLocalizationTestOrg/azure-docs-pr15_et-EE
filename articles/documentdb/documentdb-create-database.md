<properties 
    pageTitle="Kuidas luua andmebaasi DocumentDB | Microsoft Azure'i" 
    description="Saate teada, kuidas andmebaasi portaali Online'i teenuse kasutamise Azure'i DocumentDB lõõskav kiire ja globaalse skaala NoSQL andmebaasi loomine." 
    keywords="Kuidas andmebaasi loomine" 
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
    ms.date="10/17/2016" 
    ms.author="mimig"/>

# <a name="how-to-create-a-database-for-documentdb-using-the-azure-portal"></a>Kuidas luua andmebaasi DocumentDB Azure'i portaalis

Microsoft Azure'i DocumentDB kasutamiseks peab teil on [DocumentDB konto](documentdb-create-account.md), andmebaasi, kogumi ja dokumendid. Azure'i portaalis DocumentDB andmebaasid on nüüd loodud samal ajal kogumi loomisel. 

Portaali DocumentDB andmebaasi ja saidikogumi loomiseks vaadake teemat [Azure portaalis DocumentDB kogumi loomise kohta](documentdb-create-collection.md).

## <a name="other-ways-to-create-a-documentdb-database"></a>Muud võimalused DocumentDB andmebaasi loomine

Andmebaasi ei saa luua portaalis, saate luua ka need [DocumentDB SDK-d](documentdb-sdk-dotnet.md) või [REST API](https://msdn.microsoft.com/library/mt489072.aspx)abil. Andmebaasidega, kasutades .NET SDK töötamise kohta leiate teavet teemast [.net-i andmebaasi näited](documentdb-dotnet-samples.md#database-examples). Node.js SDK abil andmebaasidega töötamise kohta leiate teavet teemast [Node.js andmebaasi näited](documentdb-nodejs-samples.md#database-examples). 

## <a name="next-steps"></a>Järgmised sammud

Kui teie andmebaas ja saidikogumi on loodud, saate [JSON dokumentide lisamine](documentdb-view-json-document-explorer.md) dokumendi Exploreriga portaalis [importimine dokumentide](documentdb-import-data.md) saidikogumi DocumentDB andmete migreerimise tööriista abil või kasutage ühte [DocumentDB SDK-d](documentdb-sdk-dotnet.md) CRUD-toiminguid. DocumentDB on .NET, Java, Python, Node.js ja JavaScripti API SDK-d. Näitab, kuidas luua, eemaldamine, värskendada ja kustutada dokumentide .NET koodinäiteid, leiate [.NET dokumendi näited](documentdb-dotnet-samples.md#document-examples). Node.js SDK abil dokumentidega töötamise kohta leiate teavet teemast [Node.js dokumendi näited](documentdb-nodejs-samples.md#document-examples). 

Pärast seda, kui teil on dokumentide kogumi, saate [DocumentDB SQL-i](documentdb-sql-query.md) [päringute](documentdb-sql-query.md#executing-sql-queries) käivitada teie dokumentide vastu [Päringu Exploreri](documentdb-query-collections-query-explorer.md) portaali, [REST API -ga](https://msdn.microsoft.com/library/azure/dn781481.aspx)või üks [SDK-d](documentdb-sdk-dotnet.md). 
