<properties 
    pageTitle="DocumentDB hierarhilise ressursi mudeli ja kontseptsioonide | Microsoft Azure'i" 
    description="Lisateavet DocumentDB's hierarhiline mudel andmebaase, saidikogumite, kasutaja määratletud funktsioon (UDF), dokumendid, õiguste haldamiseks ja muud ressursid."
    keywords="Hierarhiline mudel documentdb Azure'i, Microsoft Azure'i"   
    services="documentdb" 
    documentationCenter="" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-hierarchical-resource-model-and-concepts"></a>DocumentDB hierarhilise ressursi mudel ja mõisted

Andmebaasi üksuste DocumentDB haldava edaspidi **ressursid**. Loogika URI identifitseerib kordumatult iga ressursi. Saate suhelda ressursside standard HTTP verbide, taotluse/vastuse päised ja olek koodide kasutamine. 

Selle artikli lugemist, on teil saama vastavad järgmistele küsimustele.

- Mis on DocumentDB's ressursi mudel?
- Mis on määratletud ressursid asemel kasutaja määratletud ressursse?
- Kuidas ressursi adressaate?
- Kuidas toimivad saidikogumid?
- Kuidas toimivad salvestatud toimingute, päästikute ja kasutaja määratletud funktsioonid (UDF)

## <a name="hierarchical-resource-model"></a>Hierarhiline ressursi mudel
Nagu järgmisel joonisel on näidatud, DocumentDB hierarhilise **Ressursi mudel** koosneb ressursid andmebaasi kasutajakontot, iga adresseeritavad loogilise ja ühed URI kaudu. Kui soovite **kanali** selles artiklis on edaspidi kogumi ressursid. 

>[AZURE.NOTE] DocumentDB pakub väga tõhusa TCP-protokolli, mis on ka RESTful oma side mudelit, mis on saadaval [.net-i kliendi SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)kaudu.

![DocumentDB hierarhilise ressursi mudel][1]  
**Hierarhiline ressursi mudel**   

Ressursside töötamise alustamiseks peate [DocumentDB andmebaasi konto loomine](documentdb-create-account.md) Azure tellimuse abil. Andmebaasi konto võib koosneda **andmebaase**, kumbki sisaldab mitut **saidikogumid**, mis sisaldavad omakorda **Salvestatud toimingute, käivitab, UDF-ID, dokumentide** ja seotud **manused** (Eelvaatefunktsioon). Andmebaasi ka seotud **Kasutajad**, igal versioonil **õigused** juurdepääsuks saidikogumid, Salvestatud toimingute, päästikute, UDF-ID, dokumendid ja manused. Kuigi andmebaase, kasutajaid, õiguste ja saidikogumite on süsteemi määratletud ressursse tuntud skeemid, dokumendid ja manused sisaldavad suvalise, kasutaja määratletud JSON sisu.  

|Ressurss   |Kirjeldus
|-----------|-----------
|Andmebaasi konto   |Andmebaasi kontoga on seotud andmebaase ja bloobimälu manuste (Eelvaatefunktsioon) jaoks määratud hulk. Saate luua ühe või mitme andmebaasi kontod Azure tellimuse abil. Lisateabe saamiseks külastage meie [hinnad lehele](https://azure.microsoft.com/pricing/details/documentdb/).
|Andmebaasi   |Andmebaas on loogiline container üle saidikogumid liigendatud dokumendi salvestusruumi. Samuti on kasutajate ümbrises.
|Kasutaja   |Loogilise nimeruumi õiguste ulatuse määramiseks. 
|Õiguste |Luba luba seostatud kasutaja teatud ressursile juurdepääsu.
|Saidikogumi |Kogumi on JSON dokumendid ja seotud JavaScripti rakenduse loogika ümbris. Kogumi on tasustatav üksus, milles [Hind](documentdb-performance-levels.md) on määratud kogumise tulemuslikkuse tase. Saidikogumite saab ühendada ühe või mitme sektsioonid/serveri ja saab skaala toime praktiliselt piiramatu salvestusruum või läbilaskevõime mahtu.
|Salvestatud protseduur   |Rakenduse loogika kirjutatud JavaScript, mis on registreeritud koguda ja ülekandega täita andmebaasimootor jooksul.
|Päästik    |Rakenduse loogika kirjutatud JavaScript käivitada enne või pärast kummagi lisa asendamine või kustutamine toimingut.
|UDF-I    |Rakenduse loogika kirjutatud JavaScript. UDF-ID, mis võimaldavad teil mudel kohandatud päringu tehtemärk ja seetõttu laiendamine core DocumentDB päringukeel.
|Dokumendi   |Kasutaja määratletud (suvalise) JSON sisu. Vaikimisi skeemi pole vaja määratleda ega teisene indeksite vaja kõik dokumendid, mis on lisatud kogumi kohta.
|(Eelvaade) Manuse   |Manuse on teisiti dokument, mis sisaldab viited ja välise bloobimälu/meedia seotud metaandmed. Arendaja saate valida on bloobimälu, haldab DocumentDB või salvestada selle välise bloobimälu teenusepakkuja, nt OneDrive'i, Dropboxi jne. 


## <a name="system-vs-user-defined-resources"></a>Süsteemi vs kasutaja määratletud ressursid
Andmebaasi kontod, andmebaaside, saidikogumite kasutajad, õigused, Salvestatud toimingute, päästikute ja UDF - on fikseeritud skeemi ja nimetatakse süsteemi ressursse. Seevastu dokumendid ja manused ei ole piiranguid skeemi ja kasutaja määratletud ressursid näited. Klõpsake DocumentDB nii süsteem ja kasutaja määratletud ressursid on esitatud ja hallata kirjetena standardile vastavate JSON. Ressursside süsteemi või kasutaja määratletud, on järgmised ühised atribuudid.

> [AZURE.NOTE] Märkus kõik süsteemi loodud ressursi atribuudid on eesliide oma JSON esituse allkriipsu (_).

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Atribuut</strong></p></td>
            <td valign="top"><p><strong>Kasutaja kõigest või süsteemi loodud?</strong></p></td>
            <td valign="top"><p><strong>Otstarve</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Süsteemi loodud</p></td>
            <td valign="top"><p>Süsteemi loodud kordumatu ja hierarhiliste ressursi identifikaator</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Süsteemi loodud</p></td>
            <td valign="top"><p>nõutav loodetav kokkulangevus juhtelemendi ressursi etag</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Süsteemi loodud</p></td>
            <td valign="top"><p>Viimati värskendatud ajatempli ressursi</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Süsteemi loodud</p></td>
            <td valign="top"><p>Kordumatu adresseeritavad URI ressursi</p></td>
        </tr>
        <tr>
            <td valign="top"><p>ID</p></td>
            <td valign="top"><p>Süsteemi loodud</p></td>
            <td valign="top"><p>Kasutaja määratletud kordumatu nimi (sama väärtusega partition klahv) ressursi. Kui kasutaja ID-d ei määra, id on süsteemi genereeritud</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Kaabel kujutis ressursid
DocumentDB mandaadi mis tahes kaitstud laiendid JSON standard- või kodeeringute; See töötab JSON dokumentide tavavaate nõuetele.  
 
### <a name="addressing-a-resource"></a>Ressursi tegelemine
Kõik ressursid on adresseeritavad URI. Ressursi **_self** atribuudi väärtust tähistab suhteline URI ressursi. URI koosneb soovitud /\<kanali\>/ {_rid} tee segmente:  

|Funktsiooni _self väärtus |Kirjeldus
|-------------------|-----------
|/DBS   |Kanali andmebaaside andmebaasi konto
|/DBS/ {dbName}  |Andmebaasi, kelle id sobitamine väärtus {dbName}
|{dbName} /DBS/ /colls/   |Klõpsake jaotises andmebaasi kogumite kanal
|/colls/ /DBS/ {dbName} {collName} |Saidikogumi, kelle id sobitamine väärtus {collName}
|/colls/ /DBS/ {dbName} {collName} / dokumendid    |Kanali dokumentide kogumi alusel
|/DBS/ {dbName} {collName} /colls/ /docs/ {docId}    |Dokumendi ID-sobitamine väärtus {doc}
|{dbName} /DBS/ /users/   |Klõpsake jaotises andmebaasi kasutajate kanal
|/users/ /DBS/ {dbName} {kasutajanimi}   |Kasutaja id sobitamine väärtus {kasutaja}
|/users/ /DBS/ {dbName} {kasutajanimi} / õigused   |Kanali jaotises kasutaja õigused
|/DBS/ {dbName} {kasutajanimi} /users/ /permissions/ {permissionId}    |Õigus, kelle id sobitamine väärtus {õigus}
  
Iga ressursi on kordumatu kasutaja määratletud nimi, id atribuutide kaudu. Märkus: dokumentide, kui kasutaja id, ei määra meie toetatud SDK-d automaatselt genereerida dokumendi kordumatu ID-ga. Id on kasutaja määratletud string, kuni 256 märki, mis on teatud ema ressursile kontekstis kordumatud. 

Iga ressursi on ka on süsteemi genereeritud hierarhilise ressursiidentifikaator (nimetatakse ka mõne RID), mis on saadaval atribuudi _rid kaudu. RIDI kodeeritakse kogu hierarhia antud ressursi ja see on mugav sisemise esitus viitamistervikluse jõustamiseks jaotatud viisil kasutada. RIDI on kordumatu andmebaasi konto ja seda kasutatakse ettevõttesiseselt, DocumentDB tõhusa marsruutimiseks nõudmata rist partition otsingud. Funktsiooni _self ja _rid atribuutide väärtused on nii alternatiivse ja kanoonilise kujutised ressursi. 

DocumentDB REST API-de toetavad marsruutimine taotluste ID-d ja _rid atribuutide ja ressursside lahendamine.

## <a name="database-accounts"></a>Andmebaasi kontod
Ühe või mitme DocumentDB andmebaasi kontod Azure tellimuse abil saate ette valmistada.

Saate [luua ja hallata DocumentDB andmebaasi kontod](documentdb-create-account.md) veebisaidil [http://portal.azure.com/](https://portal.azure.com/)Azure portaali kaudu. Loomise ja haldamise andmebaasi konto jaoks on vaja haldus ja saab teha vaid Azure tellimuse. 

### <a name="database-account-properties"></a>Andmebaasi konto atribuudid
Ettevalmistamise ja andmebaasi konto haldamine saate konfigureerida ja lugeda järgmised atribuudid.  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Atribuudi nimi</strong></p></td>
            <td valign="top"><p><strong>Kirjeldus</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Vormindusühtsuse poliitika</p></td>
            <td valign="top"><p>Atribuudi vaikimisi järjepidevuse taset kõik kogumite oma andmebaasi konto konfigureerimine. Vormindusühtsuse taseme kohta soovi [x-ms-järjepidevuse-tase] taotluse päise abil saate alistada. <p><p>Pange tähele, et selle atribuudi kehtib ainult <i>kasutaja määratletud ressursid</i>. Kõik süsteemi määratletud ressursid on konfigureeritud toetama loeb/päringud tugev järjepidevus.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Luba võtmed</p></td>
            <td valign="top"><p>Need on esmaseid ja teiseseid juhtslaidi ja kirjutuskaitstud tutvustatakse, mida haldus juurdepääsu ressursside andmebaasi konto all.</p></td>
        </tr>
    </tbody>
</table>

Pange tähele, et lisaks ettevalmistamise, konfigureerimise ja Azure portaali konto andmebaasi haldamine, saate ka programmiliselt luua ja hallata DocumentDB andmebaasi kontod, kasutades [Azure DocumentDB REST API-de](https://msdn.microsoft.com/library/azure/dn781481.aspx) kui ka [Kliendi SDK-d](https://msdn.microsoft.com/library/azure/dn781482.aspx).  

## <a name="databases"></a>Andmebaasid
Andmebaasi DocumentDB on üks või mitu saidikogumite ja kasutajate loogilise ümbris, nagu on näidatud järgmisel joonisel. Saate luua mis tahes arvul andmebaaside DocumentDB andmebaasi kasutajakontot pakkumine piirangud kehtivad.  

![Andmebaasi konto ja saidikogumite hierarhilise mudel][2]  
**Andmebaas on loogiline container kasutajatele ja saidikogumid**

Andmebaas võib sisaldada praktiliselt piiramatu dokumendi salvestamiseks liigendatud saidikogumid, mis moodustavad tehingu domeenide jaoks sisalduvad need dokumendid. 

### <a name="elastic-scale-of-a-documentdb-database"></a>Elastne skaala DocumentDB andmebaasi
DocumentDB andmebaas on vaikimisi – alates paar GB SSD tagatud dokumendi salvestamiseks ja ettevalmistatud läbilaskevõime terabait elastne. 

Erinevalt traditsioonilise RDBMS andmebaasi, on rakendatud DocumentDB andmebaasist ühe masina. DocumentDB, kus teie taotlus skaala vajadused kasvab, saate luua veel saidikogumid, andmebaasi või mõlemad. Tõepoolest erinevad esimese poole rakendustes Microsoft on kasutanud DocumentDB tarbija mahus luues äärmiselt mahukas DocumentDB andmebaaside iga kogumite, mis sisaldavad tuhandeliste TB salvestusruumi dokumendi. Saate kasvata või Kahanda andmebaasi lisada või eemaldada saidikogumid rakenduse skaala nõuetele. 

Suvalist arvu saidikogumid sees kehtivad pakkumine andmebaasi loomine Iga saidikogumi on tagatud SSD salvestusruumi ja läbilaskevõime sõltuvalt valitud jõudluse taseme teie eest ette valmistatud.

Andmebaasi DocumentDB on ka kasutajate ümbris. Kasutaja, Lülita sisse on loogiline nimeruumi õiguste kogumi, mis pakub kohandatud autoriseerimine ja juurdepääs kogumite, dokumendid ja manused.  
 
Nagu ka muud ressursid DocumentDB ressursi mudeli, andmebaaside saab luua, asendatakse, kustutatud, lugege või loetletud abil hõlpsasti [Azure'i DocumentDB REST API-d](https://msdn.microsoft.com/library/azure/dn781481.aspx) või mis tahes [Kliendi SDK-d](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB tagab tugev järjepidevuse lugemiseks või päringute metaandmeid, andmebaasi ressursi. Andmebaasi kustutamine automaatselt tagab, et te ei pääse mõnda saidikogumid või sisalduvate kasutajad.   

## <a name="collections"></a>Saidikogumid
DocumentDB saidikogumi on ümbris JSON dokumentide jaoks. Kogumi on ka skaala päringud ja tehingute kogum. 

### <a name="elastic-ssd-backed-document-storage"></a>Elastne SSD tagatud dokumendi salvestamiseks
Kogumi on otseselt elastne - kasvab automaatselt, kui lisate või eemaldate dokumentide ühel vaja. Saidikogumite on loogilised ressursid ja võib hõlmata füüsiliste sektsioonid või serverites. Sektsioonid sees kogumi arvu määrab DocumentDB salvestusruumi suurus ja ettevalmistatud läbilaskevõime oma saidikogumi põhjal. Iga sektsiooni DocumentDB on määratud hulk seotud SSD tagatud salvestusruumi ja on kopeeritud jaoks kõrge kättesaadavus. Sektsiooni haldus haldab täielikult Azure'i DocumentDB ja teil pole keerukate koodi kirjutamiseks ja hallata oma sektsioonid. DocumentDB saidikogumid on **praktiliselt piiramatu** salvestusruum ja läbilaskevõime. 

### <a name="automatic-indexing-of-collections"></a>Automaatse indekseerimise saidikogumid
DocumentDB on tõene skeemi tasuta andmebaasi süsteem. See endale või mis tahes skeemi nõue JSON dokumendid. Dokumentide lisamisel kogumi DocumentDB indeksid automaatselt need ja need on saadaval, saate päringu. Dokumentide automaatse indekseerimise nõudmata skeemi või sekundaarne registrid on klahv võimalus, DocumentDB ja kirjutamine optimeeritud, Lukusta vaba ja log liigendatud index hooldustööd meetoditega on lubatud. DocumentDB toetab püsiv maht väga kiiresti kirjutab ühtsete päringute kandmise ajal. Nii dokumendis kui ka index salvestusruumi kasutatakse iga saidikogumi tarbitud talletamist arvutamiseks. Saate määrata selle salvestusruumi ja jõudluse kompromisse seostatud indekseerimine kogumi indekseerimise poliitika konfigureerimisega. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Indekseerimise poliitika kogumi konfigureerimine
Indekseerimise iga saidikogumi poliitika võimaldab teil teha jõudlus ja salvestusruumi seostatud indekseerimine kompromisse. Järgmised suvandid on saadaval osana indekseerimise konfigureerimine.  

-   Valige, kas selle saidikogumi automaatselt indeksid kõik dokumendid või mitte. Vaikimisi kõigi dokumentide automaatselt registrisse. Saate välja lülitada automaatse indekseerimise ja valikuliselt ainult teatud dokumentide lisamine indeks. Seevastu valikuliselt saate jätta ainult teatud dokumendid. Saate seda saavutada automaatse atribuudi olema tõene või väär indexingPolicy kogumi kohta ja kasutamise ajal, asendades täitvatel dokumendi [x-ms-indexingdirective] taotluse päis.  
-   Valige, kas kaasamiseks või välistamiseks kindlad teed või mustritega dokumentides registrist. Võite saavutada see säte includedPaths ja klõpsake indexingPolicy kogumi excludedPaths vastavalt. Samuti saate konfigureerida selle salvestusruumi ja jõudluse kompromisse jaoks mustrite teatud tee vahemik ja räsi päringuid. 
-   Valida sünkroonse (ühtsete) ja (rongile) asünkroonse register värskendused. Vaikimisi värskendatakse registri sünkroonselt iga lisa, Asenda või Kustuta dokumendi kogumi. See võimaldab päringute au mis loeb dokumendi järjepidevuse samal tasemel. Kuigi DocumentDB on optimeeritud kirjutamine ja toetab püsiv dokumendi kirjutab koos sünkroonse index hooldus ja serveeritakse ühtsete päringuid, saate konfigureerida teatud saidikogumite värskendada oma indeks laisalt. Rongile indekseerimine suurendab põhjalikumaks kirjutamine jõudlus ja sobib eelkõige lugemis-raske saidikogumid hulgi manustamisest stsenaariumid.

Indekseerimise poliitika saab muuta täites panna kogumist. Selle saavutamiseks [Kliendi SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx) [Azure portaali](https://portal.azure.com) või [Azure'i DocumentDB REST API-de](https://msdn.microsoft.com/library/azure/dn781481.aspx)kaudu.

### <a name="querying-a-collection"></a>Päringute kogumik
Dokumentide kogumi sees võib olla suvaline skeeme ja dokumentide kogumi jooksul saate teha päringuid ilma skeemi ega teisene indeksite algusest peale. Selle saidikogumi [DocumentDB SQL-süntaks](https://msdn.microsoft.com/library/azure/dn782250.aspx), mis sisaldab RTF-vormingus hierarhilise, relatsiooniline, ja ruumilise tehtemärgid ja laiendatavuse kaudu JavaScript põhinev UDF-ID abil saate teha päringuid. JSON grammatika võimaldab modelleerimine JSON dokumentide puude siltidega puu sõlmed nimega. See on ära kasutada nii DocumentDB's automaatse indekseerimise meetodite abil kui ka DocumentDB's SQL-i keel. DocumentDB päringukeele koosneb kolmest peamised aspektid.   

1.  Väike hulk päringu toimingute kohta, mida puustruktuuris, sh hierarhilise päringud ja prognooside loomulikult vastendada. 
2.  Relatsiooniline toimingud, sh koosseis, filter, prognooside, agregaadid ja ise ühenduste alamhulga. 
3.  Puhas JavaScripti vastavalt UDF-ID, mis töötavad (1 ja 2).  

DocumentDB päringu mudeli püüab tasakaalustada funktsioone, tõhusust ja lihtsam. DocumentDB andmebaasimootor algupäraselt kogub ja käivitab päringu SQL-lausete. Kogumi [Azure'i DocumentDB REST API-d](https://msdn.microsoft.com/library/azure/dn781481.aspx) või mis tahes [Kliendi SDK-d](https://msdn.microsoft.com/library/azure/dn781482.aspx)abil saate teha päringuid. .NET SDK kaasas LINQ pakkuja.

> [AZURE.TIP] Saate proovida DocumentDB ja käivitage SQL-päringud meie andmekomplekti [Päring mänguväljak](https://www.documentdb.com/sql/demo)suhtes.

### <a name="multi-document-transactions"></a>Mitme dokumendi tehinguid
Andmebaasitoiminguid ette turvalised ja prognoositavad programmeerimise mudeli samaaegseid muudatusi andmetega. RDBMS, traditsiooniline nii kirjutamist äriloogika on **Salvestatud toimingute** ja/või **päästikute** kirjutamine ja saatmiseks andmebaasiserveri selgituseks täitmiseks. RDBMS, rakenduse programmeerija peab tegelema kahe eri programmeerimise keelte: 

- (Kanneteväline) rakenduse programmeerimiskeele (nt JavaScript, Python, C#, Java jne)
- T-SQL-i, selgituseks programmeerimiskeel, mis on algupäraselt sooritatud andmebaas

Alusel selle tõsiselt pühendunud JavaScript ja JSON otse andmebaasimootor, pakub DocumentDB intuitiivne programmeerimise mudelit, mis otse sisse saidikogumid salvestatud toimingute ja päästikute vastavalt JavaScripti rakenduse loogika teostamiseks. See võimaldab mõlemat järgmistest:

- Tõhus rakendamine kokkulangevus juhtida, taastamine automaatse indekseerimise JSON objekti graafikute otse andmebaasimootor
- Juhtelemendi voog loomulikult väljendada, muutuv ulatuse määramine, ülesande ja integreerimise erandi töötlemise primitiivid koos andmebaasitoiminguid otse programmeerimiskeele JavaScripti osas

JavaScripti loogika registreeritud saidikogumi tasemel, siis saate välja antud saidikogumi andmebaasi tehtavad toimingud dokumendid. DocumentDB peidetult murtakse vastavalt JavaScripti salvestatud toimingute ja käivitab sees on ümbritseva HAPPE tehingud hetktõmmise eraldamise kogu kogumi dokumendid. Täitmise käigus, kui JavaScripti põhjustab erandi, siis kogu tehing on katkestatud. Tulemuseks programmeerimise mudeli on väga lihtne veel võimsaid. JavaScripti arendajad saada kasutamisel endiselt oma tuttavad keele importida ja teegi primitiivid "püsival" programmeerimise mudel.   

JavaScripti täitma otse andmebaasimootor sama nimega puhvri pargi aadressiruumi võimaldab kiire ja selgituseks täitmise andmebaasi toimingute suhtes dokumentide kogumi. Lisaks DocumentDB andmebaasimootor teeb selle JSON tõsiselt pühendunud ja JavaScripti kõrvaldab mis tahes impedantsi lahknevuse tüüpi süsteemid rakenduse ja andmebaasi vaheline.   

Pärast kogumi loomist saate registreerida salvestatud toimingute, päästikute ja UDF-ID koguda [Azure'i DocumentDB REST API-d](https://msdn.microsoft.com/library/azure/dn781481.aspx) või mis tahes [Kliendi SDK-d](https://msdn.microsoft.com/library/azure/dn781482.aspx)abil. Pärast registreerimist saate viidata ja neid käivitada. Kaaluge kirjutatud täiesti JavaScript, allpool kood on kaks argumenti (nimi ja autori nime) ja loob uue dokumendi, dokumendi päringute ja seejärel värskendab see – kõik, peidetud HAPPE tehingust talletatud järgmist. Mis tahes hetkel täitmise ajal, kui JavaScript erand Expression.Error, kogu tehing käsk.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()
            
        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);
                
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);
                        
                        context.getResponse().setBody(matchingDocuments.length);
                       
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

Kliendi saate "saata" ülaltoodud JavaScripti loogika andmebaasi selgituseks täitmise HTTP postiga. HTTP meetodite kasutamise kohta leiate lisateavet teemast [rahulik kasutusviisid DocumentDB ressurssidega](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


Teate, et kuna andmebaasi mõistab algupäraselt JSON- ja JavaScripti, on pole süsteemi tüübilahknevuse, pole "OR vastenduse" või koodi genereerimine maagiline nõutav.   

Salvestatud toimingute ja käivitab suhelda kogumi ja dokumentide kogumi kaudu täpselt määratletud objektimudel, mis seab saidikogumi praeguses kontekstis.  

Saidikogumite DocumentDB sisse saab luua, kustutada, lugeda või loetletud hõlpsalt kasutada [Azure DocumentDB REST API-d](https://msdn.microsoft.com/library/azure/dn781481.aspx) või mis tahes [Kliendi SDK-d](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB pakub alati tugev järjepidevuse lugemiseks või päringute metaandmeid, kogumi. Kustutamise kogumi automaatselt tagab ei pääse mõni dokumente, manused, Salvestatud toimingute, päästikute, ja UDF sisalduvate.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Salvestatud toimingute, päästikute ja kasutaja määratletud funktsioonid (UDF)
Eelmises jaotises kirjeldatud kirjutamise rakenduse loogika käivitamiseks otse tehingu andmebaasimootor sees. Rakenduse loogika saate kirjutatud täiesti JavaScript ja saab modelleerida salvestatud protseduur, päästik või mõne UDF. JavaScripti koodi salvestatud toimingu või käivitamiseks lisada, asendada, kustutada, lugemis- või päringu dokumentide kogumi. Teisalt, ei saa JavaScripti sees on UDF-i lisamine, asendamine või dokumentide kustutamine. UDF-ID nummerdada on päringu tulemite hulk dokumendid ja aedvili teise tulemite hulk. Jaoks mitme rendiõigus, jõustab DocumentDB range broneerimiseks vastavalt ressursside juhtimine. Iga salvestatud protseduur, käivitada või mõne UDF saab fikseeritud quantum operatsioonisüsteemi ressursside oma tööd teha. Lisaks salvestatud toimingute, käivitab või UDF-ID ei saa link välise JavaScripti teekide suhtes ja on musta nimekirja, kui need ületavad nende ressursside neile määratud. Saate registreerida, unregister salvestatud toimingute, käivitab või UDF kogumi, kasutades REST API-d.  Registreerimisel on salvestatud protseduur, päästik või mõne UDF eelnevalt koostatud ja salvestatud bait-koodi, mida saab hiljem täidetud. Järgmises jaotises illustreerimine kasutamist DocumentDB JavaScripti SDK registreerida, käivitada ja unregister salvestatud protseduur, päästik ja soovitud UDF-i. JavaScripti SDK on lihtne ümbris [DocumentDB REST API-d](https://msdn.microsoft.com/library/azure/dn781481.aspx). 

### <a name="registering-a-stored-procedure"></a>Registreerimisel salvestatud protseduur
Salvestatud protseduur registreerimise loob uue ressursi salvestatud protseduur kogumi HTTP postiga.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();
            
            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };
    
    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>Salvestatud protseduuri käitamisel
Salvestatud toimingu teostamine tehakse väljastanud mõne HTTP POST vastu mõne olemasoleva salvestatud protseduur ressursi saates parameetrid protseduuri koosolekukutse kehasse.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Salvestatud protseduur registreerimise tühistamine
Registreerimise tühistamine salvestatud protseduur tehakse lihtsalt väljastanud HTTP kustutada mõne olemasoleva salvestatud protseduur ressursside suhtes.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Enne päästik registreerimine
Registreerimise käivitamiseks on tehtud uue ressursi päästik HTTP postiga kogumi loomine. Saate määrata, kui käivitamiseks on mõne eelnevalt või postituse päästik ja toimingu tüüp võib olla seostatud (nt loomine, Asenda, kustutamine või kõik).   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }
    
    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Enne päästik täitmine
Täitmise käivitamiseks on töö lõpetanud, määrates lisamispäästiku olemasoleva nime postituse/panna/Kustuta taotluse dokumendi ressursi kaudu taotluse header väljastamise ajal.  
 
    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Enne päästik registreerimise tühistamine
Registreerimise tühistamine käivitamiseks toimub lihtsalt emissiooni HTTP kustutada mõne olemasoleva päästik ressursside suhtes.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Soovitud UDF registreerimine
Registreerimise on UDF tehakse loomine UDF uue ressursi kogumi HTTP postiga.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-the-query"></a>Soovitud UDF osana päringu käivitamisel
Soovitud UDF saab määrata SQL-päringu osana ja seda kasutatakse nii, et laiendada core [SQL-i päring keele DocumentDB](https://msdn.microsoft.com/library/azure/dn782250.aspx).

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Soovitud UDF registreerimise tühistamine 
Registreerimise tühistamine on UDF lihtsalt valmis väljastanud HTTP kustutada mõne olemasoleva UDF-i ressursikeskuse suhtes.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Kuigi ülaltoodud Koodilõigud ilmnes registreerimise (POSTITA), registreeringu tühistamine (pane), lugege/loendi (saada) ja täitmise (POSTITA) kaudu [DocumentDB JavaScripti SDK](https://github.com/Azure/azure-documentdb-js), saate ka [REST API-d](https://msdn.microsoft.com/library/azure/dn781481.aspx) või muud [Kliendi SDK-d](https://msdn.microsoft.com/library/azure/dn781482.aspx). 

## <a name="documents"></a>Dokumentide
Saate lisada, asendada, kustutada, lugemine, loetlemine ja päringu suvalise JSON dokumentide kogumi. DocumentDB kohustavad teid mis tahes skeemi ja ei nõua teisene registrid toetamiseks päringute üle dokumentide kogumi.   

Olles tõeliselt avatud andmebaasi teenus, DocumentDB leiutada eriotstarbelisi andmetüüpide (nt kuupäev aja järgi) või teatud kodeeringute JSON dokumentide jaoks. Pange tähele, et ei nõua DocumentDB mis tahes teisiti JSON põhimõtted kodifitseerida seosed erinevaid dokumente; DocumentDB SQL-süntaks pakub väga võimas hierarhilise ja seoselisi päringu tehtemärgid päringu ja projekti dokumentide ilma teisiti marginaalid või vajate kodifitseerida seoseid dokumentide abil eristada atribuudid.  
 
Kui muud ressursid dokumentide loomist, asendatud, kustutatud, vt, nummerdatud ja päringu abil hõlpsasti REST API-d või mis tahes [Kliendi SDK-d](https://msdn.microsoft.com/library/azure/dn781482.aspx). Dokumendi kustutamine kohe vabastada kvoodi vastav kõik pesastatud manused. Lugege järjepidevuse taseme dokumentide järgib järjepidevuse poliitika andmebaasi kontol. See poliitika võib kaalu kohta – soovi sõltuvalt rakenduse järjepidevuse nõuded. Kui päringu dokumente, lugege järjepidevuse järgib indekseerimise režiimi kogumise seadmine. Jaoks "järjepidevuse" see järgib konto järjepidevuse poliitika. 

## <a name="attachments-and-media"></a>Manuste ja meedia
>[AZURE.NOTE] Manuse ja meedia ressursid on eelvaade funktsioonid.
 
DocumentDB saate talletada kahendarvu plekid/media DocumentDB või oma meediumide poest. Lisaks võimaldab metaandmeid, meedia osas teisiti dokument nimega manus tähistada. Manuse DocumentDB on teisiti (JSON) dokument, mis viitab media/bloobimälu talletatud mujal. Manuse on lihtsalt teisiti dokument, mis sisaldab metaandmete meediumi, mis on talletatud meediumide salvestusruumi (nt asukoht, Autor jne). 

Kaaluge social lugemine rakendus, mis kasutab DocumentDB tindimarginaalid talletamiseks ja kommentaarid, sh metaandmete tõstab esile järjehoidjad, hinnangute, meeldib/ei meeldi seotud e-raamat antud kasutaja jne.   

-   Raamat ise sisu talletatakse hoidmiseks meediumi kas saadaval meediumide poe või DocumentDB andmebaasi konto osana. 
-   Rakenduse salvestada iga kasutaja metaandmete erinevate dokumendina - nt yesseniaj metaandmete Vihik1 talletatakse viidatud /colls/joe/docs/book1 dokumendis. 
-   Osutab antud raamatu kasutaja sisu lehtede manused on salvestatud all vastava dokumendi nt /colls/joe/docs/book1/chapter1 /colls/joe/docs/book1/chapter2 jne. 

Arvestage, et ülaltoodud näidetes sõbralik ID ressursi hierarhia edastada. Ressursside pääseb kaudu REST API-de kaudu kordumatu ressursi ID-d. 

Meediumi, mida haldab DocumentDB viitab _media atribuut manuse meedia selle URI. DocumentDB tagavad prügi koguda meediumi kui kõik tasumata viiteid kõrvaldatakse. DocumentDB automaatselt genereeritud manust, kui saadate uue meedia ja kuvab _media osutamiseks äsja lisatud meediumi. Kui valite talletamiseks meediumi kaug-bloobimälu poes, haldab teie (nt OneDrive'i, Azure Storage, Dropboxi jne), saate siiski kasutada manuste meediumi viidata. Sel juhul saate manuse ise luua ja asustamine atribuudi _media.   

Nagu kõik muud ressursid, mille saab luua manused, asendatakse, kustutada, lugeda või loetletud abil hõlpsasti REST API-d või mis tahes kliendi SDK-d. Sarnaselt dokumente, lugege järjepidevuse tase manused järgmiselt järjepidevuse poliitika andmebaasi konto. See poliitika võib kaalu kohta – soovi sõltuvalt rakenduse järjepidevuse nõuded. Kui päringu manuste jaoks, lugege järjepidevuse järgib indekseerimise režiimi kogumise seadmine. Jaoks "järjepidevuse" see järgib konto järjepidevuse poliitika. 
 
## <a name="users"></a>Kasutajatele
Kasutaja DocumentDB tähistab loogilise nimeruumi rühmitamiseks õigused. Kasutaja DocumentDB pruugi kasutaja mõnda identiteedihaldussüsteemi või eelmääratletud rakenduste roll. DocumentDB, kasutaja lihtsalt tähistab mõne võtmiseks rühmitamiseks kogumi õigused jaotises andmebaasi.   

Mitme rendiõigus rakendamiseks oma rakenduse saate kasutajad luua tegelike kasutajate või rentnikud teie rakenduse DocumentDB, mis vastab. Seejärel saate luua Accessi juhtida saidikogumid, dokumendid, manused, jne vastavad antud kasutaja õigusi.   

Nagu rakenduste on vaja oma kasutaja kasvu skaala, saate vastu Kildu eri viisid andmete. Saate mudeli iga kasutaja järgmiselt:   

-   Iga kasutaja kaarte andmebaasi.
-   Iga kasutaja kaarte kogum. 
-   Dokumendid, mis vastavad mitmele kasutajale minge sihtotstarbeline saidikogumi. 
-   Dokumendid, mis vastavad mitmele kasutajale minge kogumi saidikogumid.   

Sõltumata teatud sharding strateegia valida, saate modelleerimise tegelike kasutajate kasutajad DocumentDB andmebaasi ja seostada iga kasutaja peene tekstuuriga õigused.  

![Kasutaja saidikogumid][3]  
**Sharding strateegiaid ja modelleerimine kasutajad**

Kõik muud ressursid, nt kasutajate DocumentDB saab luua, asendatakse, kustutatud, lugege või loetletud abil hõlpsasti REST API-d või mis tahes kliendi SDK-d. DocumentDB pakub alati tugev järjepidevuse lugemiseks või päringute metaandmeid, kasutaja ressursi. On vaja rõhutada, et kustutada kasutaja automaatselt tagab, et te ei pääse ühte sisalduvate õigused. Isegi juhul, kui selle DocumentDB korporatiivsetes osana taustal kustutatud kasutaja õiguste piirmäära, kustutatud õigused on saadaval kohe uuesti kasutamiseks.  

## <a name="permissions"></a>Õigused
Accessi juhtelemendi perspektiivi, näiteks andmebaasi kontod, käsitletakse andmebaaside, kasutajate ja õiguste *haldus* ressursse, kuna see nõuab administraatoriõigusi. Teisalt, veebirakendust jaotises antud andmebaasi ressursse, sh saidikogumid, dokumendid, manused, Salvestatud toimingute, päästikute ja UDF-ID ja lugeda *rakenduse ressursid*. Luba mudel vastab kahte tüüpi materjale ja rollid, et juurdepääs neile (st administraator ja kasutajale), määratleb *kiirklahvide*kahte tüüpi: *juhtslaidi* ja *ressursside võti*. Juhtslaidi võti on osa andmebaasi konto ja on esitatud arendaja (või administraator) kes on ettevalmistamise andmebaasi konto. See juhtslaidi võti on administraatori semantika, et seda saab kasutada õiguste haldus- ja rakenduste ressurssidele. Ressursi klahvi on seevastu Varundustöö kiirklahv, mis lubab juurdepääsu ressursile *kindla* rakenduse. Seega see sisaldab andmebaasi kasutaja ja õigused kasutajal on teatud ressursi (nt saidikogumi, dokumendi, manuse, salvestatud protseduur, päästik või UDF) seos.   

Ainus võimalus saada ressursi võti on õigus ressursi jaotises antud kasutaja loomise abil. Pange tähele, et luua või luba tuua, lahti esitatakse päises autoriseerimine. Loa ressurssi seob ressurss, juurdepääsu ja kasutajale. Pärast loomise õigus ressursi, peab kasutaja ainult esitama seotud ressursside võti oluline ressursile juurdepääsu saamiseks. Seega saate ressursside klahvi vaadelda loogilise ja tihendatud kujutis loa ressurssi.  

Nagu kõik muud ressursid, mille saab luua DocumentDB õigused, asendada, kustutada, lugeda või loetletud abil hõlpsasti REST API-d või mis tahes kliendi SDK-d. DocumentDB pakub alati tugev järjepidevuse lugemiseks või päringute metaandmeid, on õigus. 

## <a name="next-steps"></a>Järgmised sammud
Lisateave töötamine ressursid [rahulik kasutusviisid DocumentDB ressurssidega](https://msdn.microsoft.com/library/azure/mt622086.aspx)HTTP käskude abil.


[1]: media/documentdb-resources/resources1.png
[2]: media/documentdb-resources/resources2.png
[3]: media/documentdb-resources/resources3.png

