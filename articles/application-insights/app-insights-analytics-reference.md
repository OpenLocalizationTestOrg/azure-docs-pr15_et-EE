<properties 
    pageTitle="Viide Analytics rakenduses rakenduse ülevaated | Microsoft Azure'i" 
    description="Laused Analytics tööriista võimas otsing, rakenduse ülevaated viide. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="awills"/>

# <a name="reference-for-analytics"></a>Kasutusanalüüsi tutvustus

[Analytics](app-insights-analytics.md) on [Rakenduse ülevaated](app-insights-overview.md)võimsaid otsingufunktsiooni. Nende lehtede kirjeldada Analytics päringukeele.

> [AZURE.NOTE] Kui teie rakendus pole andmete saatmine veel rakenduse ülevaated [test drive Analytics meie jäljendatud andmeid](https://analytics.applicationinsights.io/demo) .

## <a name="index"></a>Index


**Andke** [andke](#let-clause)


**Päringute ja tehtemärgid** [Count](#count-operator)  |  [hindamine](#evaluate-operator) | [laiendamine](#extend-operator) | [Liitu](#join-operator) | [limiit](#limit-operator) | [mvexpand](#mvexpand-operator) | [sõeluda](#parse-operator) | [projekti](#project-operator) | [projekti eemal](#project-away-operator) | [vahemiku](#range-operator) | [vähendada](#reduce-operator) | [renderdada direktiivi](#render-directive) | [piirata klausel](#restrict-clause) | [sortimine](#sort-operator) | [summarize](#summarize-operator) | [võtta](#take-operator) | [top](#top-operator) | [ülemine pesastatud](#top-nested-operator) | [Liidu](#union-operator) | [kus](#where-operator) | [where-in](#where-in-operator)

**Liitmised** [mis tahes](#any)  |  [argmax](#argmax) | [argmin](#argmin) | [avg](#avg) | [buildschema](#buildschema) | [count](#count) | [countif](#countif) | [dcount](#dcount) | [dcountif](#dcountif) | [makelist](#makelist) | [makeset](#makeset) | [max](#max) | [min](#min) | [protsentiili](#percentile) | [protsentiilid](#percentiles) | [percentilesw](#percentilesw) | [percentilew](#percentilew) | [stdev](#stdev) | [summa](#sum) | [dispersioon](#variance)

**Scalar-atribuutide** [Kahendmuutujaga literaalide](#boolean-literals)  |  [Loogikaoperaatorid](#boolean-operators) | [tekitab](#casts) | [skalaarse võrdlemine](#scalar-comparisons) | [gettype](#gettype) | [räsi](#hash) | [iff](#iff) | [isnotnull](#isnotnull) | [isnull](#isnull) | [notnull](#notnull) | [toscalar](#toscalar)

**Arvude** [Aritmeetilised tehtemärgid](#arithmetic-operators)  |  [Arvuline literaalide](#numeric-literals) | [abs](#abs) | [prügikasti](#bin) | [exp](#exp) | [floor](#floor) | [gamma](#gamma) | [log](#log) | [rand](#rand) | [sqrt](#sqrt) | [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

**Kuupäev ja kellaaeg** [Kuupäeva ja kellaaja avaldised](#date-and-time-expressions)  |  [Kuupäeva ja kellaaja literaalide](#date-and-time-literals) | [tagasi](#ago) | [datepart](#datepart) | [dayofmonth](#dayofmonth) | [dayofweek](#dayofweek) | [dayofyear](#dayofyear) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth) | [getyear](#getyear) | [nüüd](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

**String** [GUID](#guids)  |  [Obfuscated stringi literaalide](#obfuscated-string-literals) | [Stringi literaalide](#string-literals) | [stringvõrdluses](#string-comparisons) | [countof](#countof) | [eraldamiseks](#extract) | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty)| [parseurl](#parseurl) | [asendamine](#replace) | [tükeldamine](#split) | [strcat](#strcat) | [strlen](#strlen) | [alamstringi](#substring) | [tolower](#tolower) | [toupper](#toupper)

**Massiive, objektide ja dünaamiline** [Massiivi ja objekti literaalide](#array-and-object-literals)  |  [Dünaamilise objekti funktsioonide](#dynamic-object-functions) | [dünaamiliste objektide anda, et punktides](#dynamic-objects-in-let-clauses) | [JSON tee avaldiste](#json-path-expressions) | [nimed](#names) | [arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [vahemikus](#range) | [todynamic](#todynamic) | [treepath](#treepath)



## <a name="let"></a>Andke

### <a name="let-clause"></a>Andke klausel

**Tabeli võimaldavad - tabeli nimetamine**

    let recentReqs = requests | where timestamp > ago(3d); 
    recentReqs | count

**Skalaarse võimaldavad - väärtus nimetamine**

    let interval = 3d; 
    requests | where timestamp > ago(interval)

**Lambda võimaldavad - funktsiooni nimetamine**

    let Recent = 
       (interval:timespan) { requests | where timestamp > ago(interval) };
    Recent(3h) | count

    let us_date = (t:datetime) { strcat(getmonth(t),'/',dayofmonth(t),'/',getyear(t)) }; 
    requests | summarize count() by bin(timestamp, 1d) | project count_, day=us_date(timestamp)

Anda, et klausel seob tabeli tulem, skalaarväärtuse või funktsiooni [nime](#names) . Klausel on päringule eesliite ja sidumine on see päring. (Võimaldavad ei võimalda nimi asjad, mida kasutada hiljem seansi.)

**Süntaks**

    let name = scalar_constant_expression ; query

    let name = query ; query

    let name = (parameterName : type [, ...]) { plain_query }; query

    let name = (parameterName : type [, ...]) { scalar_expression }; query

* *Tüüp:* `bool`, `int`, `long`, `double`, `string`, `timespan`, `datetime`, `guid`,[`dynamic`](#dynamic-type)
* *plain_query:* Päringu, mis ei lase-klausli eelkinnitatud.

**Näited**

    let rows = (n:long) { range steps from 1 to n step 1 };
    rows(10) | ...


Omas liitumiseks tehke järgmist.

    let Recent = events | where timestamp > ago(7d);
    Recent | where name contains "session_started" 
  	| project start = timestamp, session_id
  	| join (Recent 
        | where name contains "session_ended" 
        | project stop = timestamp, session_id)
      on session_id
  	| extend duration = stop - start 


## <a name="queries-and-operators"></a>Päringute ja tehtemärgid

Päringu üle oma telemeetria koosneb allika voo, müügivõimaluste filtreid, millele järgneb viide. Näiteks:


```AIQL
requests // The request table starts this pipeline.
| where client_City == "London" // filter the records
   and timestamp > ago(3d)
| count 
```
    
Iga filtri eelkinnitatud Triibu märgi `|` on näiteks *tehtemärk*, mõned parameetrid. Tehtemärk sisendi on tabel, mis on põhjustanud eelnev kohaletoimetamisel. Enamikul juhtudel kõik parameetrid on [skalaarse avaldiste](#scalars) sisend veerud üle. Mõnel juhul, parameetrid on Sisestuskeel veergude nimed ja mõnel juhul, see parameeter on teine tabel. Päringu tulem on alati tabeli, isegi siis, kui see on ainult üks veerg ja üks rida.

Päringute võib sisaldada ühe reapiirid, kuid lõpetatakse tühi rida. Need võivad sisaldada kommentaaride vahel `//` ning rea lõppu.

Päring võib eelkinnitatud, üks või mitu [lasta osalaused](#let-clause), mis määratlevad Scalar-atribuutide, tabelite või funktsioonid, mida saab kasutada päringu raames.

```AIQL

    let interval = 3d ;
    let city = "London" ;
    let req = (city:string) {
      requests
      | where client_City == city and timestamp > ago(interval) };
    req(city) | count
```

> `T`kasutatakse päringu alltoodud näiteid tähistamiseks eelmises kohaletoimetamisel või andmeallika tabelis.
> 

### <a name="count-operator"></a>Count tehtemärk

Funktsiooni `count` tehtemärk tagastab kirjete (ridade) arvu Sisestuskeel kirjekomplekti.

**Süntaks**

    T | count

**Argumendid**

* *T*: tabelina esitatud andmed, kelle kirjed on loendatavaid.

**Annab vastuseks**

See funktsioon tagastab tabeli, kus ühe kirje ja tüüp veeru `long`. Ainult lahtri väärtus *T*kirjete arvu. 

**Näide**

```AIQL
requests | count
```

### <a name="evaluate-operator"></a>tehtemärk hindamine

`evaluate`on laiend süsteem, mis võimaldab eripärase algoritmide lisatavatest päringud.

`evaluate`peab olema viimase tehtemärk päringu teel (välja arvatud võimalik `render`). See peab asuma, funktsioon kehasse.

[hinnata autocluster](#evaluate-autocluster) | [hinnata korv](#evaluate-basket) | [hinnata diffpatterns](#evaluate-diffpatterns) | [extractcolumns hindamine](#evaluate-extractcolumns)

#### <a name="evaluate-autocluster"></a>autocluster hindamine

     T | evaluate autocluster()

AutoCluster leiab levinud mustrite eraldi atribuute (mõõdud) andmete ja kas vähendada väheste mustrite (kas see on 100 või 100 k ridade) algse päringu tulemused. AutoCluster on välja töötatud analüüsimiseks tõrked (nt erandid, jookseb), kuid saate töötada potentsiaalselt mis tahes filtreeritud andmehulgas. 

**Süntaks**

    T | evaluate autocluster( arguments )

**Annab vastuseks**

AutoCluster tagastab mustreid, mis jäädvustada osade andmete väärtustes levinud koos üle mitme eraldi atribuute (tavaliselt väike) hulk. Rea tulemuste esindab iga mustri. 

Kahe esimese veeru on count ja välja algse päringu ridu, mis muster teatav protsent. Ülejäänud veerud on algse päringu ja nende väärtus on kas kindla väärtuse veerust või "*" tähendab muutujaga. 

Pange tähele, et mustreid ei ole ühendamata: nad võib olla kattuvaid ja tavaliselt katta kogu algse read. Teatud ridade võib-olla ei kuulu mustrile.

**Näpunäited**

* Kasutage `where` ning `project` Sisestuskeel toru vähendamiseks andmed lihtsalt mis teile huvi.
* Kui teile kuvatakse huvitav rida, võiksite sinna täpsemaks, lisades selle teatud väärtusi süvitsi oma `where` filter.

**Argumendid (kõik valikuline.)**

* `output=all | values | minimal` 

    Tulemuste vorming. Veergude arv ja protsenti alati kuvada tulemused. 

 * `all`-panust kõik veerud on väljund
 * `values`-filtreerib ainult veergude "*" tulemuste
 * `minimal`-filtreerib ka veerud, mis on identne algse päringu kõiki ridu. 


* `min_percent=`*kahekordne* (vaikimisi: 1)

    Minimaalne protsent ulatus loodud read.

    Näide:`T | evaluate autocluster("min_percent=5.5")`


* `num_seeds=`*int* (vaikimisi: 25) 

    Seemnete määratleb algoritmi algse kohalik otsing punktide arv. Mõnel juhul sõltuvalt struktuuri andmed, suurendab seemnete suurendab kaudu aeglasem päringu Miinuseks ruumi suurendada otsingu tulemuste arvu (või kvaliteedi). Num_seeds argument on mõlemast suunast tulemuste väheneb nii, et vähendada selle all 5 saavutab väikese jõudlustäiustused kõrgemale 50 harva loob täiendavad mustrite.

    Näide:`T | evaluate autocluster("num_seeds=50")`


* `size_weight=`*0 < kahekordsete < 1*+ (vaikimisi: 0,5)

    Annab teile mõned üle üldine (kõrge ulatust) ja tähenduslik (palju väärtustes). Suurenevad size_weight vähendab tavaliselt mustrite ja iga mustri on tavaliselt suuremad protsendi. Väheneb size_weight tavaliselt toodab veel rohkem väärtustes ja väiksem protsent ulatus mustriga. Funktsiooni kaitstud valem on kaalutud geomeetrilise keskmise normaliseeritud üldise Keskmine ja tähenduslik Keskmine size_weight ja 1-size_weight nimega kaalu vahel. 

    Näide:`T | evaluate autocluster("size_weight=0.8")`


* `weight_column=`*column_name*

    Leiab iga rea vastavalt määratud sisend (vaikimisi on iga rea kaal 1"), levinud kasutamist paksus veerg on konto valimite või bucketing/koondamine juba manustatud iga rea andmeid.

    Näide:`T | evaluate autocluster("weight_column=sample_Count")` 



#### <a name="evaluate-basket"></a>korv hindamine

     T | evaluate basket()

Korv leiab kõik sagedased mustrite eraldi atribuute (mõõdud) andmed ja tagastab kõik sagedased mustreid, mis on möödas sagedus lävi algse päringu. Korv on tagatud kõik sagedased mustrite andmeid leida, kuid ei ole tagatud olema Polünoomne käitusaja. Päringu käitusaja lineaarne ridade arvu, kuid mõnel juhul võib olla eksponentsiaalse (mõõdud) veergude arv. Korv põhineb Apriori algoritmi algselt välja korv analüüsi andmete hankimiseks. 

**Annab vastuseks**

Kõik mustrite kuvataks rohkem kui määratud murdosa (vaikimisi 0,05) sündmused.

**Argumendid (kõik valikuline.)**


* `threshold=`*0.015 < kahekordsete < 1* (vaikimisi: 0,05) 

    Määrab minimaalsete suhte käsitletaks sagedased read (mustrite väiksem suhe ei tagastata).

    Näide:`T | evaluate basket("threshold=0.02")`


* `weight_column=`*column_name*

    Leiab iga rea vastavalt määratud sisend (vaikimisi on iga rea kaal 1"), levinud kasutamist paksus veerg on konto valimite või bucketing/koondamine juba manustatud iga rea andmeid.

    Näide: T | basket("weight_column=sample_Count") hindamine


* `max_dims=`*1 < int* (vaikimisi: 5)

    Määrab maksimaalne arv mittekorreleeruvate mõõtmed korv, piiratud vaikimisi vähendamiseks käitusaja päringu kohta.


* `output=minimize` | `all` 

    Tulemuste vorming. Veergude arv ja protsenti alati kuvada tulemused.

 * `minimize`-filtreerib ainult veergude "*" tulemused.
 * `all`-panust kõik veerud on väljundi.




#### <a name="evaluate-diffpatterns"></a>diffpatterns hindamine

     requests | evaluate diffpatterns("split=success")

Diffpatterns võrdleb kahe andmehulga sama struktuuri ja leiab mustrite eraldi atribuute (mõõdud), mis iseloomustab kahe andmehulga erinevused. Diffpatterns on välja töötatud analüüsimiseks tõrked (nt võrdlemine tõrkeid ebaõnnestumist – antud aja jooksul), kuid potentsiaalselt leida andmete kahte tüüpi sama struktuuri erinevused. 

**Süntaks**

`T | evaluate diffpatterns("split=`*BinaryColumn*`" [, arguments] )`

**Annab vastuseks**

Diffpatterns tagastab (tavaliselt väike) hulk mustrite, mis hõlmavad andmete kaks eri osade (st mustri suur osa andmehulga esimese ridu ja madal protsent teine rida). Rea tulemuste esindab iga mustri.

Esimesed neli veergu on arv ja protsent välja algse päringu ridu, mis mustri igas kogumis, viienda veeru on kaks erinevuse (absoluutne protsent punktides). Ülejäänud veerud on algse päringu ja nende väärtus on kas kindla väärtuse veerust või * tähendab muutujaga. 

Pange tähele, et mustreid ei ole erinevate: nad võib olla kattuvaid ning tavaliselt katta kogu algse read. Teatud ridade võib-olla ei kuulu mustrile.

**Näpunäited**

* Kasutage where ja projekti Sisestuskeel toru vähendamiseks andmed lihtsalt mis teile huvi.

* Kui teile kuvatakse huvitav rida, mida soovite seda edasi, lisades selle kindlad väärtused where filtreerimiseks üldiseks.

**Argumendid**

* `split=`*veeru nimi* (nõutav)

    Veerg peab olema täpselt kahest väärtusest. Vajaduse korral selliste veeru loomiseks:

    `requests | extend fault = toint(resultCode) >= 500` <br/>
    `| evaluate diffpatterns("split=fault")`

* `target=`*string*

    Ütleb ainult otsida algoritmi mustrid, mis on suurem protsendimäär target andmehulga, sihtkohas peab olema üks kahest väärtusest Tükelda veerg.

    `requests | evaluate diffpatterns("split=success", "target=false")`

* `threshold=`*0.015 < kahekordsete < 1* (vaikimisi: 0,05) 

    Määrab kaks vahe minimaalne mustri (suhe).

    `requests | evaluate diffpatterns("split=success", "threshold=0.04")`

* `output=minimize | all`

    Tulemuste vorming. Tulemused kuvatakse alati veergude arv ja protsenti. 

 * `minimize`-filtreerib ainult veergude "*" tulemuste
 * `all`-panust kõik veerud on väljund

* `weight_column=`*column_name*

    Leiab iga rea vastavalt määratud sisend (vaikimisi iga rida on kaal 1"). Weight (kaal) veeru ühine kasutamine on konto valimite või bucketing/koondamine juba manustatud iga rea andmeid.

    `requests | evaluate autocluster("weight_column=itemCount")`






#### <a name="evaluate-extractcolumns"></a>extractcolumns hindamine

     exceptions | take 1000 | evaluate extractcolumns("details=json") 

Extractcolumns kasutatakse rikastamine tabel koos mitme lihtsa veerud, mis on dünaamiliselt ekstraktitud kokku (pool) liigendatud veerud nende tüüpi järgi. Praegu see toetab json veergude ainult nii dünaamiline ja sariväljaanne jsons string.


* `max_columns=`*int* (vaikimisi: 10) 

    Uus lisatud veergude arv on dünaamiline ja see võib olla väga suurte (tegelikult on erinevate klahvide json kõigi kirjete arv) nii, et peate selle piiratud. Uue veeru laskuvas järjestuses sagedus alusel sorditakse ja kuni max_columns lisatakse tabelisse.

    `T | evaluate extractcolumns("json_column_name=json", "max_columns=30")`


* `min_percent=`*kahekordne* (vaikimisi: 10.0) 

    Teine võimalus piirata uute veergude ignoreerimine veerud, mille sagedus on väiksem kui min_percent.

    `T | evaluate extractcolumns("json_column_name=json", "min_percent=60")`


* `add_prefix=`*bool* (vaikimisi: true) 

    Kui väärtus on true keerukate veeru nimi lisatakse eesliide ekstraktitud veergude nimed.


* `prefix_delimiter=`*string* (vaikimisi: "_") 

    Kui add_prefix = true see parameeter määratleb eraldaja, mis kasutab concatenate uute veergude nimed.

    `T | evaluate extractcolumns("json_column_name=json",` <br/>
    `"add_prefix=true", "prefix_delimiter=@")`


* `keep_original=`*bool* (vaikimisi: false) 

    Kui väärtus on true, säilitatakse väljundi tabeli algse (json) veerud.


* `output=query | table` 

    Tulemuste vorming. 

 * `table`-Väljund on sama tabeli saabunud miinus määratud Sisestuskeel veergude pluss uut veergu, mis olid saadud Sisestuskeel veerud.
 * `query`-Väljund on string, mis tähistab päringu teete tulemi tabelina. 




### <a name="extend-operator"></a>tehtemärk laiendamine

     T | extend duration = stopTime - startTime

Ühe või mitme arvutatud veeru lisamine tabelisse. 


**Süntaks**

    T | extend ColumnName = Expression [, ...]

**Argumendid**

* *T:* Sisendtabel.
* *ColumnName:* Lisatavad veerud nimi. [On tõstutundlikud ning võivad sisaldada tähestikuline,](#names) arv või '_' märki. Kasutage `['...']` või `["..."]` tsitaat märksõnade või muude klaviatuurilt puuduvate märkide nimedega.
* *Avaldis:* Arvutus olemasolevad veerud üle.

**Annab vastuseks**

Sisendtabel määratud täiendavad veergudega koopia.

**Näpunäited**

* Kasutage [`project`](#project-operator) selle asemel, kui soovite ka drop või veerge ümber nimetada.
* Ärge kasutage `extend` lihtsalt toomiseks kasutada pikk avaldises lühem nimi. `...| extend x = anonymous_user_id_from_client | ... func(x) ...` 

    Tabeli veergude kohalikke indekseeritud; teie uus nimi määratleb täiendav veerg, mis pole indekseeritud nii, et päring tõenäoliselt aeglasemalt.

**Näide**

```AIQL
traces
| extend
    Age = now() - timestamp
```


### <a name="join-operator"></a>Liitu tehtemärk

    Table1 | join (Table2) on CommonColumn

Liidab kaks tabelit read vastavaid väärtusi määratud veeru järgi.


**Süntaks**

    Table1 | join [kind=Kind] (Table2) on CommonColumn [, ...]

**Argumendid**

* *Tabel1* - "Vasak pool' selle ühenduse.
* *Tabel 2* - "paremas servas' ühendus. See võib olla pesastatud päringu avaldis, mis väljundid tabeli.
* *CommonColumn* - veerg, mis on kahe tabeli sama nimi.
* *Laadi* – saate määrata, kuidas kahe tabeli read peavad olema täidetud.

**Annab vastuseks**

Tabel:

* Iga veeru kahe tabeli, sh kattuvad klahvid iga veerg. Veerud paremal pool kuvatakse automaatselt ümber nimetada, kui nime konflikte on.
* Üht rida iga match Sisestuskeel tabelite vahel. Vaste on ühest tabelist, millel on sama väärtuse kõigi valitud rea soovitud `on` reana teise tabeli väljad. 

* `Kind`määratlemata

    Ainult ühe rea vasakus servas on täidetud iga väärtuse jaoks soovitud `on` võti. Väljund sisaldab üht rida iga mängu selle rea reaga, alates paremalt.

* `Kind=inner`
 
     On rea väljundi vastendatud read kombinatsioonide alla, vasakule ja paremale.

* `kind=leftouter`(or `kind=rightouter` or `kind=fullouter`)

     Lisaks sisemise vastet, on üht rida iga rea vasakule (või paremale), isegi juhul, kui see on vasteid. Sel juhul vasteta väljundi lahtrid sisaldavad tühiväärtusi.

* `kind=leftanti`

     Tagastab kõik kirjed, mis sisaldavad vasteid paremalt vasakule küljele. Tulemuste tabelis on ainult veergude vasakult poolelt. 
 
Kui loendis on mitu rida nende väljade jaoks sama väärtusega, kuvatakse kõigi kombinatsioonide read.

**Näpunäited**

Parima jõudluse:

* Kasutage `where` ja `project` vähendamiseks Sisestuskeel tabelite, veergude ja ridade arvud enne selle `join`. 
* Kui ühe tabeli on alati väiksem kui teise, kasutage seda ühendus (veetorustiku) vasakus servas.
* Veergude ühendamine vaste peab olema sama nimi. Kasutage projekti tehtemärki vajadusel ümber nimetada veeru üks tabel.

**Näide**

Saada pikendada tegevuste Logi, kus mõned kirjete märkimine käivitamine ja tegevuse lõppu. 

```AIQL
    let Events = MyLogTable | where type=="Event" ;
    Events
  	| where Name == "Start"
  	| project Name, City, ActivityId, StartTime=timestamp
  	| join (Events
           | where Name == "Stop"
           | project StopTime=timestamp, ActivityId)
        on ActivityId
  	| project City, ActivityId, StartTime, StopTime, Duration, StopTime, StartTime

```


### <a name="limit-operator"></a>piiratud tehtemärk

     T | limit 5

Tagastab määratud arvu ridade sisendtabel. Ei ole kindel, millised kirjed tagastatakse. (Kindlate kirjete tagastamiseks kasutage [`top`](#top-operator).)

**Alias (pseudonüüm)**`take`

**Süntaks**

    T | limit NumberOfRows


**Näpunäited**

`Take`on lihtne ja tõhus viis oma tulemuste kuvamiseks, kui töötate interaktiivseks. Pange tähele, et ei garanteeri kõiki kindla ridu, andes või andes neile kindlas järjekorras.

On peidetud piirarvu kliendile tagastatav ridade isegi juhul, kui te ei kasuta `take`. Tõstke see piirang, kasutage funktsiooni `notruncation` kliendi võimalus.



### <a name="mvexpand-operator"></a>mvexpand tehtemärk

    T | mvexpand listColumn 

Avardab sellise loendi dünaamiline tipitud lahtri (JSON) nii, et iga kirje on eraldi real. Kõigi muude lahtrite laiendatud Real dubleeritakse. 

(Vt ka [`summarize makelist`](#summarize-operator) mis täidab funktsiooni vastupidist.)

**Näide**

Oletame, et sisendtabel on:

|A:int|B:string|D:Dynamic|
|---|---|---|
|1|"Tere"|{"key": "väärtus"}|
|2|"world"|[0,1, "k", "v"]|

    mvexpand D

Tulemus on:

|A:int|B:string|D:Dynamic|
|---|---|---|
|1|"Tere"|{"key": "väärtus"}|
|2|"maailma"|0|
|2|"maailma"|1|
|2|"maailma"|"k"|
|2|"maailma"|"v"|


**Süntaks**

    T | mvexpand  [bagexpansion=(bag | array)] ColumnName [limit Rowlimit]

    T | mvexpand  [bagexpansion=(bag | array)] [Name =] ArrayExpression [to typeof(Typename)] [limit Rowlimit]

**Argumendid**

* *ColumnName:* Tulemis on massiivid nimega veerus laiendada mitu rida. 
* *ArrayExpression:* Avaldise saades massiivi. Selle vormi kasutamise korral lisatakse uus veerg ja säilib olemasolev fail.
* *Nimi:* Uue veeru nimi.
* *Typename:* Tekitab teatud tüüpi laiendatud avaldis
* *Auditipäringu:* Iga algse rea loodud ridade maksimaalne arv. Vaikimisi on 128.

**Annab vastuseks**

Mitme rea iga väärtuse massiivis nimega veerus või avaldises massiiv.

Laiendatud veerg on alati dünaamiline tüüp. Enamus kasutada näiteks `todatetime()` või `toint()` kui soovite arvutada või väärtuste liitmine.

Kaks viisi – Atribuudikonteineri laiendusi on toetatud:

* `bagexpansion=bag`: Üks kirje Atribuudikotid laiendatakse Atribuudikotid. See on vaikimisi laiendamine.
* `bagexpansion=array`: Kahe-elementi Atribuudikotid laiendatakse `[` *klahvi*`,`*väärtus* `]` massiiv struktuurid, ühtse juurdepääsu võtmed ja väärtused (kui ka, näiteks töötab eraldiseisva loenduse koondamine üle atribuutide nimesid). 

**Näited**


    exceptions | take 1 
  	| mvexpand details[0]

Jagab iga üksuse välja üksikasjad ridadesse erandite kirje.



### <a name="parse-operator"></a>sõeluda tehtemärk

    T | parse "I got 2 socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long *


    T | parse kind=relaxed
          "I got no socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long * 

    T |  parse kind=regex "I got socks for my 63rd birthday" 
    with "(I|She) got" present "for .*?" year:long * 

Ekstraktib string väärtused. Saate kasutada liht- või tavalise avaldis sobitamine.

**Süntaks**

    T | parse [kind=regex|relaxed] SourceText 
        with [Match | Column [: Type [*]] ]  ...

**Argumendid**

* `T`: Sisendtabel.
* `kind`: 
 * `simple`(vaikeväärtus): selle `Match` stringi on tavaline stringide.
 * `relaxed`: kui tekst ei veeru tüübiks sõeluda, on veeru määratud null ja selle sõeluda jätkub 
 * `regex`: selle `Match` stringid on regulaaravaldised.
* `Text`: Veeru või muu avaldis, mis annab tulemiks või saab teisendada string.
* *Match:* Järgmine osa stringi vastavad ja Loobu.
* *Veerg:* Saate määrata selle veeru stringi järgmise osa. Kui see pole olemas, luuakse veerg.
* *Tüüp:* Sõelub stringi järgmise osa tüübiks määratud int, nt kuupäev, kahekordseks. 


**Annab vastuseks**

Sisendtabel, laiendatud veergude järgi.

Elemendid on `with` klausel sobitatakse omakorda lähteteksti suhtes. Iga element närimiskondid välja kogumi allika tekst: 

* Sõnasõnaline string või Lihtavaldise kattuvad kursor liigub Vastenda kestus.
* Regex sõeluda, tavaline avaldise saate kasutada minimeerimine tehtemärki "?" järgmised match kiiresti liikuda.
* Veeru nimi, tüüp on sõelub teksti määratud tüüpi. Kui liiki = lõdvestunud, mis õnnestu sõeluda muudab kehtetuks kogu mustri sobitamine.
* Veeru nimi ilma tüüp, või "stringiga", tippige kopeerib märkide avamiseks järgmist match väikseima arvu.
* ' *' Ignoreerib märkide avamiseks järgmist match väikseima arvu. Saate kasutada '*' alguses ja lõpus mustri, või pärast peale stringi või stringi vasteid vahel.

Kõik elemendid on sõeluda muster peab vastama õigesti; muul juhul valmistatakse tulemeid. Selle reegli erandiks on see, et kui selline = lõdvestunud, kui sõelumine tipitud muutuja nurjub, kuvatakse sõeluda ülejäänud jätkub.

**Näited**

*Lihtne:*

```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse "I got 2 socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```

x | Counter | Esita | Aasta
---|---|---|---
1 | 2 | sokid | 63

*Paindlikumad:*

Kui sisend sisaldab vaste iga tipitud veeru jaoks, lõdvestunud sõeluda tulemus on sama lihtne sõeluda. Kuid kui üks tipitud veerud pole õigesti sõeluda, lõdvestunud sõeluda töötlemine ülejäänud muster, arvestades lihtsa sõeluda lakkab töötamast ja ei mis tahes tulemi loomiseks.


```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse kind="relaxed"
        "I got several socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```


x  | Esita | Aasta
---|---|---
1 |  sokid | 63


*Regex:*

```AIQL

// Run a test without reading a table:
range x from 1 to 1 step 1 
// Test string:
| extend s = "Event: NotifySliceRelease (resourceName=Scheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41, releaseTime=02/17/2016 08:41:00, previousLockTime=02/17/2016 08:40:00)" 
// Parse it:
| parse kind=regex s 
  with ".*?[a-zA-Z]*=" resource 
       ", total.*?sliceNumber=" slice:long *
       "lockTime=" lock
       ",.*?releaseTime=" release 
       ",.*?previousLockTime=" previous:date 
       ".*\\)"
| project-away x, s
```

ressurss | sektorit | lukustamine | Vabastage | eelmise
---|---|---|---|---
Ajasti | 16 | 02/17/2016 08:41:00 | 02/17/2016 08:41 | 2016-02-17T08:40:00Z

### <a name="project-operator"></a>projekti tehtemärk

    T | project cost=price*quantity, price

Valige veerud lisada, ümber nimetada või langev ja uue arvutatud veergude lisamine. Tulemis veergude järjestuse on määratud käsul argumendid. Ainult veergude määratud argumentidega kaasatud tulemus: kõik teised panus kõrvaldatakse.  (Vt ka `extend`.)


**Süntaks**

    T | project ColumnName [= Expression] [, ...]

**Argumendid**

* *T:* Sisendtabel.
* *ColumnName:* Kuvatakse väljund veeru nimi. Kui *avaldis*ei ole, peab veeru selle nime kuvatakse sisend. [On tõstutundlikud ning võivad sisaldada tähestikuline,](#names) arv või '_' märki. Kasutage `['...']` või `["..."]` tsitaat märksõnade või muude klaviatuurilt puuduvate märkide nimedega.
* *Avaldis:* Valikuline skalaaravaldis Sisestuskeel veergudele viitamine. 

    See on juriidiline sisendi uue arvutatud veeru olemasoleva veeru sama nimega tagastamiseks.

**Annab vastuseks**

Tabel, milles on nimega argumentidena ja nii palju veerge read nimega sisendtabel.

**Näide**

Järgmises näites on kujutatud mitmeid erinevaid toiminguid, mida saab teha, kasutades funktsiooni `project` tehtemärk. Sisendtabel `T` on kolm veergu, mille tüüp on `int`: `A`, `B`, ja `C`. 

```AIQL
T
| project
    X=C,               // Rename column C to X
    A=2*B,             // Calculate a new column A from the old B
    C=strcat("-",tostring(C)), // Calculate a new column C from the old C
    B=2*B,              // Calculate a new column B from the old B
    ['where'] = client_City // rename, using a keyword as a column name
```

### <a name="project-away-operator"></a>projekti eemal tehtemärk

    T | project-away column1, column2, ...

Määratud veergude välistamine. Tulem on kõik Sisestuskeel veerge, välja arvatud teie nime.

### <a name="range-operator"></a>vahemiku tehtemärk

    range LastWeek from ago(7d) to now() step 1d

Loob väärtuste ühe veeruga tabel. Pange tähele, et see ei sisalda müügivõimaluste sisestusteade. 

|LastWeek|
|---|
|2015-12-05 09:10:04.627|
|2015-12-06 09:10:04.627|
|...|
|09:10:04.627 2015 / 12 / 12|



**Süntaks**

    range ColumnName from Start to Stop step Step

**Argumendid**

* *ColumnName:* Ühe veeru väljundi tabeli nimi.
* *Alustamine:* Väikseim väärtus väljund.
* *Peatamine:* Suurim väärtus loodud väljund (või seotud kõrgeim väärtus, kui *toimingu* juhiseid üle selle väärtuse).
* *Samm:* Kaks järjestikust väärtuste vahe. 

Argumendid peavad olema arv-, kuupäeva-või kuuline ajavahemik. Ta ei saa viidata veergude mis tahes tabeli. (Kui soovite arvutada vahemiku põhjal on sisendtabel, kasutage [vahemiku *funktsioon*](#range), võib-olla [mvexpand tehtemärk](#mvexpand-operator).) 

**Annab vastuseks**

Nimega *ColumnName*, väärtused on *alustada*, *alustage*ühe veeruga tabel + *samm*... kuni ja kaasa arvatud *lõpetada*.

**Näide**  

```AIQL
range Steps from 1 to 8 step 3
```

Ühe veeruga tabel nimega `Steps` mille tüüp on `long` ja väärtused on `1`, `4`, ja `7`.

**Näide**

    range LastWeek from bin(ago(7d),1d) to now() step 1d

Tabelisse keskööst möödunud seitse päeva. Prügikast (floor) funktsioon vähendab iga kord, kui päeva alguses.

**Näide**  

```AIQL
range timestamp from ago(4h) to now() step 1m
| join kind=fullouter
  (traces
      | where timestamp > ago(4h)
      | summarize Count=count() by bin(timestamp, 1m)
  ) on timestamp
| project Count=iff(isnull(Count), 0, Count), timestamp
| render timechart  
```

Näitab, kuidas `range` väike, erakorralist, mõõde tabel, mida kasutatakse tutvustada nullide kui lähteandmete on ei sisalda väärtusi loomiseks võib kasutada tehtemärki.

### <a name="reduce-operator"></a>tehtemärk vähendamine

    exceptions | reduce by outerMessage

Proovib rühmitada sarnaste kirjed. Iga rühma väljundid tehtemärk on `Pattern` arvab kirjeldab selle rühma ja `Count` kirjete selles rühmas.


![](./media/app-insights-analytics-reference/reduce.png)

**Süntaks**

    T | reduce by  ColumnName [ with threshold=Threshold ]

**Argumendid**

* *ColumnName:* Veerg, kui soovite uurida. Peab olema stringi tüüpi.
* *Lävi:* Väärtus vahemiku {0..1}. Vaikimisi on 0,001. Suure sisendina, lävi peaks olema väike. 

**Annab vastuseks**

Kaks veergu, `Pattern` ja `Count`. Paljudel juhtudel saab mustri täieliku väärtuse veerust. Mõnel juhul see saab levinud terminite tuvastamine ja asendada muutuv osad "*".

Näiteks tulemi `reduce by city` võivad olla: 

|Mustri | Count |
|---|---|
| San * | 5182 |
| Saint * | 2846 |
| Moskva | 3726 |
| \*-on-\* | 2730 |
| Pariis | 27163 |


### <a name="render-directive"></a>renderdada direktiivi

    T | render [ table | timechart  | barchart | piechart ]

Renderda suunab esitluse kiht tabeli kuvamiseks tehke järgmist. Viimase elemendi toru peaks olema. See on mugavam alternatiiv juhtelementide ekraanil, saate uurida päringu kindla esitluse meetodi abil.

### <a name="restrict-clause"></a>piira klausel 

Saate määrata kogumi tabelinimede saadaval tehtemärgid, mida järgida. Näiteks:

    let e1 = requests | project name, client_City;
    let e2 =  requests | project name, success;
    // Exclude predefined tables from the union:
    restrict access to (e1, e2);
    union * |  take 10 

### <a name="sort-operator"></a>sortimine tehtemärk 

    T | sort by country asc, price desc

Ühe või mitme veeru järgi sortimiseks sisendtabel read tellimusse.

**Alias (pseudonüüm)**`order`

**Süntaks**

    T  | sort by Column [ asc | desc ] [ `,` ... ]

**Argumendid**

* *T:* Sisestusmeetodi sortimiseks tabel.
* *Veerg:* *T* veeru sortimise aluseks. Väärtused tüüpi peab olema arv, kuupäev, kellaaeg või string.
* `asc`Sordi tõusvas järjestuses, madal kõrge üheks alusel. Vaikimisi on `desc`, laskuvas kõrge, madal.

**Näide**

```AIQL
Traces
| where ActivityId == "479671d99b7b"
| sort by Timestamp asc
```
Kõik read tabeli jälgi kindlal `ActivityId`, nende ajatempli järgi sorditud.

### <a name="summarize-operator"></a>Summeeri tehtemärk

Tagastab tabeli, mis koondab sisu sisendtabel.
 
    requests
  	| summarize count(), avg(duration), makeset(client_City) 
      by client_CountryOrRegion

Tabel, mis näitab iga riigi arv, Keskmine taotluse kestus ja linnade kogum. Iga erinevate riigi väljund on rea. Väljundi veergude kuvamine count, kestus, linna ja riigi. Kõigi Sisestuskeel veergude ignoreeritakse.


    T | summarize count() by price_range=bin(price, 10.0)

Tabel, mis näitab, mitu üksust on hinnad iga intervalli [0,10.0], [10.0,20.0] jne. Selles näites on veeru loendamiseks ja hinna vahemiku. Kõigi Sisestuskeel veergude ignoreeritakse.


**Süntaks**

    T | summarize
         [  [ Column = ] Aggregation [ `,` ... ] ]
         [ by
            [ Column = ] GroupExpression [ `,` ... ] ]

**Argumendid**

* *Veerg:* Valikuline tulemi veeru nimi. Vaikimisi nimi saadud avaldist. [On tõstutundlikud ning võivad sisaldada tähestikuline,](#names) arv või '_' märki. Kasutage `['...']` või `["..."]` tsitaat märksõnade või muude klaviatuurilt puuduvate märkide nimedega.
* *Koondamine:* Kõne koondamine funktsioonile näiteks `count()` või `avg()`, argumentidena nimedega veerg. Lugege teemat [liitmised](#aggregations).
* *GroupExpression:* Avaldise veerud üle, mis pakub erinevate väärtuste kogumi. Tavaliselt on kas veeru nimi, mis juba sisaldab väärtusi, piiratud kogumit või `bin()` arv- ega veeru argumendina. 

Kui esitate arv- ega avaldis kasutamata `bin()`, Analytics rakendatakse see automaatselt intervalliga `1h` korda, või `1.0` arvude jaoks.

Kui te ei esita mõnda *GroupExpression,* liidetakse terve tabeli ühe väljundi rida.



**Annab vastuseks**

Sisestuskeel read on korraldatud rühmadesse samad väärtused, millel on `by` avaldised. Seejärel määratud liitmisfunktsioonid on arvutatud üle iga rühma, mis toodab üht rida iga rühma. Tulemus, mis sisaldab soovitud `by` arvutatud veerge ja ka vähemalt üks veerg iga kokkuvõtte. (Mõned liitmisfunktsioonid tagastada mitme veeru).

Tulemus on sama palju ridu kui seal on erinevate kombinatsioonide `by` väärtused. Kui soovite üle vahemikud arvväärtuste Summeeri, kasutage `bin()` vahemike vähendamiseks eraldi väärtused.

**Märkus**

Kuigi saate sisestada suvalise avaldiste liitmise ja rühmitamise avaldised, on lihtne Veerunimedes kasutada või rakendamiseks tõhusam `bin()` arvuline veerule.



### <a name="take-operator"></a>võtta tehtemärk

Pseudonüüm [limiit](#limit-operator)


### <a name="top-operator"></a>ülemise tehtemärk

    T | top 5 by Name desc nulls first

Tagastab määratud veeru alusel sorditud esimesele *N* kirjed.


**Süntaks**

    T | top NumberOfRows by Sort_expression [ `asc` | `desc` ] [`nulls first`|`nulls last`] [, ... ]

**Argumendid**

* *NumberOfRows:* *T* tagastamiseks ridade arv.
* *Sort_expression:* Avaldis, mille ridade sortimiseks. See on tavaliselt veeru nimi. Saate määrata rohkem kui üks sort_expression.
* `asc`või `desc` (vaikimisi) võidakse kuvada juhtelemendi kas valiku on tegelikult "Põhja" või "üles" vahemiku.
* `nulls first`või `nulls last` tühiväärtusi kuvamiskoha juhtelemendid. `First`Vaikimisi on `asc`, `last` on vaikimisi `desc`.


**Näpunäited**

`top 5 by name`on võrdne pinnapealselt `sort by name | take 5`. Kuid see on kiirem ja alati sorditud tagastab tulemused, arvestades `take` anna mingit garantiid, nt.

### <a name="top-nested-operator"></a>ülemine pesastatud tehtemärk

    requests 
  	| top-nested 5 of name by count()  
    , top-nested 3 of performanceBucket by count() 
    , top-nested 3 of client_CountryOrRegion by count()
  	| render barchart 

Annab hierarhilise tulemit, kus iga tase on eelmisest tasemest süvitsimineku. See on kasulik küsimustele, et heli nagu "top 5 taotlusi, millised iga need, mis on ämbrid ülemisel 3 jõudluse ja iga need, mis on top 3 riigid taotlused pärit?"

**Süntaks**

   T | ülemine pesastatud N on veeru liitmise teel [;...]

**Argumendid**

* N:int - arvu ridade võrra tagasi või läheb viimine uuele tasemele. Päringu kolm taset, kus N on 5, 3 ja 3, saab 45 ridade arv.
* VEERU - veeru alusel rühmitamine liitmiseks. 
* KOONDAMINE - [koondamine funktsioon](#aggregations) rakendamiseks iga rühma read. Nende liitmised tulemuste määrab ülemise rühmi, et kuvada.


### <a name="union-operator"></a>ühendi märk

     Table1 | union Table2, Table3

Kulub kaks või enam tabelit ja tagastab nende kõigi ridade. 

**Süntaks**

    T | union [ kind= inner | outer ] [ withsource = ColumnName ] Table2 [ , ...]  

    union [ kind= inner | outer ] [ withsource = ColumnName ] Table1, Table2 [ , ...]  

**Argumendid**

* *Tabel1*, *Tabel2* ...
 *  Tabeli nime nagu `requests`, või tabel, mis on määratletud on [lasta klausel](#let-clause); või
 *  A päring avaldis, nagu näiteks`(requests | where success=="True")`
 *  Metamärkide abil määratud tabelite kogum. Näiteks `e*` moodustaksid Liidu kõik tabelid, mis on määratletud nime, kelle nimi algas "e"erandid"tabel koos eelmise anda, et punktides.
* `kind`: 
 * `inner`– Tulemus on veerud, mis on kõigile Sisestuskeel tabelid alamhulga.
 * `outer`– Tulemus on kõik veerud, mis ilmnevad mis tahes sisendeid. Lahtrid, mis on määratletud ei kuvatakse Sisestuskeel rida on seatud `null`.
* `withsource=`*ColumnName:* Kui määratud, kaasatakse väljund veeru nimega *ColumnName* väärtusega näitab, millised lähtetabel täiendusi iga rea kohta.

**Annab vastuseks**

Tabel koos nii palju ridu, kui seal on Sisestuskeel tabelite ja nii palju veerge, kui seal on kordumatu veerunimede sisendeid.

**Näide**

```AIQL

let ttrr = requests | where timestamp > ago(1h);
let ttee = exceptions | where timestamp > ago(1h);
union tt* | count
```
Kõigi tabelite, mille nimi algab "tt" Union.


**Näide**

```AIQL

union withsource=SourceTable kind=outer Query, Command
| where Timestamp > ago(1d)
| summarize dcount(UserId)
```
Erinevad kasutajad, mille on koostanud kas arv on `exceptions` sündmuse või `traces` sündmuse viimase päeva jooksul. Tulemis näitab "Lähtetabel" veeru "Päring" või "Käsk".

```AIQL
exceptions
| where Timestamp > ago(1d)
| union withsource=SourceTable kind=outer 
   (Command | where Timestamp > ago(1d))
| summarize dcount(UserId)
```

See tõhusam versioon annab sama tulemuse. See filtreerib iga tabeli enne liidu loomist.

### <a name="where-operator"></a>Kui tehtemärk

     requests | where resultCode==200

Filtreerib tabeli ridu, mis vastavad predikaat alamhulga.

**Alias (pseudonüüm)**`filter`

**Süntaks**

    T | where Predicate

**Argumendid**

* *T:* Tabeli sisendit, kelle kirjed on filtreerida.
* *Põhiliste:* A `boolean` [avaldis](#boolean) *T*veerud üle. See on *T*iga rea kohta hinnatud.

**Annab vastuseks**

*T* , mille *põhiliste* on ridade `true`.

**Näpunäited**

Parima jõudluse saamine

* **Kasutage lihtsate võrdluste** veergude nimed ja konstantide vahel. ("Pidev" tähendab konstandi üle tabeli – nii, et `now()` ja `ago()` on OK ja seega määratakse skalaarse väärtuste abil on [ `let` klausel](#let-clause).)

    Näiteks eelistate `where Timestamp >= ago(1d)` et `where floor(Timestamp, 1d) == ago(1d)`.

* **Simplest tingimustega esimene**: kui teil on mitu klauslitest Siamilaiset koos `and`, sellele esmalt osalaused, mis hõlmavad ainult ühe veeru. Nii `Timestamp > ago(1d) and OpId == EventId` on parem kui teistpidi.


**Näide**

```AIQL
traces
| where Timestamp > ago(1h)
    and Source == "Kuskus"
    and ActivityId == SubActivityIt 
```

Kirjed, mis on vanemad 1 tund, nimega "Kuskus" allikast ja on kaks veergu sama väärtusega. 

Pange tähele, et Panime kaks veergu võrdlus viimase, ei saa kasutada registri ja jõustab skannida.


### <a name="where-in-operator"></a>Kui tehtemärk

    requests | where resultCode !in (200, 201)

    requests | where resultCode in (403, 404)

**Süntaks**

    T | where col in (expr1, expr2, ...)
    T | where col !in (expr1, expr2, ...)

**Argumendid**

* `col`: Tabeli veerus.
* `expr1`...: Skalaarse avaldiste loendit.

Kasutage `in` kaasata ainult read, kus kasutatakse `col` on võrdne väljendeid `expr1...`.

Kasutage `!in` kaasata ainult read, kus `col` ei ole võrdne või väljendeid `expr1...`.  


## <a name="aggregations"></a>Liitmised

Liitmised on funktsioonidest, mida kasutatakse väärtuste [summarize toiming](#summarize-operator)loodud rühmad ühendamiseks. Dcount() on näiteks selles päringus on koondamine funktsioon:

    requests | summarize dcount(name) by success

### <a name="any"></a>mis tahes 

    any(Expression)

CEIP valib ühe rea rühma ja tagastab määratud avaldise väärtust.

See on kasulik, näiteks siis, kui mõni veerg sisaldab sarnaseid väärtusi (nt "tõrge tekst" veerus) suure hulga ja soovite selle veeru liitindeksi rühma võtme kordumatu väärtus iga proovi. 

**Näide**  

```

traces 
| where timestamp > now(-15min)  
| summarize count(), any(message) by operation_Name 
| top 10 by count_level desc 
```

<a name="argmin"></a>
<a name="argmax"></a>
### <a name="argmin-argmax"></a>argmin argmax

    argmin(ExprToMinimize, * | ExprToReturn  [ , ... ] )
    argmax(ExprToMaximize, * | ExprToReturn  [ , ... ] ) 

Otsib rea rühma, mis vähendab/maksimeerib *ExprToMaximize*ja tagastab väärtuse *ExprToReturn* (või `*` tagastamiseks kogu rida).

**Näpunäide**: möödunud kaudu veerud automaatselt ümber. Veendumaks, et kasutate õige nimed, kontrolli tulemusi abil `take 5` enne tulemuste toru teise tehtemärk.

**Näited**

Taotluse nimede kuvamine pikima taotluse toimumise:

    requests | summarize argmax(duration, timestamp) by name

Kuva kõik pikima kutse, mitte ainult ajatempli üksikasjad:

    requests | summarize argmax(duration, *) by name


Iga mõõdiku koos oma ajatempli ja muude andmete vähima väärtuse otsimiseks tehke järgmist.

    metrics 
  	| summarize minValue=argmin(value, *) 
      by name


![](./media/app-insights-analytics-reference/argmin.png)
 


### <a name="avg"></a>AVG

    avg(Expression)

Arvutab *Avaldise* rühma üle keskmise.

### <a name="buildschema"></a>buildschema

    buildschema(DynamicExpression)

Tagastab minimaalse skeem, mis võimaldab kõigi väärtuste *DynamicExpression*. 

Parameetri veerutüübi peaks olema `dynamic` -massiivi- või atribuudi sõidukeis. 

**Näide**

    exceptions | summarize buildschema(details)

Tulem:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Pange tähele, et `indexer` kasutatakse märkimiseks, kus tuleks kasutada arvuline indeks. Skeemi teatud lubatud teed (eeldades, näiteks nende indeksite on vahemikus):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)

**Näide**

Oletame, et Sisestuskeel veerg sisaldab kolme dünaamiline väärtused:

| |
|---|
|`{"x":1, "y":3.5}`
|`{"x":"somevalue", "z":[1, 2, 3]}`
|`{"y":{"w":"zzz"}, "t":["aa", "bb"], "z":["foo"]}`


Tulemiks oleva skeemi oleks:

    { 
      "x":["int", "string"], 
      "y":["double", {"w": "string"}], 
      "z":{"`indexer`": ["int", "string"]}, 
      "t":{"`indexer`": "string"} 
    }

Skeemi ütleb meile, et:

* Juurkausta objekt on neli omadused nimega ümbris x-, y-, y- ja t.
* Atribuut nimega "x", mis võiks olla tüüpi "int" või "string" tüüpi.
* Atribuut nimega "y", mis võivad kas tüüp "topelt" või mõne muu container atribuut nimega "w" tüüp "string".
* Funktsiooni ``indexer`` märksõna näitab, et "y" ja "t" on massiivid.
* Iga üksuse massiivis "y" on mõni int või string.
* "t" on stringide massiiv.
* Iga atribuut on peidetult kohustuslik ja mõni massiivi võib olla tühi.

##### <a name="schema-model"></a>Skeemi näidis

Tagastatud skeemi süntaks on järgmine:

    Container ::= '{' Named-type* '}';
    Named-type ::= (name | '"`indexer`"') ':' Type;
    Type ::= Primitive-type | Union-type | Container;
    Union-type ::= '[' Type* ']';
    Primitive-type ::= "int" | "string" | ...;

Need on samaväärsed alamhulga kodeeritud dünaamilise väärtusena TypeScript tüüp marginaalid. Masinakirjas, oleks skeemi näide:

    var someobject: 
    { 
      x?: (number | string), 
      y?: (number | { w?: string}), 
      z?: { [n:number] : (int | string)},
      t?: { [n:number]: string } 
    }


### <a name="count"></a>Count

    count([ Predicate ])

Annab vastuseks arvu, mille *põhiliste* väärtuseks ridade `true`. Kui pole *põhiliste* pole määratud, tagastab jaotises kirjete arv. 

**Näpunäide täiuslik**: kasutage `summarize count(filter)` asemel`where filter | summarize count()`

> [AZURE.NOTE] Vältige count() taotlusi, erandid või muid sündmusi, mis on ilmnenud arvu leidmiseks. Kui [valimite](app-insights-sampling.md) on kasutusel, kuvatakse need säilitatakse rakenduse ülevaated andmepunktide arv olla väiksem kui algse sündmuste arv. Selle asemel kasutada `summarize sum(itemCount)...`. Atribuudi itemCount kajastab algse sündmused, mis esindab iga säilitata andmepunkti arv.

### <a name="countif"></a>COUNTIF

    countif(Predicate)

Annab vastuseks arvu, mille *põhiliste* väärtuseks ridade `true`.

**Näpunäide täiuslik**: kasutage `summarize countif(filter)` asemel`where filter | summarize count()`

> [AZURE.NOTE] Vältige countif() taotlusi, erandid või muid sündmusi, mis on ilmnenud arvu leidmiseks. Kui [valimite](app-insights-sampling.md) on kasutusel, on andmepunktide arv olla väiksem kui tegelik sündmuste arv. Selle asemel kasutada `summarize sum(itemCount)...`. Atribuudi itemCount kajastab algse sündmuste, mis esindab iga säilitata andmepunkti.

### <a name="dcount"></a>DCount

    dcount( Expression [ ,  Accuracy ])

Tagastab *Avaldise* erinevate väärtuste arv hinnanguline rühma. (Erinevate väärtuste loend, kasutage [`makeset`](#makeset).)

*Täpsuse*, kui määratud, määrab saldo ja täpsuse.

 * `0`= vähemalt täpne ja kiireim arvutamise.
 * `1`Vaikimisi, mis nõuded täpsuse ja arvutamise ajal; kohta 0,8% tõrge.
 * `2`= kõige täpne ja parema arvutamine; kohta 0,4% tõrge.

**Näide**

    pageViews 
  	| summarize cities=dcount(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/dcount.png)


### <a name="dcountif"></a>dcountif

    dcountif( Expression, Predicate [ ,  Accuracy ])

Tagastab *avaldis* ridade erinevate väärtuste arv hinnanguline rühma, mille *põhiliste* on täidetud. (Erinevate väärtuste loend, kasutage [`makeset`](#makeset).)

*Täpsuse*, kui määratud, määrab saldo ja täpsuse.

 * `0`= vähemalt täpne ja kiireim arvutamise.
 * `1`Vaikimisi, mis nõuded täpsuse ja arvutamise ajal; kohta 0,8% tõrge.
 * `2`= kõige täpne ja parema arvutamine; kohta 0,4% tõrge.

**Näide**

    pageViews 
  	| summarize cities=dcountif(client_City, client_City startswith "St") 
      by client_CountryOrRegion


### <a name="makelist"></a>makelist

    makelist(Expr [ ,  MaxListSize ] )

Annab vastuseks on `dynamic` (JSON) *Expr* jaotises väärtused massiivi. 

* *MaxListSize* on valikuline täisarv piirangut maksimaalne arv elemente, tagastatakse (vaikimisi *128*).

### <a name="makeset"></a>makeset

    makeset(Expression [ , MaxSetSize ] )

Annab vastuseks on `dynamic` (JSON) massiivi *Expr* võtab jaotises erinevate väärtuste kogum. (Näpunäide: ainult erinevate väärtuste loendamiseks kasutage [`dcount`](#dcount).)
  
*  *MaxSetSize* on valikuline täisarvuline piirangut maksimaalne arv elemente, tagastatakse (vaikimisi *128*).

**Näide**

    pageViews 
  	| summarize cities=makeset(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/makeset.png)

Vt ka selle [ `mvexpand` tehtemärk](#mvexpand-operator) vastupidist funktsiooni.


### <a name="max-min"></a>Max, min

    max(Expr)

Arvutab *Expr*maksimumväärtus.
    
    min(Expr)

Arvutab *Avaldise*miinimum.

**Näpunäide**: see pakub min või max oma – näiteks kõrgeim või madalaim hind. Kuid kui soovite muude veergude Real – näiteks majutuse tarnija nimi – kasutage [argmin või argmax](#argmin-argmax).


<a name="percentile"></a>
<a name="percentiles"></a>
<a name="percentilew"></a>
<a name="percentilesw"></a>
### <a name="percentile-percentiles-percentilew-percentilesw"></a>protsentiili, protsentiilid, percentilew percentilesw

    percentile(Expression, Percentile)

Tagastab määratud protsentiili jaotises *avaldis* hinnang. Täpsus sõltub tihedus populatsiooni piirkonna protsentiili.
    
    percentiles(Expression, Percentile1 [ , Percentile2 ...] )

Nagu `percentile()`, kuid arvutab arvu protsentiilväärtused (mis on kiirem kui arvutamine iga protsentiili eraldi).

    percentilew(Expression, WeightExpression, Percentile)

Kaalutud protsentiil. Kasutage seda eelnevalt kokkuvõtlike andmete.  `WeightExpression`on täisarv, mis näitab, mitu algse ridade esindab iga liidetud rida.

    percentilesw(Expression, WeightExpression, Percentile1, [, Percentile2 ...])

Nagu `percentilew()`, kuid arvutab arvu protsentiilväärtused.

**Näited**


Väärtus `duration` mis on suurem kui 95% valimist seadmine ja väiksem kui 5% valimi määramine, arvutatud iga taotluse nime:

    request 
  	| summarize percentile(duration, 95)
      by name

Argument ",..." terve tabeli arvutamiseks.

Korraga mitme protsentiilid erinevate taotluse nimede arvutamine

    
    requests 
  	| summarize 
        percentiles(duration, 5, 20, 50, 80, 95) 
      by name

![](./media/app-insights-analytics-reference/percentiles.png)

Tulemused näitavad, et taotluse /Events/Index, 5% taotlusi on vastanud on väiksem kui 2.44s, pooled neist 3.52s, ja 5% on aeglasem kui 6.85s.

Mitme statistika arvutamine

    requests 
  	| summarize 
        count(), 
        avg(Duration),
        percentiles(Duration, 5, 50, 95)
      by name

#### <a name="weighted-percentiles"></a>Kaalutud protsentiilid

Saate kasutada juhul, kui andmed on eelnevalt liidetud kaalutud protsentiili funktsioone. 

Oletame näiteks, et rakenduse sooritab mitu tuhat toimingute sekundis ja soovite teada oma latentsus. Lihtne lahendus oleks luua rakenduse ülevaated kutse- või kohandatud sündmuse iga toimingu. See looks palju liiklust, kuigi kohandatava proovide tuleks muudatuste jõustumiseks vähendamiseks seda. Kuid otsustate rakendada veel paremaks lahenduse: kirjutate rakenduse liita andmed enne saatmist rakenduse ülevaated mõned koodi. Liidetud Kokkuvõte saadetakse intervalliga vähendamine andmete võib-olla mõni minut punktid.

Teie kood võtab voo latentsus mõõtmed millisekundites. Näiteks:
    
     { 15, 12, 2, 21, 2, 5, 35, 7, 12, 22, 1, 15, 18, 12, 26, 7 }

Ta loendab mõõtmed järgmist sisu:`{ 10, 20, 30, 40, 50, 100 }`

Perioodiliselt, teeb selle sarja TrackEvent kõnesid, üks iga ämber iga kõne kohandatud mõõtmetega. 

    foreach (var latency in bins.Keys)
    { telemetry.TrackEvent("latency", null, 
         new Dictionary<string, double>
         ({"latency", latency}, {"opCount", bins[latency]}}); }

Kasutusanalüüsi, kuvatakse ühe sellise rühma sündmused umbes järgmine:

`opCount` | `latency`| tähendus
---|---|---
8 | 10 | = 8 toimingute 10ms prügikast
6 | 20 | = 20 msek prügikasti 6 toimingud
3 | 30 | = 3 toimingute 30ms prügikast
1 | 40 | = 1 toimingute 40ms prügikast

Sündmuse latentsused algse jaotuse täpset pilti saamiseks kasutame `percentilesw`:

    customEvents | summarize percentilesw(latency, opCount, 20, 50, 80)

Tulemus on sama, nagu, kui kasutasime oli tavaline `percentiles` mõõtmed algse kogum.

> [AZURE.NOTE] Kaalutud protsentiilid pole rakendatav [valimisse andmed](app-insights-sampling.md), kus iga valimisse rea esindab algse read, mitte aluse juhusliku valimi. Tavaline protsentiili funktsioonide sobivad valimisse andmed.

#### <a name="estimation-error-in-percentiles"></a>Protsentiilid hinnang tõrge

Protsentiilid liitmise pakub ligikaudse väärtuse [T-Digest](https://github.com/tdunning/t-digest/blob/master/docs/t-digest-paper/histo.pdf). 

Mõned olulisi punkte. 

* Soovitud protsentiili väärtus erinevad piirid hinnang tõrketeade. Parim täpsus on [0..100] otstes mastaapimiseks, protsentiilid 0 ja 100 on täpne miinimum- ja maksimumväärtused pöördfunktsiooni väärtuse. Täpsuse väheneb järk-järgult keskel, skaala. See on kõige kehvemad veebisaidil mediaani ning ülempiir on 1%. 
* Tõrge piirid järgimine kohta rank, pole, klõpsake väärtust. Oletame, et protsentiili (X 50) tagastatud väärtus Xm. Hinnang tagab, et vähemalt 49 – kõige 51% X väärtused on väiksemad kui Xm. Ei ole teoreetiline piiratud Xm ja tegeliku keskväärtuse x vahe.

### <a name="stdev"></a>Funktsioon STDEV

     stdev(Expr)

Tagastab üle rühma *Expr* standardhälve.

### <a name="variance"></a>dispersioon

    variance(Expr)

Tagastab *avaldis* hälbe üle rühma.

### <a name="sum"></a>summa

    sum(Expr)

Tagastab *Avaldise* summa üle rühma.                      


## <a name="scalars"></a>Scalar-atribuutide

[tekitab](#casts) | [võrdlemine](#scalar-comparisons)
<br/>
[GetType](#gettype) | [räsi](#hash) | [iff](#iff) |  [isnull](#isnull) | [isnotnull](#isnotnull) | [notnull](#notnull) | [toscalar](#toscalar)

Toetatud on:

| Tüüp      | Täiendavad nimed   | .Net-i tüübi |
| --------- | -------------------- | -------------------- |
| `bool`    | `boolean`            | `System.Boolean`     |
| `datetime`| `date`               | `System.DateTime`    |
| `dynamic` |                      | `System.Object`      |
| `guid`    | `uuid`, `uniqueid`   | `System.Guid`        |
| `int`     |                      | `System.Int32`       |
| `long`    |                      | `System.Int64`       |
| `double`  | `real`               | `System.Double`      |
| `string`  |                      | `System.String`      |
| `timespan`| `time`               | `System.TimeSpan`    |

### <a name="casts"></a>Tekitab

Saate siirata ühest teise. Üldiselt, kui teisendamine on mõistlik, see ei tööta:

    todouble(10), todouble("10.6")
    toint(10.6) == 11
    floor(10.6) == 10
    toint("200")
    todatetime("2016-04-28 13:02")
    totimespan("1.5d"), totimespan("1.12:00:00")
    toguid("00000000-0000-0000-0000-000000000000")
    tostring(42.5)
    todynamic("{a:10, b:20}")

Kontrollige, kas stringi saab teisendada teatud tüüpi.

    iff(notnull(todouble(customDimensions.myValue)),
       ..., ...)

### <a name="scalar-comparisons"></a>Skalaarse võrdlemine

||
---|---
`<` |Vähem
`<=`|Väiksem või võrdne
`>` |Suurem
`>=`|Suurem või võrdne
`<>`|Ei võrdu
`!=`|Ei võrdu 
`in`| Õige operandi (dünaamiline) massiiv ja vasakut operandi on võrdne selle elemendi.
`!in`| Õige operand on (dünaamiline) massiiv ja vasakut operandi ei võrdu selle osi.




### <a name="gettype"></a>GetType

**Annab vastuseks**

String, mis tähistab ühe argumendi tõesusväärtuse aluseks salvestusruumi tüüp. See on eriti kasulik, kui teil on väärtused liiki `dynamic`: sel juhul `gettype()` paljastavad, kuidas kodeeritakse väärtus.

**Näited**

|||
---|---
`gettype("a")` |`"string" `
`gettype(111)` |`"long" `
`gettype(1==1)` |`"int8"`
`gettype(now())` |`"datetime" `
`gettype(1s)` |`"timespan" `
`gettype(parsejson('1'))` |`"int" `
`gettype(parsejson(' "abc" '))` |`"string" `
`gettype(parsejson(' {"abc":1} '))` |`"dictionary"` 
`gettype(parsejson(' [1, 2, 3] '))` |`"array"` 
`gettype(123.45)` |`"real" `
`gettype(guid(12e8b78d-55b4-46ae-b068-26d7a0080254))` |`"guid"` 
`gettype(parsejson(''))` |`"null"`
`gettype(1.2)==real` | `true`

### <a name="hash"></a>räsi

**Süntaks**

    hash(source [, mod])

**Argumendid**

* *Allikas*: allika skalaarse arvutatakse räsi.
* *Mod*: selle modulo rakendatav räsi tulemuse väärtus.

**Annab vastuseks**

Xxhash (pikk) väärtus on antud scalar modulo antud mod väärtus (kui see on määratud).

**Näited**

```
hash("World")                   // 1846988464401551951
hash("World", 100)              // 51 (1846988464401551951 % 100)
hash(datetime("2015-01-01"))    // 1380966698541616202
```
### <a name="iff"></a>IFF

Funktsiooni `iff()` funktsioon hindab esimese argumendi (predikaat) ja tagastab teine või muu argumendid, olenevalt sellest, kas predikaat väärtuse `true` või `false`. Teine ja kolmas argumendid peavad olema sama tüüpi.

**Süntaks**

    iff(predicate, ifTrue, ifFalse)


**Argumendid**

* *predikaat:* Avaldis, mis annab tulemiks on `boolean` väärtus.
* *ifTrue:* Avaldis, mida saab hinnata ja selle väärtus tagastatakse funktsiooni kui *predikaat* tulemiks `true`.
* *ifFalse:* Avaldis, mida saab hinnata ja selle väärtus tagastatakse funktsiooni kui *predikaat* tulemiks `false`.

**Annab vastuseks**

See funktsioon tagastab väärtuse *ifTrue* kui *predikaat* `true`, või *ifFalse* muidu väärtus.

**Näide**

```
iff(floor(timestamp, 1d)==floor(now(), 1d), "today", "anotherday")
```

<a name="isnull"/></a>
<a name="isnotnull"/></a>
<a name="notnull"/></a>
### <a name="isnull-isnotnull-notnull"></a>IsNull, isnotnull, notnull

    isnull(parsejson("")) == true

Ühe argumendi võtab aega ja ütleb, kas see on null.

**Süntaks**


    isnull([value])


    isnotnull([value])


    notnull([value])  // alias for isnotnull

**Annab vastuseks**

Tõene või väär olenevalt sellest, kas väärtus on tühi või pole tühi.


|x|IsNull(x)
|---|---
| "" | FALSE
|"x" | FALSE
|parsejson("")|True
|parsejson("[]")|FALSE
|parsejson("{}")|FALSE

**Näide**

    T | where isnotnull(PossiblyNull) | count

Pange tähele, et on ka muid võimalusi selle mõju saavutamiseks.

    T | summarize count(PossiblyNull)

### <a name="toscalar"></a>toscalar

Hindab päringu või avaldis, ja tagastab tulemi, kui ühe väärtuse. See funktsioon on kasulik etapiviisilise arvutused; näiteks arvutamise koguarv sündmuste ja seejärel kasutamine, et võrdlusalus.

**Süntaks**

    toscalar(query)
    toscalar(scalar)

**Annab vastuseks**

Hinnatud argument. Kui argument on tabel, tagastab funktsioon esimese rea esimesse veergu. (Hea tava on korraldada, et argument on ainult üks veerg ja rida).

**Näide**

```AIQL

    // Get the count of requests 5 days ago:
    let baseline = toscalar(requests  
        | where floor(timestamp, 1d) == floor(ago(5d),1d) | count);
    // List the counts relative to that baseline:
    requests | summarize daycount = count() by floor(timestamp, 1d)  
  	| extend relative = daycount - baseline
```




### <a name="boolean-literals"></a>Kahendmuutujaga literaalide

    true == 1
    false == 0
    gettype(true) == "int8"
    typeof(bool) == typeof(int8)

### <a name="boolean-operators"></a>Toetatud brauserid

    and 
    or 

    

## <a name="numbers"></a>Arvude

[abs](#abs) | [bin](#bin) | [exp](#exp) | [floor](#floor) | [gamma](#gamma) |[log](#log) | [rand](#rand) | [range](#range) | [sqrt](#sqrt) 
| [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

### <a name="numeric-literals"></a>Arvuline literaalide

|||
|---|---
|`42`|`long`
|`42.0`|`real`

### <a name="arithmetic-operators"></a>Aritmeetilised tehtemärgid

|| |
|---|-------------|
| + | Lisamine         |
| - | Lahutamine    |
| * | Korrutamine    |
| / | Jagamine      |
| % | Modulo      |
||
|`<` |Vähem
|`<=`|Väiksem või võrdne
|`>` |Suurem
|`>=`|Suurem või võrdne
|`<>`|Ei võrdu
|`!=`|Ei võrdu 


### <a name="abs"></a>ABS

**Süntaks**

    abs(x)

**Argumendid**

* x-täisarv, real või kuuline ajavahemik

**Annab vastuseks**

    iff(x>0, x, -x)

<a name="bin"></a><a name="floor"></a>
### <a name="bin-floor"></a>prügikasti, floor

Ümardab arvu allapoole täisarvuks väärtused kordne antud prügikasti suurus. Palju kasutatakse funktsiooni [`summarize by`](#summarize-operator) päringu. Kui teil on hajutatud väärtuste kogumi, rühmitatakse need väiksemat määratud väärtusest.

Alias (pseudonüüm) `floor`.

**Süntaks**

     bin(value, roundTo)
     floor(value, roundTo)

**Argumendid**

* *väärtus:* Arvu, kuupäeva või kuuline ajavahemik. 
* *roundTo:* "Prügikasti suurus". Arvu, kuupäeva või aeg, mille *jagab*. 

**Annab vastuseks**

*RoundTo* *väärtus*allapoole lähima kordseni.  
 
    (toint((value/roundTo)-0.5)) * roundTo

**Näited**

Avaldis | Tulemus
---|---
`bin(4.5, 1)` | `4.0`
`bin(time(16d), 7d)` | `14d`
`bin(datetime(1953-04-15 22:25:07), 1d)`|  `datetime(1953-04-15)`


Järgmine avaldis arvutab histogramm, kestus, 1 teise suurusega ämber:

```AIQL

    T | summarize Hits=count() by bin(Duration, 1s)
```

### <a name="exp"></a>exp

    exp(v)   // e raised to the power v
    exp2(v)  // 2 raised to the power v
    exp10(v) // 10 raised to the power v


### <a name="floor"></a>Floor

Alias [`bin()`](#bin).

### <a name="gamma"></a>funktsioon gamma

[Funktsioon gamma](https://en.wikipedia.org/wiki/Gamma_function)

**Süntaks**

    gamma(x)

**Argumendid**

* *x:* Reaalarv

Positiivse täisarvu jaoks `gamma(x) == (x-1)!` näiteks `gamma(5) == 4 * 3 * 2 * 1`.

Vt ka [loggamma](#loggamma).


### <a name="log"></a>log

    log(v)    // Natural logarithm of v
    log2(v)   // Logarithm base 2 of v
    log10(v)  // Logarithm base 10 of v


`v`peaks olema reaalarv > 0. Teisiti, tagastatakse tühi.

### <a name="loggamma"></a>loggamma


Absoluutväärtus [gammafunktsiooni](#gamma)naturaallogaritmi.

**Süntaks**

    loggamma(x)

**Argumendid**

* *x:* Reaalarv


### <a name="rand"></a>rand

Juhuslik arv generaator.

* `rand()`-reaalarv vahemikus 0,0 ja 1.0
* `rand(n)`-täisarv vahemikus 0 kuni n-1




### <a name="sqrt"></a>sqrt

Funktsiooni ruutjuure.  

**Süntaks**

    sqrt(x)

**Argumendid**

* *x:* Reaalarv > = 0.

**Annab vastuseks**

* Positiivne arv nii, et`sqrt(x) * sqrt(x) == x`
* `null`Kui argument on negatiivne või ei saa teisendada soovitud `real` väärtus. 




### <a name="toint"></a>toint

    toint(100)        // cast from long
    toint(20.7) == 21 // nearest int from double
    toint(20.4) == 20 // nearest int from double
    toint("  123  ")  // parse string
    toint(a[0])       // cast from dynamic
    toint(b.c)        // cast from dynamic

### <a name="tolong"></a>tolong

    tolong(20.7) == 21 // conversion from double
    tolong(20.4) == 20 // conversion from double
    tolong("  123  ")  // parse string
    tolong(a[0])       // cast from dynamic
    tolong(b.c)        // cast from dynamic


### <a name="todouble"></a>todouble

    todouble(20) == 20.0 // conversion from long or int
    todouble(" 12.34 ")  // parse string
    todouble(a[0])       // cast from dynamic
    todouble(b.c)        // cast from dynamic



## <a name="date-and-time"></a>Kuupäev ja kellaaeg


[tagasi](#ago) | [dayofmonth](#dayofmonth) | [dayofweek](#dayofweek) |  [dayofyear](#dayofyear) |[datepart](#datepart) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth)|  [getyear](#getyear) | [nüüd](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

### <a name="date-and-time-literals"></a>Kuupäeva ja kellaaja literaalide

|||
---|---
**kuupäev ja kellaaeg**|
`datetime("2015-12-31 23:59:59.9")`<br/>`datetime("2015-12-31")`|Ajad on alati UTC-vormingus. Kuupäeva faktitabeli annab aega täna.
`now()`|Praeguse kellaaja.
`now(`-*kuuline ajavahemik*`)`|`now()-`*kuuline ajavahemik*
`ago(`*kuuline ajavahemik*`)`|`now()-`*kuuline ajavahemik*
**kuuline ajavahemik**|
`2d`|2 päeva
`1.5h`|1,5-tunnise 
`30m`|30 minutiga
`10s`|10 sekundit
`0.1s`|0.1 teise
`100ms`| 100 millisekundilist
`10microsecond`|
`1tick`|100ns
`time("15 seconds")`|
`time("2")`| 2 päeva
`time("0.12:34:56.7")`|`0d+12h+34m+56.7s`

### <a name="date-and-time-expressions"></a>Kuupäeva ja kellaaja avaldised

Avaldis |Tulemus
---|---
`datetime("2015-01-02") - datetime("2015-01-01")`| `1d`
`datetime("2015-01-01") + 1d`| `datetime("2015-01-02")`
`datetime("2015-01-01") - 1d`| `datetime("2014-12-31")`
`2h * 24` | `2d`
`2d` / `2h` | `24`
`datetime("2015-04-15T22:33") % 1d` | `timespan("22:33")`
`bin(datetime("2015-04-15T22:33"), 1d)` | `datetime("2015-04-15T00:00")`
||
`<` |Vähem
`<=`|Väiksem või võrdne
`>` |Suurem
`>=`|Suurem või võrdne
`<>`|Ei võrdu
`!=`|Ei võrdu 




### <a name="ago"></a>tagasi

Lahutab antud kuuline ajavahemik algab UTC praeguse kellaaja. Nagu `now()`, seda funktsiooni saab kasutada mitu korda lauses, ja viitab UTC kellaaja saab sama kõik instantiations.

**Süntaks**

    ago(a_timespan)

**Argumendid**

* *a_timespan*: intervall lahutada UTC praeguse kellaaja (`now()`).

**Annab vastuseks**

    now() - a_timespan

**Näide**

Kõik read koos ajatempel tunnis:

```AIQL

    T | where timestamp > ago(1h)
```

### <a name="datepart"></a>DatePart

    datepart("Day", datetime(2015-12-14)) == 14

Ekstraktib kuupäeva täisarv määratud osa.

**Süntaks**

    datepart(part, datetime)

**Argumendid**

* `part:String`-{"Year", "Kuu", "Päev", "Tund", "Minute", "Teise", "Millisekundilist", "Microsecond", "Nanosecond"}
* `datetime`

**Annab vastuseks**

Pikk, mis tähistab määratud osa.


### <a name="dayofmonth"></a>dayofmonth

    dayofmonth(datetime("2016-05-15")) == 15 

Kuu päeva järjenumber.

**Süntaks**

    dayofmonth(a_date)

**Argumendid**

* `a_date`: A `datetime`.


### <a name="dayofweek"></a>dayofweek

    dayofweek(datetime("2015-12-14")) == 1d  // Monday

Päeva pärast eelmist täisarvuline pühapäev, nagu on `timespan`.

**Süntaks**

    dayofweek(a_date)

**Argumendid**

* `a_date`: A `datetime`.

**Annab vastuseks**

Funktsiooni `timespan` Kuna keskööst alguses eelmise pühapäev, ümardada päevade täisarv.

**Näited**

```AIQL
dayofweek(1947-11-29 10:00:05)  // time(6.00:00:00), indicating Saturday
dayofweek(1970-05-11)           // time(1.00:00:00), indicating Monday
```

### <a name="dayofyear"></a>dayofyear

    dayofyear(datetime("2016-05-31")) == 152 
    dayofyear(datetime("2016-01-01")) == 1 

Aasta päeva järjenumber.

**Süntaks**

    dayofyear(a_date)

**Argumendid**

* `a_date`: A `datetime`.

<a name="endofday"></a><a name="endofweek"></a><a name="endofmonth"></a><a name="endofyear"></a>
### <a name="endofday-endofweek-endofmonth-endofyear"></a>endofday endofweek, endofmonth, endofyear

    dt = datetime("2016-05-23 12:34")

    endofday(dt) == 2016-05-23T23:59:59.999
    endofweek(dt) == 2016-05-28T23:59:59.999 // Saturday
    endofmonth(dt) == 2016-05-31T23:59:59.999 
    endofyear(dt) == 2016-12-31T23:59:59.999 


### <a name="getmonth"></a>getmonth

Saada month number (1 – 12) on kuupäev ja kellaaeg.

**Näide**

    ... | extend month = getmonth(datetime(2015-10-12))

    --> month == 10

### <a name="getyear"></a>getyear

Aasta toomine on kuupäev ja kellaaeg.

**Näide**

    ... | extend year = getyear(datetime(2015-10-12))

    --> year == 2015

### <a name="now"></a>Nüüd

    now()
    now(-2d)

Praeguse kellaaja UTC kell, võite asendada antud kuuline ajavahemik. Seda funktsiooni saab kasutada mitu korda lauses ja viitab kellaaega on sama kõik eksemplarid.

**Süntaks**

    now([offset])

**Argumendid**

* *offset:* A `timespan`, lisatakse UTC praeguse kellaaja. Vaikimisi: 0.

**Annab vastuseks**

Kui UTC praeguse kellaaja lisamine `datetime`.

    now() + offset

**Näide**

Määratleb intervalli sündmus, mis on tähistatud predikaat alates:

```AIQL
T | where ... | extend Elapsed=now() - timestamp
```

<a name="startofday"></a><a name="startofweek"></a><a name="startofmonth"></a><a name="startofyear"></a>
### <a name="startofday-startofweek-startofmonth-startofyear"></a>startofday startofweek, startofmonth, startofyear

    date=datetime("2016-05-23 12:34:56")

    startofday(date) == datetime("2016-05-23")
    startofweek(date) == datetime("2016-05-22") // Sunday
    startofmonth(date) == datetime("2016-05-01")
    startofyear(date) == datetime("2016-01-01")



### <a name="todatetime"></a>todatetime

Alias (pseudonüüm) `datetime()`.

     todatetime("2016-03-28")
     todatetime("03/28/2016")
     todatetime("2016-03-28 14:34:00")
     todatetime("03/28/2016 2:34pm")
     todatetime("2016-03-28T14:34.5Z")
     todatetime(a[0]) 
     todatetime(b.c) 

Kontrollige, kas string on kehtiv kuupäev.

     iff(notnull(todatetime(customDimensions.myDate)),
         ..., ...)


### <a name="totimespan"></a>totimespan

Alias (pseudonüüm) `timespan()`.

    totimespan("21d")
    totimespan("21h")
    totimespan(request.duration)

### <a name="weekofyear"></a>weekofyear

    weekofyear(datetime("2016-05-14")) == 21
    weekofyear(datetime("2016-01-03")) == 1
    weekofyear(datetime("2016-12-31")) == 53

Täisarv tulem tähistab nädalanumbri ISO 8601 standard järgi. Nädala esimene päev on pühapäev ja aasta esimese nädala on selle nädala, mis sisaldab selle aasta esimene neljapäev. (Viimase päeva aastas seetõttu võib sisaldada osa nädalapäevade 1 järgmise aasta või esimese päeva võib sisaldada osa 52 või 53 eelmise aasta nädal.)


## <a name="string"></a>String

[countof](#countof) | [eraldamiseks](#extract) | [extractjson](#extractjson)  | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty) | [parseurl](#parseurl) | [asendamine](#replace) | [tükeldamine](#split) | [strcat](#strcat) | [strlen](#strlen) | [alamstringi](#substring) | [tolower](#tolower) | [tostring](#tostring) | [toupper](#toupper)


### <a name="string-literals"></a>Stringi literaalide

Reeglid, mis on sama mis JavaScripti.

Stringide võib ümbritsetud kas ühe- või kahekordseks hinnapakkumise märgid. 

Kurakriipsu (`\`) kasutatakse pääseda märki, näiteks `\t` (tab), `\n` (newline) ja eksemplarid, lisades hinnapakkumise märk.

* `'this is a "string" literal in single \' quotes'`
* `"this is a 'string' literal in double \" quotes"`
* `@"C:\backslash\not\escaped\with @ prefix"`

### <a name="obfuscated-string-literals"></a>Muudetud stringi literaalide

Muudetud stringi literaalide on stringid, mida Analytics on segane kui kirjutamine string (näiteks jälitamine). Segadust protsessi asendab kõik muudetud märki, algus (`*`) märgi.

Muudetud stringi, sõnasõnaline moodustamiseks prepend `h` või "H". Näiteks:

```
h'hello'
h@'world' 
h"hello"
```

### <a name="string-comparisons"></a>Stringvõrdluses

Tehtemärk|Kirjeldus|Tõstutundlik|True näide
---|---|---|---
`==`|Võrdub |Jah| `"aBc" == "aBc"`
`<>` `!=`|Ei võrdu|Jah| `"abc" <> "ABC"`
`=~`|Võrdub |Ei| `"abc" =~ "ABC"`
`!~`|Ei võrdu |Ei| `"aBc" !~ "xyz"`
`has`|Parempoolne-vasakpoolne-side (RHS) on kogu termin vasak-vasakpoolne-side (vasak)|Ei| `"North America" has "america"`
`!has`|RHS pole täielik Termini LHS|Ei|`"North America" !has "amer"` 
`hasprefix`|RHS on eesliide LHS Termini|Ei|`"North America" hasprefix "ame"`
`!hasprefix`|RHS ei ole termin, mis tahes LHS eesliide|Ei|`"North America" !hasprefix "mer"`
`hassuffix`|RHS on termini LHS järelliide|Ei|`"North America" hassuffix "rth"`
`!hassuffix`|RHS ei ole termin, mis tahes LHS järelliite|Ei|`"North America" !hassuffix "mer"`
`contains` | RHS esineb alamstringi LHS|Ei| `"FabriKam" contains "BRik"`
`!contains`| RHS ei esine LHS|Ei| `"Fabrikam" !contains "xyz"`
`containscs` | RHS esineb alamstringi LHS|Jah| `"FabriKam" contains "Kam"`
`!containscs`| RHS ei esine LHS|Jah| `"Fabrikam" !contains "Kam"`
`startswith`|RHS on ka algsel alamstringi LHS.|Ei|`"Fabrikam" startswith "fab"`
`!startswith`|RHS pole mõni algse alamstringi LHS.|Ei|`"Fabrikam" !startswith "abr"`
`endswith`|RHS on terminal alamstringi, LHS.|Ei|`"Fabrikam" endswith "kam"`
`!endswith`|RHS pole terminal alamstringi, LHS.|Ei|`"Fabrikam" !endswith "ka"`
`matches regex`|LHS sisaldab vastet RHS|Jah| `"Fabrikam" matches regex "b.*k"`
`in`|Mis tahes elemente võrdne|Jah|`"abc" in ("123", "345", "abc")`
`!in`|Mis tahes elemente ei võrdu|Jah|`"bc" !in ("123", "345", "abc")`

Kasutage `has` või `in` kui sa testida olemasolu kogu leksikaalse Termini - st sümbol või mõne tähtedest ja numbritest koosnev Wordi määratletud mittetärke või algusest või lõpust välja. `has`sooritab kiiremini `contains`, `startswith` või `endswith`. Esimene need päringud on kiirem:

    EventLog | where continent has "North" | count;
    EventLog | where continent contains "nor" | count





### <a name="countof"></a>countof

    countof("The cat sat on the mat", "at") == 3
    countof("The cat sat on the mat", @"\b.at\b", "regex") == 3

Loendab esinemiskordade stringi alamstringi. Ainult string vasteid võivad kattuda; regex vasteid ei ole.

**Süntaks**

    countof(text, search [, kind])

**Argumendid**

* *teksti:* String.
* *Otsing:* Ainult string või tavalise avaldis sobitamiseks *teksti*sees.
* *selline:* `"normal"|"regex"` Vaikimisi `normal`. 

**Annab vastuseks**

Mitu korda, mille saab vastendada otsingustringi ümbrises. Ainult string vasteid võivad kattuda; regex vasteid ei ole.

**Näited**

|||
|---|---
|`countof("aaa", "a")`| 3 
|`countof("aaaa", "aa")`| 3 (mitte 2!)
|`countof("ababa", "ab", "normal")`| 2
|`countof("ababa", "aba")`| 2
|`countof("ababa", "aba", "regex")`| 1
|`countof("abcabc", "a.c", "regex")`| 2
    



### <a name="extract"></a>eraldamiseks

    extract("x=([0-9.]+)", 1, "hello x=45.6|wo") == "45.6"

Vaste on [Lihtavaldise](#regular-expressions) toomine tekstistring. Soovi korral võite seda siis teisendab ekstraktitud alamstringi näidatud tüüp.

**Süntaks**

    extract(regex, captureGroup, text [, typeLiteral])

**Argumendid**

* *regex:* [Lihtavaldise](#regular-expressions).
* *captureGroup:* Positiivne `int` konstandi näitab jäädvustada rühm eraldamiseks. 0 tähistab kogu vastet, sobitada esimene "(" ümarsulg")" Lihtavaldise, 2 või rohkem edaspidised sulgude väärtuseks 1.
* *teksti:* A `string` otsimiseks.
* *typeLiteral:* Mõni valikuline tüüp literaalmärgid (nt `typeof(long)`). Kui see on esitatud teisendatakse ekstraktitud alamstringi seda tüüpi. 

**Annab vastuseks**

Kui *regex* vaste *teksti*: alamstringi, sobitada vastu näidatud jäädvustada rühma *captureGroup*, võite teisendatakse *typeLiteral*.

Kui ei leita, või tippige teisendus nurjub: `null`. 

**Näited**

Näide stringi `Trace` otsitakse definitsiooni jaoks `Duration`. Funktsiooni match teisendatakse `real`, siis korrutatakse aeg konstandi (`1s`) nii, et `Duration` tüüp on `timespan`. Selles näites on võrdne 123.45 sekundit:

```AIQL
...
| extend Trace="A=1, B=2, Duration=123.45, ..."
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s) 
```

Selles näites on võrdne `substring(Text, 2, 4)`:

```AIQL
extract("^.{2,2}(.{4,4})", 1, Text)
```

<a name="notempty"></a>
<a name="isnotempty"></a>
<a name="isempty"></a>
### <a name="isempty-isnotempty-notempty"></a>isEmpty, isnotempty, notempty

    isempty("") == true

True, kui argument on tühi string või on tühi.
Vt ka [isnull](#isnull).


**Süntaks**

    isempty([value])


    isnotempty([value])


    notempty([value]) // alias of isnotempty

**Annab vastuseks**

Näitab, kas see argument on isnull või tühja stringi.

|x|isEmpty(x)
|---|---
| "" | True
|"x" | FALSE
|parsejson("")|True
|parsejson("[]")|FALSE
|parsejson("{}")|FALSE


**Näide**


    T | where isempty(fieldName) | count


### <a name="parseurl"></a>parseurl

URL-i osade tükeldada.

**Süntaks**

    parseurl(urlstring)

**Argumendid**

* *urlstring:* URL-I.

**Annab vastuseks**

Objekti, mis sisaldavad osade stringidena.

**Näide**

    parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")

    {
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
    }

### <a name="replace"></a>asendamine

Asendage kõik regex vasteid teise stringi.

**Süntaks**

    replace(regex, rewrite, text)

**Argumendid**

* *regex:* [Lihtavaldise](https://github.com/google/re2/wiki/Syntax) Otsitav *tekst*. See võib sisaldada jäädvustada rühmade sulgudes"(" ")". 
* *ümberkirjutamine:* Mis tahes vaste tehtud *matchingRegex*regex asendamine. Kasutage `\0` viidata kogu vastet, `\1` jäädvustada esimese rühma, `\2` jne edaspidised jäädvustada rühmade jaoks.
* *teksti:* String.

**Annab vastuseks**

pärast kõigi vasteid *regex* asendamine hinnanguid *ümberkirjutamine* *teksti* . Vasteid ei kattuks.

**Näide**

See lause:

```AIQL
range x from 1 to 5 step 1
| extend str=strcat('Number is ', tostring(x))
| extend replaced=replace(@'is (\d+)', @'was: \1', str)
```

On järgmised tulemused.

| x    | Str | asendatakse|
|---|---|---|
| 1    | Arv on 1.000000  | Oli: 1.000000|
| 2    | Arv on 2.000000  | Oli: 2.000000|
| 3    | Arv on 3.000000  | Oli: 3.000000|
| 4    | Arv on 4.000000  | Oli: 4.000000|
| 5    | Arv on 5.000000  | Oli: 5.000000|
 



### <a name="split"></a>tükeldatud

    split("aaa_bbb_ccc", "_") == ["aaa","bbb","ccc"]

Jagab antud string vastavalt antud eraldaja ja tagastab stringi massiivi conatined kasutatavat võrdluslaadi abil. Soovi korral võib olla teatud alamstringi tagastatud kui on olemas.

**Süntaks**

    split(source, delimiter [, requestedIndex])

**Argumendid**

* *Allikas*: allika string, mis on antud eraldaja järgi kahasse.
* *eraldaja*: selleks, et jagada allika stringi kasutatav eraldaja.
* *requestedIndex*: valikuline 0-põhine registri `int`. Kui, sisaldab tagastatud stringi massiivi nõutud alamstringi kui on olemas. 

**Annab vastuseks**

Stringi massiiv, mis sisaldab kasutatavat antud andmeallika stringi võrdluslaadi, mis on eraldatud antud eraldaja alusel.

**Näited**

```
split("aa_bb", "_")           // ["aa","bb"]
split("aaa_bbb_ccc", "_", 1)  // ["bbb"]
split("", "_")                // [""]
split("a__b")                 // ["a","","b"]
split("aabbcc", "bb")         // ["aa","cc"]
```




### <a name="strcat"></a>strcat

    strcat("hello", " ", "world")

Ühendab 1 – 16 argumendid, mis peab olema stringide vahel.

### <a name="strlen"></a>strlen

    strlen("hello") == 5

Stringi pikkus.

### <a name="substring"></a>Alamstringi

    substring("abcdefg", 1, 2) == "bc"

Alates antud index antud andmeallika stringi alamstringi ekstraktida. Soovi korral saate määrata nõutud alamstringi pikkus.

**Süntaks**

    substring(source, startingIndex [, length])

**Argumendid**

* *Allikas:* Andmeallika string, mis on alamstringi sai.
* *startingIndex:* 0-põhine märgi alguskoha nõutud alamstringi.
* *pikkus:* Valikuline parameeter, mida saate kasutada funktsiooni alamstringi soovitud märkide arvu määramiseks. 

**Annab vastuseks**

Alamstringi antud string. Funktsiooni alamstringi algab startingIndex (0-põhine) märgi paigutus ja jätkuvalt lõppu tekstistring või pikkus märke, kui määratud.

**Näited**

```
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
```

### <a name="tolower"></a>tolower

    tolower("HELLO") == "hello"

Teisendab alumise juhul string.

### <a name="toupper"></a>toupper

    toupper("hello") == "HELLO"

Teisendab tekstistringi läbivate suurtähtedega.



### <a name="guids"></a>GUID

    guid(00000000-1111-2222-3333-055567f333de)


## <a name="arrays-objects-and-dynamic"></a>Massiive, objektide ja dünaamiline

[literaalide](#dynamic-literals) | [casting](#casting-dynamic-objects) | [tehtemärgid](#operators) | [lasta osalaused](#dynamic-objects-in-let-clauses)
<br/>
[arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [vahemikus](#range) | [treepath](#treepath) | [todynamic](#todynamic) | [zip](#zip)


Siin ongi tulemus erandi rakenduse ülevaated päring. Väärtus `details` on massiiv.

![](./media/app-insights-analytics-reference/310.png)

**Indekseerimine:** Registri massiivid ja objektid, just nagu JavaScripti.

    exceptions | take 1
  	| extend 
        line = details[0].parsedStack[0].line,
        stackdepth = arraylength(details[0].parsedStack)

* Kasutades `arraylength` ja muud Analytics funktsioonid (mitte ".length"!)

**Casting** Mõnel juhul tuleb oma hääle element, mis teil ekstraktida objekti, kuna selle tüüpi võib muutuda. Näiteks `summarize...to` vajab teatud tüüpi:

    exceptions 
  	| summarize count() 
      by toint(details[0].parsedStack[0].line)

    exceptions 
  	| summarize count() 
      by tostring(details[0].parsedStack[0].assembly)

**Literaalide** Konkreetsete massiivi või Atribuudikonteineri objekti loomine, kirjutage JSON-stringi ja cast:

    todynamic('[{"x":"1", "y":"32"}, {"x":"6", "y":"44"}]')


**mvexpand:** Tõmmake objekti atribuutide eraldi ridadesse, saate kasutada mvexpand.

    exceptions | take 1 
  	| mvexpand details[0].parsedStack[0]


![](./media/app-insights-analytics-reference/410.png)


**treepath:** Kõik teed keerukate objektide otsimine

    exceptions | take 1 | project timestamp, details 
  	| extend path = treepath(details) 
  	| mvexpand path


![](./media/app-insights-analytics-reference/420.png)

**buildschema:** Minimaalne skeem, mis võimaldab kõigi väärtuste tabeli avaldise leidmiseks tehke järgmist.

    exceptions | summarize buildschema(details)

Tulem:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Pange tähele, et `indexer` kasutatakse märkimiseks, kus tuleks kasutada arvuline indeks. Skeemi teatud lubatud teed (eeldades, näiteks nende indeksite on vahemikus):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)



### <a name="array-and-object-literals"></a>Massiivi ja objekti literaalide

Dünaamiliste literaalmärgid loomiseks kasutage `parsejson` (alias (pseudonüüm) `todynamic`) koos mõne JSON tekstistringi argumenti:

* `parsejson('[43, 21, 65]')`-arvumassiivi
* `parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')` 
* `parsejson('21')`-dünaamiline tüüp, mis sisaldab numbrit ühe väärtuse
* `parsejson('"21"')`-dünaamiline tüüp, mis sisaldavad stringi ühe väärtuse

Teate, et erinevalt JavaScripti, JSON mandaatide jutumärke kasutamine (`"`) stringide ümber. Seetõttu on üldiselt lihtsam hinnapakkumise JSON-kodeeringuga stringi literaalide kasutamise ühe-hinnapakkumised (`'`).

Selles näites loob dünaamilise väärtuse ja kasutab selle väljad:

```

T
| extend person = parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')
| extend n = person.name, add = person.address.street
```


### <a name="dynamic-object-functions"></a>Dünaamiliste objekti funktsioonid

|||
|---|---|
| *väärtus* `in` *massiiv*| True, kui seal on *massiiv* element, mis == *väärtus*<br/>`where City in ('London', 'Paris', 'Rome')`
| *väärtus* `!in` *massiiv*| Väärtus TRUE, kui *massiiv* on pole mis == *väärtus*
|[`arraylength(`massiiv`)`](#arraylength)| Kui see pole massiivi null
|[`extractjson(`tee, objekt`)`](#extractjson)|Kasutab liikuda objekti tee.
|[`parsejson(`Allikas`)`](#parsejson)| JSON-stringi muutub dünaamilise objekti.
|[`range(`saatja, adressaat, juhis`)`](#range)| Väärtused massiivi
|[`mvexpand`listColumn](#mvexpand-operator) | Tiražeerib üht rida iga loendis määratud lahtri väärtuse.
|[`summarize buildschema(`veerg`)`](#buildschema) |Tuletab tüüp skeemi veeru sisu:
|[`summarize makelist(`veeru`)`](#makelist)| Lömastab rühmad ridade ja paigutab selle veeru väärtuste massiivi.
|[`summarize makeset(`veerg`)`](#makeset) | Rühmade ridade lömastab ja paigutab veeru väärtused massiivi ilma kordamise.

### <a name="dynamic-objects-in-let-clauses"></a>Dünaamiliste objektide anda, et punktides


[Lasta klauslitest](#let-clause) poe dünaamiline väärtused stringidena, nii need kaks klauslitest võrdväärse ja mõlemad on vaja selle `parsejson` (või `todynamic`) enne kasutamist lähimaks:

    let list1 = '{"a" : "somevalue"}';
    let list2 = parsejson('{"a" : "somevalue"}');

    T | project parsejson(list1).a, parsejson(list2).a




### <a name="arraylength"></a>arraylength

Dünaamiliste massiivi elementide arv.

**Süntaks**

    arraylength(array)

**Argumendid**

* *massiiv:* A `dynamic` väärtus.

**Annab vastuseks**

*Massiiv*, elementide arv või `null` kui *massiivis* pole massiiv.

**Näited**

```
arraylength(parsejson('[1, 2, 3, "four"]')) == 4
arraylength(parsejson('[8]')) == 1
arraylength(parsejson('[{}]')) == 1
arraylength(parsejson('[]')) == 0
arraylength(parsejson('{}')) == null
arraylength(parsejson('21')) == null
```



### <a name="extractjson"></a>extractjson

    extractjson("$.hosts[1].AvailableMB", EventText, typeof(int))

Määratud elementi JSON teksti, kasutades tee avaldist välja tuua. Soovi korral teisendada ekstraktitud stringi teatud tüüpi.


**Süntaks**

```

    string extractjson(jsonPath, dataSource) 
    resulttype extractjson(jsonPath, dataSource, typeof(resulttype))
```


**Annab vastuseks**

See funktsioon teeb JsonPath päringu andmeallikas, mis sisaldab kehtiv JSON-stringi, võite selle väärtuse teisendamine mõnda muud tüüpi sõltuvalt Kolmas argument sisse.



**Näide**

[Nurksulgudega] märke ja dot märkega on samaväärne.

    ... | extend AvailableMB = extractjson("$.hosts[1].AvailableMB", EventText, typeof(int)) | ...

    ... | extend AvailableMD = extractjson("$['hosts'][1]['AvailableMB']", EventText, typeof(int)) | ...



**Jõudluse näpunäited**

* Where-klauslitest rakendada enne abil`extractjson()`
* Soovitame kasutada koos [ekstraktida](#extract) Lihtavaldise vaste. See saab käitada väga palju kiiremini ja kehtib juhul, kui selle JSON toodetud malli.
* Kasutage `parsejson()` kui teil on vaja rohkem kui ühe väärtuse ekstraktida soovitud JSON.
* Kaaluge võimalust, võttes JSON, sõeluda veebisaidil manustamisest deklareerimise veeru tüüp olema dünaamiline.

### <a name="json-path-expressions"></a>JSON tee avaldised

|||
|---|---|
|`$`|Root objekt|
|`@`|Praegune objekt|
|`[0]`|Massiivi allindeks|
|`.`või`[0]` | Lapse|

*(Me pole praegu rakendada metamärke, nii kui, union või sektoritele.)*




### <a name="parsejson"></a>parsejson

Tõlgendab on `string` [JSON-väärtus](http://json.org/)) ja tagastab vastava väärtuse nimega `dynamic`. See on parem abil `extractjson()` kui vajate rohkem kui üks osa JSON liitindeksi objekti eraldamiseks.

**Süntaks**

    parsejson(json)

**Argumendid**

* *json:* JSON dokument.

**Annab vastuseks**

Objekti tüüp `dynamic` *json*määratud.

**Näide**

Järgmises näites, kui `context_custom_metrics` on mõne `string` , et näeb välja järgmine: 

```
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

Järgmine fragment toob väärtuse ja seejärel soovitud `duration` pesa objekti ning et see toob pesast, `duration.value` ja  `duration.min` (`118.0` ja `110.0`vastavalt).

```AIQL
T
| ...
| extend d=parsejson(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```



### <a name="range"></a>vahemik

Funktsiooni `range()` funktsioon (olevat segi selle `range` tehtemärk) genereeritud dünaamiliste massiiv, hoides sarja väärtuste võrdsel.

**Süntaks**

    range(start, stop, step)

**Argumendid**

* *Alustamine:* Tulemuseks massiivi esimese elemendi väärtuse. 
* *peatamine:* Tulemuseks massiivi viimase elemendi väärtuse või vähim väärtus, mis on suurem kui viimase elemendi massiivis tulemiks oleva ja sees täisarv kordne *toimingu* *käivitamine*.
* *samm:* Kaks järjestikust elementide massiivi vahe.

**Näited**

Järgmine näide tagastab `[1, 4, 7]`:

```AIQL
range(1, 8, 3)
```

Järgmine näide tagastab massiivi hoides kogu päeva aastas 2015:

```AIQL

    range(datetime(2015-01-01), datetime(2015-12-31), 1d)
```

### <a name="todynamic"></a>todynamic

    todynamic('{"a":"a1", "b":["b1", "b2"]}')

Teisendab tekstistringi dünaamiline väärtus.

### <a name="treepath"></a>treepath

    treepath(dynamic_object)

Loetleb kõik lehed dünaamilise objekti tuvastamine tee avaldised. 

**Annab vastuseks**

Massiivi tee avaldised.

**Näited**

    treepath(parsejson('{"a":"b", "c":123}')) 
    =>       ["['a']","['c']"]
    treepath(parsejson('{"prop1":[1,2,3,4], "prop2":"value2"}'))
    =>       ["['prop1']","['prop1'][0]","['prop2']"]
    treepath(parsejson('{"listProperty":[100,200,300,"abcde",{"x":"y"}]}'))
    =>       ["['listProperty']","['listProperty'][0]","['listProperty'][0]['x']"]

Märkus selle "[0]" viitab massiivi, kuid ei määra index, mis kasutavad teatud tee.

### <a name="zip"></a>ZIP

    zip(list1, list2, ...)

Ühendab kogumi loendite kordseid üks loend.

* `list1...`: Väärtuste loend

**Näited**

    zip(parsejson('[1,3,5]'), parsejson('[2,4,6]'))
    => [ [1,2], [3,4], [5,6] ]

    
    zip(parsejson('[1,3,5]'), parsejson('[2,4]'))
    => [ [1,2], [3,4], [5,null] ]


### <a name="names"></a>Nimed

Nimede võib olla kuni 1024 märki. Need on tõstutundlikud ja võib sisaldada tähti, numbreid ja allakriipsutatud märkideks (`_`). 

Hinnapakkumise nimi kasutades ["… '] or [" ... "] muude klaviatuurilt puuduvate märkide kaasamiseks või kasutage märksõna nimena. Näiteks:

```AIQL

    requests | 
    summarize  ["distinct urls"] = dcount(name) // non-alphanumerics
    by  ['where'] = client_City, // using a keyword as a name
        ['outcome!'] = success // non-alphanumerics
```


|||
|---|---|
|["tee\\file\n\'x\'"] | Kasutage \ pääseda märki|
|["d-e.=/f#\n"] | |
|[@'path\file'] | Pole põgenemine - \ on sõnasõnaline|
|[@"\now& siis\"] | |
|[kui] | Kasutades märksõna keele nimi|

[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


