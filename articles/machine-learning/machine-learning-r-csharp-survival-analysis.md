<properties 
    pageTitle="Azure seadme õ Survival analüüs | Microsoft Azure'i" 
    description="Survival analüüsi sündmuse esinemiskord tõenäosuse." 
    services="machine-learning" 
    documentationCenter="" 
    authors="zhangya" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="zhangya"/> 


#<a name="survival-analysis"></a>Survival analüüs 

Paljudel juhtudel jaotises hinnatavate peamine tulemus on intressi sündmuse aega. Teisisõnu, küsimus ", kui see sündmus toimub?" teil palutakse. Näited, kaaluge olukordades, kus kirjeldatakse andmete möödunud aeg (päeva, aastat, läbitud vahemaa jne) kuni intressi (uuesti, doktorikraad saanud, piduri Valimisklahvistik tõrge) sündmus esineb. Iga eksemplari andmeid tähistab objekt (lisamine patsientide, õppur, on auto jne).


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Selle [veebiteenuse]( https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) vastused "mis on tõenäosus, et intressi sündmus toimub aja n jaoks objekti x?" küsimus Mudeli survival analüüsi pakkudes selle veebiteenuse võimaldab kasutajatel andmeid mudel ja testida. Katse põhiteema on modelleerimise kulunud aeg, kuni intressi sündmus. 

>See veebiteenus võib tarbitud kasutajaid – potentsiaalselt mobiilirakenduse kaudu veebisaidi või isegi kohalikus arvutis, näiteks. Kuid veebiteenuse eesmärk on ka olla näide sellest, kuidas saab kasutada Azure seadme õ veebiteenuste peal R-koodi loomiseks. Vaid mõned koodiread R ja klõpsu nuppudel Azure seadme õ Studio, saate katse loodud R koodi ja veebiteenuse avaldatud. Seejärel saate veebiteenuse avaldatud Azure'i turuplatsi ja tarbitud kasutajatele ja seadmetele kogu maailmas pole taristu häälestamine veebiteenuse autori.  

##<a name="consumption-of-web-service"></a>Veebiteenuse tarbimine

Sisendandmete skeemiga veebiteenuse on näidatud järgmises tabelis. Kuus liiki andmed on vaja sisend: andmete koolitus, testimine andmed, aeg huvipakkuvaid "time" registri dimensiooni index "sündmus" mõõtme ja muutuja tüüpi (pidev või tegur). Koolitus andmed on esindatud stringi, kus read on eraldatud komaga ja veergude semikoolonitega. Andmete funktsioonid on paindlik. Kõik elemendid Sisestuskeel string peab olema arv. Koolitus andmeid, "ajadimensiooni" näitab aja ühikute (päevad, aastat, läbitud vahemaa jne) möödunud alguspunkti uuringu (patsientide, vastuvõtmine uimastitega kohtlemine programmid, õpilaste käivitamine PhD õpperühma, auto alates juhtida jne) kuni intressi (selle patsientide naasevad uimastitega kasutus saamine doktorikraad, millel piduri Valimisklahvistik tõrge auto õpilase sündmus jne.) toimub. "Sündmus" mõõde näitab, kas huvi sündmus uuringu lõpus. Väärtus "sündmuse = 1" tähendab, et intressi sündmuse ajal tähisega "" ajadimensiooni; "sündmuste = 0" tähendab, et intressi sündmus ei ole esinenud tähisega "ajadimensiooni" ajaks.

- trainingdata - märgistringi. Read eraldatakse komadega ja veergude semikoolonitega. Iga rida sisaldab "ajadimensiooni", "sündmus" mõõde ja ennustaja muutujate.
- testingdata - ühe rea, mis sisaldab teatud objektile ennustaja muutujad.
- time_of_interest - intressi n möödunud aeg.
- index_time - veeru indeks "" ajadimensiooni (alates 1).
- index_event - veeru indeks "sündmus" dimensiooni (alates 1).
- variable_types – see eraldavate semikoolonitega märgistringi. 0 tähistab pidev muutujate ja 1 tegur muutujate.


Väljund on tõenäosuse konkreetse aja seostada. 

>See teenus majutatud Azure'i turuplatsi, on teenuse OData; need võivad nn POST või GET meetodite abil. 

On mitu võimalust tarbimine automatiseeritud viisil teenuse (nt rakendus on [siin](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

###<a name="starting-c-code-for-web-service-consumption"></a>C# koodi web teenuse tarbimine alates:
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




Selle testi tõlgendamise on järgmine. Eeldades, et andmete eesmärk on modelleerimise kulunud aeg kuni uimastitega kasutus pöördu patsientide, kes saadud kaks programmid. Väljundi web teenuse loeb: patsientide 35 aastat, võttes eelmise ravimi kohtlemine 2 korda, võttes pikk elu kohtlemine programmi, ja nii heroiini ja kokaiini kasutamine, naasevad uimastitega kasutus tõenäosus on 95.64% päeva 500.

##<a name="creation-of-web-service"></a>Veebiteenuse loomine

>See veebiteenus loodi Azure seadme õ. Tasuta prooviversioon, samuti sissejuhatavad videod katsete ja [avaldamise veebiteenuste](machine-learning-publish-a-machine-learning-web-service.md)loomise kohta, vt [azure.com/ml](http://azure.com/ml). Allpool on loodud web teenuse ja näiteks koodi iga katse jooksul moodulid katse kuvatõmmis.

Azure'i masina õ, sees uus tühi katse on loodud ja kahe [Käivitada R skripti] [ execute-r-script] moodulid olid tõmmatakse peale tööruumi. Andmete skeemi loodi [R skripti käivitada]lihtsa[execute-r-script], mis määratleb sisendandmete skeemi veebiteenuse. Selle mooduli siis lingitud teise [Käivitada R skripti] [ execute-r-script] moodul, mis suure töö. Selle mooduli ei andmete eeltöötlus mudeli building ja prognoose. Andmete eeltöötlus etapis tähistab pikk string sisendandmete ümber ja ümber raami andmed. Juhises mudeli building välise R paketi "survival_2.37 7.zip" installitakse esmalt läbi survival analüüs. Siis täidetakse pärast sarja andmete töötlemise tööülesannete funktsiooni "coxph". Funktsiooni "coxph" survival analüüsi üksikasjad saate lugeda R dokumentatsiooni kohta. Juhises ennustamine testimise eksemplari on varustatud koolitatud mudeli funktsiooni "surfit" ja survival kõvera testimise töövooeksemplari on toodetud nimega "kõver" muutujana. Lõpuks saadakse intressi aega tõenäosuse. 

###<a name="experiment-flow"></a>Katse flow:

![katsetamiseks meilivoo][1]

####<a name="module-1"></a>Moodul 1:

    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"
    
    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data to output port
    
####<a name="module-2"></a>Moodul 2:

    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare to build model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct the execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get the predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find the event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results to send to web service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




##<a name="limitations"></a>Piirangud

See veebiteenus saate teha ainult arvväärtusi nimega funktsiooni muutujate (veerge). Veeru "sündmus" saate teha ainult väärtuse 0 või 1. Veeru "time" peab olema positiivne täisarv.

##<a name="faq"></a>KKK
Korduma kippuvad küsimused tarbimine veebiteenuse või avaldamine teenuses Azure turuplatsilt, leiate [siit](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
