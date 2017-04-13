<properties
    pageTitle="Andmed koos Microsoft Avro Library serialiseerida | Microsoft Azure'i"
    description="Siit saate teada, kuidas Windows Azure Hdinsightiga kasutab Avro serialiseerida suur andmed."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>


# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Serialiseerida andmete Hadoopi with Microsoft Avro Library

Selles teemas kirjeldatakse, kuidas kasutada <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro teegi</a> serialiseerida objektide ja muude andmestruktuurid voogu selleks, et püsida neid mälu, andmebaasi või faili ja kuidas need taastada algse objektide deserialiseerimiseks.

[AZURE.INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

##<a name="apache-avro"></a>Apache Avro
<a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro teegi</a> rakendab Apache Avro andmete sariväljaanne süsteemi Microsoft.NET keskkonnas. Apache Avro pakub tihendatud kahendarvu andmevahetusvorming sariväljaanne. <a href="http://www.json.org" target="_blank">JSON</a> kasutab määratleda puhvrina keele interoperability keele diagnostika skeem. Ühe keelega seeriasertide andmeid saab lugeda teise. Praegu C, C++, C#, Java, PHP, Python ja Ruby on toetatud. Vormingu üksikasjalikku teavet leiate <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro määratlus</a>. Pange tähele, et Microsoft Avro Library praegune versioon ei toeta kaugprotseduurikutse kõned (kaugprotseduurikutsete) osa sellest määratlus.

Objekti Avro süsteemi sarjadesse jaotatud kujutis koosneb kahest osast: skeemi ja tegelik väärtus. Avro skeemi kirjeldatakse keele sõltumatul andmemudeli sarjadesse jaotatud andmed koos JSON. See on esitus-kõrvuti binaarkuju andmete abil. Kellel on binaarkuju eraldi skeemi võimaldab kirjutatakse pole väärtust kohta üldkulud, tegemise sariväljaanne kiire ja kujutis koos iga objekti.

##<a name="the-hadoop-scenario"></a>Hadoopi stsenaarium
Apache Avro serialiseerimisvormingus kasutatakse ulatuslikult Windows Azure Hdinsightiga ja muudes Apache Hadoop. Avro pakub mugav viis tähistada keerukate andmestruktuurid Hadoopi MapReduce projekti raames. Avro failid (Avro objekti container faili) vorming on loodud toetama jaotatud MapReduce programmeerimise mudel. Võtme funktsiooni, mis võimaldab jaotuse on "splittable" ühte saate otsida mis tahes faili punktis ja lugemise alustamine kindla plokk failid.

##<a name="serialization-in-avro-library"></a>Sariväljaanne Avro teegis
.Net-i teegi jaoks Avro toetab jaotamine objektide kahel viisil:

- **peegelduse** - tüüpi The JSON skeemi automaatselt ehitatud andmete .NET tüübid, millega seeriasertide lepingu atribuudid.
- **üldise kirje** - A JSON skeemi on otseselt määratud kirje, mis tähistab [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) ainekursuse, kui .NET tüüpe pole skeemiga seeriasertide andmete kirjeldamiseks.

Kui andmete skeemiga on teada kirjutaja ja lugeja voo, saate andmeid saata ilma selle skeemi. Juhul kui kasutatakse Avro objekti container faili, talletatakse skeemiga failis. Saate määrata muid parameetreid, näiteks kasutatud andmete pakkimine, kodek. Need stsenaariumid on lähemalt kirjeldatud ja illustreeritud kood allpool toodud näidetes.


## <a name="install-avro-library"></a>Installige Avro teek

Järgnevalt on nõutav enne installimist teek.

- <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4.</a>
- <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 või uuemad versioonid)

Pange tähele, et Newtonsoft.Json.dll sõltuvus laaditakse automaatselt Microsoft Avro Library installi. Järgnevalt on esitatud järgmine kord.


Microsoft Avro Library levitatakse Nugeti paketina, mille saab installida Visual Studio kaudu järgmist:

1. Valige menüü **Projekt** -> **Halda Nugeti pakettide...**
2. "Microsoft.Hadoop.Avro" **Online otsing** väljale Otsi.
3. **Installige** kõrval nuppu **Microsoft Azure Hdinsightiga Avro teek**.

Pange tähele, et selle Newtonsoft.Json.dll (> = 6.0.4) sõltuvus ka koos Microsoft Avro Library automaatselt alla.

Kui soovite külastage <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsofti Avro teegi avalehel</a> praeguse väljalaskemärkmed lugeda.


Microsoft Avro Library lähtekood on saadaval veebisaidil <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro teegi avalehel</a>.

##<a name="compile-schemas-using-avro-library"></a>Kompileerida skeemid Avro teegi kasutamine

Microsoft Avro Library sisaldab koodi genereerimine kasuliku, mis võimaldab luua C# tüüpi automaatselt eelnevalt määratletud JSON skeemi alusel. Koodi genereerimine kasuliku ei ole jaotatud kahendarvu käivitatava, kuid saate hõlpsalt ehitatakse kaudu järgmist:

1. Alla laadida <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK Hadoopi</a>ZIP-faili Hdinsightiga SDK lähtekoodi uusima versiooniga. (Ikooni **alla laadida** , mitte **allalaadimist** vahekaarti.)

2. Ekstrakti Hdinsightiga SDK kataloogi arvutisse koos .NET Framework 4 installitud ja Interneti-ühendus vajalik sõltuvus NuGet-paketid allalaadimiseks. Allpool me oletame, et lähtekoodi ekstraktimist C:\SDK.

3. Minge kausta C:\SDK\src\Microsoft.Hadoop.Avro.Tools ja käivitage build.bat. (Faili helistab MSBuild .NET Frameworki jaotuse 32-bitine versioon. Kui soovite kasutada Office'i 64-bitine versioon, redigeerida build.bat, märkuste failis.) Veenduge, et koostamine on eduka. (Mõnes süsteemis MSBuild võib põhjustada hoiatusi. Need hoiatused ei mõjuta kasuliku kui vigu pole koostamine.)

4. Kompileeritud kasuliku asub C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.


Tutvustab käsurea süntaks, käivitage järgmine käsk: kaust, kus asub koodi genereerimine kasuliku:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

Kasuliku katsetada, saate luua JSON skeemi näidisfaili koos lähtekoodi C# tunnid. Käivitage järgmine käsk:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

See peaks andes kaks C# praegust kausta failid: SensorData.cs ja Location.cs.

Loogika teisendamisel JSON skeemi tüüpi C# koodi genereerimine kasuliku kasutav, vt fail GenerationVerification.feature asub C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.

Pange tähele, et nimeruumid ekstraktitakse JSON skeem, mis on kirjeldatud eelmises punktis mainitud faili loogika abil. Skeemi ekstraktimist nimeruumid ülimuslik sõltumata on esitatud kasuliku käsureal parameetriga/n. Kui soovite nimeruumid, mis sisalduvad skeemiga alistada, kasutage parameetrit /nf. Kõik nimeruumid muutmiseks soovitud SampleJSONSchema.avsc my.own.nspace, käivitage järgmine käsk:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="samples"></a>Näidised
Kuus selles teemas esitatud näidetest ei toeta Microsoft Avro Library erinevates stsenaariumides. Microsoft Avro Library on mõeldud töötamiseks mis tahes voo. Nendes näidetes manipuleeritakse andmete voogu mälu, mitte faili voole või andmebaaside lihtne ja järjepidevuse kaudu. Lähenemisviisi tootmiskeskkonnas sõltub täpse stsenaarium nõuetele, andmeallika ja maht, jõudluse piiranguid ja ka muud tegurid.

Kaks esimest näited serialiseerida ja andmeatribuutide andmed üheks mälu voo puhvrid peegeldus ja üldise kirjete abil tehke järgmist. Kummalgi juhul skeemiga eeldatakse, et lugejad ja poolt ühiselt riba-kohta.

Kolmas ja neljas näited serialiseerida ja andmeatribuutide andmete Avro objekti container failid abil tehke järgmist. Kui andmed on salvestatud Avro container failis, selle skeem alati säilitatakse see Kuna skeemiga peab olema jagatud vahemäluasukohaga.

Neli esimest näiteid valimi saab alla laadida <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-86055923" target="_blank">Azure koodinäiteid</a> saidile.

Viienda näites kohandatud tihendamise kodek Avro objekti container failide kasutamise kohta. Selles näites kood sisaldava proovi saab alla laadida <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111" target="_blank">Azure koodinäiteid</a> saidile.

Kuuenda valimi näitab, kuidas kasutada Avro sariväljaanne Azure'i bloobimälu andmete üleslaadimine ja seejärel analüüsida kasutades taru mõne Hdinsightiga (Hadoopi) kobar. See saab alla laadida <a href="https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure koodinäiteid</a> saidile.

Siin on lingid kuus näidised, mida käsitletakse teemat.

 * <a href="#Scenario1">**Peegelduse sariväljaanne**</a> - The JSON skeemi tüübid seeriasertide automaatselt ehitatud andmete lepingu atribuute.
 * <a href="#Scenario2">**Üldise kirje sariväljaanne**</a> - The JSON skeemi järel konkreetselt nimetatud kirje, kui pole .NET tüüp on saadaval peegeldus.
 * <a href="#Scenario3">**Sariväljaanne abil objekti container faile peegelduse**</a> - The JSON skeemi automaatselt ehitatud ja koos sarjadesse jaotatud andmetega Avro objekti container faili kaudu ühiskasutusse antud.
 * <a href="#Scenario4">**Objekti container failide üldise kirjetega kasutamine sariväljaanne**</a> - The JSON skeemi on konkreetselt sariväljaanne enne määratud ja koos andmetega Avro objekti container faili kaudu ühiskasutusse antud.
 * <a href="#Scenario5">**Objekti container failide kodekiga kohandatud tihendamise abil sariväljaanne**</a> – näiteks näitab, kuidas luua kohandatud .net-i rakendamine Deflate andmete tihendamise kodekiga Avro objekti container faili.
 * <a href="#Scenario6">**Abil Avro üleslaadimiseks teenuse Microsoft Azure Hdinsightiga andmed**</a> – näiteks näitab, kuidas Avro sariväljaanne suhtleb Hdinsightiga teenus. Juurdepääs on Windows Azure Hdinsightiga kobar ja Azure active tellimus on vaja käivitada selles näites.

###<a name="Scenario1"></a>Näide 1: Peegeldus sariväljaanne

JSON skeemi sisutüüpide jaoks saab automaatselt ehitada Microsoft Avro Library kaudu andmete peegeldus, C# objektide seeriasertide lepingu atribuute. Microsoft Avro Library loob mõne [**IAvroSeralizer<T> **](http://msdn.microsoft.com/library/dn627341.aspx) tuvastamiseks seeriasertide väljad.

Selles näites objektid ( **SensorData** klass koos liikme **asukoha** kirjel) on seeriasertide mälu voog ja selle voo on omakorda deserialized. Tulem on siis võrreldes kinnitada, et taastatud **SensorData** objekt on identne algse algne eksemplar.

Selles näites skeemiga eeldatakse, et lugejad ja poolt, jagatud nii Avro objekti container vorming ei ole vaja. Näiteks serialiseerida ja andmeatribuutide andmed üheks mälu puhvrid abil peegeldus objekti container vorming, kui skeemiga peab jagatud andmetega, vt <a href="#Scenario3">sariväljaanne objekti container failide peegeldus kasutamine</a>.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-2-serialization-with-a-generic-record"></a>Näide 2: Sariväljaanne üldise kirje

JSON skeemi saab otseselt määrata üldise kirje kui peegeldus ei saa kasutada, kuna andmeid ei saa esindatud .NET tunnid, kellel on andmete kaudu. See meetod on tavaliselt aeglasem kui peegeldus abil. Sellisel juhul skeemi jaoks andmete võib olla dünaamiline, st ei saa teada kompileerimise ajal. Komaga eraldatud väärtuste (CSV) faile, mille skeemi pole teada enne, kui see on muutnud Avro vormingusse käitusajal andmed on kujutatud dünaamiline stsenaarium selline.

Selles näites on näha, kuidas luua ja kasutada ka [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) määrata JSON skeemi, kuidas see andmetega asustada ja serialiseerida ja andmeatribuutide seda. Tulem on siis võrreldes kinnitada, et taastatud kirje on identne algse algne eksemplar.

Selles näites skeemiga eeldatakse, et lugejad ja poolt, jagatud nii Avro objekti container vorming ei ole vaja. Näiteks serialiseerida ja andmeatribuutide andmed üheks mälu puhvrid abil objekti container vorming üldine kirje, kui skeemiga peab olema lisatud sarjadesse jaotatud andmetega, vt <a href="#Scenario4">sariväljaanne objekti container failide üldise kirjetega kasutamise</a> näide.


    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn will be then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Näide 3: Sariväljaanne peegeldus objekti container failide ja sariväljaanne kasutamine

Selles näites on sarnane stsenaarium <a href="#Scenario1">esimene näide</a>, kus skeemiga peidetult määratud peegeldus. Erinevus on, et siin, skeemi ei eeldatakse, et lugejale, et see deserializes teada. **SensorData** objekte seeriasertide ja nende peidetult määratud skeemi on talletatud Avro objekti container faili tähistab [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) klassi.

Selles näites, kus on seeriasertide andmed [**SequentialWriter<SensorData> **](http://msdn.microsoft.com/library/dn627340.aspx) ja deserialized koos [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx). Tulem on võrreldakse algse eksemplari identiteedi tagamiseks.

Objekti container faili andmetele, on tihendatud kaudu vaikimisi [**Deflate**] [ deflate-100] tihendamise kodekiga .NET Framework 4. Vt <a href="#Scenario5">viienda näites</a> saate teada, kuidas kasutada uuem ja parem versiooni [**Deflate**] selles teemas[ deflate-110] tihendamise kodekiga .NET Framework 4.5 saadaval.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will be compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Näide 4: Sariväljaanne objekti container failide ja sariväljaanne kasutamine koos üldise kirje

Selles näites on sarnane stsenaarium <a href="#Scenario2">teine näide</a>, kus skeemiga järel konkreetselt nimetatud JSON. Erinevus on, et siin, skeemi ei eeldatakse, et lugejale, et see deserializes teada.

Testi andmehulga [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objektide loendi kaudu konkreetselt määratletud JSON skeemiga koguda ja seejärel salvestatud tähistab [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) klassi objekti container faili. Container loob kirjutaja, kasutatava serialiseerida tihendamata mälu voog, siis salvestatud faili andmed. Kasutatud loomise lugejale parameetri [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) määrab, et andmed pole võimalik tihendada.

Andmed on failist loetud ja deserialized kogumise objektid. Selle saidikogumi võrreldakse Avro kirjeid nii, et kinnitada, et need on identne algse loendit.


    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will not be compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




###<a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Näide 5: Sariväljaanne kohandatud tihendamise kodekiga objekti container failide kasutamine

Viienda näites kohandatud tihendamise kodek Avro objekti container failide kasutamise kohta. Selles näites kood sisaldava proovi saab alla laadida [Azure koodinäiteid](http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111) saidile.

[Avro määratlus](http://avro.apache.org/docs/current/spec.html#Required+Codecs) võimaldab kasutamist on valikuline tihendamise kodekiga (lisaks **Null** ja **Deflate** vaikesätted). Selles näites ei rakenda täiesti uue kodekiga, nt käre (nimetatakse toetatud valikuline kodek [Avro Specification](http://avro.apache.org/docs/current/spec.html#snappy)). See näitab, kuidas kasutada .NET Framework 4.5 rakendamise [**Deflate**] [ deflate-110] kodekiga, mis pakub parem tihendamise algoritmi põhjal [zlib](http://zlib.net/) tihendamise teegis kui vaikeversiooniks .NET Framework 4.


    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It will catch the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it will rely on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

###<a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Näide 6: Abil Avro üles laadida andmete teenuse Microsoft Azure Hdinsightiga

Kuuenda näide illustreerib mõningaid teenusega Windows Azure Hdinsightiga nendega seotud programmeerimise viise. Selles näites kood sisaldava proovi saab alla laadida [Azure koodinäiteid](https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3) saidile.

Valimi teeb järgmist.

* Loob ühenduse mõne olemasoleva Hdinsightiga teenuse kobar.
* Serializes mitu CSV-faili ja laadib tulemi Azure'i bloobimälu. (CSV-failid on jaotatud koos valimi ja tähistada väljavõte AMEX Stock ajalooliste andmete jaotatud [Infochimps](http://www.infochimps.com/) perioodi 1970-2010. Valimi loeb CSV-faili andmeid, teisendab kirjete **Stock** klassi eksemplarid ja seejärel serializes need peegeldus abil. Börsidiagrammi tüüp määratlus on loodud JSON skeemi Microsoft Avro Library koodi genereerimine kasuliku kaudu.
* Loob uue välise tabeli nimega **varude** taru ja seda, et andmed on üles laaditud eelmises etapis lingid.
* Käivitab päringu abil taru üle **varude** tabeli.

Lisaks valimi teostab puhastamise protseduuri enne ja pärast põhi toimingute tegemiseks. Ajal puhastamine, eemaldatakse kõik seotud Azure'i bloobimälu andmete ja kaustad ja taru tabeli katkeb. Samuti võite kutsuda puhastamise protseduuri valimi käsurea kaudu.

Valimi on järgmine kohustuslik tarkvara.

* Microsoft Azure'i aktiivne tellimus ja selle tellimuse ID-ga.
* Tellimuse koos vastavate privaatvõti serdi haldus. Serdi peaks olema installitud praeguse kasutaja säilitamise valimi käivitamiseks kasutatud arvutis.
* Aktiivse Hdinsightiga kobar.
* Azure Storage konto seotud Hdinsightiga kobar eelmise nõutav, koos vastavate esmane või teisene kiirklahv.

Kogu teave eeltingimused tuleb sisestada valimi konfiguratsiooni faili enne, kui valim on käivitada. On kaks võimalust seda teha.

* Valimi juurkaust app.config faili redigeerimine ja siis koostada valimi
* Esmalt koostada valimi ja siis käsku Redigeeri AvroHDISample.exe.config kataloogis koostamine

Mõlemal juhul tuleks teha kõik muudatused on **<appSettings>** jaotis Sätted. Järgige kommentaarid faili.
Valimi käitatakse käsurea kaudu, kasutades järgmist käsku (vastasel juhul, kui ZIP-faili abil valimi oli eeldatakse, et ekstraktida C:\AvroHDISample; kui kasutada asjakohaste faili tee):

    AvroHDISample run C:\AvroHDISample\Data

Puhasta klaster, käivitage järgmine käsk:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
