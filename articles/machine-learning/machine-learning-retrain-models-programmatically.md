<properties
    pageTitle="Seadme õppe mudelid programmiliselt ümber | Microsoft Azure'i"
    description="Saate teada, kuidas programmiliselt ümber mudeli ja värskendada veebiteenuse Azure seadme õ äsja koolitatud mudeli kasutamine."
    services="machine-learning"
    documentationCenter=""
    authors="raymondlaghaeian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="raymondl;garye;v-donglo"/>


# <a name="retrain-machine-learning-models-programmatically"></a>Seadme õppe mudelid programmiliselt ümber  

Ülevaate, saate teada, kuidas koolitada programmiliselt Azure seadme õ Web teenuse C# ja seadme õ paketi täitmise teenuse abil.

Kui teil on ümberõppe mudelit, järgmised juhendavad tutvustused näitab, kuidas värskendada oma sõnastikupõhise veebiteenuse mudeli:

- Kui olete juurutanud klassikaline veebiteenuse masina õ veebiteenuste portaalis, vt [ümber klassikaline veebiteenusest](machine-learning-retrain-a-classic-web-service.md). 
- Kui olete juurutanud uus veebiteenus, vt [ümber abil arvuti õ halduse cmdletid uus veebiteenus](machine-learning-retrain-new-web-service-using-powershell.md).

Ümberõpe protsessi ülevaate leiate teemast [ümber masina õ mudeli](machine-learning-retrain-machine-learning-model.md).

Kui soovite alustada olemasoleva uue Azure ressursihaldur vastavalt veebiteenuse, vt [olemasoleva sõnastikupõhise veebiteenuse ümber](machine-learning-retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Koolitus katse loomine
 
Selle näite puhul saate kasutada "näide 5: rongis, Test, hinnata kahendarvu liigitamiseks: täiskasvanud andmekomplekti" Microsoft Azure'i masina õ proovi. 
    
Katse loomiseks tehke järgmist.

1.  Logige sisse Microsoft Azure'i masina Õppekeskuse Studio. 
2.  Klõpsake armatuurlaual alumises paremas nurgas nuppu **Uus**.
3.  Microsofti Samples, valige proovi 5.
4.  Ümber nimetada katse, ülaosas katse lõuend, valige katse nimi "näide 5: rongis, Test, hinnata kahendarvu liigitamiseks: täiskasvanud andmekomplekti".
5.  Tippige rahvaloendus mudel.
6.  Katse allosas lõuend, klõpsake nuppu **Käivita**.
7.  Klõpsake **veebiteenuse seadistamine** ja valige **ümberõppele veebiteenus**. 

    ![Algne katse.][2]

Diagramm 2: Algne katse.

## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Sõnastikupõhise katse luua ja avaldada veebiteenuse  

Järgmisena saate luua Predicative katse.

1.  Katse lõuend allosas nuppu **Määra veebiteenuse** ja valige **Sõnastikupõhise veebiteenus**. See salvestab mudeli koolitatud mudel ja lisab web teenuse sisestus- ja väljundi moodulid. 
2.  Klõpsake nuppu **Käivita**. 
3.  Pärast katse on töö lõpetanud, klõpsake **Juurutada veebiteenuse [Classic]** või **Juurutada veebiteenuse [uus]**.

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>Koolitus katse nimega koolitus veebiteenuse juurutamine

Koolitatud mudeli koolitada peab juurutamist koolitus katse nimega ümberõppele veebiteenuse loodud. See veebiteenus vajab *Web teenuse väljundi* mooduli ühendatud selle * [Rongi mudel] [ train-model] * mooduli, on võimalik, andes uue koolitatud mudelid.

1. Koolitus katse naasmiseks klõpsake vasakul paanil ikooni katsete ja seejärel klõpsake nimega rahvaloendus mudeli katse.  
2. Tippige otsinguväljale Otsi katse üksuste veebiteenus. 
3. Lohistage *Web teenuse sisendi* mooduli katse lõuendile ja selle väljundi ühenduse moodulit *Clean puuduvad andmed* .  See tagab, et ümberõpet andmete töötlemise samamoodi nagu algses treeningu andmed.
4. Lohistage kaks *veebiteenuse väljundi* moodulid katse lõuend. Üks ja väljundi *Mudeli hindamine* mooduli teise *Rongi mudeli* mooduli väljundi ühenduse. Web teenuse väljundi **Rongi mudel** annab meile uus koolitatud mudel. Manustatud **Hinnata mudeli** väljund tagastab selle mooduli väljund, mis on tulemusi.
5. Klõpsake nuppu **Käivita**. 

Järgmine peab juurutada koolitus katse, on veebipõhine teenus, mis toodab koolitatud mudeli ja mudeli hindamise tulemused. Selleks sõltuvad teie järgmise toimingute kogum, kas töötate klassikaline veebiteenuse või uus veebiteenus.  
  
**Klassikaline veebiteenuse**

Katse lõuend allosas käsku **Määra veebiteenuse** ja valige **Juurutamine veebiteenuse [Classic]**. Veebiteenuse **armatuurlaua** kuvatakse API võti ja API spikrilehele paketi täitmist. Luua koolitatud mudeleid saab kasutada ainult paketi täitmise viisi.

**Uue veebiteenuse**

Katse lõuend allosas käsku **Määra veebiteenuse** ja valige **Juurutamine veebiteenuse [uus]**. Web teenuse Azure seadme õ veebiteenuste portaali avatakse Deploy teenuse veebilehele. Tippige oma veebiteenuse nimi ja valige makse leping, siis klõpsake käsku **Deploy**. Luua koolitatud mudeleid saab kasutada ainult meetod paketi täitmine

Mõlemal juhul, kui katse on lõpule jõudnud, töötab, tulemiks oleva töövoo peaks välja nägema järgmine:

![Tulemiks oleva töövoo pärast käivitada.][4]

Diagramm 3: Tulemuseks pärast Käivita töövoog.

## <a name="retrain-the-model-with-new-data-using-bes"></a>Uute andmetega, kasutades BES mudeli ümber

Selle näite puhul te kasutate C# ümberõpe rakenduse loomiseks. Python või R proovi kood abil saate selle ülesande.

Helistamiseks ümberõpet API:

1. Luua C# konsooli rakendus Visual Studio (uus -> Project -> Windowsi töölaua -> konsooli rakendus).
2.  Seadme õ veebiteenuse portaali sisse logida.
3.  Kui töötate klassikaline veebiteenuse, klõpsake käsku **Klassikaline veebiteenused**.
    1.  Klõpsake nuppu veebiteenus, millega töötate.
    2.  Klõpsake nuppu Vaikesäte lõpp-punkti.
    3.  Klõpsake **tarbimine**.
    4.  Klõpsake jaotises **Proovi kood** **tarbida** lehe allosas **paketi**.
    5.  Selle toimingu jätkake 5.
4.  Kui töötate uus veebiteenus, klõpsake **Veebiteenused**.
    1.  Klõpsake nuppu veebiteenus, millega töötate.
    2.  Klõpsake **tarbimine**.
    3.  Klõpsake jaotises **Proovi kood** tarbida lehe allosas **paketi**.
5.  Paketi täitmise näidis C# koodi kopeerida ja kleepida selle faili Program.cs, alustades seda kindlasti nimeruumi jääb samaks.

Lisage Nugeti pakett Microsoft.AspNet.WebApi.Client määratud kommentaarid. Viide Microsoft.WindowsAzure.Storage.dll lisamiseks võib esmalt peate installima Microsoft Azure'i salvestusruumi teenuste kliendi teek. Lisateavet leiate teemast [Windows salvestusruumi Services](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-the-apikey-declaration"></a>Värskendage apikey deklareerimine

Otsige üles **apikey** deklareerimise.

    const string apiKey = "abc123"; // Replace this with the API key for the web service

**Tarbida** lehe jaotises **lihtsa tarbimine teave** primaarvõtme leidke ja kopeerige see **apikey** deklaratsiooni.

### <a name="update-the-azure-storage-information"></a>Azure Storage teabe värskendamine

BES proovi kood lisatud faili Azure Storage kohalikule kettale (nt "C:\temp\CensusIpnput.csv"), töötleb ja kirjutab tulemused tagasi Azure Storage.  

Selle ülesande peab salvestusruumi konto nimi, võti ja container teavet tuua salvestusruumi konto kaudu klassikaline Azure portaali ja Värskenda väärtused kood. 

1. Klassikaline Azure portaali sisse logida.
1. Klõpsake vasakpoolsel navigeerimispaanil veerus, **salvestusruumi**.
1. Valige loendist salvestusruumi kontod, üks retrained mudeli talletamiseks.
1. Klõpsake lehe allosas nuppu **Kiirklahvide haldamine**.
1. Kopeerige ja salvestage **Accessi primaarvõtme** ja sulgege dialoogiboks. 
1. Klõpsake lehe ülaosas **ümbriste**.
1. Valige mõni olemasolev ümbris või looge uus ja salvestage nimi.

Otsige *StorageAccountName*, *StorageAccountKey*ja *StorageContainerName* deklaratsiooni ja väärtusi, mis on salvestatud Azure portaalist värskendada.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name
            
Samuti peate veenduma, Sisestuskeel fail on saadaval kood määratud asukohas. 

### <a name="specify-the-output-location"></a>Väljundi asukoha määramine

Taotleda last väljundi asukoha määramisel *RelativeLocation* määratud faili laiend peab olema määratud ilearner. 

Vaadake järgmises näites:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

>[AZURE.NOTE] Oma väljundi asukohtade nimed võivad erineda need web teenuse väljundi moodulid lisatud järjestuse põhjal ülevaate. Kuna see koolitus eksperimenteerida kaks väljundit häälestamist tulemused sisaldavad salvestusruumi asukohateavet mõlemat.  

![Ümberõpe väljund][6]

Diagramm 4: Ümberõpe väljundi.

## <a name="evaluate-the-retraining-results"></a>Ümberõpe tulemuste hindamine
 
Rakenduse käivitamisel väljund sisaldab URL-i ja SAS luba juurdepääsemise hindamise tulemused.

Näete tulemusi retrained mudeli kombineerides *BaseLocation*, *RelativeLocation*ja *SasBlobToken* väljundi tulemused *output2* (nagu on näidatud eelnev ümberõpe väljundi pilt) ja kleepides täielik URL brauseri aadressiribale.  

Uurige tulemid kindlaks teha, kas äsja koolitatud mudel toimib hästi piisavalt olemasolev fail asendada.

Kopeerige *BaseLocation*, *RelativeLocation*ja *SasBlobToken* väljundi tulemused, kasutage neid ümberõppe käigus.

## <a name="next-steps"></a>Järgmised sammud

Kui olete juurutanud sõnastikupõhise veebiteenuse, klõpsates **Juurutamine veebiteenuse [Classic]**, lugege teemat [ümber klassikaline veebiteenusest](machine-learning-retrain-a-classic-web-service.md).

Kui olete juurutanud sõnastikupõhise veebiteenuse, klõpsates **Juurutamine veebiteenuse [uus]**, vt [ümber abil arvuti õ halduse cmdletid uus veebiteenus](machine-learning-retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/