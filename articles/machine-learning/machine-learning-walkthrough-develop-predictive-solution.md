<properties
    pageTitle="Krediitkaardiga masina õ seotud ohtude sõnastikupõhise lahendus | Microsoft Azure'i"
    description="Näitab, kuidas luua ennustav lahendus krediitkaardiga riskianalüüs Azure seadme õ Studio üksikasjaliku selgituse."
    keywords="krediitkaardiga risk, ennustav lahenduse, riskianalüüs"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Juhend: Arendamise krediitkaardiga riskianalüüs Azure seadme õppe ennustav lahendus

Oletame, et teil on vaja inimese krediitkaardiga risk annavad krediitkaardiga rakendus teabe põhjal prognoosida.  

Krediitkaardiga riskianalüüs on probleem, muidugi, kuid vaatame veidi lihtsustada küsimus parameetrid. Seejärel me saate seda kasutada näide selle kohta, kuidas saate kasutada Microsoft Azure'i masina õ masina õ Studio ja seadme õ veebiteenuse ennustav lahenduse loomiseks.  

Klõpsake selle üksikasjaliku selgituse jälgime ennustav mudeli masina õ Studios arendamise ja seejärel rakendades Azure seadme õ web teenust. Me kuvatakse käivitamine avalikult kättesaadavaks krediitkaardiga risk andmetega, arendamise ja koolitada prognoositava mudeli, et andmeid ja seejärel juurutada mudeli nimega veebiteenus, mida saate kasutada teised krediitkaardiga riskianalüüs.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<!-- -->

>[AZURE.TIP] Alla laadida ning printida skeem, mis annab ülevaate arvuti õ Studio võimaluste, leiate [Azure'i masina õ Studio võimaluste ülevaade diagramm](machine-learning-studio-overview-diagram.md).

Krediitkaardiga risk assessment lahenduse loomiseks tuleb meil tehke järgmist.  

1.  [Seadme õ tööruumi loomine](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Laadige üles olemasolevad andmed](machine-learning-walkthrough-2-upload-data.md)
3.  [Looge uus katse](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Koolitada ja hindate mudelid](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Veebiteenuse juurutamine](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Accessi veebiteenuse](machine-learning-walkthrough-6-access-web-service.md)

Juhendis põhineb lihtsustatud versioon on [kahendarvu klassifikatsioon: krediidi risk ennustamine](http://go.microsoft.com/fwlink/?LinkID=525270) valimi katse [Cortana ärianalüüsi Galerii](http://gallery.cortanaintelligence.com/).
