##<a name="create-client"></a>Kliendi ühenduse loomine

Saate luua kliendi ühenduse loomise teel on `WindowsAzure.MobileServiceClient` objekti.  Asendage `appUrl` Mobile rakenduse URL-i abil.

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

##<a name="table-reference"></a>Töötamine tabelitega

Accessi või Värskenda andmeid, viite taustväärtus tabeli loomine. Asendage `tableName` tabeli nimi

```
var table = client.getTable(tableName);
```

Kui olete tabeli viide, saate töötada koos teie tabel:

* [Päringu tabeli](#querying)
  * [Andmete filtreerimine](#table-filter)
  * [Andmete lehekülgede sirvimine](#table-paging)
  * [Andmete sortimine](#sorting-data)
* [Andmete lisamine](#inserting)
* [Andmete muutmine](#modifying)
* [Andmete kustutamine](#deleting)

###<a name="querying"></a>Kuidas: päringu tabeli viide

Kui olete tabeli viide, saate seda kasutada päringu serveris.  Päringute tehakse "LINQ nagu" keel.
Kõik andmed tabelist, kasutage järgmist:

```
/**
 * Process the results that are received by a call to table.read()
 *
 * @param {Object} results the results as a pseudo-array
 * @param {int} results.length the length of the results array
 * @param {Object} results[] the individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - the properties are the columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

Funktsiooni edu nimetatakse tulemustega.   Ärge kasutage `for (var i in results)` edu toimida, mis kuvatakse teave, mis sisaldab tulemused üle itereerima kui päringu muud funktsioonid (näiteks `.includeTotalCount()`) kasutatakse.

Päringusüntaksit kohta lisateabe saamiseks vaadake [päringu objekti dokumentatsiooni].

####<a name="table-filter"></a>Andmete filtreerimine serveris

Saate kasutada ka `where` klausel tabeli viide:

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

Saate kasutada ka funktsiooni, mis filtreerib objekt.  Sel juhul on `this` muutuja on määratud praegune objekt on filtreeritud.  Järgnevalt on taastatud võrdväärse eelnev näide:

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

####<a name="table-paging"></a>Andmete lehekülgede sirvimine

Kasutada take() ja skip() meetoditest.  Oletagem näiteks, kui soovite tabeli tükeldada 100-rea kirjed:

```
var totalCount = 0, pages = 0;

// Step 1 - get the total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

Funktsiooni `.includeTotalCount()` meetod totalCount välja tulemused objekti lisamine.  TotalCount väli täidetakse arv kirjeid, mida oleks tagastatud, kui kasutatakse pole Piipar.

Seejärel saate lehtede muutuja ja mõnes Kasutajaliidese nuppude lehe loetleda; uusi kirjeid iga lehe laadimise loadPage() abil.  Tuleks rakendada mingi vahemällu kiiruse juurdepääsu kirjed, mis on juba laaditud.


####<a name="sorting-data"></a>Kuidas: sorditud andmeid

Kasutage .orderBy() või .orderByDescending() päringu võimalust:

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Päringu objekti kohta lisateabe saamiseks vaadake [päringu objekti dokumentatsiooni].

###<a name="inserting"></a>Kuidas: andmete sisestamine

JavaScripti objekti loomine vastav kuupäevaga ja kõne table.insert() asünkroonselt.

```
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

Eduka sisestamine, klõpsake lisatud üksus, tagastatakse sünkroonimine toimingute jaoks vajalike täiendavate väljadega.  Peate värskendama oma vahemälu see teave hiljem värskendusi.

Pange tähele, et Azure'i Mobile'i rakendused Node.js Server SDK toetab dünaamiline skeemi arendamiseks.
Dünaamiliste skeemi puhul skeemi tabeli värskendatakse pealt, saate tabeli veergude lisamine, määrates neid lihtsalt lisada või värskendada toiming.  Soovitame välja lülitada dünaamilise skeemi enne rakenduse tootmisele.

###<a name="modifying"></a>Kuidas: andmete muutmine

Sarnaselt .insert() meetod, peaksite luua Update objekti ja seejärel helistage .update().  Värskenda objekti peab sisaldama ID-d kirjet värskendada – see saadud kirje lugemise ajal või kui helistate .insert().

```
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

###<a name="deleting"></a>Kuidas: andmete kustutamine

Helistage .del() meetodit kirje kustutamiseks.  Liigu ID on objekti viide:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
