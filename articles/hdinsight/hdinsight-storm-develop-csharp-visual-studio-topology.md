<properties
   pageTitle="Visual Studio ja C# Apache Storm topoloogiatest | Microsoft Azure'i"
   description="Saate teada, kuidas luua Storm topoloogiatest C# luues lihtne sõna count topoloogia Visual Studio Visual Studio Hdinsightiga tööriistade abil."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/27/2016"
   ms.author="larryfr"/>

# <a name="develop-c-topologies-for-apache-storm-on-hdinsight-using-hadoop-tools-for-visual-studio"></a>C# topoloogiatest arendamise Apache Storm klõpsake Hdinsightiga Visual Studio Hadoopi tööriista abil

Saate teada, kuidas luua C# Storm topoloogia Visual Studio Hdinsightiga tööriistade abil. Selles õppeteemas tutvustatakse protsessi Storm uue projekti loomise Visual Studios, seda kohalikult testimine ja juurutamine see on Apache Storm Hdinsightiga kobar kohta.

Samuti saate teada, kuidas luua hübriid topoloogiatest, mis kasutavad C# ja Java komponendid.

> [AZURE.IMPORTANT] Ajal juhised selle dokumendi toetuvad Windowsi arenduskeskkond Visual Studio, saab esitada kompileeritud projekti Linuxi või Windowsi-põhiste Hdinsightiga kobar. Ainult Linuxi-põhiste kogumite loodud pärast 28-10-2016 tugi SCP.NET topoloogiatest.
>
> C# topoloogia Linux-põhine kobar kasutamiseks peate värskendama Microsoft.SCP.Net.SDK Nugeti pakett kasutatud projekti versioon 0.10.0.6 või uuem versioon. Paketi versioon peab vastama ka Storm installitud Hdinsightiga põhiversiooni. Torm Hdinsightiga versioonid 3.3 ja 3.4 kasutada näiteks torm versioon 0.10.x ajal Hdinsightiga 3.5 kasutab Storm 1.0.x.
> 
> C# topoloogiatest Linux-põhine kogumite kohta tuleb kasutada .NET 4.5 ja ühe abil saate käivitada Hdinsightiga kobar. Enamik asju töötavad, kuid Kontrollige [Ühilduvust ühe](http://www.mono-project.com/docs/about-mono/compatibility/) dokumendi võimalike vastuolude puhul.

## <a name="prerequisites"></a>Eeltingimused

- Visual Studio järgmised versioon

    - Visual Studio 2012 koos [4 värskendamine](http://www.microsoft.com/download/details.aspx?id=39305)

    - Visual Studio 2013 [värskendus 4](http://www.microsoft.com/download/details.aspx?id=44921) või [Visual Studio 2013 ühenduse](http://go.microsoft.com/fwlink/?LinkId=517284)

    - Visual Studio 2015 või [Visual Studio 2015 ühenduse](https://go.microsoft.com/fwlink/?LinkId=532606)

- Azure'i SDK 2.9.5 või uuem versioon

- Hdinsightiga Tools for Visual Studio: teemast installida ja konfigureerida Hdinsightiga tools for Visual Studio [Hdinsightiga Tools for Visual Studio kasutamise alustamine](hdinsight-hadoop-visual-studio-tools-get-started.md) .

    > [AZURE.NOTE] Visual Studio Express Hdinsightiga Tools for Visual Studio pole toetatud

-   Apache Storm Hdinsightiga kobar kohta: teemast [Alustamine Apache Storm Hdinsightiga kohta](hdinsight-apache-storm-tutorial-get-started.md) üksikasjalikud juhised klaster loomiseks.

## <a name="templates"></a>Mallid

Hdinsightile Tools for Visual Studio pakuvad järgmised Mallid:

| Projekti tüüp | Näitab |
| ------------ | ------------- |
| Torm rakendus | Tühja Storm topoloogia projekti |
| Torm Azure SQL-i kirjutaja näidis | Kuidas kirjutada Azure SQL-andmebaasiga |
| Torm DocumentDB lugeja näidis | Kuidas lugeda: Azure'i DocumentDB |
| Torm DocumentDB kirjutaja näidis | Kuidas kirjutada Azure'i DocumentDB |
| Torm EventHub lugeja näidis | Kuidas lugeda Azure'i sündmuse jaoturi kaudu |
| Torm EventHub kirjutaja näidis | Kuidas kirjutada Azure'i sündmus jaoturi |
| Torm HBase lugeja näidis | Kuidas lugeda HBase Hdinsightiga kogumite kohta |
| Torm HBase kirjutaja näidis | Kuidas kirjutada HBase Hdinsightiga kogumite kohta |
| Torm hübriid näidis | Kuidas kasutada Java komponent |
| Torm näidis | Põhilised word count topoloogia |

> [AZURE.NOTE] HBase lugeja ja kirjutaja näidised abil suhelda mõne HBase kohta Hdinsightiga kobar, mitte HBase Java API HBase REST API-ga.

Selles dokumendis toimingutes on tavaline Storm rakenduse projekti tüüp abil luua uus topoloogia.

## <a name="create-a-c-topology"></a>C# topoloogia loomine

1. Kui teil on juba installitud uusim versioon Hdinsightiga tööriistad Visual Studio, lugege teemat [Hdinsightiga Tools for Visual Studio kasutamise alustamine](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Avage Visual Studio ja valige **fail** > **Uus**ja seejärel **projekti**.

3. Laiendage kuval **Uue projekti** **installitud** > **Mallid**ja valige **Hdinsightiga**. Valige loendist Mallid, **Storm rakendus**. Ekraani allservas Sisestage **WordCount** rakenduse nimi.

    ![pilt](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

4. Pärast seda, kui projekt on loodud, siis peaks olema järgmised failid:

    - **Program.cs**: See määratleb oma projekti topoloogia. Pange tähele, et vaikimisi topoloogia, üks tila ja üks Pikselöök luuakse vaikimisi.

    - **Spout.cs**: näide tila, mis eraldab juhuslike arvude.

    - **Bolt.cs**: näide polt, mis hoiab arvude kiiratava tila arv.

    Projekti loomise osana allalaaditud uusima [SCP.NET pakettide](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/) Nugeti.

    [AZURE.INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

Järgmistes jaotistes, muudate selle projekti põhilised WordCount rakenduseks.

### <a name="implement-the-spout"></a>Rakendada Tila

1. Avage **Spout.cs**. Otsikuid kasutatakse andmete topoloogia lugemiseks välise andmeallikaga. Põhilised tila jaoks on:

    - **NextTuple**: kui tila on lubatud eraldavad uue kordseid tormi nimega.

    - **ACK** (ainult selgituseks topoloogia): tegeleb kinnitused algataja topoloogias kordseid saadetud selle tila jaoks muud komponendid. Tunnistades korteeži võimaldab tila teada, et see on töötlemine õnnestus järgmise etapi komponendid.

    - **Nurjuda** (ainult selgituseks topoloogia): tegeleb kordseid, mis on fail töötlemise muude komponentide topoloogias. See pakub võimalust uuesti eraldavad kordne, et seda saab töödelda uuesti.

2. Asendage **tila** ainekursuse sisu järgmist. See loob tila, mis eraldab CEIP lause topoloogia sisse.

        private Context ctx;
        private Random r = new Random();
        string[] sentences = new string[] {
            "the cow jumped over the moon",
            "an apple a day keeps the doctor away",
            "four score and seven years ago",
            "snow white and the seven dwarfs",
            "i am at two with nature"
        };

        public Spout(Context ctx)
        {
            // Set the instance context
            this.ctx = ctx;

            Context.Logger.Info("Generator constructor called");

            // Declare Output schema
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // The schema for the default output stream is
            // a tuple that contains a string field
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
        }

        // Get an instance of the spout
        public static Spout Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Spout(ctx);
        }

        public void NextTuple(Dictionary<string, Object> parms)
        {
            Context.Logger.Info("NextTuple enter");
            // The sentence to be emitted
            string sentence;

            // Get a random sentence
            sentence = sentences[r.Next(0, sentences.Length - 1)];
            Context.Logger.Info("Emit: {0}", sentence);
            // Emit it
            this.ctx.Emit(new Values(sentence));

            Context.Logger.Info("NextTuple exit");
        }

        public void Ack(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }

        public void Fail(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }
    
    Mõne hetke kaudu aru saada, mis teeb järgmine kood kommentaare lugeda.

### <a name="implement-the-bolts"></a>Rakendada poldid

1. Projekti **Bolt.cs** olemasoleva faili kustutada.

2. **Lahenduste Explorer**, paremklõpsake projekti ja valige **Lisa** > **Uus üksus**. Loendist valige **Storm polt**ja sisestage **Splitter.cs** nimi. Korrake seda teise bolt nimega **Counter.cs**loomiseks.

    - **Splitter.cs**: polt, mis jagab lausete üksikuteks sõnadeks ja eraldab uue voo sõnade rakendatakse.

    - **Counter.cs**: rakendab polt, mis loendab iga sõna ja eraldab uue voo ja sõnade loendamine iga sõna.

    > [AZURE.NOTE] Need poldid lihtsalt lugeda ja kirjutada voole, kuid saate kasutada ka polt allikatest, nt andmebaasist või teenuse suhelda.

3. Avage **Splitter.cs**. Teate, et see on ainult üks viis vaikimisi: **käivitamine**. Seda nimetatakse ka siis, kui poldi saab korteeži töötlemiseks. Siin saate lugeda sissetulevate kordseid töötlemine ja Väljamineva meili kordseid eraldavad.

4. Asendage **tükeldi** ainekursuse sisu järgmine kood:

        private Context ctx;

        // Constructor
        public Splitter(Context ctx)
        {
            Context.Logger.Info("Splitter constructor called");
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // Input contains a tuple with a string field (the sentence)
            inputSchema.Add("default", new List<Type>() { typeof(string) });
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // Outbound contains a tuple with a string field (the word)
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance of the bolt
        public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Splitter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the sentence from the tuple
            string sentence = tuple.GetString(0);
            // Split at space characters
            foreach (string word in sentence.Split(' '))
            {
                Context.Logger.Info("Emit: {0}", word);
                //Emit each word
                this.ctx.Emit(new Values(word));
            }

            Context.Logger.Info("Execute exit");
        }

    Mõne hetke kaudu aru saada, mis teeb järgmine kood kommentaare lugeda.

5. Avage **Counter.cs** ja asendage ainekursuse sisu järgmist:

        private Context ctx;

        // Dictionary for holding words and counts
        private Dictionary<string, int> counts = new Dictionary<string, int>();

        // Constructor
        public Counter(Context ctx)
        {
            Context.Logger.Info("Counter constructor called");
            // Set instance context
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string field - the word
            inputSchema.Add("default", new List<Type>() { typeof(string) });

            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string and integer field - the word and the word count
            outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance
        public static Counter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Counter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the word from the tuple
            string word = tuple.GetString(0);
            // Do we already have an entry for the word in the dictionary?
            // If no, create one with a count of 0
            int count = counts.ContainsKey(word) ? counts[word] : 0;
            // Increment the count
            count++;
            // Update the count in the dictionary
            counts[word] = count;

            Context.Logger.Info("Emit: {0}, count: {1}", word, count);
            // Emit the word and count information
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
            Context.Logger.Info("Execute exit");
        }

    Mõne hetke kaudu aru saada, mis teeb järgmine kood kommentaare lugeda.

### <a name="define-the-topology"></a>Topoloogia määratlemine

Otsikuid ja poldid paigutatakse graafiku, mis määratleb, kuidas andmevahetus osade vahel. See topoloogia graafik on järgmine:

![pilt, kuidas on korraldatud komponendid](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Laused on nõrgunud tila, mis on jaotatud eksemplarides tükeldusriba polt. Tükeldi polt jaotab laused sõnad, mis asuvad ning Counter polt.

Kuna sõnade peetakse kohalik eksemplar Counter, soovime veenduge, et meilivoo kindlaid sõnu sama Counter polt eksemplari nii, et meil on ainult üks näiteks hoida silma peal kindla sõna. Kuid tükeldusriba poldid, eriti pole oluline, mis polt saab mis lause, seega soovime lihtsalt laadimine saldo laused üle nende aknad.

Avage **Program.cs**. Oluliste meetod on **GetTopologyBuilder**, mis võimaldab määratleda topoloogia, mis on esitatud torm. Asendage **GetTopologyBuilder** sisu rakendada eespool kirjeldatud topoloogia järgmine kood:

        // Create a new topology named 'WordCount'
        TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

        // Add the spout to the topology.
        // Name the component 'sentences'
        // Name the field that is emitted as 'sentence'
        topologyBuilder.SetSpout(
            "sentences",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
            },
            1);
        // Add the splitter bolt to the topology.
        // Name the component 'splitter'
        // Name the field that is emitted 'word'
        // Use suffleGrouping to distribute incoming tuples
        //   from the 'sentences' spout across instances
        //   of the splitter
        topologyBuilder.SetBolt(
            "splitter",
            Splitter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
            },
            1).shuffleGrouping("sentences");

        // Add the counter bolt to the topology.
        // Name the component 'counter'
        // Name the fields that are emitted 'word' and 'count'
        // Use fieldsGrouping to ensure that tuples are routed
        //   to counter instances based on the contents of field
        //   position 0 (the word). This could also have been
        //   List<string>(){"word"}.
        //   This ensures that the word 'jumped', for example, will always
        //   go to the same instance
        topologyBuilder.SetBolt(
            "counter",
            Counter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
            },
            1).fieldsGrouping("splitter", new List<int>() { 0 });

        // Add topology config
        topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
        {
            {"topology.kryo.register","[\"[B\"]"}
        });

        return topologyBuilder;

Mõne hetke aru saada, mis teeb järgmine kood kommentaarid läbi lugeda.

## <a name="submit-the-topology"></a>Esitage topoloogia

1. **Solution Exploreris**projekti paremklõpsake ja valige **Edasta torm Hdinsightiga kohta**.

    > [AZURE.NOTE] Kui kuvatakse vastav viip, Sisestage sisselogimise Azure tellimuse. Kui teil on mitu tellimust, logige sisse ühe, mis sisaldab teie Storm Hdinsightiga kobar kohta.

2. Valige oma Storm klõpsake Hdinsightiga kobar **Storm kobar** ripploendist ja valige **Edasta**. Kui esitamise õnnestus **väljundi** aknas abil saate jälgida.

3. Kui topoloogia on edukalt esitatud, **Storm topoloogiatest** jaoks klaster peaks kuvatama. Valige loendist saate vaadata teavet esitatava topoloogia **WordCount** topoloogia.

    > [AZURE.NOTE] Saate vaadata ka **Storm topoloogiatest** **Server**Explorer: **Azure'i**laiendamine > **Hdinsightiga**, paremklõpsake torm Hdinsightiga kobar sisse ja seejärel valige **Vaate Storm topoloogiatest**.

    Otsikuid või poldid linkide abil saate vaadata teavet nende komponentide. Iga valitud avatakse uus aken.

4. Klõpsake **Topoloogia** koondvaate **tappa** topoloogia lõpetada.

    > [AZURE.NOTE] Torm topoloogiatest jätkatakse seni, kuni ta on inaktiveeritud või klaster on kustutatud.

## <a name="transactional-topology"></a>Selgituseks topoloogia

Eelmise topoloogia on transaktsioone. Komponendid sees topoloogia juuruta uuesti läbimängimise sõnumite töötlemine nurjumisel topoloogias komponendi võrra funktsioone. Näide selgituseks topoloogia uue projekti loomine ja valige **Storm valimi** nimega projekti tüüp.

Selgituseks topoloogiatest rakendada toetama andmete taasesitamine järgmist:

- **Metaandmete vahemällu**: tila peab talletada metaandmeid kiiratav nii, et andmeid saab tuua ja kiiratav uuesti, kui tõrge ilmneb andmed. Kuna kiiratava valimi andmed on väike, talletatakse kordus sõnastiku jaoks iga kordse toorandmetega.

- **ACK**: saate helistada iga polt topoloogias `this.ctx.Ack(tuple)` ack, et see on edukalt töödeldud korteeži. Kui kõik poldid on jälitatud korteeži, on `Ack` tila meetod on vaja järgida. See võimaldab eemaldada kordus vahemällu salvestatud andmeid, kuna andmed on täielikult töödeldud tila.

- **Nurjuda**: saate helistada iga polt `this.ctx.Fail(tuple)` näitamaks, et töötlemine on korteeži nurjus. Selle levib soovitud `Fail` tila, kus saate uuesti esitada kordne, kasutades meetodit vahemälus talletatud metaandmete.

- **Jada ID**: režiimis korteeži, järjestuse ID saab määrata. See peaks olema väärtus, mis tuvastab korteeži kordus (Ack ja Fail) töötlemiseks. Näiteks tila **Storm proovi** projekti kasutab järgmisi režiimis andmeid:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    See eraldab uue korteeži, mis sisaldab vaikimisi voog, lause jada ID väärtusega sisalduvate **lastSeqId**. Selle näite puhul **lastSeqId** suurendatakse lihtsalt iga korteeži kiiratav.

Nagu on näidatud **Storm proovi** projekti, kas komponent on selgituseks saab määrata käivitamise ajal konfiguratsiooni põhjal.

## <a name="hybrid-topology"></a>Hübriid topoloogia

Hübriid topoloogiatest, kus on teatud komponendid C# ja teised Java loomiseks saate kasutada ka Hdinsightiga tools for Visual Studio.

Näide hübriid topoloogia uue projekti loomise ja **Storm hübriid**valimi. See loob täielikult kommenteeritud proovi, mis sisaldab mitut topoloogiatest, mis illustreerivad järgmist:

- **Java tila** ja **C# polt**: määratletud **HybridTopology_javaSpout_csharpBolt**

    - Selgituseks versioon on määratletud **HybridTopologyTx_javaSpout_csharpBolt**

- **C# tila** ja **Java polt**: määratletud **HybridTopology_csharpSpout_javaBolt**

    - Selgituseks versioon on määratletud **HybridTopologyTx_csharpSpout_javaBolt**

        > [AZURE.NOTE] Selle versiooni ka näitab, kuidas kasutada Clojure koodi tekstifaili Java osana.

Topoloogia, mida kasutatakse, kui projekt on esitatud vahetamiseks lihtsalt teisaldada selle `[Active(true)]` topoloogia, mida soovite kasutada enne esitamist klaster märge.

> [AZURE.NOTE] Java kõik failid, mis on nõutavad on antud osa selle projekti **JavaDependency** kausta.

Võtke arvesse järgmist loomisel ja esitamine hübriid topoloogia:

- **JavaComponentConstructor** tuleb kasutada luua uue eksemplari Java klassi tila või polt.

- **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** tuleks kasutada serialiseerida andmete või sealt välja Java komponentide JSON Java objektid.

- Topoloogia server esitamisel peab **täiendavad konfiguratsioone** suvandi abil saate määrata **Java failiteed**. Määratud tee peaks olema JAR failid, mis sisaldavad teie Java klassi sisaldava kausta.

### <a name="azure-event-hubs"></a>Azure'i sündmuse jaoturi

SCP.Net versioon 0.9.4.203 tutvustab uutel ja meetod spetsiaalselt töötamiseks soovitud sündmuse jaoturi tila (Java tila mis loeb sündmus keskuse kaudu.) Topoloogia, mis kasutab seda tila loomisel kasutage järgmist:

- **EventHubSpoutConfig** klassi: loob objekti, mis sisaldab tila komponendi konfigureerimine

- **TopologyBuilder.SetEventHubSpout** meetod: lisab sündmuse jaoturi tila komponent topoloogia

> [AZURE.NOTE] Ajal neid lihtsam töötada sündmuse jaoturi tila kui muud Java komponendid, peate kasutama funktsiooni CustomizedInteropJSONSerializer endiselt vahemälukausta tila andmed.

## <a id="configurationmanager"></a>ConfigurationManager abil

Ära ConfigurationManager abil otsida väärtused: bolt ja tila komponendid; See viib kursori null erand. Selle asemel projekti konfigureerimine on läinud Storm topoloogia topoloogia kontekstis paaris /-väärtuse. Iga komponendi, mis sõltub väärtused peavad tuua need konteksti lähtestamisel.

Järgmine kood näitab, kuidas tuua need väärtused:

    public class MyComponent : ISCPBolt
    {
        // To hold configuration information loaded from context
        Configuration configuration;
        ...
        public MyComponent(Context ctx, Dictionary<string, Object> parms)
        {
            // Save a copy of the context for this component instance
            this.ctx = ctx;
            // If it exists, load the configuration for the component
            if(parms.ContainsKey(Constants.USER_CONFIG))
            {
                this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
            }
            // Retrieve the value of "Foo" from configuration
            var foo = this.configuration.AppSettings.Settings["Foo"].Value;
        }
        ...
    }

Kui kasutate on `Get` meetod eksemplari oma komponent tagastamiseks peate veenduma, et see edastab nii on `Context` ja `Dictionary<string, Object>` ehitaja parameetrid. Järgmises näites on näiteks basic `Get` meetod, mis õigesti edastab need väärtused:

    public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new MyComponent(ctx, parms);
    }

## <a name="how-to-update-scpnet"></a>Kuidas värskendada SCP.NET

Tehtud versioonides SCP.NET toetavad paketi versioonitäiendus Nugeti kaudu. Kui uus värskendus on saadaval, saate mõne uuele versioonile ülemineku soov. Käsitsi kontrollida täiendada, tehke järgmist.

1. **Solution Exploreris**, paremklõpsake projekti ja valige **Halda Nugeti paketid**.

2. Paketi manager, valige **värskendused**. Kui värskendus on saadaval, siis see kirjas. Paketi selle installimiseks nuppu **Värskenda** .

> [AZURE.IMPORTANT] Kui projekti on üks SCP.NET, mis ei kasuta Nugeti pakett värskenduste varasemates versioonides loodud, peate tegema uue versiooni kuvamiseks tehke järgmist:
>
> 1. **Solution Exploreris**, paremklõpsake projekti ja valige **Halda Nugeti paketid**.
> 2. **Otsinguväljale** abil otsida ja seejärel lisage **Microsoft.SCP.Net.SDK** projekti.

## <a name="troubleshooting"></a>Tõrkeotsing

### <a name="null-pointer-exceptions"></a>Null kursori erandid

C# topoloogia kasutamisel koos Linux-põhine Hdinsightiga kobar polt ja tila lugeda käitusajal Otsingukonfiguratsiooni sätetest võib tagastada null kursori erandid ConfigurationManager kasutavad komponendid. See juhtub siis laaditud domeeni konfiguratsiooni ei ole komplekti, mis sisaldab projekti.

Projekti konfigureerimine on läinud Storm topoloogia topoloogia kontekstis paaris /-väärtuse ja võib laadida sõnastiku objekt, mille edastatakse oma komponendid, kui nad on lähtestatud.

Järgmises näites näitab laadimise väärtuste määramine topoloogia konteksti, selle dokumendi jaotisest [ConfigurationManager](#configurationmanager) .

### <a name="systemtypeloadexception"></a>System.TypeLoadException

C# topoloogia Linux-põhine Hdinsightiga kobar koos kasutamisel võivad ilmneda järgmine tõrketeade:

    System.TypeLoadException: Failure has occurred while loading a type.

Tavaliselt saabuvat kasutamisel on kahendarvuks mis ei ühildu .NET, mis toetab ühe versiooni.

Jaoks Linux-põhine Hdinsightiga kogumite, veenduge, et projekti kasutab .NET 4.5 koostatud kahendfaile.


### <a name="test-a-topology-locally"></a>Kohalik topoloogia testimine

Kuigi see on lihtne juurutada topoloogia arvutikobaras, mõnel juhul peate topoloogia kohalikult testida. Kasutage järgmist käivitamiseks ja testimiseks näide topoloogia selles õpetuses kohalikult sisse oma arenduskeskkond.

> [AZURE.WARNING] Kohaliku testimine toimib ainult basic, C# ainult topoloogiatest. Ei tohi kasutada kohaliku testimine hübriid topoloogiatest või topoloogiatest, mis kasutavad mitu voole, nagu saate tõrked.

1. **Lahenduste Explorer**, paremklõpsake projekti ja valige **Atribuudid**. Projekti atribuudid, muuta **väljundi tüüp** **Konsooli rakendus**.

    ![väljundi tüüp](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

    > [AZURE.NOTE] Ärge unustage muuta **väljund tüüpi** tagasi **Klassiteek** enne juurutamist topoloogia arvutikobaras.

2. **Lahenduste Explorer**, paremklõpsake projekti ja seejärel valige **Lisa** > **Uus üksus**. Valige **tunni** ja sisestage **LocalTest.cs** klassi nime. Lõpuks nuppu **Lisa**.

3. Avage **LocalTest.cs** ja lisage järgmine **kasutamine** lause ülaosas.

        using Microsoft.SCP;

4. Kasutage järgmist **LocalTest** ainekursuse sisu.

        // Drives the topology components
        public void RunTestCase()
        {
            // An empty dictionary for use when creating components
            Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

            #region Test the spout
            {
                Console.WriteLine("Starting spout");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext spoutCtx = LocalContext.Get();
                // Get a new instance of the spout, using the local context
                Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

                // Emit 10 tuples
                for (int i = 0; i < 10; i++)
                {
                    sentences.NextTuple(emptyDictionary);
                }
                // Use LocalContext to persist the data stream to file
                spoutCtx.WriteMsgQueueToFile("sentences.txt");
                Console.WriteLine("Spout finished");
            }
            #endregion

            #region Test the splitter bolt
            {
                Console.WriteLine("Starting splitter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext splitterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);


                // Set the data stream to the data created by the spout
                splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    splitter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                splitterCtx.WriteMsgQueueToFile("splitter.txt");
                Console.WriteLine("Splitter bolt finished");
            }
            #endregion

            #region Test the counter bolt
            {
                Console.WriteLine("Starting counter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext counterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Counter counter = Counter.Get(counterCtx, emptyDictionary);

                // Set the data stream to the data created by splitter bolt
                counterCtx.ReadFromFileToMsgQueue("splitter.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    counter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                counterCtx.WriteMsgQueueToFile("counter.txt");
                Console.WriteLine("Counter bolt finished");
            }
            #endregion
        }

    Mõne hetke läbi koodi kommentaare lugeda. Järgmine kood kasutab **LocalContext** käivitamiseks komponendid on arenduskeskkond ja see ei lahene andmevoos tekstifailidest kohalikule kettale komponentide vahel.

5. Avage **Program.cs** ja lisage järgmine **Main** meetod.

        Console.WriteLine("Starting tests");
        System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
        // Initialize the runtime
        SCPRuntime.Initialize();

        //If we are not running under the local context, throw an error
        if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
        {
            throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
        }
        // Create test instance
        LocalTest tests = new LocalTest();
        // Run tests
        tests.RunTestCase();
        Console.WriteLine("Tests finished");
        Console.ReadKey();

6. Salvestage soovitud muudatused ja seejärel klõpsake **F5** või valige **silumine** > **Käivitamine silumine** projekti käivitamiseks. Konsooli aknas peaks kuvada ja logige olek kontrollib edenemist. Kui kuvatakse **kontrollib lõpetanud** , vajutage suvalist klahvi, akna sulgemiseks.

7. Otsige üles kataloog, mis sisaldab projekti, näiteks **Windows Exploreri** abil **C:\Users\<your_user_name > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. Selles kaustas **Prügikast**avada, ja klõpsake **silumine**. Peaksite nägema teksti failid, mis toodeti kui testide parandusfunktsiooni: sentences.txt, counter.txt ja splitter.txt. Iga tekstifaili avamine ja andmete uurimine.

    > [AZURE.NOTE] Stringi andmed on jätkunud massiivina decimal väärtuste need failid. Näiteks \[[97,103,111]] **splitter.txt** fail on sõna "ja".

Kuigi testimine lihtsa sõna loendamiseks rakenduse kohalik on päris väga väike tegelik väärtus on, kui teil on keerukas topoloogia, mis suhtleb väliste andmeallikate või sooritab keeruliste andmeanalüüside. Kui töötate sellise projekti, peate oma komponendid probleemid eristamiseks katkestuspunkte ja liikuda koodi määramiseks.

> [AZURE.NOTE] Kindlasti määrama **projekti tüüp** tagasi **Klassiteek** enne juurutamist torm Hdinsightiga kobar kohta.

### <a name="log-information"></a>Logiteave

Saate hõlpsasti logida teavet oma topoloogia komponente, kasutades `Context.Logger`. Näiteks järgmine loob teavitamise Logi kirje:

    Context.Logger.Info("Component started");

Andmeid saab vaadata **Hadoopi teenuse Logi**, mille leiate **Server Explorer**. Laiendage oma Storm Hdinsightiga kobar klõpsake kirjet ja seejärel laiendage **Hadoopi teenuse Logi**. Lõpetuseks, valige logifaili kuvamiseks.

> [AZURE.NOTE] Logid salvestatakse Azure Storage konto, mida kasutatakse klaster. Kui see on mõni muu tellimus, kui olete sisse loginud Visual Studio abil, peate tellimuse, mis sisaldab salvestusruumi konto, mida soovite seda teavet vaadata sisse logida.

### <a name="view-error-information"></a>Tõrketeabe kuvamine

Töötava topoloogia ilmnenud tõrgete kuvamiseks tehke järgmist:

1. **Server Explorer**, paremklõpsake torm Hdinsightiga kobar sisse ja valige **vaate Storm topoloogiatest**.

2. **Tila** ja **poldid**on **Viimase tõrke** veeru teavet ilmnenud on viimase tõrke.

3. Valige osa, mis sisaldab viga loetletud **Tila Id** või **Polt Id** . Mida kuvatakse lehel üksikasjad kirjas lehe allosas jaotise **tõrgete** täiendav teave.

4. Lisateabe saamiseks valige **Port** **haldavad isikud** jaotise lehe kuvamiseks Storm töötaja log viimase mõni minut.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud, kuidas töötada ja juurutamine Visual Studio Storm topoloogiatest Hdinsightiga tööriistad, saate teada, kuidas [protsessi sündmuste Azure'i sündmuse jaoturis torm Hdinsightiga kohta](hdinsight-storm-develop-csharp-event-hub-topology.md).

Näiteks topoloogia C#, mis kinnitanud voo andmete voogu mitme, vt [C# Storm näide](https://github.com/Blackmist/csharp-storm-example).

Leida C# topoloogiatest loomise kohta lisateavet, külastage [SCP.NET GettingStarted.md](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

Veel võimalusi tööta Hdinsightiga ja veel Storm Hdinsightiga proovide, leiate järgmistest:

**Microsofti SCP.NET**

* [SCP programmeerimise juhend](hdinsight-storm-scp-programming-guide.md)

**Apache Storm Hdinsightiga kohta**

- [Juurutada ja kontrollida topoloogiatest Apache torm Hdinsightiga kohta](hdinsight-storm-deploy-monitor-topology.md)

- [Näide topoloogiatest Storm Hdinsightiga kohta](hdinsight-storm-example-topology.md)

**Apache Hadoopi Hdinsightiga**

- [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

- [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)

- [Hadoopi Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)

**Apache HBase Hdinsightiga kohta**

- [Klõpsake Hdinsightiga HBase töötamise alustamine](hdinsight-hbase-tutorial-get-started.md)
