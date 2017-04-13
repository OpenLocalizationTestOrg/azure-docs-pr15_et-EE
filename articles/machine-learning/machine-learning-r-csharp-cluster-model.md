<properties 
    pageTitle="Mudeli klaster | Microsoft Azure'i" 
    description="Mudeli kobar" 
    services="machine-learning" 
    documentationCenter="" 
    authors="FrancescaLazzeri" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="lazzeri"/> 


#<a name="cluster-model"></a>Mudeli kobar    

Kuidas me prognoosida rühmad krediidi limiidid käitumise vähendamiseks tasuta välja krediitkaardi emitentide? Kuidas me Määratle rühmad isik tunnuste töötajate tööl oma jõudluse parandamiseks? Kuidas arstide liigitada patsientide rühmadesse nende haiguste tunnuste? Põhimõtteliselt saab kõik neile küsimustele vastata kobar analüüsi kaudu.   


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
   
Kobar analüüsi jaotab vaatluste rühmadesse kahe või enama üksteist välistavad tundmatu põhinev muutujate kogum. Kobar analüüsi eesmärk on avastada süsteemi korraldamine vaatluste, tavaliselt inimesed või omaduste, rühmadesse, kus rühma liikmete jagada atribuudid ühist. See [teenus](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) kasutab K-tähendab meetod, levinumaid klastrite tehnika kobar suvalise andmetega rühmadesse. See veebiteenus võtab andmed ja kogumite sisendina ja annab tulemiks prognoose, mille k rühmad, kuhu kuulub iga vaatluste arv k. 

>See veebiteenus võib tarbitud kasutajaid – potentsiaalselt mobiilirakenduse kaudu veebisaidi või isegi kohalikus arvutis, näiteks. Kuid veebiteenuse eesmärk on ka olla näide sellest, kuidas saab kasutada Azure seadme õ veebiteenuste peal R-koodi loomiseks. Vaid mõned koodiread R ja klõpsu nuppudel Azure seadme õ Studio, saate katse loodud R koodi ja veebiteenuse avaldatud. Seejärel saate veebiteenuse avaldatud Azure'i turuplatsi ja tarbitud kasutajatele ja seadmetele kogu maailmas pole taristu häälestamine veebiteenuse autori.  

##<a name="consumption-of-web-service"></a>Veebiteenuse tarbimine   
See veebiteenus k rühmade kogumi arvestuses rühmitab andmed ja väljundi rühmakuuluvus iga rea kohta. Veebiteenuse eeldab, et lõppkasutaja andmeid sisestada stringina, kus read on eraldatud komaga (,) ja veerud on semikoolonitega (;). Veebiteenuse eeldab, et korraga 1 rida. Mõni näide andmekomplekti võib välja näha selline:

![Näidisandmed][1]

Oletame, et kasutaja kalendriüksusi andmed omaette 3 üksteist välistavad rühmad. Sisestada Ülaltoodud andmehulga oleks järgmine andmeid: väärtus = "10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3". Väljund on prognoositud rühmakuuluvust iga read.

>See teenus majutatud Azure'i turuplatsi, on teenuse OData; need võivad nn POST või GET meetodite abil. 

On mitu võimalust tarbimine automatiseeritud viisil teenuse (nt rakendus on [siin](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C# koodi web teenuse tarbimine alates:

    public class Input
    {
            public string value;
            public string k;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
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

Azure'i masina õ, sees uus tühi katse on loodud ja kahe [Käivitada R skripti] [ execute-r-script] moodulid tõmmatakse peale tööruumi. Andmete skeemi loodi [R skripti käivitada]lihtsa[execute-r-script]. Klõpsake andmete skeemiga seotud uuesti [Käivitada R Script]on loodud jaotises kobar mudel[execute-r-script]. [R skripti käivitada] [ execute-r-script] kasutatakse kobar mudeli, veebiteenuse seejärel kasutab funktsiooni "k-tähendab", mis on Varemkoostatud [Käivitada R skripti] [ execute-r-script] Azure seadme õppe.    
   

     
![Katse meilivoo][3]

####<a name="module-1"></a>Moodul 1: 
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)
    
    maml.mapOutputPort("mydata");     
    

####<a name="module-2"></a>Moodul 2:
    # Map 1-based optional input ports to variables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");
   
 
##<a name="limitations"></a>Piirangud
See on väga lihtne näide klastrite veebiteenusest. Nagu näha Ülaltoodud näites koodi, rakendatakse tõrge püüdmine ja teenuse eeldab pidev muutuja (kategooriline funktsioone pole lubatud), on kõik selle teenuse ainult sisendina arvväärtuste selle veebiteenuse loomise ajal. Teenuse praegu töötleb ka piiratud andmete maht, tähtaeg taotluse/vastuse laadi veebiteenuse kutses ja mudeli on ei mahu iga kord selle veebiteenuse nimetatakse asjaolu. 

##<a name="faq"></a>KKK
Korduma kippuvad küsimused tarbimine veebiteenuse või avaldamine teenuses Azure turuplatsilt, leiate [siit](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
