<properties
    pageTitle="Andmete üleslaadimiseks Azure'i otsing | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Saate teada, kuidas andmete üleslaadimine registri Azure'i otsing."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search"></a>Laadi andmed Azure'i otsing
> [AZURE.SELECTOR]
- [Ülevaade](search-what-is-data-import.md)
- [.NET-I](search-import-data-dotnet.md)
- [ÜLEJÄÄNUD](search-import-data-rest-api.md)


## <a name="data-upload-models-in-azure-search"></a>Andmete üleslaadimine mudelite Azure otsing
On kaks võimalust Azure'i otsinguregistri oma andmetega asustada. Esimene variant on käsitsi lükates andmete registrisse Azure'i Search [REST API -ga](search-import-data-rest-api.md) või [.NET SDK](search-import-data-dotnet.md)abil. Teine võimalus on [toetatud andmeallikas osutage](search-indexer-overview.md) Azure'i otsinguregistri ja andke Azure'i otsing automaatselt teie andmete toomine lisandmoodulisse otsinguteenus.

Sellest juhendist hõlmab juhised tõuketeatised näidise andmete üleslaadimine (mis on toetatud ainult [REST API](search-import-data-rest-api.md) ja [.NET SDK](search-import-data-dotnet.md)), kuid saate ikkagi kohta lisateavet allpool pull mudel.

### <a name="push-data-to-an-index"></a>Tõuketeatised registri andmed

Seda moodust viitab andmete programmiliselt saata Azure'i otsingu kättesaadavaks otsimiseks. On liiga nõrk latentsus nõuetele rakendusi (nt juhul, kui otsite vaja toimingute sünkroonis dünaamiline varude andmebaasid), tõuketeatised mudel on ainult.

Saate [REST API -ga](https://msdn.microsoft.com/library/azure/dn798930.aspx) või [.NET SDK](search-import-data-dotnet.md) registri tõuketeatised andmed. Praegu ei toeta tööriist surudes andmete portaali kaudu.

See lähenemine on paindlikumad pull mudeli, kuna saate üles laadida dokumente ükshaaval või pakkidena (kuni 1000 eurot paketi või 16 MB, kumb piirang on esimene). Tõuketeatised mudel võimaldab teil dokumente üles laadida Azure'i otsimiseks olenemata sellest, kui teie andmed on.

### <a name="pull-data-into-an-index"></a>Andmete toomine lisandmoodulisse registri

Pull mudeli toetatud andmeallika indekseerimise ja automaatselt lisatud andmed üheks Azure'i otsing indeks teie eest. Jälgides muutusi ja kustutab olemasolevad dokumendid Lisaks uute dokumentide tuvastamine, indexers eemaldada vajadust aktiivselt Halda indeks andmed.

Azure'i otsing, rakendatakse see võimalus kaudu *indexers*, [bloobimälu (eelvaade)](search-howto-indexing-azure-blob-storage.md), [DocumentDB](http://aka.ms/documentdb-search-indexer), [Azure SQL-andmebaas ja SQL Server Azure'i VMs](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)praegu saadaval.

Indekseerija funktsioon on avatud [Azure portaali](search-import-data-portal.md) samuti nagu [REST API -ga](https://msdn.microsoft.com/library/azure/dn946891.aspx).
