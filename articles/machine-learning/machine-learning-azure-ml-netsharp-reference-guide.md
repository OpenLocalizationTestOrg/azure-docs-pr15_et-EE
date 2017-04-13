<properties 
    pageTitle="Juhend Net # närvivõrgud määratlus keel | Microsoft Azure'i" 
    description="Süntaks on Net # neural võrkude määratlus keel koos kohandatud Närvivõrgus andmemudeli loomine Microsoft Azure'i ml Net kasutamise näited#" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jeannt" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="jeannt"/>



# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning"></a>Azure'i masina õppe Net # Närvivõrgus määratlus keele juhend

## <a name="overview"></a>Ülevaade
Net # on välja töötatud Microsoft, mis võimaldab määratleda Närvivõrgus arhitektuurides Närvivõrgus moodulid Microsoft Azure'i masina õppe keel. Selles artiklis kirjeldatakse:  

-   Seotud närvivõrgud põhimõtted
-   Närvivõrgus nõuded ja esmane komponentide määratlemine
-   Süntaks ja märksõnade Net # määratlus keel
-   Net abil loodud kohandatud närvivõrgud näited# 
    
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

## <a name="neural-network-basics"></a>Närvivõrgus põhialused
Närvivõrgus struktuuri koosneb ***sõlmed*** , mis on korraldatud ***kihid***, ja kaalutud ***ühendused*** (või ***servad***) sõlme vahel. Ühendused on suunamata ja iga ühendus on ***Allika*** sõlm ja ***sihtkoha*** sõlm.  

Iga ***koolitatavad kiht*** (peidetud või mõne väljundi layer) on üks või mitu ***ühendust kimbud***. Ühenduse komplekt koosneb allika kiht ja ühendused selle andmeallika kiht kirjelduse. Sama ***Allika kiht*** ja sama ***sihtkoht kiht***jagamiseks kõigi ühenduste antud kogumis. Net # peetakse ühenduse kogumi kuuluvad kogumisse tema sihtkoht kiht.  
 
NET # toetab erinevat tüüpi ühenduse kimbud, mis võimaldab teil kohandada viisi sisendeid on vastendatud peidetud kihid ja väljundid vastendatud.   

Vaike- või standard komplekt on **täielik kogum**, kus iga sõlm allika kihis on ühendatud iga sõlm sihtkoha kiht.  

Lisaks Net # toetab Täpsem ühendus kimbud järgmist nelja tüüpi:  

-   **Filtreeritud kimbud**. Kasutaja saab määratlemine predikaat allika kiht sõlm ja sõlme sihtkoha kiht asukohta. Sõlmed on ühendatud iga kord, kui predikaat on True.
-   **Convolutional kimbud**. Kasutaja saate määratleda small alasid sõlmed allpool. Iga sõlme sihtkoha kihis on ühendatud ühe reserveerimise sõlmed allpool.
-   **Ühendamise kimbud** ja **vastuse normaliseerimine komplektid**. Need on sarnane convolutional kimbud, et kasutaja määratletud small alasid sõlmed allpool. Erinevus on, et need pakendame servad kaalu ei ole koolitatavad. Selle asemel rakendatakse eelmääratletud funktsioon sõlm lähteväärtuste sihtkoha sõlm väärtuse määramiseks.  

Kasutades Net # Närvivõrgus struktuuri võimaldab määratleda keerukate struktuurid, nt sügav närvivõrgud või convolutions suvalise mõõtmed, mis on teada, et parandada õ andmete pilt, heli või video.  

## <a name="supported-customizations"></a>Toetatud kohandused
Loote Azure seadme õ Närvivõrgus mudeleid ülesehituse saab kohandada ulatuslikult kasutades Net #. Sa saad:  

-   Luua peidetud kihid ja reguleerimine kihiti sõlmed arv.
-   Saate määrata, kuidas kihid on omavahel ühendada.
-   Määratleda teisiti ühenduvuse struktuurid, nt convolutions ja ühiskasutuse kimbud paksus.
-   Saate määrata erinevaid aktiveerimise funktsioonid.  

Süntaksi määratlus keel leiate teemast [Struktuuri määratlus](#Structure-specifications).  
 
Näiteid määratlemine närvivõrgud Õppekeskuse ülesandeid, simplex keerukas, et mõned levinud masina leiate [näiteid](#Examples-of-Net#-usage).  

## <a name="general-requirements"></a>Üldised nõuded
-   Peab olema null või rohkem peidetud kihid täpselt ühe väljundi layer ja vähemalt üks sisendväärtuste kiht. 
-   Iga kiht on sõlmed, põhimõtteliselt korraldatud suvalise mõõtmed ristküliku massiivi kindla arvu. 
-   Sisestuskeel kihid on seotud koolitatud parameetreid pole ja kus eksemplari andmete siseneb võrgu tähistada. 
-   Koolitatavad kihid (peidetud ja väljund kihid) on seotud koolitatud parameetrid, kaalu ja hälvete tuntud. 
-   Lähte-kui ka sõlmed peavad olema eraldi. 
-   Ühendused peavad olema atsüklilised; Teisisõnu, ei saa olla ühendused, mis viib tagasi algseks sõlm ahelas.
-   Väljundi kiht ei saa andmeallika kiht ühenduse komplekt.  

## <a name="structure-specifications"></a>Struktuur tehnilised andmed
Närvivõrgus struktuuri määratlus koosneb kolmest osast: **pidev deklareerimise**, **kiht deklareerimise**, **ühenduse deklareerimise**. Olemas on ka kuvatakse **ühiskasutus deklareerimise** valikuline jaotis. Jaotised saab määrata mis tahes järjestuses.  

## <a name="constant-declaration"></a>Konstandi deklaratsioon 
Konstandi deklareerimise pole kohustuslik. See loob võimaluse määratlemiseks kasutatakse Närvivõrgus määratluse väärtused. Deklaratsioon lause koosneb identifikaatori järgneb võrdusmärk ja väärtus avaldis.   

Näiteks järgmine lause määratleb pidev **x**:  


    Const X = 28;  

Kahe või enama konstantide määratleda korraga, ümbritsege looksulgudega identifikaator nimed ning väärtused ja need semikoolonitega eraldada. Näiteks:  

    Const { X = 28; Y = 4; }  

Täisarv, reaalarv, loogikaväärtus (True või False) või matemaatilise avaldise, saab iga ülesande avaldis paremas servas. Näiteks:  

    Const { X = 17 * 2; Y = true; }  

## <a name="layer-declaration"></a>Kiht deklaratsioon
Kiht deklaratsiooni ei vaja. See määratleb suurus ja allikas kiht, sh selle ühenduse kimbud ja atribuute. Deklaratsioon lause kiht (sisestusteade, peidetud või väljund), nimi algab järgneb mõõtmed kiht (korteeži positiivsed täisarvud). Näiteks:  

    input Data auto;
    hidden Hidden[5,20] from Data all;
    output Result[2] from Hidden all;  

-   Toote mõõtmed on sõlmed kiht arv. Selles näites on kaks mõõtmed [5,20], mis tähendab kihis on 100 sõlmed.
-   Kihid saate deklareeritaks ükskõik millises järjekorras, ühe erandiga: kui määratud on rohkem kui üks sisestuskeel kiht, on deklareeritud tellimuse peab vastama funktsioonid sisendandmete järjestuse.  


Sõlmed kiht arv määratakse automaatselt määramiseks kasutage võtmesõna **Automaatne** . **Automaatne** märksõna on erinev mõju, sõltuvalt kiht.  

-   Sisestuskeel kiht avalduse sõlmed arv on mitmeid funktsioone, sisendandmete.
-   Peidetud kiht deklareerimise, sõlmed arv on arv, mis on määratud parameetri väärtus **peidetud sõlmed arv**. 
-   Väljundi kiht deklareerimise, sõlmed arv on kaks klassi liigitamine, 1 regressiooni ja väljundi sõlmed multiclass liigitamiseks arv võrdub 2.   

Näiteks järgmine võrgu määratlus võimaldab mahu kõik kihid määratakse automaatselt:  

    input Data auto;
    hidden Hidden auto from Data all;
    output Result auto from Hidden all;  


Kiht deklareerimise koolitatavad kiht (peidetud või väljund kihid) saate soovi korral kaasata väljundi funktsioon (nimetatakse ka aktiveerimise funktsioon), mis on vaikimisi **kujutavad** liigitamine mudelid ja **lineaarse** regressioonisirge mudelid. (Isegi juhul, kui te kasutate vaikimisi, te saate selgesõnaliselt funktsiooni aktiveerimisel, kui soovitud selgust.)

Väljundi järgmised funktsioonid on toetatud:  

-   kujutavad
-   lineaarne
-   softmax
-   rlinear
-   ruutu
-   sqrt
-   srlinear
-   ABS
-   tanh 
-   brlinear  

Näiteks järgmine deklaratsioon kasutab funktsiooni **softmax** :  

    output Result [100] softmax from Hidden all;  

## <a name="connection-declaration"></a>Ühenduse deklaratsioon
Kohe pärast määratlemine koolitatavad kiht, peab deklareerida olete määratlenud kihid ühendused. Ühenduse komplekt deklareerimise algab soovitud märksõna **kaudu**, millele järgneb nimi on komplekt allika kiht ja luua ühendus komplekt tüüpi.   

Praegu on viit tüüpi ühenduse kimbud toetatud:  

-   **Täielik** kimbud, tähistatud märksõna **Kõik**
-   **Filtreeritud** kimbud, mis tähistab soovitud märksõna **kus**järgneb põhiliste avaldis
-   **Convolutional** kimbud, mis on tähistatud soovitud märksõna **convolve**, konvolutsioon atribuute, millele järgneb
-   Märksõnade **max pool** või **tähendab rakenduskausta** kimbud **koondamine**
-   **Vastuse normaliseerimine** kimbud tähistatud märksõna **vastuse norm**      

## <a name="full-bundles"></a>Täielik kimbud  

Täielik ühendus komplekt sisaldab iga sõlme ühenduse iga sõlme sihtkoha layer allpool. See on vaikimisi võrgu ühenduse tüüp.  

## <a name="filtered-bundles"></a>Filtreeritud kimbud
Filtreeritud ühenduse kogumi määratlus sisaldab palju põhiliste, väljendatud süntaktiliselt, nt C# lambda avaldis. Järgmises näites määratleb kaks filtreeritud kimbud:  

    input Pixels [10, 20];
    hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  

-   Predikaat jaoks _ByRow_, **s** on parameetri tähistav registri üheks ristküliku massiiv sõlmed Sisestuskeel kiht, _pikslit_, ja **d** on parameetri tähistav registri üheks sõlmed peidetud kiht, _ByRow_massiiv. Nii **s** - **d** on täisarvud pikkus kahe korteeži. Põhimõtteliselt **s** vahemike üle täisarvude koos paari _0 < = s [0] < 10_ ja _0 < = s[1] < 20_, ja **d** ulatub üle paari täisarvud, kus _0 < = d [0] < 10_ ja _0 < = d[1] < 12_. 
-   Põhiliste avaldise paremas servas on tingimus. Selles näites iga väärtuse **s** - **d** nii, et tingimus on tõene, on serva allika kiht sõlme sihtkoha kiht sõlm. Seega näitab selle filtriavaldise kogumisse sisaldab määratletud **s** **d** kõik juhul, kui s [0] on võrdne d [0] määratletud sõlm sõlme ühenduse.  

Soovi korral saate määrata kogum, kaalu filtreeritud kogumi jaoks. **Kaalu** atribuudi väärtus peab olema korteeži ujuv punkt väärtused pikkusega, mis vastab määratud kogumisse ühenduste arv. Vaikimisi genereeritud juhusliku ID-ga kaalu.  

Weight (kaal) väärtused on rühmitatud sihtkoha sõlm register. Kui esimese sihtkoha sõlm on ühendatud K allika sõlmed, **kaalu** korteeži esimene _K_ elemendid on kaalu esimese sihtkoha sõlme allika index järjestuses. Sama kehtib ülejäänud sihtkoha sõlmed.  

On võimalik määrata kaalu otse konstandi väärtustena. Näiteks, kui soovite õpitut kaalu varem, saate need määrata nimega konstantide abil süntaksit:

    const Weights_1 = [0.0188045055, 0.130500451, ...]


## <a name="convolutional-bundles"></a>Convolutional kimbud
Kui koolitus andmed on ühtlase struktuuri, kasutatakse tavaliselt convolutional ühendused teavet andmete üksikasjalik funktsioone. Näiteks pildi-, heli- või videofailide andmeid, ruumiline või ajaline dimensionaalsus võib olla üsna ühtsed.  

Convolutional kimbud tööle ristküliku **tuumad** paigutatakse mõõtmete kaudu. Sisuliselt iga tuum määratleb **tuum rakenduste**edaspidi rakendatud kohaliku eeslinnades, kaalu kogumi. Iga tuum rakenduse vastab andmeallika kiht, mis on nimetatud **keskne sõlm**sõlm. Kaalu on tuum on ühiskasutuses ühendusi. Convolutional kogumis iga tuum on ristküliku ja kõik tuum rakendused on sama suurus.  

Convolutional kimbud ei toeta järgmisi atribuute.

**InputShape** määratleb dimensionaalsus allika kiht eesmärgil selle convolutional komplekt. Väärtus peab olema positiivsed täisarvud korteeži. Korrutis on täisarvud peab olema allpool sõlmed arvu, kuid vastasel korral ei ole vaja vastavad dimensionaalsus, deklareeritud allpool. See korteeži pikkus muutub convolutional kogumisse **arity** väärtus. (Tavaliselt arity viitab argumendid või operandi, mida saavad teha funktsiooni arvu.)  

Kujundi ja asukohtade tuumade määratlemiseks kasutada **KernelShape**, **jooksusammu**, **täidis**, **LowerPad**ja **UpperPad**atribuute:   

-   **KernelShape**: (nõutav) määratleb iga tuum convolutional koguhinna, dimensionaalsus. Väärtus peab olema positiivsed täisarvud pikkusega, mis võrdub arity kogumi korteeži. Iga osa sellest korteeži peab olema vastava osa **InputShape**üle. 
-   **Jooksusammu**: (valikuline) määratleb libistades samm suurused konvolutsioon (iga dimensiooni sammhaaval suurus), mis on keskne sõlmed vaheline kaugus. Väärtus peab olema positiivsed täisarvud pikkusega, mis on arity kogumi korteeži. Iga osa sellest korteeži peab olema vastava osa **KernelShape**üle. Vaikeväärtus on korteeži kõik komponendid ühte võrdne. 
-   **Ühiskasutus**: (valikuline) määratleb iga mõõtme konvolutsioon ühiskasutuse jaoks soovitud jämedus. Väärtus võib olla ühe kahendväärtuse või korteeži kahendmuutujaga väärtuste pikkusega, mis on arity kogumi. Ühe kahendväärtuse laiendatakse olema määratud väärtus võrdub kõigi komponentide õige pikkus korteeži. Vaikeväärtus on korteeži, mis koosneb täidetud kõik väärtused. 
-   **MapCount**: (valikuline) määratleb funktsioon arvu kaartide convolutional kogumisse. Väärtus võib olla üks positiivne täisarv või korteeži positiivse täisarvu pikkusega, mis on arity kogumi. Ühe täisarv on õige pikkus koos esimese komponendid määratud väärtus võrdub korteeži olevat ja kõik ülejäänud komponendid võrdne ühte. Vaikeväärtus on üks. Funktsioon kaartide arv on komponentide kordne korrutis. Faktooring kokku üle komponendid määratleb, kuidas funktsioon kaardi väärtused on rühmitatud sihtkoha sõlmed. 
-   **Kaalu**: (valikuline) määratleb algse kaalu koguhinna. Väärtus peab olema korteeži ujuva punkt väärtused pikkusega, mis on mitu korda tuumad kaalu arvu kohta tuum, selle artikli määratletud. Vaikimisi kaalu on genereeritud juhusliku ID-ga.  

On kahte tüüpi atribuudid, mis juhib täidise, teineteist atribuudid.

-   **Täidis**: (valikuline) määrab, kas sisend peaks olema tegeliku **vaikimisi täidise värviskeemi**abil. Väärtus võib olla üks loogikaväärtus või võib olla korteeži kahendmuutujaga väärtuste pikkusega, mis on arity kogumi. Ühe kahendväärtuse laiendatakse olema määratud väärtus võrdub kõigi komponentide õige pikkus korteeži. Kui mõõde väärtus on True, allika on loogiliselt tegeliku selle mõõde toetamiseks tuum täiendavad rakendused, lahtrid väärtusega null abil nii, et keskse sõlmed esimese ja viimase tuumade selle mõõde on ees- ja perekonnanimi sõlmed selle andmeallika kiht mõõde. Seega iga dimensiooni "fiktiivne" sõlmed arv määratakse automaatselt, mahutamiseks täpselt _(InputShape [d] - 1) / jooksusammu [d] + 1_ tuumad kihti, pehmendatud allikas. Kui mõõde väärtus on False, tuumade on määratletud nii, et sõlmed mõlemal küljel, mis on välja jäetud arv on sama (kuni 1 erinevus). Atribuudi vaikeväärtus on korteeži kõik komponendid võrdne False.
-   **UpperPad** ja **LowerPad**: (valikuline) Sisesta paremini juhtida täidise kasutada summa. **Oluline:** Neid atribuute saate määratletud ainult juhul, kui ülaltoodud **täidis** atribuut on määratletud ***ei*** . Väärtused peaks olema täisarvuline kordseid pikkusega, mis on arity kogumi. Kui määratud on neid atribuute, "fiktiivne" sõlmed lisatakse iga mõõtme Sisestuskeel kiht alumise ja ülemise lõpetatakse. Sõlmed arv lisatakse alumise ja ülemise otsa iga mõõde on määratud **LowerPad**[i] ja [k] **UpperPad**vastavalt. Selleks, et tuumad vasta ainult "real" sõlmed, mitte "fiktiivne" sõlmed, peavad olema täidetud järgmised tingimused:
    -   Iga komponendi **LowerPad** peab olema tingimata vähem kui KernelShape [d] / 2. 
    -   Iga komponendi **UpperPad** peab olema väiksem kui KernelShape [d] / 2. 
    -   Neid atribuute vaikeväärtus on korteeži kõik komponendid, mis on võrdne 0. 

Selle sätte **täidis** = true võimaldab nii palju täidise vajavad "keskpunkt" tuum "real" sees sisestusteade. See muudab matemaatika veidi arvutamisel väljundi suurus. Üldiselt väljundi suurus _D_ arvutatakse järgmiselt _D = (I - K) / S + 1_, kus _I_ on sisendi suurus, _K_ on tuum suurus, _S_ on sammu, ja _/_ on jagatise täisarvulise (round nulli suunas). Kui seate UpperPad = [1, 1] on sisendi suurus _ma_ sisuliselt 29, ja seetõttu _D = (29-5) / 2 + 1 = 13_. Juhul, kui **täidis** = true, põhiosas, _ma_ saab bumped kuni _K - 1_; Seega _D = ((28 + 4) - 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14_. **UpperPad** ja **LowerPad** väärtuste määramisega saate palju rohkem kontrolli täidise kui kui seate lihtsalt **täidis** = true.

Lisateavet convolutional võrkude ja nende rakenduste kohta leiate järgmistest artiklitest:  

-   [http://deeplearning.net/tutorial/Lenet.html](http://deeplearning.net/tutorial/lenet.html )
-   [http://Research.microsoft.com/pubs/68920/icdar03.pdf](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
-   [http://People.csail.mit.edu/jvb/Papers/cnn_tutorial.pdf](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Ühendamise kimbud
**Ühendamise komplekt** rakendab geomeetria convolutional ühenduvuse sarnane, kuid kasutab eelmääratletud funktsioone sõlm lähteväärtuste tuletada sihtkoha sõlme väärtuse. Seega on ühendamise kimbud pole koolitatavad state (kaalu või hälvete). Ühendamise kimbud toeta kõik peale **ühiskasutuse**, **MapCount**ja **kaalu**convolutional atribuudid.  

Tavaliselt summeeritud külgnevatesse ühendamise üksuste tuumade ei kattuks. Kui sammu [d] on võrdne iga dimensiooni KernelShape [d], saadud kiht on traditsiooniline kohaliku ühendamise kiht, mis on tavaliselt convolutional närvivõrgud. Iga sihtkoha sõlme arvutab maksimaalne või selle andmeallika kiht tuum tegevuste keskväärtuse.  

Järgmine näide illustreerib ühendamise komplekt: 

    hidden P1 [5, 12, 12]
      from C1 max pool {
        InputShape  = [ 5, 24, 24];
        KernelShape = [ 1,  2,  2];
        Stride      = [ 1,  2,  2];
      }  

-   Kogumi arity on 3 (kordseid **InputShape**, **KernelShape**ja **jooksusammu**pikkus). 
-   Allpool sõlmed on _5 *24* 24 = 2880_. 
-   See on traditsiooniline kohaliku ühendamise kiht, sest **KernelShape** ja **jooksusammu** on võrdsed. 
-   Sõlmed sihtkoha kihis on _5 *12* 12 = 1440_.  
    
Kihid ühendamise kohta lisateabe saamiseks lugege järgmisi artikleid:  

-   [http://www.cs.Toronto.edu/~Hinton/absps/imagenet.pdf](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (Jaotis 3.4)
-   [http://CS.NYU.edu/~Koray/publis/lecun-iscas-10.pdf](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
-   [http://CS.NYU.edu/~Koray/publis/Jarrett-iccv-09.pdf](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)
    
## <a name="response-normalization-bundles"></a>Vastuse normaliseerimine kimbud
**Vastuse normaliseerimine** on kohalik normaliseerimine värviskeemi, mis lisati esmalt Geoffrey Hintoni, et al, raamatus [ImageNet le enda koos sügav Convolutional närvivõrgud](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf). Vastuse normaliseerimine kasutatakse üldistus neural võrkude. Üks neuron pildistamisel väga kõrge aktiveerimise tasemel kohaliku vastuse normaliseerimine kiht tõkestab ümbritseva neuronite aktiveerimise tase. Selleks on kolm parameetrit (***α***, ***β***, ja ***k***) ja convolutional struktuuri (või naabruskond kujundi) abil. Iga neuron sihtkoha kiht ***y*** vastab neuron ***x*** allpool. Aktiveerimise tase ***y*** on esitatud järgmine valem, kus ***f*** on neuroni aktiveerimise ja ***Nx*** on tuum (või määramine, mis sisaldab neuronite reserveerimise ***x***), määratletud convolutional järgmine struktuur:  

![][1]  

Vastuse normaliseerimine kimbud toeta kõik peale **ühiskasutuse**, **MapCount**ja **kaalu**convolutional atribuudid.  
 
-   Kui tuum sisaldab neuronite sama kaardi kujul ***x***, normaliseerimine värviskeemi on edaspidi **sama kaardi normaliseerimine**. Esimese koordinaatide **InputShape** sisse olema sama kaardi normaliseerimine määratlemiseks väärtuse 1.
-   Kui tuum sisaldab neuronite ruumilise asendis ***x***, kuid neutronid on muud kaardid, nimetatakse normaliseerimine värviskeemi **kaartide normaliseerimine üle**. Seda tüüpi vastuse normaliseerimine rakendab külgmine pidurdamise inspireeritud tüüp reaalarv neuronites leitud loomise konkurentsiga suur aktiveerimise tasemete seas neuron väljundeid arvutada erinevaid kaarte vormile. Kaartide normaliseerimine üle määratlemiseks esimese koordinaatide peab olema täisarv, mis on suurem kui üks ja pole suurem arv kaarte ja ülejäänud koordinaate peab olema väärtuse 1.  

Kuna vastuse normaliseerimine kimbud rakendada eelmääratletud funktsioon sõlm lähteväärtuste sihtkoha sõlm väärtuse määramiseks, on neil pole koolitatavad state (kaalu või hälvete).   

**Teatis**: sõlmed sihtkoha kiht vastavad neuronite, mis on keskne sõlmed tuumade. Näiteks kui KernelShape [d] on paaritu arv, siis _KernelShape [d] / 2_ vastab keskse tuum sõlm. Kui _KernelShape [d]_ on paaris, keskses sõlm on _KernelShape [d] / 2-1_. Seetõttu, kui **täidis**[d] on False, esimene ja Viimane _KernelShape [d] / 2_ sõlmed pole vastavate sõlmed sihtkoha kiht. Selle olukorra vältimiseks määratlemine **täidis** nimega [true, tõene,..., true].  

Lisaks eespool kirjeldatud nelja atribuute, vastuse normaliseerimine kimbud toetavad järgmisi atribuute:  

-   **Alfa**: (nõutav) määratleb ujukoma väärtus, mis vastab ***α*** eelmise valemis. 
-   **Beetajaotuse**: (nõutav) määratleb ujukoma väärtus, mis vastab ***β*** eelmise valemis. 
-   **Offset**: (valikuline) määratleb ujukoma väärtus, mis vastab eelmise valemiga ***k*** . Vaikimisi 1.  

Järgmises näites määratleb vastuse normaliseerimine kogumi, kasutades järgmisi atribuute:  

    hidden RN1 [5, 10, 10]
      from P1 response norm {
        InputShape  = [ 5, 12, 12];
        KernelShape = [ 1,  3,  3];
        Alpha = 0.001;
        Beta = 0.75;
      }  

-   Allpool sisaldab viis kaarte, iga mõõtmega Ofensiva # 12 x 12, 1440 sõlmed summeerimine. 
-   Väärtus **KernelShape** näitab, et see on sama kaardi normaliseerimine kiht, kus on reserveerimise ristkülik 3 x 3. 
-   **Polster** vaikeväärtus on False, seega kiht sihtkoht on ainult 10 sõlmed iga dimensiooni. Ühe sõlme kaasata sihtkoha kiht, mis vastab iga sõlme allpool, lisage täidise = [true; true; true]; ja RN1 suuruse muutmine [5, 12, 12].  

## <a name="share-declaration"></a>Deklaratsioon ühiskasutusse andmine 
NET # soovi toetab mitme kimbud ühiskasutusega kaalu määratlemine. Mis tahes kahe kimbud kaalu saab jagada, kui nende struktuurid on samad. Järgmise süntaksi määratleb kimbud ühiskasutusega kaalu:  

    share-declaration:
        share    {    layer-list    }
        share    {    bundle-list    }
       share    {    bias-list    }
    
    layer-list:
        layer-name    ,    layer-name
        layer-list    ,    layer-name
    
    bundle-list:
       bundle-spec    ,    bundle-spec
        bundle-list    ,    bundle-spec
    
    bundle-spec:
       layer-name    =>     layer-name
    
    bias-list:
        bias-spec    ,    bias-spec
        bias-list    ,    bias-spec
    
    bias-spec:
        1    =>    layer-name
    
    layer-name:
        identifier  

Näiteks saate määrata ühiskasutus-deklaratsioon kiht nimed, mis näitab, et nii kaalu ja hälvete tuleks jagada.  

    Const {
      InputSize = 37;
      HiddenSize = 50;
    }
    input {
      Data1 [InputSize];
      Data2 [InputSize];
    }
    hidden {
      H1 [HiddenSize] from Data1 all;
      H2 [HiddenSize] from Data2 all;
    }
    output Result [2] {
      from H1 all;
      from H2 all;
    }
    share { H1, H2 } // share both weights and biases  

-   Sisestuskeel funktsioonid on kaks võrdse suurusega Sisestuskeel kihid liigendatud. 
-   Peidetud kihid Arvutage Sisestuskeel kihid kõrgema taseme funktsioonid. 
-   Ühiskasutus-deklaratsioon määrab _H1_ ja _H2_ tuleb arvutada kaudu nende vastavate sisendina samal viisil.  
 
Teise võimalusena selle võib määrata kahte eraldi ühiskasutus deklaratsiooni järgmiselt:  

    share { Data1 => H1, Data2 => H2 } // share weights  

<!-- -->

    share { 1 => H1, 1 => H2 } // share biases  

Lühike vorm saate kasutada ainult siis, kui kihid sisaldavad ühe kogumi. Üldiselt jagamine on võimalik, ainult siis, kui oluline on samad, mis tähendab, et neil on sama suur, sama convolutional geomeetria ja jne.  

## <a name="examples-of-net-usage"></a>Net # kasutamise näiteid
Sellest jaotisest leiate mõned näited kasutamist Net # lisada peidetud kihid, määratleda nii, et peidetud kihid teised kihid suhelda ja luua convolutional võrgustikke.   

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Lihtne kohandatud Närvivõrgus määratlemine: "Tere, maailm" näide
Selles lihtsas näites näitab, kuidas luua Närvivõrgus mudelit, mis on ühe peidetud kiht.  

    input Data auto;
    hidden H [200] from Data all;
    output Out [10] sigmoid from H all;  

Näide illustreerib mõned põhilised käsud järgmiselt:  

-   Esirea määratleb Sisestuskeel kiht (nimega _andmed_). Kui kasutada võtmesõna **automaatselt** , sisaldab selle Närvivõrgus funktsiooni kõik veerud automaatselt Sisestuskeel näidetes. 
-   Teisel real loob peidetud kiht. _H_ nimi on määratud peidetud kiht, mis on 200 sõlmed. See on täielikult ühendatud Sisestuskeel kiht.
-   Kolmanda rea määratleb väljundi kiht (nimega _O_), mis sisaldab 10 väljundi sõlmed. Kui soovitud Närvivõrgus kasutatakse liigitamine, on üks väljundi sõlm klassi. Märksõna **kujutavad** näitab, et funktsiooni väljund on rakendatud väljundi kiht.   

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Mitme peidetud kihid määratlemine: arvuti nägemine näide
Järgmises näites näitab, kuidas määrata veidi keerulisem Närvivõrgus mitme kohandatud peidetud kihid.  

    // Define the input layers 
    input Pixels [10, 20];
    input MetaData [7];
    
    // Define the first two hidden layers, using data only from the Pixels input
    hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;
    
    // Define the third hidden layer, which uses as source the hidden layers ByRow and ByCol
    hidden Gather [100] 
    {
      from ByRow all;
      from ByCol all;
    }
    
    // Define the output layer and its sources
    output Result [10]  
    {
      from Gather all;
      from MetaData all;
    }  

See näide illustreerib närvivõrgud määratlus keele mitmesuguseid funktsioone:  

-   Struktuuri on kaks Sisestuskeel, _pikslit_ ja _metaandmed_.
-   _Pikslit_ kiht on allika kiht kahe ühenduse teenusepakettide, sihtkoha kihid, _ByRow_ ja _ByCol_.
-   Kihid _kogumine_ ja _tulemus_ on mitu ühendust kimbud sihtkoha kihid.
-   Väljundi kiht, _tulemus_on sihtkoha kiht kahe ühenduse kimbud; keegi on teise taseme peidetud (kogumine) kui sihtkoha kiht ja teine sihtkoha kiht nimega Sisestuskeel kiht (metaandmed).
-   Peidetud kihid, _ByRow_ ja _ByCol_, määrata filtreeritud ühenduvuse põhiliste avaldiste abil. Täpsemalt sõlme sisse veebisaidil _ByRow_ [x ja y] on ühendatud sõlmed _pikslites_ , mis on võrdne selle sõlm esimese koordinaatide, esimese index koordinaatide x. Samuti sõlme _ByCol veebisaidil [x ja y] on ühendatud sõlmed _pikslites_ , mis on teine registri koordineerimine mõnda soovitud sõlm teise koordinaatide, y.  

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Määratleda convolutional võrgu multiclass liigitamiseks: numbri tuvastus näide
Järgmised võrgu määratlus on loodud arvude ja see näitab mõningaid täiustatud viise kohandamise Närvivõrgus.  

    input Image [29, 29];
    hidden Conv1 [5, 13, 13] from Image convolve 
    {
       InputShape  = [29, 29];
       KernelShape = [ 5,  5];
       Stride      = [ 2,  2];
       MapCount    = 5;
    }
    hidden Conv2 [50, 5, 5]
    from Conv1 convolve 
    {
       InputShape  = [ 5, 13, 13];
       KernelShape = [ 1,  5,  5];
       Stride      = [ 1,  2,  2];
       Sharing     = [false, true, true];
       MapCount    = 10;
    }
    hidden Hid3 [100] from Conv2 all;
    output Digit [10] from Hid3 all;  


-   Struktuuri on üks sisendväärtuste kiht _pilt_.
-   Märksõna **convolve** näitab, et nimega _Conv1_ ja _Conv2_ kihid on convolutional kihid. Kõik need kiht deklareerimise järgneb konvolutsioon atribuutide loendi.
-   Net on kolmanda peidetud kiht, _Hid3_, mis on täielikult ühendatud teise peidetud kiht _Conv2_.
-   Väljundi kiht, _number_, on ühendatud ainult kolmanda peidetud kiht _Hid3_. Märksõna **Kõik** näitab, et väljundi kiht on täielikult ühendatud _Hid3_.
-   Arity, konvolutsioon on kolm (pikkus kordseid **InputShape**, **KernelShape**, **jooksusammu**ja **ühiskasutus**). 
-   Kaalu kohta tuum on _1 + **KernelShape**\[0] * * *KernelShape**\[1]* **KernelShape**\[2] = 1 + 1 *5* 5 = 26. Või 26 * 50 = 1300_.
-   Iga peidetud kiht sõlmed saate arvutatakse järgmiselt:
    -   **NodeCount**\[0] = (5 - 1) / 1 + 1 = 5.
    -   **NodeCount**\[1] = (13-5) / 2 + 1 = 5. 
    -   **NodeCount**\[2] = (13-5) / 2 + 1 = 5. 
-   Sõlmed koguarvu saab arvutada, kasutades deklareeritud dimensionaalsus kiht, [50, 5, 5] järgmiselt: _ **MapCount** * * *NodeCount**\[0]* **NodeCount**\[1] * * *NodeCount**\[2] = 10* 5 *5* 5_
-   Kuna **ühiskasutus**[d] on False ainult _d == 0_, tuumad on _ **MapCount** * * *NodeCount**\[0] = 10* 5 = 50_. 


## <a name="acknowledgements"></a>Kinnitused

Net # keele kohandamise närvivõrgud arhitektuur on välja töötanud Microsofti Shon Katzenberger (arhitekt, seadme õ) ja Alexey Kamenev (Tammsalu, Microsoft Researchi). Seda kasutatakse ettevõttesiseselt Õppekeskuse projekte ja ulatuvad pilt tuvastamise teksti analytics rakenduste arvutisse. Lisateabe saamiseks lugege teemat [Neural võrke Azure'i ml - Net # tutvustus](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)


[1]:./media/machine-learning-azure-ml-netsharp-reference-guide/formula_large.gif
 
