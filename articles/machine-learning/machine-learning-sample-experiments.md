<properties
    pageTitle="Õppekeskuse valimi katsete koopia arvutisse | Microsoft Azure'i"
    description="Saate teada, kuidas valimi masina Õppekeskuse katsete abil saate luua uue katsete Cortana ärianalüüsi Galerii ja Microsoft Azure'i masina Õppekeskuse."
    services="machine-learning"
    documentationCenter=""
    authors="cjgronlund"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="cgronlun;chhavib;olgali"/>

# <a name="copy-sample-experiments-to-create-new-machine-learning-experiments"></a>Kopeerige valimi katsed luua uue masina Õppekeskuse katsete
Saate teada, kuidas alustada valimi katsete [Cortana ärianalüüsi](http://gallery.cortanaintelligence.com/) galeriist loomine algusest peale arvuti õ katsete asemel. Näidiste abil saate koostada oma seadme Õppekeskuse lahendus.

Galerii on valimi katsete Microsoft Azure'i masina õ meeskonnatöö kui ka arvuti õ ühenduse ühiskasutuses näidised. Samuti saate esitada küsimusi või kommenteerida katsete kohta.

Vaadake, kuidas kasutada Galerii, vaadake selle 3-minutilise video [kopeerimine teiste inimeste tööd teha andmete teadus](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) sarja [Andmete teadus algajatele](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="find-an-experiment-to-copy-in-cortana-intelligence-gallery"></a>Cortana ärianalüüsi galeriis kopeerimiseks katse otsimine

Vaadata, millised katsed on saadaval, avage [Galerii](http://gallery.cortanaintelligence.com/) ja klõpsake lehe ülaosas **katsete** .

### <a name="find-the-newest-or-most-popular-experiments"></a>Uuematest või kõige populaarsemate katsete otsimine

Sellel lehel saate vaadata **hiljuti lisatud** katsete või liikuge kerides allapoole jaotiseni vaadata, **mis on populaarsed** või viimaste **populaarsed Microsoft katsete**.

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a>Otsige katse, mis vastab teatud

Kõik katsed sirvida:

1. Klõpsake nuppu **Sirvi kõiki** lehe ülaosas.
2. Valige jaotises **Täpsemalt piiritlemiseks**, **katse** , et näha kõiki katsete galeriis.
3. Leiate katsete, mis vastavad teie vajadustele paar võimalust:
    * **Valige vasakul filtrid.** Katsete on normaalne PKL-põhise tuvastusalgoritm kasutavate sirvimiseks valige **katse** jaotises **Kategooriad**ja **PCA-Based normaalne tuvastamise** **Kasutatud algoritmide kohta**. (Kui te ei näe seda algoritmi, klõpsake käsku **Kuva kõik** loendi allservas.)<br></br>
      ![](./media/machine-learning-sample-experiments/refine-the-view.png)
    *  **Kasutage otsinguvälja.** Näiteks katsete numbri tuvastus seotud Microsofti poolt kahe-klassi tugi vektorkuju masina algoritmi kasutavate leidmiseks sisestage "numbri tuvastus" otsinguväljale. Valige **katse**, **ainult Microsoft sisu**ja **Kahe-klassi tugi vektorkuju seadme**filtrid:![](./media/machine-learning-sample-experiments/search-for-experiments.png) 
4. Klõpsake nuppu katse kohta leiate lisateavet.
5. Käivitamine ja/või katse muutmiseks klõpsake käsku **Ava Studios** on katse lehel.

    > [AZURE.NOTE] Avage seadme õ Studios katse, peate logige sisse oma Microsofti konto identimisteave. Kui teil pole veel masina õ tööruumi, luuakse tasuta prooviversiooni tööruumi. [Siit saate teada, kaasatava masina õ tasuta prooviversioon](https://azure.microsoft.com/pricing/details/machine-learning/)

    ![](./media/machine-learning-sample-experiments/example-experiment.png) 


## <a name="use-a-template-in-machine-learning-studio"></a>Seadme õ Studios malli kasutamine

Ka saate luua uue katse masina õ Studio abil Galerii valimi mallina.

1. Logige sisse oma Microsofti konto identimisteave [Studio](https://studio.azureml.net)ja seejärel klõpsake nuppu **Uus** , et luua uus katse.
2. Sirvige valimi sisu ja klõpsake.

Luuakse uus katse oma tööruumi kasutamise katse valimi mallina.

## <a name="next-steps"></a>Järgmised sammud
- [Oma andmete ettevalmistamine](machine-learning-data-science-import-data.md)
- [Proovige oma katse R](machine-learning-r-quickstart.md)
- [Vaadake üle valimi R katsete](machine-learning-r-csharp-web-service-examples.md)
- [Veebi teenus API loomine](machine-learning-publish-a-machine-learning-web-service.md)
- [Olete valmis kasutada rakendusi sirvida](https://datamarket.azure.com/browse?query=machine+learning)
