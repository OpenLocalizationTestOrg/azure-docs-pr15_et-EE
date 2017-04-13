<properties 
    pageTitle="Juurdepääs andmekomplektide masina Õppimine Python kliendi teegiga | Microsoft Azure'i" 
    description="Installige ja kasutage Python kliendi teek juurdepääsuks ja haldamiseks Azure seadme õ andmeid turvaliselt kohaliku Python keskkonnast." 
    services="machine-learning" 
    documentationCenter="python" 
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
    ms.author="huvalo;bradsev" />


#<a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Accessi andmekomplektide Python Azure seadme Õppimine Python kliendi teegi kasutamine 

Microsoft Azure'i masina Õppimine Python kliendi teek eelvaade saate lubada turvaline juurdepääs teie Azure seadme õ andmekomplektide kohaliku Python keskkonnast ja lubab loomist ja haldamist andmekomplektide tööruumis.

Selles teemas antakse juhiseid kohta:

* installige arvutisse Õppimine Python kliendi teek 
* juurdepääs ja andmekomplektide, sealhulgas juhised, kuidas saada luba juurde pääseda kohaliku Python keskkonna Azure seadme õ andmekomplektide
*  vahe andmekomplektide pääseda katsete
*  loetlemine andmekomplektide, juurdepääs metaandmete andmekomplekt sisu lugeda, luua uue andmekomplektide ja värskendada olemasoleva andmekomplektide Python kliendi teek abil

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

 
##<a name="prerequisites"></a>Eeltingimused

Python kliendi teek on testitud järgmiste keskkondades alusel:

 - Windowsi, Maci ja Linuxi
 - Python 2.7, 3.3 ja 3.4

See on sõltuvus kohta järgmised:

 - taotlused
 - Python-dateutil
 - pandas

Soovitame kasutada Python jaotuse nagu ülaltoodud [Anaconda](http://continuum.io/downloads#all) või [võrastiku](https://store.enthought.com/downloads/), kus Python, IPython ja kolme paketi installitud. Kuigi IPython pole tingimata vajalik, on hea keskkonna käsitsemiseks ja visualiseerimine andmete interaktiivseks.


###<a name="installation"></a>Kuidas installida Azure seadme Õppimine Python kliendi teek

Selles teemas kirjeldatud toimingu lõpuleviimiseks, peab olema installitud ka Azure seadme Õppimine Python kliendi teek. See on saadaval [Python paketi register](https://pypi.python.org/pypi/azureml). Installimiseks oma Python keskkonnas, käivitage järgmine käsk: kohaliku Python keskkonna:

    pip install azureml

Teise võimalusena saate alla laadida ja installida [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python)allikatest.

    python setup.py install

Kui teil on git teie arvutisse installitud, saate installida otse git hoidla pip:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


##<a name="datasetAccess"></a>Juurdepääs andmekomplektide Studio Koodilõigud abil

Python kliendi teek pääsete Programmeerimisjuurdepääs oma olemasoleva andmekomplektide käivitatud katsete kaudu.

Studio web liides, saate luua Koodilõigud, mis sisaldavad kogu vajaliku teabe alla laadida ja andmeatribuutide andmekomplektide Pandas DataFrame objektina asukoht teie arvutis.

### <a name="security"></a>Juurdepääs andmetele Turve

Koodilõigud, mis on esitatud Studio kasutamine koos Python kliendi teek sisaldab oma tööruumi id ja luba Turbeloa. Need anda täielikku juurdepääsu oma tööruumi ja peab olema kaitstud, nagu parool.

Turvalisuse huvides on saadaval kasutajatele, kellel on nende rolli määramine **omanikuna** tööruumi ainult koodi koodilõigu funktsioonid. Teie roll kuvatakse Azure seadme õ Studio **lehel jaotises **sätted**** .

![Turvalisus][security]

Kui teie roll **omanikuna**pole määratud, saate kas taotlus olema reinvited omanikuna või paluge teile koodilõigu tööruumi omanik.

Saada luba luba, tehke ühte järgmistest.



- Küsi omaniku luba. Omanikud pääsete juurde oma autoriseerimine sõned oma tööruumi Studios lehel sätted. Valige vasakul paanil **sätted** ja klõpsake nuppu **Luba SÕNED** esmaseid ja teiseseid märkide kuvamiseks.  Kuigi esmane või teisene autoriseerimine sõned saab kasutada koodilõigu, on soovitatav, et omanikud ühiskasutusse anda ainult sõned teisene autoriseerimine.

![](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

- Paluge ülendatakse rolli omanik.  Selleks, praegune omanik tööruumi tuleb esmalt eemaldamine tööruumi ja seejärel uuesti kutsuda saate selle omaniku.

Kui arendajad saanud tööruumi id ja luba Turbeloa, nad suudavad tööruumiga, sõltumata nende rolli koodilõigu abil.

Luba sõned hallatakse **Autoriseerimine SÕNED** lehel jaotises **sätted**. Te saate need taastada, kuid see toiming tühistab eelmise Luba juurdepääs.

### <a name="accessingDatasets"></a>Accessi andmekomplektide kohaliku Python rakendusest

1. Klõpsake arvuti õ Studio, valige vasakul asuvalt navigeerimisribalt **ANDMEKOMPLEKTIDE** .

2. Valige andmekomplekti soovite juurde pääseda. Mis tahes kogumid saate valida loendist **Minu ANDMEKOMPLEKTIDE** või **näidised** loendist.

3. Klõpsake selle alt tööriistariba **luua andmete pääsukood**. Kui andmed on vastuolus Python kliendi teek vormingus, on see nupp keelatud.

    ![Andmekomplektide][datasets]

4. Valige koodilõigu aken, mis kuvatakse ja selle kopeerimine lõikelauale.

    ![Pääsukoodi][dataset-access-code]

5. Kleepige kood oma kohaliku Python rakenduse märkmik.

    ![Märkmiku][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Accessi vahe andmekomplektide masina õ katsete kaudu

Pärast arvuti õ Studios käitatakse katse, on võimalik juurde pääseda vahe andmekomplektide väljundi sõlmed mooduleid. Vahe andmekomplektide on andmeid, mis on loodud ja kasutatud vahe juhised käitamisel mudeli tööriista.

Vahe andmekomplektide pääseb kui andmevorming ühildub Python kliendi teek.

Toetatud failivormingud (konstandid need on selle `azureml.DataTypeIds` klassi):

 - Lihttekst
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader

Saate määrata libistamisel üle mooduli väljundi sõlm vorming. See kuvatakse koos sõlme nimi, kuvatakse kohtspikrina.

Mõned moodulid, nt [tükeldatud] [ split] moodulis nimega vormingus väljund `Dataset`, mis ei toeta Python kliendi teek.

![Andmekomplekti vorming][dataset-format]

Peate kasutama teisendamine mooduli, näiteks [CSV-vormingus failide teisendamine][convert-to-csv], et saada väljund toetatud vormingusse.

![GenericCSV vormindamine][csv-format]

Järgmised toimingud Kuva näide, mis loob katse, see käivitub ja vahe andmekomplekti pääseb juurde.

1. Saate luua uue katse.

2. Saate lisada mõne **täiskasvanud rahvaloendus tulu kahendarvu liigitamine andmekomplekti** mooduli.

3. Lisa [tükeldatud] [ split] moodulisse ja ühendage andmekomplekti mooduli väljundi panuse.

4. Lisada [CSV-vormingus failide teisendamine] [ convert-to-csv] mooduli ja ühendage panuse ühte [tükeldatud] [ split] mooduli väljundid.

5. Salvestamine katse, käivitage see ja oodake, kuni see töötab lõpuleviimiseks.

6. Klõpsake selle väljundi sõlme kohta [teisendamine CSV] [ convert-to-csv] mooduli.

7. Kontekstimenüü kuvamisel valige **luua andmete pääsukood**.

    ![Kontekstimenüü][experiment]

8. Koodilõigu valige ja kopeerige see lõikelauale Kuvatavas aknas.

    ![Pääsukoodi][intermediate-dataset-access-code]

9. Kleepige kood oma märkmiku.

    ![Märkmiku][ipython-intermediate-dataset]

10. Saate visualiseerida andmeid matplotlib abil. See kuvab histogrammi vanus veeru:

    ![Histogramm][ipython-histogram]


##<a name="clientApis"></a>Kasutage juurde pääseda, lugeda, luua ja hallata andmekomplektide masina Õppimine Python kliendi teek

### <a name="workspace"></a>Tööruumi

Tööruum on sisendpunkti Python kliendi teegi. Sisestage soovitud `Workspace` klassi oma tööruumi id ja luba sümboolne eksemplari loomine:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Andmekomplektide loetlemine

Loendada kõik andmekomplektide antud tööruumis:

    for ds in ws.datasets:
        print(ds.name)

Loendada ainult kasutaja loodud andmekomplektide:

    for ds in ws.user_datasets:
        print(ds.name)

Loendada ainult andmekomplektide näide:

    for ds in ws.example_datasets:
        print(ds.name)

Pääsete juurde andmekomplekt nimi (mis pole tõstutundlik).

    ds = ws.datasets['my dataset name']

Või sellele juurde pääsevad, index, tehke järgmist.

    ds = ws.datasets[0]


### <a name="metadata"></a>Metaandmete

Andmekomplektide on metaandmete, lisaks sisu. (Vahe andmekomplektide erandi Sellel reeglil on ja on mis tahes metaandmete.)

Mõned metaandmete väärtused on määratud kasutaja loomise ajal:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Teised on Azure ml määratud väärtused.

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Lugege teemat funktsiooni `SourceDataset` klassi Lisateavet saadaolevate metaandmete.


### <a name="read-contents"></a>Sisu lugemine

Koodilõigud, mis on esitatud arvuti õ Studio automaatselt alla laadida ja andmeatribuutide andmekomplekti Pandas DataFrame objektile. Seda saab teha selle `to_dataframe` meetod:

    frame = ds.to_dataframe()

Kui eelistate töötlemata andmete alla laadida ja selle vahemäluasukohaga ise teha, mis on soovitud suvand. Praegu on ainus võimalus vormingud, näiteks "Hädaabiteabe", mis ei saa andmeatribuutide Python kliendi teek.

Teksti sisu järgmiselt:

    text_data = ds.read_as_text()

Binaarsed sisu järgmiselt:

    binary_data = ds.read_as_binary()

Saate avada ka lihtsalt voo sisu.

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Looge uus andmekomplekti

Python kliendi teek võimaldab teil laadida andmekomplektide Python programmis. Nende andmekomplektide on siis oma tööruumi kasutamiseks saadaval.

Kui teil on oma andmete Pandas DataFrame, kasutage järgmine kood:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Kui teie andmed on juba seeriasertide, saate kasutada:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Python kliendi teek on võimalik serialiseerida Pandas DataFrame, et järgmistes vormingutes (konstandid need on selle `azureml.DataTypeIds` klassi):

 - Lihttekst
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader


### <a name="update-an-existing-dataset"></a>Mõne olemasoleva andmekomplekti värskendamine

Kui proovite üles laadida uut andmekomplekti nimega, mis vastab mõne olemasoleva andmekomplekti, saate konflikti tõrge.

Mõne olemasoleva andmekomplekti värskendada, peate esmalt saada olemasoleva andmekomplekti viide:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Seejärel kasutage `update_from_dataframe` serialiseerida ja asendamiseks andmekomplekti Azure sisu:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Kui soovite serialiseerida andmed mõnda muusse vormingusse, määrake väärtus valikuline `data_type_id` parameeter.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Soovi korral saate määrata uus kirjeldus, määrates väärtus on `description` parameeter.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

Soovi korral saate määrata uus nimi väärtust, määrates selle `name` parameeter. Nüüdsest saate tuua andmekomplekti abil ainult uus nimi. Järgmine kood värskendab andmeid, nimi ja kirjeldus.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

Funktsiooni `data_type_id`, `name` ja `description` parameetrid on valikuline ja eelmine väärtus vaikeväärtus. Funktsiooni `dataframe` parameeter on alati vaja.

Kui teie andmed on juba seeriasertide, kasutage `update_from_raw_data` asemel `update_from_dataframe`. Kui te just edastada `raw_data` asemel `dataframe`, see toimib sarnaselt.



<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
