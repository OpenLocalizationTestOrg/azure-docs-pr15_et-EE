<properties
    pageTitle="Meeleolu analüüsi Azure'i voo Analytics ja Azure seadme õ abil | Microsoft Azure'i"
    description="Kasutaja määratletud funktsioon ja seadme Õppekeskuse kasutamine voo Analytics töö"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>


<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/04/2016" 
    ms.author="jeffstok"
/>

# <a name="sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Meeleolu analüüsi, kasutades Azure voo Analytics ja Azure arvuti koolitus #

Selles artiklis on mõeldud aitavad kiiresti luua lihtsa Azure'i voo Analytics töö Azure'i kohapeal õ integreerimise. Me kasutada meeleolu analytics masina õ mudeli galeriist Cortana ärianalüüsi voogesituse teksti andmete analüüsimiseks ja määrata meeleolu Keskmine reaalajas. Selle artikli teave aitab teil mõista stsenaariumid nagu sisse voogesituse Twitteri andmete reaalajas meeleolu analytics, analüüsida kirjete toe töötajate ja klientide vestlused ja kommentaarid foorumite, ajaveebide ja videoid, lisaks palju muud reaalajas, ennustava hinded stsenaariumid hindab.

See artikkel annab CSV-vormingus näidisfaili tekstiga sisendina Azure'i bloobimälu näidatud järgmisel pildil. Töö kehtib meeleolu analytics mudeli kasutaja määratletud funktsiooni (UDF) teksti Näidisandmete bloobimälu poest. Seetõttu paigutatakse sama bloobimälu poe teise CSV-failis. 

![Voo Analytics masina õpetused](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

Järgmisel pildil näitab selle konfiguratsiooni. Suurema stsenaariumi puhul saate asendada bloobimälu streaming Azure'i sündmuse jaoturi sisend Twitteri andmeid. Lisaks võib koostada [Microsoft Power BI](https://powerbi.microsoft.com/) reaalajas visualiseerimine liitväärtuse meeleolu.    

![Voo Analytics masina õpetused](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Eeltingimused

Eeltingimused lõpuleviimise, mis on näidatud selles artiklis on järgmised:

-   Azure'i aktiivne tellimus.
-   CSV-faili osa andmeid. Saate alla laadida faili, mis on näidatud joonisel 1 [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample Data/sampleinput.csv)või saate luua oma faili. Selles artiklis on endale kasutada ühte github allalaadimiseks.

Kõrge, tõendada selle artikli toimingu lõpuleviimiseks kuvatakse tehke järgmist:

1.  Azure'i bloobimälu Sisestuskeel CSV-faili üleslaadimine.
2.  Lisada meeleolu analytics mudeli galeriist Cortana ärianalüüsi oma Azure seadme õ tööruumi.
3.  Selle mudeli juurutamine veebiteenuse masina õ tööruumi nimega.
4.  Looge voo Analytics tööd, mis nõuab selle veebiteenuse funktsioonina määratlemiseks meeleolu teksti sisestada.
5.  Alustada tööd voo Analytics ja jälgida väljund.

## <a name="upload-the-csv-input-file-to-blob-storage"></a>Sisestuskeel CSV-faili üleslaadimine bloobimälu

Selle toimingu abil saate mis tahes CSV-faili, nagu on juba määratud github allalaadimiseks saadaval. Saate [Azure'i salvestusruumi Explorer](http://storageexplorer.com/) või Visual Studio faili üles laadida või saate kasutada kohandatud koodi. Visual Studio näidete kasutamine

1.  Klõpsake Visual Studio **Azure'i** > **salvestusruumi** > **Manustamine välise salvestusruumi**. Sisestage **konto nimi** ja **konto võti**.  

    ![Voo Analytics masina õ Server Explorer](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-server-explorer.png)  

2.  Lisatud juhises 1 mälumahu, klõpsake nuppu **Loo bloobimälu Container**ja sisestage loogiline nimi. Kui olete loonud ümbris, avage see sisu vaatamiseks. (See on tühi sel hetkel).  

    ![Kasutusanalüüsi masina õ voona, luua bloobimälu](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-create-blob.png)  

3.  CSV-faili üleslaadimiseks klõpsake **Bloobimälu üles**ja klõpsake **faili kohalikule kettale**.  

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Cortana ärianalüüsi galeriist meeleolu analytics mudeli lisamine

1.  Cortana ärianalüüsi Galerii [sõnastikupõhise meeleolu analytics mudeli](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) alla laadida.  
2.  Seadme õ Studios, klõpsake nuppu **Ava Studios**.  

    ![Kasutusanalüüsi masina õ voona, avage seadme õ Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3.  Sisselogimiseks minge tööruumi. Valige asukoht, mis sobib kõige paremini oma asukoht.
4.  Klõpsake lehe allosas **käivitada** .  
5.  Pärast protsessi edukalt töötab, klõpsake nuppu **Veebiteenus juurutamine**.
6.  Meeleolu analytics mudel on kasutamiseks valmis. Kinnitamiseks, klõpsake nuppu **testi** ja anda oma panus teksti, näiteks "I kuulda Microsoft." Test peaks tagastama tulemus on järgmine:

`'Predictive Mini Twitter sentiment analysis Experiment' test returned ["4","0.715057671070099"]...`  

![Voo Analytics kohapeal õ, analüüsi andmed](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-analysis-data.png)  

Veerus **rakendused** linki **Excel 2010 ja varasemate töövihiku** API võtme ja URL, mida peate hiljem seadistada voo Analytics töö. (Selle toimingu kasutamiseks on vajalik ainult arvuti õ mudeli teise Azure'i konto tööruumist. Selles artiklis eeldatakse, nii on see käsitlema seda stsenaariumi.)  

Pange tähele veebi teenuse URL-i ja juurdepääsu võti allalaaditud Exceli failist, nagu allpool näidatud:  

![Voo Analytics masina õ, Kiire ülevaade](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  

## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Voo Analytics tööd, mis kasutab seadme õ mudelit loomine

1.  Avage [Azure'i portaalis](https://manage.windowsazure.com).  
2.  Klõpsake nuppu **Uus** > **Data Services** > **voo Analytics** > **kiire loomine**. Sisestage nimi oma töö **Töö nimi**, sisestada sobiv piirkond **piirkonna**töö ja valige konto **piirkondliku jälgimise salvestusruumi**konto.    
3.  Pärast töö loomist **sisendina** menüü, klõpsake nuppu **Lisa sisend**.  

    ![Kasutusanalüüsi masina õ voona, seadme õ sisestusmeetodi lisamine](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-input-screen.png)  

4.  **Sisestusmeetodi lisamine** viisardi esimesel lehel Valige **andmevoos**ja seejärel klõpsake nuppu **edasi**. Järgmisel lehel nuppu **Bloobimälu** soovitud sisendina ja seejärel klõpsake nuppu **edasi**.  
5.  Viisardi lehel **Bloobimälu talletusmahu** pakuvad salvestusruumi konto bloobimälu container nime varem määratletud kui teil on üles laaditud andmed. Klõpsake nuppu **edasi**. Klõpsake **Sündmust serialiseerimisvormingus** **CSV**. Aktsepteerige **sariväljaanne** sätetelehe ülejäänud vaikeväärtust. Klõpsake nuppu **OK**.  
6.  Klõpsake menüü **väljundeid** nuppu **Lisa väljund**.  

    ![Kasutusanalüüsi masina õ voona, lisada väljund](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-output-screen.png)  

7.  Klõpsake **Bloobimälu**ja sisestage siis sama parameetrid, välja arvatud ümbris. **Sisestatud** väärtus on konfigureeritud lugeda ümbris nimega "test" kui ka **CSV-** failis videoloendi. Sisestage **väljundi**, "testoutput". Container nimed peavad olema erinevad. Veenduge, et see ümbris olemas.     
8.  Klõpsake nuppu **edasi** selle väljundi **sariväljaanne sätete**konfigureerimiseks. Nagu **Sisestuskeel**nuppu **CSV**, ja seejärel klõpsake nuppu **OK** .
9.  Vahekaardi **funktsioonide** nuppu **Lisa masina õ funktsiooni**.  

    ![Voona Analytics masina õ, seadme õ funktsiooni lisamine](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-ml-function.png)  

10. Leidke lehel **Veebiteenuse õ arvuti sätted** arvuti õ tööruumi, veebiteenuse ja vaikimisi lõpp-punkti. Selle artikli sätted käsitsi kasum konfigureerimise veebiteenus, mis tahes tööruumi, kui teate URL-i ja on API võti tundmine rakendada. Sisestage lõpp-punkti **URL-i** ja **API võti**. Klõpsake nuppu **OK**.    

    ![Voo Analytics masina õ, seadme õ veebiteenuse](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-web-service.png)    

11. Klõpsake menüü **päring** muuta päringu järgmiselt:    

 ```
    WITH subquery AS (  
        SELECT text, sentiment(text) as result from input  
    )  
 
    Select text, result.[Scored Labels]  
    Into output  
    From subquery  
 ```    
12. Klõpsake nuppu **Salvesta** päringu salvestamiseks.

## <a name="start-the-stream-analytics-job-and-observe-the-output"></a>Alustada tööd voo Analytics ja jälgida väljund

1.  Klõpsake nuppu **Käivita** töö lehe allosas.
2.  **Päringu dialoogiboks käivitamine**nuppu **Kohandatud aeg**ja seejärel valige aeg enne, kui saate üles laadida CSV bloobimälu. Klõpsake nuppu **OK**.  

    ![Voo Analytics masina õ, kohandatud aeg](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-custom-time.png)  

3.  Avage soovitud bloobimälu CSV-faili, näiteks Visual Studio üleslaadimiseks kasutatud tööriista abil.
4.  Mõne minuti pärast töö käivitamist väljundi container on loodud ja CSV-faili üleslaadimisel selle.  
5.  Avage fail vaikimisi CSV redaktoris. Midagi sarnast järgmine peaks olema kuvatud:  

    ![Voo Analytics masina õ, CSV-vaade](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  

## <a name="conclusion"></a>Kokkuvõte

Selles artiklis näitab, kuidas luua voo Analytics tööd, mis loeb streaming tekstandmeid ja kehtib meeleolu analytics andmete reaalajas. Kõik need saab täita ilma muretsema üksikasjadest, meeleolu analytics mudeli loomine. See on üks eeliseid Cortana ärianalüüsi komplekti.

Saate vaadata ka Azure kohapeal õ funktsioon seotud mõõdikute. Selleks klõpsake vahekaardil **kuvar** . Kolm funktsioon seotud mõõdikute kuvatakse.  

- **Funktsioon taotlusi** näitab saadetud masina õ veebiteenuse arvu.  
- **Funktsioon sündmuste** näitab taotluse sündmuste arv. Vaikimisi sisaldab iga seadme õ veebiteenuse taotluse kuni 1000 sündmused.  
- **Funktsioon taotlusi nurjus** näitab kohapeal õ veebiteenuse nurjunud taotluste arvu.  

    ![Voo Analytics masina õ, seadme õ kuvarit kuvamine](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-monitor-view.png)  
