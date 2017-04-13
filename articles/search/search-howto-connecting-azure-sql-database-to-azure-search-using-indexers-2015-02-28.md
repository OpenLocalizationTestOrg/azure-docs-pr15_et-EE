<properties 
    pageTitle="Azure'i SQL-andmebaasiga ühenduse Azure'i otsingu abil Indexers | Microsoft Azure'i | Indexers" 
    description="Saate teada, kuidas Azure'i SQL-andmebaasi andmete toomine Azure'i Otsinguindeksi indexers abil." 
    services="search" 
    documentationCenter="" 
    authors="chaosrealm" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="05/26/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Azure'i SQL-andmebaasi ühenduse Azure'i otsingu abil indexers

Azure'i otsinguteenuse on majutatud cloud otsingu teenus, mis hõlbustab andma hea otsing kogemus. Enne kui saate otsida, peate oma andmete Azure'i otsinguregistri asustamiseks. Kui andmete elu Azure SQL-andmebaasis, uus **Azure'i otsingu indekseerija Azure SQL-i andmebaasi** (või **Azure SQL-i indekseerija** lühikese) Azure'i otsingus indekseerimise automatiseerida. See tähendab, et teil on vähem koodi kirjutamiseks ja vähem infrastruktuur hoolduse kohta.

Praegu indexers töötavad ainult Azure'i SQL-andmebaasi, SQL Server Azure'i VMs ja [Azure'i DocumentDB](../documentdb/documentdb-search-indexer.md). Selles artiklis me keskenduda indexers, mis töötavad Azure SQL-andmebaas. Kui soovite näha täiendavad andmeallikate kasutajatugi, esitage [Azure'i otsingu tagasiside Foorum](https://feedback.azure.com/forums/263029-azure-search/)tagasiside.

Käesolevas artiklis hõlmab mehaanika indexers abil, kuid me kuvatakse ka süvitsiminek funktsioonid ja käitumist, mis on saadaval vaid versioonidega SQL-i andmebaasid (nt integreeritud muutuste jälituse).

## <a name="indexers-and-data-sources"></a>Indexers ja andmeallikad

Häälestada ja konfigureerida indekseerija Azure SQL-i, saate helistada [Azure'i Search REST API](http://go.microsoft.com/fwlink/p/?LinkID=528173) luua ja hallata **indexers** ja **andmeallikate**. 

Saate [indekseerija klassi](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.indexer.aspx) [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)või andmete importimine viisardi [Azure portaali](https://portal.azure.com) loomine ja ajastamine indekseerija.

**Andmeallika** saate määrata, millised andmed registrisse, mandaati juurde pääseda andmete ja -poliitikad, mis võimaldavad Azure'i otsingu tõhusaks tuvastamiseks muutuste andmed (uued, muudetud või kustutatud read). See on määratletud on sõltumatu ressurss, et seda saab kasutada mitut indexers.

Mõne **indekseerija** on ressurss, mis ühendab andmeallikate target otsingu registrid. Indekseerija kasutatakse järgmiselt:
 
- Ühekordse andmetega asustada registri koopia teha.
- Registri värskendamine muudatustega andmeallika ajakava.
- Käivitage nõudmisel registri värskendamiseks vastavalt vajadusele. 

## <a name="when-to-use-azure-sql-indexer"></a>Millal kasutada Azure SQL-i indekseerija

Sõltuvalt mitmest tegurist seotud andmete, Azure SQL-i indekseerija kasutamine võib või ei pruugi olla sobiv. Kui teie andmed vastab järgmistele nõuetele, saate kasutada Azure SQL-i indekseerija: 

- Kõik andmed on pärit ühe tabeli või kuvamine
    - Kui andmed on hajutatud mitme tabeli, saate vaate loomine ja selle indekseerija vaate kasutamine. Siiski arvestage, et kui kasutate vaadet, ei saa kasutada SQL serveri integreeritud muuta tuvastamine. Üksikasjalikumat teavet jaotisest. 
- Andmeallika kasutatud andmetüüpide ei toeta selle indekseerija. Enamikul, kuid mitte kõiki selle SQL Toetatavad andmetüübid. Lisateavet leiate teemast [vastendamise andmetüüpide Azure'i otsing](http://go.microsoft.com/fwlink/p/?LinkID=528105). 
- Ei vaja lähedal registri kui rea muudatusi reaalajas värskendused. 
    - Selle indekseerija uuesti indekseerimine tabeli kõige iga 5 minuti järel. Kui sageli oma andmete muutmine ja muudatuste vaja kajastuma registri sekundite või ühe, soovitame otse [Azure'i otsingu Index API -ga](https://msdn.microsoft.com/library/azure/dn798930.aspx) . 
- Kui teil on suur andmekomplekt ja kavandamine Käivita indekseerija ajakavas oma skeemi võimaldab meil tõhus tuvastamine muudetud (ja kustutada, kui see on rakendatav) read. Lisateavet leiate artiklist "Hõivamine muudetud ja kustutatud ridade" allpool. 
- Indekseeritud väljad rea maht ei ületa maksimaalmahu Azure otsingu indekseerimine taotlus, mis on 16 MB. 

## <a name="create-and-use-an-azure-sql-indexer"></a>Loomine ja kasutamine indekseerija Azure SQL-i

Esmalt looge andmeallikas. 

    POST https://myservice.search.windows.net/datasources?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }


Saate ühendusstringi toomine [Azure klassikaline portaali](https://portal.azure.com); Kasutage funktsiooni `ADO.NET connection string` suvand.

Looge target Azure'i otsinguregistri kui teil pole veel üks. Saate seda teha [portaali UI](https://portal.azure.com) või [Luua registri API](https://msdn.microsoft.com/library/azure/dn798941.aspx)kaudu.  Veenduge, et teie target index skeemiga on ühildu lähtetabel skeemiga - [vastenduse vahel SQL Azure'i otsingu andmetüübid ja](#TypeMapping) üksikasju vt.

Lõpuks luua selle indekseerija, andes selle nime ja andmete lähte- ja registri viitamine:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Sellisel viisil loodud indekseerija pole ajakava. See käivitub automaatselt kord kohe pärast selle loomist. Saate kasutada seda uuesti igal ajal **käivitada indekseerija** päringuga.

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2015-02-28 
    api-key: admin-key

Saate kohandada mitme aspektide indekseerija käitumine, nagu paketi suurus ja kui palju dokumente saate vahele enne mõne indekseerija täitmise on nurjunud. Lisateabe saamiseks vt [Loomine indekseerijat API](https://msdn.microsoft.com/library/azure/dn946899.aspx).
 
Võib-olla peate esmalt lubama Azure teenused teie andmebaasiga ühenduse loomiseks. [Ühenduse loomine: Azure'i](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) leiate juhised, kuidas seda teha.

Registraatori olekut ja täitmise jälgimiseks ajalugu (indekseeritud üksuste arv ebaõnnestumisi, jne), kasutage taotluse **registraatori olekut** : 

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2015-02-28 
    api-key: admin-key

Vastuse peaks välja nägema umbes järgmine: 

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null 
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null 
            },
            ... earlier history items 
        ]
    }

Ajaloo täitmise sisaldab kõige viimati lõplikus täitmised, mis on pööratud ajalises järjestuses sorditud (nii, et uusim täitmise jõuab vastusesse) kuni 50. Lisateavet vastuse leiate [Registraatori olekut hankimine](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Käivitage indexers ajakava

Samuti saate korraldada indekseerija käivitamiseks perioodiliselt ajakava. Selleks lisage lihtsalt loomisel või selle indekseerija värskendamise **ajakava** atribuuti. Järgmises näites on kuvatud panna taotluse selle indekseerija värskendamiseks:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

**Intervall** parameeter on nõutav. Intervalli viitab kaks järjestikust indekseerija täitmised alguse vahelise aja. Väikseim lubatud intervall on 5 minuti jooksul; pikima on üks päev. See peab vormindatud XSD-"dayTimeDuration" väärtus (piiratud Alamhulk on [ISO 8601 kestus](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) -väärtus). See muster on: `P(nD)(T(nH)(nM))`. Näited: `PT15M` iga 15 minuti `PT2H` iga 2 tundi.

Valikuline **startTime** näitab, et kui kavandatud täitmised alustaks; kui see on välja jäetud, kasutatakse praeguse UTC aega. Praegu saab varem –, kus on ajastatud juhul esimese täitmise, kui selle indekseerija peab töötama pidevalt, kuna selle startTime.  

Korraga saab käitada ainult ühte täitmise antud indekseerija. Kui indekseerija on juba täitmisel, kui järgmise peaks käivitamiseks, täitmise vahele ja elulookirjeldused järgmise intervalliga, eeldades, et muud indekseerija töö töötab.

Vaatame näiteks teha konkreetseid. Oletame, et saaksime järgmine kord tunnis ajakava, mis on konfigureeritud: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Siin on, mis juhtub: 

1. Esimese indekseerija täitmise käivitab või umbes 1 märts 2015 12.00 UTC.
1. Oletame, et see täitmise võtab 20 minutit (või mis tahes ajal väiksem kui 1 tund).
1. Teine täitmise käivitab või umbes 1 märts 2015 01:00:00 
1. Nüüd Oletame, et täitmise see võtab rohkem kui üks tund (see nõuab dokumentide see, et tegelikult suur hulk, kuid see on kasulik näide) – näiteks 70 minuti – nii, et see on lõpule jõudnud umbes 2:10:00
1. See on nüüd 2 kella, kord kolmanda täitmise alustamiseks. Siiski, kuna teine täitmise kella 1 töötab, kolmanda täitmise vahele. Kolmanda täitmise algab 3: 00

Saate lisada, muuta või kustutada olemasolevate indekseerija ajakava **sellele indekseerija** taotluse abil. 

## <a name="capturing-new-changed-and-deleted-rows"></a>Hõivamine uued, muudetud ja kustutatud read

Kui kasutate ajakava ja teie tabel sisaldab ravisaajate ridade arvu, kasutage andmete muutmine tuvastamise poliitika nii, et selle indekseerija tõhusaks saate tuua ainult uusi või muudetud ridu ilma andmeotsingu kogu tabel.

### <a name="sql-integrated-change-tracking-policy"></a>SQL-i integreeritud muutuste jälituse poliitika

Kui SQL-andmebaasi toetab [muutuste jälituse](https://msdn.microsoft.com/library/bb933875.aspx), soovitame **SQL integreeritud muutmine jälgimise poliitika**. See poliitika lubab kõige tõhusam muutuste jälituse ja see lubab ka Azure otsingu tuvastamiseks kustutatud read pole vaja konkreetsete "pehmed Kustuta" veeru lisamine tabelisse.

Integreeritud muutuste jälituse on toetatud, alustades järgmine SQL serveri andmebaasi versiooni:
 
- SQL Server 2008 R2 ja uuemad versioonid, kui kasutate SQL Server Azure'i VMs. 
- Azure SQL-i andmebaasi V12, kui kasutate Azure SQL-andmebaas.

Kui SQL-i integreeritud jälituse poliitika, kasutades määratud eraldi andmete kustutamise tuvastamise poliitika - see poliitika on tugi tuvastamise kustutatud read.

Selle poliitika saab kasutada ainult tabelid; ei saa kasutada vaatega. Peate lubama muutuste jälituse kasutate, enne kui saate selle poliitika tabel. Vaadake [lubamine ja keelamine muutuste jälituse](https://msdn.microsoft.com/library/bb964713.aspx) olevaid juhiseid. 

Kasutage seda poliitika, luua või värskendada oma andmeallika umbes järgmine:
 
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
      }
    }

### <a name="high-water-mark-change-detection-policy"></a>Kõrge vee märgi muuta tuvastamine poliitika

Kui SQL-i integreeritud muutuste jälitamise poliitika on soovitatav, ei saa seda kasutada, kui teie andmed on vaates või kui te kasutate vanemat versiooni SQL Azure'i andmebaasi. Sellisel juhul kaaluge kõrge vee märgi poliitika. Kui tabel sisaldab veerg, mis vastab järgmistele kriteeriumitele, saate kasutada selle poliitika:

- Lisab kõik määrama veeru väärtuse. 
- Kõik värskendused üksuse muuta ka veeru väärtuse. 
- Selle veeru väärtus suureneb iga muudatuse.
- Päringud, mis kasutavad mõne `WHERE` klausel sarnane `WHERE [High Water Mark Column] > [Current High Water Mark Value]` saab teostada tõhusalt.

Näiteks on indekseeritud **rowversion** veerg on soovitud optimaalne testversiooni kõrge vee märgi veeru. Kasutage seda poliitika, luua või värskendada oma andmeallika umbes järgmine: 

    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
      }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Pehmete kustutamine veeru kustutamise tuvastamise poliitika

Ridade kustutamisel tabelist Allika tõenäoliselt soovite nende ridade kustutamiseks need otsinguregistrisse. Kui kasutate SQL-i integreeritud jälituse poliitika, see on tehtud eest teie eest. Siiski kõrge vee märgi muutuste jälitamise poliitika ei aidata teil kustutatud read. Mida teha? 

Kui read on viidud tabelist, olete õnne – ei ole võimalik tuletamine olemasolu kirjeid, mida pole enam olemas.  Siiski saate "sujuvate Kustuta" tehnika märkimiseks nimega loogiliselt kustutatud ridade eemaldamine tabelist veeru lisamise ja märgistus ridu kustutada selle veeru markeri väärtuse kasutamine ilma.

Kustuta sujuvate tehnika kasutamisel saate määrata selle sujuvate kustutamine poliitika järgmiselt loomise või värskendamisel andmeallika: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

Pange tähele, et **softDeleteMarkerValue** peab olema string – kasutage stringi kujutis oma tegelik väärtus. Näiteks kui teil on mõni täisarv veerg, kuhu kustutatud read on märgitud väärtus 1, kasutada `"1"`; Kui teil on BITINE veerg, kuhu kustutatud read on märgitud loogikaväärtus true, kasutage `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>SQL-andmetüübid ja Azure otsingu andmetüübid kaardistamine

|SQL-andmetüüp | Lubatud target index Väljatüübid |Märkmete 
|------|-----|----|
|bit|Edm.Boolean Edm.String| |
|int, smallint, tinyint |Edm.Int32, Edm.Int64, Edm.String| |
| suur täisarv | Edm.Int64 Edm.String | |
| hõljumine Real, |Edm.Double Edm.String | |
| smallmoney raha komakohtade arv | Edm.String| Azure'i otsing ei toeta kümnendarvu tüüpi teisendamisel Edm.Double, kuna see kaotaks täpsus |
| char nchar, varchar, nvarchar | Edm.String<br/>Collection(EDM.string)|Muutes stringi veeru Collection(Edm.String) nõuab eelvaade API versioon 2015-02-28-eelvaade. Vt [käesoleva artikli](search-api-indexers-2015-02-28-preview.md#CreateIndexer) üksikasjad| 
|smalldatetime, kuupäev ja kellaaeg, datetime2, kuupäeva, datetimeoffset |Edm.DateTimeOffset Edm.String| |
|uniqueidentifer | Edm.String | |
|geograafia | Edm.GeographyPoint | Toetatakse ainult geograafia eksemplari tüüp punkti SRID 4326 (vaikesäte) | | 
|ROWVERSION| N/A |Rea-veerud ei saa otsinguregister talletada, kuid neid saab kasutada muutuste jälituse | |
| aeg, kuuline ajavahemik, kahendarvuks, varbinaar, image, xml, geomeetria, CLR-i andmetüübid | N/A |Pole toetatud |


## <a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

**Q:** Saate kasutada Azure SQL-i indekseerija IaaS VMs Azure SQL-i andmebaaside?

V: Jah. Siiski, peate esmalt lubama oma otsinguteenuse andmebaasiga ühenduse loomiseks oma, tehes järgmist kaks asja. Lugege lisateavet artikli [konfigureerimine on Azure VM SQL Server Azure'i otsingu indekseerija ühendus](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md) .

1. Võimalik, et peate oma andmebaasi konfigureerimine usaldusväärse serdiga nii, et Otsimisteenuse abil saate avada SSL-i ühendused andmebaasi.
2. Konfigureerige tulemüür lubada juurdepääsu oma otsinguteenuse IP-aadress.

**Q:** Saate kasutada Azure SQL-i indekseerija töötab asutusesisese SQL-andmebaase? 

V: me soovitame või toetavad selline, nagu seda teha nõuaks saate avada oma Interneti-liikluse andmebaase. 

**Q:** Saate kasutada Azure SQL-i indekseerija peale töötab IaaS Azure SQL serveri andmebaasi? 

V: me ei toeta seda stsenaariumi, kuna me ei ole testinud indekseerija mis tahes andmebaasidega peale SQL serveri.  

**Q:** Saate luua mitme indexers ajakava töötab? 

V: Jah. Siiski ainult üks indekseerija saate töötama ühe sõlme korraga. Kui teil on vaja töötab üheaegselt mitu indexers, kaaluge ülespoole oma otsinguteenuse Otsi rohkem kui ühe üksuse. 

**Q:** Töötab indekseerija mõjutab päringu töökoormus? 

V: Jah. Indekseerija üks sõlmed sisse oma otsinguteenus töötab ja selle sõlme ressursse jagatud indekseerimine ja päringu liikluse ja muude API kutsed. Kui käivitate intensiivse indekseerimine ja päringu töökoormus ja ilmneda 503 tõrgete või suurenevad vastuse korda kõrge, kaaluge oma otsinguteenuse ülespoole.
