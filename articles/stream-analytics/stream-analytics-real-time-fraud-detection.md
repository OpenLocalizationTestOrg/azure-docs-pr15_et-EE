<properties
    pageTitle="Voo Analytics: Reaalajas pettuse tuvastamise | Microsoft Azure'i"
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

Saate teada, kuidas luua-lõpuni lahenduse reaalajas pettuse avastamiseks Azure'i voo Analytics. Sündmuste toomiseks Azure'i sündmuse jaoturi, kirjutage voo Analytics päringud koondamine või teavitamine ja tulemuste saatmiseks on väljundi valamu, et saada teadmisi andmete reaalajas töötlemise üle. Reaalajas normaalne avastamine side on selgitatud, kuid sobib näide tehnika ka muud tüüpi pettuse avastamine, nt krediitkaardi või identiteedi varastamine stsenaariumid.

Voo Analytics on täielikult hallatav teenus, mis pakub latentsusajaga, väga kättesaadav, skaleeritav, keerukate sündmuse töötlemiseks üle streaming andmed pilveteenuses. Lisateavet leiate teemast [Sissejuhatus Azure'i voo Analytics](stream-analytics-introduction.md).


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Stsenaarium: Side ja SIM pettuse tuvastamise reaalajas

Side ettevõttel suurel hulgal andmete sissetulevate kõnede jaoks. Ettevõtte peab oma andmete põhjal järgmist.

* Andmete mõistliku summa vähendamine ja saada ülevaateid kohta klientide kasutamine eri geograafiliste piirkondade lõikes.
* Nii, et ettevõtte saate hõlpsasti vastata teavitades kliendid või teenuse sulgemise tuvastamine reaalajas SIM pettuse (mitme kõned ümber samal ajal, kuid geograafiliselt eri asukohtades olevate sama ID).

Kanoonilise Internet, asjade stsenaariumid on telemeetria või andmeid anduritelt. Klientide soovite andmete liitmine või reaalajas kõrvalekaldeid kohta teatisi.

## <a name="prerequisites"></a>Eeltingimused

- Microsofti allalaadimiskeskusest alla laadida [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip)
- Valikuline: Lähtekoodi [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator) sündmuse generaator

## <a name="create-azure-event-hubs-input-and-consumer-group"></a>Azure'i sündmuse jaoturi sisestus- ja tarbija rühma loomine

Valimi rakendus ei loo sündmused ja murravad eksemplari sündmuste jaoturi reaalajas töötlemiseks. Teenuse siini sündmuse jaoturi on sündmuse manustamisest voo Analyticsi eelistatud meetod. Saate lisateavet sündmuse jaoturi [Azure'i teenus siini](/documentation/services/service-bus/)dokumentatsiooni.

Mõni sündmus keskuse loomiseks tehke järgmist.

1.  [Azure'i portaal](https://manage.windowsazure.com/), klõpsake nuppu **Uus** > **Rakenduse teenuste** > **Teenuse SIINI** > **Sündmuse JAOTURI** > **Kiiresti luua**. Sisestage nimi, piirkond ja uue sündmuse keskuse loomiseks uuel või olemasoleval nimeruum.  
2.  Hea tava, lugege iga voo Analytics töö ühe sündmuse jaoturi tarbija rühma. Me sõelub protsessi hiljem tarbija rühma loomine. [Lisateavet leiate teemast tarbija rühmade kohta](https://msdn.microsoft.com/library/azure/dn836025.aspx). Tarbija rühma loomiseks avage äsja loodud sündmuse jaoturi, klõpsake vahekaarti **RÜHMI** , klõpsake nuppu **Loo** lehe allservas ja seejärel andke oma tarbija rühma jaoks nimi.
3.  Sündmuse jaoturi juurdepääsu andmiseks vajame ühiskasutusega juurdepääsu poliitika loomiseks. Oma sündmuse jaoturi vahekaarti **KONFIGUREERIMINE** .
4.  Jaotises **ÜHISKASUTUSES JUURDEPÄÄSUPOLIITIKAID**luua uue poliitika, mis on õiguste **haldamine** .

    ![Ühiskasutusse antud Juurdepääsupoliitikaid, kus saate luua poliitika õigustega haldamine.](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-shared-access-policies.png)

5.  Klõpsake lehe allosas **salvestada** .
6.  Avage **armatuurlaud**, klõpsake **ÜHENDUSETEAVET** lehe allosas ning seejärel kopeerige ja salvestage ühendusteabe.

## <a name="configure-and-start-the-event-generator-application"></a>Sündmuse generaator rakenduse käivitamiseks ja konfigureerimiseks

Oleme pakkunud klientrakendusega, mis luua valimi sissetuleva kõne metaandmete ja lükake see sündmus jaoturi. Järgmiste juhiste abil saate häälestada selle rakenduse.  

1.  [TelcoGenerator.zip faili](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip)alla laadida ja pakkige see lahti kataloogiga.

    > [AZURE.NOTE] Windows võib blokeerida alla zip-fail. Paremklõpsake faili ja valige **Atribuudid**. Kui kuvatakse teade "See fail pärineb mõnest teisest arvutist ja võib olla selle arvuti kaitsmiseks blokeeritud", märkige ruut **Aktiveeri** ja seejärel klõpsake nuppu Rakenda zip-fail.

2.  Telcodatagen.exe.config Microsoft.ServiceBus.ConnectionString ja EventHubName väärtuste asendamine oma sündmuse jaoturi ühendusstringi ja nimi.

    Azure'i portaalis kopeeritud ühendusstringi paigutab ühenduse nime lõpus. Eemaldage kindlasti "; EntityPath =<value>", on" lisada võti = "välja.

3.  Käivitage rakendus. Funktsiooni kasutamine on järgmine:

    telcodatagen.exe [#NumCDRsPerHour] [SIM kaart pettuse tõenäosuse] [#DurationHours]

Järgmises näites toob 1000 sündmuste 20% on tõenäosuse pettuse kahe tunni jooksul.

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


## <a name="create-a-stream-analytics-job"></a>Voo Analytics töö loomine
Nüüd kus oleme voo side sündmuste, me seadistada voo Analytics töö neid sündmusi reaalajas analüüsimiseks.

### <a name="provision-a-stream-analytics-job"></a>Voo Analytics töö ettevalmistamine

1.  Azure'i portaalis, klõpsake nuppu **Uus** > **DATA SERVICES** > **Voo ANALYTICS** > **Kiiresti luua**.
2.  Määrake järgmised väärtused ja seejärel **Luua voo ANALYTICS töö**.

    * **Töö nimi**: sisestage töö nimi.

    * **Piirkond**: valige piirkond, kuhu soovite töö. Kaaluma töö ja sündmuse jaoturi sama piirkonna parema jõudluse tagamiseks ja veenduge, et te ei maksta andmete edastamiseks piirkondade vahel.

    * **Salvestusruumi konto**: valige Azure storage konto, mida soovite kasutada kõik voo Analytics tööd, mis töötavad selle piirkonna jälgimisega seotud andmete talletamiseks. Teil on võimalik valida olemasoleva salvestusruumi konto või looge uus.

3.  Klõpsake vasakpoolsel paanil voo Analytics tööde nimekirja **Voo ANALYTICS** .

    ![Voo Analytics teenuse ikoon](./media/stream-analytics-real-time-fraud-detection/stream-analytics-service-icon.png)

    Uue töö kuvatakse olek on **loodud**. Pange tähele, et lehe allservas nuppu **START** on keelatud. Enne töö alustamist tuleb konfigureerida töö sisendi, väljundi ja päringu.

### <a name="specify-job-input"></a>Määrake töö sisendit
1.  Oma töösse voo Analytics **SISENDINA** lehe ülaservas nuppu ja klõpsake **SISESTUSMEETODI lisamine**. Avanevas dialoogiboksis juhendab teid mitu sisendit häälestamise juhiseid.
2.  Klõpsake **ANDMEVOOS**, ja klõpsake nuppu paremale.
3.  Klõpsake **SÜNDMUST JAOTURI**, ja klõpsake paremal.
4.  Tippige või valige kolmanda lehel järgmised väärtused:

    * **SISESTUSMEETODI alias (PSEUDONÜÜM)**: Sisestage sõbralik nimi, näiteks *CallStream*, selle töö. Pange tähele, et kasutate selle nimi päringu hiljem.

    * **Sündmuse JAOTURI**: kui teie loodud sündmuse jaoturi on sama tellimuse voo Analytics töö, valige sündmuse jaoturi nimeruumi.

        Kui teie sündmuse jaoturi on mõni muu tellimus, valige **Kasuta sündmuse keskus teise tellimusest**ja sisestage teavet **Teenuse SIINI NIMERUUMI**, **Sündmuse JAOTURI nimi**, **Sündmuse JAOTURI poliitika nimi**, **Sündmuse JAOTURI poliitika võti**ja **Sündmuse JAOTURI PARTITION COUNT**käsitsi.

    * **Sündmuse JAOTURI nimi**: valige jaoturi sündmuse nimi.

    * **Sündmuse JAOTURI poliitika nimi**: märkige selles õpetuses loodud sündmuse jaoturi poliitika.

    * **Sündmuse JAOTURI tarbija rühma**: tippige selles õpetuses loodud tarbija rühma nimi.

5.  Klõpsake paremal.
6.  Määrake järgmised väärtused:

    * **Sündmuse SERIALIZER vorming**: JSON
    * **Kodeerimine**: UTF8
7.  Nuppu **märkige ruut** selle andmeallika lisamiseks ja kinnitamiseks, et voo Analytics saate ühendada sündmuse jaoturi.

### <a name="specify-job-query"></a>Määrake töö päring

Voo Analytics toetab lihtne, deklaratiivseid päringu mudelit, mis kirjeldab teisendused reaalajas töötlemiseks. Keele kohta leiate lisateavet teemast [Azure voo Analytics päringu keel viide](https://msdn.microsoft.com/library/dn834998.aspx). Selle õpetuse aitab teil koostamine ja päringute Testige oma kõne reaalajas andmevoo üle.

#### <a name="optional-sample-input-data"></a>Valikuline: Sisestuskeel näidisandmed
Tegelik töö andmetega päringu kinnitamiseks saate funktsiooni **NÄIDISANDMETE** oma voo sündmused ekstraktida ja luua lisamine. JSON-faili sündmuste testimiseks.  Järgmised toimingud näitab, kuidas seda teha. Meil on olemas [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) näidisfaili katsetamiseks.

1.  Valige sündmuse jaoturi sisendit ja klõpsake lehe allservas **NÄIDISANDMED** .
2.  Avanevas dialoogiboksis määrake soovitud **ALGUSKELLAAEG** hakata koguma andmeid ja **kestus** , kui palju kasutamine lisaandmete saamiseks.
3.  Klõpsake nuppu **märkige** alustamiseks valimite andmete sisend.  Võib kuluda minut või kaks andmefaili tuleb luua.  Kui on lõppenud, klõpsake käsku **üksikasjad**, laadige loodud. JSON fail ja salvestage see.

    ![Laadige alla ja salvestage töödeldud andmete JSON-failis](./media/stream-analytics-real-time-fraud-detection/stream-analytics-download-save-json-file.png)

#### <a name="pass-through-query"></a>Läbiv päring

Kui soovite arhiivida iga sündmuse, saate kõiki välju last sündmuse või sõnumi lugemiseks läbiv päring. Alustamiseks tehke lihtsa läbiv päring selle projektide sündmuse kõiki välju.

1.  **PÄRINGU** Kasutusanalüüsi voo töö lehe ülaosas nuppu.
2.  Lisage järgmine kood redaktori:

        SELECT * FROM CallStream

    > [AZURE.IMPORTANT] Veenduge, et varem määratud sisestatud nimi vastab Sisestuskeel allika nimi.

3.  Klõpsake jaotises päringuredaktor nuppu **testi** .
4.  Lisage faili test. Kasutamiseks loodud eelmisi juhiseid abil või kasutage [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/SampleDataFiles/Telco.json).
5.  Klõpsake nuppu **kontrolli** ja vaadake tulemused kuvatakse allpool päringu definitsiooni.

    ![Päringutulemite määratlus](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Veeru projektsiooni

Meil kuvatakse nüüd vähendada tagastatud väljade väiksema.

1.  Päringu koodi redigeerija muutmiseks tehke järgmist.

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  Klõpsake jaotises päringu tulemite kuvamiseks päringuredaktoris **Käivita uuesti** .

    ![Päringuredaktori väljund.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Arvu sissetulevad kõned regiooniti: langema aken, kus koondamine

Sissetulevate kõnede piirkonna arvu võrdlemiseks kasutame on [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) saada Rühmitusalus **SwitchNum** iga viie sekundi sissetulevate kõnede arvu.

1.  Päringu koodi redigeerija muutmiseks tehke järgmist.

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    See päring kasutab **Ajatempli,** märksõna last ajaline arvutamisel kasutatava ajatempli välja määramiseks. Kui sellel väljal pole määratud, tuleks akendesüsteemide toimingut teha aeg, et iga sündmuse sündmuse jaoturi abil. Vt ["Saabumisel Vs rakenduse aeg" voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Pange tähele, et atribuudi **System.Timestamp** abil pääsete juurde ajatempel iga akna lõppu.

2.  Klõpsake jaotises päringu tulemite kuvamiseks päringuredaktoris **Käivita uuesti** .

    ![Päringutulemite Timestand järgi](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>SIM pettuse tuvastamise koos iseendaga ühendamine

Potentsiaalselt pettusega kasutus tuvastamiseks vaatame kõnede, mis on sama kasutaja, kuid eri asukohtades olevate pärit vähem kui 5 minutit.  Me [Liitu](https://msdn.microsoft.com/library/azure/dn835026.aspx) voo kõne sündmuste iseendaga sellisel juhul otsida.

1.  Päringu koodi redigeerija muutmiseks tehke järgmist.

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  Klõpsake jaotises päringu tulemite kuvamiseks päringuredaktoris **Käivita uuesti** .

    ![Päringutulemite ühenduse](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Väljundi valamu loomine

Nüüd, kui meil on määratletud sündmuse voo, sündmuse jaoturi, mis neelata sündmuste ja päringu täitmiseks üle viia, on teisendus sisestusmeetodi viimases etapis on määratleda mõne väljundi valamu töö. Me kirjutada Azure'i bloobimälu sündmuste pettusega käitumist.

Järgmiste juhiste abil saate luua bloobimälu ümbris, kui teil pole veel üks.

1.  Olemasoleva salvestusruumi konto kasutamine või salvestusruumi uue konto loomiseks klõpsake ikooni **uus > DATA SERVICES > SALVESTUSRUUM > kiiresti luua**, ja järgige juhiseid.
2.  Valige konto, salvestusruumi, **ÜMBRISTE** lehe ülaservas nuppu ja seejärel klõpsake nuppu **Lisa**.
3.  Määrake oma container **nimi** ja seadke oma **ACCESSI** **Avaliku bloobimälu**.

## <a name="specify-job-output"></a>Määrake töö väljund

1.  Oma töösse voo Analytics **väljundi** lehe ülaservas nuppu ja klõpsake **Väljundi lisada**. Avanevas dialoogiboksis juhendab teid häälestada oma väljundi mitu toimingut.
2.  Klõpsake **BLOOBIMÄLU**, ja klõpsake nuppu paremale.
3.  Tippige või valige kolmanda lehel järgmised väärtused:

    * **Väljundi alias (PSEUDONÜÜM)**: Sisestage sõbralik nimi selle töö väljund.
    * **Tellimus**: kui loodud bloobimälu on sama tellimuse voo Analytics töö, valige **Kasuta salvestusruumi konto praeguselt tellimuselt**. Kui salvestusruumi on mõni muu tellimus, klõpsake nuppu **Kasuta salvestusruumi konto teise tellimusest**ja **Salvestusruumi konto**, **Salvestusruumi konto võti**ja **CONTAINER**teabe käsitsi sisestamiseks.
    * **Salvestusruumi konto**: valige salvestusruumi konto nimi.
    * **CONTAINER**: valige ümbris nimi.
    * **FAILINIME EESLIIDE**: tippige faili eesliite bloobimälu väljundi kirjutamisel kasutada.

4.  Klõpsake paremal.
5.  Määrake järgmised väärtused:

    * **Sündmuse SERIALIZER vorming**: JSON
    * **Kodeerimine**: UTF8

6.  Nuppu **märkige ruut** selle andmeallika lisamiseks ja kinnitamiseks, et voo Analytics saate ühendada salvestusruumi konto.

## <a name="start-job-for-real-time-processing"></a>Reaalajas töötlemiseks töö alustamine

Kuna töö input, päringus ja väljundi kõik pole määratud, oleme reaalajas pettuse tuvastamise voo Analytics töö alustamiseks valmis.

1.  Klõpsake **ARMATUURLAUA**töö **alustamiseks** klõpsake lehe allosas.
2.  Avanevas dialoogiboksis nuppu **töö ALGUSKELLAAEG**ja seejärel klõpsake dialoogiboksi allservas nuppu **kontrollimine** . Töö olek muutub **algus** ja muutub peagi **töötab**.

## <a name="view-fraud-detection-output"></a>Vaate pettuse tuvastamise väljund

Kasutage tööriista nagu [Azure'i salvestusruumi Exploreri](http://storageexplorer.com/) või [Azure Exploreri](http://www.cerebrata.com/products/azure-explorer/introduction) pettusega sündmuste kuvamiseks, nagu need on kirjutatud oma väljund reaalajas.  

![Pettuse tuvastamise: pettusega sündmuste vaadata reaalajas](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Tootetugi
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
