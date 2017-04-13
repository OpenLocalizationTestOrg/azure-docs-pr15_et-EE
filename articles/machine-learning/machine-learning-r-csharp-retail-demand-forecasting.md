<properties 
    pageTitle="Prognoosi koostamiseks - ETS + STL | Microsoft Azure'i" 
    description="Prognoosi koostamiseks - ETS + STL" 
    services="machine-learning" 
    documentationCenter="" 
    authors="xueshanz" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="yijichen"/> 

#<a name="forecasting---ets--stl"></a>Prognoosi koostamiseks - ETS + STL  

See [veebiteenus]( https://datamarket.azure.com/dataset/aml_labs/demand_forecast) rakendab hooajaline Trend lagunemine (STL) ja Eksponentsilumise (ETS) mudelite koostada prognoose, mille kasutaja ajalooliste andmete põhjal. Mõne kindla toote nõudmisel suurendab see aasta? Saate ma prognoosida minu toote müügi jaoks jõulude nii, et ma saate tõhusaks kavandamiseks minu varude? Prognoosimise mudelid on sobivad, et sellist küsimusi. Antud viimase andmeid, nende mudelite läbi peidetud trende ja prognoosida tulevaste trendide hooajalisus. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
 
>See veebiteenus võib tarbitud kasutajaid – potentsiaalselt mobiilirakenduse kaudu veebisaidi või isegi kohalikus arvutis, näiteks. Kuid veebiteenuse eesmärk on ka olla näide sellest, kuidas saab kasutada Azure seadme õ veebiteenuste peal R-koodi loomiseks. Vaid mõned koodiread R ja klõpsu nuppudel Azure seadme õ Studio, saate katse loodud R koodi ja veebiteenuse avaldatud. Seejärel saate veebiteenuse avaldatud Azure'i turuplatsi ja tarbitud kasutajatele ja seadmetele kogu maailmas pole taristu häälestamine veebiteenuse autori.  
 
##<a name="consumption-of-web-service"></a>Veebiteenuse tarbimine 

See teenus aktsepteerib 4 argumendid ja arvutab prognooside.
On sisestatud argumente.

* Sagedus - näitab sagedus toorandmetega (iga päev/nädala/kuu/kvartali/aasta).
* Horisont - tulevikus prognoos aja.
* Kuupäev – uue aja sarja andmeid lisada.
* Väärtus - lisada uue aja sarja andmete väärtused.

Teenuse väljund on prognoositud arvutatud väärtused.
 
Valimi sisendit võib olla: 

* Sagedus – 12
* Horisont – 12
* Kuupäeva - 1/15/2012 2/15/2012; 3/15/2012; 4/15/2012; 5/15/2012; 6/15/2012; 7/15/2012; 8 / 15/2012 9/15/2012; 10/15/2012; 11/15/2012; 12/15/2012; 1/15/2013 2/15/2013; 3/15/2013; 4/15/2013; 5/15/2013; 6/15/2013; 7/15/2013; 8 / 15/2013 9/15/2013; 10/15/2013; 11/15/2013; 12/15/2013; 1/15/2014 2/15/2014; 3/15/2014; 4/15/2014; 5/15/2014; 6/15/2014; 7/15/2014; 8 / 15/2014; 9/15/2014
* Value - 3,479; 3,68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3,65; 3.709; 3.682; 3.511; 3.429 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509

>See teenus majutatud Azure'i turuplatsi, on teenuse OData; need võivad nn POST või GET meetodite abil. 

On mitu võimalust tarbimine automatiseeritud viisil teenuse (nt rakendus on [siin](http://microsoftazuremachinelearning.azurewebsites.net/StlEtsForecasting.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C# koodi web teenuse tarbimine alates:

    public class Input
    {
            public string frequency;
            public string horizon;
            public string date;
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };         var json = JsonConvert.SerializeObject(input);
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

Azure'i masina õ, sees uus tühi katse on loodud. Valimi sisendandmete on üles laaditud eelmääratletud andmete skeem. Lingitud andmete skeemiga on mõne [Käivitada R skripti] [ execute-r-script] moodul, mis loob STL ja ETS prognoosi koostamiseks mudelite abil stl, "ets" ja 'forecast' töötab: R. 

###<a name="experiment-flow"></a>Katse flow:

![Katse meilivoo][2]

####<a name="module-1"></a>Moodul 1:
 
    # Add in the CSV file with the data in the format shown below 
![Näidisandmed][3]   

####<a name="module-2"></a>Moodul 2:

    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)
    
    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]
    
    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)
    
    # Fit a time series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- stl(train_ts,  s.window="periodic")
    train_model <- forecast(fit1, h = data$horizon, method = 'ets')
    plot(train_model)
    
    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")
    
    # Data output
    maml.mapOutputPort("data.forecast");

##<a name="limitations"></a>Piirangud 

See on väga lihtne näide ETS + STL prognoosi koostamiseks. Nagu näha Ülaltoodud näites koodi, rakendatakse tõrge püüdmine ja teenuse eeldatakse, et kõiki muutujaid on pidev/positiivsed väärtused ja sagedus peaks olema täisarv, mis on suurem kui 1. Kuupäeva- ja väärtuse vektorite pikkus peaks olema sama ja kellaaja sarja pikkus peab olema suurem kui 2 * sagedus. Kuupäeva muutuja peaksite järgima vorming KK/PP/AAAA.

##<a name="faq"></a>KKK
Korduma kippuvad küsimused tarbimine veebiteenuse või avaldamine teenuses Azure turuplatsilt, leiate [siit](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img1.png
[2]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img2.png
[3]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
