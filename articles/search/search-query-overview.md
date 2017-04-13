<properties
    pageTitle="Päringu Azure'i otsinguregistri | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Azure'i Otsingus Otsingupäringu koostamiseks ja kasutada otsinguparameetreid otsingutulemuste filtreerimine ja sortimine."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index"></a>Azure'i otsinguregistri päring
> [AZURE.SELECTOR]
- [Ülevaade](search-query-overview.md)
- [Portaal](search-explorer.md)
- [.NET-I](search-query-dotnet.md)
- [ÜLEJÄÄNUD](search-query-rest-api.md)

Azure'i otsing otsing taotluste esitamisel on mitmeid parameetrite kõrval tegelik sõnad rakenduse väljale Otsi tipitud määratud. Need päringu parameetrite abil saate otsingu kogemusi mõned täpsemat kontrolli saavutamiseks.

Allpool on loend, mis lühidalt selgitab levinud kasutava päringu parameetrite Azure'i otsing. Täielik ulatus päringu parameetrite ja nende käitumine, lugege üksikasjalikku lehtede [REST API -ga](https://msdn.microsoft.com/library/azure/dn798927.aspx) ja [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.searchparameters_properties.aspx).

## <a name="types-of-queries"></a>Tüüpi päringud

Azure'i otsing pakub palju võimalusi väga võimas päringute loomine. Päringu kasutate kahte peamist tüüpi on `search` ja `filter`. A `search` päringu otsib üles ühe või mitme tingimuse kõik _otsitavate_ väljade indeks ja töötab nii, nagu ootate otsingumootori nagu Google või Bingi töötamiseks. A `filter` päringu hindab loogikaavaldis üle kõik _filtreeritavad_ väljad registri. Erinevalt `search` päringud `filter` päringute vastama välja, mis tähendab, et need on tõstutundlikud string väljade sisu.

Otsingud ja Filtrid saate kasutada koos või eraldi. Kui te neid kasutada koos, esmalt rakendatakse filtrit kogu registri ja seejärel otsing on läbi filtri tulemused. Filtrite seetõttu võib olla kasulik tehnika päringu jõudluse parandamiseks, kuna need vähendada dokumentidest, mis tuleb otsingupäringu töötlemine.

Filtriavaldiste süntaks on alamhulk [OData filter keel](https://msdn.microsoft.com/library/azure/dn798921.aspx). Otsingupäringu saate [lihtsustatud süntaks](https://msdn.microsoft.com/library/azure/dn798920.aspx) või [Lucene päringusüntaksit](https://msdn.microsoft.com/library/azure/mt589323.aspx) , mis on allpool.

### <a name="simple-query-syntax"></a>Lihtsa päringu süntaks
[Lihtsa päringu süntaks](https://msdn.microsoft.com/library/azure/dn798920.aspx) on kasutatud Azure'i otsingu päringu vaikekeele. Lihtsa päringusüntaksit toetab paljusid levinud otsingu tehtemärgid AND, sh või mitte, fraasi, järelliite ja tehtemärkide järgnevus.

### <a name="lucene-query-syntax"></a>Lucene päringu süntaks
[Lucene päringusüntaksit](https://msdn.microsoft.com/library/azure/mt589323.aspx) võimaldab teil kasutada suuresti vastu ja väljendusvõimalustega päringukeele välja töötatud [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)osana.

Päringu süntaksit kasutamine võimaldab teil hõlpsasti saavutada järgmisi võimalusi: [välja ulatusega päringud](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fields), [udune otsing](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fuzzy), [lähedus otsing](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_proximity), [Termini suurendada](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_termboost), [Lihtavaldise otsing](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_regex), [metamärkide otsing](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_wildcard), [süntaks põhikomponentide](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_syntax)ja [päringute kasutamine toetatud brauserid](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_boolean).



## <a name="ordering-results"></a>Tulemuste tellimine
Otsingupäringu tulemuste saamisel saate taotleda, et Azure'i otsing toimib tellitud väärtused konkreetsel väljal tulemused. Vaikimisi Azure'i Search tellimusi otsingutulemite iga dokumendi otsing Keskmine, mis on saadud [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)asukoha alusel.

Kui soovite Azure'i Otsingu tulemused tellitud väärtus välja Otsing Keskmine, saate kasutada tagastamiseks on `orderby` otsida parameeter. Saate määrata väärtus on `orderby` parameeter välja nimed ja kõned on [ `geo.distance()` funktsioon](https://msdn.microsoft.com/library/azure/dn798921.aspx) ruumilisel väärtuste jaoks. Iga avaldise saate järgneb `asc` näitamaks, et tulemid on nõutav tõusvas järjestuses, ja `desc` näitamaks, et tulemid on nõutav laskuvas järjestuses. Vaikimisi järjestus tõusvas järjestuses.

## <a name="paging"></a>Lehtede nummerdamine
Azure'i otsingu abil on lihtne Piipar otsingutulemite rakendada. Abil soovitud `top` ja `skip` parameetrid, saate sujuvalt probleemi otsingu taotlusi, mis võimaldavad teil saada kokku otsingu tulemuste komplekti mõistliku, järjestatud laste, mis võimaldavad hõlpsalt hea otsing UI tavad. Saamisel nende tulemite väiksem kohta, samuti saate dokumentide arvu otsingutulemite kogu kogumi.

Lisateavet leiate teemast Piipar artiklist [Kuidas lehe Otsingu tulemused Azure'i otsingus](search-pagination-page-layout.md)otsingutulemite kohta.


## <a name="hit-highlighting"></a>Tulemus esiletõstmine
Azure'i otsing, rõhutades otsingutulemeid, mis vastavad otsingupäringu täpse osa on lihtne, kasutades funktsiooni `highlight`, `highlightPreTag`, ja `highlightPostTag` parameetrid. Saate määrata millised _otsitavate_ väljad peaks ning täpsustades täpse stringi silte lisada algus ja lõpp sobitatud teksti Azure'i otsing tagastab rõhutada tema sobitatud teksti.
