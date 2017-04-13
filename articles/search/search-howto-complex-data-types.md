<properties
    pageTitle="Kuidas mudel keerukate andmetüüpidega Azure'i otsingus | Microsoft Azure'i otsing"
    description="Pesastatud või hierarhilisi andmestruktuure saab modelleerida Azure'i otsinguregistri lapik Reahulk ja saidikogumite andmetüüp abil sisse."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="complex data types; compound data types; aggregate data types"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="09/07/2016"
    ms.author="liamca"
/>

# <a name="how-to-model-complex-data-types-in-azure-search"></a>Kuidas mudel keerukate andmetüüpidega Azure'i otsing

Välise andmekomplektide asustamiseks Azure'i otsingu registri mõnikord kasutada kaasata ei jagunevad korralikult tabeli Reahulk hierarhilise või pesastatud allstruktuure. Need struktuurid näiteid võib kaasa mitme asukohad ja telefoninumbrid ühe kliendi, mitme värvid ja suurused ühe SKU, mitu autorit ühe raamat, jne. Terminite modelleerimine, võite näha järgmisi struktuurid edaspidi *keerukate andmetüüpidega*, *ühendi andmetüübid*, *kombineeritud andmetüübid*või *liitmine andmetüübid*, nimi mõne.

Keerukate andmetüüpidega ei toeta algupäraselt Azure'i otsing, kuid tõestatud lahendus sisaldab lamedamad struktuuri ja seejärel kasutades andmetüüp **saidikogumi** taastamiseks sisemise ülesehituse kaheosaline toiming. Selles artiklis kirjeldatud viisil jälgimine võimaldab soovitud sisu otsida, lihvitud, filtreerida ja sortida.

## <a name="example-of-a-complex-data-structure"></a>Näide: keerukate andmestruktuur

Tavaliselt asub andmete kogumi JSON- või XML-dokumentide või üksuste NoSQL poes, nt DocumentDB. Struktuuriga, tuleneb juures on keerukas on mitu tütarüksused, mida soovite otsida ja filtreeritud.  Alguspunktina illustreerimiseks lahendus, tehke järgmist JSON dokument, mille kogumi kontaktide näitena on loetletud.

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

Kuigi väljad nimega "id", "nimi" ja "ettevõte" saab hõlpsasti vastavusse üks-ühele väljadena jooksul registri Azure'i otsing, "kohad" väljal massiivi asukohtadesse, võttes nii kogum asukoha ID-d, kui ka asukoha kirjeldus. Azure'i otsing ei ole andmetüüp, mis toetab seda, et läheb vaja teistmoodi mudel selle Azure'i otsing. 

> [AZURE.NOTE] Selle meetodi on ka kirjeldatud Kirk Evans ajaveebipostituse [Indekseerimine DocumentDB Azure otsingu abil](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), mis näitab tehnika nimega "lamedamad andmed", mille puhul on teil väljal nimega `locationsID` ja `locationsDescription` , mis on nii [saidikogumite](https://msdn.microsoft.com/library/azure/dn798938.aspx) (või massiivi stringid).   

## <a name="part-1-flatten-the-array-into-individual-fields"></a>Osa 1: Ühenda eraldi massiiv

Registri Azure'i otsing, mis sobib tuvastatavate loomiseks üksikute väljade jaoks pesastatud karkassi: `locationsID` ja `locationsDescription` andmetüübiga [saidikogumid](https://msdn.microsoft.com/library/azure/dn798938.aspx) (või massiivi stringid). Need väljad soovite indekseerida väärtusi "1" ja "2" üheks selle `locationsID` välja John Smith ja väärtused "3" ja "4" üheks selle `locationsID` välja jaoks Jen Campbell.  

Azure'i otsing oma andmeid peaks välja nägema selline: 

![Näidisandmete 2 reaga](./media/search-howto-complex-data-types/sample-data.png)


## <a name="part-2-add-a-collection-field-in-the-index-definition"></a>Osa 2: Index määratlemisel saidikogumi välja lisamine

Registri skeemiga, võib välja määratlused sarnanevad selles näites.

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a>Kinnitage otsingu käitumist ja soovi korral registri laiendamine

Eeldades, et olete loonud registri ja andmed laaditud, saate nüüd testida lahenduse otsimine päringu täitmise suhtes andmekomplekti kinnitamiseks. Iga **saidikogumi** välja peaks olema **otsitavate**, **filtreeritavad** ja **facetable**. Peaks oskama käivitada, päringute, näiteks:

* Otsige üles kõik inimesed, kes on Adventureworks Headquarters töötada.
* Saada Avaleht Office'iga isikute arvu.  
* Kuva inimesed, kes töötavad kodu asukoht, mis muud esindused need töötavad koos iga asukohas inimeste arvu.  

Kui selle meetodi kuulub peale on, kui peate tegema otsing, mis ühendab nii asukoht id kui ka asukoha kirjeldus. Näiteks:

* Kõigi isikute leidmine, mille jaoks on neil Avaleht Office ja on asukoht ID on 4.  

Kui te ei mäleta, algse sisu vaadanud umbes järgmine:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Siiski nüüd, kui meil on eraldatud andmeid eraldi väljadele, me ei ole võimalik teada, kui Office kodu jaoks Jen Campbell, mis on seotud `locationsID 3` või `locationsID 4`.  

Sel juhul käsitlema määratleda indeks, mis ühendab kõik andmed ühe saidikogumi teise välja.  Selles näites me kutsume väli `locationsCombined` ja me eraldi sisu on `||` kuigi saate valida, mis tahes eraldaja, mis teie arvates oleks märkide sisu jaoks ainulaadsed. Näiteks: 

![Näidisandmete 2 reaga eraldaja abil](./media/search-howto-complex-data-types/sample-data-2.png)

Kasutades seda `locationsCombined` välja me saate nüüd majutada rohkem päringuid, näiteks:

* Kuva arv on 'Home Office' asukoht Id "4" töötavad inimesed.  
* Otsige asukoht Id "4" kodu asukoht töötavad inimesed. 

## <a name="limitations"></a>Piirangud

See meetod on kasulik stsenaariumid arvu, kuid see pole rakendatav iga kord.  Näiteks:

1. Kui teil pole staatiline väljade kogumi oma keerukate andmetüüp ja polnud ühel väljal kõigi võimalike tüüpide vastendamiseks. 
2. Pesastatud objektide uuendamine nõuab teatud lisatööd täpselt mida on vaja värskendada Azure'i otsinguregistrisse määratlemiseks

## <a name="sample-code"></a>Proovi kood

Näiteks saate vaadata kuidas indeks keerukate JSON andmehulgas Azure'i otsingu ja sooritada päringute arv tuvastatavate selle [GitHub repo](https://github.com/liamca/AzureSearchComplexTypes)veebisaidil.

## <a name="next-step"></a>Järgmise juhise juurde

Azure'i otsingu UserVoice [hääletada kohalikke tugi keerukate andmetüüpidega](https://feedback.azure.com/forums/263029-azure-search) lehekülg ja sisestage täiendavad sisendit, mida soovite käsitleda funktsioonide rakendamise kohta. Ka jõuate mulle otse sisse veebisaidil Twitteri @liamca.


 