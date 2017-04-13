<properties 
    pageTitle="Seadme õ mudeli jõudluse hindamine | Microsoft Azure'i" 
    description="Selgitab, kuidas hinnata mudeli jõudluse Azure seadme õ." 
    services="machine-learning"
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="bradsev;garye" />


# <a name="how-to-evaluate-model-performance-in-azure-machine-learning"></a>Kuidas hinnata mudeli jõudluse Azure seadme õpetused

See teema näitab, kuidas hindab mudeli Azure seadme õ Studios ja annab lühiülevaate saadaval mõõdikute selle ülesande jaoks. Kolm kuuluva õ tavastsenaariumid on esitatud: 

* regressioonisirge
* binaarsed liigitamine 
* multiclass liigitamine

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Väärtustamisest jõudlust mudeli on üks core andmete teadus protsessi etappe. See näitab, kuidas eduka hinded (prognoose) andmekomplekt koolitatud mudel. 

Azure'i arvuti toetab mudeli hindamise kaks selle learning moodulid põhi seadme kaudu: [Hinnata mudeli] [ evaluate-model] ja [Mudeli rist kinnitage][cross-validate-model]. Need moodulid võimaldavad näha, kuidas mudelisse sooritab mõõdikute, mis on tavaliselt kasutatakse masina õppimine ja statistika arvu.

##<a name="evaluation-vs-cross-validation"></a>Tutvumist võimaldava vs Cross valideerimine##
Hindamine ja rist valideerimine on standardne viisid mudelisse mõõta. Mõlemad luua hindamise mõõdikud, mida saate kontrollida või võrreldes teiste mudelite võrdlus.

[Hinnata mudeli] [ evaluate-model] eeldab, et poolitusjoonega andmekomplekti sisendit (või juhul, mida soovite võrrelda 2 eri mudelite 2). See tähendab, et [Koolitada mudeli] abil mudelisse nägemiseks peate[ train-model] mooduli ja teete prognoose mõne andmehulga [Keskmine mudeli] abil[ score-model] mooduli, enne kui saate tulemusi hinnata. Väärtustamise on põhjal poolitusjoonega siltide/tõenäosus koos true sildid, mis kõik on väljundi [Keskmine mudeli] [ score-model] mooduli.

Teise võimalusena saate rist valideerimine arvu (10 voldid) toimingute rongi Keskmine hinnata tegemiseks automaatselt sisendandmete mitmeid eri. Sisendandmete on tükeldatud 10 osa, kus on üks ette testimiseks, ja muud 9 koolitus. Seda protsessi korratakse 10 korda ja hindamise mõõdikute on keskmiselt. See aitab kindlaks teha, kui hästi mudeli oleks üldistada uue andmekomplektide abil. [Rist valideerimiseks mudeli] [ cross-validate-model] mooduli võtab mudelit, mis treenimata ja mõned siltidega andmekomplekti ja väljundid hindamise tulemused iga 10 voldid, lisaks keskmised tulemused.

Järgmistes jaotistes koostada lihtsaid regressiooni ja liigitamine mudelite ja hinnata oma tegevust, kasutades nii [Hinnata mudeli] [ evaluate-model] ja [Mudeli rist kinnitage] [ cross-validate-model] moodulid.

##<a name="evaluating-a-regression-model"></a>Hindamisel regressiooni mudel##
Oletame, et soovime prognoosida auto hind väljendab mõne funktsiooni, nt mõõtmed, Hobujõud, mootori andmed ja jne. See on tüüpilised regressiooni probleem, kus target muutuja (*Hind*) on pidev arvulise väärtuse. Me ei sobi lihtne lineaarregressioon mudelit, mis on antud funktsiooni väärtuste auto, saate prognoosida, et auto hind. See regressiooni mudel saab kasutada sama andmekomplekti me väljaõppe klõpsake Keskmine. Pärast kõigi soovitud auto prognoositud hinnad on meil saate hindab mudeli vaadates palju prognoose erineda hinnad keskmiselt. Kujutamiseks, kasutame *auto hind andmete (Raw) andmekomplekti* saadaval **Salvestatud andmekomplektide** jaotises Azure seadme õ Studios.
 
###<a name="creating-the-experiment"></a>Loomise katse###
Tööruumi Azure seadme õ Studios järgmised moodulid lisamiseks tehke järgmist.

- Auto andmed (Raw)
- [Lineaarregressioon][linear-regression]
- [Rongi mudel][train-model]
- [Keskmine mudel][score-model]
- [Mudeli hindamine][evaluate-model]


Ühenduse pordid, nagu allpool näidatud joonisel 1 ja määrake silt veeru [Rongi mudeli] [ train-model] mooduli *Hind*.
 
![Hindamisel regressiooni mudel](media/machine-learning-evaluate-model-performance/1.png)

Joonis 1. Hindamisel regressiooni mudel.

###<a name="inspecting-the-evaluation-results"></a>Kontrollimise tulemuste hindamine###
Pärast töötab katse, võite klõpsata väljundi Port [Hinnata mudeli] [ evaluate-model] mooduli ja valige *Visualiseeri* hindamise tulemite kuvamiseks. Hindamise mõõdikud regressiooni mudelid saadaval on: *Tähendab absoluutne tõrge*, *Absoluutne tõrge tähendab Root*, *Suhteline absoluutne viga*, *Suhteline ruudu tõrge*ja *Ruudu, määramine*.

Termini "tõrge" siin tähistab vahe prognoositud väärtus ja väärtus true. Absoluutväärtus või ruudu selle vahe on tavaliselt arvutatud jäädvustada tõrge kokku ulatus üle kõik aknad, nagu true ja prognoositud väärtuse vahe võib olla mõnel juhul negatiivne. Tõrge mõõdikute mõõta sõnastikupõhise regressiooni mudeli osas oma prognoose kaudu tõeste väärtuste keskmise standardhälve. Lower veaväärtusi tähendab mudeli on täpsem prognoose teha. Mõne Üldine tõrge meetermõõdustik 0 tähendab mudeli sobib andmed täiesti.

Määramine, mis on ka R kompleksarvu ruudu, on ka standardne viis mõõte, kuidas mudel sobib andmed. Seda võidakse tõlgendada variatsioonide selgitada mudeli osa. Suurema osa on parem sel juhul, kui 1 näitab täiuslik sobib.
 
![Lineaarse regressioonisirge hindamise mõõdikud](media/machine-learning-evaluate-model-performance/2.png)

Joonis 2. Lineaarse regressioonisirge hindamise mõõdikute.

###<a name="using-cross-validation"></a>Rist valideerimise abil###
Nagu varem mainitud, saate teha korduvat koolitus, saades ja automaatselt kasutades [Rist valideerimiseks mudeli] [ cross-validate-model] mooduli. Sel juhul peate on andmekomplekt ja treenimata mudelit, mis [Rist valideerimiseks mudeli] [ cross-validate-model] mooduli (vt alltoodud joonist). Teate, et peate määrama veeru silt *Hind* [Rist valideerimiseks mudeli] [ cross-validate-model] mooduli atribuudid.

![Rist regressiooni mudeli kontrollimine](media/machine-learning-evaluate-model-performance/3.png)

Joonis 3. Rist-regressiooni mudeli kontrollimine.

Pärast töötab katse, saab kontrollida hindamise tulemused, klõpsates õige väljund porti [Rist valideerimiseks mudeli] [ cross-validate-model] mooduli. Üksikasjalik vaade mõõdikud annab iga iteratsiooni (korda) ja keskmised tulemused iga mõõdikute (joonis 4).
 
![Rist – valideerimise tulemused regressiooni mudel](media/machine-learning-evaluate-model-performance/4.png)

Joonis 4. Rist – valideerimise tulemused regressiooni mudel.

##<a name="evaluating-a-binary-classification-model"></a>Hindamisel mudeli kahendarvu liigitamine##
Binaarsed liigitamine stsenaariumi target muutuja on vaid kahe võimalike tulemuste, näiteks: {0; 1} või {false, true}, {negatiivne, positiivne}. Oletagem, et teile andmekomplekti täiskasvanud töötajate mõned demograafiliste ja töö muutujate ja teil palutakse omast, kahendarvu muutuja väärtusi prognoosida {"< = 50K", "> 50K"}. Teisisõnu, negatiivne klassi tähistab töötajad, kes väiksem või võrdne 50K aastas ja positiivne klassi kõigi teiste töötajatega. Nagu regressiooni puhul, saame koolitada mudeli, Keskmine mõned andmed ja tulemusi hinnata. Peamine erinevus siin on valik Azure seadme õ arvutab mõõdikute ja väljundid. Illustreerimiseks tulu taseme ennustamine stsenaarium, kasutame [täiskasvanud](http://archive.ics.uci.edu/ml/datasets/Adult) andmekomplekti Azure seadme õ katse luua ja hindab kaks klassi logistilise regressiooni mudeli levinumaid kahendarvu klassifitseerijale.

###<a name="creating-the-experiment"></a>Loomise katse###
Tööruumi Azure seadme õ Studios järgmised moodulid lisamiseks tehke järgmist.

- Täiskasvanud rahvaloendus tulu kahendarvu liigitamine andmekomplekti
- [Kaks klassi logistilise regressiooni][two-class-logistic-regression]
- [Rongi mudel][train-model]
- [Keskmine mudel][score-model]
- [Mudeli hindamine][evaluate-model]

Ühenduse pordid, nagu allpool näidatud joonisel 5 ja määrake silt veeru [Rongi mudeli] [ train-model] mooduli *tulu*.

![Hindamisel mudeli kahendarvu liigitamine](media/machine-learning-evaluate-model-performance/5.png)

Joonis 5. Hindamisel kahendarvu liigitamine mudel.

###<a name="inspecting-the-evaluation-results"></a>Kontrollimise tulemuste hindamine###
Pärast töötab katse, võite klõpsata väljundi Port [Hinnata mudeli] [ evaluate-model] mooduli ja valige *Visualiseeri* hindamise tulemusi (joonis 7). Hindamise mõõdikud kahendarvu liigitamine mudelite jaoks saadaval on: *täpsuse*, *täpsuse*, *tagasi kutsuda*, *F1 Keskmine*ja *AUC*. Lisaks mooduli väljundid segadust maatriksi true positiivsed, false negatiivsed, vale-positiivsed ja true negatiivsed ning *ROC*, *Arvutustäpsuse/tagasivõtmise*ja *tõstke* kõverjoonte arvu.

Täpsus on lihtsalt osa õigesti salastatud eksemplarid. See on tavaliselt esimene mõõdiku vaatate on klassifitseerijale võrdlemisel. Kui test andmed on tasakaalustamata (kus enamik linnanimede kuuluvad ühte klassi) või olete rohkem huvitatud täitmisel kas üks klassi täpsuse ei tõesti jäädvustada on klassifitseerijale tõhusust. Tulu taseme liigitamine stsenaariumi endale teil on mõned andmed, kus 99% linnanimede tähistada inimesed, kellel on väiksem kui või võrdne 50K aastas teenida katsetamine. On võimalik saavutada 0,99 täpsuse prognoosida ainekursuse "< = 50K" kõik eksemplarid. Sellisel juhul kuvatakse klassifitseerijale üldine head tööd teha, kuid tegelikkuses nurjub liigitada mis tahes suure sissetulekuga isikutele (1%) õigesti.

Seetõttu on kasulik, jäädvustada täpsemale aspektid hindamise täiendavad mõõdikud arvutada. Enne dokumendi selliste mõõdikute üksikasju, on oluline mõista kahendarvu liigitamine hindamise segadust maatriks. Klassi siltide koolituse määramine saab võtta ainult 2 võimalikud väärtused, mida me tavaliselt viidata nii positiivne või negatiivne. Positiivsete ja negatiivsete eksemplarid, mis on klassifitseerijale prognoosib õigesti nimetatakse true positiivsete (TP) ja true negatiivsete (TN), vastavalt. Samuti valesti salastatud eksemplari nimetatakse false positiivsete (FP) ja väära negatiivsete (FN). Segadust maatriks on lihtsalt tabeli, mis näitab iga 4 kategooria alla eksemplaride arv. Azure'i masina õ otsustab automaatselt, mis on kaks tunde andmekomplekti on positiivne klassi. Kui klassi sildid on kahendväärtus või täisarvud, siis "true" või "1" siltidega eksemplarid on määratud positiivne klassi. Kui sildid on stringid, nagu tulu andmekomplekti puhul siltide sorditakse tähestikulises järjestuses ja esimese taseme valitakse negatiivne klassi teise taseme on positiivne klassi.

![Binaarsed liigitamine segadust maatriks](media/machine-learning-evaluate-model-performance/6a.png)

Joonis 6. Binaarsed liigitamine segadust maatriks.

Naasmine tulu liigitamine probleemi oleks soovime mitu hindamise küsimusi, millele aidake meil mõista täitmiseks kasutatud klassifitseerijale. Väga loomulikus küsimus: "välja isikud, kellele mudeli ennustatud teenida > 50 K (TP + võimalusi), kui palju on liigitatud õigesti (TP)?" Saate selle küsimusele vaadates **arvutustäpsuse** mudelit, mis on osa positiivsed, mis on õigesti liigitatud: TP/(TP+FP). Omaette levinud küsimus on "välja kõik kõrge teenida tulu töötajad > 50 k (TP + FN), kui palju ei klassifitseerijale liigitada õigesti (TP)". See on tegelikult selle **tagasivõtmine**või true määra: TP/(TP+FN) soovitud salastaja. Märkate, et oleks selge kompromissidest täpsuse ja tagasivõtmise. Näiteks antud suhteliselt tasakaalustatud andmekomplekti, oleks klassifitseerijale, mis prognoosib enamasti näiteid, kõrge tagasivõtmise, kuid üsna madal täpsus paljude negatiivne eksemplari soovite valesti klassifitseerinud tulemuseks false positiivsed suure hulga. Diagrammi, ja kuidas neid kahte mõõdikute muutub kuvamiseks võite klõpsata ' ARVUTUSTÄPSUSE/tagasivõtmise' kõvera hindamise tulem väljundi lehel (joonis 7 osa vasakus ülanurgas).

![Binaarsed liigitamine hindamise tulemused](media/machine-learning-evaluate-model-performance/7.png) joonis 7. Binaarsed liigitamine hindamise tulemused.

Teise seotud meetermõõdustik, mida kasutatakse sageli on **F1 Keskmine**, mis võtab tagasivõtmise arvesse nii täpsust. See on need 2 mõõdikute harmoonilise keskmise ja on arvutatud sellisena: F1 = 2 (täpsus tagasivõtmise x) / (arvutustäpsuse + tagasivõtmise). F1 Keskmine on hea võimalus summarize väärtustamise üks number, kuid see on alati hea tava nii täpsuse ja tagasivõtmise kokku viia paremini mõistmiseks on klassifitseerijale käitumise vaadata.

Lisaks saate ühte kontrolli vs tulemuste määra **Vastuvõtja opsüsteem omadus (ROC)** kõver ja vastava **Ala all the kõvera (AUC)** väärtuse true määra. Lähemal selle kõvera on vasakus ülanurgas, parem on klassifitseerijale jõudlust on (mis on maksimeerimine true määra ajal minimeerimine tulemuste määra). Kõverad, mis on diagonaalääris graafiku, tulemus liigitused, et teha prognoose lähedale juhusliku aim on tavaliselt lähedal.

###<a name="using-cross-validation"></a>Rist valideerimise abil###
Nagu näiteks regressiooni, saame teha rist valideerimine korduvalt koolitada, Keskmine ja muu kohta andmed automaatselt hinnata. Samuti kasutame [Rist valideerimiseks mudeli] [ cross-validate-model] mooduli, treenimata logistilise regressiooni mudelit, mis ja Andmekomplekt. Veeru silt peab olema seatud *tulu* [Rist valideerimiseks mudeli] [ cross-validate-model] mooduli atribuudid. Pärast opsüsteemi katse ja klõpsake õige väljund Port [Cross valideerimiseks mudeli] [ cross-validate-model] mooduli, me näeme kahendarvu liigitamine argumendil väärtused iga volditud Lisaks keskväärtus ja standardhälve iga. 
 
![Rist kahendarvu liigitamine mudeli kontrollimine](media/machine-learning-evaluate-model-performance/8.png)

8-kujund. Rist-kahendarvu liigitamine mudeli kontrollimine.

![Binaarsed klassifitseerijale rist – valideerimise tulemused](media/machine-learning-evaluate-model-performance/9.png)

Joonis 9. Binaarsed klassifitseerijale rist – valideerimise tulemused.

##<a name="evaluating-a-multiclass-classification-model"></a>Hindamisel mudeli Multiclass liigitamine##
See katse kasutame populaarsed [Iris](http://archive.ics.uci.edu/ml/datasets/Iris "Iris") andmekomplekti, mis sisaldab 3 erinevat tüüpi (tunnid) iris taimest eksemplarid. 4 funktsiooni väärtust (õietupp laiuse ja kroonleht laiuse) iga eksemplari pole. Eelmise katsete väljaõppe ja testitud mudelid, kasutades sama kogumid. Siin kasutame [Tükeldatud andmetega] [ split] mooduli loomine 2 kohta andmed, koolitada esimese, Keskmine ja teine hinnata. Andmekomplekti Iris [UCI masina õ hoidla](http://archive.ics.uci.edu/ml/index.html)avalikult kättesaadav ja saab alla soovitud [Andmete importimine] [ import-data] mooduli.

###<a name="creating-the-experiment"></a>Loomise katse###
Tööruumi Azure seadme õ Studios järgmised moodulid lisamiseks tehke järgmist.

- [Andmete importimine][import-data]
- [Multiclass otsust mets][multiclass-decision-forest]
- [Andmete tükeldamine][split]
- [Rongi mudel][train-model]
- [Keskmine mudel][score-model]
- [Mudeli hindamine][evaluate-model]

Ühendage pordid, nagu allpool näidatud joonisel 10.

Määrake silt veeru indeks [Rongi mudeli] [ train-model] mooduli 5. Andmekomplekti Päisereata, kuid me ei tea, et klassi sildid on viienda veeru.

[Andmete importimine] nuppu[ import-data] mooduli ja Sea *andmeallika* atribuut *HTTP kaudu Web URL-i*ja *URL-i* http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data.

Määrata eksemplari kasutatakse koolitus [Tükeldatud andmetega] murdosa[ split] mooduli (nt 0,7).
 
![Hindamisel Multiclass klassifitseerijale](media/machine-learning-evaluate-model-performance/10.png)

Joonis 10. Hindamisel Multiclass klassifitseerijale

###<a name="inspecting-the-evaluation-results"></a>Kontrollimise tulemuste hindamine###
Käivita katse ja klõpsake nuppu väljund Port [Hinnata]mudeli[evaluate-model]. Hindamise tulemused on esitatud kujul segadust maatriks, sel juhul. Maatriks kuvatakse tegelike vs kõik 3 Tunnid prognoositud eksemplari.
 
![Multiclass liigitamine hindamise tulemused](media/machine-learning-evaluate-model-performance/11.png)

Joonis 11. Multiclass liigitamine hindamise tulemused.

###<a name="using-cross-validation"></a>Rist valideerimise abil###
Nagu varem mainitud, saate teha korduvat koolitus, saades ja automaatselt kasutades [Rist valideerimiseks mudeli] [ cross-validate-model] mooduli. Peate andmekomplekt, mudelit, mis treenimata ja [Mudeli rist kinnitage] [ cross-validate-model] mooduli (vt alltoodud joonist). Uuesti peate määrama sildi veeru [Rist valideerimiseks mudeli] [ cross-validate-model] mooduli (veeru indeks 5, praegusel juhul). Pärast töötab katse ja klõpsake right väljund port [Rist valideerimiseks mudeli][cross-validate-model], saab kontrollida argumendil väärtused iga volditud samuti keskväärtus ja standardhälve. Siin kuvatakse mõõdikud on sarnane arutada kahendarvu liigitamine juhul need. Pange tähele, et multiclass liigitamine, positiivsete ja negatiivsete true ja false positiivsete ja negatiivsete on tehtud lugedes klassi alusel, nagu on üldiselt positiivne või negatiivne klassi. Näiteks kui arvutuste täpsuse või tagasivõtmise 'Iris setosa' klassi, eeldatakse, et see on positiivne klassi ja kõik teised negatiivne.
 
![Rist Multiclass liigitamine mudeli kontrollimine](media/machine-learning-evaluate-model-performance/12.png)

Joonis 12. Rist-Multiclass liigitamine mudeli kontrollimine.


![Rist – valideerimise tulemused mudeli Multiclass liigitamine](media/machine-learning-evaluate-model-performance/13.png)

Joonis 13. Rist – valideerimise tulemused Multiclass liigitamine mudel.


<!-- Module References -->
[cross-validate-model]: https://msdn.microsoft.com/library/azure/75fb875d-6b86-4d46-8bcc-74261ade5826/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[multiclass-decision-forest]: https://msdn.microsoft.com/library/azure/5e70108d-2e44-45d9-86e8-94f37c68fe86/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-logistic-regression]: https://msdn.microsoft.com/library/azure/b0fd7660-eeed-43c5-9487-20d9cc79ed5d/
 
