<properties
    pageTitle="Azure'i otsingu lucene päringu näited | Microsoft Azure'i otsing"
    description="Lucene päringusüntaksit udune otsing, lähedus otsing, Termini suurendada, Lihtavaldise otsingu ja metamärkide otsing."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="Lucene query analyzer syntax"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="liamca"
/>

# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Lucene päringu süntaksinäiteid Azure otsingus päringute koostamise

Kui päringute loomine Azure otsingu jaoks, saate kasutada vaikimisi [lihtsa päringusüntaksit](https://msdn.microsoft.com/library/azure/dn798920.aspx) või alternatiivne [Lucene päringu parseri Azure'i otsing](https://msdn.microsoft.com/library/azure/mt589323.aspx). Lucene päringu parseri toetab keerukamaid päringu importida, nt välja ulatusega päringud, udune otsing, lähedus otsing, Termini suurendada ja reqular avaldis otsing.

Selles artiklis saate samm Lucene päringusüntaksit ja tulemuste kõrvuti kuvamine näited kaudu. Näited vastuolus eellaaditud otsinguregistri [JSFiddle](https://jsfiddle.net/), testimise skripti ja HTML-i redaktorit online koodi sisse. 

Paremklõpsake päringu näide URL-ide avamine JSFiddle eraldi brauseriaknas.

> [AZURE.NOTE] Järgmised näited kasutada otsinguregistri, mis koosneb töö saadaval põhjal esitatud [Linn New York OpenData](https://nycopendata.socrata.com/) algatus Andmekomplekt. Neid andmeid ei saa pidada praeguse või lõpule viidud. Liivakasti teenus, mida Microsoft on register. Teil pole vaja Azure tellimuse- või Azure otsingu proovige need päringud.

## <a name="viewing-the-examples-in-this-article"></a>Selle artikli näited vaatamine

Saate määrata kõik selle artikli näited Lucene päringu parseri parameetri**queryType** otsingu kaudu. Lucene päringu parseri oma kood kasutamisel kuvatakse **queryType** iga soovi korral saate määrata.  Lubatud väärtused sisaldavad **lihtsa**|**täielik**, **lihtne** vaike- ja **täielik** Lucene päringu parseri jaoks. Üksikasjad päringu parameetrite määramise kohta leiate [Otsingu dokumendid (Azure'i otsingu teenuse REST API)](https://msdn.microsoft.com/library/azure/dn798927.aspx) .

**Näide 1** – järgmine päring koodilõigu uue brauseris lehe, mis laadib JSFiddle ja käivitab päringu avamiseks paremklõpsake:
- [& queryType = täielikku & Otsi = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

See päring tagastab dokumendid (Liivakasti teenus on laaditud) registrist tööde haldamine

Klõpsake uues brauseriaknas kuvatakse JavaScripti lähte-kui ka HTML-i väljundi kõrvuti. Skripti viited päring, mis on esitatud näide URL-ide selle artikli. Näiteks **näide 1**, aluseks oleva päringu on järgmine:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

Teate päringu kasutab on eelkonfigureeritud Azure'i nimetatakse nycjobs. Parameetri **searchFields** piirab otsingu lihtsalt business väljale pealkiri. **QueryType** on seatud **täielik**, mis juhendab Azure'i otsingu selle päringu Lucene päringu parseri kasutamine.

### <a name="fielded-query-operation"></a>Pidevalt päringu toiming

Saate muuta selle artikli näited, määrates **fieldname:searchterm** ehituse määratleda pidevalt päringu toiming, kus väli on üksiku sõna ja otsingusõna on ka üksiku sõna või fraasi, võite koos toetatud brauserid. Järgmised näited.

- business_title:(senior NOT junior)
- olek: ("New York" ja "Uus Jersey")

Kindlasti mitme stringide jutumärkidesse panna, kui soovite nii stringide hinnatav ühtse tervikuna, nagu käesoleval juhul, kui otsite kaks erinevat linnade väli asukoht. Tagama tehtemärk on suurtähestusega nagu näha ei ole ja AND.

Määratud **fieldname:searchterm** väli peab olema otsitavate välja. Üksikasjad välja määratluste registri atribuudid kasutamise kohta leiate [Create Index (Azure'i otsingu teenuse REST API)](https://msdn.microsoft.com/library/azure/dn798941.aspx) .

## <a name="fuzzy-search"></a>Udune otsing

Udune otsing leiab vasteid, et on sarnane ehituse. Kohta [Lucene dokumentatsiooni](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)kohta, põhinevad udune otsingud [Damerau-Levenshtein kaugus](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

Kasutage udune otsingu tilde "~" sümbol valikuline parameeter, väärtus vahemikus 0 kuni 2, mis täpsustab Redigeeri kauguse üksiku sõna lõpus. Näiteks "sinine ~" või "sinine ~ 1" return sinine, blues ja liimi.

**Näide 2** --paremklõpsake järgmine päring koodilõigu seda proovida. Selle päringu otsib business pealkirjad koos Termini kõrgemate neile, kuid mitte junior:

- [& queryType = täielikku & Otsi = business_title:senior ei ole junior](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="proximity-search"></a>Lähedus otsing

Lähedus otsinguid kasutatakse terminite otsimine, mis on üksteise lähedal dokumendi. Lisa tilde "~" fraasi lõpus sümbol, millele järgneb lähedus äärist loodud sõnade arvu. Näiteks "teha airport" ~ 5 otsib terminite teha ja airport 5 tähtedevahelist üksteisest dokumendi.

**Näide 3** – paremklõpsake järgmine päring koodilõigu. Selle päringu otsib töökohtade Termini sidusettevõtte (kui see on valesti kirjutatud):

- [& queryType = täielikku & Otsi = business_title:asosiate ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

**Näide 4** --paremklõpsake päringu. Otsida töö terminiga "vanemanalüütik", kui see on eraldatud rohkem kui ühe sõna võrra.

- [& queryType = täielikku & Otsi = business_title: "vanemanalüütik" ~ 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Näide 5** – proovima uuesti eemaldamine sõnade vahel Termini "vanemanalüütik".

- [& queryType = täielikku & Otsi = business_title: "vanemanalüütik" ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting"></a>Periood suurendada

Termini suurendamine viitab järjestamine dokumendi, mis on suurem kui see sisaldab võimendatud termin, dokumendid, mis sisaldavad terminit suhtes. See erineb hinded profiilid, et hinded profiilid suurendada teatud väljad, mitte teatud tingimustel. Järgmine näide aitab illustreerida erinevused.

Kaaluge hinded profiili, mis suurendab vastab teatud väljal, nt **Žanr** musicstoreindex näites. Termini suurendamine saab täiendavaks suurendamiseks teatud otsingu tingimused, mis on suurem kui teised. Näiteks "rock ^ 2 elektroonilise" tõstab **Žanr** välja, mis on suurem kui muude otsitavate väljade registri otsinguterminid sisaldavaid dokumente. Lisaks kuvatakse dokumendid, mis sisaldavad otsingusõna "rock" kõrgem "e" tulemusena Termini boost väärtus (2) otsingusõna kohal.

Termini suurendamiseks, kasutage nooltega, "^", sümbol Otsing sõna lõpus teguriga boost (arv). Suurem boost tegur, rohkem asjakohaseid saab võrreldes teiste otsinguterminid termin. Vaikimisi on boost tegur 1. Kuigi boost tegur peab olema positiivne, võib olla väiksem kui 1 (nt 0,2).

**Näide 6** – paremklõpsake päringu. Otsige töö terminiga "arvuti analüütik", kus näeme nii sõna arvuti ja analüütik tulemusi pole veel analüütik töö on ülaosas tulemused.

- [& queryType = täielikku & Otsi = business_title:computer analüütik](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Näide 7** --proovida uuesti, aja suurendamine tulemiks Termini arvuti Termini analüütik üle, kui mõlemad sõnad pole olemas.

- [& queryType = täielikku & Otsi = business_title:computer ^ 2 analüütik](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression"></a>Lihtavaldise

Lihtavaldise otsingu vaste vahel kaldkriipsu "/", kui dokumenteeritud [regulaaravaldis ainekursuse](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html)sisu.

**Näide 8** --paremklõpsake päringu. Otsige töö kas kõrgemate või Standard termin.

- `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

Selles näites URL-i ei renderdata õigesti lehel. Lahendusena, kopeerige alltoodud URL ja kleepige see brauseris URL-i aadress:    `http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`


## <a name="wildcard-search"></a>Metamärkide otsing

Saate ära süntaks kordseni (\*) või ühe (?) märgi metamärkide otsingud. Pange tähele, et Lucene päringu parseri toetab need sümbolid ja ühe Termini pole fraasi.

**Näide 9** --paremklõpsake päringu. Otsige tööd, mis sisaldavad eesliite prog, mis sisaldab äri pealkirjad programmeerimisega ja see programmeerija.

- [& queryType = täielik & $select = business_title & search = business_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:prog*)

Te ei saa kasutada mõnda * või? Kui esimene märk otsingu sümbol.


## <a name="next-steps"></a>Järgmised sammud

Proovige oma kood, mis määrab Lucene päringu parseri. Järgmised lingid selgitatakse, kuidas häälestada otsingupäringuid .NET-ja REST API-ga. Linkide kasutage vaikimisi lihtsa süntaks nii, et on vaja teada käesolevast artiklist **queryType**määramiseks.

- [Päringu Azure'i otsinguregistri SDK .net-i abil](search-query-dotnet.md)
- [Päringu Azure'i otsinguregistri REST API abil](search-query-rest-api.md)


 