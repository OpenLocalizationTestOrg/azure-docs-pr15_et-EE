<properties
    pageTitle="Alustamine voo Analytics: reaalajas pettuse tuvastamise | Microsoft Azure'i"
    description="Saate teada, kuidas luua reaalajas pettuse tuvastamise lahenduse voo Analytics. Kasutage mõnda sündmust jaoturi reaalajas sündmuse töötlemiseks."
    keywords="Normaalne avastamine, pettuse avastamine, reaalajas normaalne automaattuvastus"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok" />



# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Azure'i voo Analyticsi kasutamise alustamine: reaalajas pettuse avastamine

Saate teada, kuidas luua-lõpuni lahenduse reaalajas pettuse avastamiseks Azure'i voo Analytics. Sündmuste toomiseks on Azure sündmuse jaoturi, kirjutada voo Analytics päringuid koondamine või teavitamine ja tulemuste saatmiseks on väljundi valamu, et saada ülevaade andmete reaalajas töötlemise üle. Reaalajas normaalne avastamine side kuulub, kuid sobib näide tehnika ka muud tüüpi pettuse avastamine, nt krediitkaardi või identiteedi varastamine stsenaariumi.

Voo Analytics on täielikult hallatav teenus, mis hõlmab latentsusajaga, väga kättesaadav, scalable keerukate sündmuse töötlemiseks üle streaming andmed pilveteenuses. Lisateavet leiate teemast [Sissejuhatus Azure'i voo Analytics](stream-analytics-introduction.md).


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Stsenaarium: Side ja SIM pettuse tuvastamise reaalajas

Side ettevõttel suurel hulgal andmete sissetulevate kõnede jaoks. Ettevõtte peab oma andmete põhjal järgmist.
* Arvu vähendada; andmed allapoole mõistliku summa ja saada geograafiliste piirkondade ja ülevaateid kliendi kasutamise kohta.
* Tuvasta SIM pettuse (mitme helistaja umbes samal ajal, kuid geograafiliselt eri asukohtades olevate sama ID) reaalajas nii, et kliendid andmise või teenuse sulgemise hõlpsasti vastata.

Kanoonilise Internet, asjade stsenaariumide on tonn telemeetria või sensori andmete loodud – ja klientide soovite need liitmine või teavita kõrvalekaldeid reaalajas üle.

## <a name="prerequisites"></a>Eeltingimused

- Microsofti allalaadimiskeskusest alla laadida [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) 
- Valikuline: Lähtekoodi [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/DataGenerators/TelcoGenerator) sündmuse generaator

## <a name="create-an-azure-event-hubs-input-and-consumer-group"></a>Azure'i sündmuse jaoturi sisend ja tarbija rühma loomine

Valimi rakendus on luua sündmuste ja murravad eksemplari sündmuste jaoturi reaalajas töötlemiseks. Sündmuse jaoturi [Azure'i teenus siini](/documentation/services/service-bus/)dokumentatsiooni kohta leiate teavet teemast Lisateave teenuse siini sündmuse jaoturi on sündmuse manustamisest voo Analyticsi eelistatud meetod.

Mõni sündmus keskuse loomiseks tehke järgmist.

1.  [Azure'i portaalis](https://manage.windowsazure.com/) nuppu **Uus** > **Rakenduse teenuste** > **Teenuse siini** > **Sündmuse jaoturi** > **Kiiresti luua**. Sisestage nimi, piirkond ja uue sündmuse keskuse loomiseks uuel või olemasoleval nimeruum.  
2.  Hea tava, lugege iga voo Analytics töö ühe sündmuse jaoturi tarbija rühma. Kas tutvustame teile allpool tarbija rühma loomise protsess ja saate [Lisateavet rühmi](https://msdn.microsoft.com/library/azure/dn836025.aspx). Tarbija rühma loomiseks avage äsja loodud sündmuse jaoturi ja klõpsake vahekaarti **Rühmi** ja seejärel klõpsake nuppu **Loo** lehe allservas ja tarbija rühma nimi.
3.  Sündmuse jaoturi juurdepääsu andmiseks vajame ühiskasutusega juurdepääsu poliitika loomiseks.  Oma sündmuse jaoturi vahekaarti **konfigureerimine** .
4.  Jaotises **Ühiskasutuses Juurdepääsupoliitikaid**, saate luua uue poliitika õiguste **haldamine** .

    ![Ühiskasutusse antud Juurdepääsupoliitikaid, kus saate luua poliitika õigustega haldamine.](./media/stream-analytics-get-started/stream-ananlytics-shared-access-policies.png)

5.  Klõpsake lehe allosas **salvestada** .
6.  Liikuda **armatuurlaua** ja **Ühenduseteavet** lehe allosas nuppu ja seejärel kopeerige ja salvestage ühendusteabe.

## <a name="configure-and-start-event-generator-application"></a>Sündmuse generaator rakenduse käivitamiseks ja konfigureerimiseks

Oleme pakkunud klientrakendusega, mis luua valimi sissetulevate kõnede metaandmete ja lükake see sündmus jaoturi. Järgige selle rakenduse häälestamiseks allpool toodud juhiseid.  

1.  [TelcoGenerator.zip faili](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip)alla laadida. Siis lahti kataloogi.

    **Märkus**: Windows võib blokeerida alla zip-fail. Paremklõpsake faili ja valige Atribuudid. Kui sõnum "See fail pärineb mõnest teisest arvutist ja võib olla blokeeritud kaitsmiseks selle arvuti." seejärel märkige ruut "Aktiveeri" ja klõpsake nuppu Rakenda zip-fail.

2.  **Telcodatagen.exe.config** Microsoft.ServiceBus.ConnectionString ja EventHubName väärtuste asendamine oma sündmuse jaoturi ühendusstringi ja nimi.

    **Märkus**: ühendusstringi kopeeritud Azure portaali kohast nime lõpus ühendus. Eemaldage kindlasti on "; EntityPath =<value>"lisa Key = välja.

3.  Käivitage rakendus. Funktsiooni kasutamine on järgmine:

   telcodatagen.exe [#NumCDRsPerHour] [SIM kaart pettuse tõenäosuse] [#DurationHours]

Järgmises näites toob 1000 sündmuste 20% on tõenäosuse pettuse 2 tunni jooksul.

    telcodatagen.exe 1000 .2 2

Kuvatakse kirjed, mis saadetakse teie sündmuse jaoturi. Mõned võtmeväljade, mida me kasutame reaalajas pettuse tuvastamise selles rakenduses on määratletud siin.

| Kirje | Määratlus |
| ------------- | ------------- |
| CallrecTime | Kõne aeg ajatemplit. |
| SwitchNum | Kõne ühendatakse telefoni aktiveerimine. |
| CallingNum | Funktsiooni helistaja telefoninumber. |
| CallingIMSI | Rahvusvaheline mobiilsideseadmete abonendi identiteedi (IMSI).  Funktsiooni helistaja ainuidentifikaator. |
| CalledNum | Kõne adressaadi telefoninumber. |
| CalledIMSI | Rahvusvaheline mobiilsideseadmete abonendi identiteedi (IMSI).  Adressaadile ainuidentifikaator. |


## <a name="create-stream-analytics-job"></a>Voo Analytics töö loomine
Nüüd kus oleme voo side sündmuste, me seadistada voo Analytics töö neid sündmusi reaalajas analüüsimiseks.

### <a name="provision-a-stream-analytics-job"></a>Voo Analytics töö ettevalmistamine

1.  Azure'i portaalis, klõpsake **uus > Data Services > voo Analytics > kiiresti luua**.
2.  Määrake järgmised väärtused ja seejärel **Luua voo Analytics töö**.

    * **Töö nimi**: sisestage töö nimi.

    * **Piirkond**: valige piirkond, kuhu soovite töö. Kaaluma töö ja sündmuse jaoturi sama piirkonna parema jõudluse tagamiseks ja veenduge, et te ei maksta andmete edastamiseks piirkondade vahel.

    * **Salvestusruumi konto**: valige Azure storage konto, mida soovite kasutada kõigi voo Analytics töid selle piirkonna jaoks jälgimisega seotud andmete talletamiseks. Teil on võimalik luua uue või olemasoleva salvestusruumi konto valimiseks.

3.  Klõpsake vasakpoolsel paanil voo Analytics tööde nimekirja **Voo Analytics** .

    ![Voo Analytics teenuse ikoon](./media/stream-analytics-get-started/stream-analytics-service-icon.png)

4.  Uue töö kuvatakse olek on **loodud**. Pange tähele, et lehe allservas nuppu **Start** on keelatud. Enne töö alustamist tuleb konfigureerida töö sisendi, väljundi ja päringu.

### <a name="specify-job-input"></a>Määrake töö sisendit
1.  Voo Analytics tööpäevad **sisendina** lehe ülaosas nuppu ja klõpsake **Sisestusmeetodi lisamine**. Avanevas dialoogiboksis annab teile kaudu seadistamine sisendit arv.
2.  Valige **Andmevoos**ja seejärel klõpsake nuppu paremale.
3.  Valige **Sündmuse keskus**ja seejärel nuppu paremale.
4.  Tippige või valige kolmanda lehel järgmised väärtused:

    * **Sisestuskeel alias (pseudonüüm)**: Sisestage sõbralik nimi selle töö, nt *CallStream*sisestada. Pange tähele, et kasutate selle nimi päringu hiljem.
    * **Sündmuse jaoturi**: kui teie loodud sündmuse jaoturi on sama tellimuse voo Analytics töö, valige sündmuse jaoturi nimeruumi.

    Kui teie sündmuse jaoturi on mõni muu tellimus, valige **Kasuta sündmuse keskus teise tellimusest** ja **Teenuse siini Namespace**, **Sündmuse jaoturi nimi**, **Sündmuse jaoturi poliitika nimi**, **Sündmuse jaoturi poliitika võti**ja **Sündmuse jaoturi Partition Count**teabe käsitsi sisestamiseks.

    * **Sündmuse jaoturi nimi**: valige jaoturi sündmuse nimi.

    * **Sündmuse jaoturi poliitika nimi**: märkige selles õpetuses loodud sündmuse-jaoturi poliitika.

    * **Sündmuse jaoturi tarbija rühma**: tarbija jaotis, mis on loodud selles õpetuses tüüp.
5.  Klõpsake paremal.
6.  Määrake järgmised väärtused:

    * **Sündmuse Serializer vorming**: JSON
    * **Kodeerimine**: UTF8
7.  Klõpsake nuppu kontrolli selle andmeallika lisamiseks ja kinnitamiseks, et voo Analytics saate ühendada sündmuse jaoturi.

### <a name="specify-job-query"></a>Määrake töö päring

Voo Analytics toetab lihtne, deklaratiivseid päringu mudeli kirjeldamiseks teisendused reaalajas töötlemiseks. Keele kohta leiate lisateavet teemast [Azure voo Analytics päringu keel viide](https://msdn.microsoft.com/library/dn834998.aspx). Selle õpetuse aitab teil koostamine ja päringute Testige oma kõne reaalajas andmevoo üle.

#### <a name="optional-sample-input-data"></a>Valikuline: Sisestuskeel näidisandmed
Tegelik töö andmetega päringu kinnitamiseks saate funktsiooni **Näidisandmete** oma voo sündmuste ekstraktida ja luua lisamine. JSON-faili sündmuste testimiseks.  Järgmised toimingud näitab, kuidas seda teha ja ka esitanud [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) näidisfaili katsetamiseks.

1.  Valige sündmuse jaoturi sisendit ning **Näidisandmete** lehe allosas.
2.  Kuvatavas dialoogiboksis Määrake **Algusaeg** alustamiseks koguda andmeid ja **kestus** , kui palju kasutamine lisaandmete saamiseks.
3.  Sisend valimite andmete käivitamiseks nuppu kontrolli.  Võib kuluda minut või kaks andmefaili tuleb luua.  Kui protsess on lõpule jõudnud, klõpsake nuppu **üksikasjad** ja laadige alla ja salvestage soovitud. JSON loodud fail.

    ![Laadige alla ja salvestage töödeldud andmete JSON-failis](./media/stream-analytics-get-started/stream-analytics-download-save-json-file.png)

#### <a name="passthrough-query"></a>PassThrough-päring

Kui soovite arhiivida iga sündmuse, saate kõiki välju last sündmuse või sõnumi lugemiseks passthrough-päring. Kõigepealt teha lihtsa passthrough päringu selle projektide sündmuse kõik väljad.

1.  **Päringu** Kasutusanalüüsi voo töö lehe ülaosas nuppu.
2.  Lisage järgmine kood redaktori:

        SELECT * FROM CallStream

    > Veenduge, et Sisestuskeel allika nimi vastab teie määratud varem sisend nime.

3.  Klõpsake jaotises päringuredaktor nuppu **testi** .
4.  Esita Proovifail, kas üks eelmisi juhiseid abil loodud või kasutada [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json).
5.  Klõpsake nuppu kontrolli ja vaadake tulemused kuvatakse allpool päringu definitsiooni.

    ![Päringutulemite määratlus](./media/stream-analytics-get-started/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Veeru projektsiooni

Meil kuvatakse nüüd arvu vähendada; väiksema tagastatud väljad.

1.  Päringu koodi redigeerija muutmiseks tehke järgmist.

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  Klõpsake jaotises päringu tulemite kuvamiseks päringuredaktoris **Käivita uuesti** .

    ![Päringuredaktori väljund.](./media/stream-analytics-get-started/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Arvu sissetulevad kõned regiooniti: langema aken, kus koondamine

Võrdlemiseks summa selle sissetulevate kõnede müüginäitajaid piirkonna kohta me saate kasutada [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) saada arvu sissetulevate kõnede Rühmitusalus SwitchNum iga 5 sekundi järel.

1.  Päringu koodi redigeerija muutmiseks tehke järgmist.

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    See päring kasutab **Ajatempli,** märksõna last ajaline arvutamisel kasutatava ajatempli välja määramiseks. Kui sellel väljal pole määratud, tuleks akendesüsteemide toimingut teha kellaaega iga sündmuse jõudis sündmuse jaoturi abil. Vt ["Saabumisel Vs rakenduse aeg" voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Pange tähele, et atribuudi **System.Timestamp** abil pääsete juurde ajatempel iga akna lõppu.

2.  Klõpsake jaotises päringu tulemite kuvamiseks päringuredaktoris **Käivita uuesti** .

    ![Päringutulemite Timestand järgi](./media/stream-analytics-get-started/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>SIM pettuse tuvastamise koos iseendaga ühendamine

Potentsiaalselt pettusega kasutus tuvastamiseks vaatame kõnede sama kasutaja, kuid erinevatest asukohtadest pärinevate vähem kui 5 minutit.  Me [Liitu](https://msdn.microsoft.com/library/azure/dn835026.aspx) voo kõne sündmuste iseendaga sellisel juhul otsida.

1.  Päringu koodi redigeerija muutmiseks tehke järgmist.

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  Klõpsake jaotises päringu tulemite kuvamiseks päringuredaktoris **Käivita uuesti** .

    ![Päringutulemite ühenduse](./media/stream-analytics-get-started/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Väljundi valamu loomine

Nüüd, kui meil on määratletud sündmuse voo, on sündmuse jaoturi neelata sündmuste ja päringu sisendi täitmiseks üle viia, on teisendus viimases etapis on määratleda mõne väljundi valamu töö.  Me kirjutada bloobimälu sündmused pettusega käitumist.

Järgige juhiseid, et luua bloobimälu ümbris, kui teil pole veel üks.

1.  Olemasoleva salvestusruumi konto kasutamine või salvestusruumi uue konto loomiseks klõpsake ikooni **uus > DATA SERVICES > SALVESTUSRUUM > kiiresti luua** ja juhiseid järgides.
2.  Valige konto, salvestusruumi, **ÜMBRISTE** lehe ülaservas nuppu ja seejärel klõpsake nuppu **Lisa**.
3.  Määrake oma container **nimi** ja seadke avaliku bloobimälu oma **ACCESSI** .

## <a name="specify-job-output"></a>Määrake töö väljund

1.  Oma töö voo Analytics **väljundi** lehe ülaosas nuppu ja klõpsake **Väljundi lisada**. Avanevas dialoogiboksis annab teile juhised häälestamiseks oma väljundi mitmete.
2.  Valige **BLOOBIMÄLU**ja seejärel klõpsake nuppu paremale.
3.  Tippige või valige kolmanda lehel järgmised väärtused:

    * **Väljundi alias (PSEUDONÜÜM)**: Sisestage sõbralik nimi selle töö väljund.
    * **Tellimus**: kui bloobimälu, mille lõite on sama tellimuse voo Analytics töö, valige **Kasuta salvestusruumi konto praeguselt tellimuselt**. Kui salvestusruumi on mõni muu tellimus, valige **Kasuta salvestusruumi konto teise tellimusest** ja **Salvestusruumi konto**, **Salvestusruumi konto võti**, **CONTAINER**teabe käsitsi sisestamiseks.
    * **Salvestusruumi konto**: valige salvestusruumi konto nimi.
    * **CONTAINER**: valige ümbris nimi.
    * **FAILINIME EESLIIDE**: tippige faili eesliite bloobimälu väljundi kirjutamisel kasutada.

4.  Klõpsake paremal.
5.  Määrake järgmised väärtused:

    * **Sündmuse SERIALIZER vorming**: JSON
    * **Kodeerimine**: UTF8

6.  Klõpsake nuppu kontrolli selle andmeallika lisamiseks ja kinnitamiseks, et voo Analytics saate ühendada salvestusruumi konto.

## <a name="start-job-for-real-time-processing"></a>Reaalajas töötlemiseks töö alustamine

Kuna töö input, päringus ja väljundi kõik pole määratud, oleme reaalajas pettuse tuvastamise voo Analytics töö alustamiseks valmis.

1.  Klõpsake **ARMATUURLAUA**töö **alustamiseks** klõpsake lehe allosas.
2.  Kuvatavas dialoogiboksis valige **töö ALGUSAEG** ja seejärel klõpsake dialoogiboksi allservas nuppu kontrolli. Töö olek muutub **algus** ja liigub varsti **töötab**.

## <a name="view-fraud-detection-output"></a>Vaate pettuse tuvastamise väljund

Tööriista nagu [Azure'i salvestusruumi Exploreri](https://azurestorageexplorer.codeplex.com/) või [Azure Exploreri](http://www.cerebrata.com/products/azure-explorer/introduction) abil saate vaadata pettusega sündmused, nagu nad kirjutada oma väljund reaalajas.  

![Pettuse tuvastamise: pettusega sündmuste vaadata reaalajas](./media/stream-analytics-get-started/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Tootetugi
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
