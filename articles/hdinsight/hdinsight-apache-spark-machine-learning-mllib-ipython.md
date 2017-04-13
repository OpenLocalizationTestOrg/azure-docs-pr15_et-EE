<properties 
    pageTitle="Apache Spark abil saate koostada seadme õ rakendusi Hdinsightiga | Microsoft Azure'i" 
    description="Üksikasjalike juhiste kohta, kuidas luua seadme õ rakendusi Apache Spark märkmike kasutamine" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="machine-learning-predictive-analysis-on-food-inspection-data-using-mllib-with-apache-spark-cluster-on-hdinsight-linux"></a>Masina õ: sõnastikupõhine analüüsi toiduga kontrolli andmed, kasutades Apache Spark MLlib klaster Hdinsightiga Linux

> [AZURE.TIP] Selles õpetuses on saadaval Jupyter märkmiku säde (Linux) klaster, mis on loodud Hdinsightiga ka. Märkmiku teenuse abil saate käivitada Python Koodilõigud märkmikust ise. Õpetuse kaudu sees märkmiku täitmiseks luua säde kobar, käivitage Jupyter märkmiku (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), ja seejärel käivitage märkmikule **säde masina õppe - toiduga kontrolli ennustav analüüs MLLib.ipynb** **Python** kaustas.


Selles artiklis näitab, kuidas kasutada **MLLib**, säde 's sisseehitatud arvuti Õppekeskuse teegid sooritada lihtsa ennustav analüüs on avatud andmekomplekti. MLLib on core säde teek, mis pakub mitmesuguseid Utiliidid, mis on abiks seadme õ ülesanded, sh Utiliidid, mis sobivad:

* Liigitamine

* Regressioonisirge

* Rühmitamise

* Teema: modelleerimine

* Ainsuses väärtus lagunemise (SVD) ja peamine komponent analüüs (PKL)

* Juhul testimine ja näidis-statistiku arvutamine

Selles artiklis esitatakse on lihtne lähenemine *liigitamine* logistilise regressiooni kaudu.

## <a name="what-are-classification-and-logistic-regression"></a>Mis on liigitamine ja logistilise regressiooni?

*Liigitamine*, väga levinud masina Õppekeskuse tööülesande, on sisendandmete kategooriasse sortida. See on töö liigitamine algoritmi aru saada, kuidas määrata "siltide" sisestada andmeid. Näiteks võib arvate arvuti õ algoritmi aktsepteerib börsidiagrammi teabe sisendina ja jagab varude ühte kahest järgmisest kategooriast: varude, mis peaks müüte ja mis teil peaks jääma.

Logistilise regressiooni on liigitamine kasutatav algoritmi. Säde 's logistilise regressiooni API on kasulik *binaarsed liigitamine*või liigitamine sisendandmete ühte kahest rühmad. Logistiline regressioon kohta leiate lisateavet teemast [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).

Kokkuvõttes toodab logistilise regressiooni protsessi *logistilist funktsiooni* saab kasutada prognoosida tõenäosus on Sisestuskeel vektorkuju kuulub rühma ühe või teise.  

## <a name="what-are-we-trying-to-accomplish-in-this-article"></a>Mida me üritavad selle artikli?

Kasutate säde sooritada teatud sõnastikupõhise analüüs toiduga kontrolli andmed (**Food_Inspections1.csv**), mis on saadud [Chicago linna andmete portaali](https://data.cityofchicago.org/)kaudu. Tuvastatavate sisaldab teavet toidu kontrolle, mis olid Pärnu, sh teavet iga toidu loomist, mis on kontrollitud, rikkumised, mis ei leia (vajaduse korral) ja kontrolli tulemusi. Andmed CSV-fail on juba seotud kobar veebisaidil **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**salvestusruumi konto saadaval.

Mõned alltoodud toimingutes arendate mudeli kulub edasi või ei toiduga kontrolli kuvamiseks. 

## <a name="start-building-a-machine-learning-application-using-spark-mllib"></a>Seadme õ rakenduse abil säde MLlib koostamise alustamine

1. [Azure portaali](https://portal.azure.com/)kaudu startboard, klõpsake paani klaster säde (kui te kinnitatud selle startboard). Samuti saate liikuda klaster jaotises **Sirvi kõiki** > **Hdinsightiga kogumite**.   

2. Keelest säde kobar valige **Kobar armatuurlaud**, ja klõpsake nuppu **Jupyter märkmiku**. Kui kuvatakse vastav viip, sisestage administraatori identimisteave klaster.

    > [AZURE.NOTE] Võib-olla ka jõuate Jupyter märkmiku jaoks klaster, avades järgmine URL brauseri. Klaster nime __CLUSTERNAME__ asendamiseks tehke järgmist.
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Looge uus märkmik. Klõpsake nuppu **Uus**ja seejärel klõpsake nuppu **PySpark**.

    ![Jupyter uue märkmiku loomine] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.createnotebook.png "Jupyter uue märkmiku loomine")

3. Uus märkmik on loodud ja nimega Untitled.pynb avada. Klõpsake selle märkmiku nime kohal ja sisestage sõbralik nimi.

    ![Esita märkmiku nimi] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.notebook.name.png "Esita märkmiku nimi")

3. Kuna te abil PySpark tuum märkmiku loonud, peate looma kõik kontekstides otseselt. Säde ja taru konteksti luuakse automaatselt teie eest, kui käivitate kood esimest lahtrit. Saate alustada koostamise Õppekeskuse rakenduse importimise tüübid seda stsenaariumi jaoks nõutavad arvuti. Selleks, viige kursor lahtrisse ja vajutage klahvikombinatsiooni **SHIFT + ENTER**.


        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Mõne Sisestuskeel dataframe ehitada

Me ei saa kasutada `sqlContext` sooritamiseks teisendused Liigendatud andmete põhjal. Esimene tööülesanne on laadimiseks säde SQL *dataframe*Näidisandmete ((**Food_Inspections1.csv**)). 

1. Kuna töötlemata andmete on CSV-vormingus, tuleb kasutada säde kontekstis iga rida faili mällu struktureerimata tekstina; seejärel kasutage Python's CSV teegi sõeluda eraldi ridadele. 


        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value
        
        inspections = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)


2. Meil on nüüd CSV-fail nimega mõne RDD. Andke meile tuua ühe rea RDD mõista skeemi andmed.


        inspections.take(1)


    Peaksite nägema umbes selline väljund:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]


3. Ülaltoodud väljundi annab meile aimu, milline skeemiga Sisestuskeel faili; fail sisaldab nime iga üksus, loomise, aadress, kontroll ja koha, muu hulgas andmete tüüpi. Vaatame valige mõni veerud, mis on abiks meie sõnastikupõhise analüüsi ja rühmale tulemused dataframe, mida me kasutame siis ajutine tabeli loomiseks.


        schema = StructType([
        StructField("id", IntegerType(), False), 
        StructField("name", StringType(), False), 
        StructField("results", StringType(), False), 
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')

4. Nüüd on *dataframe* `df` aluseks saame teha meie analüüs. Meil on kõne **CountResults**ajutise tabeli. Oleme lisanud 4 veeru huvi selle dataframe: **id**, **nimi**, **tulemuste**ja **rikkumised**. 
    
    Vaatame saada väikese valimi andmeid:

        df.show(5)

    Peaksite nägema umbes selline väljund:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a>Andmete mõistmine

1. Alustame saada tunnet, mis sisaldab meie andmekomplekti. Näiteks, mis on erinevate väärtused veerus **tulemused** ?


        df.select('results').distinct().show()

    
    Peaksite nägema umbes selline väljund:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
    
2. Kiirülevaate visualiseeringu aitab meil põhjus jaotuse kohta järgmised tulemused. Meil on juba **CountResults**ajutise tabeli andmeid. Järgmine SQL-päringu käivitada vastu tabelis, et paremini mõista, kuidas tulemused jagatakse.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    Funktsiooni `%%sql` maagiline, millele järgneb `-o countResultsdf` tagab, et päringu väljund püsis kohalikult Jupyter server (tavaliselt headnode klaster). Väljund on jätkunud [Pandas](http://pandas.pydata.org/) dataframe koos määratud nimi **countResultsdf**nimega.
    
    Peaksite nägema umbes selline väljund:
    
    ![SQL-päringu väljund] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/query.output.png "SQL-päringu väljund")

    Lisateavet selle `%%sql` maagiline, samuti muud magics saadaval PySpark tuum, lugege teemat [tuumad Jupyter märkmikke säde Hdinsightiga kogumite saadaval](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

3. Saate Matplotlib kasutatud andmete visualiseerimine teegi lisamine diagrammi loomiseks. Kuna soovitud diagrammi tuleb luua kohalikult nõutud **countResultsdf** dataframe, koodilõigu peab algama selle `%%local` maagiline. See tagab kood käivitatakse kohalikult Jupyter serveris.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        
        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Peaksite nägema umbes selline väljund:

    ![Tulem väljund](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_13_1.png)


4. Saate vaadata 5 erinevate tulemusi, et kontrolli võib olla:
    
    * Business ei asu 
    * Nurjuda
    * Jõustamine
    * PSS w / tingimused ja
    * Tegevuse 

    Andke meile arendada mudelit, mis saab tulemusi toiduga kontrolli, võttes arvesse rikkumised. Kuna logistilise regressiooni kahendarvu liigitamine meetod, on mõttekas meie andmete rühmitamiseks ühte kahest järgmisest kategooriast: **nurjuvad** ja **edasi**. Mõne "arvestatud w / tingimused" on endiselt Pass, nii, et kui me koolitada mudelit, me ei arvestage kahe tulemuse võrdväärse. Andmete muude tulemustega ("Business ei asu", "välja Business") ei ole kasulik, et me nende eemaldab meie koolituse määramine. See peaks olema korras, kuna need kaks kategooriat moodustavad väga väikese osa tulemused ikkagi.

5. Andke meile minna ja teisendada meie olemasoleva dataframe (`df`) uue dataframe, kus iga kontrolli on esitatud arvuna paari sildi-rikkumised sisse. Meie puhul sildi, `0.0` tõrge, sildi, tähistab `1.0` tähistab õnnestumise ja sildi, `-1.0` tähistab peale need kaks mõned tulemused. Meil on välja filtreerida need teiste tulemuste arvutamisel uute andmete paneeli.


        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')


    Vaatame tuua ühe rea siltidega andmed näha, kuidas see välja näeb.


        labeledData.take(1)


    Peaksite nägema umbes selline väljund:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]


## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Sisestuskeel dataframe logistilise regressiooni mudeli loomine

Meie lõplik ülesanne on sildistatud andmete teisendamiseks vormingut, mida saab analüüsida logistilise regressiooni. Sisestusmeetodi logistilise regressiooni algoritmile peaks olema kogum, *sildi-funktsioon vektorkuju paarideks*, kus "funktsioon" on vektorkuju arvu, mis tähistab Sisestuskeel punkti mingil viisil. Jah, läheb vaja võimalus "rikkumised" veergu, mis on poolstruktuur ja see sisaldab palju kommentaare vaba-teksti, reaalarvudeks, mis võivad kergesti mõista masina massiivi teisendada. 

Ühe tavalise masina lähenemisviisi poole töötlemiseks loomulikus keeles on määrata iga erinevate sõna "index" ja sisestage soovitud vektorkuju masina Õppekeskuse algoritmi, et iga indeksi väärtus sisaldab see sõna tekstistringi suhteline sagedus. 

MLLib abil on lihtne selle toimingu sooritamiseks. Esmalt meil kuvatakse "tokenize" iga rikkumised stringi saada üksikuteks sõnadeks iga stringi ja siis kasutame on `HashingTF` teisendada iga kriteeriumikogum sõned funktsiooni vektorkuju, mis saab edasi siis logistilise regressiooni algoritmi, et koostada mudel. Me tuleb läbi kõiki neid toiminguid "müügivõimaluste" abil.


    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    
    model = pipeline.fit(labeledData)


## <a name="evaluate-the-model-on-a-separate-test-dataset"></a>Mudeli eraldi testi andmekogumis hindamine

Kasutame me varem loodud *prognoosida,* mis tulemuste mudeli toimub uue kontrolli, põhineb rikkumised, mida. Me koolitada selle andmekomplekti **Food_Inspections1.csv**mudel. Andke meile kasutamine teise andmehulga, **Food_Inspections2.csv**, *hinnata* selle mudel uute andmete tugevus. Selle teise andmehulga (**Food_Inspections2.csv**) olema seostatud klaster vaikimisi salvestusruumi ümbrises.

1. Allpool koodilõigu loob uue dataframe, **predictionsDf** , mis sisaldab mudeli ennustamiseks. Väljavõte loob ka **ennustatud väärtuste** põhjal on dataframe ajutise tabeli.


        testData = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns


    Peaksite nägema umbes selline väljund:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
        
        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']

2. Vaadake ühte prognoose. Käivitage see koodilõigu.

        predictionsDf.take(1)

    Kuvatakse ennustamine esimese kirje testi andmehulgas.

3. Funktsiooni `model.transform()` meetod rakendada sama teisendus sama skeemi uusi andmeid ja ennustamiseks liigitamiseks andmed jõudma. Me teha mõned lihtsad statistika saada tunnet, kuidas täpselt meie prognoose olid.


        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR 
                                              (prediction = 1 AND (results = 'Pass' OR 
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()
        
        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    Väljund näeb välja umbes järgmine:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate


    Logistilise regressiooni abil säde annab meile mudelit, mis on täpsed rikkumised kirjeldused inglise ja kas antud business oleks edasi või ei toiduga kontrolli vahel seose. 

## <a name="create-a-visual-representation-of-the-prediction"></a>Visuaali prognoosi loomine

Saame nüüd ehitada lõplik visualiseeringu hõlbustavate põhjuse kohta selle testi tulemused. 

1. Alustame selle eri prognoose ja tulemuste tabelist **prognoose** ajutine varem loodud. Järgmised päringud eraldi väljund *true_positive*, *false_positive*, *true_negative*ja *false_negative*. Päringute alloleval, oleme välja lülitada visualiseeringu abil `-q` ja salvestada ka väljund (abil `-o`) nimega dataframes, mida saate siis kasutada funktsiooni `%%local` maagiline. 

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions') 

2. Kasutage järgmist koodilõigu luua diagrammi, kasutades **Matplotlib**.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')
    
    Peaksite nägema järgmine väljund.
    
    ![Ennustamine väljund](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_26_1.png)


    Selle diagrammi "kasum" viitab nurjunud toiduga kontrolli, kui tulemus on negatiivne, mis viitab edastatud kontrolli.

## <a name="shut-down-the-notebook"></a>Märkmiku sulgeda

Pärast lõpetamist töötab rakendus, peaksite sulgumist ressursside vabastamiseks märkmik. Selleks märkmikku menüü **fail** nuppu **Sule ja peatada**. See on Sule ja Sule märkmik.


## <a name="seealso"></a>Vt ka


* [Ülevaade: Apache Spark klõpsake Azure Hdinsightiga](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Stsenaariumid

* [Bi säde: andmeanalüüside interaktiivsed Hdinsightiga säde kasutamine koos Ärianalüüsi tööriistade kohta](hdinsight-apache-spark-use-bi-tools.md)

* [Seadme õppimisega säde: kasutamine säde rakenduses Hdinsightiga building temperatuur HVAC andmete analüüsimiseks](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Säde Streaming: Kasutamine säde rakenduses reaalajas streaming rakenduste Hdinsightiga](hdinsight-apache-spark-eventhub-streaming.md)

* [Veebisaidi logi analüüs Hdinsightiga säde kasutamine](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Luua ja kasutada rakendusi

* [Kasutades Scala rakendusena loomine](hdinsight-apache-spark-create-standalone-application.md)

* [Käivitage töö eemalt säde klaster Liviuse abil](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tööriistad ja laiendid

* [Hdinsightiga tööriistade lisandmooduli huvitav idee abil saate luua ja esitage säde Scala rakendusi](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Hdinsightiga tööriistade lisandmooduli huvitav idee abil silumine säde rakenduste kaugühenduse teel](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Säde kobar klõpsake Hdinsightiga Zeppelin märkmike kasutamine](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Tuumad Jupyter märkmiku säde kobar Hdinsightiga jaoks saadaval](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Välise pakettide Jupyter märkmike kasutamine](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter oma arvutisse installida ja luua ühenduse mõne Hdinsightiga säde kobar](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Ressursside haldamine

* [Ressursid Apache Spark kobar rakenduses Windows Azure Hdinsightiga haldamine](hdinsight-apache-spark-resource-manager.md)

* [Töötavate on Apache Spark kobar rakenduses Hdinsightiga jälitamine ja silumine tööde haldamine](hdinsight-apache-spark-job-debugging.md)
