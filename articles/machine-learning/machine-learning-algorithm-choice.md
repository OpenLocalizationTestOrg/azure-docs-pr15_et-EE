<properties
    pageTitle="Kuidas valida kohapeal õppe algoritme | Microsoft Azure"
    description="Kuidas valida Azure masin õppe algoritme järelevalve all ja järelevalveta õppe Klasterdamine, klassifikatsiooni või regressiooni katsetes."
    services="machine-learning"
    documentationCenter=""
    authors="brohrer"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>
    
<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="08/09/2016"
    ms.author="brohrer;garye" />

# <a name="how-to-choose-algorithms-for-microsoft-azure-machine-learning"></a>Kuidas valida Microsoft Azure'i masinõppe algoritmid

Vastus küsimusele "Milline masin õppe algoritm kasutada?" on alati "See sõltub." See sõltub suuruse, kvaliteedi ja et andmed. See sõltub, mida sa tahad teha vastus. See sõltub sellest, kuidas matemaatikat algoritmi oli tõlgitud juhised kasutatava arvuti. Ja see sõltub sellest, kui palju aega teil on. Isegi kõige kogenenumad andmete teadlased ei saa öelda, millist algoritmi täidab parim enne neid.

## <a name="the-machine-learning-algorithm-cheat-sheet"></a>Masinõpet algoritm Cheat Sheet

**Microsoft Azure masin õppe algoritm Cheat Sheet** aitab teil valida õige masina õppimisvõime algoritm oma ennustav lahendusi Microsofti Azure masin õppe algoritme Raamatukogu.
See artikkel sisaldab üksikasjalikke kasutusvõimaluste.

> [AZURE.NOTE] Alla laadida petma lehte ja järgige selles artiklis koos, minge [masinate algoritm petma lehte Microsoft Azure masin õppe Studio](machine-learning-algorithm-cheat-sheet.md).

Käesoleva juhendaja on väga konkreetsele sihtgrupile meeles: alguses andmed teadlane koos undergraduate taseme masinõpet, et valida algoritmi alustada Azure masin õppe Studio. See tähendab, et teatud üldistuste tegemiseks ja oversimplifications, et see punkt te suunda. See tähendab ka, et mitmeid algoritme, mis on loetletud siin. Azure'i masinõppe kasvab sisaldama täielikumat meetoditega tõestada, lisame need.

Need soovitused on koostatud tagasiside ja palju andmeid teadlased ja masin spetsialistide nõuandeid. Me ei ole nõus kõike, kuid olen püüdnud ühtlustada meie arvamused arvesse töötlemata konsensust. Enamik kirjadest lahkarvamused algavad "See sõltub..."

### <a name="how-to-use-the-cheat-sheet"></a>Kuidas kasutada petma lehte

Loe tee ja algoritm etiketid diagramm nimega "jaoks * &lt;tee silt&gt; * kasutada * &lt;algoritmi&gt;*." Näiteks "For *speed* kasutada *kahe klassi logistilise regressiooni*." Kohaldatakse mõnikord mitu haru.
Mõnikord ei ükski neist täiuslik sobib. Nad on mõeldud reegel pöidla soovitused, nii et ärge muretsege see on täpne.
Rääkisin ütles et andmete levitava leida parimaid algoritmi ainus kindel viis on proovida neid kõiki.

Siin on näide eksperimendi, mis püüab mitu algoritme vastu samu andmeid ja võrreldakse tulemusi [Cortana luure Galerii](http://gallery.cortanaintelligence.com/) : [võrrelda mitme klassi klassifikaatorite: kiri tunnustamise](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

>[AZURE.TIP] Alla laadida ja printida diagrammi, kus antakse ülevaade kohapeal õppe Studio võimalusi, vt [joonis Azure masin õppe Studio võimalusi](machine-learning-studio-overview-diagram.md).

## <a name="flavors-of-machine-learning"></a>Masinõpet maitsed

### <a name="supervised"></a>Järelevalve all

Kuuluva õppe algoritme teha prognoose näidete põhjal. Näiteks saab ajaloolise aktsiahindade ohu arvailtaisiin tulevikus hindadega. Iga näide kasutada treeninguks nimeks on huvi väärtus — sellisel juhul aktsiahind. Kuuluva õppe algoritm otsib nende väärtus märgiste mustrid. Seda saab kasutada mis tahes teavet, mis võib olla oluline — päeval nädalas, hooaja, ettevõtte finantsandmete, tööstusharu, häiriv geopolicitical sündmuste esinemine — ja iga algoritm otsib erinevaid mustreid. Pärast algoritm on leitud parimaks muster on võimalik, et muster kasutab oletuste sildita testide andmeid — homse eest.

See on populaarne ja kasulik tüüpi masinõpet. Ühe erandiga kõik Azure masin õppe moodulite Finantsinspekt õppe algoritme. On mitmete eri tüüpi kuuluva õppe, mis on esitatud Azure masin õppe jooksul: klassifikatsioon, regressiooni ja anomaaliate avastamise.

* **Klassifikatsioon**. Andmeid kasutatakse ennustada kategooria, nimetatakse järelevalve alla kuuluv õppe ka klassifikatsioon. See on nii, kui paigutada pilt pildina "kass" või "koer". Kui ainult kaks valikut, seda nimetatakse **kaks klassi** või **ladinakeelne klassifikatsioon**. Kui on veel kategooriaid, nagu prognoosimisel NCAA Märts Madness turniiri võitja, see probleem on tuntud **mitme klassi klassifikatsioon**.

* **Regressiooni**. Kui väärtus on ennustatakse, nagu ka aktsiahinnad, nimetatakse järelevalve alla kuuluv õppe regressiooni.

* **Anomaaliate avastamiseks**. Mõnikord eesmärk on selgitada andmepunktid, mis on lihtsalt ebatavaline. Pettuste avastamine, näiteks kõik väga ebatavaline krediitkaardi kulude struktuuri on kahtlane. Võimalikke erinevusi on nii palju ja nii vähe, et ei ole võimalik õppida mida pettuse koolitus näited näeb. Lähenemisviisi, mis võtab anomaaliate avastamiseks on lihtsalt teada, mida tavaline tegevus välja (kasutades ajalugu-pettusega seotud tehingute) ja midagi, mis on oluliselt erinev.

### <a name="unsupervised"></a>Järelevalveta

Järelevalveta õppe andmepunktid on seostatud silte. Algoritmi järelevalveta õppe eesmärk on hoopis kuidagi andmete korraldamiseks või selle struktuuri kirjeldamiseks. See võib tähendada rühmitades klastrite või leida eri viiside ning keerulisi andmeid, et tundub lihtsam või rohkem organiseeritud.

### <a name="reinforcement-learning"></a>Õppe tugevdamine

Õppe tugevdamine algoritmi saab valida vastava toimingu iga andmepunkti vastuseks. Õppe algoritmi saab ka tasu signaali mõni aeg hiljem, mis näitab, kui hea otsus oli.
Selle algoritmi muudab oma strateegiat selleks, et saavutada kõrgeima auhinna. Praegu ei ole algoritm moodulid Azure kohapeal õppida õppimise tugevdamine. Õppe tugevdamine on levinud robootika, kui andurite lugemeid ühel hetkel ajas on andmepunkti ja algoritm tuleb valida roboti järgmise tegevuse. Samuti on looduslik sobivad asjade Interneti rakendusi.

## <a name="considerations-when-choosing-an-algorithm"></a>Kaalutlused valides algoritmi

### <a name="accuracy"></a>Täpsus

Kõige täpsem vastus võimalik saada pole alati vaja.
Vahel on piisav, seda kasutada soovitud ühtlustamiseks. Kui see juhtub, võimalik lõigata töötlemise ajal oluliselt rohkem ühtlustada meetodeid seismise. Rohkem ühtlustada meetodeid teine eelis on see, et nad loomulikult kipuvad vältima [overfitting](https://youtu.be/DQWI1kvmwRg).

### <a name="training-time"></a>Koolituse ajal

Mitu minutit või tundi väljaõppeks mudel erineb väga palju algoritme. Koolituse aeg on sageli tihedalt seotud täpsus — üks on tavaliselt lisatud teine. Lisaks mõned algoritmid on andmepunktide arv tundlikumad kui teised.
Kui aeg on piiratud seda autot algoritm, valik, eriti kui andmed on suur.

### <a name="linearity"></a>Lineaarsuse

Palju masin õppe algoritme teha lineaarsuse kasutada. Lineaarne klassifitseerimise algoritmid endale, et klassid on võimalik eristada sirge (või kõrgem-mõõtmelise analoog). Need hõlmavad logistilist regressiooni ning toetab vector masinad (nagu Azure'i masinõppe rakendamist).
Lineaarse regressiooni algoritme endale, et andmeid suundumusi jälgida otse. Need oletused ei ole halb ette mõningaid probleeme, kuid teised nad alandada täpsust.

![Mittelineaarsed klassi bounday][1]

***Mittelineaarsed klassi piiri*** *-tuginedes lineaarne klassifikatsioon algoritm tulemuseks madal täpsus*

![Järgides mittelineaarsete andmete][2]

***Järgides mittelineaarsete andmete*** *-lineaarse regressiooni meetodil annaks palju suuremad vead, kui on vajalik*

Vaatamata oma ohud, lineaarne algoritmid on väga populaarne, kui esimene rida rünnak. Enamasti on tegemist algorithmically lihtne ja kiire treenida.

### <a name="number-of-parameters"></a>Parameetrite arv

Parameetrid on andmete teadlane saab muuta algoritmi seadistamisel nupud. Need on numbrid, mis mõjutavad selle algoritmi käitumine, nagu lubatud viga või iteratsioonide või Valikud variante algoritm käitumise vahel. Koolituse ajal ja täpsuse algoritmi võib mõnikord olla üsna tundlik saada lihtsalt õiget sätted. Tavaliselt nõuavad algoritme suure hulga parameetritega kõige eksituse leida hea kombinatsioon.

Samuti ei [parameeter pühkimine](machine-learning-algorithm-parameters-optimize.md) moodul ploki Azure'i masinõppe, et automaatselt kõik mis tahes granulaarsus valite parameetri kombinatsioonidele. Kuigi see on suurepärane võimalus veendumaks, et olete mõõteulatus parameeter ruumi, aega, et mudel rong suureneb eksponentsiaalselt parameetrite arv.

Positiivsest küljest on, et võttes mitu parameetrit tavaliselt näitab, et algoritm on paindlikumad. On tihti võimalik saavutada väga hea täpsus. Sätestatud leiad parameetrisätetest õiget kombinatsiooni.

### <a name="number-of-features"></a>Mitmeid funktsioone

Teatud tüüpi andmeid funktsioone võib olla väga suur võrreldes andmepunktide arv. See kehtib eelkõige geneetika või tekstandmeid. Suur hulk funktsioone saab raba näha mõned õppe algoritme, muudab koolituse unfeasibly pikka aega. Vektori värvid suurused sobivad eriti hästi juhul (vt allpool).

### <a name="special-cases"></a>Erijuhtudel

Mõned õppe algoritme oletusi kindla struktuuri andmeid või soovitud tulemusi. Kui sa leiad ühe, mis sobib teie vajadustele, siis võib see anda rohkem kasulikke tulemusi, täpsemaid prognoose või kiiremini koolituse korda.

|**Algoritm**|**Täpsus**|**Koolituse ajal**|**Lineaarsuse**|**Parameetrid**|**Märkmed**|
|---|:---:|:---:|:---:|:---:|---|
|**Kaks klassi klassifikatsioon**| | | | | |
|[logistilise regressiooni](https://msdn.microsoft.com/library/azure/dn905994.aspx)                    | |●|●|5| |
|[otsuse metsa](https://msdn.microsoft.com/library/azure/dn906008.aspx)|●|○| |6| |
|[otsuse džungel](https://msdn.microsoft.com/library/azure/dn905976.aspx)|●|○| |6|Väike mälukasutus|
|[võimendatud Otsustepuu](https://msdn.microsoft.com/library/azure/dn906025.aspx)|●|○| |6|Suur mälukasutus|
|[Närvivõrgus](https://msdn.microsoft.com/library/azure/dn905947.aspx)|●| | |9|[Täiendav kohandamine on võimalik](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[Keskmine perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx)|○|○|●|4| |
|[toetada vektori masin](https://msdn.microsoft.com/library/azure/dn905835.aspx)| |○|●|5|Hea suur funktsioon määrab|
|[kohapeal sügav toetust vektori masin](https://msdn.microsoft.com/library/azure/dn913070.aspx)|○| | |8|Hea suur funktsioon määrab|
|[Bayes' DH](https://msdn.microsoft.com/library/azure/dn905930.aspx)| |○|●|3| |
|**Mitme klassi klassifikatsioon**| | | | | |
|[logistilise regressiooni](https://msdn.microsoft.com/library/azure/dn905853.aspx)| |●|●|5| |
|[otsuse metsa](https://msdn.microsoft.com/library/azure/dn906015.aspx)|●|○| |6| |
|[otsuse džungel](https://msdn.microsoft.com/library/azure/dn905963.aspx)|●|○| |6|Väike mälukasutus|
|[Närvivõrgus](https://msdn.microsoft.com/library/azure/dn906030.aspx)|●| | |9|[Täiendav kohandamine on võimalik](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[ühe-v-kõik](https://msdn.microsoft.com/library/azure/dn905887.aspx)|-|-|-|-|Kaks klassi meetodi valitud majutusasutusi|
|**Regressiooni**| | | | | |
|[lineaarne](https://msdn.microsoft.com/library/azure/dn905978.aspx)| |●|●|4| |
|[Bayesi lineaarse](https://msdn.microsoft.com/library/azure/dn906022.aspx)| |○|●|2| |
|[otsuse metsa](https://msdn.microsoft.com/library/azure/dn905862.aspx)|●|○| |6| |
|[võimendatud Otsustepuu](https://msdn.microsoft.com/library/azure/dn905801.aspx)|●|○| |5|Suur mälukasutus|
|[Kiire metsa quantile](https://msdn.microsoft.com/library/azure/dn913093.aspx)|●|○| |9|Väljamakseid, mitte punkt ennustused|
|[Närvivõrgus](https://msdn.microsoft.com/library/azure/dn905924.aspx)|●| | |9|[Täiendav kohandamine on võimalik](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[Poissoni](https://msdn.microsoft.com/library/azure/dn905988.aspx)| | |●|5|Tehniliselt log-lineaarne. Arvu prognoosimiseks|
|[järgarvu](https://msdn.microsoft.com/library/azure/dn906029.aspx)| | | |0|Ennustavad auaste-tellimine|
|**Anomaaliate avastamiseks**| | | | | |
|[toetada vektori masin](https://msdn.microsoft.com/library/azure/dn913103.aspx)|○|○| |2|Eriti hea suur funktsioon määrab|
|[PKL-ga anomaaliate avastamiseks](https://msdn.microsoft.com/library/azure/dn913102.aspx)| |○|●|3| |
|[K-vahendid](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)| |○|●|4|Klastrite algoritm|


**Algoritm omadused:**

**●** - näitab suurepärane täpsus ja kiire koolituse korda lineaarsuse kasutamine

**○** - näitab hea täpsus ja mõõduka koolitus korda

## <a name="algorithm-notes"></a>Algoritm märkmed

### <a name="linear-regression"></a>Lineaarse regressiooni

Nagu eespool mainitud, sobib [lineaarse regressiooni](https://msdn.microsoft.com/library/azure/dn905978.aspx) joon (või lennuk või hyperplane) andmeid. See on tööhobune, lihtne ja kiire, kuid on liiga lihtne, mõned probleemid.
[Lineaarse regressiooni õpetus](machine-learning-linear-regression-in-azure.md)siit.

![Lineaarse trendi andmeid][3]

***Lineaarse trendi andmeid***

### <a name="logistic-regression"></a>Logistilise regressiooni

Kuigi äravahetamiseni hõlmab "regressioon" nimi, logistilise regressiooni on tegelikult võimas [kaks klassi](https://msdn.microsoft.com/library/azure/dn905994.aspx) ja [multiclass](https://msdn.microsoft.com/library/azure/dn905853.aspx) klassifikatsioon. See on kiire ja lihtne. Asjaolu, et ta kasutab s '-kujuline kõver sirge asemel muudab andmete jagamise rühmadesse loomulik seisund. Logistilise regressiooni annab lineaarne astmetevahelised piirid, seega kui kasutate seda, veenduge, et lineaarne ühtlustamise on midagi, mida sa võid elada.

![Logistiline regressioon kaks klassi andmed ühe funktsiooniga][4]

***Logistilise regressiooni kaks klassi andmetele vaid ühe funktsiooni*** *-klassi piiri on punkt, kus logistiline kõver on lihtsalt nii lähedal mõlemat*

### <a name="trees-forests-and-jungles"></a>Džunglisse puud ja metsad

Otsuse metsad ([regressiooni](https://msdn.microsoft.com/library/azure/dn905862.aspx), [kahe -](https://msdn.microsoft.com/library/azure/dn906008.aspx)ja [multiclass](https://msdn.microsoft.com/library/azure/dn906015.aspx)), otsuse džunglisse ([kaks klassi](https://msdn.microsoft.com/library/azure/dn905976.aspx) ja [multiclass](https://msdn.microsoft.com/library/azure/dn905963.aspx)) ja võimendatud otsus puud ([regressiooni](https://msdn.microsoft.com/library/azure/dn905801.aspx) ja [kaks klassi](https://msdn.microsoft.com/library/azure/dn906025.aspx)) kõik põhinevad otsuse puud foundational masin õppe mõiste. On palju variante, otsus puud, kuid nad kõik teevad sama asja — funktsioon ruumi jagada piirkondadeks enamasti sama sildiga. Need võivad olla järjepidev kategooria või püsivat väärtust, sõltuvalt sellest, kas te teete klassifikatsiooni või regressioon.

![Otsustepuu jaotab funktsioon ruumi][5]

***Otsustamisskeemi jaotab funktsioon ruumi piirkondadesse umbes ühtsed väärtused***

Kuna funktsioon ruumi jagatakse suvaliselt väikeste piirkondade, on lihtne ette kujutada, jagades piisavalt peeneks on üks andmepunkt piirkonna – äärmuslik näide overfitting. Selle vältimiseks suur hulk puid on valmistatud matemaatiline erihooldust võetud puud ei korreleeru. "Otsus metsa" on puu, mis väldib overfitting. Otsuse metsade kasutada palju mälu. Otsuse džunglisse on variant, mis tarbib vähem mälu kulul väljaõppe veidi kauem aega.

Võimendatud otsus puud vältida overfitting, mitu korda nad jagada ja kui vähe on lubatud igas piirkonnas. Algoritm ehitab jada puud, millest igaüks õpib kompenseerida jäänud puu enne viga. Tulemuseks on väga täpne õppija, et kipub palju mälu kasutada. Täielik tehniline kirjeldus, vaadake [Friedman on originaal paber](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Kiire metsa quantile regressioon](https://msdn.microsoft.com/library/azure/dn913093.aspx) on otsus puude erijuhtum, kus soovite teada mitte ainult andmete piirkonnas, kuid ka selle jaotuse quantiles kujul tüüpiline (mediaan) väärtus.

### <a name="neural-networks-and-perceptrons"></a>Närvivõrgud ja perceptrons

Närvivõrgud on aju inspireeritud õppe algoritme, mis hõlmab [multiclass](https://msdn.microsoft.com/library/azure/dn906030.aspx), [kaks klassi](https://msdn.microsoft.com/library/azure/dn905947.aspx)ja [regressiooni](https://msdn.microsoft.com/library/azure/dn905924.aspx) . Nad tulevad lõpmatu mitmekesisus, kuid maksimaalselt Azure'i masinõppe närvivõrgud on kõik suunatud atsüklilised graafikud näol. See tähendab, et häälsisestuse edasi edasi (kunagi tahapoole) kihid kaudu enne on muutunud väljundid. Igal tasandil on sisendite kaalutud kombinatsioone, kokku võtta ja edasi järgmise kihi. See kombinatsioon lihtsaid arvutusi tulemusi võime õppida kogenud klassi piirid ja andmeid suundumusi, näiliselt maagiliste. Sellised võrgustikud mitu kihiline teha "sügavat õppimist", mis toidab nii palju tech aruandlus ja ulme.

See suure jõudlusega ei tule tasuta, kuigi. Närvivõrgud võib pikka aega rongi, eriti suure andmekogumi saatmiseks koos palju funktsioone. Neil on ka rohkem parameetreid kui enamik algoritme, mis tähendab, et parameetri pühkimine laiendab koolituse ajal väga palju.
Ja overachievers, kes soovivad [määrata oma võrgu struktuur](http://go.microsoft.com/fwlink/?LinkId=402867), võimalused on ammendamatu.

<a name="boundaries-learned-by-neural-networks6"></a>![Õppinud närvivõrgud piirid][6]
---------------------------

***Õppinud närvivõrgud piire võib olla keeruline ja ebaregulaarne***

[Kaks klassi, mis on perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) on neuraaltoru võrgustikke vastus skyrocketing koolituse korda. Ta kasutab võrgu struktuur, mis annab lineaarne astmetevahelised piirid. On peaaegu primitiivne tänapäevaste normide järgi, kuid see on pikka aega töötada jõuliselt ja on piisavalt väike, et kiiresti õppida.

### <a name="svms"></a>SVMs

Vektori värvid suurused (SVMs) leida piiri, mis eraldab vastuvõetud kui varu võimalikult lai. Kui kaks klassi ei saa selgelt eraldi, algoritme leida parim võimalik piiri. Nagu on Azure'i masinõppe, [kaks klassi SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx) see ainult sirge joonega. (SVM-rääkida, ta kasutab lineaarset tuuma.) Kuna lineaarne ühtlustamisega tagab, on võimalik üsna kiiresti. Kui ta tõesti paistab on funktsioon-intensiivne andmetega, nagu tekst või genoomse. Sellisel juhul on võimalik eraldi klassid, kiiremini ja vähem overfitting kui enamik teisi algoritme, lisaks nõuda vaid tagasihoidlik kogus mälu SVMs.

![Toetust vektori masin klassi piiri][7]

***Tüüpiline toetust vektori masin klassi piiri maksimeerib eraldab kahte klassi marginaal***

Teise toote Microsoft Research, [kaks klassi kohapeal sügav SVM](https://msdn.microsoft.com/library/azure/dn913070.aspx) on mittelineaarne variant SVM, mis säilitab enamuse lineaarne versioon kiirus ja mälu tõhusust. See on ideaalne juhul, kui lineaarse lähenemine ei anna piisavalt täpsed vastused. Arendajad säilitab see kiiresti probleem purustamine maha võtta hunnik väike lineaarne SVM probleeme. Loe [täielikku kirjeldust](http://research.microsoft.com/um/people/manik/pubs/Jose13.pdf) üksikasjade kuidas nad tõmmatakse välja seda trikki.

Kasutades tark pikendamine mittelineaarne SVMs, [esimese klassi SVM](https://msdn.microsoft.com/library/azure/dn913103.aspx) juhib piir, et tihedalt annab ülevaate kogu andmekogumi. See on kasulik anomaaliate avastamiseks. Uus andmepunkti, mis jäävad kaugele nimetatud piiri on ebatavaline, et olla märkimisväärne.

### <a name="bayesian-methods"></a>Bayesi meetodid

Bayesi meetodid on väga soovitav kvaliteet: nad vältida overfitting. Nad seda tehes eelnevalt mõned oletusi vastus jaotumist. Teine byproduct selle lähenemisviisi, et nad on väga vähe parameetreid. Azure'i masinõpet on mõlemad Bayesi algoritmid klassifikatsioon ([kaks klassi Bayes DH](https://msdn.microsoft.com/library/azure/dn905930.aspx)) ja regressiooni ([Bayesi lineaarse regressiooni](https://msdn.microsoft.com/library/azure/dn906022.aspx)).
Pange tähele, et need eeldada, et andmed saab jagada või sobivad sirge joonega.

Ajalooline Märkus, töötati Bayes' punkti masinad on Microsoft Research. Neil on mõned erakordselt ilus teoreetiline töö nende taga. Huvitatud õpilane on suunatud [algse artikli JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) ja [insightful blogi Chris piiskop](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Spetsiaalsed algoritmid

Kui teil on väga konkreetne eesmärk võib olla õnne. Azure'i masinõppe kogumikus on algoritme, mis on spetsialiseerunud rank Sisestusabi ([järgarvu regressioon](https://msdn.microsoft.com/library/azure/dn906029.aspx)), arvu ennustamine ([Poissoni regressioon](https://msdn.microsoft.com/library/azure/dn905988.aspx)) ja jälgitavus (üks [põhikomponentide analüüsi](https://msdn.microsoft.com/library/azure/dn913102.aspx) ja teise [vektori masin toetab](https://msdn.microsoft.com/library/azure/dn913103.aspx)s).
Ja seal on üksildane klastrite algoritm samuti ([K-vahendeid](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)).

![PKL-ga anomaaliate avastamiseks][8]

***PKL-ga anomaaliate avastamiseks*** *-suur osa andmeid satub stereotüüpsete jaotus; punktid nendel dramaatiliselt jaotus on kahtlustatava*

![Andmeid rühmitada, kasutades K][9]

***Andmed on grupeeritud kasutades K 5 klastrite***

On ka mis ansambel [ühe-v-kõik multiclass klassifitseerijale](https://msdn.microsoft.com/library/azure/dn905887.aspx), katkestades N-klassi liigituse probleem n-1 kaks klassi klassifitseerimise probleeme. Täpsus, koolituse ajal ja lineaarsust omadused määrab kaks klassi klassifikaatorite kasutada.

![Kaks klassi klassifikaatorite koos moodustavad kolme klassi klassifitseerijale][10]

***Kaks klassi klassifikaatorite paari ühendada, et moodustada kolme klassi klassifitseerijale***

Azure'i kohapeal õppida ka juurdepääsu võimas masin õppe raames [Vowpal Wabbit](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf)pealkirja.
VW trotsib kategoriseerimine siin, sest seda saab õppida klassifikatsioon ja regressiooni ja võite isegi õppida osaliselt sildita andmete. Te saate konfigureerida kasutada mitmeid algoritme, kaotus funktsioone ja optimeerimise algoritmid. See oli mõeldud maapinnast kuni tõhusus, paralleelne ja väga kiire. Ta tegeleb naeruväärselt suur funktsioonikogumit, ilmneb vähese vaevaga.
Alustas ja juhtis Microsoft Researchi John Langford, VW on Vormel 1 kanne väljal laos auto algoritme. Iga probleemi sobib VW, kuid kui sinu ei ei pruugi kuigi ronida õppimiskõver selle kasutajaliides. Seda turustatakse ka [eraldi avatud lähtekoodi](https://github.com/JohnLangford/vowpal_wabbit) mitmes keeles.


<!-- Media -->

[1]: ./media/machine-learning-algorithm-choice/image1.png
[2]: ./media/machine-learning-algorithm-choice/image2.png
[3]: ./media/machine-learning-algorithm-choice/image3.png
[4]: ./media/machine-learning-algorithm-choice/image4.png
[5]: ./media/machine-learning-algorithm-choice/image5.png
[6]: ./media/machine-learning-algorithm-choice/image6.png
[7]: ./media/machine-learning-algorithm-choice/image7.png
[8]: ./media/machine-learning-algorithm-choice/image8.png
[9]: ./media/machine-learning-algorithm-choice/image9.png
[10]: ./media/machine-learning-algorithm-choice/image10.png
