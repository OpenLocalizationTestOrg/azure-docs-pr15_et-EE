<properties 
    pageTitle="Käivitada Python masina õ skriptide | Microsoft Azure'i" 
    description="Liigenduste kujunduse põhimõtted tugi Python skriptide Azure seadme õppimine ja basic kasutamise stsenaariumid, võimaluste ja piirangud." 
    keywords="Python masina Õppekeskuse pandas, python pandas, python skriptid, käivitada python skriptide"
    services="machine-learning"
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Azure'i masina Õppekeskuse Studios Python masina õ skriptide käivitada

Selles teemas kirjeldatakse praeguse tugi Python skriptide Azure seadme õppe põhimõtetega kujundus. Peamised võimaluste on ka liigendatud, sh importimise kood, eksportimise visualiseeringud toetus ja lõpuks mõned piirangud ja poolelioleva töö arutatakse.

[Python](https://www.python.org/) hädavajalik palju andmeteadlaste rinnas tööriista. See on:

-  elegantne ja lühike süntaks 
-  mitu platvormi tugi, 
-  suur kogumine võimsaid teekidest, ja 
-  Vanemad arengu tööriistad. 

Python on kasutatakse tavaliselt kasutatakse masina õ modelleerimine töövoo kõigi etappide, andmete põhjal neelata ja funktsioon ehituse ja mudeli koolitus, ja valideerimise ja juurutamise mudelite töötlemine. 

Azure'i masina Õppekeskuse Studio toetab krae Python skriptide masina Õppekeskuse katse ja ka sujuvalt avaldamiseks neid scalable operationalized veebiteenuste Microsoft Azure eri osadeks.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Kujundage Python skriptide masina õppe põhimõtted
Esmane liidest Python Azure seadme õ Studios on [Käivitada Python skripti] kaudu[ execute-python-script] mooduli joonisel 1.

![image1](./media/machine-learning-execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![image2](./media/machine-learning-execute-python-scripts/embedded-machine-learning-python-script.png)

Joonis 1. **Käivitada Python skript** moodul.

[Käivitada Python skript] [ execute-python-script] mooduli aktsepteerib kuni kolm sisendina ja annab tulemiks kahe väljundid (allpool), nagu selle R analoog, [R skripti käivitada] [ execute-r-script] mooduli. Käivitatakse Python kood sisestatakse väljale parameeter nimega spetsiaalselt nimega sisendpunkti funktsiooni nimetatakse `azureml_main`. Siin on olulisemad põhimõtted rakendada selle mooduli.

1.  *Peab olema sellistes Python kasutajad.* Enamik Python kasutajate tegur oma koodi funktsioonide sees moodulid, kui nii palju käivitatava laused ülataseme moodulis on üsna harva. Selle tulemusena võtab skripti välja spetsiaalselt nimega Python funktsiooni asemel ainult laused jada. Funktsiooni avatud objektid on standardne Python teegi nagu [Pandas](http://pandas.pydata.org/) andmete paneelide ja [NumPy](http://www.numpy.org/) massiivi.
2.  *Peab olema vahel kohalik kvaliteetse ja Pilveteenusest täitmised.* On kasutatud käivitada Python koodi kirjutamata põhineb [Anaconda](https://store.continuum.io/cshop/anaconda/) 2.1, levinumate platvormidel teaduslik Python jaotuse. See on kõige levinum Python paketid 200 lähedal. Seetõttu andmeteadlaste silumine ja saada oma koodi Azure seadme õ ühilduv Anaconda isikuid. Seejärel käivitage see Azure seadme õ katse osana kõrge confidence olemasoleva arengu keskkonnas, nt [IPython](http://ipython.org/) märkmiku või [Python Tools for Visual Studio](http://aka.ms/ptvs) abil. Lisaks on `azureml_main` sisendpunkti on vanilje Python funktsioon ja saate autoriks ilma Azure seadme õ teatud koodi või SDK installitud.
3.  *Peab olema sujuvalt composable muude Azure seadme õ mooduleid.* [Käivitada Python skript] [ execute-python-script] mooduli aktsepteerib sisendi ja väljundi, nagu standard Azure seadme õ andmekomplektide. Aluseks oleva raames läbipaistev ja tõhusalt sillad Azure seadme õppimine ja Python runtimes, (toetab funktsioone nagu puuduvate väärtuste). Python seetõttu saab koos olemasoleva Azure seadme õ töövood, sealhulgas need, mis R ja SQLite sisse helistada. Üks seetõttu saab kavandada töövoogude mis:
  * andmete eeltöötlus ja puhastamiseks, Python ja Pandas kasutamine 
  * kanali andmete SQL-i transformatsioon, liitumist mitme andmekomplektide vormi funktsioone, 
  * koolitada olulisel saidikogumi algoritmide kasutamine Azure seadme õ, mudelite ja 
  * hindab ja pärast töötlemine tulemused, kasutades R.


## <a name="basic-usage-scenarios-in-machine-learning-for-python-scripts"></a>Seadme õ Python skripte põhialused stsenaariumid
Selles jaotises me küsitluse mõne lihtsa kasutab [Käivitada Python skript] [ execute-python-script] mooduli.
Nagu varem mainitud, Python moodulisse sisendeid on esitatud Pandas andmete raame. Lisateavet Python Pandas ja kuidas seda saab kasutada andmete töödelda ja tõhusalt leiate *Python andmeanalüüsi* (O'Reilly 2012) W. McKinney. Funktsioon peab andma ühe Pandas andmete raami pakitud Python [sequence](https://docs.python.org/2/c-api/sequence.html) , nt korteeži, loendi või NumPy massiivi sees. Esimese väljundi pordi mooduli siis tagastatakse järjestusel esimene element. Selle kava on näidatud joonisel 2.

![image3](./media/machine-learning-execute-python-scripts/map-of-python-script-inputs-outputs.png)

Joonis 2. Parameetrite abil sisendi pordid ja väljundi pordi tagastusväärtus kaardistamine.

Üksikasjalikumad semantika Sisestuskeel pordid vastendamine ja parameetrid on `azureml_main` funktsioon on esitatud tabel 1:

![image1T](./media/machine-learning-execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Tabel 1. Funktsioon parameetrite abil sisendi pordid vastendust.

Vastenduse Sisestuskeel pordid ja funktsiooni parameetrid vahel on positsiooniga seotud. Funktsiooni esimene parameeter on vastendatud esimese ühendatud Sisestuskeel pordi ja teine sisend (kui see on ühendatud) on vastendatud funktsiooni teine parameeter.

## <a name="translation-of-input-and-output-types"></a>Tõlke sisend- ja tüübid
Nagu varem selgitatud, teisendatakse Sisestuskeel andmekogumite Azure seadme õ Pandas raamid ja väljundi andmete paneelid teisendatakse tagasi Azure seadme õ andmekomplektide andmed. Järgmised teisendamine käib.

1.  Stringi- ja arvväärtused veerud, teisendatakse need-on ja Pandas "NA" väärtused teisendatakse puuduvate väärtuste Andmekomplekt. Sama tulemus, mis juhtub teel tagasi (Pandas NA väärtused teisendatakse puuduvate väärtuste Azure seadme õ).
2.  Registri vektorite Pandas ei toeta Azure seadme õ. Kõik sisendandmete raamid funktsiooni Python on alati 64-bitine numbriline indeks vahemikus 0 kuni 1 miinus ridade arv. 
3.  Azure'i masina õ andmekomplektide ei tohi olla dubleeritud veergude nimed ja veergude nimed, mis pole stringide. Kui mõni väljundi andmete paneeli sisaldab mittearvulise veergu, raames helistab `str` klõpsake veergude nimed. Samuti kõik dubleeritud veerunimede automaatselt moonutatud kindlustada nimed on kordumatu. Järelliide (2) on lisatud esimese duplikaat, (3) teise dubleeritud, jne.

## <a name="operationalizing-python-scripts"></a>Standardi Python skriptide
Mis tahes [Käivitada Python skript] [ execute-python-script] moodulid kasutatud hinded katse nimetatakse veebiteenuse avaldamisel. Näiteks joonis 3 näitab hinded katse, mis sisaldab koodi ühe Python avaldist hinnata. 

![image4](./media/machine-learning-execute-python-scripts/figure3a.png)

![image5](./media/machine-learning-execute-python-scripts/python-script-with-python-pandas.png)

Joonis 3. Veebiteenuse jaoks Python avaldise hindamisel.

See katse loodud veebiteenuse võtab sisestada Python avaldis (stringi), mis saadetakse Python tõlgi ja tagastab tabeli, mis sisaldab nii avaldist ja hinnatud tulemi.

## <a name="importing-existing-python-script-modules"></a>Olemasoleva Pythoni skript moodulid importimine
Levinud kasutamine juhul palju andmeteadlaste on lisada olemasoleva Python skriptide Azure seadme õ katsete. Ühendades ja kõik kood kleepimise ühe skripti boksi, [Käivitada Python skript] [ execute-python-script] mooduli aktsepteerib kolmanda Sisestuskeel portide, mille saab ühendada zip-fail, mis sisaldab Pythoni moodulid. Fail on seejärel lahti pakitud täitmise raamistik käitusajal ja sisu lisatakse teegi tee Python Tõlgi. Funktsiooni `azureml_main` funktsioon saate importige need moodulid otse juurde.

Näiteks, kaaluge faili sisaldava lihtsa "Tere, maailm!" funktsiooni Hello.py.

![image6](./media/machine-learning-execute-python-scripts/figure4.png)

Joonis 4. Kasutaja määratletud funktsioon.

Järgmiseks loome faili Hello.zip, mis sisaldab Hello.py:

![image7](./media/machine-learning-execute-python-scripts/figure5.png)

Joonis 5. Kasutaja määratletud Python kood ZIP-faili.

Laadige see nimega andmekomplekt Azure seadme õ Studio sisse. Luua ja käivitada lihtsa katse, mis kasutab Python Hello.zip faili lisades kolmanda Sisestuskeel pordi käivitada Python skripti, nagu on näidatud joonisel.

![image8](./media/machine-learning-execute-python-scripts/figure6a.png)

![image9](./media/machine-learning-execute-python-scripts/figure6b.png)

Joonis 6. Kasutaja määratletud Python kood valimi eksperimenteerida üles laaditud, zip-failina.

Mooduli väljund näitab, et zip-fail on pakkimata ja funktsiooni `print_hello` tõepoolest käivitada.
 
![image10](./media/machine-learning-execute-python-scripts/figure7.png)
 
Joonis 7. Kasutaja määratletud funktsioon kasutab sees [Käivitada Python skript] [ execute-python-script] mooduli.

## <a name="working-with-visualizations"></a>Visualiseeringute töötamine
Krundid loodud MatplotLib, mida saate visualiseeritakse brauseri saate [Käivitada Python skript]tagastatud[execute-python-script]. Kuid krundid ei ole automaatselt ümber piltidele, kui need on R. kasutamisel Nii kasutaja tuleb selgesõnaliselt salvestada mis tahes krundid PNG-failid kui need on tagastatakse tagasi Azure seadme õ. 

Selleks, et luua pilte MatplotLib, peab võistlema järgmist:

* Vaikimisi Qt-põhiste renderdaja "AGG" on kirjutamata minna 
* Looge uus joonis objekt 
* saada telg ja luua sinna kõik krundid 
* Joonis PNG-failina salvestamine 

Järgmine joonis 8, mis loob punkt graafiku maatriksi Pandas scatter_matrix funktsiooni kasutamine on kujutatud seda toimingut.
 
![image1v](./media/machine-learning-execute-python-scripts/figure-v1-8.png)

8-kujund. Piltide salvestamine MatplotLib arvud.



Joonis 9 näitab katse, mis kasutab tagastamiseks kasutada eelnevalt skripti kujundab teise väljundi pordi kaudu.

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9a.png) 
     
![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9b.png) 

Joonis 9. Visualiseerimine krundid loodud Python kood.

On võimalik tagastada mitu arvud, võite need salvestada erinevaid pilte, Azure seadme õ käitusaja jätkab kõik pildid ja ühendab need visualiseerimiseks.


## <a name="advanced-examples"></a>Täiustatud näited
Anaconda keskkond, mis on installitud Azure seadme Õppekeskuse sisaldab levinud pakettide nagu NumPy, SciPy, ja Scikits lugege ja need tõhus saab tüüpilised masina õ teel eri andmete töötlemise toimingute jaoks. Näiteks järgmine katse ja script on näidatud ansambel osalejatele Scikits – saate teada, kuidas arvutada funktsiooni tähtsus hinded andmepaani. Hinded seejärel saab teha järelevalve funktsiooni valik enne edaspidise mõne muu seadme õ mudel.

Funktsiooni Python arvutada tähtsus hinded ja järjestuses vastavalt selle funktsioonid on allpool näidatud:

![image11](./media/machine-learning-execute-python-scripts/figure8.png)

Joonis 10. Funktsioon rank funktsioonide järgi hinded.
 Järgmine katse seejärel arvutab ja tagastab tähtsus hinded funktsioonide "Pima India II" andmekomplekti Azure seadme õppe:

![image12](./media/machine-learning-execute-python-scripts/figure9a.png)
![image13](./media/machine-learning-execute-python-scripts/figure9b.png)    
    
Joonis 11. Proovida Pima India II andmekomplekti rank funktsioone.

## <a name="limitations"></a>Piirangud 
[Käivitada Python skript] [ execute-python-script] praegu on järgmised piirangud:

1.  *Liivakastikoodi täitmise.* Käitusaja Python on praegu liivakastikoodi ja seetõttu ei luba juurdepääs võrku või kohaliku failisüsteemi püsiv viisil. Kõik failid on salvestatud kohalikult, eraldatakse ja kustutatud pärast mooduli lõppemist. Python kood ei pääse juurde enamiku kataloogide see töötab, erandiks on praeguse kataloogi ja selle alamkatalooge arvutis.
2.  *Keerukate arendamise ja silumine toe puudumine.* Python moodul ei toeta praegu IDE funktsioone, näiteks IntelliSense'i ja silumine. Ka mooduli nurjumisel käitusajal täielik Python virnas jälitus on saadaval, kuid tuleb vaadata väljundi Logi mooduli. Praegu soovitame teil tekib ja silumine keskkonnas, nt IPython oma Python skripte ja seejärel importige mooduli kood.
3.  *Ühe andmete paneeli väljund.* Python sisendpunkti on lubatud ainult ühe andmete raami väljundina tagastamiseks. Ei ole võimalik praegu naasta suvalise Python objektide, näiteks koolitatud mudelite otse Azure seadme õ runtime. [R skripti käivitada], näiteks[execute-r-script], mis on sama piirang, on siiski võimalik paljudes juhtudel hapukurk objektidele baitide massiivis ja seejärel tagasi andmete raami selle sees.
4.  *Ei ole võimalik Python installi kohandada*. Praegu on ainus võimalus lisada kohandatud Pythoni moodulid kaudu zip faili süsteem eespool kirjeldatud. Kuigi see on võimalik small moodulid, on suur moodulid (eriti kohalikke dll) või suure hulga moodulid tülikas. 


##<a name="conclusions"></a>Järeldusi
[Käivitada Python skript] [ execute-python-script] moodul võimaldab andmete teadlane lisada kood Python pilve majutatud arvuti õ töövoogude Azure seadme õppida ja sujuvalt käivitama neid veebiteenuse osana. Python script mooduli interoperates loomulikult koos muude Azure seadme õ moodulid ja kasutada mitmesuguseid tööülesandeid eelse töötlemise, funktsioon eraldamine, hindamise ja tulemuste pärast töötlemise: andmete uurimine. Täitmiseks kasutatud taustväärtus runtime põhineb Anaconda hästi testitud ja levinumate Python jaotuse. Järgmine lihtne teile rongis olemasoleva koodi vara pilve.

Me oodata annavad täiendavaid funktsioone [Käivitada Python skript] [ execute-python-script] mooduli, näiteks võimalust koolitada ja kiireks mudelite Python ja lisada parem tugi arendamise ja silumine Azure seadme õ Studio kood.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate teemast [Python Arenduskeskus](/develop/python/).

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
