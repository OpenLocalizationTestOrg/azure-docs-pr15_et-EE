<properties 
   pageTitle="Alustamine Lake andmesalve | Azure'i" 
   description="Portaali abil saate luua Lake andmesalve konto ja Lake andmesalve põhiliste toimingute" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Azure'i Lake andmesalve Azure'i portaalis kasutamise alustamine

> [AZURE.SELECTOR]
- [Portaal](data-lake-store-get-started-portal.md)
- [PowerShelli](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-GA](data-lake-store-get-started-rest-api.md)
- [Azure'i CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Saate teada, kuidas Azure'i portaal abil saate luua konto Azure andmesalve Lake ja põhiliste toimingute näiteks luua kaustu, üles- ja allalaadimine andmefailid, kustutada oma konto jne. Lake andmesalve kohta leiate lisateavet teemast [Ülevaade Azure'i andmed Lake pood](data-lake-store-overview.md).

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

## <a name="do-you-learn-fast-with-videos"></a>Kas saate teada, videote abil kiiresti?

Vaadake järgmisi videoid Lake andmesalve alustamine.

* [Lake andmesalve konto loomine](https://mix.office.com/watch/1k1cycy4l4gen)
* [Andmete Lake andmesalve Exploreriga andmete haldamine](https://mix.office.com/watch/icletrxrh6pc)

## <a name="create-an-azure-data-lake-store-account"></a>Azure'i andmesalve Lake konto loomine

1. Uue [Azure portaali](https://portal.azure.com)sisse logida.

2. Klõpsake nuppu **Uus**, klõpsake **andmete + salvestusruumi**ja klõpsake **Azure andmesalve Lake**. Lugege teavet **Azure'i andmesalve Lake** tera, ja klõpsake nuppu **Loo** tera alumises vasakus nurgas.

3. Pakuvad **Uus andmesalve Lake** tera, väärtused, nagu on näidatud allpool ekraanipildi.

    ![Azure'i andmesalve Lake uue konto loomine] (./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Azure'i andmed Lake uue konto loomine")

    - **Tellimuse**. Valige tellimus, mille alusel soovite Lake andmesalve uue konto loomine.
    - **Ressursirühm**. Valige ressursi olemasolevasse rühma või nuppu **Loo ressursirühma** loomist. Ressursirühma on ümbris, mis on seotud ressursid rakenduse. Lisateavet leiate teemast [Ressursi rühmade Azure](azure-resource-manager/resource-group-overview.md#resource-groups).
    - **Asukoht**: valige asukoht, kuhu soovite luua Lake andmesalve konto.

4. Valige **Kinnita Startboard** , kui soovite Lake andmesalve konto, mida soovite selle Startboard juurde pääsema.

5. Klõpsake nuppu **Loo**. Kui valisite konto, mida soovite selle startboard kinnitada, on tagasi suunatakse soovitud startboard ja te näete edenemise Lake andmesalve konto ettevalmistamise. Kui andmesalve Lake konto on ette valmistatud, näitab konto tera üles.

6. Laiendage **Essentialsi** drop-down Lake andmesalve konto teabe nagu ressursi rühmitage on osa, asukoht, jne. Klõpsake **Kiirkäivituse** muud ressursid, mis on seotud Lake andmesalve linkide kuvamiseks ikooni.

    ![Teie Azure'i andmesalve Lake konto] (./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Teie Azure'i andmed Lake konto")

## <a name="createfolder"></a>Azure'i andmesalve Lake konto kaustade loomine

Saate luua kaustu jaotises konto Lake andmesalve haldamiseks ja andmete talletamiseks.

1. Avage äsja loodud Lake andmesalve konto. Vasakul paanil klõpsake nuppu **Sirvi**, klõpsake **Lake andmesalve**ja klõpsake keelest Lake andmesalve konto nime, mille alusel soovite luua kaustu. Kui teil on kinnitatud konto, mida soovite selle startboard, klõpsake selle konto paani.

2. Klõpsake oma Lake andmesalve konto blade **Andmete Explorer**.

    ![Lake andmesalve konto kaustade loomine] (./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Lake andmesalve konto kaustade loomine")

3. Lake andmesalve konto tera, klõpsake käsku **Uus kaust**, sisestage uue kausta nimi ja seejärel klõpsake nuppu **OK**.
    
    ![Lake andmesalve konto kaustade loomine] (./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Lake andmesalve konto kaustade loomine")
    
    Äsja loodud kausta on loetletud **Andmete Explorer** tera. Saate luua pesastatud kaustu kuni igale tasandile.

    ![Andmete Lake konto kaustade loomine] (./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Andmete Lake konto kaustade loomine")


## <a name="uploaddata"></a>Azure'i andmesalve Lake konto andmete üleslaadimine

Saate üles laadida oma andmed otse juurtasemel konto Azure andmesalve Lake või kontol loodud kausta. Ekraanipildi all, klõpsake juhiste abil faili üleslaadimine alamkaust keelest **Andmete Explorer** . See ekraanipildi faili üleslaadimisel sub kausta, mis on näidatud lingiridade, (punane ruut märgitud).

Kui otsite Näidisandmete üles laadida, saate kausta **Kiirabi andmed** [Azure'i andmed Lake Git hoidla](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

![Andmete üleslaadimine] (./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Andmete üleslaadimine")


## <a name="properties"></a>Atribuudid ja salvestatud andmeid saadaolevad toimingud

Klõpsake äsja lisatud faili avada tera **Atribuudid** . Faili ja toiminguid saate teha failiga seostatud atribuudid on saadaval selle tera. Azure'i andmesalve Lake konto esile tõstetud allpool ekraanipildi punane väljale saate kopeerida ka faili täielik tee.

![Klõpsake menüüs andmed atribuudid] (./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Klõpsake menüüs andmed atribuudid")

* Klõpsake nuppu **Eelvaade** otse brauseri kaudu faili eelvaate kuvamiseks. Saate määrata ka eelvaate vorming. Klõpsake nuppu **Eelvaade**, klõpsake **vormingus** **Faili eelvaate** tera ja **Eelvaade failivormingus** tera määrake soovitud suvandid, nt ridade arvu kuvamiseks kodeerimine kasutamiseks eraldaja jne.

  ![Failivorming eelvaade] (./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Failivorming eelvaade")

* Klõpsake faili allalaadimiseks oma arvutisse **alla laadida** .

* Klõpsake **faili ümber nimetada** faili ümber nimetada.

* Klõpsake **faili kustutada** faili kustutada.


## <a name="secure-your-data"></a>Kaitsta teie andmeid

Saate tagada Azure andmesalve Lake kontole juurdepääsemine Azure Active Directory ja juurdepääsu reguleerimine (ACL) talletatud andmed. Juhised selle kohta, kuidas seda teha, leiate [Azure'i andmesalve Lake andmete turvamine](data-lake-store-secure-data.md).


## <a name="delete-azure-data-lake-store-account"></a>Kustuta Azure'i andmesalve konto

Teie Lake andmesalve tera Azure'i andmesalve Lake konto kustutamiseks klõpsake nuppu **Kustuta**. Toimingu kinnitamiseks, palutakse teil sisestada nimi konto, mille soovite kustutada. Sisestage konto nimi ja seejärel klõpsake käsku **Kustuta**.

![Kustuta andmete Lake konto] (./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Kustuta andmete Lake konto")


## <a name="next-steps"></a>Järgmised sammud

- [Turvaline andmete andmesalve Lake](data-lake-store-secure-data.md)
- [Azure'i andmeanalüüsi Lake Lake andmesalve kasutamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Lake andmesalve Azure Hdinsightiga kasutamine](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Accessi andmete Lake poe diagnostikalogid](data-lake-store-diagnostic-logs.md)
