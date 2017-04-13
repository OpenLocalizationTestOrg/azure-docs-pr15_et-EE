<properties
    pageTitle="Siit saate teada, kuidas hallata AzureML veebiteenuste API haldusega | Microsoft Azure'i"
    description="Näitab, kuidas hallata AzureML veebiteenuste API haldusega juhendit."
    keywords="seadme Õppekeskuse, api haldus"
    services="machine-learning"
    documentationCenter=""
    authors="roalexan"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="roalexan" />


# <a name="learn-how-to-manage-azureml-web-services-using-api-management"></a>Siit saate teada, kuidas hallata AzureML veebiteenuste API haldusega

##<a name="overview"></a>Ülevaade

Sellest juhendist näitab, kuidas kiiresti alustada API halduse abil saate hallata oma AzureML veebiteenused.

##<a name="what-is-azure-api-management"></a>Mis on Azure API haldus?

Azure'i API haldus on Azure'i teenus, mille abil saate hallata oma REST API lõpp-punktid, määratledes kasutajate juurdepääsu, kasutuse pidurdamise ja armatuurlaua jälgimine. Klõpsake [siin](https://azure.microsoft.com/services/api-management/) Azure'i API haldamise kohta. Klõpsake [siin](../api-management/api-management-get-started.md) juhised, kuidas alustada Azure'i API halduse jaoks. Muud juhendi, mis põhineb juhendi, hõlmab rohkem teemasid, sh teatis konfiguratsioone, taseme hinnad, reageerimine, kasutaja autentimine, toodete, arendaja tellimused ja kasutus dashboarding loomine.

##<a name="what-is-azureml"></a>Mis on AzureML?

AzureML on Azure'i teenus Õppekeskuse arvutisse, mis võimaldab teil hõlpsasti koostada, juurutada ja täiustatud analüüsi lahenduste ühiskasutus. Klõpsake [siin](https://azure.microsoft.com/services/machine-learning/) AzureML kohta.

##<a name="prerequisites"></a>Eeltingimused

Sellest juhendist tegemiseks vajate:

* Azure'i konto. Kui teil pole Azure'i konto, klõpsake [siin](https://azure.microsoft.com/pricing/free-trial/) täpsemat teavet tasuta prooviversiooni konto loomiseks.
* Konto AzureML. Kui teil pole kontot AzureML, klõpsake [siin](https://studio.azureml.net/) täpsemat teavet tasuta prooviversiooni konto loomiseks.
* Tööruumi, teenuse ja api_key juurutatud veebiteenuse AzureML katse jaoks. Klõpsake [siin](machine-learning-create-experiment.md) üksikasjalikku teavet loomise katse AzureML. Klõpsake [siin](machine-learning-publish-a-machine-learning-web-service.md) AzureML katse nimega veebiteenuse juurutamise kohta. Vaheldumisi lisa A on juhised, kuidas luua ja testida lihtsa AzureML katse ja juurutada veebiteenusest.

##<a name="create-an-api-management-instance"></a>API halduse eksemplari loomine

Allpool on juhised API halduse abil saate hallata oma AzureML veebiteenus. Esmalt Looge teenuse eksemplari. [Klassikaline portaali](https://manage.windowsazure.com/) sisse logida ja klõpsake nuppu **Uus** > **Rakenduse teenuste** > **API halduse** > **loomine**.

![eksemplari loomine](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

Määrake kordumatu **URL-i**. Sellest juhendist kasutab **demoazureml** – teil tuleb valida midagi muud. Valige soovitud **tellimus** ja **regiooni** jaoks oma eksemplari. Pärast soovitud valikud, klõpsake nuppu edasi.

![Looge teenuse 1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

Määrake väärtus **Ettevõtte nimi**. Sellest juhendist kasutab **demoazureml** – teil tuleb valida midagi muud. Sisestage oma meiliaadress **administraatori** e-post. Selle e-posti aadressi kasutatakse API halduse süsteemist teatised.

![Looge teenuse 2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

Märkige ruut luua oma eksemplari. *Kulub luuakse uus teenus kuni 30 minutit*.

##<a name="create-the-api"></a>API loomine

Kui eksemplari on loodud, on järgmiseks API loomiseks. API koosneb toimingute kohta, mida saate kasutada kliendi rakendus. Olemasoleva web Services puhverdatud on API toimingud. Sellest juhendist loob API-de selle puhverserver olemasoleva AzureML RRS ja BES veebiteenused.

API-d on loodud ja API Publisheri portaalis Azure klassikaline portaali kaudu avatavas konfigureeritud. Valige Publisheris portaali jõuda oma eksemplari.

![Valige eksemplari](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

Klõpsake nuppu **Halda** API halduse teenust Azure klassikaline portaalis.

![teenuse haldamine](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

Klõpsake vasakul menüüst **API halduse** **API-de** ja klõpsake **API lisada**.

![API-halduse-menüü](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

Tippige **AzureML Demo API** **Web API nimi**. Tippige **https://ussouthcentral.services.azureml.net** **veebiteenuse URL**. Tippige **azureml-demo** **Web API URL-i järelliite**. Märkige **HTTPS** värviskeemi **Web API URL-i** . Valige **Starter** **tooted**. Kui olete lõpetanud, klõpsake nuppu **Salvesta** API loomiseks.

![Lisage-uue-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

##<a name="add-the-operations"></a>Toimingute lisamine

Klõpsake nuppu **Lisa toiming** selle API toimingute lisamiseks.

![Lisa toiming](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

Kuvatakse aken **uue toiming** ja vaikimisi valitakse menüü **allkiri** .

##<a name="add-rrs-operation"></a>Lisage RRS toiming

Esmalt looge AzureML RRS teenuse eest. Valige **POSTITA** **HTTP-Verbi**. Tippige **/services/ /workspaces/ {tööruumi} {teenuse} / execute?api versiooni {apiversion} = & üksikasjad = {üksikasjad}** **malli URL-i**. Tippige **RRS käivitada** **kuvatav nimi**.

![Lisage-rrs-toiming-allkiri](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Klõpsake **vastuste** > **lisamine** vasakul ja valige **200 OK**. Klõpsake nuppu **Salvesta** see toiming salvestamiseks.

![Lisage-rrs-toiming-vastus](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

##<a name="add-bes-operations"></a>BES lisamistoimingute

Kuvatõmmised ei sisalda BES toimingute, nagu need on väga sarnased lisamise RRS toiming.

###<a name="submit-but-not-start-a-batch-execution-job"></a>Esitage (kuid mitte start) paketi täitmise töö

Klõpsake **toimingu lisada** API AzureML BES toimingu lisamiseks. Valige **POSTITA** **HTTP-Verbi**. Tippige **/services/ /workspaces/ {tööruumi} {teenuse} / jobs?api versiooni = {apiversion}** **malli URL-i**jaoks. Tippige **BES esitada** **kuvatav nimi**. Klõpsake **vastuste** > **lisamine** vasakul ja valige **200 OK**. Klõpsake nuppu **Salvesta** see toiming salvestamiseks.

###<a name="start-a-batch-execution-job"></a>Paketi täitmise töö alustamine

Klõpsake **toimingu lisada** API AzureML BES toimingu lisamiseks. Valige **POSTITA** **HTTP-Verbi**. Tippige **/workspaces/ {tööruumi} {teenuse} /services/ /jobs/ {jobid} / start?api versiooni = {apiversion}** **malli URL-i**jaoks. Tippige **BES Start** **kuvatav nimi**. Klõpsake **vastuste** > **lisamine** vasakul ja valige **200 OK**. Klõpsake nuppu **Salvesta** see toiming salvestamiseks.

###<a name="get-the-status-or-result-of-a-batch-execution-job"></a>Oleku või tulemi paketi täitmise töö.

Klõpsake **toimingu lisada** API AzureML BES toimingu lisamiseks. **HTTP-Verbi,**valige **saada** . Tippige **/workspaces/ {tööruumi} /services/ {teenuse} {jobid} /jobs/ ?api versiooni = {apiversion}** **malli URL-i**jaoks. Tippige **BES olek** **kuvatav nimi**. Klõpsake **vastuste** > **lisamine** vasakul ja valige **200 OK**. Klõpsake nuppu **Salvesta** see toiming salvestamiseks.

###<a name="delete-a-batch-execution-job"></a>Paketi täitmise töö kustutamine

Klõpsake **toimingu lisada** API AzureML BES toimingu lisamiseks. **HTTP-Verbi,**valige **Kustuta** . Tippige **/workspaces/ {tööruumi} /services/ {teenuse} {jobid} /jobs/ ?api versiooni = {apiversion}** **malli URL-i**jaoks. Tippige **BES kustutamine** **kuvatav nimi**. Klõpsake **vastuste** > **lisamine** vasakul ja valige **200 OK**. Klõpsake nuppu **Salvesta** see toiming salvestamiseks.

##<a name="call-an-operation-from-the-developer-portal"></a>Kõne toimingu portaalist arendaja

Toimingute saab kutsuda otse arendaja portaali, mis pakub mugavam ülevaatus ja testimine API toimingud. Selles juhises juhend helistate **RRS käivitada** meetod, mis lisati **AzureML Demo API**. Klõpsake **portaali arendaja** menüü ülaosas paremal klassikaline portaali.

![Arendaja-portaal](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

Klõpsake **API-de** ülalt menüüst, ja klõpsake **AzureML Demo API** kuvamiseks toimingud saadaval.

![demoazureml-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

Valige toiming **RRS käivitada** . Klõpsake **seda proovida**.

![Proovige-it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

Taotluse parameetrite, tippige oma **tööruumi**, **teenuse**, **2.0** jaoks **apiversion**ja **true** **üksikasjad**. Võite leida oma **tööruumi** ja **teenuse** AzureML web teenuse armatuurlaua (vt **testi veebiteenuse** liites A).

Taotluse päised, klõpsake nuppu **Lisa päis** ja tippige **Sisutüüp** ja **rakenduse/json**, ja seejärel klõpsake nuppu **Lisa päis** ja tippige **autoriseerimine** ja **esitaja <YOUR AZUREML SERVICE API-KEY> **. Leiate AzureML web teenuse armatuurlaua **api võti** (vt **testi veebiteenuse** liites A).

Tippige **{"Sisendeid": {"input1": {"ColumnNames": ["Col2"], "väärtust": [["see on hea päev"]]}}, "GlobalParameters": {}}** taotluse keha.

![azureml-demo-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Klõpsake nuppu **saada**.

![saatmine](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

Pärast toimingu rakendatakse, kuvab arendaja portaali **URL-i nõutav** tagaandmebaas teenus, **vastuse olek**, **vastuse päiseid**ja mis tahes **vastuse sisu**.

![vastuse olek](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

##<a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Lisa A – loomine ja testimine lihtsa AzureML web teenus

###<a name="creating-the-experiment"></a>Loomise katse

Allpool on lihtne AzureML katse loomine ja juurutamine selle nimega veebiteenuse juhised. Web teenuse võtab, sisestada suvalise tekstiveeru ning tagastab kogumi funktsioonid, mis on esitatud täisarvuna. Näiteks:

Teksti | Räsitud tekst
--- | ---
See on hea päev | 1 1 2 2 0 2 0 1

Esmalt teie valitud brauseris liikuda: [https://studio.azureml.net/](https://studio.azureml.net/) ja sisestage oma kasutajanimi ja parool sisse logida. Järgmisena looge uus tühi katse.

![Otsingu-katse-Mallid](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Pange selle nimeks **SimpleFeatureHashingExperiment**. Laiendage **Salvestatud andmekomplektide** ja **Raamatute ülevaateid Amazon** lohistada oma katse.

![lihtne-funktsiooni räsi-katse](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Laiendage **Andmete teisendamiseks** ja **Stringimanipuleerimisfunktsioonide** ja lohistage oma katse **Andmekomplekti veergude valimine** . **Valige veerud andmekomplekti** **raamat ülevaateid Amazon** ühenduse.

![Valige-veerud](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

Klõpsake **Andmekomplekti veergude valimine** ja seejärel klõpsake nuppu **Käivita veeru Vaateselektori** ja valige **Col2**. Klõpsake muudatuste rakendamiseks märke.

![Valige-veerud](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

Laiendage **Teksti analüüsi** ja lohistage **Funktsiooni räsi** katse. **Valige veerud andmekomplekti** ühenduse loomine **funktsiooni räsi**.

![ühenduse loomine projekti veerud](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Tippige **Hashing bitsize** **3** . See loob 8 (23) veerud.

![räsi bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

Selles etapis soovite nuppu **Käivita** testimiseks katse.

![käivitamine](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

###<a name="create-a-web-service"></a>Veebiteenuse loomine

Nüüd looge veebiteenusest. Laiendage **Veebiteenuse** ja lohistage oma katse **sisestatud** . **Funktsioon räsi** **sisendit** ühenduse. Lohistage **väljundi** ka oma katse. **Funktsioon räsi** **väljundi** ühenduse.

![väljundi – kui soovite-funktsiooni-räsi](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Klõpsake nuppu **Avalda veebiteenus**.

![avaldamine veebiteenuse](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

Klõpsake nuppu **Jah** katse avaldada.

![Jah avaldamine](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

###<a name="test-the-web-service"></a>Veebiteenuse testimine

Teenuse AzureML web koosneb RSS-i (taotluse/vastuse teenus) ja BES (paketi täitmise teenus) lõpp-punktid. RSS on sünkroonse täitmiseks. BES on asünkroonne töö täitmiseks. Oma veebiteenuse valimi Python allikas allpool testimiseks võimalik, et peate Laadige alla ja installige Azure'i SDK Python (vt: [Kuidas installida Python](../python-how-to-install.md)).

Peate valimi allika allpool ka **tööruumi**, **teenuse**ja **api_key** oma katse. Leiate tööruumi ja teenuse, klõpsates kas **Taotluse/vastuse** või **Paketi täitmise** oma katse web teenuse armatuurlaua jaoks.

![Otsi-tööruumi-ja teenus](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

**Api_key** leiate oma katse web teenuse armatuurlaua klõpsates.

![Otsi-api võti](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

####<a name="test-rrs-endpoint"></a>Lõpp-punkti testi RRS

#####<a name="test-button"></a>Nuppu Test

**Testimiseks** klõpsake armatuurlaual web teenuse on lihtne viis testimiseks RRS lõpp-punkti.

![test](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

Tippige **see on hea päev** **col2**. Klõpsake märke.

![Sisestage andmed](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

Kuvatakse umbes

![väljundi näidis](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

#####<a name="sample-code"></a>Proovi kood

Teine võimalus Testige oma RRS on oma kliendi kood. Kui klõpsate armatuurlaua ja liikuge alla **taotluse/vastuse** , kuvatakse proovi kood C#, Python ja R. Näete ka RRS taotluse taotluse URI, sh süntaks päise ja keha.

Sellest juhendist kuvatakse töö Python näide. Peate **tööruumi**, **teenuse**ja **api_key** oma katse seda muuta.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

####<a name="test-bes-endpoint"></a>Lõpp-punkti testi BES
Armatuurlaua ja liikuge all olevat nuppu **paketi täitmist** . Kuvatakse proovi kood C#, Python ja R. Näete ka süntaks BES taotlusi esitada tööd, alustada tööd, oleku või tulemuste töö ja töö kustutamine.

Sellest juhendist kuvatakse töö Python näide. Peate muutma selle **tööruumi**, **teenuse**ja **api_key** oma katse. Lisaks peate muutma **salvestusruumikonto nimi**, **salvestusruumi konto võti**ja **salvestusruumi container nimi**. Lõpetuseks, peate **Sisestuskeel faili** asukoht ja **väljundfail**asukoha muutmiseks.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
