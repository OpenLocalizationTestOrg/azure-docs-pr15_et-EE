<properties
    pageTitle="PowerShelli moodul masina õ | Microsoft Azure'i"
    description="Azure'i masina õ PowerShelli moodul on saadaval avaliku eelvaate. PowerShelli abil saate luua ja hallata tööruumid, katsete, web services ja palju muud."
    keywords="katsetamiseks lineaarse regressioonisirge, seadme õ algoritmide kohta, seadme õ õpetuse, ennustava modelleerimine tehnika, andmete teadus katse"
    services="machine-learning"
    documentationCenter=""
    authors="hning86"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="garye;haining"/>

# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>Microsoft Azure'i masina õ PowerShelli moodul

Azure'i masina õ PowerShelli moodul on võimas, mis võimaldab teil tööruumid, katsete, andmekomplektide, web serivces ja haldamine Windows PowerShelli abil.

Saate vaadata dokumente ja moodulit koos täieliku lähtekoodi veebisaidil [https://aka.ms/amlps](https://aka.ms/amlps)alla laadida. 

## <a name="what-is-the-machine-learning-powershell-module"></a>Mis on masina õ PowerShelli moodul?

Seadme õ PowerShelli mooduli lisamine. NET põhinev DLL moodul, mis võimaldab teil täielikult Windows PowerShelli kaudu hallata Azure seadme õ tööruumid, katsete, andmekomplektide, web services ja web teenuse lõpp-punktid. Koos moodulit, saate alla laadida täielik lähtekoodi, mis sisaldab puhtalt eraldatud [C# API kiht](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). See tähendab, et saate viidata selle DLL .NET projekti kaudu ja hallata Azure seadme õ .net-i koodi kaudu. Lisaks sõltub DLL aluseks REST API-d, mille abil saate kasutada otse oma lemmik klient.

## <a name="what-can-i-do-with-the-powershell-module"></a>Mida saab teha PowerShelli mooduli?

Siin on mõned toimingud, mida saate teha selle PowerShelli mooduli. Tutvuge [selleks on](https://aka.ms/amlps) need ja palju rohkem funktsioone.

- Uue tööruumi halduse serdiga ([Uus-AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace)) ettevalmistamine
- Eksportimine ja importimine JSON faili, mis tähistab katse graafik, ([Ekspordi-AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) ja [Impordi-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
- Käivitage katse ([Start-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
- Loomine veebiteenuse välja sõnastikupõhise katse ([Uus-AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
- Luua uue lõpp-punkti avaldatud veebiteenuse ([Lisa-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
- Autonoomsest RRS ja/või BES web teenuse lõpp-punkti ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) ja [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Siin on kiire näide olemasoleva katse käivitamiseks PowerShelli kaudu:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Põhjalikumalt kasutamine juhul lugege artiklit väga tihti nõutav ülesannete automatiseerimiseks PowerShelli mooduli kasutamise kohta: [mitme seadme õppe mudelid ja web loomine teenuse lõpp-punktid: ühe katse PowerShelli kaudu](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Kuidas alustada?

Seadme õ PowerShelliga alustamiseks [vabastage paketi](https://github.com/hning86/azuremlps/releases) GitHub alla laadida ja järgige [juhiseid installi](https://github.com/hning86/azuremlps/blob/master/README.md). Peate blokeeringu allalaaditud lahti pakitud DLL ja seejärel importige need oma PowerShelli keskkonnas. Enamik cmdlet-käskude nõuavad, et te ei sisesta tööruumi ID, tööruumi autoriseerimine luba ja Azure piirkond, kus tööruum on. Lihtsaim viis anda need on vaikimisi config.json faili, mis on üksikasjalikult installimise juhiseid. Muidugi võite klooni git puu ja muuta või kompileerida koodi kohalikult Visual Studio abil.

## <a name="next-steps"></a>Järgmised sammud

PowerShelli moodul kasutab ka edaspidi parandada ja selle eelvaate aja jooksul laiendatud. Jälgige [Cortana ärianalüüsi ja](https://blogs.technet.microsoft.com/machinelearning/) seadme õ ajaveebi rohkem uudiseid ja teavet.
