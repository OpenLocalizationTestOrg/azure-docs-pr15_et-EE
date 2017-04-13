<properties 
pageTitle="Indekseerija toimingud (Azure'i otsingu teenuse REST API: 2015 02 28 eelvaates) | Azure'i otsingu eelvaade API" 
description="Indekseerija toimingud (Azure'i otsingu teenuse REST API: 2015-02-28-eelvaade)" 
services="search" 
documentationCenter="" 
authors="chaosrealm" 
manager="pablocas"
editor="" />

<tags 
ms.service="search" 
ms.devlang="rest-api" 
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na" 
ms.date="09/07/2016" 
ms.author="eugenesh" />

#<a name="indexer-operations-azure-search-service-rest-api-2015-02-28-preview"></a>Indekseerija toimingud (Azure'i otsingu teenuse REST API: 2015-02-28-eelvaade)#

> [AZURE.NOTE] Selles artiklis kirjeldatakse indexers [2015 02 28 eelvaates REST API](search-api-2015-02-28-preview.md). See API versioon lisab eelvaate versiooni Azure'i bloobimälu indekseerija koos dokumendi eraldamine ja Azure Tabelimäluga indekseerija pluss muud täiustused. API toetab samuti üldiselt kättesaadav (GA) indexers, sh indexers Azure'i SQL-andmebaasi, SQL Server Azure'i VMs ja Azure'i DocumentDB.

## <a name="overview"></a>Ülevaade ##

Azure'i otsing saate integreerida otse mõned levinud andmeallikate eemaldamist vajate oma andmed koodi kirjutamiseks. Saate häälestada selle üles, saate helistada Azure'i otsingu API luua ja hallata **indexers** ja **andmeallikate**. 

Mõne **indekseerija** on ressurss, mis ühendab andmeallikate target otsingu registrid. Indekseerija kasutatakse järgmiselt: 

- Ühekordse andmetega asustada registri koopia teha.
- Registri ja ajakava andmeallika muudatused sünkroonida. Graafik on osa indekseerija määratlus.
- Autonoomsest nõudmisel registri värskendamiseks vastavalt vajadusele. 

Mõne **indekseerija** on kasulik, kui soovite registri tavaline värskendused. Saate selle indekseerija määratluse osana on Tekstisisese ajakava seadistamine või käivitage see nõudmisel [Käivitada indekseerija](#RunIndexer)abil. 

**Andmeallika** saate määrata, milliseid andmeid on vaja indekseerida, identimisteabe andmed ja poliitikate Azure'i otsingu tõhus tuvastamiseks muudatuste andmeid (nt muuta või kustutada andmebaasi tabeli ridade) juurde pääseda. See on määratletud on sõltumatu ressurss, et seda saab kasutada mitut indexers.

Praegu toetatakse järgmiste andmeallikatega:

- **Azure'i SQL-andmebaas** ja **SQL Server Azure'i VMs**. Suunatud walk-through, leiate [artiklist](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md). 
- **Azure'i DocumentDB**. Suunatud walk-through, leiate [artiklist](../documentdb/documentdb-search-indexer.md). 
- **Azure'i bloobimälu**, sh dokumendi failivormingud: PDF-i, Microsoft Office (DOCX/dokumenti, XSLX/XLS, PPT/PPTX-faili, sõnum), HTML, XML, ZIP ja lihtteksti failid (sh JSON). Suunatud walk-through, leiate [artiklist](search-howto-indexing-azure-blob-storage.md).
- **Azure'i Tabelimälu**. Suunatud walk-through, leiate [artiklist](search-howto-indexing-azure-tables.md).
     
Me arutate, lisamise tugi täiendavad andmeallikate tulevikus. Aidake meil tähtsuse neid otsuseid, esitage [Azure'i otsingu tagasiside Foorum](http://feedback.azure.com/forums/263029-azure-search)tagasisidet.

Lugege teemat [Teenuste piirangud](search-limits-quotas-capacity.md) ülempiirid seotud indekseerija ja andmete allikas ressursid.

## <a name="typical-usage-flow"></a>Tüüpilised kasutus meilivoo ##

Saate luua ja hallata indexers ja andmeallikate kaudu lihtne HTTP päringuid (Kustuta postituse toomine, pane) suhtes on antud `data source` või `indexer` ressursi.

Kuidas häälestada automaatse indekseerimise on tavaliselt neli järgmist toimingut:

1. Määratlege andmeallikas, mis sisaldab andmeid, mis tuleb indekseerida. Pidage meeles, et Azure'i otsing ei toeta kõiki andmeallika Esita andmetüübid. Loendi vt [toetatud andmetüübid](https://msdn.microsoft.com/library/azure/dn798938.aspx) .

2. Azure'i otsinguregistri kelle skeemi ühildub Andmeallika loomine
  
3. Mõne Azure'i otsingu andmeallika loomine, nagu on kirjeldatud [Andmeallika loomine](#CreateDataSource).
  
4. Looge Azure'i otsingu indekseerija kirjeldatud [Indekseerija loomine](#CreateIndexer).

Plaanite peaks ühe indekseerija target index ja andmete allikas kombinatsioonide loomise kohta. Saate määrata mitu indexers sama registrisse kirjutamine ja saate uuesti kasutada mitut indexers sama andmeallikas. Indekseerija saab kasutada ainult ühe andmeallika korraga ja ainult ühe registrisse kirjutada. 

Pärast indekseerija loomist saate tuua selle toiminguga [Hankida registraatori olekut](#GetIndexerStatus) Andmetäite olek. Samuti saate käivitada indekseerija (asemel või lisaks ajakava perioodiliselt töötavate) igal ajal [Käivitada indekseerija](#RunIndexer) selle toimingu abil.

<!-- MSDN has 2 art files plus a API topic link list -->


## <a name="create-data-source"></a>Andmeallika loomine ##

Azure'i otsing, kasutatakse andmeallika indexers, ühenduse andmeid sihtotstarbelise või ajastatud andmete värskendamine target register. Saate luua uue andmeallika Azure'i otsinguteenuse abil HTTP POST-taotluse sees.
    
    POST https://[service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Teise võimalusena saate kasutada ja määrata andmeallika nimi URI. Kui andmeallika olemas, luuakse see.

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]

**Märkus**: andmeallikate maksimumarv lubatud riigiti hinnad taseme. Tasuta teenus võimaldab kuni 3 andmeallikad. Standard teenus võimaldab 50 andmeallikad. Üksikasjad leiate [Teenuste piirangud](search-limits-quotas-capacity.md) .

**Taotlemine**

HTTPS on vaja kõik hooldustaotlused. **Andmeallika loomine** taotluse saate ehitatakse postituse või pane meetodi abil. POSTITUSE kasutamisel peate sisestama andmeallika nimi koos andmeallika määratluse koosolekukutse kehasse. PANNA, mille nimi on osa URL-i. Kui andmeallikas pole olemas, on loodud. Kui see on juba olemas, värskendatakse uue määratluse. 

Andmeallika nimi peab olema väiketähtedes, käivitage tähe või numbri, on pole kaldkriipsud või punktid ja olema väiksem kui 128 märki. Pärast andmeallika nimi algab tähe või numbri, ülejäänud nimi võib sisaldada mis tahes tähe, arv ja kriipsud, kui kriipsud mittejärjestikuste. Üksikasjad leiate [nimetamise reeglid](https://msdn.microsoft.com/library/azure/dn857353.aspx) .

Funktsiooni `api-version` on nõutav. Praegune versioon on `2015-02-28`.

**Taotluse päised**

Järgmises loendis kirjeldatakse nõutavate ja soovituslike taotluse päised. 

- `Content-Type`: Nõutav. See määramine`application/json`
- `api-key`: Nõutav. Funktsiooni `api-key` kasutatakse otsingu teenust taotluse kinnitamiseks. See on kordumatu teenust stringiväärtus. **Andmeallika loomine** taotluse peab sisaldama mõne `api-key` päise määramine teie administraator võti (erinevalt päringu klahvi). 
 
Samuti peate teenuse ehitada taotluse URL-i nimi. Saate avada teenuse nimi ja `api-key` teenuse armatuurlaua [Azure portaali](https://portal.azure.com/)kaudu. Lugege teemat [loomine otsinguteenuse portaalis](search-create-service-portal.md) lehel navigeerimiseks spikker.

<a name="CreateDataSourceRequestSyntax"></a>
**Koosolekukutse kehasse süntaks**

Taotluse kehatekstis leiduvad andmed allikas määratlus, mis sisaldab tüüpi andmeallika identimisteabe lugeda, andmeid, samuti on valikuline andmeid muuta tuvastamise ja andmete kustutamise tuvastamise poliitika kasutatakse tõhus tuvastamiseks muudetud või kustutatud andmeallika kasutamisel koos regulaarselt ajastatud indekseerija andmed. 

Taotluse last struktureerimine süntaks on järgmine. Valimi taotlus on esitatud selles teemas edasi.

    { 
        "name" : "Required for POST, optional for PUT. The name of the data source",
        "description" : "Optional. Anything you want, or nothing at all",
        "type" : "Required. Must be one of 'azuresql', 'documentdb', 'azureblob', or 'azuretable'",
        "credentials" : { "connectionString" : "Required. Connection string for your data source" },
        "container" : { "name" : "Required. The name of the table, collection, or blob container you wish to index" },
        "dataChangeDetectionPolicy" : { Optional. See below for details }, 
        "dataDeletionDetectionPolicy" : { Optional. See below for details }
    }

Koosolekukutse sisaldab järgmised atribuudid: 

- `name`: Nõutav. Andmeallika nimi. Andmeallika nimi peab ainult sisaldavad väiketähed, numbrit või kriipsud, ei saa alustamine või lõpetamine koos kriipsud ja piirdub 128characters.
- `description`: Valikuline kirjeldus. 
- `type`: Nõutav. Peab olema üks toetatud andmeallikatüüpide:
    - `azuresql`-Azure SQL-andmebaasi või SQL Server Azure'i VMs
    - `documentdb`-Azure'i DocumentDB
    - `azureblob`-Azure'i bloobimälu
    - `azuretable`-Azure'i Tabelimälu
- `credentials`:
    - Nõutav `connectionString` atribuut määrab andmeallika ühendusstring. Ühendusstringi vorming sõltub andmeallika tüüp: 
        - Azure SQL-i, on see tavalisel SQL serveris ühendusstring. Kui kasutate Azure portaali ühendusstringi toomiseks, kasutage funktsiooni `ADO.NET connection string` suvand.
        - DocumentDB, peab olema ühendusstringi järgmises vormingus: `"AccountEndpoint=https://[your account name].documents.azure.com;AccountKey=[your account key];Database=[your database id]"`. Kõik väärtused on nõutav. Need leiate [Azure'i portaalis](https://portal.azure.com/).  
        - Azure'i bloobimälu ja Tabelimälu, on see salvestusruumi konto ühendusstring. Vorming on kirjeldatud [allpool](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/). Nõutav on lõpp-punkti HTTPS-protokolli.  
- `container`, nõutav: andmete registrisse abil saate määrata selle `name` ja `query` atribuudid: 
    - `name`, nõutav:
        - Azure SQL-i: määrab tabelit või vaadet. Saate kasutada skeemi sobivad nimed, näiteks `[dbo].[mytable]`.
        - DocumentDB: saate määrata selle saidikogumi. 
        - Azure'i bloobimälu: määrab salvestusruumi ümbrises.
        - Azure'i Tabelimälu: saate määrata selle tabeli nimi. 
    - `query`, Valikuline:
        - DocumentDB: võimaldab teil määrata päringu, mis lömastab mõnda suvalise JSON dokumendi paigutust üheks tasapinnalise skeem, mis Azure'i otsing saate indekseerida.  
        - Azure'i bloobimälu: võimaldab teil määrata virtuaalse kausta bloobimälu pakendi sees. Näiteks bloobimälu tee `mycontainer/documents/blob.pdf`, `documents` saab kasutada virtuaalse kausta.
        - Azure'i Tabelimälu: võimaldab teil määrata päringu, mis filtreerib ridade imporditava määramine.
        - Azure SQL-i: päringu ei toetata. Kui teil on vaja seda funktsiooni, Palun hääletada [selle päringusoovituste](https://feedback.azure.com/forums/263029-azure-search/suggestions/9893490-support-user-provided-query-in-sql-indexer)
- Valikuline `dataChangeDetectionPolicy` ja `dataDeletionDetectionPolicy` atribuudid on kirjeldatud allpool.

<a name="DataChangeDetectionPolicies"></a>
**Andmete muutmine tuvastamise poliitika**

Andmete muutmine tuvastamise poliitika eesmärk on andmeid muudetud üksuste tõhus tuvastamiseks. Toetatud poliitikate sõltuvad andmeallika tüüp. Järgmistes jaotistes kirjeldatakse iga poliitika. 

***Kõrge vesimärgi tuvastamise poliitika*** 

Selle poliitika abil teie andmeallikas, mis sisaldab veergu või atribuut, mis vastab järgmistele tingimustele:
 
- Lisab kõik määrama veeru väärtuse. 
- Kõik värskendused üksuse muuta ka veeru väärtuse. 
- Selle veeru väärtus suureneb iga muudatuse.
- Filtri punkt, mis sarnaneb järgmise kasutavate päringute `WHERE [High Water Mark Column] > [Current High Water Mark Value]` saab teostada tõhusalt.

Näiteks SQL Azure'i andmeallikad, on indekseeritud kasutamisel `rowversion` veerg on optimaalne testversiooni kasutamiseks koos kõrge vee märgi poliitika. 

Selle poliitika saate määratletud järgmiselt:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
    } 

> [AZURE.NOTE] Andmeallikate DocumentDB kasutamisel tuleb kasutada funktsiooni `_ts` DocumentDB vara. 

> [AZURE.NOTE] Azure'i bloobimälu andmeallikate Azure'i otsingu kasutamisel automaatselt kasutab kõrge vesimärgi muutmine tuvastamise poliitika on bloobimälu viimati muudetud ajatempli; te ei pea sellise poliitika määramiseks.   

***SQL-i integreeritud tuvastamise poliitika***

Kui SQL-andmebaasi toetab [muutuste jälituse](https://msdn.microsoft.com/library/bb933875.aspx), soovitame SQL integreeritud muutmine jälgimise poliitika. Selle poliitika võimaldab kõige tõhusam muutuste jälituse ja võimaldab Azure'i otsingu tuvastamiseks kustutatud read pole vaja on mõne konkreetsete "pehmed Kustuta" veeru oma skeemi.

Integreeritud muutuste jälituse on toetatud, alustades järgmine SQL serveri andmebaasi versiooni: 
- SQL Server 2008 R2, kui kasutate SQL Server Azure'i VMs.
- Azure SQL-i andmebaasi V12, kui kasutate Azure SQL-andmebaas.  

Kui SQL-i integreeritud muutuste jälitamise poliitika, kasutades määratud eraldi andmete kustutamise tuvastamise poliitika – see poliitika on tugi tuvastamise kustutatud read. 

Selle poliitika saab kasutada ainult tabelid; ei saa kasutada vaatega. Peate lubama muutuste jälituse kasutate, enne kui saate selle poliitika tabel. Vaadake [lubamine ja keelamine muutuste jälituse](https://msdn.microsoft.com/library/bb964713.aspx) olevaid juhiseid.    
 
Kui struktureerimine **Andmeallika loomine** taotlus, SQL-i integreeritud muutuste jälituse poliitika saab määrata järgmiselt:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
    }

<a name="DataDeletionDetectionPolicies"></a>
**Andmete kustutamise tuvastamise poliitika**

Andmete kustutamise tuvastamise poliitika eesmärk on kustutatud andmete tõhus tuvastada. Praegu on ainus toetatud poliitika on `Soft Delete` poliitika, mis võimaldab tuvastamise põhineva väärtuse kustutatud üksuste lisamine `soft delete` veeru- või andmeallika atribuut. Selle poliitika saate määratletud järgmiselt:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the column that specifies whether a row was deleted", 
        "softDeleteMarkerValue" : "the value that identifies a row as deleted" 
    }

**Märkus:** Ainult veergude stringi, täisarv või kahendmuutujaga väärtused on toetatud. Väärtus, mida kasutatakse `softDeleteMarkerValue` peab olema string, isegi siis, kui vastav veerg on täisarvud või loogikaväärtusi. Näiteks kui teie andmeallikas kuvatakse väärtus 1, kasutage `"1"` nimega soovitud `softDeleteMarkerValue`.    

<a name="CreateDataSourceRequestExamples"></a>
**Kehateksti näited**

Kui kavatsete kasutada andmeallika indekseerija, mis käivitatakse, selles näites on kujutatud muutmine ja kustutamine tuvastamise poliitika määramine: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy", "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }

Kui kavatsete kasutada andmeallika andmete ühekordse koopia, saate välja jätta poliitikaid:

    { 
        "name" : "asqldatasource",
        "description" : "anything you want, or nothing at all",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" }
    } 

**Vastus**

Eduka taotlemise: 201 loodud. 

<a name="UpdateDataSource"></a>
## <a name="update-data-source"></a>Andmeallika värskendamine ##

Saate värskendada abil HTTP sellele taotluse olemasolevas andmeallikas. Andmeallika värskendamine taotluse URI nime määramine

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Funktsiooni `api-version` on nõutav. Praegune versioon on `2015-02-28`. [Azure'i otsingu Versioonimine](https://msdn.microsoft.com/library/azure/dn864560.aspx) on üksikasjad ja alternatiivne versioonide kohta lisateavet.

Funktsiooni `api-key` peab olema mõne administraatori võti (erinevalt päringu klahvi). Vaadake [Otsingu teenuse REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Lisateavet klahvid jaotises autentimine. [Loo otsinguteenuse portaalis](search-create-service-portal.md) selgitatakse, kuidas saada teenuse URL-i ja tootenumbri taotluse kasutatakse atribuutide.

**Taotlemine**

Koosolekukutse kehasse süntaks on sama mis [Andmeallika loomine taotlused](#CreateDataSourceRequestSyntax).

> [AZURE.NOTE] Osa atribuute ei saa värskendada olemasolevas andmeallikas. Näiteks ei saa muuta olemasoleva andmeallika tüüp.  

> [AZURE.NOTE] Kui te ei soovi muuta olemasoleva andmeallika ühendusstring, saate määrata sõnasõnaline `<unchanged>` jaoks ühendusstring. Sellest on abi olukordades, kus peate värskendama andmed allikas, kuid pole mugavat juurdepääsu ühendusstringi, kuna see on turvalisus tundlik teave.

**Vastus**

Eduka taotlemise: 201 loodud kui uus andmeallikas on loodud ja 204 sisu puudub kui värskendati olemasolevas andmeallikas.

<a name="ListDataSource"></a>
## <a name="list-data-sources"></a>Loendis andmeallikad ##

**Andmeallikate loendi** toiming tagastab oma Azure'i otsinguteenuse andmeallikate loendi. 

    GET https://[service name].search.windows.net/datasources?api-version=[api-version]
    api-key: [admin key]

Funktsiooni `api-version` on nõutav. Praegune versioon on `2015-02-28`. 

Funktsiooni `api-key` peab olema mõne administraatori võti (erinevalt päringu klahvi). Vaadake [Otsingu teenuse REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Lisateavet klahvid jaotises autentimine. [Loo otsinguteenuse portaalis](search-create-service-portal.md) selgitatakse, kuidas saada teenuse URL-i ja tootenumbri taotluse kasutatakse atribuutide.

**Vastus**

Eduka taotlemise: 200 OK.

Siin on näide vastuse keha.

    {
      "value" : [
        {
          "name": "datasource1",
          "type": "azuresql",
          ... other data source properties
        }]
    }

Pange tähele, et vastus allapoole just teile huvi atribuudid saate filtreerida. Näiteks, kui soovite ainult andmete allikas nimede loend, kasutage OData `$select` päringu suvand:

    GET /datasources?api-version=205-02-28&$select=name

Sel juhul vastuse eeltoodud näites oleks kuvatakse järgmiselt: 

    {
      "value" : [ { "name": "datasource1" }, ... ]
    }

See on kasulik tehnika läbilaskevõime salvestada, kui teil on palju andmeallikate oma otsinguteenuse.

<a name="GetDataSource"></a>
## <a name="get-data-source"></a>Andmeallika hankimine ##

**Saada andmeallikas** toimingu saab andmeallika määratluse Azure'i otsing.

    GET https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

Funktsiooni `api-version` on nõutav. Praegune versioon on `2015-02-28`. 

Funktsiooni `api-key` peab olema mõne administraatori võti (erinevalt päringu klahvi). Vaadake [Otsingu teenuse REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Lisateavet klahvid jaotises autentimine. [Loo otsinguteenuse portaalis](search-create-service-portal.md) selgitatakse, kuidas saada teenuse URL-i ja tootenumbri taotluse kasutatakse atribuutide.

**Vastus**

Olekukoodi: 200 OK, tagastatakse eduka vastuse ootamine.

[Andmeallika loomine näide taotlusi](#CreateDataSourceRequestExamples)näidetes sarnaneb vastus. 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted", 
            "softDeleteMarkerValue" : "true" }
    }

> [AZURE.NOTE] Määrata selle `Accept` taotluse päis `application/json;odata.metadata=none` kui helistamiseks selle API seda põhjustab `@odata.type` atribuut välja jätta vastus ja te ei saa eristada andmete muutmine ja andmete kustutamise tuvastamise poliitika eri tüüpi. 

<a name="DeleteDataSource"></a>
## <a name="delete-data-source"></a>Andmeallika kustutamine ##

**Andmeallika kustutamine** toimingut eemaldab teie Azure'i otsinguteenuse andmeallikas.

    DELETE https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Kui mis tahes indexers viidata andmeallikas, mis te olete kustutamine, Kustutustoiming endiselt jätkata. Siiski on need indexers üleminek olekusse tõrke korral oma järgmise Käivita.  

Funktsiooni `api-version` on nõutav. Praegune versioon on `2015-02-28`. 

Funktsiooni `api-key` peab olema mõne administraatori võti (erinevalt päringu klahvi). Vaadake [Otsingu teenuse REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Lisateavet klahvid jaotises autentimine. [Loo otsinguteenuse portaalis](search-create-service-portal.md) selgitatakse, kuidas saada teenuse URL-i ja tootenumbri taotluse kasutatakse atribuutide.

**Vastus**

Olekukoodi: 204 sisu puudub, tagastatakse eduka vastuse ootamine.

<a name="CreateIndexer"></a>
## <a name="create-indexer"></a>Indekseerija loomine ##

Saate luua uue indekseerija Azure'i otsinguteenuse abil HTTP POST-taotluse sees.
    
    POST https://[service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Teise võimalusena saate kasutada ja määrata andmeallika nimi URI. Kui andmeallika olemas, luuakse see.

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]

> [AZURE.NOTE] Esimese taseme hinnad riigiti indexers lubatud maksimaalne arv. Tasuta teenus võimaldab kuni 3 indexers. Standard teenus võimaldab 50 indexers. Üksikasjad leiate [Teenuste piirangud](search-limits-quotas-capacity.md) .

Funktsiooni `api-version` on nõutav. Praegune versioon on `2015-02-28`. 

Funktsiooni `api-key` peab olema mõne administraatori võti (erinevalt päringu klahvi). Vaadake [Otsingu teenuse REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Lisateavet klahvid jaotises autentimine. [Loo otsinguteenuse portaalis](search-create-service-portal.md) selgitatakse, kuidas saada teenuse URL-i ja tootenumbri taotluse kasutatakse atribuutide.


<a name="CreateIndexerRequestSyntax"></a>
**Koosolekukutse kehasse süntaks**

Taotluse sisu sisaldab indekseerija määratlus, mis määratleb andmeallika ja target registri indekseerimine, samuti valikuline indekseerimise ajakava ja parameetrid. 


Taotluse last struktureerimine süntaks on järgmine. Valimi taotlus on esitatud selles teemas edasi.

    { 
        "name" : "Required for POST, optional for PUT. The name of the indexer",
        "description" : "Optional. Anything you want, or null",
        "dataSourceName" : "Required. The name of an existing data source",
        "targetIndexName" : "Required. The name of an existing index",
        "schedule" : { Optional. See Indexing Schedule below. },
        "parameters" : { Optional. See Indexing Parameters below. },
        "fieldMappings" : { Optional. See Field Mappings below. },
        "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.  
    }

**Indekseerija ajakava**

Soovi korral saate indekseerija määratleda ajakava. Kui ajakava on olemas, käivituvad selle indekseerija perioodiliselt ühe ajakava. Graafik sisaldab järgmisi atribuute.

- `interval`: Nõutav. Kestus väärtus, mis määrab intervall- või perioodi indekseerija töötab. Väikseim lubatud intervall on 5 minuti jooksul; pikima on üks päev. See peab vormindatud XSD-"dayTimeDuration" väärtus (piiratud Alamhulk on [ISO 8601 kestus](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) -väärtus). See muster on: `"P[nD][T[nH][nM]]"`. Näited: `PT15M` iga 15 minuti `PT2H` iga 2 tundi. 

- `startTime`: Nõutav. Mõne UTC kuupäeva ja kellaaja kui selle indekseerija peab algama töötab. 

**Indekseerija parameetrid**

Indekseerija soovi korral saate määrata mitu parameetrid, mis mõjutavad tema käitumist. Kõik parameetrid on valikulised.  

- `maxFailedItems`: Üksused, mis ei pruugi enne indekseerija run käsitletakse ei saa indekseerida arv. Vaikimisi on 0. Teavet nurjunud üksused on tagastatud [Saada registraatori olekut](#GetIndexerStatus) toiming. 

- `maxFailedItemsPerBatch`: Üksuste arv, mis võib nurjuda iga paketi enne indekseerija run indekseeritud käsitletakse tõrke. Vaikimisi on 0.

- `base64EncodeKeys`: Määrab, kas dokumendi klahvid on base-64-kodeeritud. Azure'i otsing paneb märgid, mis võib olla kohal on dokumendi klahvi piirangud. Siiski oma lähteandmete väärtused võivad sisaldada sobimatuid märke. Kui on vaja selliste väärtuste kuvamiskuju dokumendi klahvid indekseerida, saate määrata selle lipu tõene. Vaikimisi on `false`.

- `batchSize`: Määrab on andmeallikast andmete lugemine ja indekseeritud üksuste arv ühes paketi jõudluse parandamiseks. Vaikimisi sõltub andmeallika tüüp: Azure SQL-i ja DocumentDB 1000 ja Azure'i bloobimälu 10.

**Väli vastendused**

Saate välja vastendused target registris olev muu välja nimi välja nimi andmeallika vastendamiseks. Näiteks kaaluge lähtetabeliga väljaga `_id`. Azure'i otsing ei luba nii, et välja nimeks tuleb allkriipsu, alustades välja nime. Seda saab teha abil soovitud `fieldMappings` atribuut indekseerija järgmiselt: 
    
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 

Saate määrata mitu välja vastendused: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

Nii lähte- ja välja nimed on väiketähed.

<a name="FieldMappingFunctions"></a>
***Välja vastenduse funktsioonid***

Väli vastendused saate kasutada ka muuta lähteväärtuste välja *vastenduse funktsioonide*abil.

Ainult ühe sellise funktsiooni ei toeta praegu: `jsonArrayToStringCollection`. See sõelub välja, mis sisaldab vormindatud JSON massiivi Collection(Edm.String) välja target registris olev string. See on mõeldud kasutamiseks koos Azure SQL-i indekseerija, kuna SQL-i pole kohalikke saidikogumi andmetüüp. Seda saab kasutada järgmiselt: 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

Oletagem näiteks, et väljal Allikas stringi `["red", "white", "blue"]`, siis sihtvälja tüüp `Collection(Edm.String)` on täidetud kolme väärtustega `"red"`, `"white"` ja `"blue"`.

Pange tähele, et selle `targetFieldName` atribuut on vabatahtlik; Kui välja jäetud soovitud `sourceFieldName` väärtust. 

<a name="CreateIndexerRequestExamples"></a>
**Kehateksti näited**

Järgmises näites luuakse indekseerija, mis kopeerib andmete viidatud tabel on `ordersds` andmeallikat soovitud `orders` index ajakavas, mis algab 1 jaan 2015 UTC ja töötab tunnis. Iga indekseerija kutsumise kuvatakse siis, kui mitte rohkem kui 5 üksuste nurjuda iga paketi indekseeritud ja kuni 10 üksuste nurjuda indekseeritud kokku. 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }

**Vastus**

201 loodud eduka taotlus.


<a name="UpdateIndexer"></a>
## <a name="update-indexer"></a>Indekseerija värskendamine ##

Olemasoleva indekseerija HTTP sellele taotluse abil saate värskendada. Taotluse URI värskendada indekseerija nime määramine

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Funktsiooni `api-version` on nõutav. Praegune versioon on `2015-02-28`. 

Funktsiooni `api-key` peab olema mõne administraatori võti (erinevalt päringu klahvi). Vaadake [Otsingu teenuse REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Lisateavet klahvid jaotises autentimine. [Loo otsinguteenuse portaalis](search-create-service-portal.md) selgitatakse, kuidas saada teenuse URL-i ja tootenumbri taotluse kasutatakse atribuutide.

**Taotlemine**

Koosolekukutse kehasse süntaks on sama mis [loomine indekseerija taotlused](#CreateIndexerRequestSyntax).

**Vastus**

Eduka taotlemise: 201 loodud kui uus indekseerija on loodud ja 204 sisu puudub kui olemasolev indekseerija värskendati.


<a name="ListIndexers"></a>
## <a name="list-indexers"></a>Loendi Indexers ##

**Loendi Indexers** toiming tagastab oma Azure'i otsinguteenuse indexers loendit. 

    GET https://[service name].search.windows.net/indexers?api-version=[api-version]
    api-key: [admin key]


Funktsiooni `api-version` on nõutav. Eelvaateversiooni on `2015-02-28-Preview`. [Azure'i otsingu Versioonimine](https://msdn.microsoft.com/library/azure/dn864560.aspx) on üksikasjad ja alternatiivne versioonide kohta lisateavet.

Funktsiooni `api-key` peab olema mõne administraatori võti (erinevalt päringu klahvi). Vaadake [Otsingu teenuse REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Lisateavet klahvid jaotises autentimine. [Loo otsinguteenuse portaalis](search-create-service-portal.md) selgitatakse, kuidas saada teenuse URL-i ja tootenumbri taotluse kasutatakse atribuutide.

**Vastus**

Eduka taotlemise: 200 OK.

Siin on näide vastuse keha.

    {
      "value" : [
      {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        ... other indexer properties
      }]
    }

Pange tähele, et vastus allapoole just teile huvi atribuudid saate filtreerida. Näiteks, kui soovite ainult indekseerija nimede loend, kasutage OData `$select` päringu suvand:

    GET /indexers?api-version=2014-10-20-Preview&$select=name

Sel juhul vastuse eeltoodud näites oleks kuvatakse järgmiselt: 

    {
      "value" : [ { "name": "myindexer" } ]
    }

See on kasulik tehnika läbilaskevõime salvestada, kui teil on palju indexers oma otsinguteenuse.


<a name="GetIndexer"></a>
## <a name="get-indexer"></a>Indekseerija hankimine ##

**Saada indekseerija** toimingu saab Azure'i otsingu indekseerija määratlus.

    GET https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Funktsiooni `api-version` on nõutav. Eelvaateversiooni on `2015-02-28-Preview`. 

Funktsiooni `api-key` peab olema mõne administraatori võti (erinevalt päringu klahvi). Vaadake [Otsingu teenuse REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Lisateavet klahvid jaotises autentimine. [Loo otsinguteenuse portaalis](search-create-service-portal.md) selgitatakse, kuidas saada teenuse URL-i ja tootenumbri taotluse kasutatakse atribuutide.

**Vastus**

Olekukoodi: 200 OK, tagastatakse eduka vastuse ootamine.

Vastus on sarnane [loomine indekseerija näide taotlusi](#CreateIndexerRequestExamples)näited: 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }


<a name="DeleteIndexer"></a>
## <a name="delete-indexer"></a>Indekseerija kustutamine ##

**Indekseerija kustutamine** toimingut eemaldab teie Azure'i otsinguteenuse indekseerija.

    DELETE https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Indekseerija kustutamisel valmimiseni käivitavad indekseerija täitmised poolelioleva sel ajal, kuid pole veel täitmised on ajastatud. Kasutada puudub indekseerija katsete tulemuseks HTTP olekukoodi 404 ei leitud. 
 
Funktsiooni `api-version` on nõutav. Eelvaateversiooni on `2015-02-28-Preview`. 

Funktsiooni `api-key` peab olema mõne administraatori võti (erinevalt päringu klahvi). Vaadake [Otsingu teenuse REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Lisateavet klahvid jaotises autentimine. [Loo otsinguteenuse portaalis](search-create-service-portal.md) selgitatakse, kuidas saada teenuse URL-i ja tootenumbri taotluse kasutatakse atribuutide.

**Vastus**

Olekukoodi: 204 sisu puudub, tagastatakse eduka vastuse ootamine.

<a name="RunIndexer"></a>
## <a name="run-indexer"></a>Käivitage indekseerija ##

Lisaks töötab perioodiliselt ajakava, saate nõudmisel **Käivitada indekseerija** toimingu kaudu kasutada ka indekseerija: 

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [admin key]

Funktsiooni `api-version` on nõutav. Eelvaateversiooni on `2015-02-28-Preview`. 

Funktsiooni `api-key` peab olema mõne administraatori võti (erinevalt päringu klahvi). Vaadake [Otsingu teenuse REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Lisateavet klahvid jaotises autentimine. [Loo otsinguteenuse portaalis](search-create-service-portal.md) selgitatakse, kuidas saada teenuse URL-i ja tootenumbri taotluse kasutatakse atribuutide.

**Vastus**

Olekukoodi: 202 aktsepteeritud tagastatakse eduka vastuse ootamine.

<a name="GetIndexerStatus"></a>
## <a name="get-indexer-status"></a>Saada registraatori olekut ##

**Saada registraatori olekut** toiming toob indekseerija praeguse oleku ja täitmise ajalugu: 

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [admin key]


Funktsiooni `api-version` on nõutav. Eelvaateversiooni on `2015-02-28-Preview`. 

Funktsiooni `api-key` peab olema mõne administraatori võti (erinevalt päringu klahvi). Vaadake [Otsingu teenuse REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Lisateavet klahvid jaotises autentimine. [Loo otsinguteenuse portaalis](search-create-service-portal.md) selgitatakse, kuidas saada teenuse URL-i ja tootenumbri taotluse kasutatakse atribuutide.

**Vastus**

Olekukoodi: 200 OK eduka vastuse ootamine.

Vastuse sisu sisaldab teavet üldine registraatori seisund olekut, viimase indekseerija kutsumise, samuti tehtud indekseerija manamine ajalugu (kui see on olemas). 

Valimi vastuse keha näeb välja umbes järgmine: 

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

**Registraatori olekut**

Registraatori olekut võib olla üks järgmistest väärtustest:

- `running`näitab, et selle indekseerija töötab tavalisel viisil. Pange tähele, et mõned indekseerija täitmised endiselt ei, nii, et see on hea mõte kasutada funktsiooni `lastResult` atribuudi ka. 

- `error`näitab, et selle indekseerija ilmnes tõrge, mida ei saa parandada inimsekkumist. Näiteks andmeallika identimisteabe võib olla aegunud või skeemi andmeallika või target indeks on muutunud server on viis. 

**Indekseerija täitmise tulem**

Mõne indekseerija täitmise tulem sisaldab teavet ühe indekseerija täitmise. Viimane tulemus on uurinud nimega soovitud `lastResult` atribuut registraatori olekut. Teiste tehtud tulemused, kui see on olemas, tagastatakse nimega soovitud `executionHistory` atribuut registraatori olekut. 

Indekseerija täitmise tulem sisaldab järgmised atribuudid: 

- `status`: kuvatakse Andmetäite olekut. Üksikasjad leiate allpool [Registraatori täitmise olekut](#IndexerExecutionStatus) . 

- `errorMessage`: tõrketeade nurjunud täitmiseks. 

- `startTime`: UTC-vormingus see täitmise käivitamisel aeg.

- `endTime`: UTC-vormingus, kui see täitmise lõppes aeg. Selle väärtuseks on seatud täitmine on pooleli.

- `errors`: Üksusetaseme tõrkeid, kui mõni massiivi. Iga kirje sisaldab dokumendi klahvi (`key` atribuudi) ja tõrketeade (`errorMessage` atribuudi). 

- `itemsProcessed`: andmete allikas üksused (nt tabeli read), mis on indekseerija proovitakse registrisse selle käitamise ajal. 

- `itemsFailed`: mis see nurjus üksuste arvu.  
 
- `initialTrackingState`: alati `null` esimese indekseerija täitmiseks kasutatud andmeallikas pole lubatud või kui andmeid muuta jälgimise poliitika. Kui selline poliitika on lubatud, edaspidised täitmised see väärtus näitab väärtus töödelda selle täitmise jälituse esimese (madalam). 

- `finalTrackingState`: alati `null` kui andmeid muuta jälitus poliitika on lubatud kasutatav andmeallikas. Muul juhul tähistab väärtus edukalt töödelda selle täitmise jälituse uusima (suurim). 

<a name="IndexerExecutionStatus"></a>
**Registraatori täitmise olekut**

Registraatori täitmise olekut sisaldab ühe indekseerija täitmise olekut. See võib olla järgmised väärtused.

- `success`näitab, et täitmise indekseerija on lõpule viidud.

- `inProgress`näitab, et täitmise indekseerija on pooleli. 

- `transientFailure`näitab, ei ole mõne indekseerija täitmise. Vt `errorMessage` atribuudi üksikasjad. Tõrge võib või võivad nõuda inimsekkumist parandamiseks - näiteks probleemide skeemi vastuolu andmeallika ja target registri nõuab Kasutajatoiming, kui ajutiste andmete allikas tööseisakute ei. Indekseerija manamine jätkavad kohta ajakava, kui üks on olemas. 

- `persistentFailure`näitab, et selle indekseerija ei ole nii, et nõuab inimsekkumist. Ajastatud indekseerija täitmised Peata. Pärast probleemiga, kasutage lähtestamine indekseerijat API ajastatud täitmised taaskäivitama. 

- `reset`näitab, et selle indekseerija on lähtestatud kõne lähtestamine indekseerijat API (vt allpool). 

<a name="ResetIndexer"></a>
## <a name="reset-indexer"></a>Indekseerija lähtestamine ##

**Lähtesta indekseerija** toiming lähtestab muutuste jälitamise olek seostatud selle indekseerija. See võimaldab teil käivitada nullist uuesti indekseerimine (näiteks siis, kui teie andmete allikas skeemi on muudetud) või muuta selle indekseerija seotud andmeallika andmete muutmine tuvastamise poliitika.   

    POST https://[service name].search.windows.net/indexers/[indexer name]/reset?api-version=[api-version]
    api-key: [admin key]

Funktsiooni `api-version` on nõutav. Eelvaateversiooni on `2015-02-28-Preview`. 

Funktsiooni `api-key` peab olema mõne administraatori võti (erinevalt päringu klahvi). Vaadake [Otsingu teenuse REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Lisateavet klahvid jaotises autentimine. [Loo otsinguteenuse portaalis](search-create-service-portal.md) selgitatakse, kuidas saada teenuse URL-i ja tootenumbri taotluse kasutatakse atribuutide.

**Vastus**

Olekukoodi: 204 sisu puudub eduka vastuse ootamine.

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>SQL-andmetüübid ja Azure otsingu andmetüübid vahel kaardistamine ##

<table style="font-size:12">
<tr>
<td>SQL-andmetüüp</td>  
<td>Lubatud target index Väljatüübid</td>
<td>Märkmete</td>
</tr>
<tr>
<td>bit</td>
<td>Edm.Boolean Edm.String</td>
<td></td>
</tr>
<tr>
<td>int, smallint, tinyint</td>
<td>Edm.Int32, Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>suur täisarv</td>
<td>Edm.Int64 Edm.String</td>
<td></td>
</tr>
<tr>
<td>hõljumine Real,</td>
<td>Edm.Double Edm.String</td>
<td></td>
</tr>
<tr>
<td>smallmoney raha<br>Decimal<br>arvuline
</td>
<td>Edm.String</td>
<td>Azure'i otsing ei toeta kümnendarvu tüüpi teisendamisel Edm.Double, kuna see kaotaks täpsus
</td>
</tr>
<tr>
<td>char nchar, varchar, nvarchar</td>
<td>Edm.String<br/>Collection(EDM.string)</td>
<td>[Välja vastendamise funktsioonid](#FieldMappingFunctions) vt täpsemat teavet muuta stringi veeru lisamine Collection(Edm.String) selles dokumendis</td>
</tr>
<tr>
<td>smalldatetime, kuupäev ja kellaaeg, datetime2, kuupäeva, datetimeoffset</td>
<td>Edm.DateTimeOffset Edm.String</td>
<td></td>
</tr>
<tr>
<td>uniqueidentifer</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>geograafia</td>
<td>Edm.GeographyPoint</td>
<td>Toetatakse ainult geograafia eksemplari tüüp punkti SRID 4326 (vaikesäte)</td>
</tr>
<tr>
<td>ROWVERSION</td>
<td>N/A</td>
<td>Rea-veerud ei saa otsinguregister talletada, kuid neid saab kasutada muutuste jälituse</td>
</tr>
<tr>
<td>aeg, kuuline ajavahemik<br>binaarsed, varbinaar, pildi,<br>XML-i geomeetria CLR-i andmetüübid</td>
<td>N/A</td>
<td>Pole toetatud</td>
</tr>
</table>

## <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Vastenduse JSON andmetüübid ja Azure otsingu andmetüübid ##

<table style="font-size:12">
<tr>
<td>JSON-andmetüüp</td> 
<td>Lubatud target index Väljatüübid</td>
<td>Märkmete</td>
</tr>
<tr>
<td>bool</td>
<td>Edm.Boolean Edm.String</td>
<td></td>
</tr>
<tr>
<td>Kujunduse integraal arvud</td>
<td>Edm.Int32, Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Ujukoma numbrid</td>
<td>Edm.Double Edm.String</td>
<td></td>
</tr>
<tr>
<td>string</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>massiive primitiivne tipib, nt ["a", "b" \ "c"]</td>
<td>Collection(EDM.string)</td>
<td></td>
</tr>
<tr>
<td>Stringide, mis näeb välja kuupäevad</td>
<td>Edm.DateTimeOffset Edm.String</td>
<td></td>
</tr>
<tr>
<td>GeoJSON punkti objektid</td>
<td>Edm.GeographyPoint</td>
<td>GeoJSON on JSON objektide järgmises vormingus: {"tüüp": "Punkt", "koordinaadid": [pikk, Läti]} </td>
</tr>
<tr>
<td>JSON-objektid</td>
<td>N/A</td>
<td>Pole toetatud; Azure'i otsingu toetab praegu ainult lihtsad tüübid ja stringi saidikogumid</td>
</tr>
</table>