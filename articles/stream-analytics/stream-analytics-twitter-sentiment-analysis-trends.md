<properties
    pageTitle="Reaalajas Twitteri meeleolu analüüsi voo Analytics | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada voo Analytics reaalajas Twitteri meeleolu analüüsi jaoks. Üksikasjalikud juhised: sündmuse genereerimine andmete reaalajas armatuurlaual."
    keywords="reaalajas Twitteri trendi analüüs, meeleolu analüüs, sotsiaalmeedia analüüsi, trendi analüüsi näide"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="social-media-analysis-real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Sotsiaalmeedia analüüsi: Azure'i voo Analytics reaalajas Twitteri meeleolu analüüsi

Saate teada, kuidas luua meeleolu analüüsi lahenduse sotsiaalmeedia Analyticsi reaalajas Twitteri sündmuste Azure'i sündmuse jaoturi abil. Teile kirjutada päring Azure'i voo Analytics andmete analüüsimiseks. Saate siis kas talletage hiljem tutvumiseks või kasutada armatuurlaua ja [Power BI](https://powerbi.com/) reaalajas aru saada.

Sotsiaalmeedia analytics tööriistad aitavad mõista trendid teemasid, st teemade ja hoiakud, mis on suure hulga postitusi sotsiaalmeedia ettevõtted. Meeleolu analüüsi, mida nimetatakse ka *arvamust kaevandamine*, kasutab sotsiaalmeedia analytics tööriistad suhtumist mõne toote, soovitame jne. Reaalajas Twitteri trendi analüüsi on hea näide, kuna mudeli #-sildi profiililehe tellimus võimaldab kuulata teatud märksõnu ja analüüsida meeleolu kanalis.

## <a name="scenario-sentiment-analysis-in-real-time"></a>Stsenaarium: Meeleolu analüüsi reaalajas

Mõni ettevõte, mis on meedia veebisait on huvitatud saada eelis konkurentide, kus on saidi sisu, mis on seotud kohe oma lugejatele. Ettevõte kasutab sotsiaalmeedia analüüsi teemadel, mis on seotud lugejad, tehes Twitteri reaalajas meeleolu analüüs. Täpsemalt reaalajas Twitter trendid teemasid tuvastamiseks ettevõtte vajadustele reaalajas analytics säutsu mahu ja meeleolu peamised teemad. Jah, sisuliselt on meeleolu analüüs analytics mootori, mis põhineb selle kanali sotsiaalmeedia.

## <a name="prerequisites"></a>Eeltingimused
-   Twitteri konto ja [OAuthi juurdepääsu luba](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)
-   [TwitterClient.zip](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip) Microsofti allalaadimiskeskus
-   Valikuline: Lähtekoodi [GitHub](https://aka.ms/azure-stream-analytics-twitterclient) Twitteri klient

## <a name="create-an-event-hub-input-and-a-consumer-group"></a>Sündmuse jaoturi, mis sisestusteade ja tarbija rühma loomine

Valimi rakenduse saab luua sündmuste ja murravad eksemplari sündmuste jaoturi (mõni sündmus keskuses lühikese). Teenuse siini sündmuse jaoturi on sündmuse manustamisest voo Analyticsi eelistatud meetod. [Teenuse siini](/documentation/services/service-bus/)dokumentatsiooni sündmuse jaoturi dokumentatsioonist.

Järgmiste juhiste abil saate luua ka sündmuse jaoturi.

1.  Azure'i portaalis, klõpsake nuppu **Uus** > **Rakenduse teenuste** > **Teenuse SIINI** > **Sündmuse JAOTURI** > **Kiiresti luua**, ja sisestage nimi, piirkond ja uude või olemasolevasse nimeruum.  
2.  Hea tava, lugege iga voo Analytics töö ühe sündmuse jaoturi tarbija rühma. Me sõelub protsessi hiljem tarbija rühma loomine. Saate lisateavet tarbija rühmade [Azure'i sündmuse jaoturi ülevaade](https://msdn.microsoft.com/library/azure/dn836025.aspx). Tarbija rühma loomiseks avage äsja loodud sündmuse jaoturi, klõpsake vahekaarti **RÜHMI** , klõpsake nuppu **Loo** lehe allservas ja seejärel andke oma tarbija rühma jaoks nimi.
3.  Sündmuse jaoturi juurdepääsu andmiseks vajame ühiskasutusega juurdepääsu poliitika loomiseks. Oma sündmuse jaoturi vahekaarti **KONFIGUREERIMINE** .
4.  Jaotises **ÜHISKASUTUSES JUURDEPÄÄSUPOLIITIKAID**, saate luua uue poliitika õiguste **haldamine** .

    ![Ühiskasutusse antud Juurdepääsupoliitikaid, kus saate luua poliitika õigustega haldamine.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-ananlytics-shared-access-policies.png)

5.  Klõpsake lehe allosas **salvestada** .
6.  Avage **ARMATUURLAUD**, klõpsake **ÜHENDUSETEAVET** lehe allosas ning seejärel kopeerige ja salvestage ühendusteabe. (Kasutage Kopeeri kuvatavat ikooni all olevat ikooni Otsi.)

## <a name="configure-and-start-the-twitter-client-application"></a>Twitteri klientrakendusega käivitamiseks ja konfigureerimiseks

Oleme pakkunud kliendi rakendus, mis loob ühenduse Twitteri andmete kaudu [Twitter Streaming API -de](https://dev.twitter.com/streaming/overview) kogumiseks säutsu sündmuste parameetritega kogumi teemade kohta. Tööriista [Sentiment140](http://help.sentiment140.com/) avatud allika kasutatakse iga säutsu meeleolu väärtuse määramiseks.

- 0 = negatiivne
- 2 = neutraalne
- 4 = positiivne

Seejärel säutsu sündmused on viinud jaoturi sündmus.  

Rakenduse häälestamiseks tehke järgmist

1.  [Laadige lahendus TwitterClient](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip).
2.  Avage TwitterClient.exe.config ja oauth_consumer_key, oauth_consumer_secret, oauth_token ja oauth_token_secret Twitteri märgid, mis on teie väärtuste asendamine.  

    [Juhised OAuthi juurdepääsu sümboolse loomiseks](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)  

    Pange tähele, et saate luua märgiks tühja taotluse.  
3.  Ühendusstringi ja nimi oma sündmuse jaoturi TwitterClient.exe.config EventHubConnectionString ja EventHubName väärtuste asendamine. Ühendusstringi, mis varem kopeeritud pakub ühendusstringi nii teie sündmuse jaoturi nime, et olla kindel, need eraldada ja iga õige välja paigutamiseks. Näiteks, võtke arvesse järgmisi ühendusstring.

        Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey;EntityPath=yourhub

    TwitterClient.exe.config fail peaks sisaldama oma sätteid nagu järgmises näites:

        add key="EventHubConnectionString" value="Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey"
        add key="EventHubName" value="yourhub"

    On oluline märkida, et tekst "EntityPath =" ei __ole__ EventHubName väärtus kuvatakse.

4.  *Valikuline:* Saate reguleerida märksõnu otsida.  Vaikimisi otsib selle rakenduse "Azure, Skype, XBox, Microsoft, Seattle".  Soovi korral saate reguleerida **twitter_keywords** TwitterClient.exe.config, klõpsake selle väärtused.
5.  Käivitage TwitterClient.exe rakenduse käivitamiseks. Kuvatakse säutsu sündmuste saadetakse teie sündmuse jaoturi **CreatedAt**, **teema**ja **SentimentScore** väärtustega.

    ![Meeleolu analüüsi: saadetud sündmuse jaoturiga SentimentScore väärtused.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-sentiment-output-to-event-hub.png)

## <a name="create-a-stream-analytics-job"></a>Voo Analytics töö loomine

Nüüd, kui säutsu sündmused on streaming reaalajas Twitteri, saame seadistada voo Analytics töö neid sündmusi reaalajas analüüsimiseks.

### <a name="provision-a-stream-analytics-job"></a>Voo Analytics töö ettevalmistamine

1.  [Azure'i portaal](https://manage.windowsazure.com/), klõpsake nuppu **Uus** > **DATA SERVICES** > **Voo ANALYTICS** > **Kiiresti luua**.
2.  Määrake järgmised väärtused ja seejärel **Luua voo ANALYTICS töö**.

    * **Töö nimi**: sisestage töö nimi.
    * **Piirkond**: valige piirkond, kuhu soovite töö. Kaaluma töö ja sündmuse jaoturi sama piirkonna parema jõudluse tagamiseks ja veenduge, et te ei maksta andmete edastamiseks piirkondade vahel.
    * **Salvestusruumi konto**: valige Azure storage konto, mida soovite kasutada kõik voo Analytics tööd, mis töötavad selle piirkonna jälgimisega seotud andmete talletamiseks. Teil on võimalik luua uue või olemasoleva salvestusruumi konto valimiseks.

3.  Klõpsake vasakpoolsel paanil voo Analytics tööde nimekirja **Voo ANALYTICS** .  
    ![Voo Analytics teenuse ikoon](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-service-icon.png)

    Uue töö kuvatakse olek on **loodud**. Pange tähele, et lehe allservas nuppu **START** on keelatud. Enne töö alustamist tuleb konfigureerida töö sisendi, väljundi ja päringu.


### <a name="specify-job-input"></a>Määrake töö sisendit
1.  Oma töösse voo Analytics **SISENDINA** lehe ülaosas nuppu ja klõpsake **SISESTUSMEETODI lisamine**. Avanevas dialoogiboksis annab teile kaudu seadistamine sisendit arv.
2.  Klõpsake **ANDMEVOOS**, ja klõpsake nuppu paremale.
3.  Klõpsake **SÜNDMUST JAOTURI**, ja klõpsake paremal.
4.  Tippige või valige kolmanda lehel järgmised väärtused:

    * **SISESTUSKEEL alias (PSEUDONÜÜM)**: Sisestage sõbralik nimi selle töö sisend, nt *TwitterStream*. Pange tähele, et te kasutate selle nimi päringu hiljem.
    **Sündmuse JAOTURI**: kui teie loodud sündmuse jaoturi on sama tellimuse voo Analytics töö, valige sündmuse jaoturi nimeruumi.

        Kui teie sündmuse jaoturi on mõni muu tellimus, klõpsake nuppu **Kasuta sündmuse jaoturi teise tellimusest**ja sisestage teavet **Teenuse SIINI NIMERUUMI**, **Sündmuse JAOTURI nimi**, **Sündmuse JAOTURI poliitika nimi**, **Sündmuse JAOTURI poliitika võti**ja **Sündmuse JAOTURI PARTITION COUNT**käsitsi.

    * **Sündmuse JAOTURI nimi**: valige jaoturi sündmuse nimi.

    * **Sündmuse JAOTURI poliitika nimi**: märkige selles õpetuses loodud sündmuse jaoturi poliitika.

    * **Sündmuse JAOTURI tarbija rühma**: tippige selles õpetuses loodud tarbija rühma nimi.
5.  Klõpsake paremal.
6.  Määrake järgmised väärtused:

    * **Sündmuse SERIALIZER vorming**: JSON
    * **Kodeerimine**: UTF8

7.  Nuppu **märkige ruut** selle andmeallika lisamiseks ja kinnitamiseks, et voo Analytics saate ühendada sündmuse jaoturi.

### <a name="specify-job-query"></a>Määrake töö päring

Voo Analytics toetab lihtne, deklaratiivseid päringu mudelit, mis kirjeldab teisendused. Keele kohta leiate lisateavet teemast [Azure voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Selle õpetuse aitab teil koostamine ja testida päringute Twitteri andmete üle.

#### <a name="sample-data-input"></a>Näide andmete sisestamine

Tegelik töö andmetega päringu kinnitamiseks saate **NÄIDISANDMETE** funktsiooni eraldada sündmuste oma voo ja testimiseks sündmuste .json faili loomine.

1.  Valige sündmuse jaoturi sisendit ja klõpsake lehe allservas **NÄIDISANDMED** .
2.  Avanevas dialoogiboksis määrake soovitud **ALGUSKELLAAEG** hakata koguma andmeid ja **kestus** , kui palju kasutamine lisaandmete saamiseks.
3.  Klõpsake nuppu **üksikasjad** ja klõpsake seejärel linki **klõpsake siin** alla laadida ja salvestada loodud .json fail.

#### <a name="pass-through-query"></a>Läbiv päring
Alustamiseks teeme lihtsa läbiv päring selle projektide sündmuse kõiki välju.

1.  **PÄRINGU** Kasutusanalüüsi voo töö lehe ülaosas nuppu.
2.  Koodi redaktoris, asendage algne Päringumall järgmist:

        SELECT * FROM TwitterStream

    Veenduge, et varem määratud sisestatud nimi vastab Sisestuskeel allika nimi.

3.  Klõpsake jaotises päringuredaktor nuppu **testi** .
4.  Minge oma .json näidisfaili.
5.  Klõpsake nuppu **kontrollida** ja vaadake tulemused päringu definitsiooni all.

    ![Tulemused kuvatakse allpool päringu definitsiooni](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sentiment-by-topic.png)

#### <a name="count-of-tweets-by-topic-tumbling-window-with-aggregation"></a>Count tweets teema: langema aken, kus koondamine

Mainimiste vahel teemade arvu võrdlemiseks kasutame on [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) saada mainimiste arvu teema iga viie sekundi järel.

1.  Päringu koodi redigeerija muutmiseks tehke järgmist.

        SELECT System.Timestamp as Time, Topic, COUNT(*)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

    See päring kasutab **AJATEMPLI,** märksõna last ajaline arvutamisel kasutatava ajatempli välja määramiseks. Kui sellel väljal pole määratud, tuleks akendesüsteemide toimingut teha aeg, et iga sündmuse sündmuse jaoturi abil.  Lugege lisateavet [voo Analytics päringu](https://msdn.microsoft.com/library/azure/dn834998.aspx)viite jaotises "Saabumisel Vs rakenduse aeg".

    Selle päringu pääseb ajatempel iga akna lõppu ka atribuudi **System.Timestamp** abil.

2.  Klõpsake jaotises päringu tulemite kuvamiseks päringuredaktoris **Käivita uuesti** .

#### <a name="identify-trending-topics-sliding-window"></a>Määratlege trendid teemasid: libistades aken

Trendid teemasid tuvastamiseks vaatame teemad, mis ületavad lävi väärtus mainimiste sisse antud aja jooksul. Selleks et selles õpetuses, me kontrollida teemad, mis on nimetatud rohkem kui 20 korda viimase viie sekundi lisamine [SlidingWindow](https://msdn.microsoft.com/library/azure/dn835051.aspx)abil.

1.  Koodi redaktori abil päringu muutmine: valige System.Timestamp aeg, teema, COUNT (*) nimega Mainimised kaudu TwitterStream AJATEMPLI, CreatedAt rühma alus SLIDINGWINDOW (s, 5), võttes loendamine teema (*) > 20

2.  Klõpsake jaotises päringu tulemite kuvamiseks päringuredaktoris **Käivita uuesti** .

    ![Libistades aknas päringu väljund](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-query-output.png)

#### <a name="count-of-mentions-and-sentiment-tumbling-window-with-aggregation"></a>Count mainimised ja meeleolu: langema aken, kus koondamine
Lõplik päring, mida me test kasutab **TumblingWindow** mainimised, Keskmine, miinimum, maksimum ja standardhälve meeleolu keskmine arv saamiseks iga teema iga viie sekundi järel.

1.  Päringu koodi redigeerija muutmiseks tehke järgmist.

        SELECT System.Timestamp as Time, Topic, COUNT(*), AVG(SentimentScore), MIN(SentimentScore),
        Max(SentimentScore), STDEV(SentimentScore)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

2.  Klõpsake jaotises päringu tulemite kuvamiseks päringuredaktoris **Käivita uuesti** .
3.  See on päringut, mida me kasutame meie armatuurlaud.  Klõpsake lehe allosas **salvestada** .


## <a name="create-output-sink"></a>Väljundi valamu loomine

Nüüd, kui meil on määratletud sündmuse voo, sündmuse jaoturi, mis neelata sündmuste ja päringu täitmiseks üle viia, on teisendus sisestusmeetodi viimases etapis on määratleda mõne väljundi valamu töö.  Me kirjutada liidetud säutsu sündmused meie töö päringust Azure'i bloobimälu.  Võib ka push tulemused Azure SQL-andmebaasiga, Azure'i tabelimälu või sündmuse jaoturi, sõltuvalt teie konkreetse rakenduse vajab.

Kui teil pole veel üks, tehke järgmist bloobimälu, ümbris loomiseks:

1.  Olemasoleva salvestusruumi konto kasutamine või salvestusruumi uue konto loomiseks klõpsake ikooni **Uus** > **DATA SERVICES** > **salvestusruumi** > **Kiiresti luua**, ja seejärel järgige ekraanil.
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

## <a name="start-job"></a>Töö alustamine

Kuna töö input, päringus ja väljundi kõik pole määratud, oleme voo Analytics töö alustamiseks valmis.

1.  Klõpsake **ARMATUURLAUA**töö **alustamiseks** klõpsake lehe allosas.
2.  Avanevas dialoogiboksis nuppu **töö ALGUSKELLAAEG**ja seejärel klõpsake dialoogiboksi allservas nuppu **kontrollimine** . Töö olek muutub **algus** ja muutub peagi **töötab**.


## <a name="view-output-for-sentiment-analysis"></a>Vaate väljundi meeleolu analüüsi jaoks

Pärast oma töö töötab ja töötlemine reaalajas Twitteri voo, valida, kuidas soovite vaadata väljundi meeleolu analüüsi jaoks. Tööriista nagu [Azure'i salvestusruumi Exploreri](https://azurestorageexplorer.codeplex.com/) või [Azure Exploreri](http://www.cerebrata.com/products/azure-explorer/introduction) abil saate vaadata oma töö väljundi reaalajas. Siit saate [Power BI](https://powerbi.com/) rakenduse lisada kohandatud armatuurlaua nagu järgmine pilt laiendada.

![Sotsiaalmeedia analüüsi: voo Analytics meeleolu analüüsi (arvamust kaevandamine) väljundi Power BI armatuurlaua.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-output-power-bi.png)

## <a name="get-support"></a>Tootetugi
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
