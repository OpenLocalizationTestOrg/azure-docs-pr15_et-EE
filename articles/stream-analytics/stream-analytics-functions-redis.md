<properties
    pageTitle="Väljund on Azure Redis vahemälu, kasutades Azure funktsioone, Azure'i voo Analytics | Microsoft Azure'i"
    description="Siit saate teada, on Azure funktsiooni kasutamise kohta ühendatud teenuse siini järjekorda asustamiseks on Azure Redis vahemälu väljundi voo Analytics töö."
    keywords="andmevoos redis vahemälu teenuse siini järjekord"
    services="stream-analytics"
    authors="ryancrawcour"
    manager="jhubbard"
    documentationCenter=""
    />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/26/2016"
    ms.author="ryancraw"/>

# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Kuidas säilitada Azure'i voo Analytics andmete on Azure Redis vahemälu Azure'i funktsioonide abil

Azure'i voo Analytics abil saate kiiresti arendamise ja madala hinnaga lahendusi, et saada reaalajas teadmisi seadmed, andurid, taristu, ja rakenduste või mis tahes andmevoo juurutamine. See võimaldab erinevate kasutamise juhtudel nagu reaalajas haldus ja jälgimine, käsk ja juhtelemendi, pettuse avastamine, ühendatud auto ja palju muud. Mitmel puhul soovite andmete väljundit Azure'i voo Analytics, nt on Azure Redis vahemälu poodi jaotatud andmete talletamiseks.

Oletame, et olete side ettevõtte osa. Soovite SIM pettuse, kus mitu sama identiteeti, samas kui helistaja aeg, kuid muu geograafiliselt asukohad. Teil on ülesanne talletamine kõigi võimalike pettuse telefonikõned on Azure Redis vahemälu. Selles blogis pakume juhised selle kohta, kuidas saate hõlpsasti tööülesande lõpetama. 

## <a name="prerequisites"></a>Eeltingimused
Täitke [Reaalajas pettuse tuvastamise] [ fraud-detection] walk-through Ash

## <a name="architecture-overview"></a>Arhitektuuri ülevaade
![Kuvatõmmis arhitektuur](./media/stream-analytics-functions-redis/architecture-overview.png)

Nagu eelmisel joonisel näha, võimaldab voo Analytics streaming sisendandmete esitatakse selle kohta päring ja saadetud väljund. Põhineb väljund, Azure'i funktsioonid saab siis käivitamine teatud tüüpi sündmus. 

Selles blogis me keskenduda Azure'i funktsioonid osa sellest müügivõimaluste või täpsemalt käivitamise sündmus, mis talletab pettusega andmete vahemälu.
Pärast lõpetamist [Reaalajas pettuse tuvastamise] [ fraud-detection] õpetuses olete sisend (sündmuse jaoturi), päringu ja väljund (bloobimälu) juba konfigureeritud ja töötab. Selles blogis me muuta väljundi kasutada hoopis teenuse siini järjekorda. Pärast seda, me ühendust mõne Azure'i funktsioon selles järjekorras. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Loomine ja ühenduse teenuse siini järjekorda väljund
Teenuse siini järjekorra loomiseks järgige juhiseid 1 ja 2 .net-i jaotisele [Alustamine teenuse siini järjekorrad][servicebus-getstarted].
Nüüd loome ühenduse voo Analytics töö, mis on loodud mõnes varasemas pettuse tuvastamise walk-through järjekorda.



1. Azure'i portaalis minge **väljundeid** tera oma töö ja valige käsk **Lisa** lehe ülaosas.

    ![Väljundeid lisamine](./media/stream-analytics-functions-redis/adding-outputs.png)

2. Valige **Teenuse siini järjekorda** nimega **valamu** ja järgige ekraanil kuvatavaid juhiseid. Veenduge, et valida nimeruumi teenuse siini järjekorra loodud [Alustamine teenuse siini järjekorrad][servicebus-getstarted]. Kui olete lõpetanud, klõpsake nuppu "parem" nupp.
3. Määrake järgmised väärtused:
    - **Sündmuse Serializer vorming**: JSON
    - **Kodeerimine**: UTF8
    - **Vorming**: joon, mis on eraldatud

4. Klõpsake nuppu **Loo** selle andmeallika lisamiseks ja kinnitamiseks, et voo Analytics saate ühendada salvestusruumi konto.

5. Klõpsake menüüs **päring** asendada praeguse päringu järgmist. Asendage *[Teie teenuse SIINI nimi]* sammus 3 loodud väljund nimi. 

    ```    

        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2

        INTO [YOUR SERVICE BUS NAME]
    
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
    
        WHERE CS1.SwitchNum != CS2.SwitchNum
    
    ```

## <a name="create-an-azure-redis-cache"></a>Mõne Azure Redis vahemälu loomine
.Net-i jaotisest [Kuidas kasutada Azure Redis vahemälu] järgides on Azure Redis vahemälu luua[ use-rediscache] seni, kuni jaotises nimega ***konfigureerimine vahemälu kliendid***.
Kui olete lõpetanud, peate uue Redis vahemälu. Valige jaotises **Kõik sätted** **Pääsuklahvide** ja pange kirja ***esmane ühendusstring***.

![Kuvatõmmis arhitektuur](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Azure'i ülesande loomine
Järgige [oma esimese Azure'i funktsioon Loo] [ functions-getstarted] õpetuse alustada Azure'i funktsioonid. Kui teil on juba mõne Azure funktsioon, mida soovite kasutada, siis jätkake edasi [Redis vahemällu kirjutamist](#Writing-to-Redis-Cache)

1. Portaalis valige rakendus teenuste vasakult navigeerimispaanilt ja klõpsake nuppu funktsiooni rakenduse veebisaidi avamiseks oma Azure funktsioon rakenduse nimi.
    ![Kuvatõmmis rakenduse services funktsioon loendist](./media/stream-analytics-functions-redis/app-services-function-list.png)

2. Klõpsake **Uus funktsioon > ServiceBusQueueTrigger – C#**. Järgmiste väljade, täitke järgmised juhised.
    - **Järjekorra nimi**: [Alustamine teenuse siini järjekorrad] kuhjuda loomisel sisestatud nimi sama nimi[ servicebus-getstarted] (mitte nime teenus). Veenduge, et kasutate voo Analytics väljundisse ühendatud järjekorda.
    - **Teenuse siini ühendus**: valige **Lisa ühendusstringi**. Ühendusstringi leidmiseks klassikaline portaalis, valige **Teenus siini**, teenuse siini loodud ja **ÜHENDUSETEAVET** ekraani allosas. Veenduge, et olete sellel lehel avalehel. Kopeerige ja kleepige ühendusstring. Võite sisestada mis tahes ühenduse nimi.
    
        ![Kuvatõmmis siini ühendust](./media/stream-analytics-functions-redis/servicebus-connection.png)
    - **AccessRights**: valige **haldamine**


3. Klõpsake nuppu **Loo**

## <a name="writing-to-redis-cache"></a>Redis vahemälu kirjutamine
Oleme nüüd loonud Azure'i funktsioon, mis loeb teenuse siini järjekorda. Kõik, mis on jäänud teha on meie funktsiooni abil saate kirjutamise andmed Redis vahemälu. 

1. Valige oma äsja loodud **ServiceBusQueueTrigger**ja klõpsake Kuva paremas ülanurgas nuppu **funktsioon rakenduse sätted** . Valige **avage rakendus Teenusesätted > Sätted > rakenduse sätted**

2. Stringide jaotises ühenduse loomine jaotises **nimi** nimi. Kleepige olete leidnud **loomine Redis vahemälu** samm **väärtus** jaotise esmane ühendusstring. Valige **kohandatud** , kus on kirjas **SQL-andmebaasi**.

3. Klõpsake ülaosas nuppu **Salvesta** .

    ![Kuvatõmmis siini ühendust](./media/stream-analytics-functions-redis/function-connection-string.png)

4. Nüüd minge tagasi rakenduse Teenusesätted ja valige **Tööriistad > rakenduse teenuse redaktor (eelvaade) > sisse > avage**.

    ![Kuvatõmmis siini ühendust](./media/stream-analytics-functions-redis/app-service-editor.png)

5. Teie valitud toimetaja, looge JSON fail nimega **project.json** järgmisega ja salvestage see oma kohalikule kettale.

        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }

6. Seda faili üles laadida üheks juurkaust oma funktsiooni (mitte WWWROOT). Peaksite nägema fail nimega **project.lock.json** automaatselt kuvada, mis kinnitab, et selle Nugeti pakette "StackExchange.Redis" ja "Newtonsoft.Json" importimist.

7. Failis **run.csx** asendada järgmine kood varem genereeritud koodi. Asendage funktsiooni lazyConnection **poeandmete Redis vahemälu**juhises 2 loodud nimega "CONN nimi".

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;
    
    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");
    
        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-the-stream-analytics-job"></a>Voo Analytics töö alustamine

1. Käivitage rakendus telcodatagen.exe. Funktsiooni kasutamine on järgmine:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````

2. Portaali keelest voo Analytics töö nuppu **alustamiseks** klõpsake lehe ülaosas.

    ![Kuvatõmmis – töö alustamine](./media/stream-analytics-functions-redis/starting-job.png)

3. **Töö alustamine** tera, mis kuvatakse, valige **kohe** ja klõpsake Kuva allservas nuppu **Start** . Töö oleku muutumise algus ja pärast teatud aja muudatustest töötab.
 
    ![Alusta töö aeg valiku kuvatõmmis](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Käivitage lahenduse ja märkige ruut tulemused
Oma **ServiceBusQueueTrigger** lehele naasmine nüüd peaks nähtaval olema log laused. Nende logides, et sai midagi teenuse siini järjekorda, pannakse andmebaas, ja selle kasutamise ajal klahvi välja tuua!

Veenduge, et teie andmed on vahemälu Redis, avage uus portaalis leht Redis vahemälu (nagu on näidatud eelmises etapis [loomine Azure Redis vahemälu](#Create-an-Azure-Redis-Cache) ) ja valige konsooli.

Nüüd saate kirjutada Redis käsud kinnitada, et andmed on tegelikult vahemälu.

![Redis konsooli kuvatõmmis](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Järgmised sammud
Oleme oodatud uut võimalust Azure'i funktsioonide voo analytics teha koos ja loodame, et see eemaldab teie jaoks uusi võimalusi. Kui teil on tagasisidet, mida soovite edasi, Julgelt [Azure'i UserVoice saidi](https://feedback.azure.com/forums/270577-stream-analytics)kasutamine.

Kui olete uus Microsoft Azure'i, kutsume teid proovima liitumisel [tasuta prooviversiooni Azure'i konto](https://azure.microsoft.com/pricing/free-trial/). Kui olete kasutanud voo Analytics, siis kutsume teid [esimese voo Analytics tööpäevad](stream-analytics-create-a-job.md)loomiseks.

Kui teil on vaja mõnda abi või teil on küsimusi, postitamine [MSDN-i](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) või [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) Foorumid. 

Saate vaadata ka järgmistest allikatest:

- [Azure'i funktsioonide tootearendusmaterjal](../azure-functions/functions-reference.md)
- [Azure'i funktsioonide C# tootearendusmaterjal](../azure-functions/functions-reference-csharp.md)
- [Azure'i funktsioonide F # tootearendusmaterjal](../azure-functions/functions-reference-fsharp.md)
- [Azure'i funktsioonide NodeJS tootearendusmaterjal](../azure-functions/functions-reference.md)
- [Azure'i funktsioonide päästikute ja seosed](../azure-functions/functions-triggers-bindings.md)
- [Kuidas jälgida Azure'i Redis vahemälu](../redis-cache/cache-how-to-monitor.md)

Kursis kõigi uudiste ja funktsioone, järgige [@AzureStreaming](https://twitter.com/AzureStreaming) Twitter.


[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
