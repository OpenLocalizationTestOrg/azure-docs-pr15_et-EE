<properties
   pageTitle="Power BI abil Lake andmesalve andmete analüüsiks | Microsoft Azure'i"
   description="Power BI abil salvestatud Azure'i Lake poes andmete analüüsiks"
   services="data-lake-store" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Power BI abil Lake andmesalve andmete analüüsiks

Sellest artiklist saate teada, kuidas kasutada Power BI Desktopi analüüsida ja visualiseerida andmeid, mis on talletatud Azure'i Lake poes.

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

- **Azure'i andmesalve Lake konto**. Järgige veebisaidil [Alustamine Azure'i Lake andmesalve Azure'i portaalis](data-lake-store-get-started-portal.md). Selles artiklis eeldatakse, et on juba loodud Lake andmesalve konto, nimetatakse **mybidatalakestore**, ja üles laadida andmete näidisfaili (**Drivers.txt**). See näidisfaili on [Azure andmete Lake Git hoidla](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt)allalaadimiseks saadaval.

- **Power BI Desktopi**. See saate [Microsofti](https://www.microsoft.com/en-us/download/details.aspx?id=45331)allalaadimiskeskusest alla. 


## <a name="create-a-report-in-power-bi-desktop"></a>Power BI Desktopi aruande loomine

1. Käivitage Power BI Desktopi teie arvutis.

2. Lindil **Avaleht** nuppu **Too**ning seejärel klõpsake nuppu rohkem. Dialoogiboksis **Andmete hankimine** klõpsake **Azure**, **Azure andmesalve Lake**nuppu ja seejärel nuppu **Ühenda**.

    ![Ühenduse loomine andmesalve Lake] (./media/data-lake-store-power-bi/get-data-lake-store-account.png "Ühenduse loomine andmesalve Lake")

3. Kui kuvatakse dialoogiboks kohta on arengu faasis konnektor, valida, kas jätkata.

4. Dialoogiboksis **Microsoft Azure'i andmesalve** pakuvad URL-i kontole Lake andmesalve ja seejärel klõpsake nuppu **OK**.

    ![URL-i andmesalve Lake] (./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL-i andmesalve Lake")

5. Klõpsake dialoogiboksis järgmise Lake andmesalve kontole sisse logida **sisse logida** . Teid suunatakse teie asutuse lehel sisselogimine. Järgige viipasid kontole sisse logima.

    ![Logige sisse teenusekomplekti andmesalve Lake] (./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Logige sisse teenusekomplekti andmesalve Lake")

6. Kui teil on edukalt sisse logitud, klõpsake nuppu **Ühenda**.

    ![Ühenduse loomine andmesalve Lake] (./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Ühenduse loomine andmesalve Lake")

7. Järgmise dialoogiboksis kuvatakse kontole Lake andmesalve üleslaaditud faili. Kontrollige teavet, ja klõpsake nuppu **Laadi**.

    ![Laadi andmete andmete Lake poest] (./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Laadi andmete andmete Lake poest")

8. Pärast seda, kui andmed on edukalt laaditud Power BI, kuvatakse menüü **väljad** järgmised väljad.

    ![Imporditud väljad] (./media/data-lake-store-power-bi/imported-fields.png "Imporditud väljad")

    Visualiseerimiseks ja andmete analüüsimine, saame eelistab siiski olema üks järgmistest väljadest andmed

    ![Soovitud väljad] (./media/data-lake-store-power-bi/desired-fields.png "Soovitud väljad")

    Järgmiste juhiste juurde, uuendame päringu teisendada imporditud andmed soovitud formaat.

9. **Avaleht** klõpsake lindil nuppu **Redigeeri päringud**.

    ![Päringu redigeerimine] (./media/data-lake-store-power-bi/edit-queries.png "Päringu redigeerimine")

10. Klõpsake aknas Päringuredaktor veergu **sisu** , klõpsake jaotises **kahendarvu**.

    ![Päringu redigeerimine] (./media/data-lake-store-power-bi/convert-query1.png "Päringu redigeerimine")

11. Kuvatakse faili ikooni, mis tähistab **Drivers.txt** üleslaaditud faili. Paremklõpsake faili ja klõpsake **CSV**.  

    ![Päringu redigeerimine] (./media/data-lake-store-power-bi/convert-query2.png "Päringu redigeerimine")

12. Peaksite nägema väljund, nagu allpool näidatud. Teie andmed on nüüd saadaval vorming, mille abil saate luua.

    ![Päringu redigeerimine] (./media/data-lake-store-power-bi/convert-query3.png "Päringu redigeerimine")

13. Lindi **Avaleht** nuppu **Sule ja Rakenda**ja seejärel klõpsake nuppu **Sule ja rakendamine**.

    ![Päringu redigeerimine] (./media/data-lake-store-power-bi/load-edited-query.png "Päringu redigeerimine")

14. Kui päring on värskendatud, kuvatakse menüü **väljad** uue visualiseeringu jaoks saadaolevad väljad.

    ![Värskendatud väljad] (./media/data-lake-store-power-bi/updated-query-fields.png "Värskendatud väljad")

15. Andke meile tähistada iga linna antud riigi draiverid sektordiagrammi loomine Selleks saate teha järgmisi valikuid.

    1. Klõpsake menüü visualiseeringute nuppu sektordiagrammi sümbol.

        ![Sektordiagrammi loomine] (./media/data-lake-store-power-bi/create-pie-chart.png "Sektordiagrammi loomine")

    2. Veerud, mida me kasutada on **veerg 4** (nimi) ja **veeru 7** (nimi). Lohistage need veerud **väljad** menüü **visualiseeringud** menüü nagu allpool näidatud.

        ![Visualiseeringute loomine] (./media/data-lake-store-power-bi/create-visualizations.png "Visualiseeringute loomine")

    3. Nüüd näeb sektordiagrammi nagu allpool näidatud.

        ![Sektordiagrammi loomine] (./media/data-lake-store-power-bi/pie-chart.png "Visualiseeringute loomine")

16. Valides konkreetses riigis lehe taseme filtrid, näete kohe iga linna valitud riigi juhtide arv. Näiteks valige vahekaardil **visualiseeringute** jaotises **lehe tasemel filtrid**, **Brasiilia**.

    ![Valige riik] (./media/data-lake-store-power-bi/select-country.png "Valige riik")

17. Sektordiagrammi värskendatakse automaatselt kuvada draiverid linnade Brasiilia.

    ![Draiverid riigis] (./media/data-lake-store-power-bi/driver-per-country.png "Iga riigi draiverid")

18. Klõpsake menüü **fail** käsku **Salvesta** visualiseerimine Power BI Desktopi failina salvestada.

## <a name="publish-report-to-power-bi-service"></a>Aruande avaldamine Power BI teenus

Kui olete loonud visualiseerimine Power BI Desktopi, saate seda jagada teistega, avaldades selle teenuse Power BI. Juhised selle kohta, kuidas seda teha, vt [avaldamine Power BI Desktopi kaudu](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Vt ka

* [Lake andmesalve abil andmete Lake Analytics andmete analüüsiks](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
