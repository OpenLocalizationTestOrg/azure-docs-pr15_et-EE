<properties
    pageTitle="Seadme õ halduse PowerShelli cmdlet-käskude abil uus veebiteenus ümber | Microsoft Azure'i"
    description="Saate teada, kuidas programmiliselt ümber mudeli ja värskendada veebiteenuse äsja koolitatud mudeli kasutamine Azure seadme õ masina õ halduse PowerShelli cmdlet-käskude abil."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-new-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Seadme õ halduse PowerShelli cmdlet-käskude abil uus veebiteenus ümber

Kui te ümber uus veebiteenus, värskendate sõnastikupõhise web teenuse määratlus viitamiseks uus koolitatud mudel.  

## <a name="prerequisites"></a>Eeltingimused

Peate seadistatud koolitus katse ja sõnastikupõhise katse nagu on näidatud [ümber masina õ mudelid programmiliselt](machine-learning-retrain-models-programmatically.md). 

>[AZURE.IMPORTANT] Kui ka Azure ressursihaldur (uus) vastavalt Õppekeskuse veebiteenuse saab juurutada sõnastikupõhise katse. 
 
Juurutamine veebiteenuste kohta lisateabe saamiseks vt [Deploy teenuse Azure seadme õ web](machine-learning-publish-a-machine-learning-web-service.md).

Selle protsessi jaoks on vaja Azure seadme õ cmdlet-käsud on installitud. Installimist arvutisse õ cmdlet-käsud leiate teemast [Azure seadme õ cmdlettide](https://msdn.microsoft.com/library/azure/mt767952.aspx) viide MSDN-is.

Kopeeritud ümberõpe väljundi järgmine teave:

* BaseLocation
* RelativeLocation

Järgmised sammud

1.  Ressursihaldur Azure'i kontosse sisse logida.
2.  Saada web teenuste määratlemine
3.  Eksportida JSON Web teenuste määratlemine
4.  Värskendage ilearner bloobimälu klõpsake soovitud JSON viide.
5.  Funktsiooni JSON importimine Web teenuste määratlemine
6.  Värskendage veebiteenuse uus Web teenuse määratlemine

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Ressursihaldur Azure'i kontosse sisselogimine

Teil tuleb esmalt sisse logida keskkonnas PowerShelli cmdlet-käsu [Lisa-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) abil oma Azure'i kontosse.

## <a name="get-the-web-service-definition"></a>Saada Web teenuste määratlemine

Järgmiseks saada veebiteenuse helistate [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet-käsk. Web Service määratlus on sisemine esitus koolitatud mudeli veebiteenuse ja ei saa otse muuta. Veenduge, et oma sõnastikupõhise katse ja ei koolitus katse toomisel Web teenuse määratlus.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Olemasoleva web teenuse ressursi rühma nime määratlemiseks käivitage Get-AzureRmMlWebService cmdlet-käsk ilma kuvamiseks veebiteenuste tellimuse parameetrid. Leidke veebiteenuse, ja seejärel vaadake oma web teenuse ID-ga. Ressursirühma nimi on neljas elemendi ID, kohe pärast *resourceGroups* element. Järgmises näites on vaike-MachineLearning-SouthCentralUS ressursi rühma nime.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Teise võimalusena määramiseks olemasoleva web teenuse ressursi rühma nime, logige sisse Microsoft Azure'i masina õ veebiteenuste portaali. Valige veebiteenus. Ressursi rühma nimi on vahetult pärast *resourceGroups* elemendi URL-i veebiteenus, viies element. Järgmises näites on vaike-MachineLearning-SouthCentralUS ressursi rühma nime.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Eksportida JSON Web teenuste määratlemine

Muuta määratlust koolitatud mudeli kasutamine värskelt koolitatud mudeli, peate esmalt abil cmdlet-käsu [Ekspordi-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) JSON-vormingus faili eksportida.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>Värskendage ilearner bloobimälu klõpsake soovitud JSON viide.

Vara, leidke [koolitatud mudeli], värskendage *uri* väärtus *locationInfo* sõlm ilearner bloobimälu URI-ga. URI luuakse *BaseLocation* ja *RelativeLocation* ümberõpe kõne BES väljundi kombineerides.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
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

## <a name="import-the-json-into-a-web-service-definition"></a>Funktsiooni JSON importimine Web teenuste määratlemine

Kasutage [Impordi-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet muudetud JSON-faili uuesti teisendada teenuse määratlus, mille abil saate värskendada Predicative katse.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Värskendage veebiteenuse uus Web teenuse määratlemine

Lõpuks saate [Värskendus-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet sõnastikupõhise katse värskendada.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Kokkuvõte

Seadme õ PowerShelli halduse cmdletid saate värskendada koolitatud mudel sõnastikupõhise veebiteenuse lubada stsenaariumid, näiteks:

* Perioodiliste mudel ümberõpe uute andmetega.
* Jaotuse mudeli klientidele, andes neile ümber abil oma andmete mudelisse eesmärk.
