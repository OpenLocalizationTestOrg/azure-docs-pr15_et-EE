<properties
    pageTitle="Exceli lisandmoodul masina õ veebiteenuste jaoks | Microsoft Azure'i"
    description="Kuidas kasutada Azure seadme õ veebiteenused otse Excelis ilma koodi kirjutamata."
    services="machine-learning"
    documentationCenter=""
    authors="tedway"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/05/2016"
    ms.author="tedway;garye" />

# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Exceli lisandmooduli Azure seadme õ veebiteenuste jaoks

Exceli abil on lihtne kõne veebiteenused otse koodi kirjutamiseks vajamata.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Mõne olemasoleva veebiteenuse kasutamiseks töövihikus

1. Avage [Exceli näidisfaili](http://aka.ms/amlexcel-sample-2), mis sisaldab Exceli lisandmooduli ja aspektide kohta Titanic andmeid.
2. Veebiteenuse valimiseks klõpsake seda – "Titanic Survivor ennustaja (Exceli lisandmooduli näidis) [Keskmine]" selles näites.

    ![Valige veebiteenus][01]

3. See viib teid **Predict** jaotis.  See töövihik sisaldab juba näidisandmed, kuid tühja töövihiku Excelis valige lahter ja klõpsake **andmete kasutamise näidis**.
4. Valige andmed, mille päised ja klõpsake ikooni sisendandmete vahemik.  Veenduge, et oleks märgitud ruut "Minu andmetel on päised".
5. Sisestage jaotises **väljundi**, lahtris number, millel soovite väljundi olla, näiteks "H1" siin.
6. Klõpsake **prognoosida**.

    ![Prognoosida jaotis][02]

## <a name="steps-to-add-a-new-web-service"></a>Uue veebiteenuse lisamise juhised

Veebiteenuse juurutamine või kasutada mõne olemasoleva veebiteenus. Veebiteenuse juurutamine kohta leiate lisateavet teemast [kiirtutvustus samm 5: Azure'i masina õ veebiteenuse juurutamine](machine-learning-walkthrough-5-publish-web-service.md).

Saada oma veebiteenuse API võti. Kui te seda sõltub sellest, kas teie avaldatud klassikaline masina õ veebiteenuse, uue arvuti õ veebiteenuse.

**Klassikaline veebiteenuse kasutamine** 

1. Seadme õ Studios, klõpsake vasakul paanil jaotises **VEEBITEENUSED** ja valige veebiteenus.

    ![Valige Studio edastamine veebiteenusele][04]

2. Kopeerige veebiteenuse API võti.

    ![Studio API võti][05]

3. **ARMATUURLAUA** vahekaardil veebiteenuse linki **Taotluse/vastuse** .
4. Otsige üles jaotis **Taotlemine URI** .  Kopeerige ja salvestage URL-i.

>[AZURE.NOTE] Nüüd on võimalik saada klassikaline masina õ veebiteenuse API võti [Azure seadme õ veebiteenuste](https://services.azureml.net) portaali sisse logida.

**Uue veebiteenuse kasutamine**

1. [Azure'i masina õ veebiteenuste](https://services.azureml.net) portaalis, klõpsake **Veebiteenuste**ja valige oma veebiteenus. 
2. Klõpsake **tarbimine**.
3. Otsige **lihtsa tarbimine teave** jaotises. Kopeerige ja salvestage **Primaarvõtme** ja **Päringu vastuse** URL-i.


## <a name="steps-to-add-a-new-web-service"></a>Uue veebiteenuse lisamise juhised

1. Veebiteenuse juurutamine või kasutada mõne olemasoleva veebiteenus. Veebiteenuse juurutamine kohta leiate lisateavet teemast [kiirtutvustus samm 5: Azure'i masina õ veebiteenuse juurutamine](machine-learning-walkthrough-5-publish-web-service.md).
2. Klõpsake **tarbimine**.
3. Otsige **lihtsa tarbimine teave** jaotises. Kopeerige ja salvestage **Primaarvõtme** ja **Päringu vastuse** URL-i.
2. Excelis, liikuge jaotisse **Veebiteenuste** (kui olete jaotises **Predict** , klõpsake tagasinoolt veebiteenuste loendi avamiseks).

    ![Minge Web teenuse valimine][03]
    
3. Klõpsake **Lisa veebiteenus**.
4. Exceli lisandmooduli teksti väljale **URL-i**URL-i kleepida.
5. Kleepige tekst väljale **API võti**esmane/API võti.
6. Klõpsake nuppu **Lisa**.

    ![Klassikaline veebiteenuse URL-i ja API võti.][06]

10. Veebiteenuse kasutamiseks järgige eelmises, "Juhised kasutamiseks on olemasolev veebiteenuse."

## <a name="sharing-your-workbook"></a>Töövihiku andmine ühiskasutusse

Kui salvestate töövihiku, siis salvestatakse ka API/esmane võti lisasite veebiteenuste jaoks. Mida tähendab, et ainult peaks töövihiku ühiskasutusse üksikisikute usaldate.

Küsimuste esitamine mis tahes kohta järgmisest jaotisest kommentaari või meie [Foorum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
