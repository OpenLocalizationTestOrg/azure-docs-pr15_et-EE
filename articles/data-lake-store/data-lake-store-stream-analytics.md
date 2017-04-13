<properties
   pageTitle="Voona andmete voo Analytics andmete Lake salvestuskohta | Azure'i"
   description="Kasutada Azure voo Analytics andmete voo Azure'i andmed Lake poest"
   services="data-lake-store,stream-analytics" 
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
   ms.date="07/07/2016"
   ms.author="nitinme"/>

# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Azure'i salvestusruumi bloobimälu andmete Lake salvestuskohta abil Azure'i voo Analytics andmete voo

Sellest artiklist saate teada, kuidas kasutada Azure andmesalve Lake väljund Azure'i voo Analytics tööd. Selles artiklis tutvustatakse lihtsa stsenaarium, mis loeb andmed on salvestusruumi Azure'i bloobimälu (input) ja kirjutab andmeid andmesalve Lake (väljund).

>[AZURE.NOTE] Sel ajal, loomine ja konfigureerimine Lake andmesalve väljundeid voo Analyticsi toetatakse ainult [Azure klassikaline portaali](https://manage.windowsazure.com). Seega kasutatakse osa selle õpetuse Azure'i klassikaline portaal.

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

- **Luba Azure tellimuse** andmete Lake poe avaliku eelvaate. Vaadake [juhiseid](data-lake-store-get-started-portal.md#signup).

- **Azure Storage konto**. Kasutage bloobimälu container sellelt kontolt töökohta voo Analytics andmete sisestamiseks. Selles õpetuses endale loote salvestusruumi konto nimega **datalakestoreasa** ja ümbris nimega **datalakestoreasacontainer**kontol. Kui olete loonud ümbris, jaoks Näidisandmete faili üleslaadimine. Saate andmeid näidisfaili [Azure'i andmed Lake Git hoidla](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). Erinevate kliendid, nagu [Azure'i salvestusruumi Exploreri](http://storageexplorer.com/)abil saate andmete üleslaadimine bloobimälu ümbrises.

    >[AZURE.NOTE] Kui loote Azure portaali konto, veenduge, et loote **klassikaline** juurutamise mudeliga. See tagab pääseb salvestusruumi konto Azure klassikaline portaali, kuna see on kasutame voo Analytics töö loomine. Juurde Azure portaali kaudu klassikaline juurutamise salvestusruumi konto loomise juhised leiate teemast [Azure Storage konto](../storage/storage-create-storage-account/#create-a-storage-account).
    >
    > Või saate luua salvestusruumi konto Azure klassikaline portaalist.

- **Azure'i andmesalve Lake konto**. Järgige veebisaidil [Alustamine Azure'i Lake andmesalve Azure'i portaalis](data-lake-store-get-started-portal.md).  


## <a name="create-a-stream-analytics-job"></a>Voo Analytics töö loomine

Alustate voo Analytics tööd, mis sisaldab mõnda Sisestuskeel lähte-kui ka mõnda väljundi loomine. Selles õpetuses allikas on mõni Azure'i bloobimälu container ja sihtkoht on Lake andmesalve.

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)logima.

2. Alt vasakult ekraani, klõpsake nuppu **Uus**, **Data Services**, **Voo Analytics**, **Kiiresti luua**. Sisestage väärtused, nagu allpool näidatud ja seejärel klõpsake nuppu **Loomine voo Analytics töö**.

    ![Voo Analytics töö loomine] (./media/data-lake-store-stream-analytics/create.job.png "Voo Analytics töö loomine")

## <a name="create-a-blob-input-for-the-job"></a>Bloobimälu sisendit töö loomine

1. Avage leht voo Analytics töö, klõpsake vahekaarti **sisendina** ja seejärel klõpsake nuppu **Lisa sisend** viisardi käivitamiseks.

2. Lehel **Lisa sisend oma töö** valige **andmevoos**ja seejärel noolt edasi.

    ![Lisa sisend oma töö] (./media/data-lake-store-stream-analytics/create.input.1.png "Lisa sisend oma töö")

3. Lehel **Lisa andmete voo oma töö** valige **bloobimälu**ja seejärel noolt edasi.

    ![Lisa andmed voo töö] (./media/data-lake-store-stream-analytics/create.input.2.png "Lisa andmed voo töö")

4. Klõpsake lehel **bloobimälu talletusmahu** ette sisendandmete allikana kasutatavat bloobimälu üksikasjad.

    ![Sisestage soovitud bloobimälu salvestusruumi sätted] (./media/data-lake-store-stream-analytics/create.input.3.png "Sisestage soovitud bloobimälu salvestusruumi sätted")

    * **Pseudonüümi Sisestuskeel sisestusklahvi**. See on esitate sisestusmeetodi töö kordumatu nimi.
    * **Valige salvestusruumi konto**. Veenduge, et konto salvestusruumi on sama piirkonna voo Analytics töö või olete tekivad lisatasu andmed piirkondade vahel liikumine.
    * **Sisesta salvestusruumi ümbrises**. Saate luua uue container või valida mõne olemasoleva ümbrises.

    Klõpsake noolt edasi.

5. Lehel **sariväljaanne sätete** seadmine sariväljaanne vormingus **CSV**, **menüü**, eraldaja kodeeringus **UTF8**, ja klõpsake märke.

    ![Sariväljaanne sätted] (./media/data-lake-store-stream-analytics/create.input.4.png "Sariväljaanne sätted")

6. Kui olete lõpetanud, klõpsake viisardi, bloobimälu sisend lisatakse vahekaardil **sisendina** ja **diagnostika** veeru näitama **OK**. Saate ka konkreetselt testida sisend ühendus kasutades allosas nuppu **Testi ühendust** .

## <a name="create-a-data-lake-store-output-for-the-job"></a>Lake andmesalve väljundi töö loomine

1. Avage leht voo Analytics töö, klõpsake vahekaarti **väljundeid** ja seejärel klõpsake nuppu **Lisa väljund** viisardi käivitamiseks.

2. Lehel **Lisa oma töö väljund** valige **Lake andmesalve**ja edasi noolt.

    ![Lisa oma töö väljund] (./media/data-lake-store-stream-analytics/create.output.1.png "Lisa oma töö väljund")

3. **Autoriseerin ühendus** , kui olete Lake andmesalve konto juba loonud, klõpsake lehe **Lubada kohe**. Muul juhul valige **Registreeruge kohe** uue konto loomine. Selles õpetuses mõeldud Oletame, et teil on juba loodud (nagu mainitud eelduseks) Lake andmesalve konto. Teile automaatselt lubatud mandaadiga, kellega te Azure klassikaline portaali sisse logitud.

    ![Lubada andmesalve Lake] (./media/data-lake-store-stream-analytics/create.output.2.png "Lubada andmesalve Lake")

4. **Andmete Lake poe sätete** lehel sisestage teave, nagu on näidatud allpool ekraanipildi.

    ![Määrake andmesalve Lake sätted] (./media/data-lake-store-stream-analytics/create.output.3.png "Määrake andmesalve Lake sätted")

    * **Enter pseudonüümi väljundi**. See on esitate töö väljundi jaoks kordumatu nimi.
    * **Määrake Lake andmesalve konto**. Peaks olema juba loodud, nimetatud nõutav.
    * **Määrake tee eesliite mustri**. See on vajalik väljund faili, mis saavad Lake andmesalve voo Analytics töö tuvastamiseks. Töö kirjutanud väljundeid pealkirjad on GUID vormingus, sh eesliite aitab tuvastada kirjutatud väljundi. Kui soovite arvutamiseks viitesse lisada kuupäeva- ja kellaajatempli eesliite osana veenduge, et lisate `{date}/{time}` eesliite muster. Kui te seda, **Kuupäeva **ja **Kellaaja vormingu** väljad on lubatud ja valige vorming valikut.

    Klõpsake noolt edasi.

5. Lehel **sariväljaanne sätted** seadmine sariväljaanne vormingus **CSV**, **menüü**, eraldaja kodeeringus **UTF8**, ja klõpsake märkeruutu.

    ![Määrake vorming] (./media/data-lake-store-stream-analytics/create.output.4.png "Määrake vorming")

6. Kui olete lõpetanud, klõpsake viisardi, Lake andmesalve väljundi lisatakse vahekaardil **väljundeid** ja **diagnostika** veeru näitama **OK**. Saate ka konkreetselt testida väljund ühendus kasutades allosas nuppu **Testi ühendust** .

## <a name="run-the-stream-analytics-job"></a>Voo Analytics töö

Voo Analytics töö käitamiseks peate käivitama päringu menüü päring. Selles õpetuses mõeldud saate päringut valimi, asendades kohatäiteid sisestusmeetodi töö ja väljund pseudonüümid, nagu on näidatud allpool ekraanipildi.

![Käivita päring] (./media/data-lake-store-stream-analytics/run.query.png "Käivita päring")

Klõpsake nuppu **Salvesta** ekraani allservast ülespoole ja klõpsake nuppu **Alusta**. Dialoogiboksis, valige **Kohandatud aeg**ja seejärel valige soovitud kuupäev varem, nt **1-1-2016**. Klõpsake märkeruutu, et alustada tööd. Võib kuluda kuni paar minutit, et alustada tööd.

![Määrake töö aeg] (./media/data-lake-store-stream-analytics/run.query.2.png "Määrake töö aeg")

Pärast töö käivitub, klõpsake vahekaardil **kuvar** , et näha, kuidas andmete töötlemise.

![Töö jälgimine] (./media/data-lake-store-stream-analytics/run.query.3.png "Töö jälgimine")

Lõpetuseks, saate [Azure portaali](https://portal.azure.com) konto Lake andmesalve avamiseks ja kontrollige, kas andmed on edukalt kirjutatud, et konto.

![Kinnita väljund] (./media/data-lake-store-stream-analytics/run.query.4.png "Kinnita väljund")

Paanil andmete Explorer teade, et väljund on kirjutatud kausta määratletud Lake andmesalve väljastussätted (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Vt ka

* [Mõne Hdinsightiga kobar kasutada Lake andmesalve loomine](data-lake-store-hdinsight-hadoop-use-portal.md)
