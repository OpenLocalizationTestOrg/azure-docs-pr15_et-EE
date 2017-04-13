<properties
   pageTitle="Veebiteenuse skaleerimist | Microsoft Azure'i"
   description="Saate teada, kuidas mastaapimiseks veebiteenuse suurendades kokkulangevus ja lisada uue lõpp-punktid."
   services="machine-learning"
   documentationCenter=""
   authors="neerajkh"
   manager="srikants"
   editor="cgronlun"
   keywords="Azure'i masina Õppekeskuse, web services, tulemustabeli kasutuselevõtmise üle ka, skaala lõpp-punkti, kokkulangevus"
   />
<tags
   ms.service="machine-learning"
   ms.devlang="NA"
   ms.workload="data-services"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.date="10/05/2016"
   ms.author="neerajkh"/>

# <a name="scaling-a-web-service"></a>Skaleerimist edastamine veebiteenusele

>[AZURE.NOTE] Selles teemas kirjeldatakse meetodite kehtivad klassikaline masina õ veebiteenuse abil. 

Vaikimisi iga avaldatud veebiteenuse on konfigureeritud toetama 20 samaaegseid taotlusi ja võib olla kuni 200 samaaegseid taotlusi. Azure'i klassikaline portaali pakub võimalus Määrake selle väärtuseks, Azure seadme õ optimeerib automaatselt sätte parima jõudluse oma veebiteenuse, et ja portaali väärtus ignoreeritakse. 

Kui leping API 200 Max samaaegseid kõned väärtusest suurem koormusega helistamiseks ei toeta, peaksite looma sama teenuses mitme lõpp-punktid. Saate siis CEIP levitada oma laadi kõigis neid.

## <a name="add-new-endpoints-for-same-web-service"></a>Uute lõpp-punktide jaoks sama veebiteenuse lisamine

Skaala muutmise kohta veebiteenuse on ühine ülesanne. Mõned põhjused, miks skaala on üle 200 samaaegseid taotlusi toetavad, kättesaadavust läbi mitme lõpp-punktid või Sisestage veebiteenuse eraldi lõpp-punktid. Saate suurendada, lisades täiendavaid lõpp-punktid [Azure klassikaline portaali](https://manage.windowsazure.com/) või [Azure seadme õ veebiteenuse](https://services.azureml.net/) portaali kaudu sama veebiteenuse skaala.

Uue lõpp-punktid lisamise kohta leiate lisateavet teemast [Loomise lõpp-punktid](machine-learning-create-endpoint.md).

Pidage meeles, mis abil kõrge kokkulangevus count võib kahjustada, kui helistate API vastavalt kõrge määra. Võite kohata juhuslik ajalõpud ja/või diagrammi latentsus kui suhteliselt laadi sellele API suure koormuse jaoks konfigureeritud.

Sünkroonse API-d kasutatakse tavaliselt olukordades, kus on soovitud madal latentsus. Latentsus siin tähendab aega kulub API ühe taotlust lõpule viia, ja ei moodustavad võrgu viivitusi. Oletame, et teil on API koos 50-ms latentsus. Täielikult kasutamine saadaval läbilaskevõime ahendamise taseme kõrge ja Max samaaegseid kõned = 20, kellele peate helistama selle API 20 * 1000 / 50 = 400 korda sekundis. Pikendatakse seda edasi, Max samaaegseid kõned 200 võimaldab teil helistada API 4000 korda sekundis, eeldades, et 50-ms latentsus.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
