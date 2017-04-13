<properties 
    pageTitle="Normaliseeritud normaaljaotuse Web teenuse komplekti | Microsoft Azure'i" 
    description="Normaliseeritud normaaljaotuse Web Service Suite" 
    services="machine-learning" 
    documentationCenter="" 
    authors="ireiter" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="ireiter"/> 

#<a name="normal-distribution-suite"></a>Normaliseeritud normaaljaotuse komplekti



Normaliseeritud normaaljaotuse Suite on valimi veebiteenuste ([generaator]( https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile kalkulaator]( https://datamarket.azure.com/dataset/aml_labs/ndq5), [Tõenäosuse kalkulaator]( https://datamarket.azure.com/dataset/aml_labs/ndp5)), mis aitavad loomise ja töötlemise tavaline jaotuse. Teenuste luba genereerimine normaaljaotuse jada pikkusega, quantiles kaudu antud tõenäosuse arvutamiseks ja arvutamise tõenäosuse antud quantile kaudu. Kõigi teenuste eraldab eri väljundeid vastavalt valitud (vt allpool kirjeldus). Normaliseeritud normaaljaotuse komplekti põhineb R funktsioonide qnorm, rnorm ja pnorm, mis sisalduvad R statistika paketi.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>See veebiteenus võib tarbitud kasutajaid – potentsiaalselt mobiilirakenduse kaudu veebisaidi või isegi kohalikus arvutis, näiteks. Kuid veebiteenuse eesmärk on ka olla näide sellest, kuidas saab kasutada Azure seadme õ veebiteenuste peal R-koodi loomiseks. Vaid mõned koodiread R ja klõpsu nuppudel Azure seadme õ Studio, saate katse loodud R koodi ja veebiteenuse avaldatud. Seejärel saate veebiteenuse avaldatud Azure'i turuplatsi ja tarbitud kasutajatele ja seadmetele kogu maailmas pole taristu häälestamine veebiteenuse autori.  
 

##<a name="consumption-of-web-service"></a>Veebiteenuse tarbimine
Normaliseeritud normaaljaotuse Suite sisaldab järgmisi 3 teenuseid.

###<a name="normal-distribution-quantile-calculator"></a>Normaliseeritud normaaljaotuse Quantile kalkulaator
See teenus aktsepteerib 4 argumenti on normaaljaotusega ja arvutab seotud quantile.

On sisestatud argumente.

* p - ühe tõenäosuse jaotusfunktsiooni abil ürituse. 
* Keskmine – normaliseeritud normaaljaotuse keskmise.
* SD - normaliseeritud normaaljaotuse standardhälbe. 
* Side - L jaotuse alumises servas ja U ülemise servas jaotuse.

Teenuse väljund on arvutatud quantile, mis on seotud antud tõenäosus.

###<a name="normal-distribution-probability-calculator"></a>Normaliseeritud normaaljaotuse tõenäosuse kalkulaator
See teenus aktsepteerib 4 argumenti on normaaljaotusega ja arvutab seotud tõenäosus.

On sisestatud argumente.

* k-ühe quantile normaaljaotuse sündmus. 
* Keskmine – normaliseeritud normaaljaotuse keskmise.
* SD - normaliseeritud normaaljaotuse standardhälbe. 
* Side - L jaotuse alumises servas ja U ülemise servas jaotuse.

Teenuse väljund on arvutatud tõenäosus, mis on seotud antud quantile.

###<a name="normal-distribution-generator"></a>Normaliseeritud normaaljaotuse generaator
See teenus aktsepteerib 3 argumenti on normaaljaotusega ja genereeritud juhusliku arvud, mis on normaaljaotusega jada. Järgmised argumendid tuleb see jooksul taotluse:

* n - vaatluste arv. 
* Keskmine – normaliseeritud normaaljaotuse keskmise.
* SD - normaliseeritud normaaljaotuse standardhälbe. 

Teenuse väljund on normaaljaotusega argumentide keskväärtuse ja sd pikkus n jada.

>See teenus majutatud Azure'i turuplatsi, on teenuse OData; need võivad nn POST või GET meetodite abil. 

On mitu võimalust tarbimine automatiseeritud viisil teenuse (nt rakendused on siin: [generaator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [Tõenäosuse kalkulaator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Quantile kalkulaator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>C# koodi web teenuse tarbimine alates:

###<a name="normal-distribution-quantile-calculator"></a>Normaliseeritud normaaljaotuse Quantile kalkulaator
    public class Input
    {
            public string p;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { p = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="normal-distribution-probability-calculator"></a>Normaliseeritud normaaljaotuse tõenäosuse kalkulaator
    public class Input
    {
            public string q;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="normal-distribution-generator"></a>Normaliseeritud normaaljaotuse generaator
    public class Input
    {
            public string n;
            public string mean;
            public string sd;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text };
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
>See veebiteenus loodi Azure seadme õ. Tasuta prooviversioon, samuti sissejuhatavad videod katsete ja [avaldamise veebiteenuste](machine-learning-publish-a-machine-learning-web-service.md)loomise kohta, vt [azure.com/ml](http://azure.com/ml). 

Allpool on loodud web teenuse ja näiteks koodi iga katse jooksul moodulid katse kuvatõmmis.

###<a name="normal-distribution-quantile-calculator"></a>Normaliseeritud normaaljaotuse Quantile kalkulaator

Katse flow:

![Katse meilivoo][2]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }
    q = qnorm(param$p,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    q = 2* param$mean - q
    } else if (param$side =='L') {
    q = q
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(q)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-probability-calculator"></a>Normaliseeritud normaaljaotuse tõenäosuse kalkulaator
Katse flow:

![Katse meilivoo][3]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    prob = pnorm(param$q,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    prob = 1 - prob
    } else if (param$side =='B') {
    prob = ifelse(prob<=0.5,prob * 2, 1)
    } else if (param$side =='L') {
    prob = prob
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-generator"></a>Normaliseeritud normaaljaotuse generaator
Katse flow:

![Katse meilivoo][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Piirangud 

Need on väga lihtsa ümbritseva normaliseeritud normaaljaotuse näited. Nagu näha Ülaltoodud näites koodi, väike tõrge püüdmine rakendatakse.

##<a name="faq"></a>KKK
Korduma kippuvad küsimused tarbimine veebiteenuse või avaldamine teenuses Azure turuplatsilt, leiate [siit](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png
 
