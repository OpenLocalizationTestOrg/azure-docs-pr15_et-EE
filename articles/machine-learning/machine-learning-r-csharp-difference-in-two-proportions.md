<properties 
    pageTitle="Vahe proportsioone Test | Microsoft Azure'i" 
    description="Vahe proportsioone Test" 
    services="machine-learning" 
    documentationCenter="" 
    authors="aniedea" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="aniedea"/> 


#<a name="difference-in-proportions-test"></a>Vahe proportsioone Test


Kahe proportsioone statistiliselt erinevad? Oletame, et kasutaja soovib võrdlemiseks kaks filmi kindlaks teha, kui üks filmi on oluliselt suurema osa "meeldib" Millal võrreldes teiste. Suure valimi, mis võib olla statistiliselt vahe proportsioone 0,50 ja 0,51. Väike näidise, seal ei pruugi olla piisavalt andmeid, et otsustada, kui need proportsioonid on tegelikult erinevad. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

See [veebiteenus]( https://datamarket.azure.com/dataset/aml_labs/prop_test) teostab juhul testi erinevust kahe proportsioone põhineb kasutaja sisendist teatud omadustega elementide arv ja 2 võrdlus rühmade katsete arv. Üks võimalik stsenaariumi, võib see veebiteenus kutsuda kaudu filmi võrdlus rakenduse teade kasutaja, kas üks filmi on väga "meeldis" sagedamini, kui tagasiside põhjal.

>See veebiteenus võib tarbitud kasutajaid – potentsiaalselt mobiilirakenduse kaudu veebisaidi või isegi kohalikus arvutis, näiteks. Kuid veebiteenuse eesmärk on ka olla näide sellest, kuidas saab kasutada Azure seadme õ veebiteenuste peal R-koodi loomiseks. Vaid mõned koodiread R ja klõpsu nuppudel Azure seadme õ Studio, saate katse loodud R koodi ja veebiteenuse avaldatud. Seejärel saate veebiteenuse avaldatud Azure'i turuplatsi ja tarbitud kasutajatele ja seadmetele kogu maailmas pole taristu häälestamine veebiteenuse autori.


##<a name="consumption-of-web-service"></a>Veebiteenuse tarbimine

See teenus aktsepteerib 4 argumendid ja ei saanud juhul testida proportsioonid.

On sisestatud argumente.

* Successes1 - valimi 1 edu sündmuste arv.
* Successes2 - valimi 2 edu sündmuste arv.
* Total1 - 1 valimi suurus.
* Total2 - 2 valimi suurus.

Teenuse väljund on tulemus, et testida koos chi-square statistiline, df, p-väärtuse ja osa on valimi 1 ja 2 ning usaldusvahemiku piirid.

>See teenus majutatud Azure'i turuplatsi, on teenuse OData; need võivad nn POST või GET meetodite abil. 

On mitu võimalust tarbimine automatiseeritud viisil teenuse (nt rakendus on [siin](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C# koodi web teenuse tarbimine alates:

    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


##<a name="creation-of-web-service"></a>Veebiteenuse loomine

>See veebiteenus loodi Azure seadme õ. Tasuta prooviversioon, samuti sissejuhatavad videod katsete ja [avaldamise veebiteenuste](machine-learning-publish-a-machine-learning-web-service.md)loomise kohta, vt [azure.com/ml](http://azure.com/ml). Allpool on loodud web teenuse ja näiteks koodi iga katse jooksul moodulid katse kuvatõmmis.

Sees Azure seadme õ, uue tühja katse on loodud koos kahe [Käivitada R skripti] [ execute-r-script] moodulid. Andmete skeemi on määratletud esimese mooduli ajal teine moodul kasutab prop.test käsku sees R juhul testimiseks jaoks 2 proportsioone. 


###<a name="experiment-flow"></a>Katse flow:

![Katse meilivoo][2]


####<a name="module-1"></a>Moodul 1:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data to output port
    dataset1 = maml.mapInputPort(1) #read data from input port
    

####<a name="module-2"></a>Moodul 2:

    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"The proportions are different!",
    "The proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port
    

##<a name="limitations"></a>Piirangud 

See on väga lihtne näide testi 2 proportsioone vahe. Nagu näha Ülaltoodud näites koodi, rakendatakse tõrge püüdmine ja teenuse eeldab, et kõiki muutujaid on pidev.

##<a name="faq"></a>KKK
Korduma kippuvad küsimused tarbimine veebiteenuse või avaldamine teenuses Azure turuplatsilt, leiate [siit](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
