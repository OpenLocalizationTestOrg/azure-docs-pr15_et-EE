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
    ms.date="10/27/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Azure'i SQL-andmebaasi ühenduse Azure'i otsingu abil indexers

Azure'i otsinguteenuse on majutatud cloud otsingu teenus, mis hõlbustab andma hea otsing kogemus. Enne kui saate otsida, peate oma andmete Azure'i otsinguregistri asustamiseks. Kui andmete elu SQL Azure'i andmebaasi, saate uue **Azure otsingu indekseerija Azure SQL-i andmebaasi** (või **Azure SQL-i indekseerija** lühikese) indekseerimise automatiseerida. See tähendab, et teil on vähem koodi kirjutamiseks ja vähem infrastruktuur hoolduse kohta.

Selles artiklis antakse ülevaade mehaanika indexers abil, kuid samuti kirjeldatakse funktsioone, mis on ainult SQL Azure'i andmebaasid (nt integreeritud muutuste jälituse) saadaval. Azure'i otsingu toetab samuti teistest andmeallikatest, nt DocumentDB Azure'i bloobimälu ning tabelimälu. Kui soovite näha täiendavad andmeallikate jaoks, mis teie tagasiside [Azure'i otsingu tagasiside Foorum](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="indexers-and-data-sources"></a>Indexers ja andmeallikad

Saate häälestada ja konfigureerida Azure SQL-i indekseerija abil. 

- Andmete impordiviisardi [Azure'i portaal](https://portal.azure.com)
- Azure'i otsingu [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)
- Azure'i Search [REST API-ga](http://go.microsoft.com/fwlink/p/?LinkID=528173)

Selles artiklis kasutame näitab teile, kuidas luua ja hallata **indexers** ja **andmeallikate**REST API-ga. 

**Andmeallika** saate määrata, millised andmed registrisse, juurde pääseda andmete ja -poliitikad, muudatused (uued, muudetud või kustutatud read) andmete tõhus tuvastavat mandaati. See on määratletud on sõltumatu ressurss, et seda saab kasutada mitut indexers.

Mõne **indekseerija** on ressurss, mis ühendab andmeallikate target otsingu registrid. Indekseerija kasutatakse järgmiselt:
 
- Ühekordse andmetega asustada registri koopia teha.
- Registri värskendamine muudatustega andmeallika ajakava.
- Käivitage nõudmisel registri värskendamiseks vastavalt vajadusele. 

## <a name="when-to-use-azure-sql-indexer"></a>Millal kasutada Azure SQL-i indekseerija

Sõltuvalt mitmest tegurist seotud andmete, Azure SQL-i indekseerija kasutamine võib või ei pruugi olla sobiv. Kui teie andmed vastab järgmistele nõuetele, saate kasutada Azure SQL-i indekseerija: 

- Kõik andmed on pärit ühe tabeli või kuvamine
    - Kui andmed on hajutatud mitme tabeli, saate vaate loomine ja selle indekseerija vaate kasutamine. Juhul, kui kasutate vaadet, ei saa kasutada SQL serveri integreeritud muuta tuvastamine. [Lisateavet leiate jaotisest.](#CaptureChangedRows) 
- Andmeallika kasutatud andmetüüpide ei toeta selle indekseerija. Enamik, kuid mitte kõigis SQL-i andmetüübid on toetatud. Lisateavet leiate teemast [vastendamise andmetüüpide Azure'i otsing](http://go.microsoft.com/fwlink/p/?LinkID=528105). 
- Ei vaja lähedal registri kui rea muudatusi reaalajas värskendused. 
    - Selle indekseerija uuesti indekseerimine tabeli kõige iga 5 minuti järel. Kui sageli oma andmete muutmine ja muudatuste vaja kajastuma registri sekundite või ühe, soovitame otse [Azure'i otsingu Index API -ga](https://msdn.microsoft.com/library/azure/dn798930.aspx) . 
- Kui teil on suur andmekomplekt ja kavandamine Käivita indekseerija ajakavas oma skeemi võimaldab meil tõhus tuvastamine muudetud (ja kustutada, kui see on rakendatav) read. Lisateavet leiate artiklist "Hõivamine muudetud ja kustutatud ridade" allpool. 
- Indekseeritud väljad rea maht ei ületa maksimummaht Azure'i otsingu indekseerimine taotlus, mis on 16 MB. 

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

Looge target Azure'i otsinguregistri kui teil pole veel üks. Saate luua registri [portaali UI](https://portal.azure.com) või [Luua registri API](https://msdn.microsoft.com/library/azure/dn798941.aspx)abil. Veenduge, et teie target index skeemiga on ühildu lähtetabel skeemiga - [vastenduse SQL Azure'i otsingu andmetüübid ja vahel](#TypeMapping)näha.

Lõpuks luua selle indekseerija, andes selle nime ja andmete lähte- ja registri viitamine:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Sellisel viisil loodud indekseerija pole ajakava. See käivitub automaatselt, kui see loomise. Saate kasutada seda uuesti igal ajal **käivitada indekseerija** päringuga.

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2015-02-28 
    api-key: admin-key

Saate kohandada mitme aspektide indekseerija käitumine, nagu paketi suurus ja kui palju dokumente saate vahele enne mõne indekseerija käivitamine nurjub. Lisateavet leiate teemast [Loomine indekseerijat API](https://msdn.microsoft.com/library/azure/dn946899.aspx).
 
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

Ajaloo täitmise sisaldab kõige viimati lõplikus täitmised, mis on pööratud ajalises järjestuses sorditud (nii, et uusima täitmise jõuab vastusesse) kuni 50. Lisateavet vastuse leiate [Registraatori olekut hankimine](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Käivitage indexers ajakava

Samuti saate korraldada indekseerija käivitamiseks perioodiliselt ajakava. Selleks lisada loomisel või selle indekseerija värskendamise **ajakava** atribuudi. Järgmises näites on kuvatud panna taotluse selle indekseerija värskendamiseks:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

**Intervall** parameeter on nõutav. Intervalli viitab kaks järjestikust indekseerija täitmised alguse vahelise aja. Väikseim lubatud intervall on 5 minuti jooksul; pikima on üks päev. See peab vormindatud XSD-"dayTimeDuration" väärtus (piiratud Alamhulk on [ISO 8601 kestus](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) -väärtus). See muster on: `P(nD)(T(nH)(nM))`. Näited: `PT15M` iga 15 minuti `PT2H` iga 2 tundi.

Valikuline **startTime** näitab, et kui kavandatud täitmised alustaks. Kui see on välja jäetud, kasutatakse praeguse UTC aega. Praegu saab varem – juhul esimese täitmise kavandatakse kui selle indekseerija peab töötama pidevalt, kuna selle startTime.  

Korraga saab käitada ainult ühte täitmise indekseerija. Kui indekseerija töötab täitmise on ajastatud, täitmise edasi järgmise määratud aja.

Vaatame näiteks teha konkreetseid. Oletame, et saaksime järgmine kord tunnis ajakava, mis on konfigureeritud: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Siin on, mis juhtub: 

1. Esimese indekseerija täitmise käivitab või umbes 1 märts 2015 12.00 UTC.
1. Oletame, et see täitmise võtab 20 minutit (või mis tahes ajal väiksem kui 1 tund).
1. Teine täitmise käivitab või umbes 1 märts 2015 01:00:00 
1. Nüüd Oletame, et see täitmise võtab rohkem kui üks tund – näiteks 70 minutit – nii, et see on lõpule jõudnud umbes 2:10:00
1. See on nüüd 2 kella, kord kolmanda täitmise alustamiseks. Siiski, kuna teine täitmise kella 1 töötab, kolmanda täitmise vahele. Kolmanda täitmise algab 3: 00

Saate lisada, muuta või kustutada olemasolevate indekseerija ajakava **sellele indekseerija** taotluse abil. 

<a name="CaptureChangedRows">, /a >
## <a name="capturing-new-changed-and-deleted-rows"></a>Hõivamine uued, muudetud ja kustutatud read

Kui teie tabelil on palju ridu, kasutage andmete muutmine tuvastamise poliitika. Muuta tuvastamine võimaldab ilma uuesti indekseerimine kogu tabel on tõhusa otsinguga ainult uusi või muudetud ridu.

### <a name="sql-integrated-change-tracking-policy"></a>SQL-i integreeritud muutuste jälituse poliitika

Kui SQL-andmebaasi toetab [muutuste jälituse](https://msdn.microsoft.com/library/bb933875.aspx), soovitame **SQL integreeritud muutmine jälgimise poliitika**. See on kõige tõhusam poliitika. Lisaks võimaldab Azure'i otsingu tuvastamiseks kustutatud read pole vaja konkreetsete "pehmed Kustuta" veeru lisamine tabelisse.

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

<a name="HighWaterMarkPolicy"></a>
### <a name="high-water-mark-change-detection-policy"></a>Kõrge vee märgi muuta tuvastamine poliitika

Kui SQL-i integreeritud muutuste jälitamise poliitika on soovitatav, seda saab kasutada ainult tabelitega, mitte vaated. Kui kasutate vaadet, kaaluge kõrge vee märgi poliitika. Selle poliitika saab kasutada, kui teie tabelit või vaadet, sisaldab veerg, mis vastab järgmistele tingimustele:

- Lisab kõik määrama veeru väärtuse. 
- Kõik värskendused üksuse muuta ka veeru väärtuse. 
- Selle veeru väärtus suureneb iga muudatuse.
- Ühispäringu abil järgmine kus ja ORDER BY klauslitest saab teostada tõhus: `WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`.

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

> [AZURE.WARNING] Kui lähtetabel ei ole registri kõrge vee märkeruuduveerus, võib päringud kasutavad SQL-i indekseerija ajalõpuni. Eelkõige on `ORDER BY [High Water Mark Column]` klausel nõuab registri tõhus käivitamiseks, kui tabel sisaldab palju ridu. 

Kui teil ilmneb ajalõpp tõrkeid, saate kasutada funktsiooni `queryTimeout` indekseerija Otsingukonfiguratsiooni säte määramiseks väärtuse, mis on suurem kui vaikimisi 5-minutilise ajalõpp päringu ajalõpp. Näiteks määratud aeg, mille 10 minuti, loomine või värskendamine selle indekseerija pärast konfiguratsioon. 

    {
      ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

Samuti saate keelata selle `ORDER BY [High Water Mark Column]` klausel. Aga see pole soovitatav, sest kui indekseerija täitmise on katkenud tõrge, selle indekseerija uuesti töötlemine kõik read kui hiljem – see töötab ka siis, kui selle indekseerija on juba töödeldud peaaegu kõik read ajaks see katkes. Keelamine on `ORDER BY` -klauslis, kasutage funktsiooni `disableOrderByHighWaterMarkColumn` seadmine indekseerija määratluse:  

    {
     ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Pehmete kustutamine veeru kustutamise tuvastamise poliitika

Ridade kustutamisel tabelist Allika tõenäoliselt soovite nende ridade kustutamiseks need otsinguregistrisse. Kui kasutate SQL-i integreeritud jälituse poliitika, see on tehtud eest teie eest. Siiski kõrge vee märgi jälituse poliitika ei aidata teil kustutatud read. Mida teha? 

Kui read on viidud tabelist, on Azure otsingu kuidagi tuletamine olemasolu kirjeid, mida pole enam olemas.  Siiski saate "sujuvate Kustuta" tehnika loogiliselt Kustuta read, ilma neid eemaldamata tabelist. Veeru lisamiseks oma tabeli või vaate ja märgi ridu kustutada selle veeru abil.

Kustuta sujuvate tehnika kasutamisel saate määrata selle sujuvate kustutamine poliitika järgmiselt loomise või värskendamisel andmeallika: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

**SoftDeleteMarkerValue** peab olema string – kasutage stringi kujutis oma tegelik väärtus. Näiteks kui teil on mõni täisarv veerg, kuhu kustutatud read on märgitud väärtus 1, kasutada `"1"`. Kui teil on BITISE veeru, kus kustutatud read on märgitud loogikaväärtus true, kasutage `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>SQL-andmetüübid ja Azure otsingu andmetüübid kaardistamine

|SQL-andmetüüp | Lubatud target index Väljatüübid |Märkmete 
|------|-----|----|
|bit|Edm.Boolean Edm.String| |
|int, smallint, tinyint |Edm.Int32, Edm.Int64, Edm.String| |
| suur täisarv | Edm.Int64 Edm.String | |
| hõljumine Real, |Edm.Double Edm.String | |
| smallmoney raha komakohtade arv | Edm.String| Azure'i otsing ei toeta kümnendarvu tüüpi teisendamisel Edm.Double, kuna see kaotaks täpsus |
| char nchar, varchar, nvarchar | Edm.String<br/>Collection(EDM.string)|SQL-string saab asustamiseks Collection(Edm.String) välja, kui stringi tähistab JSON massiiv stringide:`["red", "white", "blue"]` | 
|smalldatetime, kuupäev ja kellaaeg, datetime2, kuupäeva, datetimeoffset |Edm.DateTimeOffset Edm.String| |
|uniqueidentifer | Edm.String | |
|geograafia | Edm.GeographyPoint | Toetatakse ainult geograafia eksemplari tüüp punkti SRID 4326 (vaikesäte) | | 
|ROWVERSION| N/A |Rea-veerud ei saa otsinguregister talletada, kuid neid saab kasutada muutuste jälituse | |
| aeg, kuuline ajavahemik, kahendarvuks, varbinaar, image, xml, geomeetria, CLR-i andmetüübid | N/A |Pole toetatud |

## <a name="configuration-settings"></a>Sätete konfigureerimine

SQL-i indekseerija seab mitu konfiguratsiooni sätted: 

|Säte |Andmetüüp | Otstarve | Vaikeväärtus |
|--------|----------|---------|---------------|
| queryTimeout | string | Määrab jaoks SQL-päringu käivitamine ajalõpp | 5 minutit ("00: 05:00") |
| disableOrderByHighWaterMarkColumn | bool | Põhjustab SQL-päringu kasutatavaid kõrge vee märgi poliitika klausel ORDER BY jätta. Vt [kõrge vee märgi poliitika](#HighWaterMarkPolicy) | FALSE | 

Nende sätete kasutatakse funktsiooni `parameters.configuration` objekti indekseerija määratlus. Näiteks 10 minuti soovite seada päringu ajalõpp, loomine või värskendage selle indekseerija pärast konfiguratsioon. 

    {
      ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

**Q:** Saate kasutada Azure SQL-i indekseerija IaaS VMs Azure SQL-i andmebaaside?

V: Jah. Siiski, peate esmalt lubama oma otsinguteenuse oma andmebaasiga ühenduse loomiseks. Lisateavet leiate teemast [konfigureerimine ühenduse SQL serveriga on Azure VM indekseerija Azure'i otsingu kaudu](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).


**Q:** Saate kasutada Azure SQL-i indekseerija töötab asutusesisese SQL-andmebaase? 

V: me soovitame või toetavad selline, nagu seda teha nõuaks saate avada oma Interneti-liikluse andmebaase. 

**Q:** Saate kasutada Azure SQL-i indekseerija peale töötab IaaS Azure SQL serveri andmebaasi? 

V: me ei toeta seda stsenaariumi, kuna me ei ole testinud indekseerija mis tahes andmebaasidega peale SQL serveri.  

**Q:** Saate luua mitme indexers ajakava töötab? 

V: Jah. Siiski ainult üks indekseerija saate töötama ühe sõlme korraga. Kui teil on vaja töötab üheaegselt mitu indexers, kaaluge ülespoole oma otsinguteenuse Otsi rohkem kui ühe üksuse. 

**Q:** Töötab indekseerija mõjutab päringu töökoormus? 

V: Jah. Indekseerija üks sõlmed sisse oma otsinguteenus töötab ja indekseerimise ja päringu liikluse ja muude API kutsed jagatud selle sõlme ressursid on. Kui käivitate intensiivse indekseerimine ja päringu töökoormus ja ilmneda 503 tõrgete või suurenevad vastuse korda kõrge, kaaluge oma otsinguteenuse ülespoole.
