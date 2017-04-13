<properties 
    pageTitle="Sõnastik vastavalt meeleolu analüüsi | Microsoft Azure'i" 
    description="Sõnastik vastavalt meeleolu analüüs" 
    services="machine-learning" 
    documentationCenter="" 
    authors="pengxia" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="pengxia"/> 



#<a name="lexicon-based-sentiment-analysis"></a>Sõnastik vastavalt meeleolu analüüs 

Kuidas saab mõõta kasutajate arvamusi ja suhtumist kaubamärkide või teemade Interneti suhtlusvõrgud, nagu Facebookis postitab, tweets, kommentaare, jne.? Meeleolu analüüsi annab meetodi analüüsimiseks küsimusi.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Üldiselt on kaks võimalust meeleolu analüüsi jaoks. Üks kasutab kuuluva õ algoritmi ja teine võib käsitleda kui järelevalveta õppimine. Kuuluva õ algoritmi üldiselt põhineb liigitamine mudeli suure selgitustega corpus. Täpsuse põhineb peamiselt marginaali kvaliteeti ja tavaliselt koolitus protsess võtab kaua aega. Peale selle, kui me rakendada algoritmi lisadomeeni, tulemus on tavaliselt pole hea. Võrreldes kontrollitud õppimisega, sõnastik järelevalveta õ kasutab meeleolu sõnastiku, mis ei nõua suurte andmete corpus salvestamise ja koolitus – mis muudab kogu protsessi kiiremaks. 

Meie [teenus](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) on ehitatud MPQA subjektiivsus sõnastik (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), mis on üks kõige sagedamini kasutatav subjektiivsus leksikonid. MPQA on negatiivne ja 2533 5097 positiivne sõna. Ja kõik need sõnad on märge tugev või nõrk polaarsus nimega. Kogu korpus on GNU Üldine avalik litsents. Veebiteenuse saab rakendada lühike laused, nt tweets ja Facebooki postitusi. 

>See veebiteenus võib tarbitud kasutajaid – potentsiaalselt mobiilirakenduse kaudu veebisaidi või isegi kohalikus arvutis näiteks. Kuid veebiteenuse eesmärk on ka olla näide sellest, kuidas saab kasutada Azure seadme õ veebiteenuste peal R-koodi loomiseks. Vaid mõned koodiread R ja klõpsu nuppudel Azure seadme õ Studio, saate katse loodud R koodi ja veebiteenuse avaldatud. Seejärel saate veebiteenuse avaldatud Azure'i turuplatsi ja tarbitud kasutajatele ja seadmetele kogu maailmas pole taristu häälestamine veebiteenuse autori.

##<a name="consumption-of-web-service"></a>Veebiteenuse tarbimine

Sisendandmete võib olla mis tahes teksti, kuid veebiteenuse töötab paremini lühikesi lauseid. Väljund on arvulise väärtuse -1 ja 1 vahel. Mis tahes väärtus 0 all – näitab, et teksti meeleolu on negatiivne; positiivne kui kohal 0. Tulemuse absoluutväärtus tähistab seotud meeleolu tugevus. 

>See teenus majutatud Azure'i turuplatsi, on teenuse OData; need võivad nn POST või GET meetodite abil. 

On mitu võimalust tarbimine automatiseeritud viisil teenuse (nt rakendus on [siin](http://microsoftazuremachinelearning.azurewebsites.net/)).

###<a name="starting-c-code-for-web-service-consumption"></a>C# koodi web teenuse tarbimine alates:

    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();
    
                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



On "Täna on hea päev." Väljund on "1", mis näitab positiivne tunne seostatud Sisestuskeel lause. 

##<a name="creation-of-web-service"></a>Veebiteenuse loomine
>See veebiteenus loodi Azure seadme õ. Tasuta prooviversioon, samuti sissejuhatavad videod katsete ja [avaldamise veebiteenuste](machine-learning-publish-a-machine-learning-web-service.md)loomise kohta, vt [azure.com/ml](http://azure.com/ml). Allpool on loodud web teenuse ja näiteks koodi iga katse jooksul moodulid katse kuvatõmmis.


Azure'i masina õ, sees uus tühi katse on loodud. Joonis näitab sõnastik põhinev meeleolu analüüsi katse liikumise. "Sent_dict.csv" fail on MPQA subjektiivsus sõnastik ja on seatud ühte [Käivitada R skripti]sisendeid[execute-r-script]. Mõne muu sisestusmeetodi on test, kus oleme tehtud valikut veeru nime muutmist, ja tükeldamine toimingute Amazon läbivaatus andmekomplekti valimisse ülevaade. Kasutame räsi paketi salvestada subjektiivsus sõnastik mälu ja Keskmine arvutus kiirendada. Kogu tekst kuvatakse tokenized "tm" paketi ja võrreldakse meeleolu sõnastikust sõna. Lõpuks arvutatakse on keskmine, lisades iga subjektiivne sõna paksuse tekst. 

###<a name="experiment-flow"></a>Katse flow:

![katsetamiseks meilivoo][2]


####<a name="module-1"></a>Moodul 1:
    
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame
 
   # <a name="install-hash-package"></a>Installige Räsi pakett
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }
          
        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")
    


##<a name="limitations"></a>Piirangud

Algoritmi seisukohalt meeleolu sõnastik põhinev analüüs on üldist analüüsi tööriist, mis võib teha paremini liigitamine meetod kindlad väljad. Negatiivne arv probleemi ei ka tegelda. Me hardcode mitu negatiivne arv sõnad meie programmi, kuid paremini on negatiivne arv sõnastiku kasutamise ja koostada mõned reeglid. Veebiteenuse sooritab paremini lühike ja lihtne laused, nt tweets ja Facebooki postitusi, kui pikk ja keerukas laused, nt Amazon arvustuste kohta. 

##<a name="faq"></a>KKK
Korduma kippuvad küsimused tarbimine veebiteenuse või avaldamine teenuses Azure turuplatsilt, leiate [siit](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

 
