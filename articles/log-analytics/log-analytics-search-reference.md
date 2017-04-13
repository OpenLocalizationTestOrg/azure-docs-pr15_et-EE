<properties
    pageTitle="Log Analytics otsida viide | Microsoft Azure'i"
    description="Log Analytics otsingu viide kirjeldatakse otsingu keel ja pakub üldist päringusüntaksit suvandid, mida saab kasutada, kui andmete otsimine ja filtreerimine avaldiste abil saate otsingut kitsendada."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="log-analytics-search-reference"></a>Logige Analytics otsingu viide

Viide järgmisest jaotisest otsingu keele kohta kirjeldatakse üldist päringu süntaks suvandite abil saate andmete otsimine ja filtreerimine avaldiste abil saate otsingut kitsendada. Samuti kirjeldatakse käsud, mille abil saate tuua andmeid vastavalt tegutseda.

Otsingud ja aspekte, mis aitavad teil tilli tagastatud väljade kohta saate lugeda-andmete [otsinguväljale ja viitamine jaotises tahukas](#search-field-and-facet-reference)sarnaseid kategooriateks.

## <a name="general-query-syntax"></a>Üldine päringusüntaksit

Süntaks:

```
filterExpression | command1 | command2 …
```

Filtri avaldisel (`filterExpression`) määratleb "kui" tingimus päringu. Päringu tulemite käske rakendada. Mitme käsu eraldada püstkriips (|).

### <a name="general-syntax-examples"></a>Üldine süntaks näited

Näited:

```
system
```

See päring tagastab tulemused, mis sisaldavad sõna "system" väljal, mis on indekseeritud täisteksti või terminite otsimine.

>[AZURE.NOTE] Kõik väljad on indekseeritud sellisel viisil, kuid kõige levinum teksti väljade (nt kirjeldused ja nimed) tavaliselt oleks.

```
system error
```

See päring tagastab tulemused, mis sisaldavad sõna *süsteem* ja *tõrge*.

```
system error | sort ManagementGroupName, TimeGenerated desc | top 10
```

See päring tagastab tulemused, mis sisaldavad sõna *süsteem* ja *tõrge*. See sorditakse tulemused *ManagementGroupName* välja (tõusvas järjestuses) ja seejärel *TimeGenerated* (jaotises laskuvas järjestuses). Kulub ainult esimese 10 tulemused.

>[AZURE.IMPORTANT] Kõikide väljade nimed ja stringi ja teksti väljade väärtused on tõstutundlikud.

## <a name="filter-expression"></a>Filtri avaldisel

Alltoodud selgitavad avaldiste filter.

### <a name="string-literals"></a>Stringi literaalide

Stringi sõnasõnaline on mis tahes string, mis ei tuvasta parseri soovitud märksõna või eelmääratletud andmetüüp (nt arv või kuupäev).

Näited:

```
These all are string literals
```

Selle päringu otsib tulemeid, mis sisaldavad esinemiskordade viis kõiki sõnu. Keerukate stringi otsingu tegemiseks ümbritsege stringi sõnasõnaline jutumärkidega, näiteks:

```
" Windows Server"
```

See ainult tagastab tulemi "Windows Server" täpset vastet.

### <a name="numbers"></a>Arvude

Funktsiooni parseri toetab kümnendarv ja ujukoma arv süntaks arvulised väljad.

Näited:

```
Type:Perf 0.5
```

```
HTTP 500
```

### <a name="datetime"></a>Kuupäev/kellaaeg

Iga tekstilõigu ka andmeid on *TimeGenerated* atribuut, mis esindab algse kuupäeva ja kellaaja kirje. Teatud tüüpi andmete Lisaks võib olla mitu kuupäeva/kellaaja väljad (nt *LastModified*).

Ajaskaala diagrammi/kellaaeg valija Log Analytics näitab jaotumine tulemused aja jooksul (vastavalt praeguse päringu käivitatud), põhineb väli *TimeGenerated* . Kuupäeva/kellaaja väljad on teatud stringi vormingut, mida saab kasutada päringute piiritlemine kindla ajavahemiku päring. Süntaksi abil viidata suhteline ajavahemike (näiteks "3 päeva tagasi ja vahel 2 tunni taguse").

Süntaks:

```
yyyy-mm-ddThh:mm:ss.dddZ
```

```
yyyy-mm-ddThh:mm:ss.ddd
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm
```

```
yyyy-mm-dd
```


Näide:

```
TimeGenerated:2013-10-01T12:20
```

Eelmise käsk tagastab ainult kirjete *TimeGenerated* väärtus on täpselt 12:20 1 oktoobris 2013. See on tõenäoline, et see annab tulemuse, kuid teil mõista idee.

Funktsiooni parseri toetab samuti Mnemoonika väärtuse kuupäev/kellaaeg kohe.


Uuesti, on tõenäoline, et see yield tulemuse, kuna andmeid ei tee seda et kiiresti süsteemis.

Need näited on absoluutne ja suhteline kuupäevade jaoks. Järgmise kolme punktides, kuvatakse selgitab, kuidas neid kasutada täpsemaid filtreid näited, kus kasutatakse suhteline kuupäevavahemikke.

### <a name="datetime-math"></a>Kuupäeva/kellaaja matemaatika

Kuupäeva/kellaaja matemaatilisi tehtemärke abil offset või kuupäeva/kellaaja lihtsate arvutuste abil kuupäeva/kellaaja väärtuse ümardada.

Süntaks:

```
datetime/unit
```

```
datetime[+|-]count unit
```

|Tehtemärk|Kirjeldus|
|---|---|
|/|Ümardab arvu määratud üksusele kuupäev ja kellaaeg. Näide: Nüüd / päev Ümardab praegune kuupäev/kellaaeg keskööst praeguse päeva.|
|+ või -|Advance määratud arv, kuupäev ja kellaaeg. Näited: nüüd + 1 tund Advance praeguse kuupäeva/kellaaja tund ahead.2013-10-01T12:00-10 päeva Advance kuupäeva väärtus 10 päeva võrra tagasi.|



Te saate kett kuupäeva/kellaaja matemaatilisi tehtemärke koos, näiteks:

```
NOW+1HOUR-10MONTHS/MINUTE
```

Järgmises tabelis on loetletud toetatud kuupäeva/kellaaja üksused.

Kuupäeva/kellaaja üksus|Kirjeldus
---|---
AASTA, AASTAT|Ümardab käesoleva aasta või Advance poolt määratud arv aastaid.
KUU, KUUD|Ümardab praegune kuu või Advance määratud kuude arvu võrra.
PÄEVA, KUUPÄEV|Ümardab tänase päeva, kuu või Advance määratud arvu päevade võrra.
TUND, TUNDI|Ümardab praeguse tund või Advance määratud arvu tunni võrra.
MINUTID MINUTIT|Ümardab praeguse minutid või Advance poolt määratud minutite arv.
TEISEKS SEKUNDIT|Ümardab arvu praeguse teise või Advance poolt määratud sekundite arv.
MILLISEKUNDILIST MILLISEKUNDIT, MILLI, MILLIS|Ümardab praeguse millisekundilist või Advance määratud arv millisekundit.


### <a name="field-facets"></a>Väli omadustega

Väli omadustega abil saate määrata otsingu tingimus kindlad väljad ja nende täpse väärtuste asemel "tasuta tekst" päringute kirjutamiseks kogu registri eri tingimustel. Oleme süntaksit mitu näited eelmistes lõikudes juba kasutanud. Siin pakume keerukamaid näited.

**Süntaks**

```
field:value
```

```
field=value
```

**Kirjeldus**

Otsib väljalt konkreetse väärtuse. Väärtus võib olla tähistab, arvu või kuupäeva/kellaaja.

Näide:

```
TimeGenerated:NOW
```

```
ObjectDisplayName:"server01.contoso.com"
```

```
SampleValue:0.3
```

**Süntaks**

*väli > väärtus*

*väli < väärtus*

*väli > = väärtus*

*väli < = väärtus*

*väli! = väärtus*

**Kirjeldus**

Pakub võrdlemine.

Näide:

```
TimeGenerated>NOW+2HOURS
```

**Süntaks**

```
field:[from..to]
```

**Kirjeldus**

Vahemiku faceting pakub.

Näide:

```
TimeGenerated:[NOW..NOW+1DAY]
```

```
SampleValue:[0..2]
```

### <a name="logical-operators"></a>Loogika tehtemärgid

Päringu keelte tugi loogika tehtemärke (*ja*, *või*ja *pole*) ja nende C-laadi aliases (*&&*, *||*, ja *!*) vastavalt. Sulgude abil saate rühmitada järgmisi tehtemärke.

Näited:

```
system OR error

```

```
Type:Alert AND NOT(Severity:1 OR ObjectId:"8066bbc0-9ec8-ca83-1edc-6f30d4779bcb8066bbc0-9ec8-ca83-1edc-6f30d4779bcb")
```

Saate argument loogika tehtemärkide ülataseme filter argumentide. Sel juhul eeldatakse tehtemärki.


Filtri avaldisel|Võrdub
---|---
süsteemi tõrge|süsteemi ja tõrge
süsteemi "Windows Server" või raskusaste: 1|süsteem ja ("Windows Server" või raskusaste: 1)

### <a name="wildcarding"></a>Wildcarding

Päringukeele toetab abil soovitud (*\*) märgi tähistada ühte või mitut märki päringu väärtust.

Näited:

 Otsi kõik arvutid "SQL" nimi, näiteks "Redmond SQL".

```
Type=Event Computer=*SQL*
```

>[AZURE.NOTE] Täna ei saa tsitaate kasutada metamärke. Sõnumi =`"*This text*"` arvestab selle (\*) kasutada mõne literaalmärgid (\*) märgi.
## <a name="commands"></a>Käsud

Päringu poolt tagastatud tulemite käske rakendada. Kasutage käsu rakendamiseks välja tulemused püstkriips (|). Mitme käsu eraldada toru märk.

>[AZURE.NOTE] Käsk nimed kirjutatud läbivate suurtähtedega või väiketähti erinevalt väljade nimed ja andmed.

### <a name="sort"></a>Sortimine

Süntaks:

    sort field1 asc|desc, field2 asc|desc, …

Kindla väljade tulemused sorditakse. Asc/laskuv eesliite pole kohustuslik. Kui need on välja jäetud, eeldatakse *asc* sortimisjärjestuse. Kui päringu kasutada käsku *sordi* konkreetselt, sortida **TimeGenerated** laskuv on vaikekäitumine, ja see alati tulemeid viimase esmalt.

### <a name="toplimit"></a>Üla-või piiravad

Süntaks:

    top number


    limit number
Piirab tulemusi ülemised N vastuse.

Näide:

    Type:Alert errors detected | top 10

Tagastab top 10 kattuvad tulemused.

### <a name="skip"></a>Jäta

Süntaks:

    skip number

Ignoreerib loetletud tulemite arv.

Näide:

    Type:Alert errors detected | top 10 | skip 200

Tagastab top 10 kattuvad tulemused alates esimesest tulemi 200.

### <a name="select"></a>Valige

Süntaks:

    select field1, field2, ...

Valige väljad tulemuste piirangud.

Näide:

    Type:Alert errors detected | select Name, Severity

Piirab tagastatud tulemite väljade *nimi* ja *tõsidust*.

### <a name="measure"></a>Mõõt

*Mõõt* käsku kasutatakse töötlemata otsingutulemite statistikafunktsioonid rakendamiseks. See on väga kasulik saada *rühma poolt* vaadete andmed üle. *Mõõt* käsu kasutamisel Log Analytics otsing kuvab tabeli liidetud tulemusi.

Süntaks:

    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]] by groupField1 [, groupField2 [, groupField3]]  [interval interval]


    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]]  interval interval



Tulemuste liitmise teel *groupField* ja liidetud mõõt väärtuste arvutamiseks *aggregatedField*abil.


|Mõõt funktsiooni nimi statistika|Kirjeldus|
|---|---|
|*aggregateFunction*|Nimi kokkuvõttefunktsioon (väiketähed). Järgmised funktsioonid on toetatud: COUNT MAX MIN summa AVG STDDEV COUNTDISTINCT PROTSENTIILI ## või PCT ## (## on mis tahes arv vahemikus 1 – 99)|
|*aggregatedField*|Väli, mis on koondatud. See väli on vabatahtlik liitväärtuse funktsiooni COUNT, kuid peab olema olemasoleva arvväärtusega välja liitmine, MAX, MIN, AVG STDDEV, või PROTSENTIILI ## või PCT ## (## on mis tahes arv vahemikus 1 – 99). Funktsiooni aggregatedField võib olla pikenda toetatud funktsioone.|
|*fieldAlias*|(Valikuline) pseudonüüm arvutatud liidetud väärtus. Kui pole määratud, siis välja nime AggregatedValue.|
|*groupField*|Tulemite hulk välja nimi on rühmitatud.|
|*Intervall*|Ajavahemik vormingus:**nnnNAME** koht: nnn on positiivne täisarv. **Nimi** on intervall nimi. Toetatud intervall nimed sisaldavad (tõstutundlik): MILLISEKUNDILIST [K] teise [K] MINUTE [K] [K] tund päeva [K] kuu [K] aasta [K]|


Suvandi intervall saab kasutada ainult kuupäeva/kellaaja väljad (nt *TimeGenerated* ja *TimeCreated*). Praegu see teenus pole rakendatakse, kuid välja kuupäev/kellaaeg, mis on taustväärtus ilma põhjustab käitustõrge. Skeemi valideerimine rakendamisel teenuse API hülgab päringut, mida kasutada ilma kuupäeva/kellaaja väljad intervall liitmiseks. Praeguse *mõõt* rakendamist toetab intervall rühmitamise mis tahes kokkuvõttefunktsiooni.

Kui alus klausel on välja jäetud, kuid intervalli on määratud (mõne teise süntaks), eeldatakse *TimeGenerated* väli on vaikimisi.

Näited:

**Näide 1**

    Type:Alert | measure count() as Count by ObjectId

*Selgitus*

Teatiste *ObjectId väärtuse* järgi rühmitab ja arvutab iga rühma teatiste arvu. Liidetud väärtus tagastatakse väljana *Count* (pseudonüüm).

**Näide 2**

    Type:Alert | measure count() interval 1HOUR

*Selgitus*

Pakuvad teatisi, 1-tunni järel *TimeGenerated* välja, ja tagastab teatiste arvu iga intervalli.

**Näide 3**

    Type:Alert | measure count() as AlertsPerHour interval 1HOUR

*Selgitus*

Sama, nagu eelmises näites, kuid liidetud pseudonüümi (*AlertsPerHour*).

**Näide 4**

    * | Mõõtke count() TimeCreated intervall 5 päeva alusel

*Selgitus*

Groups tulemused 5-päevase intervalliga *TimeCreated* välja, ja tagastab tulemite arv iga intervalli.

**Näide 5:**

    Type:Alert | measure max(Severity) by WorkflowName

*Selgitus*

Pakuvad teatisi töökoormus nime järgi, ja tagastab suurima teatiste raskusaste väärtuse iga töövoo jaoks.

**Näide 6**

    Type:Alert | measure min(Severity) by WorkflowName

*Selgitus*

Sama, nagu eelmises näites, kuid *Min* kokkuvõtliku funktsioon.

**Näide 7**

    Type:Perf | measure avg(CounterValue) by Computer

*Selgitus*

Pakuvad täiuslik arvuti ja arvutab keskmise (avg).

**Näide 8**

    Type:Perf | measure sum(CounterValue) by Computer

*Selgitus*

Sama, mis eelmises näites, kuid kasutab *funktsiooni Sum*.

**Näide 9**

    Type:Perf | measure stddev(CounterValue) by Computer

*Selgitus*

Sama, mis eelmises näites, kuid kasutab *STDDEV*.

**Näide 10**

    Type:Perf | measure percentile70(CounterValue) by Computer

*Selgitus*

Sama, mis eelmises näites, kuid kasutab *PERCENTILE70*.

**Näide 11**

    Type:Perf | measure pct70(CounterValue) by Computer

*Selgitus*

Sama, mis eelmises näites, kuid kasutab *PCT70*. Pange tähele, et *PCT ##* on ainult pseudonüümi funktsioon *PERCENTILE ##* .

**Näide 12**

    Type:Perf | measure avg(CounterValue) by Computer, CounterName

*Selgitus*

Pakuvad täiuslik esmalt arvuti ja seejärel CounterName arvutab keskmise (avg).

**Näide 13**

    Type:Alert | measure count() as Count by WorkflowName | sort Count desc | top 5

*Selgitus*

Saab ülemise viis töövoogude teatiste maksimaalne arv.

**Näide 14**

    * | Mõõt countdistinct(Computer) tüübi järgi

*Selgitus*

Loendab arvuteid iga teatamine.

**Näide 15**

    * | Mõõt countdistinct(Computer) intervall 1 tund

*Selgitus*

Loendab arvuteid aruandluse jaoks tunnis.

**Näide 16**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total” | measure avg(CounterValue) by Computer Interval 1HOUR
```

*Selgitus*

Rühma % arvuti protsessori aja ja tagastab keskmise iga 1 tund.

**Näide 17**

    Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES

*Selgitus*

Pakuvad W3CIISLog meetodiga ja tagastab suurima iga 5 minutit.

**Näide: 18**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer Interval 1HOUR
```

*Selgitus*

Rühma % protsessori aja arvuti ja tagastab miinimum, Keskmine, 75 protsentiili ja maksimum iga 1 tund.

**Näide 19**

```
Type:Perf CounterName=”% Processor Time”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer, InstanceName Interval 1HOUR
```

*Selgitus*

Rühma % protsessori aja esmalt arvuti ja seejärel eksemplari nimi ja tagastab miinimum, Keskmine, 75 protsentiili ja maksimum iga 1 tund.

**Näide 20**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | measure max(product(CounterValue,60)) as MaxDWPerMin by InstanceName Interval 1HOUR
```

*Selgitus*

Arvutab ketta maksimumarv kirjutab minutis iga kettale teie arvutis

### <a name="where"></a>Kui

Süntaks:

```
**where** AggregatedValue>20
```

Saab kasutada ainult pärast *mõõt* käsku täpsemaks funktsiooni *mõõt* koondamine on toodetud liidetud tulemite filtreerimiseks.

Näited:

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) as MAXCPU by Computer

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) by Computer | where (AggregatedValue>50 and AggregatedValue<90)

### <a name="in"></a>TOLLI

Süntaks:

```
(Outer Query) (Field to use with inner query results) IN {Inner query | measure count() by (Field to send to outer query)} (rest  of outer query)  
```

Kirjeldus: Süntaksit võimaldab teil luua liitmise ja selle koondamine sisse mõne muu välise (primaarne) otsing, mis otsib sündmused nendes väärtusega väärtuste loendi kanalile. Saate seda teha, lisades sisemine otsida looksulgudega ja söötmine tulemuste nimega võimalikud väärtused välja tehtemärk IN abil välise otsing.

Sisemise päringu näide: *arvutisse praegu puuduvad turbevärskendusi* koos koondamine järgmine päring:

```
Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer
```    

Leiab *kõik Windowsi sündmuste arvutites praegu puuduvad turbevärskendusi* päringu lõplikku sarnaneb:

```
Type=Event Computer IN {Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer}
```

### <a name="dedup"></a>Dedup

**Süntaks**

    Dedup FieldName

**Kirjeldus** Tagastab esimese dokumendi puhul iga välja antud kordumatu väärtus.

**Näide**

    Type=Event | sort TimeGenerated DESC | Dedup EventID

Eeltoodud näites tagastab ühe sündmuse (viimast, sest kasutame LASKUV TimeGenerated) EventID kohta.


### <a name="extend"></a>Laiendamine

**Kirjeldus** Võimaldab käitusaja väljade päringute loomine. Samuti saate mõõt käsk Laienda pärast kui soovite teha koondamine.

**Näide 1**

    Type=SQLAssessmentRecommendation | Extend product(RecommendationScore, RecommendationWeight) AS RecommendationWeightedScore
SQL-i Assessment soovitusi kaalutud soovitus Keskmine kuvamine

**Näide 2**

    Type=Perf CounterName="Private Bytes" | EXTEND div(CounterValue,1024) AS KBs | Select CounterValue,Computer,KBs
Väärtuse kuvamiseks KBs asemel baiti

**Näide 3**

    Type=WireData | EXTEND scale(TotalBytes,0,100) AS ScaledTotalBytes | Select ScaledTotalBytes,TotalBytes | SORT TotalBytes DESC
Skaala WireData TotalBytes väärtus nii, et kõik tulemused vahemikku 0 – 100.

**Näide 4**

```
Type=Perf CounterName="% Processor Time" | EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION
```
Kui suure täiuslik Counter väärtused, mis on väiksem kui 50% las madal ja teistele sildistamine

**Näide 5:**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | Extend product(CounterValue,60) as DWPerMin| measure max(DWPerMin) by InstanceName Interval 1HOUR
```
Arvutab ketta maksimumarv kirjutab minutis iga kettale teie arvutis

**Toetatud funktsioonid**


| Funktsioon | Kirjeldus | Süntaks: näited |
|---------|---------|---------|
| ABS | Tagastab määratud väärtuse või funktsiooni väärtuse. | `abs(x)` <br> `abs(-5)` |
| ACOS | Annab vastuseks väärtuse või funktsiooni kaar koosinus | `acos(x)` |
| ja | Tagastab väärtuse TRUE, ainult juhul, kui kõik selle operandi väärtuseks true. | `and(not(exists(popularity)),exists(price))` |
| ASIN | Tagastab väärtuse või funktsiooni kaar siinuse. | `asin(x)` |
| ATAN | Annab vastuseks väärtuse või funktsiooni kaar tangensi | `atan(x)` |
| ATAN2 |  Annab vastuseks nurga saadakse rectangular koordinaatide x, et Polaarsed koordinaadid y | `atan2(x,y)` |
| cbrt | Kuubi root |  `cbrt(x)` |
| ceil | Ümardab arvu täisarvuks | `ceil(x)`  <br> `ceil(5.6)`– Tagastab 6 |
| cos | Annab vastuseks nurga koosinuse | `cos(x)` |
| COSH | Annab vastuseks nurga hüperboolse koosinuse. | `cosh(x)` |
| def | def on lühike vaikesäte. Tagastab väärtuse "väli", või kui väli pole, tagastab määratud vaikeväärtus ja esimese väärtuse liitmine annab väärtuse, kui: `exists()==true`. | `def(rating,5)`– Def() see funktsioon tagastab reiting või kui ükski hinnang dokumendis, tagastab funktsioon 5 <br> `def(myfield, 1.0)`-võrdne`if(exists(myfield),myfield,1.0)` |
| kraadi | Teisendada radiaanid kraadideks |  `deg(x)` |
| DIV | `div(x,y)`jagab x y. | `div(1,y)` <br> `div(sum(x,100),max(y,1))` |
| dist | Tagastab kauguse vektorid, (punktides) n mitmedimensioonilised ruum vahel. Võtab power pluss kaks või enam, ValueSource eksemplarid ja arvutab kahe vektorite vahel. Iga ValueSource peab olema arv. Ei tohi olla paarisarv ValueSource eksemplarid edasi ja meetod eeldab, et esimeses pooles tähistavad esimese vektori ja teises pooles teise vektori.  | `dist(2, x, y, 0, 0)`-Arvutab selle Eukleidese kaugus between,(0,0) ja (x; y) iga dokumendi. <br> `dist(1, x, y, 0, 0)`-Arvutab Manhattan (takso) vahemaa (0,0) ja (x ja y) iga dokumendi. <br> `dist(2,,x,y,z,0,0,0)`-Eukleidese vahemaa (0,0,0) ja (x, y, z) iga dokumendi.<br>`dist(1,x,y,z,e,f,g)`-Manhattan vahelise kauguse (x, y, z) ja (e, f, g), kus iga täht on välja nimi. |
| on olemas | Annab vastuseks väärtuse TRUE, kui mõni liige välja olemas. | `exists(author)`– Väärtus on tagastab väärtuse TRUE, mis tahes dokumendi välja "autor".<br>`exists(query(price:5.00))`– Tagastab väärtuse TRUE, kui vastab "hind", "5.00". |
| exp | Annab vastuseks Astendatud Euleri arv x | `exp(x)` |
| Floor | Ümardab arvu allapoole täisarvuks | `floor(x)`  <br> `floor(5.6)`– Tagastab 5 |
| hüpo | Annab vastuseks sqrt(sum(pow(x,2),pow(y,2))) ilma vahe ületäitumise ja alatäitumise | `hypo(x,y)`  <br> ` |
| Kui | Võimaldab tingimusvormingu funktsiooni päringud. Klõpsake `if(test,value1,value2)` -testi on või viidatav väärtus või avaldis, mis tagastab loogikaväärtuse (TRUE või FALSE).  `value1`on väärtus, mis tagastatakse funktsiooni kui testi liitmine annab väärtuse TRUE. `value2`on väärtus, mis tagastatakse funktsiooni kui testi liitmine annab väärtuse FALSE. Avaldise võib olla mis tahes funktsiooni, mille väljundid kahendmuutujaga väärtused või isegi esitus arvväärtusi, kus juhul väärtuse 0 false või stringid, tõlgendatakse funktsioonid, mille puhul tühja stringi tõlgendatakse false. | `if(termfreq(cat,'electronics'),popularity,42)`– See funktsioon kontrollib iga dokumendi jaoks soovitud kuvamiseks, kui see sisaldab ka terminit "elektroonika" kass välja. Kui seda ei, siis tagastatakse väärtus populaarsus välja, muidu 42 tagastatakse väärtus. |
| lineaarne | Rakendab `m*x+c` kus m ja c on konstandid ja x on mõne suvalise funktsioon. See võrdub `sum(product(m,x),c)`, kuid veidi tõhusam, kuna seda rakendatakse ühe funktsioonina. | `linear(x,m,c) linear(x,2,4)`annab vastuseks`2*x+4` |
| LN| Annab vastuseks määratud funktsiooni loomulikus Logi |  `ln(x)` |
| log | Tagastab Logi-funktsiooni määratud alusega 10. | `log(x)   log(sum(x,100))` |
| kaardi | Kaartide kõik väärtused Sisestuskeel-funktsiooni, mis kuuluvad min ja max, kaasa arvatud x määratud sihtkohta. Argumendid min ja max peab olema konstandid. Argumendid target ja vaikimisi saab konstandid või funktsioonid. Kui x väärtus ei kuulu min ja max vahel, siis tagastatakse x-i väärtus või vaikeväärtuse tagastatakse kui 5 argumendina määratud. |  `map(x,min,max,target) map(x,0,0,1)`– Muudab mis tahes väärtusi 0 kuni 1. See võib olla kasulik vaikimisi 0 väärtusi töötlemise.<br> `map(x,min,max,target,default)    map(x,0,100,1,-1)`-Muudatused mis tahes väärtusi vahemikus 0 kuni 100-1 ja kõik muud väärtused -1.<br>  `map(x,0,100,sum(x,599),docfreq(text,solr))`– Muudab mis tahes väärtusi vahemikus 0 ja 100 x + 599 ja kõik muud väärtused sagedus Termini 'Solri' tekstiväljal. |
| Max | Tagastab suurima arvulise väärtuse mitme pesastatud funktsioonide või konstandid, mis on määratud argumentidena: `max(x,y,...)`. Funktsiooni max ka võib olla kasulik "aluskiht" teise funktsiooni või välja mõningaid määratud konstantne.  Kasutage funktsiooni `field(myfield,max)` süntaks valimise ühele mitme väärtusega väli suurima väärtuse.  | `max(myfield,myotherfield,0)` |
| min | Tagastab minimaalse arvulise väärtuse mitme pesastatud funktsioonide konstantide, mis on määratud argumentidena: `min(x,y,...)`. Funktsioon min ka võib olla kasulik edastamiseks funktsiooni abil konstandi "ülemist". Kasutage funktsiooni `field(myfield,min)` süntaks valimise ühele mitme väärtusega väli väikseima väärtuse. | `min(myfield,myotherfield,0)` |
| mod | Arvutab funktsiooni mooduli x funktsioon y. |`mod(1,x)` <br> `mod(sum(x,100), max(y,1))`   |
| MS | Tagastab erinevuse argumendid millisekundit. Kuupäevad on Unix või POSIX aja epohhi, keskööst jaanuar 1, 1970 UTC suhtes. Argumendid võivad olla indekseeritud TrieDateField või kuupäeva matemaatika konstandi kuupäeva nimi või nüüd. `ms()`on võrdne `ms(NOW)`, arv alates epohhist millisekundit. `ms(a)`annab vastuseks arvu millisekundit alates epohhi, mis tähistab argument. `ms(a,b)`annab vastuseks arvu millisekundit selle b esineb enne lisamine, mis on `a - b`.| `ms(NOW/DAY)`<br>`ms(2000-01-01T00:00:00Z)`<br>`ms(mydatefield)`<br>`ms(NOW,mydatefield)`<br>`ms(mydatefield,2000-01-01T00:00:00Z)`<br>`ms(datefield1,datefield2)` |
| pole | Funktsiooni murtud loogiliselt eitatud väärtus. | `not(exists(author))`-KEHTIB ainult siis, kui `exists(author)` on väär. |
| või | On loogiline disjunktsioon. | `or(value1,value2)`-TRUE, kui väärtus1 või väärtus2 on täidetud. |
| POW | Tõstab määratud alusega määratud astmesse. `pow(x,y)`tõstab x / y astme. |  `pow(x,y)`<br>`pow(x,log(y))`<br>`pow(x,0.5)`– Sama sqrt. |
| toote | Tagastab väärtuste või funktsioone, mis on määratud komaga eraldatud loend. `mul(...)`võib kasutada ka pseudonüümina selle funktsiooni puhul. | `product(x,y,...)`<br>`product(x,2)`<br>`product(x,y)`<br>`mul(x,y)` |
| recip | Vastastikuste funktsiooni abil `recip(x,m,a,b)` rakendamisel `a/(m*x+b)` kus m, a, b on konstandid ning x on funktsioon, mis tahes meelevaldselt complex. Kui a ja b on võrdsed ja x > = 0, see funktsioon on suurima väärtuse 1, mis langeb nagu tõus x. Suureneva väärtuse lisamine ja b koos tulemuste kõvera lamedam osa kogu funktsiooni liikumine. Neid atribuute muuta see optimaalne tihedusfunktsiooni rohkem viimatiste dokumentide suurendada, kui x on `rord(datefield)`. | `recip(myfield,m,a,b)`<br>`recip(rord(creationDate),1,1000,1000)` |
| rad | Kraadi teisendamiseks radiaanideks | `rad(x)` |
| Prindi| Ümardab arvu lähima täisarvuni. | `rint(x)`  <br> `rint(5.6)`– Tagastab 6 |
| Sin | Annab vastuseks nurga siinuse. | `sin(x)` |
| SINH | Annab vastuseks nurga hüperboolse siinuse. | `sinh(x)` |
| skaala | Funktsiooni x nii, et nad kuuluvad määratud minTarget ja maxTarget, kaasa arvatud skaalasid väärtused. Praegune rakendamise läbib kõik väärtused funktsioon saada min ja max, et see saab valida õige skaala. Praeguse rakendamist ei saa eristada, kui dokumendid on kustutatud või dokumendid, mis ei ole väärtust. Sellisel juhul kasutab 0.0 väärtused. See tähendab, et kui väärtused on tavaliselt kõik suurem kui 0,0, üks saate endiselt lõpuks 0,0 min väärtusena vastendamiseks kaudu. Sellisel juhul asjakohane `map()` funktsioon lahendusena abil saab muuta 0,0 väärtus real vahemikus, nagu järgmisel joonisel:`scale(map(x,0,0,5),1,2)` | `scale(x,minTarget,maxTarget)`<br>`scale(x,1,2)`– Nii, et kõik väärtused on vahemikus 1 – 2, kaasa arvatud skaala x väärtused. |
| sqrt | Annab vastuseks ruutjuure määratud väärtuse või funktsiooni. | `sqrt(x)`<br>`sqrt(100)`<br>`sqrt(sum(x,100))` |
| strdist | Arvutab kahe stringi vaheline kaugus. Kasutab Lucene õigekirja kontroll StringDistance liides toetab kõiki rakendusi, mis on saadaval, et paketti, ja lubab rakendustele Ühendage oma Solri 's ressursside kaudu laadimise võimalusi. strdist võtab `(string1, string2, distance measure)`. Võimalikud väärtused kauguse mõõdu on: <br>JW: Jaro-Winkler<br>Redigeeri: Levenstein või Redigeeri kauguse<br>ngram: The NGramDistance, kui määratud, saate soovi korral edasi ngram suurusega liiga. Vaikimisi on 2.<br>FQN: Täielikult kvalifitseeritud klassi nime StringDistance kasutajaliidese rakendamist. Ei – arg ehitaja peab olema.|`strdist("SOLR",id,edit)` |
| sub | Annab vastuseks x-y kaudu `sub(x,y)`. | `sub(myfield,myfield2)`<br>`sub(100,sqrt(myfield))` |
| summa | Tagastab väärtuste või funktsioone, mis on määratud komaga eraldatud loend summa. `add(...)`võib kasutada pseudonüümi selle funktsiooni puhul. | `sum(x,y,...)`<br>`sum(x,1)`<br>`sum(x,y)`<br>`sum(sqrt(x),log(y),z,0.5)`<br>`add(x,y)`|
|termfreq | Tagastab mitu korda termin, mis kuvatakse väljal dokumendi. | termfreq(Text,'memory')|
| Tan | Annab vastuseks nurga tangensi | `tan(x)` |
| tanh | Annab vastuseks nurga hüperboolse tangensi. | `tanh(x)` |



## <a name="search-field-and-facet-reference"></a>Otsingu välja ja tahukas juhend

Kui Log otsingu abil saate andmeid otsida, tulemuste kuvamine erinevate välja ja aspekte. Siiski kuvatakse teavet ei kuvata väga kirjeldav. Saate aidata teil mõista tulemused järgmine teave.



|Väli|Otsingu tüüp|Kirjeldus|
|---|---|---|
|TenantId|Kõik|Kasutatakse sektsiooni andmed|
|TimeGenerated|Kõik|Kasutatud ajaskaalat, timeselectors (väljale Otsi ja muud ekraanid) juhtida. See tähistab kui osa andmeid Genereeriti (tavaliselt agent). Aeg tähistatakse ISO-vormingus ja on alati UTC. Tüübid, mis põhinevad olemasoleva instrumentation (st sündmuste logi sisse) puhul on tavaliselt reaalajas, mis telefonilogi kirje/rida/kirje on sisse logitud mõnda muud liiki, et toodeti halduse paketid kaudu või pilves - st soovitused/teatiste/updateagent/jne, see aeg, kui see uue tükk konfiguratsiooni mingi hetktõmmise andmete kogumise või soovitus/teatise toodeti põhineb see.|
|EventID|Sündmuse|Windowsi sündmuste logi EventID|
|EventLog|Sündmuse|Kui sündmus on sisse logitud Windowsi sündmuselogi|
|EventLevelName|Sündmuse|Kriitiline / hoiatus / teabe / edu|
|EventLevel|Sündmuse|Väärtuste leidmiseks kriitilised / hoiatus / teabe / edu (kasutada EventLevelName hoopis lihtsam/loetavamaks päringute puhul)|
|SourceSystem|Kõik|Kui andmed on pärit (nii teenuse - st Toiminguhalduri OMS režiimi 'manustamine (= andmed on loodud pilveteenuses), Azure Storage (andmete vigadeta) jne|
|ObjectName|PerfHourly|Windowsi jõudlus objekti nimi|
|InstanceName|PerfHourly|Windowsi jõudlus counter eksemplari nimi|
|CounteName|PerfHourly|Windowsi jõudlus counter nimi|
|ObjectDisplayName|PerfHourly ConfigurationAlert ConfigurationObject, ConfigurationObjectProperty|Kuvada objekti suunatud Toiminguhalduri või mis avastanud töö ülevaated, või vastu teatise loodi objekti jõudluse saidikogumi reegli nimi|
|RootObjectName|PerfHourly ConfigurationAlert ConfigurationObject, ConfigurationObjectProperty|Kuvatav nimi ema ema (kahekordne majutusteenuse seosega: st SqlDatabase korraldaja korraldaja Windowsi arvuti SqlInstance) suunatud jõudluse saidikogumi reegli Toiminguhalduri või mis avastanud töö ülevaated, või vastu teatise loodi objekti objekti|
|Arvuti|Enamiku kontotüüpide|Arvuti nimi, mis kuulub andmed|
|DeviceName|ProtectionStatus|Arvuti nimi andmed kuulub (sama mis "Arvuti")|
|DetectionId|ProtectionStatus| |
|ThreatStatusRank|ProtectionStatus|Ohtu olek asukoht on arvuline esitus ohtu oleku ja sarnaselt HTTP vastuse koode, oleme jäänud lünki arvude vahemikus (mis on, miks pole ohtude on 150 mitte 100 või 0), et meil on ruumi lisada uue. Kui soovitud rollup ohtu olek ja kaitse oleku tegemiseks soovime arvuti on olnud valitud ajavahemiku jooksul kehvem riigi kuvamiseks. Arvude abil järjestada eri riikides, et saaksime vaadata kirje kõige rohkem.|
|ThreatStatus|ProtectionStatus|Kirjeldus ThreatStatus, kaartide ThreatStatusRank 1:1|
|TypeofProtection|ProtectionStatus|Ründevaratõrje toode, mis on avastatud arvuti: pole, Microsoft ründevara eemaldamise tööriista, Forefront ja jne|
|ScanDate|ProtectionStatus| |
|SourceHealthServiceId|ProtectionStatus RequiredUpdate|Selle arvuti agent siis HealthService hoolduspakett ID-d|
|HealthServiceId|Enamiku kontotüüpide|Selle arvuti agent siis HealthService hoolduspakett ID-d|
|ManagementGroupName|Enamiku kontotüüpide|Rühma nimi Toiminguhalduri manustatud agentide; muul juhul oleks tühi/tühi|
|Objektitüüp|ConfigurationObject|Tippige (nagu Toiminguhalduri halduspakett toiminguhalduri "tüüp" / klass) selle objekti avastatud Log Analytics konfiguratsiooni hindamine|
|UpdateTitle|RequiredUpdate|Nime värskenduse, mis ei leitud pole installitud|
|PublishDate|RequiredUpdate|Kui värskendus on avaldatud Microsoft update?|
|Server|RequiredUpdate|Arvuti nimi andmed kuulub (sama mis "Arvuti")|
|Toote|RequiredUpdate|Toode, mis kehtib|
|UpdateClassification|RequiredUpdate|Tüüpi update (Värskenda rollup, hoolduspakett service pack jne)|
|KBID|RequiredUpdate|KB artikkel ID, mis kirjeldatakse selle parima tava või värskendamine|
|WorkflowName|ConfigurationAlert|Reegli või kuvari, et toodeti teatise nimi|
|Raskusaste|ConfigurationAlert|Teatise tõsidust|
|Priority (prioriteet)|ConfigurationAlert|Teatise Priority (prioriteet)|
|IsMonitorAlert|ConfigurationAlert|See teatis luuakse kuvarit (true) või reegli (väär)?|
|AlertParameters|ConfigurationAlert|XML-i Logi Analytics teatise parameetrite|
|Kontekst|ConfigurationAlert|XML-i Logi Analytics teatise kontekstist|
|Töökoormus|ConfigurationAlert|Tehnoloogia või töökoormus, mis viitab teatise|
|AdvisorWorkload|Soovitused|Tehnoloogia või töökoormus, mis viitab soovitus|
|Kirjeldus|ConfigurationAlert|Teatise kirjeldus (lühikese)|
|DaysSinceLastUpdate|UpdateAgent|Mitu päeva tagasi (suhtes 'TimeGenerated' selle kirje) kas see agent installida värskendusi WSUS/Microsoft Update'i?|
|DaysSinceLastUpdateBucket|UpdateAgent|Põhineb DaysSinceLastUpdate, on kategoriseerimine aja ämbrid, kuidas kaua aega tagasi oli arvuti viimati installitud värskendusi WSUS/Microsoft Update'i|
|AutomaticUpdateEnabled|UpdateAgent|On automaatne uuendamine kontrollimine lubatud või keelatud selle agent?
|AutomaticUpdateValue|UpdateAgent|Automaatne uuendamine kontrollib Sea automaatselt alla laadida ja installida, ainult alla laadida või ainult vaadata?|
|WindowsUpdateAgentVersion|UpdateAgent|Versiooninumber Microsoft Update agent|
|WSUSServer|UpdateAgent|Millised WSUS server on see update agent suunamise?|
|OSVersion|UpdateAgent|Opsüsteemi versioon töötab see update agent|
|Nimi|Soovitused, ConfigurationObjectProperty|Nimetus soovituste või Log Analytics konfiguratsiooni hindamise atribuudi nimi|
|Väärtus|ConfigurationObjectProperty|Log Analytics konfiguratsiooni hindamise atribuudi väärtuse|
|Järgmised: KBLink|Soovitused|KB artikkel, mis kirjeldab seda head tava või Värskenda URL-i|
|RecommendationStatus|Soovitused|Soovitused on mõned tüübid, kelle kirjed 'õle", mitte ainult lisatakse need otsinguregistrisse. Olekuks kas soovitus on aktiivne/avamine või kui Log Analytics tuvastab, et see on lahendatud.|
|RenderedDescription|Sündmuse|Windowsi sündmuste sulatatud kirjeldus (korduskasutusega tekst täidetud parameetritega)|
|ParameterXml|Sündmuse|XML-i parameetriga Windowsi sündmuste (nagu näha Sündmusevaatur) jaotises "andmed"|
|EventData|Sündmuse|XML-i, kus kogu "andmed" jaotis Windowsi sündmuste (nagu näha Sündmusevaatur).|
|Allikas|Sündmuse|Sündmuste logi allikas, mis on loodud sündmuse|
|EventCategory|Sündmuse|Kategooria sündmus, otse Windowsi sündmuselogi|
|Kasutajanimi|Sündmuse|Kasutajanime ja Windowsi sündmuste (tavaliselt NT AUTHORITY\LOCALSYSTEM)|
|SampleValue|PerfHourly|Keskmise väärtuse kord tunnis liitmise jõudluse vastuolus|
|Min|PerfHourly|Jõudluse counter kord tunnis aggregate kord tunnis intervalli miinimumväärtus|
|Max|PerfHourly|Suurima väärtuse kord tunnis intervalli jõudluse counter kord tunnis aggregate|
|Percentile95|PerfHourly|Kord tunnis intervalli jõudluse counter kord tunnis kokku 95 protsentiili väärtus|
|SampleCount|PerfHourly|Mitu "töötlemata" jõudluse kontrollproove kasutati andes ühe tunni liitmine kirje|
|Ohtu|ProtectionStatus|Leitud ründevara nimi|
|StorageAccount|W3CIISLog|Azure storage konto Logi lugeda|
|AzureDeploymentID|W3CIISLog|Kuulub pilveteenuses Logi Azure juurutamise ID-d|
|Roll|W3CIISLog|Azure'i pilveteenuses Logi kuulub roll|
|RoleInstance|W3CIISLog|Azure'i rolli Logi kuuluva RoleInstance|
|sSiteName|W3CIISLog|IIS-i veebisaidi Logi kuuluva (metaandmebaasi märge); s-sitename algse Logi välja|
|sComputerName|W3CIISLog|S-arvutinimi algse Logi välja|
|sIP|W3CIISLog|Serveri IP address HTTP taotluse saadeti. Algne Logi välja s-ip|
|csMethod|W3CIISLog|HTTP-meetod (saada/postituse/jne) kasutatakse HTTP-päringu kliendi poolt. Algne log cs-meetod|
|cIP|W3CIISLog|Kliendi IP address HTTP taotlus pärineb. Algne Logi välja c-ip|
|csUserAgent|W3CIISLog|HTTP kasutajaagendi deklareeritud kliendi (brauseri või muul viisil). Cs-kasutaja agent algse Logi|
|scStatus|W3CIISLog|HTTP olekukoodi (200/403/500/jne) serveri poolt tagastatud kliendile. Algne Logi cs-olek|
|TimeTaken|W3CIISLog|Kuidas pikk (millisekundites), mille täitmiseks kulunud kutse. Timetaken algse Logi välja|
|csUriStem|W3CIISLog|Suhteline Uri (ilma hosti aadress, st "/ otsing") mis on nõutud. Cs-uristem algse Logi välja|
|csUriQuery|W3CIISLog|URI päringu. URI päringud on vajalikud ainult dünaamiline lehtede, nt ASP lehtede, nii, et sellel väljal on tavaliselt sidekriipsuga staatiline lehti.|
|Spordialade|W3CIISLog|Portide, mis saadeti HTTP-päringu (ja IIS-i kuulata, kuna see peale seda)|
|csUserName|W3CIISLog|Autenditud kasutaja nimi, kui kutse on autenditud ja mitte anonüümne|
|csVersion|W3CIISLog|HTTP-protokolli versiooni taotluse (nt "HTTP / 1.1')|
|csCookie|W3CIISLog|Küpsiste teabe|
|csReferer|W3CIISLog|Saidi kasutaja viimati külastatud. Selle saidi esitatud praeguse saidi linki.|
|csHost|W3CIISLog|Host päise (st www.mysite.com), mis on nõutav|
|scSubStatus|W3CIISLog|Tõrkekood SubStatus|
|scWin32Status|W3CIISLog|Windowsi tähis|
|csBytes|W3CIISLog|Saadetud kutse kliendilt serverisse baiti|
|scBytes|W3CIISLog|Tagastatud tagasi vastuse serverist kliendi baiti|
|ConfigChangeType|ConfigurationChange|Muuda tüüpi (WindowsServices / tarkvara / jne)|
|ChangeCategory|ConfigurationChange|Kategooria on muutunud (muudetud / lisatud / eemaldatud)|
|SoftwareType|ConfigurationChange|Tippige tarkvara (Värskenda / Application)|
|SoftwareName|ConfigurationChange|Nimi (kehtib ainult tarkvara muudatused) tarkvara|
|Publisheri|ConfigurationChange|Müüja, kellel avaldab tarkvara (kehtib ainult tarkvara muudatused)|
|SvcChangeType|ConfigurationChange|Windowsi teenuse rakendatud Muuda tüüpi (osariik / StartupType / tee / ServiceAccount) – Windows teenuse muudatused kehtivad ainult|
|SvcDisplayName|ConfigurationChange|Teenus, mis on muudetud kuvatav nimi|
|SvcName|ConfigurationChange|Mis on muudetud teenuse nimi|
|SvcState|ConfigurationChange|Uue (praeguse) teenuse olek|
|SvcPreviousState|ConfigurationChange|Eelmine teadaolevad teenust (ainult korral, kui teenuse olek muutunud)|
|SvcStartupType|ConfigurationChange|Teenuse Käivitustüüp|
|SvcPreviousStartupType|ConfigurationChange|Eelmise teenuse Käivitustüüp (ainult korral, kui teenuse Käivitustüüp muudetud)|
|SvcAccount|ConfigurationChange|Teenusekonto|
|SvcPreviousAccount|ConfigurationChange|Eelmise teenusekonto (ainult korral, kui teenusekonto muudetud)|
|SvcPath|ConfigurationChange|Teenuse Windows teed|
|SvcPreviousPath|ConfigurationChange|Eelmise tee (ainult korral, kui see on muudetud) teenuse Windows täitmisfail|
|SvcDescription|ConfigurationChange|Teenuse kirjeldus|
|Eelmise|ConfigurationChange|Eelmise oleku tarkvara (installitud / pole installitud / eelmine versioon)|
|Praeguse|ConfigurationChange|Vastavalt selle tarkvara (installitud / pole installitud / praegune versioon)|

## <a name="next-steps"></a>Järgmised sammud
Lisateabe saamiseks Logi otsingud:

- Saate tutvuda [log otsingud](log-analytics-log-searches.md) üksikasjaliku teabe kogutud lahendusi.
- [Kohandatud väljad Log Analytics](log-analytics-custom-fields.md) abil saate laiendada log otsinguid.
