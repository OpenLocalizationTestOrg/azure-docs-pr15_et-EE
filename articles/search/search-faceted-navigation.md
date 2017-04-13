<properties 
    pageTitle="Kuidas rakendada lihvitud navigeerimise Azure'i otsing | Microsoft Azure'i | Otsingu navigeerimine kategooriad" 
    description="Rakenduste integreerimine Azure otsing pilve majutatud otsinguteenuse Microsoft Azure lihvitud navigeerimise lisada." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

#<a name="how-to-implement-faceted-navigation-in-azure-search"></a>Kuidas rakendada lihvitud navigeerimise Azure'i otsing

Lihvitud navigeerimine on filtreerimise süsteem, mis pakub enda juhitud süvitsiminek navigeerimise, otsingu rakendustes. Termini "lihvitud navigeerimine" võib olla võõras, see on peaaegu on arvestades, et olete kasutanud mõnda enne. Nagu järgmises näites on kujutatud, lihvitud navigeerimine on midagi muud kui kasutada tulemite filtreerimiseks kategooriad.

## <a name="what-it-looks-like"></a>Kuidas see välja näeb

 ![][1]
  
Omadustega aitab leida, mida otsite, tagades, et te ei saa null tulemusi. Arendaja, omadustega abil saate seada oma otsingu corpus navigeerimiseks kõige enam kasu otsingukriteeriumid. Veebikaubandus rakendustes, lihvitud navigeerimise sageli ehitatud kaubamärkide, osakondade (laste kingi), suurus, hind, populaarsus ja hinnangute üle. 

Lihvitud navigeerimine erineb üle otsingu tehnoloogiad ja võib olla väga keeruline. Azure'i otsinguväljale on lihvitud navigeerimise päringu ajal ehitatud, kasutades oma skeemi varem omistatud väljadel. Päringud, mis rakenduse koostab, peate päringu saada *tahukas päringu parameetrite* saadaval tahukas filtri väärtused selle dokumendikomplekti tulemi saamiseks. Tegelikult trimmimise dokumendi tulemi määramine, rakendus peab rakendada mõne `$filter` avaldis.

Rakenduste arendamise, osas sisuosast päringute koodi kirjutamist oluline osa tööd. Mitme rakenduse käitumist, mida soovite lihvitud navigeerimisest on esitatud teenuse, sh vahemike häälestamise ja loendab tahukas tulemuste saamine tugi. Teenus sisaldab mõistlik vaikesätted, mis aitavad vältida kohmakas navigeerimise struktuuri. 

Käesolevast artiklist leiate järgmistest jaotistest:

- [Kuidas luua selle](#howtobuildit)
- [Esitluse kiht koostamine](#presentationlayer)
- [Koostada register](#buildindex)
- [Andmete kvaliteedi kontrollimine](#checkdata)
- [Päringu koostamine](#buildquery)
- [Kuidas määrata lihvitud navigeerimine](#tips)
- [Vahemiku väärtuste põhjal lihvitud navigeerimine](#rangefacets)
- [Lihvitud navigeerimine põhineb GeoPoints](#geofacets)
- [Proovige järele!](#tryitout)

##<a name="why-use-it"></a>Miks seda kasutada.
Kõige tõhusamaid otsing rakendusi on mitu suhtluse mudelid peale otsinguvälja. Lihvitud navigeerimine on alternatiivne sisendpunkti soovite otsida, on mugav alternatiivsete tippides keeruka otsingu avaldised käsitsi.

##<a name="know-the-fundamentals"></a>Põhitõdesid teate

Kui olete uus otsimiseks arengu, on parim viis mõelda lihvitud navigeerimine, et see näitab enda juhitud otsingu võimalusi. See on teatud tüüpi süvitsimineku otsingut põhjal eelmääratletud filtrid, kasutatakse kiiresti täpsustamine otsingutulemite punkt-ja valige toimingud kaudu. 

**Suhtluse mudel**

Vaatame Alustuseks mõista, päringud, mis vastuseks kasutajatoimingute paljastama järjestus on iteratiivne, otsingut lihvitud navigeerimiseks.

Alguspunkt on rakenduse leht, mis pakub lihvitud navigeerimine, kusjuures tavaliselt paigutatud. Sageli on lihvitud liikumine puustruktuuris, iga väärtuse või klõpsatava teksti märkeruudud. 

1.  Azure'i otsing saadetud päringu määrab lihvitud navigeerimise struktuuri kaudu ühe või mitme tahukas päringu parameetrid. Näiteks võivad päringu `facet=Rating`, võib-olla koos mõne `:values` või `:sort` suvand täpsemaks piiritlemiseks esitlus.
2.  Esitluse kiht renderdab otsingu lehele, mis pakub lihvitud navigeerimine, kasutades aspekte, mis on määratud taotluse.
3.  Antud lihvitud navigeerimise struktuuri, mis sisaldab reiting, näitamaks, et ainult toodete Reiting 4 või uuem versioon peaks kuvatama kasutaja klõpsuga "4". 
4.  Vastuseks taotluse saadab päring, mis sisaldab`$filter=Rating ge 4` 
5.  Esitluse layer värskendab lehel, kus vähendatud tulemihulga, mis sisaldab ainult need üksused, mis vastavad uue (sel juhul toodete hinnatud 4 ja uuemad).

Mõne tahukas on Päringuparameetri, kuid ärge ajage selle päringu sisendi. Seda kasutatakse kunagi valiku kriteeriumidena päringutes. Selle asemel mõelda tahukas päringu parameetrite sisendina navigeerimisstruktuuri, mis on saadaval tagasi vastus. Päringu parameetrite tahukas esitate, Azure'i otsingu hindab osaline tulemuste iga väärtuse tahukas on mitu dokumendid.

Teade kuvatakse `$filter` sammus 4. See on oluline osa lihvitud navigeerimine. Kuigi omadustega ja filtrid on sõltumatu API, peate nii kavatsete võimalusi pakkuda. 

**Mustrite kujundus**

Rakenduse koodi, muster on tagasi lihvitud navigeerimise struktuuri koos tahukas tulemused pluss $filter avaldis, mis tegeleb klõpsake sündmuse tahukas päringu parameetrite abil. Mõelda on `$filter` väljendamine tegelik kärpimise koodide otsingutulemite tagastatud esitlus kihti. Antud värvid tahukas, klõpsates punane värv rakendatakse kaudu on `$filter` avaldis, mis valib ainult need üksused, millel on punane värv. 

**Azure'i otsingus päringu põhialused**

Azure'i otsinguväljale taotluse on määratud ühe või mitme päringu parameetrite (vt [Dokumentide](http://msdn.microsoft.com/library/azure/dn798927.aspx) kirjeldust iga) kaudu. Päringu parameetrite ükski on nõutavad, kuid peab teil olema vähemalt üks selleks, et kehtiks päringu.

Üks või mõlemad järgmised avaldised on saavutamiseks täpsusega, üldiselt mõista võimalus filtreerida oluline tabamust, kui:

- **Otsingu =**<br/>
See parameeter on otsinguavaldise. See võib olla üks tükk teksti või keeruka otsinguavaldise, mis sisaldab mitut termineid ja tehtemärke. Server, kasutatakse otsingu avaldis otsingu, päringute otsitavate väljade registri sobitamine tingimustel, tulemeid tagastama rank järjestuses. Kui seate `search` null, päringu täitmise on üle kogu registri (ehk teisisõnu öeldes `search=*`). Käesolevas näites muude elementide päringu, nagu on `$filter` või hinded profiili, on see peamine mõjutavaid mis dokumente ei tagastata `($filter`) ja millises järjekorras (`scoringProfile` või `$orderb`y).

- **$filter =**<br/>
Filter on võimas süsteem suurus otsingutulemusi kindla dokumendi atribuutide väärtused. A `$filter` on hinnatud esmalt, faceting loogika, mis loob saadaval väärtused ja vastavate loendab iga väärtuse järgnes

Keeruka otsingu avaldiste väheneb päringu täitmise. Võimaluse korral kasutada varasemate filtriavaldiste täpsuse suurendamiseks ja päringu jõudluse parandamiseks.

Paremini mõista, kuidas lisab filtri täpsemaks, võrrelda keeruka otsinguavaldise ühele, mis sisaldab filtriavaldise:

- `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`

- `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Kuigi mõlemad päringud ei sobi, teine on parem, kui otsite parkimine Seattle motellid. Esimese päringu tugineb nende kindlaid sõnu, mis on nimetatud või ei mainitud stringi väljad, näiteks nimi, kirjeldus ja mis tahes muu otsitavate andmeid sisaldav väli. Teise päringu otsitakse täpsed vasted Liigendatud andmete ja tõenäoliselt palju Täpsem.

Rakendustes, mis sisaldavad lihvitud navigeerimine, te soovite olla kindel, et iga kasutaja toimingu lihvitud navigeerimise struktuuri on kaasas otsingutulemuste kaudu filtriavaldise lisamine väheneb.

<a name="howtobuildit"></a>
##<a name="how-to-build-it"></a>Kuidas luua selle

Azure'i otsingus lihvitud navigeerimise rakendatakse oma rakenduse koodi, mis koostab kutse, kuid tugineb eelmääratletud elementide oma skeemi.

Eelmääratletud otsingu indeks on selle `Facetable [true|false]` seatud valitud väljade lubada või keelata nende kasutamist lihvitud navigeerimisstruktuuri index atribuut. Ilma `"Facetable" = true`, välja ei saa kasutada tahukas navigeerimine.

Päringu ajal rakenduse koodis loob taotlus, mis sisaldab `facet=[string]`, taotluse parameeter, mis pakub tahukas alusel välja. Päring võib olla mitu aspekte, `&facet=color&facet=category&facet=rating`, igaüht eraldatud ampersand (&) märgi.

Rakenduse koodi ka ehitada on `$filter` lihvitud navigeerimisribal nuppu sündmuste käsitlema avaldis. A `$filter` vähendab kasutamine tahukas väärtus filtrikriteeriumi otsingutulemites.

Azure'i otsing tagastab otsingutulemite kohta otsingusõnad, kasutaja, värskenduste lihvitud navigeerimisstruktuuri koos sisestatud. Azure'i otsinguväljale lihvitud navigeerimine on ühetasemeline ehituse, fassettidega väärtustega, ja loendab, mitu tulemuste leitud igaühe puhul.

Esitluse kiht koodi pakub kasutusvõimalused. See peaks nimekiri lihvitud navigeerimine, nt sildi, väärtused, märkeruute ja arvu osadeks. Azure'i Search REST API on platvorm agnostik, nii et kasutada mis tahes keele- ja soovite platvormi. Oluline on kaasata Kasutajaliidese elemente, mis toetavad suureneva värskendamise, värskendatud Kasutajaliidese olek, kui iga täiendava tahukas on valitud. 

Järgmistes jaotistes võtame lähemalt uurida, kuidas luua iga osa, alustades esitluse kiht.

<a name="presentationlayer"></a>
##<a name="build-the-presentation-layer"></a>Esitluse kiht koostamine

Töötamine tagasi esitluse kiht aitab teil avastada nõuetele, mis võib teisiti vastamata ja mõista, millised võimalused on olulised otsingut.

Osas lihvitud navigeerimine, kuvab lihvitud navigeerimise struktuuri oma web või rakenduse leht, tuvastab kasutaja sisendist lehel ja lisab muudetavad elemendid. 

Veebirakenduste, AJAXI on levinud esitluse kiht, kuna see võimaldab teil värskendada suureneva muudatused. Võite ka ASP.net-i MVC või muude visualiseerimise platvorm, mida saate luua ühenduse mõne Azure'i otsinguteenuse http kaudu. Kogu artiklis-- **AdventureWorks kataloogi** – valimi rakenduse juhtub olema rakenduse ASP.net-i MVC.

Järgmises näites tehtud valimi taotluse failist **index.cshtml** koostab dünaamiline HTML-i struktuur lihvitud navigeerimise Otsingutulemite lehe kuvamiseks. Valimis, lihvitud navigeerimine sisseehitatud otsingutulemite lehele ja kuvatakse pärast seda, kui kasutaja on esitanud otsingutermin.

Teate, et iga tahukas on silt (värvid kategooriad hinnad), on sidumine lihvitud väljale (värvi, KatregooriaID listPrice), ja `.count` leitud tulemi tahukas üksuste arvu tagastamiseks kasutatud parameetri.

  ![][2]
 

> [AZURE.TIP] Otsingutulemite lehe kujundamisel Ärge unustage lisada süsteemi tühjendada aspekte. Kui kasutate ruudud, saavad kasutajad hõlpsalt intuit kuidas eemaldada filtrid. Muude paigutuste jaoks peate lingirea mustri või mõne muu loominguline lähenemine. Näiteks AdventureWorks kataloogi valimi rakenduse, võite klõpsata AdventureWorks kataloogi, otsingu lehe pealkirja.

<a name="buildindex"></a>
##<a name="build-the-index"></a>Koostada register

Registris kaudu index atribuudi välja-välja alusel lubatakse Faceting: `"Facetable": true`.  
Kõik Väljatüübid, mis võiks kasutada lihvitud navigeerimisribal on `Facetable` vaikimisi. Sellise välja tüübid on `Edm.String`, `Edm.DateTimeOffset`, ja kõigi arvväärtusega väli (sisuliselt kõik Väljatüübid on facetable peale `Edm.GeographyPoint`, mida ei saa kasutada lihvitud navigeerimine). 

Kui registri loomise parimate tavade lihvitud navigeerimiseks on selgesõnaliselt välja lülitada faceting väljad, mida tuleks kasutada kunagi on tahukatega.  Eelkõige stringi väljad ID ja toote nimi, nt singleton väärtuste jaoks seadma `"Facetable": false` lihvitud navigeerimisribal oma juhusliku (ja ebaefektiivne) kasutamist.

Järgmine on skeemi AdventureWorks kataloogi valimi rakenduse (lõigatud osa atribuute üldmahu vähendamiseks).

 ![][3]
 
Pange tähele, et `Facetable` stringi väljad, mida ei tohiks kasutada aspekte, nt ID või nimi on välja lülitatud. Lülitada välja, kui te ei pea faceting aitab hoida registri suurust väike ja üldiselt parandab jõudlust.

> [AZURE.TIP] Hea tava, sisaldavad täiskomplekti registri atribuudid iga välja jaoks. Kuigi `Facetable` on vaikimisi peaaegu kõigi väljade tahtlikult säte iga atribuut, mis aitavad teil iga skeemi otsust mõju läbi mõtlema. 

<a name="checkdata"></a>
##<a name="check-for-data-quality"></a>Andmete kvaliteedi kontrollimine 

Mis tahes andmekesksed rakenduses väljatöötamisel andmete ettevalmistamine on sageli üks suurem osade töö. Otsing rakendusi ei ole erand. Andmete kvaliteedi on otseselt seotud kas lihvitud navigeerimisstruktuuri realiseerumisel seda, et määrata ka selle tõhusust, mis aitab teil koostada filtreid, et vähendada tulemi ootuspäraselt.

Azure'i otsinguväljale Otsi corpus moodustavad dokumentidest, et asustada registri. Dokumentide Sisestage väärtused, mida kasutatakse arvutada aspekte. Kui soovite tahukas kaubamärgi või hind, iga dokumendi peaks sisaldama väärtused *BrandName* ja *ProductPrice* , mis on lubatud, järjepidevaid ja produktiivne kui sobiv filter.

Mõned meeldetuletused, mida nühkima jaoks on järgmised:

- Iga välja, mida soovite tahukas, küsige endalt, kas see sisaldab väärtusi, mis sobivad filtritena enda juhitud otsing. Väärtused peaks olema lühike, kirjeldav ja piisavalt eristatav pakkuda selge konkureerivate suvandite vahel.
- Õigekirjavead või peaaegu vastavaid väärtusi. Kui tahukas värvi ja väljaväärtuste oranž Ornage (õigekirjaviga), ja nii pick on tahukas põhjal välja värv.
- Kombineeritud juhul teksti saate ka hukatuslikuks lihvitud navigeerimine, oranž ja oranž esineb kaks erinevat väärtust. 
- Ühe ja mitmuse versiooni sama väärtuse võib põhjustada eraldi tahukas iga.

Kui saate kujutada, hoolikusega andmete ettevalmistamine on tähtis efektiivse lihvitud navigeerimine.

<a name="buildquery"></a>
##<a name="build-the-query"></a>Päringu koostamine

Kirjutate jaoks päringute koostamise kood tuleks Määrake sobiv päring, sh otsingu avaldised, omadustega, filtrid, hinded profiilid – midagi vormistada taotluse kasutada kõikjal. Selles jaotises uurime, kus omadustega sobivad päringu ning kuidas filtreid saab kasutada koos omadustega esitamisel vähendatud tulemite hulk.

Näide on sageli hea alustada. Järgmises näites tehtud failist **CatalogSearch.cs** koostab kutse, mis loob tahukas navigeerimine põhineb värvi, kategooria ja hinna. 

Pange tähele, et omadustega on andmeanalüüsi oluline selles valimi rakenduses. Otsingut AdventureWorks kataloogis on mõeldud umbes lihvitud navigeerimis- ja filtrid. See on selge märgatavaks lihvitud navigeerimise lehe paigutuse. Valimi taotlus sisaldab URI parameetreid omadustega (värvi, kategooria, hinnad) atribuutidena otsingu meetodi (nagu ehitatud valimi rakenduse).

  ![][4]
 
Tahukatega Päringuparameetri on määratud välja ja sõltuvalt andmetüübi, saate olla täpsemaks parameetriga, komaga eraldatud loend, mis sisaldab `count:<integer>`, `sort:<>`, `intervals:<integer>`, ja `values:<list>`. Väärtuste loend on toetatud arvandmeid vahemike häälestamisel. Kasutuse üksikasjad leiate [Otsingu dokumendid (Azure'i otsingu API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) .

Koos aspekte, tuleks koostatud rakenduse taotluse koostada ka filtrite piiritlemiseks testversiooni dokumentidest tahukas väärtuse valiku alusel. Jaoks bike poe lihvitud navigatsiooni vihjeid küsimustele, näiteks "mis värvid, tootjad ja jalgrattad tüüpi on saadaval", samal ajal filtreerimine vastused küsimustele, näiteks "millist täpse jalgrattad on punane, mountain jalgrattad, see hinna vahemik".

Kui kasutaja klõpsab "Punane", mis näitab, et ainult punane toodete peaks kuvatama, taotluse saadab järgmise päringu kaasaks `$filter=Color eq ‘Red’`.

## <a name="best-practices-for-faceted-navigation"></a>Head tavad lihvitud navigeerimiseks

Järgmises loendis on toodud mõned soovitused.

- **Täpsus**<br/>
Filtrite kasutamine. Kui peate kasutama ainult otsingu avaldiste eraldi, mis võivad põhjustada dokumendi tagastatava, mis ei sisalda tahukas täpne väärtus selle välju. 

- **Sihtrakenduse väljad**<br/>
Lihvitud süvitsiminek, soovite tavaliselt lisada ainult dokumendid, mis on tahukas väärtus (mitmetahuline) konkreetsel väljal, mitte suvalist üle kõik otsitavad väljad. Filtri lisamine suurendab sihtvälja suunake teenuse otsimiseks ainult lihvitud väljale vastav väärtus.

- **Registri tõhusust**<br/>
Kui teie rakendus kasutab lihvitud navigeerimise üksnes (ehk teisisõnu öeldes pole otsinguvälja), saate märkida välja `searchable=false`, `facetable=true` kompaktsema indeksi. Lisaks indekseerimine ilmneb ainult kogu tahukas väärtustest, kus pole word-paus või indekseerimine komponent osade mitmesõnalised väärtus.

- **Jõudluse**<br/>
Filtrite testversiooni dokumentidest Otsingu piiritlemiseks ja välistamine järjestuse. Kui teil on suur hulk dokumente, kasutades väga valikulise tahukas drill alla sageli teile märkimisväärselt parema jõudluse.


<a name="tips"></a> 
##<a name="tips-on-how-to-control-faceted-navigation"></a>Kuidas määrata lihvitud navigeerimine

Allpool on tip lehel juhised konkreetsete teemade kohta.

**Tahukatega navigeerimine iga välja jaoks siltide lisamine**

Sildid on tavaliselt määratletud HTML- või vormiteegi (**index.cshtml** valimi rakenduse). On Azure otsida tahukas navigeerimise sildid pole API või muud tüüpi metaandmete.

**Määratlemine, milliseid välju saab kasutada tahukas**

Tagasivõtmise, mis määrab registri skeemiga, millised väljad on saadaval kasutamiseks on tahukatega. Eeldades, et väli on facetable, määrab päringu kaasatavate väljade tahukas järgi. Väli, millega olete faceting pakub väärtused, mis kuvatakse sildi all. 

Väärtused, mis kuvatakse jaotises igale sildile tuuakse register. Näiteks kui väli tahukas on *värv*, väärtused, mis on saadaval täiendav filtreerimine on väärtused välja (punane, must ja jne).

Ainult arvandmete ning kuupäeva ja kellaaja väärtuste, saate otseselt määrata väärtused välja tahukas (nt `facet=Rating,values:1|2|3|4|5`). Väärtuste loend on lubatud järgmiste väljatüüpide lihtsustamiseks tahukas tulemuste eraldamine sidusaid vahemikeks (kas vahemike arvväärtusi või ajavahemikele põhjal). 

**Tahukatega tulemuste trimmimine**

Tahukatega tulemused otsingutulemites, mis vastavad tahukas Termini dokumendid. Järgmises näites otsingutulemites *cloud arvutuste*, 254 üksused on ka *sisemise määratlus* sisutüübina. Üksuste teineteist ei pruugi olla. Kui üksuse kriteeriumidele nii filtrid, arvestatakse iga. See on võimalik, millal faceting kohta `Collection(Edm.String)` väljad, mis on sageli kasutatud rakendada dokumendi sildistamine.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

Üldiselt, kui leiate, et tahukas tulemused püsivalt on liiga suur, soovitame teil lisada rohkem filtreid, nagu on selgitatud varasema lõigud, anda oma rakenduse kasutajatele Otsingu kitsendamiseks rohkem suvandeid.

**Üksuste limiit tahukas navigeerimine**

Iga lihvitud välja navigeerimise puu on vaikimisi piiri 10 väärtused. See vaikimisi navigeerimise struktuuri mõttekas, sest see hoiab hallatavaks väärtuste loend. Vaikimisi saate alistada, andes loendada väärtuse.

- `&facet=city,count:5`Saate määrata, et tagastatakse ainult esimese 5 linnade järjestatud tulemid üles leida tahukas tulemusena. Antud otsingusõna "airport" ja 32 vastet, kui päring, mis määrab `&facet=city,count:5`, ainult esimesel viiel kordumatu linnade kõige dokumentidega otsingutulemite kaasatud tahukas tulemused.

Teatis vahet tahukas ja Otsingu tulemused. Otsingutulemite on kõik dokumendid, mis vastavad päring. Tahukatega tulemused iga väärtuse tahukas vasteid. Näites sisaldab otsingutulemite linna nimed, mis pole loendis tahukas liigitamine (siinses näites 5). Tulemused, mis on välja filtreeritud lihvitud navigeerimine, muutuvad nähtavaks kasutajale, kui ta kustutab omadustega või muude omadustega peale linn valib kaudu. 

> [AZURE.NOTE] Koosolekul `count` , kui seal on rohkem kui üks tüüp võib olla selgem. Järgmine tabel pakub lühikokkuvõtet Termini kasutamise Azure'i otsingu API, proovi kood ja dokumentatsiooni. 

- `@colorFacet.count`<br/>
Esitluse koodi, peaksite count parameetri tahukas, kuvatakse tahukas tulemite arv. Tahukatega tulemuste arv näitab dokumente, mis vastavad tahukas või vahemiku arvu.

- `&facet=City,count:12`<br/>
Tahukatega päringu, saate seada väärtuse loendamine.  Vaikimisi on 10, kuid saate seda suurem või väiksem. Säte `count:12` saab ülaosas 12 vastab tahukas tulemuste dokumendi arvu järgi.

- "`@odata.count`"<br/>
Päringu vastuseks näitab see väärtus otsingutulemite kattuvad üksuste arvu. Keskmiselt on suurem kui kõik tahukas tulemused kombineeritud tõttu üksused, mis vastavad otsingutermin, summa, kuid on vasteid tahukas väärtus.


**Tasemete lihvitud navigeerimine** 

Nagu märgitud, on otsese tugi pesastamine omadustega hierarhia. Välja kasti, lihvitud navigeerimise toetab ainult ühe taseme filtrid. Kuid lahendused olemas. Saate kodeerida hierarhilise tahukas struktuuris on `Collection(Edm.String)` hierarhia kohta valige üks kirje. Rakendatakse see lahendus on selles artiklis ei käsitleta, kuid saate lugeda OData [näiteks](http://msdn.microsoft.com/library/ff478141.aspx)saidikogumid. 

**Kinnitage väljad**

Kui koostate omadustega dünaamiliselt põhjal ebausaldusväärsete kasutaja sisendist loendit, tuleks teha kinnitage, et lihvitud väljade nimed on kehtiv, või escape nimed, kui hoone URL-id, kasutades ühte `Uri.EscapeDataString()` .net-i või samaväärne oma platvormi.

**Loendab tahukas tulemites.**

Filtri lisamisel lihvitud päringu võiksite säilitamise lause tahukas (nt `facet=Rating&$filter=Rating ge 4`). Tehniliselt, fassettidega = reiting ei ole vaja, kuid seda tagastab loendab tahukas väärtuste hinnangute 4 ja uuem versioon. Näiteks, kui kasutaja klõpsab "4" ja päring sisaldab suurem või võrdne "4" filter, loendab tagastatakse, iga reiting, mis on 4 ja üles.  

**Tahukatega loendab sharding mõju**

Teatud juhtudel võib juhtuda, fassettidega loendab ei vasta komplektid, (vt [lihvitud navigeerimise Azure'i otsingus (Foorum post)](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Tahukatega arv võib olla ebatäpne tõttu sharding arhitektuur. Iga otsinguregistri on mitu shards ja igaüht aruannete ülemised N omadustega dokumendi arvu, mis seejärel ühendatakse ühe tulemi järgi. Kui mõni shards on palju vastavuses olevatele väärtustele, samas kui teised on väiksem, võib juhtuda, et mõned tahukas väärtused on puudu või jaotises arvestatud tulemuste.

Kuigi selline käitumine võib igal ajal muuta, kui teil tekib selline käitumine täna, saate töötada ümber, kunstlikult täispumbatud arvu:<number> jõustamiseks täielik teateid iga Kildu väga suurte arvuks. Kui arvu väärtus: on suurem kui või võrdne välja Üheste väärtuste arvu, siis on tagatud õigete tulemuste. Juhul, kui dokumendi loendab on väga kõrge, on jõudluse, seega see suvand läbimõeldult kasutada.

<a name="rangefacets"></a>
##<a name="facet-navigation-based-on-a-range-values"></a>Tahukatega navigeerimise vahemiku väärtuste põhjal

Faceting üle vahemikud on levinud otsingu rakenduse nõue. Vahemike on toetatud arvandmete ning kuupäeva ja kellaaja väärtusi. Lugege iga lähenemisviisi [Otsi dokumente (Azure'i otsingu API)](http://msdn.microsoft.com/library/azure/dn798927.aspx)kohta.

Azure'i otsingu lihtsustab vahemiku ehituse pakkudes arvutuste vahemiku kahel viisil. Mõlema lähenemisel Azure'i otsingu loob antud andsite sisendeid vastav vahemikud. Näiteks, kui määrate vahemiku väärtuste 10 | 20 | 30, loob see automaatselt lahtrivahemike 0 -10, 10 – 20, 20 – 30. Valimi taotlus eemaldab kõik intervalle, mis on tühi. 

**Lähenemisviisi 1: Kasutage parameetrit intervall**<br/>
Hind omadustega $10 kaupa seadmiseks määrate:`&facet=price,interval:10`


**Lähenemisviisi 2: Väärtuste loendi kasutamine**<br/>
Arvulised andmed, saate väärtuste loend.  Kaaluge tahukas vahemiku jaoks listPrice, renderdada järgmiselt:

  ![][5]

Väärtuste loendi abil **CatalogSearch.cs** failis on määratud vahemiku:

    facet=listPrice,values:10|25|100|500|1000|2500

Kõigil vahemikel on loodud, kasutades lähtepunktina nimega lõpp-loendist väärtus 0 ja seejärel lõigatud eelmise vahemiku eraldi intervallide loomiseks. Azure'i otsingu näidatakse seda osana lihvitud navigeerimine. Teil pole struktureerimine iga intervalli koodi kirjutamist.

### <a name="build-a-filter-for-facet-ranges"></a>Filtri tahukas vahemike koostamine ###

Dokumentide kasutaja valitud vahemiku põhjal filtreerida, võite kasutada funktsiooni `"ge"` ja `"lt"` filtreerimine tehtemärke kaheosaline avaldis, mis määratleb vahemiku lõpp-punktid. Näiteks, kui kasutaja valib vahemikku 10 – 25, filtri oleks `$filter=listPrice ge 10 and listPrice lt 25`.

Rakenduses valimi filtriavaldis kasutab **priceFrom** ja **priceTo** parameetrite lõpp-punktid. **BuildFilter** meetod **CatalogSearch.cs** sisaldab filtriavaldis, mis annab teile dokumentide vahemikus.

  ![][6]

<a name="geofacets"></a> 
##<a name="filtered-navigation-based-on-geopoints"></a>GeoPoints alusel filtreeritud navigeerimine

See on levinud kuvamiseks filtreerib valite poest, restorani või vastavalt teie praeguses asukohas lähedus sihtkoht abi. Kuigi lihvitud navigeerimine võib välja näha selline filter, on tegelikult lihtsalt filtri. Mainida siin neile, kes soovivad spetsiaalselt rakendamise juhised selle probleemi kindla kujundus.

Azure'i otsing, **geo.distance** ja **geo.intersects**on kaks ruumilisel.

- Funktsioon **geo.distance** tagastab kauguse km ühest, ühe on välja ja üks on konstandi edasi filtri osana. 

- Tagastab väärtuse true, kui antud punkti on antud hulknurga, kus on välja ja hulknurga on määratud koordinaadid edasi filtri osana konstandi loendi **geo.intersects** funktsioon.

Filtri näited leiate [OData avaldise süntaksisse (Azure'i otsing)](http://msdn.microsoft.com/library/azure/dn798921.aspx).

<a name="tryitout"></a>
##<a name="try-it-out"></a>Proovige järele!

Azure'i otsingu Adventure Works Demo codeplexis sisaldab viidatud selle artikli näited. Otsingutulemite töötamisel vaadake muudatuste päringu ehituse URL-i. See rakendus ei toimu lisandada omadustega URI valimisel iga.

1.  Konfigureerige valimi rakenduse kasutamiseks oma teenuse URL-i ja api võti. 

    Pange tähele skeem, mis on määratletud Program.cs faili CatalogIndexer projekti. Seda määrab värvi, listPrice, suuruse, kaalu, KatregooriaID ja modelName facetable väljad.  Vaid mõned nende (värvi, listPrice, KatregooriaID) on tegelikult rakendatud lihvitud navigeerimine.

3.  Käivitage rakendus. 

    Esialgu on nähtav ainult välja Otsing. Klõpsake nuppu Otsi kohe kõik saavutamine või tippige otsitav termin.

    ![][7]
 
4.  Sisestage otsingutermin, näiteks ratta, ja klõpsake nuppu **Otsi**. Päringu aktiveeritakse kiiresti.
 
    Lihvitud navigeerimisstruktuuri tagastada ka otsingu tulemustega.  URL-i, omadustega värve, kategooriad ja hinnad on null. Otsingutulemites, sisaldab lihvitud navigeerimisstruktuuri loendab iga tahukas tulemi jaoks.

     ![][8]
 
5.  Klõpsake soovitud värvi, kategooria ja hinna vahemik. Omadustega on tühi, kuvatakse algse otsing, kuid mida nad kasutavad väärtuste, otsingutulemite on kärbitud üksused, mis ei vasta. Pange tähele, et URI jätkab päringu muudatustest.

    ![][9]
 
6.  Saate proovida erinevaid päringu käitumist lihvitud päringu tühjendamiseks klõpsake **AdventureWorks kataloogi** lehe ülaosas.

    ![][10]
 
<a name="nextstep"></a>
##<a name="next-step"></a>Järgmise juhise juurde

Kontrollige oma teadmisi, saate lisada *modelName*tahukas väli. Indeks on juba häälestatud jaoks selle tahukas nii muutusteta indeks on nõutav. Kuid te peate muutmine HTML-i lisada uue tahukas mudelite ja päringu ehitaja tahukas välja lisamine.

Rohkem ülevaateid kujundus põhimõtetest lihvitud navigeerimine, soovitame järgmisi linke:

- [Lihvitud otsingu jaoks kujundamine](http://www.uie.com/articles/faceted_search/)
- [Kujundus mustrid: Lihvitud navigeerimine](http://alistapart.com/article/design-patterns-faceted-navigation)

Saate vaadata ka [Azure otsingu Deep Dive](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410). 45:25, on demo kuidas rakendada aspekte.

<!--Anchors-->
[How to build it]: #howtobuildit
[Build the presentation layer]: #presentationlayer
[Build the index]: #buildindex
[Check for data quality]: #checkdata
[Build the query]: #buildquery
[Tips on how to control faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/Facet-1-slide.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: http://msdn.microsoft.com/library/azure/dn798921.aspx
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: http://msdn.microsoft.com/library/azure/dn798927.aspx

 