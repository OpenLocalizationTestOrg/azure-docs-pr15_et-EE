<properties 
    pageTitle="Andmete teadus kohta Linux andmete teadus virtuaalse masina | Microsoft Azure'i" 
    description="Kuidas teha mitme levinud andmete teadus ülesannete Linux andmete teadus VM." 
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
    ms.author="bradsev;paulsh" />


# <a name="data-science-on-the-linux-data-science-virtual-machine"></a>Andmete teadus kohta Linux andmete teadus virtuaalse masina

Juhendis näidatakse, kuidas teha mitme levinud andmete teadus ülesannete Linux andmete teadus VM. Linux andmete teadus virtuaalse masina (DSVM) on saadaval Azure, mis on eelinstallitud kogum tööriistu, mis on tavaliselt kasutatakse andmete analüüsimise ja seadme õpetused virtuaalse masina pilt. Võtme tarkvara komponendid on loetletud [sätte Linux andmete teadus virtuaalse masina](machine-learning-data-science-linux-dsvm-intro.md) teema. VM pilt on lihtne alustada, tehes minutiga, andmeid teadus ilma installida ja konfigureerida iga tööriista ükshaaval. Saate hõlpsasti skaalal VM, vajaduse korral ja seda, kui ei ole kasutusel peatada. Seega on selle ressursi elastne ja maksumus tõhusa. 

Teadus Andmetoimingute näidatud selles kiirtutvustus jälgi [Meeskonnatöö andmete teadus protsessi](https://azure.microsoft.com/documentation/learning-paths/data-science-process/)kirjeldatud juhised. Selle protsessi pakub süsteemne lähenemine andmete teadus, mis võimaldab meeskondadel andmeteadlaste üle elutsükli nutikad rakenduste loomine tõhus koostöö tegemiseks. Andmete teadus protsessi näeb välja iteratiivne raamistik andmete teadus, mida saate järgneb inimesel.

Analüüsime [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) andmekomplekti ülevaate. See on kogum e-kirju, mis on tähistatud rämpsposti või sink (st ei ole rämpsposti), ja see sisaldab ka e-kirju sisu – statistika. Sisalduv statistika arutatakse edasi, kuid jaotised. 


## <a name="prerequisites"></a>Eeltingimused

Linux andmete teadus virtuaalse masina kasutamiseks peab olema järgmised:

- On **Azure tellimust**. Kui te ei ole veel üks, lugege teemat [loomine tasuta Azure'i konto juba täna](https://azure.microsoft.com/free/).
- [**Linux andmete teadus VM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). See VM ettevalmistamise kohta leiate teavet teemast [sätte Linux andmete teadus virtuaalse masina](machine-learning-data-science-linux-dsvm-intro.md). 
- [X2Go](http://wiki.x2go.org/doku.php) teie arvutisse installitud ja avada mõne XFCE seanss. Installimine ja konfigureerimine e **X2Go kliendi**kohta leiate teavet teemast [installimine ja konfigureerimine X2Go klient](machine-learning-data-science-linux-dsvm-intro.md#Installing-and-configuring-X2Go-client). 
- **AzureML konto**. Kui teil pole veel üks uus [AzureML kodulehele](https://studio.azureml.net/)kasutajaks. On tasuta kasutus kiht, mis aitavad teil alustada.


## <a name="download-the-spambase-dataset"></a>Spambase andmekomplekti allalaadimine

[Spambase](https://archive.ics.uci.edu/ml/datasets/spambase) andmekomplekti on üsna väike hulk andmeid, mis sisaldab ainult 4601 näited. See on mugav suurus, mis näitavad, et mõned andmed teadus VM kujul põhijooned hoiab ressursi nõuded mõõdukas kasutada.

>[AZURE.NOTE] Klõpsake D2 v2 suurusega Linux andmete teadus virtuaalse masina loodi juhendis. Selle suurust DSVM suudab töötlemise jaotistes toodud juhised.

Kui vajate rohkem ruumi, saate luua täiendavaid ketast ja lisada need oma VM. Ketast kasutage püsivate Azure storage, nii, et nende andmete säilib isegi siis, kui server on reprovisioned tõttu suurust või on välja lülitatud. Lisada ketta ja lisada selle oma VM, järgige [Lisa kettale Linux VM](../virtual-machines/virtual-machines-linux-add-disk.md). Need juhised kasutada Azure käsurea liides (Azure'i CLI), mis on juba installitud on DSVM. Nii saate neid toiminguid teha täiesti ise VM. Teine võimalus talletusmahu suurendamiseks on [Azure failide](../storage/storage-how-to-use-files-linux.md)kasutamine.

Andmete allalaadimine, avage terminal aknas ja käivitage see käsk:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

Allalaaditud faili ei saa päiserida, vaatame luua mõne muu fail, mis on päis. Käivitage see käsk faili loomiseks sobivat päistega.

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

Ühendab kaks faili koos käsk:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

Andmekomplekti on mitut tüüpi statistika iga e-posti. 

- Veergude, näiteks ***word\_freq\_WORDI*** näitab sõnad e-posti, mis vastavad *WORD*. Näiteks kui *word\_freq\_teha* on 1, siis 1% kõik sõnad e-posti olid *teha*. 
- Veergude, näiteks ***char\_freq\_CHAR*** e-posti kõik märgid, mis olid *CHAR*protsendi näitamiseks. 
- ***muutmine\_käivitada\_pikkus\_pikima*** on pikima pikkus suurtähed jada. 
- ***muutmine\_käivitada\_pikkus\_Keskmine*** on keskmine pikkus, kõik suurtähed järjestus. 
- ***muutmine\_käivitada\_pikkus\_kokku*** on kõik suurtähed järjestus kogupikkus. 
- ***rämpsposti*** näitab, kas meilisõnum pidas rämpsposti või mitte (1 = 0, rämpsposti ei rämpsposti =).


## <a name="explore-the-dataset-with-microsoft-r-open"></a>Andmekomplekti koos Microsoft R avatud uurimine

Vaatame läbi andmed ja tehke mõne lihtsa seadme Õppekeskuse koos R. Andmete teadus VM sisaldab [Microsoft R avatud](https://mran.revolutionanalytics.com/open/) eelinstallitud. Selles versioonis R multithreaded matemaatika teekide pakkuda töökindluse Kui ühelõimelised erinevaid versioone. Microsoft R avatud pakub reprodutseeritavuse CRAN paketi hoidlasse hetktõmmise abil.

Koopiate koodinäiteid, kasutatakse juhendis saamiseks klooni **Azure-seadme-õppe-andmete-Science** hoidla, kasutades git, mis on eelinstallitud VM. Käivitage käsureal git:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Avage terminal aknas ja R uue seansi käivitamine R interaktiivne konsooli.

>[AZURE.NOTE] Samuti saate RStudio järgmistest toimingutest. RStudio installimiseks Käivita see käsk terminalis:`./Desktop/DSVM\ tools/installRStudio.sh`

Andmete importimine ja keskkonna häälestamine, käivitage.

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

Iga veeru kokkuvõte statistika vaatamiseks tehke järgmist.

    summary(data)

Muu vaate andmed:

    str(data)

See kuvab iga muutuja ja esimese mõned väärtused andmekomplekti. 

*Rämpsposti* veerg on täisarv lugeda, kuid see on tegelikult kategooriline muutuja (või tegur). Selle tüüpi seadmiseks tehke järgmist.

    data$spam <- as.factor(data$spam)

Mõned uurimusliku analüüsi tegemiseks kasutada [ggplot2](http://ggplot2.org/) , r, mis on juba installitud VM populaarsed Graafikavidinate teegis. Pange tähele, varasemat versiooni, kuvatakse Kokkuvõte andmetest meil kokkuvõte, statistika sageduse hüüumärk märgi. Vaatame joonistada nende sagedust siin järgmised käsud:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Kuna null ribal on moonutamist soovitud diagrammi, likvideerime selle:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

On ravisaajate tihedus on suurem kui 1, mis näeb välja huvitav. Heitkem pilk lihtsalt andmeid:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Seejärel tükeldamine rämpsposti vs singi alusel:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

Nendes näidetes võimaldab teil teha sarnaselt muude veergude nendes sisalduvate andmete analüüsimiseks alad.


## <a name="train-and-test-an-ml-model"></a>Koolitada ja testida ML mudelit, mis

Nüüd vaatame koolitada paar masina õ mudelid liigitada andmekomplekti mis ajavahemiku või sink e-kirju. Me otsust puu mudeli ja selle jaotise mudeli juhusliku mets ja testige oma täpsuse oma prognoose. 

>[AZURE.NOTE] Rpart (Rekursiivsed eraldamine ja regressiooni puude) paketi kasutatud järgmine kood on juba installitud andmete teadus VM.


Kõigepealt loome tükeldage andmekomplekti koolitus ja testi komplektid:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

Ja seejärel looge Otsusepuu kuvamine e-kirju liigitada.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Siin ongi tulemus:

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

Määramaks, kuidas tehtav koolituse määramine, kasutage järgmine kood:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Määrata, kuidas tehtav testi määramine:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Proovime juhusliku mets mudel. Juhusliku metsade koolitada hulgaliselt otsust puude ja väljund klassi, mis on liigitusi kõigi üksikute otsust puude režiim. Nad pakuvad võimsam masina õppe moodust, kui nad vea, et overfit koolitus andmekomplekti otsust puu mudeli andmete keskmise. 

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-ml"></a>Azure'i ml mudeli juurutamine

[Azure'i masina Õppekeskuse Studio](https://studio.azureml.net/) (AzureML) on pilveteenuses, mis hõlbustab luua ja juurutada ennustav mudelid. Üks kena funktsioonid ja AzureML on mis tahes R funktsiooni nimega veebiteenuse avaldada. AzureML R paketi teeb lihtsaks otse soovitud DSVM meie R seansi juurutamise. 

Eelmise jaotise kood otsust puu juurutamiseks peate Azure seadme õ Studio sisse logida. Peate oma tööruumi ID-d ja on autoriseerimine luba ohkama sisse. Nende väärtuste otsimine ja käivitamine AzureML muutujate nendega järgmist.

Valige vasakpoolses menüüs **sätted** . Pange tähele oma **TÖÖRUUMI ID -d**. ![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)

Valige **Luba sõned** üldkulu menüüst ja pange tähele teie **Peamine autoriseerimine Turbeloa**. ![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)

**AzureML** paketi laadimine ja seadke siis muutujate luba ja tööruumi ID väärtused seansi R on DSVM kohta:


    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Vaatame lihtsustada mudeli selle tutvustamise hõlbustamiseks kasutusele võtta. Valige kolm muutujate lähemal root otsustuspuu ja koostada uue puu just need kolm muutujate abil.

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

Läheb vaja ennustamine funktsioon, mis võtab funktsioonide sisendina ja annab vastuseks prognoositud väärtused:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

Funktsiooni predictSpam avaldamine AzureML **publishWebService** funktsiooni abil: 

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

See funktsioon võtab **predictSpam** funktsiooni, loob nimega **spamWebService** määratletud sisendina ja väljundid veebiteenuse ja tagastab uue lõpp-punkti teave.

Avaldatud veebiteenuse, sh selle API lõpp-punkti üksikasjade kuvamine ja kiirklahvide käsuga:

    spamWebService[[2]]

Proovima esimene 10 ridade katse määrata.

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Muude saadaolevate tööriistade kasutamine

Ülejäänud jaotiste näitab, kuidas kasutada mõned installitud Linux andmete teadus VM tööriistad. Siin on tööriistade loend:

- XGBoost
- Python
- Jupyterhub
- Vurama
- PostgreSQL-i ja orav SQL-i
- SQL Serveri andmebaas


## <a name="xgboost"></a>XGBoost

[XGBoost](https://xgboost.readthedocs.org/en/latest/) on vahend, mis annab kiire ja täpne võimendatud puu rakendamist.

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

XGBoost saate helistada python või mõne käsurea.

## <a name="python"></a>Python

Kasutades Python arengu Anaconda Python jaotuse 2.7 ja 3.5 installitud on DSVM sisse. 

>[AZURE.NOTE] Anaconda jaotuse sisaldab [Condas](http://conda.pydata.org/docs/index.html), mis saab luua kohandatud keskkonnas Python, mis on erinevate versioonide ja/või installitud need paketid.

Vaatame lugemine mõnes spambase andmekomplekti ja liigitada e-kirju koos tugi vektorkuju masinad scikit – lugege:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Ennustatud väärtuste tegemine

    clf.predict(X.ix[0:20, :])

Kuidas avaldada lõpp AzureML kuvamiseks teeme lihtsam mudel kolme muutujaga kui me ei, kui me varem avaldatud R mudel. 

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Kui seate mudeli avaldamiseks AzureML:

    # Publish the model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about the resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call the model
    predictSpam.service(1, 1, 1)

>[AZURE.NOTE] See on saadaval ainult jaoks python 2.7 ja 3.5 veel ei toetata. Käivita **/anaconda/bin/python2.7**.


## <a name="jupyterhub"></a>Jupyterhub

Klõpsake soovitud DSVM Anaconda jaotuse kaasas Jupyter märkmikku, mitu platvormi keskkonna Python, R või Julia kood ja analüüsi jagamiseks. Märkmiku Jupyter pääseb juurde JupyterHub. Teie kohaliku Linux kasutajanime ja parooli abil logite ***https://\<VM DNS-i nimi või IP-aadress\>: 8000 /***. Kõik failid JupyterHub jaoks leidub directory **/etc/jupyterhub**.

Mitme valimi märkmike on juba installitud VM:

- Lugege teemat [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) valimi Python märkmiku.
- Vaadake [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) valimi **R** märkmik.
- Vt [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) jaoks teise valimi **Python** märkmikku.

>[AZURE.NOTE] Julia keele pakutakse ka käsurea Linux andmete teadus VM.


## <a name="rattle"></a>Vurama

[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (selle R Analytical tööriista abil saate teada, hõlpsasti) on graafiline R tööriist andmete hankimiseks. See on intuitiivne liides, mis hõlbustab laadimine, uurida, ja muuta andmeid ja koostada ja hinnata mudelite.  See artikkel [Rattle: A Data Miningu GUI R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) pakub lühiülevaade, mis näitab selle funktsioone.

Installimine ja käivitamine vurama järgmised käsud:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

>[AZURE.NOTE] Klõpsake soovitud DSVM ei pea installi. Kuid vurama võib teilt installimiseks pakette laadimisel.

Vurama kasutab menüü-põhine kasutajaliides. Enamik vahekaarte vastavad juhised [Andmete teadus protsessi](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), nagu andmete laadimine või seda. Andmete teadus protsessi liigub sujuvalt vasakult paremale sakke. Kuid viimase menüü sisaldab Logi R käske vurama käivitamine. 


Laadimine ja konfigureerida andmekomplekti tehke järgmist.

- Laadimise fail, seejärel valige menüü **andmed** 
- Valige Vaateselektori **faili nime** kõrval ja valige **spambaseHeaders.data**. 
- Faili laadimiseks. Valige **käivitamine** ülemises reas olevaid nuppe. Peaksite nägema kokkuvõtte iga veeru, sh selle määratletud andmetüüp, kas sisend, siht või muud tüüpi muutuja ja Üheste väärtuste arvu.
- Vurama tuvastas õigesti **rämpsposti** veerg, mis Sihtvaluuta. Valige rämpsposti veerg ja seejärel **Categoric** **Target andmetüübi** määramine.

Andmeid uurima: 

- Valige vahekaart **Uuri** . 
- Klõpsake nuppu **Kokkuvõte**ja seejärel **Käivita**, muutuv kohta leiate teavet ja mõned kokkuvõte, statistika kuvamiseks. 
- Muud tüüpi iga muutuja statistika vaatamiseks valige Muud valikud, näiteks **kirjeldada** või **põhitõed**.

Vahekaart **Uuri** ka võimaldab teil luua palju kasulik krundid. Histogrammi andmed diagrammile:


- Valige **jaotuse**.
- Kontrollige **word_freq_remove** ja **word_freq_you** **histogramm** .
- Valige **käivitada**. Peaksite nägema nii tihedusfunktsiooni krundid ühe Graphi aknas, kus on selge, et sõna "sina" kuvatakse palju sagedamini meilisõnumite kui "Eemalda".

Korrelatsiooni proovitükid on ka huvitav. Üks loomiseks tehke järgmist.


- **Korrelatsiooni** **Tüüp**, seejärel valige 
- Valige **käivitada**. 
- Vurama hoiatab, et soovitatakse kuni 40 muutujate. Valige **Jah** vaadata selle diagrammi. 

On mõned huvitav korrelatsiooni tulla: "tehnoloogia, mis on tugevalt seotud"HP"ja"labs", näiteks. See on ka tugevalt seotud "650", kuna andmehulga doonorite suunakood on 650.

Arvuliste väärtuste jaoks korrelatsiooni sõnade vahel on saadaval Uuri aknas. See on huvitav näiteks, et "tehnoloogia, mis on negatiivselt seotud"teie"ja"raha".

Vurama saate muuta andmekomplekti levinumate probleemide käsitlema. Näiteks see võimaldab teil rescale funktsioone, puuduvate väärtuste omistamiseks, toime võõrväärtuste ja eemaldada muutujate või vaatluste puuduvate andmetega. Vurama saate tuvastada seos reeglid vaatluste ja/või muutujate vahel. Selles sissejuhatavas selgituse väljapoole on need tabelduskohad.

Vurama saate teha ka kobar analüüsi. Vaatame välistada teatud funktsioonid väljund hõlpsamaks lugemiseks. Klõpsake menüü **andmed** nuppu **Ignoreeri** muutujate peale järgmiste üksuste esikümne kõrval:

- word_freq_hp
- word_freq_technology
- word_freq_george
- word_freq_remove
- word_freq_your
- word_freq_dollar
- word_freq_money
- capital_run_length_longest
- word_freq_business
- rämpsposti

Minge tagasi vahekaardile **kobar** , valige **KMeans**ja seatud *kogumite arvu* 4. Seejärel **käivitada**. Tulemused kuvatakse väljundi aknas. Ühe kobar on tõenäoliselt ärilise e-posti ja kõrge sagedus "Georg" ja "hp".

Saate luua lihtsa otsust puu masina õ mudeli: 

- Valige vahekaart **mudel** 
- Valige **puu** **Tüüp**. 
- Valige **Käivita** tekst vorm aknas väljund puu kuvamiseks. 
- Valige **Joonista** nupp graafiline versiooni vaatamine. See näeb välja üsna sarnased me saadud varem abil *rpart*puu.

Üks kena funktsioonid ja vurama on mitu masina õ võimalust käivitamiseks ja need kiiresti väärtustada. Siin on korras.

- Valige **Kõik** **Tüüp**. 
- Valige **käivitada**. 
- Kui see on lõpule viidud saate klõpsake mis tahes ühe **Tüüp**, nt **SVM**, ja tulemusi vaadata. 
- Samuti saate võrrelda **hinnata** vahekaardil seadmine kinnitamise mudelites jõudlus. Näiteks **Tõrge maatriksi** valiku näete segadust maatriksi, Üldine tõrge ja Keskmine klassi viga iga mudeli valideerimine Määrake. 
- Saate joonistada ROC kõverad, tundlikkuse analüüsi ja muud tüüpi mudeli hinnangud teha.

Kui olete lõpetanud, hoone mudelid, valige **Logi** menüü kuvamiseks R koodi käivitada vurama seansi ajal. Saate valida salvestamiseks nuppu **ekspordi** . 

>[AZURE.NOTE] Praeguse versiooni vurama on viga. Skripti muuta või selle abil, korrake hiljem, peate lisama # märk ette *ekspordi selle log...* Logi tekst. 


## <a name="postgresql--squirrel-sql"></a>PostgreSQL-i ja orav SQL-i

Funktsiooni DSVM on installitud PostgreSQL-i. PostgreSQL-i on keerukaid, avatud lähtekoodi relatsiooniandmebaasist. Selles jaotises kirjeldatakse lepingute meie rämpsposti andmekomplekti laadimine PostgreSQL-i ja seejärel selle päringu.

Enne kui saate laadite andmed, peate esmalt lubama Paroolautentimine localhost kaudu. Käsureale:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

Otsingukonfiguratsiooni faili allosas on read, mis on üksikasjalikult lubatud ühendusi.

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

"IPv4 kohaliku ühendused" rea kasutada md5 asemel ident, et saaksime saate sisse logida kasutajanime ja parooli abil muutmiseks tehke järgmist.

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

Ja taaskäivitage postgres teenuse:

    sudo systemctl restart postgresql

Psql käivitamiseks on interaktiivne, mis pakub PostgreSQL-i sisseehitatud postgres kasutajaks, käivitage järgmine käsk: viip:

    sudo -u postgres psql

Uue kasutajakonto, kasutades sama kasutajanime te olete praegu sisse logitud ja andke sellele parooli Linux konto loomiseks tehke järgmist.

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Seejärel logige oma kasutaja psql:

    psql

Ja uue andmebaasi importida andmed.

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Nüüd Alustame andmete uurimine ja mõned päringute abil **Orav SQL-i**, graafilise tööriista, mis võimaldab teil suhelda andmebaaside JDBC draiveri kaudu käivitada.

Alustamiseks käivitage orav SQL-i menüüst rakendused. Juhi seadistamine

- Valige **Windows**, siis **Vaade draiverid**. 
- Paremklõpsake **PostgreSQL-i** ja valige käsk **Muuda draiver**. 
- Valige **eest klassi tee**ja seejärel **lisada**. 
- Sisestage **faili nimi** ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** ja 
- Valige käsk **Ava**.
- Valige loendis draiverid, seejärel valige **org.postgresql.Driver** **Klassi nimi**ja valige **OK**.

Kohaliku serveriga ühenduse seadistamine
 
- Valige **Windows**, siis **vaadata pseudonüümid.** 
- Valige soovitud **+** nuppu Uus pseudonüüm. 
- *Rämpsposti andmebaasi*nime, valige rippmenüüst **draiver** **PostgreSQL-i** .
- Seadke *jdbc:postgresql://localhost/spam*URL-i. 
- Sisestage oma *kasutajanimi* ja *parool*. 
- Klõpsake nuppu **OK**. 
- **Ühenduse** akna avamiseks topeltklõpsake ***rämpsposti andmebaasi*** alias (pseudonüüm). 
- Valige **ühendust**.

Mõned päringud käivitamiseks tehke järgmist.

- Valige vahekaart **SQL-i** .
- Sisestage näiteks lihtsa päringu `SELECT * from data;` tekstiväljale päringu SQL-i menüü ülaosas. 
- Vajutage **Klahvikombinatsiooni Ctrl + Enter** selle käivitamiseks. Vaikimisi orav SQL-i tagastab esimese 100 read päringust. 

On palju rohkem päringuid saab käivitada andmete analüüsimiseks. Näiteks, kuidas sagedus *teha* Wordi erineb rämpsposti ja singi vahel?

    SELECT avg(word_freq_make), spam from data group by spam;

Või mida on omaduste e-posti sagedamini sisaldavad *3d*?

    SELECT * from data order by word_freq_3d desc;

Enamik e-kirju, millel on suur esinemine *3d* ilmselt rämpsposti, seega oleks kasulik funktsioon hoone prognoositava mudeli liigitada e-kirju.

Kui soovite teha seadme õ PostgreSQL-i andmebaasi säilitatavate andmetega, kaaluge [MADlib](http://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>SQL Serveri andmebaas

SQL Azure'i andmebaas on võimeline töötlemiseks suuri andmemahtusid, relatsiooniline- ja relatsiooniline pilvepõhist, skaala-out andmebaas. Lisateavet leiate teemast [mis on SQL Azure'i andmebaas?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Ühenduse andmebaas ja tabel luua, käivitage järgmine käsk käsureale:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Seejärel sqlcmd käsureale:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Kopeerige andmed koos bcp.

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

>[AZURE.NOTE] Allalaaditud faili rea lõpud on Windowsi-laadi, kuid bcp eeldab, et UNIX laadis peame kindlaks bcp - r lipuga.

Ja päringu sqlcmd.

    select top 10 spam, char_freq_dollar from spam;
    GO

Teil võib päringu orav SQL. PostgreSQL-i, kasutades Microsoft MSSQL serveri JDBC draiveri, mille leiate ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***jaoks sarnaseid juhiste.

## <a name="next-steps"></a>Järgmised sammud

Teemad, mis hõlmavad andmete teadus protsessi Azure tööülesandeid sõelub ülevaate leiate teemast [Meeskonnatöö andmete teadus protsess](http://aka.ms/datascienceprocess).

Muud lõpuni juhendavad tutvustused, mis illustreerivad teatud stsenaariumide meeskonnatöö andmete teadus protsessi etappe kirjeldust vt [meeskonnatöö andmete teadus protsessi juhendavad tutvustused](data-science-process-walkthroughs.md). Selle juhendavad tutvustused illustreerimine ka pilveteenuses ja kohapealsete tööriistad ja teenuste ühendamine töövoo või müügivõimaluste nutikad rakenduse loomiseks tehke järgmist.

