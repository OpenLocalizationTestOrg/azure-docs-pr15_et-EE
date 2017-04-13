<properties
    pageTitle="Olemasoleva sõnastikupõhise veebiteenuse ümber | Microsoft Azure'i"
    description="Saate teada, kuidas ümber mudeli ja värskendada veebiteenuse Azure seadme õ äsja koolitatud mudeli kasutamine."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="v-donglo"/>


# <a name="retrain-an-existing-predictive-web-service"></a>Olemasoleva sõnastikupõhise veebiteenuse ümber

Selle dokumendi kirjeldab ümberõpe protsessi järgmistel juhtudel:

* Teil on koolitus katse ja sõnastikupõhise katse, operationalized web teenust juurutatud.
* Teil on uusi andmeid, et soovite oma sõnastikupõhise veebiteenuse abil teha oma hinded.

Alustades oma olemasoleva veebiteenuse ja katsed, peate tehke järgmist:

1. Värskendage mudel.
    1. Muutke oma koolitus katse web teenuse sisendi ja väljundi lubamiseks.
    2. Koolitus katse juurutada ümberõpe veebiteenuse nimega.
    3. Kasutage mudeli koolitada koolitus katse paketi täitmise teenuse (BES).
2. Azure'i masina õ PowerShelli cmdlet-käskude abil saate uuendada sõnastikupõhise katse.
    1.  Ressursihaldur Azure'i kontosse sisse logida.
    2.  Saada web teenuse määratlus.
    3.  Saate eksportida JSON web teenuse määratlus.
    4.  Värskendage ilearner bloobimälu klõpsake soovitud JSON viide.
    5.  Funktsiooni JSON importida web teenuse määratlus.
    6.  Värskendage veebiteenuse teenuse määratlus.

## <a name="deploy-the-training-experiment"></a>Koolitus katse juurutamine

Koolitus katse nimega ümberõpe veebiteenuse juurutamiseks peate lisama web teenuse sisendi ja väljundi mudeli. *Web teenuse väljundi* mooduli kui ühendate selle katse * [Rongi mudel] [ train-model] * mooduli, lubate koolitus katse andes uue koolitatud mudeli abil saate oma sõnastikupõhise katse. Kui teil on mõni *Hinnata mudeli* mooduli, saate ka manustada web teenuse väljundi väljundina hindamise tulemuse saamiseks.

Oma koolitus katse värskendamiseks tehke järgmist.

1. *Web teenuse Sisestuskeel* mooduli ühenduse andmesisestuse (nt *Clean puuduvad andmed* moodul). Tavaliselt, mida soovite tagada teie sisendandmete töötlemine samamoodi nagu teie algsed andmed koolitus.
2. *Web teenuse väljundi* mooduli ühenduse oma *Rongi mudeli* mooduli väljund.
3. Kui teil on mõni *Hinnata mudeli* mooduli ja soovite väljund hindamise tulemused, ühenduse oma *Mudeli hindamine* mooduli väljund *Web teenuse väljundi* mooduli.

Käivitage oma katse.

Järgmiseks peate juurutama koolitus katse on veebipõhine teenus, mis toodab koolitatud mudeli ja mudeli hindamise tulemused.  

Katse lõuend allosas käsku **Määra veebiteenuse**ja valige **Juurutamine veebiteenuse [uus]**. Azure'i masina õ veebiteenuste portaali avatakse **Juurutamine veebiteenuse** lehele. Tippige oma veebiteenuse nimi, valige makse leping ja seejärel käsku **Deploy**. Paketi täitmise meetodit saate kasutada ainult luua koolitatud mudeleid.


## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Mudeli uute andmetega, kasutades BES ümber

Selles näites kirjutit C# ümberõpe rakenduse loomiseks. Python või R proovi kood abil saate selle ülesande.

Helistamiseks ümberõpe API:

1. Luua C# konsooli rakendus Visual Studio (**Uus** > **projekti** > **Windowsi töölaua** > **Konsooli rakendus**).
2.  Seadme õ veebiteenuste portaali sisse logida.
3.  Klõpsake nuppu veebiteenus, millega töötate.
2.  Klõpsake **tarbimine**.
3.  Klõpsake jaotises **Proovi kood** **tarbida** lehe allosas **paketi**.
5.  Paketi täitmise näidis C# koodi kopeerige ja kleepige see Program.cs fail. Veenduge, et nimeruumi jääb samaks.

Lisage Nugeti pakett Microsoft.AspNet.WebApi.Client, määratud kommentaarid. Viide Microsoft.WindowsAzure.Storage.dll lisamiseks võib esmalt peate installima [kliendi teek Azure Storage teenuste jaoks](https://www.nuget.org/packages/WindowsAzure.Storage).

Järgmine pilt kuvatakse lehe **tarbida** Azure seadme õ Web Services portaal.

![Lehe kasutamine][1]

### <a name="update-the-apikey-declaration"></a>Värskendage apikey deklareerimine

Leidke **apikey** deklareerimise.

    const string apiKey = "abc123"; // Replace this with the API key for the web service

**Tarbida** lehe jaotises **lihtsa tarbimine teave** primaarvõtme leidke ja kopeerige see **apikey** deklaratsiooni.

### <a name="update-the-azure-storage-information"></a>Azure Storage teabe värskendamine

BES proovi kood lisatud faili Azure Storage kohalikule kettale (nt "C:\temp\CensusIpnput.csv"), töötleb ja kirjutab tulemused tagasi Azure Storage.  

Azure Storage andmete värskendamiseks peab tuua salvestusruumikonto nimi, võti ja container teabe Azure klassikaline portaali konto salvestusruumi ja seejärel update correspondi pärast oma katse tulemuseks töövoog töötab peaks olema järgmine:

![Tulemiks oleva töövoo pärast käivitamine][4]ng väärtused kood.

1. Azure'i klassikaline portaali sisse logida.
1. Klõpsake vasakpoolsel navigeerimispaanil veerus, **salvestusruumi**.
1. Loendist salvestusruumi kontod, valige üks retrained mudeli talletamiseks.
1. Klõpsake lehe allosas nuppu **Kiirklahvide haldamine**.
1. Kopeerige ja salvestage **Accessi primaarvõtme** ja sulgege dialoogiboks.
1. Klõpsake lehe ülaosas **ümbriste**.
1. Valige mõni olemasolev ümbris või looge uus ja salvestage nimi.

Leidke *StorageAccountName*, *StorageAccountKey*ja *StorageContainerName* deklaratsiooni ja väärtusi, mis on salvestatud klassikaline portaalist värskendada.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Samuti peate veenduma Sisestuskeel fail on saadaval koodi määratud asukohas.

### <a name="specify-the-output-location"></a>Väljundi asukoha määramine

Kui määrate soovitud Väljastuskoht taotlemine last, faili, mis on määratud *RelativeLocation* pikendamine peab olema määratud `ilearner`. Vaadake järgmises näites:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Järgmine on näide ümberõpe väljundi: ![ümberõpe väljund][6]

## <a name="evaluate-the-retraining-results"></a>Ümberõpe tulemuste hindamine

Rakenduse käivitamisel väljund sisaldab URL-i ja ühiskasutusega juurdepääsu allkirjade märku, et on vaja juurdepääsu hindamise tulemused.

Näete tulemusi retrained mudeli kombineerides *BaseLocation*, *RelativeLocation*ja *SasBlobToken* väljundi tulemused *output2* (nagu on näidatud eelnev ümberõpe väljundi pilt) ja kleepides täielik URL brauseri aadressiribale.  

Uurige tulemid kindlaks teha, kas äsja koolitatud mudel toimib hästi piisavalt olemasolev fail asendada.

Kopeerige *BaseLocation*, *RelativeLocation*ja *SasBlobToken* väljundi tulemused.

## <a name="retrain-the-web-service"></a>Veebiteenuse ümber

Kui te ümber uus veebiteenus, värskendate sõnastikupõhise web teenuse määratlus viitamiseks uus koolitatud mudel. Web service määratlus on sisemine esitus koolitatud mudeli veebiteenuse ja ei saa otse muuta. Veenduge, et oma sõnastikupõhise katse ja ei koolitus katse toomisel web teenuse määratlus.

## <a name="sign-in-to-azure-resource-manager"></a>Azure'i ressursihaldur sisselogimine

Teil tuleb esmalt sisse logida PowerShelli keskkonnas oma Azure'i kontosse [Lisa-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdleti abil.

## <a name="get-the-web-service-definition-object"></a>Web Service määratlus objekti hankimine

Järgmiseks saada Web teenuse määratlus objekti helistate [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet-käsk.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Olemasoleva web teenuse ressursi rühma nime määratlemiseks käivitage Get-AzureRmMlWebService cmdlet-käsk ilma kuvamiseks veebiteenuste tellimuse parameetrid. Leidke veebiteenuse, ja seejärel vaadake oma web teenuse ID-ga. Ressursirühma nimi on neljas elemendi ID, kohe pärast *resourceGroups* element. Järgmises näites on vaike-MachineLearning-SouthCentralUS ressursi rühma nime.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Teise võimalusena Azure seadme õ veebiteenuste portaali sisselogimine määramiseks olemasoleva web teenuse ressursi rühma nime. Valige veebiteenus. Ressursi rühma nimi on vahetult pärast *resourceGroups* elemendi URL-i veebiteenus, viies element. Järgmises näites on vaike-MachineLearning-SouthCentralUS ressursi rühma nime.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Web Service määratlus objekti eksportida JSON

Määratluse koolitatud mudeli kasutada värskelt koolitatud muutmiseks peate kasutama esmalt cmdlet-käsu [Ekspordi-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) eksportimine JSON-vormingus faili.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>Värskendage viide ilearner bloobimälu

Vara, leidke [koolitatud mudeli], värskendage *uri* väärtus *locationInfo* sõlm ilearner bloobimälu URI-ga. URI luuakse *BaseLocation* ja *RelativeLocation* ümberõpe kõne BES väljundi kombineerides.

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition-object"></a>Web Service määratlus objekti soovitud JSON importimine

Kasutage [Impordi-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet teisendamine veebis teenuse määratlus objekti, mille abil saate värskendada Predikatiivses katse tagasi muudetud JSON-faili.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>Veebiteenuse värskendamine

Lõpuks värskendada sõnastikupõhise katse [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet-käsu abil.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
