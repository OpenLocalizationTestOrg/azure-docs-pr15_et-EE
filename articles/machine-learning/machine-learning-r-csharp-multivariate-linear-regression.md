<properties 
    pageTitle="Mitme muutujaga lineaarregressioon | Microsoft Azure'i" 
    description="Mitme muutujaga lineaarregressioon" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jaymathe" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="jaymathe"/> 


#<a name="multivariate-linear-regression"></a>Mitme muutujaga lineaarregressioon   
 

 
Oletame, et teil on andmekomplekt ja soovite kiiresti prognoosida sõltuv muutuja (y) iga üksiku (i) põhjal sõltumatu muutuja. Lineaarse regressioonisirge on kasutada selliseid prognoose populaarsed statistika meetod. Siin on sõltuv muutuja y eeldatakse, et pidev väärtus.  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

Lihtne stsenaarium võib olla, kus teadlane üritab prognoosida paksuse inimesel (y) vastavalt nende kõrgus (x). Täpsemate stsenaarium võib olla kus teadlane sisaldab täiendavat teavet (nt kaal, sugu, võistluse) üksikute ja avaldab prognoosida üksikute paksuse. See [veebiteenus]( https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) sobib lineaarse regressioonisirge andmemudeli andmetele ja väljundi prognoositud väärtus (y) iga vaatluste andmeid.

>See veebiteenus võib tarbitud kasutajaid – potentsiaalselt mobiilirakenduse kaudu veebisaidi või isegi kohalikus arvutis, näiteks. Kuid veebiteenuse eesmärk on ka olla näide sellest, kuidas saab kasutada Azure seadme õ veebiteenuste peal R-koodi loomiseks. Vaid mõned koodiread R ja klõpsu nuppudel Azure seadme õ Studio, saate katse loodud R koodi ja veebiteenuse avaldatud. Seejärel saate veebiteenuse avaldatud Azure'i turuplatsi ja tarbitud kasutajatele ja seadmetele kogu maailmas pole taristu häälestamine veebiteenuse autori.  

##<a name="consumption-of-web-service"></a>Veebiteenuse tarbimine  
See veebiteenus annab ennustatud väärtuste põhjal sõltumatu muutuja kõigi vaatluste sõltuv muutuja. Veebiteenuse eeldab, et lõppkasutaja andmeid sisestada stringina, kus read on eraldatud komaga (,) ja veerud on semikoolonitega (;). Veebiteenuse eeldab, et rida 1 korraga ja eeldab, et esimene veerg olema sõltuv muutuja. Mõni näide andmekomplekti võib välja näha selline:

![Näidisandmed][1]

Vaatluste ilma sõltuv muutuja tuleks sisestada "NA" y. Sisestada Ülaltoodud andmehulga oleks järgmine string andmeid: "10; 5; 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2; 1, NA; 3; 4". Väljund on prognoositud väärtus iga sõltumatu muutuja põhjal read. 

>See teenus majutatud Azure'i turuplatsi, on teenuse OData; need võivad nn POST või GET meetodite abil. 

On mitu võimalust tarbimine automatiseeritud viisil teenuse (nt rakendus on [siin](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C# koodi web teenuse tarbimine alates:

    public class Input
    {
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
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


Azure'i masina õ, sees uus tühi katse on loodud ja kahe [Käivitada R skripti] [ execute-r-script] moodulid olid tõmmatakse peale tööruumi. See veebiteenus töötab Azure seadme õ katse on aluseks R skripti. 2 osast, et see katse: skeemi määratluse ja koolitus mudeli + hinded. Esimese mooduli määratleb Sisestuskeel andmekomplekti, kus esimene muutuja on sõltuv muutuja ja ülejäänud muutujad sõltumatu oodatud struktuuri. Teine moodul sobib sisendandmete üldise lineaarse regressioonisirge mudel.  
  
![Katse meilivoo][3]

####<a name="module-1"></a>Moodul 1:
 
####<a name="schema-definition"></a>Skeemi määratlus  
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

####<a name="module-2"></a>Moodul 2:
####<a name="lm-modeling"></a>LM modelleerimine   
    data <- maml.mapInputPort(1) # class: data.frame  
  
    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  
 
##<a name="limitations"></a>Piirangud
See on väga lihtne näide mitme lineaarse regressioonisirge veebiteenusest. Nagu näha Ülaltoodud näites koodi, rakendatakse tõrge püüdmine ja teenuse eeldab pidev muutuja (kategooriline funktsioone pole lubatud), on kõik selle teenuse ainult sisendina arvväärtuste selle veebiteenuse loomise ajal. Teenuse praegu töötleb ka piiratud andmete maht, tähtaeg taotluse/vastuse laadi veebiteenuse kutses ja mudeli on ei mahu iga kord selle veebiteenuse nimetatakse asjaolu. 

##<a name="faq"></a>KKK
Korduma kippuvad küsimused tarbimine veebiteenuse või avaldamine teenuses Azure turuplatsilt, leiate [siit](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
