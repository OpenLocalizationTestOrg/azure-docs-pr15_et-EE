<properties
    pageTitle="Azure'i otsingu registri loomine | Microsoft Azure'i | Majutatud pilveteenuses otsing"
    description="Mis on Azure otsingus registri ja kuidas seda kasutada?"
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

# <a name="create-an-azure-search-index"></a>Azure'i otsingu registri loomine
> [AZURE.SELECTOR]
- [Ülevaade](search-what-is-an-index.md)
- [Portaal](search-create-index-portal.md)
- [.NET-I](search-create-index-dotnet.md)
- [ÜLEJÄÄNUD](search-create-index-rest-api.md)

## <a name="what-is-an-index"></a>Mis on registri?

*Indeks* on püsiv pood *dokumentide* ja muude kasutavad teenuse Azure otsing importida. Dokument on ühe üksuse otsitavate andmete oma registris. Näiteks on pood edasimüüjalt võib-olla dokumendi iga üksuse need müüa, uudiste ettevõttes võib olla dokumendi iga artiklis jne. Vastendus need tuttavat andmebaasi ekvivalendid: *index* sarnaneb põhimõtteliselt *tabeli*ja *dokumendid* on umbes võrdne tabeli *read* .

Kui dokumentide lisamine või üleslaadimine ja esitage Azure'i otsimiseks otsingupäringuid, saadate oma otsinguteenuse oma taotlusi teatud registrisse.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Väljatüübid ja Azure otsingu registri atribuudid

Kui määratlete oma skeemi, peate määrama nimi, tüüp ja iga välja atribuutide indeks. Välja tüüp jaotab andmeid, mis on talletatud välja. Atribuudid on seatud üksikute väljade määrata, kuidas kasutatakse välja. Järgmistes tabelites loetlemine tüübid ja saate määrata atribuute.


### <a name="field-types"></a>Väljatüübid
|Tüüp|Kirjeldus|
|------------|-----------|
|*Edm.String*|Tekst, mida saate soovi korral tuleb tokenized täisteksti otsingu (poolitus, mis jne).|
|*Collection(EDM.string)*|Loendi stringid, mida saate soovi korral tuleb tokenized otsingu jaoks. On kogumi üksuste arvu teoreetiline ülempiir, kuid kehtib 16 MB ülempiiri last suuruse saidikogumid.|
|*Edm.Boolean*|Sisaldab väärtusi, tõene/väär.|
|*Edm.Int32*|32-bitise täisarvulise väärtused.|
|*Edm.Int64*|64-bitise täisarvulise väärtused.|
|*Edm.Double*|Täpsusega arvandmed.|
|*Edm.DateTimeOffset*|Kuupäev aja esitatud OData V4 vormingus (nt `yyyy-MM-ddTHH:mm:ss.fffZ` või `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`).|
|*Edm.GeographyPoint*|Punkti tähistav maakera geograafiline asukoht.|

Siit leiate täpsemat teavet Azure'i otsingu [toetatud andmetüübid MSDN-is](https://msdn.microsoft.com/library/azure/dn798938.aspx).



### <a name="field-attributes"></a>Välja atribuutide
|Atribuut|Kirjeldus|
|------------|-----------|
|*Klahv*|String, mis sisaldab iga kasutatava dokumendi otsida dokumendi kordumatu ID-d. Iga register peab olema üks klahv. Ainult ühte välja võib olla võti ja selle tüüpi peab olema seatud Edm.String.|
|*Tagastatava*|Saate määrata, kas saate otsingutulemi välja tagasi.|
|*Filtreeritavad*|Võimaldab kasutada filtrit päringute välja.|
|*Sorditav*|Võimaldab päringu otsingutulemite abil seda välja sortida.|
|*Facetable*|Võimaldab kasutajal enda juhitud filtreerimine [lihvitud navigeerimise](search-faceted-navigation.md) struktuuri kasutatakse välja. Väljad, mis sisaldavad korduvate väärtused, mille abil saate rühmitada mitu dokumenti (nt mitu dokumenti, mida ühe kaubamärgi või teenuse kategooria) töötavad tavaliselt kõige paremini aspekte.|
|*Otsitav*|Märgib välja nagu kogu teksti otsida.|

Siit leiate täpsemat teavet Azure'i otsingu [registri atribuudid MSDN-is](https://msdn.microsoft.com/library/azure/dn798941.aspx).



## <a name="guidance-for-defining-an-index-schema"></a>Juhised skeemiga index määratlemine

Kujundamisel oma index, võtke aega kavandamise etapis iga otsust läbi mõtlema. On oluline hoida oma otsingu kasutaja kogemusi ja ettevõtte vajadustele arvesse oma registri kujundamisel, nagu iga välja peab olema määratud [õige atribuute](https://msdn.microsoft.com/library/azure/dn798941.aspx). Pärast juurutamist registri muuta hõlmab taastamine ja pealelaadimisel andmed.


Kui andmete mäluruumi aja jooksul muutuda, saate suurendada või vähendada võimsus lisada või eemaldada sektsioonid. Lisateavet leiate teemast [Azure oma otsinguteenuse haldamine](search-manage.md) või [Teenuste piirangud](search-limits-quotas-capacity.md).
