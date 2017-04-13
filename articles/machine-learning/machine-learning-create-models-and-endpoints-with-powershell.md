<properties
pageTitle="Mitme mudelite loomine ühe katse | Microsoft Azure'i"
description="PowerShelli abil saate luua mitme seadme õppe mudelid ja web teenuse lõpp-punktid sama algoritmi, kuid erinevad koolitus andmekomplektide."
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
ms.date="10/03/2016"
ms.author="garye;haining"/>

# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Luua mitme seadme õppe mudelid ja web teenuse lõpp-punktid ühe katse PowerShelli abil

Siin on levinud masina õ probleem: loodava on sama koolitus töövoo ja kasutada sama algoritmi, kuid muu koolitus andmekomplektide sisendina on palju mudeleid. Selles artiklis näidatakse, kuidas seda teha on skaala Azure seadme õ Studios vaid ühe katse abil.

Näiteks Oletame, et te oma globaalne bike rent frantsiisiäride. Soovite luua laenutus järele ajalooliste andmete põhjal prognoosida regressiooni mudel. Teil on kogu maailmas 1000 asukohad ja olete kogunud iga asukoht, mis sisaldab olulisi funktsioone, näiteks kuupäeva, kellaaja, ilm ja liikluse, mis on teatud asukohta iga Andmekomplekt.

Võiks koolitada mudelisse üks kord, kasutades ühendatud versiooni kõik andmekomplektide üle kõik asukohad. Kuid kuna iga asukohtade on kordumatu keskkonnas, oleks parem lähenemine koolitada regressiooni mudelisse eraldi, kasutades andmekomplekti iga asukoha jaoks. Nii iga koolitatud mudeli võib arvesse võtta *erinevate poe suurused, helitugevust, geograafia, populatsiooni, bike sõbralik liikluse keskkonnas jne*.

Mis võib olla parim lahendus, kuid te ei soovi luua 1000 koolitus katsete Azure seadme õppe igaüht tähistav kordumatu asukoht. Lisaks on suur ülesanne, on ka päris ebaefektiivne tundub, kuna iga katse ühesugused komponendid, välja arvatud koolitus andmekomplekti.

Õnneks, võite saavutada seda kasutades [Azure seadme õ ümberõpet API](machine-learning-retrain-models-programmatically.md) ja [Azure seadme õ](machine-learning-powershell-module.md)PowerShelliga tööülesande automatiseerimine.

> [AZURE.NOTE] Meie valimi kiiremini käitada, et me ei vähenda arvu 10 1000 asukohad. Kuid samu põhimõtteid ja juhised kehtivad 1000 asukohad. Ainus erinevus on, et kui soovite koolitada kaudu 1000 andmekomplektide tõenäoliselt soovite mõelda järgmised PowerShelli skriptide töötab samal ajal. Kuidas seda teha on selles artiklis ei käsitleta, kuid leiate näiteid PowerShelli lõimtöötlusele Internetis.  

## <a name="set-up-the-training-experiment"></a>Koolitus katse häälestamine

Me ei kavatse kasutada mõne näite [koolitus katsetamiseks](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) olete juba loodud [Cortana ärianalüüsi Galerii](http://gallery.cortanaintelligence.com). Avage see katse oma [Azure seadme õ Studio](https://studio.azureml.net) tööruumis.

>[AZURE.NOTE] Järgige selles näites koos, et soovite kasutada standard tööruumi, mitte tasuta tööruumi. Saadame luua ühe lõpp-punkti iga kliendi - 10 lõpp-punktid - kokku ja mis vajavad standard tööruumi sest tasuta tööruumi 3 lõpp-punktid. Kui teil on ainult tasuta tööruumi, alla, et lubada ainult 3 asukohtade skriptide lihtsalt muuta.

Katse kasutab ka **Andmete importimiseks** mooduli koolitus andmekomplekti *customer001.csv* importimine Azure storage konto. Oletame, et oleme kogutud koolitus andmekomplektide kõik bike asukohad ja salvestatud neid bloobimälu salvestusruumi paigal failinimedes ulatuvad *rentalloc001.csv* *rentalloc10.csv*.

![pilt](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

Pange tähele, et **Web teenuse väljundi** mooduli on lisatud moodulit **Rongi mudel** .
Kui see katse juurutatud veebiteenuse, lõpp-punkti seostatud väljundi tulemuseks koolitatud mudeli .ilearner faili vormingus.

Samuti võtke arvesse, et me häälestatud veebiteenuse parameeter URL, mis kasutab **Andmete importimine** mooduli. See võimaldab meil parameetri abil saate määrata üksikute koolitus andmekomplektide koolitada mudeli iga asukoha jaoks.
On ka muid võimalusi, mida me võivad seda teinud, nt SQL-päringu abil veebiteenuse parameetrit SQL Azure'i andmebaasi andmete toomiseks või lihtsalt kasutades **Web teenuse sisendi** mooduli läbida andmekomplekt veebiteenusele.

![pilt](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

Nüüd vaatame käivitage see koolitus katse andmekomplekti koolitus väärtus vaikimisi *rental001.csv* kasutamine. Kui vaatate väljundi **hinnata** mooduli (klõpsake väljund ja valige **Visualiseeri**), saate vaadata saame korralik läbiviimiseks *AUC* = 0,91. Selles etapis me valmis kasutama välja see koolitus katse veebiteenusest.

## <a name="deploy-the-training-and-scoring-web-services"></a>Koolitus ja hinded veebiteenuste juurutamine

Koolitus veebiteenuse juurutamiseks katse lõuend all nuppu **Web teenuse** ja valige **Veebiteenuse juurutamine**. Kõne see veebiteenus "" Bike rent koolitus".

Nüüd tuleb hinded veebiteenuse juurutamine.
Selleks me lõuend all linki saate **Määrata veebiteenuse** ja valige **Sõnastikupõhise veebiteenus**. See loob hinded katse.
Meil on vaja teha paar väiksemaid muudatusi teha seda tööd on veebipõhine teenus, nt eemaldamine sildi veeru "cnt" kaudu sisendandmete ja piirata väljundi eksemplari id ja vastava ennustatud väärtus.

Säästke töötavad, saate lihtsalt avada [sõnastikupõhise katse](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) Galerii, kuhu on juba ette valmistatud.

Veebiteenuse juurutamiseks sõnastikupõhise katse, siis klõpsake **Juurutada veebiteenuse** nuppu lõuend. Hinded veebiteenuse "Bike rent hinded" nimi ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>Looge 10 identse web teenuse lõpp-punktid PowerShelli abil

See veebiteenus sisaldab vaikimisi endpoint. Kuid me ei ole nii huvitatud vaikimisi lõpp-punkti, kuna see ei saa värskendada. Mida läheb vaja teha on 10 täiendavad lõpp-punktid, üks iga asukoha jaoks luua. Teeme selle PowerShelli abil.

Esmalt me meie PowerShelli keskkonna häälestamine:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Käivitage PowerShelli järgmine käsk:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Nüüd lõime 10 lõpp-punktid ja need kõik sisaldama sama koolitatud mudeli väljaõppe *customer001.csv*kohta. Saate vaadata nende Azure'i haldusportaal.

![pilt](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>Lõpp-punktid kasutada eraldi koolitus andmekomplektide PowerShelli kaudu värskendamine

Järgmise sammuna lõpp-punktid mudelid väljaõppe kordumatult iga kliendi üksikute andmete värskendamiseks. Kuid esmalt tuleb koostada need mudelid **Bike rent koolitus** veebiteenusest. Minge tagasi **Bike rent koolitus** veebiteenus. Meil on vaja kõne oma BES lõpp-punkti 10 korda koos 10 eri koolitus andmekomplektide tekitamiseks 10 erinevaid mudeleid. Kasutame **InovkeAmlWebServiceBESEndpoint** PowerShelli cmdlet-käsk selle tegemiseks.

Peate ka mandaat konto bloobimälu salvestusruumi sisse `$configContent`, st veebisaidil väljad `AccountName`, `AccountKey` ja `RelativeLocation`. Funktsiooni `AccountName` võib olla üks teie konto nimed, nagu on näha **Klassikaline Azure'i haldusportaal** (*salvestusruumi* menüü). Kui klõpsate salvestusruumi konto, selle `AccountKey` leiate allservas nupu **Haldamine Pääsuklahvide** ja *Accessi primaarvõtme*kopeerida. Funktsiooni `RelativeLocation` on tee suhtes salvestusruumi mudeli uue talletamiseks. Näiteks tee `hai/retrain/bike_rental/` skripti osutab ümbris nimega all `hai`, ja `/retrain/bike_rental/` on alamkaustad. Praegu ei saa luua alamkaustad portaali Kasutajaliidese kaudu, kuid on [Mitu Azure'i salvestusruumi maadeavastajad](../storage/storage-explorers.md) , mis võimaldavad teil seda teha. Soovitatav on luua uus ümbris salvestusruumi uue koolitatud mudelite (.ilearner failid) talletamiseks järgmiselt: lehelt salvestusruumi allservas nuppu **Lisa** ja pange sellele `retrain`. Kokkuvõttes allpool skripti vajalikud muudatused seotud `AccountName`, `AccountKey` ja `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

>[AZURE.NOTE] BES lõpp-punkti on ainus toetatud režiimi selle toimingu jaoks. RRS ei saa kasutada toodavad koolitatud mudelid.

Nagu näete, mitte ehitamine 10 eri BES töö json failid, me dünaamiliselt luua config stringi hoopis ja kanali *jobConfigString* parameeter **InvokeAmlWebServceBESEndpoint** cmdlet-käsu, kuna pole vaja koopia säilitamiseks kettale.

Kui kõik läheb mõne aja pärast peaksite nägema 10 .ilearner faili, *model001.ilearner* *model010.ilearner*, et teie Azure storage konto. Nüüd olete valmis värskendamiseks meie 10 hinded web teenuse lõpp-punktid nende mudelite **Paik-AmlWebServiceEndpoint** PowerShelli cmdlet-käsu abil. Pidage meeles uuesti, et saaksime ainult paik me programmiliselt varem loodud vaikelokaadisättest lõpp-punktid.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

See peaks töötama üsna kiiresti. Täitmise lõpulejõudmisel meil kuvatakse loonud 10 sõnastikupõhise web teenuse lõpp-punktid, kumbki sisaldab teatud andmekomplekti kohta laenutus asukohta, kõik ühe koolitus katse koolitada kordumatult koolitatud mudeli. Selle probleemi lahendamiseks võite proovida kutsumine nende lõpp-punktid **InvokeAmlWebServiceRRSEndpoint** cmdlet-käsu abil neile sama sisendandmete ja peaksite erinevate ennustamine tulemuste vaatamiseks, kuna koos muu koolitus komplekti õpetatakse mudelid.

## <a name="full-powershell-script"></a>Täielik PowerShelli skripti

Siin on täielik lähtekoodi kirjet.

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
