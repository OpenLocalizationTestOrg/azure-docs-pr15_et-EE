<properties
    pageTitle="Hadoopi näidised läbiviimisel Hdinsightiga | Microsoft Azure'i"
    description="Proovi, mis on esitatud Windows Azure Hdinsightiga teenuse kasutamise alustamine. Kasutage PowerShelli skriptide andmete kogumite MapReduce programmid töötavad."
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
    ms.date="10/21/2016"
    ms.author="jgao"/>

#<a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a>Käivitage Hadoopi MapReduce näidised Windowsi-põhiste Hdinsightiga

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Kogum, näidiseid on hõlbustamiseks saate hankida alustatud töötava MapReduce tööd Hadoopi kogumite kasutamine Windows Azure Hdinsightiga. Need näited on kättesaadavad iga teie loodud rühmad Hdinsightiga hallatavate. Töötavad need näidised tutvustada saate käivitada töö Hadoopi kogumite Azure PowerShelli cmdlet-käskude abil.

- [**Loendamine Wordi**][hdinsight-sample-wordcount]: loeb word juhud tekstifaili.
- [**C# streaming sõnade**][hdinsight-sample-csharp-streaming]: loeb word juhud tekstifaili Hadoopi streaming liidest kasutades.
- [**Pii prognoos**][hdinsight-sample-pi-estimator]: kasutab statistilise (arvestusliku Monte Carlo) meetod pii väärtuse.
- [**10 GB Graysort**][hdinsight-sample-10gb-graysort]: käivitage üldine otstarve GraySort 10 GB faili Hdinsightiga abil. On kolm tööd käivitamiseks: Teragen andmeid, Terasort andmete sortimiseks ja kinnitada, et andmed on õigesti sorditud Teravalidate luua.

>[AZURE.NOTE] Lähtekoodi leiate liites. 

Palju täiendav dokumendid on olemas veebis Hadoopi seotud tehnoloogiad, nagu Java-põhine MapReduce programmeerimise streaming ja dokumentatsiooni kohta, kus on kasutusel Windows PowerShelli cmdlet-käskude skriptimise. Järgmiste ressursside kohta leiate lisateavet teemast:

- [Töötada Java MapReduce programmide Hadoopi sisse Hdinsightiga](hdinsight-develop-deploy-java-mapreduce-linux.md)
- [Esitage Hadoopi töökohtade Hdinsightiga](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Sissejuhatus Azure Hdinsightiga][hdinsight-introduction]

Tänapäeval palju inimesi, valige taru ja siga MapReduce üle.  Lisateavet leiate teemast:

- [Kasutage taru sisse Hdinsightiga](hdinsight-use-hive.md)
- [Kasutage siga Hdinsightiga](hdinsight-use-pig.md)
 
**Eeltingimused**:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **mõne Hdinsightiga kobar**. Juhised sellise kogumite saab luua mitmesuguseid võimalusi, vt [loomine Hadoopi kogumite Hdinsightiga sisse](hdinsight-provision-clusters.md).
- **Töökoha Azure PowerShelli abil**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="hdinsight-sample-wordcount"></a>Wordi arv - Java 

Projekti MapReduce edastada loomisel MapReduce töö määratlus. Töö määratlus, saate määrata MapReduce programmifaili jar ja asukoha jar fail, mis on **wasbs:///example/jars/hadoop-mapreduce-examples.jar**ja selle tunni nime, argumendid.  Wordcount MapReduce programm on kaks argumenti: loendamiseks sõnade ja asukoha väljundi jaoks kasutatud lähtefail.

Lähtekoodi leiate [Lisa](#apendix-a---the-word-count-MapReduce-program-in-java).

Java MapReduce programmi, vt - [arendamise Java MapReduce programmide jaoks sisse Hdinsightiga Hadoopi](hdinsight-develop-deploy-java-mapreduce-linux.md) arendamise menetluse
 
**Word count MapReduce töö esitada**

1. Avage **Windows PowerShell ISE**. Juhised leiate teemast [installida ja konfigureerida Azure PowerShelli][powershell-install-configure].
2. Kleepige järgmist PowerShelli skripti:

        $subscriptionName = "<Azure Subscription Name>"
        $resourceGroupName = "<Resource Group Name>"
        $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name
        
        Select-AzureRmSubscription -SubscriptionName $subscriptionName
        
        # Define the MapReduce job
        $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "wordcount" `
                                    -Arguments "wasbs:///example/data/gutenberg/davinci.txt", "wasbs:///example/data/WordCountOutput1"
        
        # Submit the job and wait for job completion
        $cred = Get-Credential -Message "Enter the HDInsight cluster HTTP user credential:" 
        $mrJob = Start-AzureRmHDInsightJob `
                            -ResourceGroupName $resourceGroupName `
                            -ClusterName $clusterName `
                            -HttpCredential $cred `
                            -JobDefinition $mrJobDefinition 
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -JobId $mrJob.JobId 
        
        # Get the job output
        $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
        $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
        $defaultStorageContainer = $cluster.DefaultStorageContainer
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -DefaultStorageAccountName $defaultStorageAccount `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultContainer $defaultStorageContainer  `
            -JobId $mrJob.JobId `
            -DisplayOutputType StandardError

        # Download the job output to the workstation
        $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 
        Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force
        
        # Display the output file
        cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"

    Töö MapReduce toodab fail nimega *osa-r-00000*, mis sisaldab sõna ja loeb. Skript kasutab käsk **findstr** loetleda kõik sõnad, mis sisaldab *"ei"*.

3. Määrake esimese 3 muutujate ja käivitage skript.

## <a name="hdinsight-sample-csharp-streaming"></a>Wordi arv - C# streaming

Hadoopi pakub streaming API MapReduce, mis võimaldab teil kaardi kirjutamine ja vähendada funktsioonide Java erinevas keeles.

> [AZURE.NOTE] Selle õpetuse juhised kehtivad ainult Windowsi-põhiste Hdinsightiga kogumite. Näiteks streaming Linux-põhine Hdinsightiga kogumite, vt [arendamise Python streaming programmid Hdinsightiga](hdinsight-hadoop-streaming-python.md).

Näites kuvatakse plaanuri ja selle reduktor on täitmisfailid, et lugeda sisend [stdin] [ stdin-stdout-stderr] (rida realt) ning väljub [stdout]väljund[stdin-stdout-stderr]. Programm loendab kõik sõnad tekst.

Kui täitmisfaili määratud **mappers**, käivitab iga plaanuri tööülesande käivitatava kui eraldi protsessi, kui selle plaanuri on lähtestatud. Nagu plaanuri käivitub, see teisendab selle sisendi read ja kanalite read [stdin] [ stdin-stdout-stderr] käigus.

Vahepeal on plaanuri kogub rea rakendusse väljundi stdout käigus. See teisendab iga rea /-väärtuse paari, kogutud on plaanuri väljund. Vaikimisi eesliite joone kuni menüü esimene märk on võti ja ülejäänud rida (välja arvatud tabeldusmärgi) on väärtus. Kui rida on pole tabeldusmärgi, terve rea peetakse võti ja väärtus on null.

Kui **pärast**määratud täitmisfaili, käivitab iga reduktor tööülesande käivitatava kui eraldi protsessi, kui selle reduktor on lähtestatud. Kui reduktor käivitub, see muudab oma sisendväärtuste võti paari reaks ja selle kanalite read [stdin] [ stdin-stdout-stderr] käigus.

Vahepeal on reduktor kogub rea rakendusse väljundi [stdout] [ stdin-stdout-stderr] käigus. See muudab iga rea /-väärtuse paari, kogutud väljund on reduktor. Vaikimisi eesliite joone kuni menüü esimene märk on võti ja ülejäänud rida (välja arvatud tabeldusmärgi) on väärtus.

Hadoopi Streaming kasutajaliidese kohta leiate lisateavet teemast [Hadoopi Streaming] [Hadoopi streaming].

**Esitada on C# streaming word count töö**

- Järgige juhiseid artiklis [sõnade - Java](#word-count-java)ja asendage töö määratlus järgmist:

        $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                                -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                                -Mapper "cat.exe" `
                                -Reducer "wc.exe" `
                                -InputPath "/example/data/gutenberg/davinci.txt" `
                                -OutputPath "/example/data/StreamingOutput/wc.txt"  


    Väljundfail on:
    
        example/data/StreamingOutput/wc.txt/part-00000      
                                
## <a name="hdinsight-sample-pi-estimator"></a>PII prognoos

Pii prognoos kasutab statistilise (arvestusliku Monte Carlo) meetod pii väärtuse. Punktide paigutada juhusliku sees üksuse kuuluvad ka ruudu Lisaja ruutu koos tõenäosus on võrdne soovitud ringi pindala ringi pii/4. Pii väärtuse saab hinnata 4R, kus R on punkte, mis on kokku arvu punkte, mis on ruudu ringi sees arvu faktoriaali suhte väärtus. Suurem valimi kasutatud punktide, seda paremini hinnata on.

Skripti, mis on esitatud selle valimi edastab Hadoopi jar töö ja on häälestatud töötama koos väärtuse 16 kaardid, mis on vaja arvutada 10 miljonit valimi punktide parameetrite väärtused. Hinnanguline pii väärtuse parandamiseks saab muuta järgmiste parameetrite väärtused. Viite esimese 10 kümnendkohtade pii on 3.1415926535.

**Esitada pii prognoos töö**

- Järgige juhiseid artiklis [sõnade - Java](#word-count-java)ja asendage töö määratlus järgmist:

        $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "pi" `
                                    -Arguments "16", "10000000"

## <a name="hdinsight-sample-10gb-graysort"></a>10 GB Graysort

See näide kasutab mõõdukas 10GB andmete nii, et seda saab käivitada suhteliselt kiiresti. Kasutab MapReduce rakenduste Owen O'Malley ja Arun Murthy võidetud aastase üldotstarbeline ("daytona") Teratavu sortimise aluseks 2009 määra 0.578 TB/min (100 TB 173 minutites). Selle ja muude sortimist kriteeriumid kohta leiate lisateavet teemast [Sortbenchmark](http://sortbenchmark.org/) saidile.

See näide kasutab MapReduce programmide kolme rühma:

1. **TeraGen** on MapReduce programmi, mille abil saate luua andmete sortimiseks read.
2. **TeraSort** näidised sisendandmete ja kasutab MapReduce andmete sortimiseks kokku tellimusse. TeraSort on standardne MapReduce ülesanded, välja arvatud kohandatud partitioner, mis kasutab sorditud loendi n-1 valimisse tutvustatakse, mida võtme vahemiku jaoks iga vähendamine. Eelkõige kõik võtmed sellise proovi [i-1] < = võti < valimi, [k] saadetakse ma vähendada. See tagab, et väljundid vähendada mul on väiksem kui väljund vähendada i + 1.
3. **TeraValidate** on MapReduce programm, mis kinnitab, et väljund on sorditud globaalselt. See loob ühe kaardi faili kohta kataloogis väljundi ja iga kaart tagab, et iga võti on väiksem või võrdne eelmine. Funktsiooni kaardi loob ka kirjeid iga faili esimese ja viimase klahvide ja funktsiooni vähendamine tagab, et faili esimene võti mul on suurem kui viimase võti faili i-1. Probleemideta on staatusega vähendada väljund abil, mis on vales järjestuses.

Sisend- ja vorming, kõigi kolme rakendused, loeb ja kirjutab tekstifailid õiges vormingus. Vähendada väljund on seatud 1, mitte vaikimisi 3, kopeerimine, kuna võrdlusalus võistlus ei nõua väljundi andmeid korrata mitme sõlmed edasi.

Proovi iga vastab mõnele MapReduce programmide kirjeldatud Sissejuhatus peab kolme toimingut:

1. Andmete sortimine töö **TeraGen** MapReduce abil luua.
2. Andmete sortimiseks töö **TeraSort** MapReduce abil.
3. Veenduge, et andmed on õigesti alusel sorditud teksti **TeraValidate** MapReduce töö.

**Esitada tööd**

- Järgige juhiseid artiklis [sõnade - Java](#word-count-java)ja kasutage järgmist Töödefinitsioonide.

    $teragen = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - klassinimi "teragen" "-argumendid"-Dmapred.map.tasks=50 ","100000000","/ näide / / 10GB – sortimine-andmesisestuse"
    
    $terasort = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - klassinimi "terasort" "-argumendid"-Dmapred.map.tasks=50 ","-Dmapred.reduce.tasks=25 ","/ näide / / 10GB – sortimine-andmesisestuse","/ näide/andmete / 10GB-sortimine-väljundi"
    
    $teravalidate = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - klassinimi "teravalidate" "-argumendid"-Dmapred.map.tasks=50 ","-Dmapred.reduce.tasks=25 ","/ näide/andmete / 10GB-sortimine-väljundi","/ näide/andmete / 10GB – sortimine-kinnitada"


##<a name="next-steps"></a>Järgmised sammud 

Selles artiklis ja näidise artikleid, soovite õpitut käivitamise koos Hdinsightiga rühmad valimites Azure PowerShelli abil. Õpetused Hdinsightiga siga, taru ja MapReduce kasutamise kohta, leiate järgmistest teemadest:

* [Alustamine Hadoopi kasutamine rakenduses Hdinsightiga taru analüüsimiseks mobiiltelefoni kasutamine][hdinsight-get-started]
* [Kasutage siga Hadoopi Hdinsightiga][hdinsight-use-pig]
* [Hadoopi Hdinsightiga taru kasutamine][hdinsight-use-hive]
* [Esitage Hadoopi töökohtade Hdinsightiga] [hdinsight-submit-jobs]
* [Azure Hdinsightiga SDK dokumentatsioon][hdinsight-sdk-documentation]
* [Hadoopi rakenduses Hdinsightiga silumine: tõrketeated] [hdinsight-errors]


## <a name="appendix-a---the-word-count-source-code"></a>Lisa A – Word count lähtekoodi

    package org.apache.hadoop.examples;
    import java.io.IOException;
    import java.util.StringTokenizer;
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

    public static class TokenizerMapper
       extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
      StringTokenizer itr = new StringTokenizer(value.toString());
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        }
      }
    }

    public static class IntSumReducer
       extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                       Context context
                       ) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get();
      }
      result.set(sum);
      context.write(key, result);
      }
    }

    public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length != 2) {
      System.err.println("Usage: wordcount <in> <out>");
      System.exit(2);
        }
    Job job = new Job(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
    }


## <a name="appendix-b---the-word-count-streaming-source-code"></a>Jaotis - sõnaarvestuse streaming lähtekoodi

MapReduce programm kasutab cat.exe rakenduse vastenduse kasutajaliidese voogesituseks teksti konsooli ja wc.exe kohaldamine vähendamine liides, mis on voona dokumendi sõnade arvu loendamiseks. Plaanuri nii reduktor lugeda märki, rea kaupa, standard Sisestuskeel voo (stdin) ja kirjutada väljundit voog (stdout).

    // The source code for the cat.exe (Mapper).

    using System;
    using System.IO;

    namespace cat
    {
        class cat
        {
            static void Main(string[] args)
            {
                if (args.Length > 0)
                {
                    Console.SetIn(new StreamReader(args[0]));
                }

                string line;
                while ((line = Console.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
    }



Plaanuri kood cat.cs faili kasutab mõnda [StreamReader] [ streamreader] objekti sissetuleva voo konsooli, mis kirjutab seejärel voo väljundit voo koos staatilise [Console.Writeline] märke[ console-writeline] meetod.


    // The source code for wc.exe (Reducer) is:

    using System;
    using System.IO;
    using System.Linq;

    namespace wc
    {
        class wc
        {
            static void Main(string[] args)
            {
                string line;
                var count = 0;

                if (args.Length > 0){
                    Console.SetIn(new StreamReader(args[0]));
                }

                while ((line = Console.ReadLine()) != null) {
                    count += line.Count(cr => (cr == ' ' || cr == '\n'));
                }
                Console.WriteLine(count);
            }
        }
    }


Reduktor kood wc.cs faili kasutab mõnda [StreamReader] [ streamreader] objekti lugemise märkide standard Sisestuskeel voo, mis on väljundi cat.exe plaanuri järgi. Nii, nagu see märkide koos [Console.Writeline] [ console-writeline] meetod, see loeb sõnad, tühikute ja iga sõna lõpus lõpetada rida märkide loendamine. Seejärel kirjutab kogusumma väljundit voo koos [Console.Writeline] [ console-writeline] meetod.





## <a name="appendix-c---the-pi-estimator-source-code"></a>Lisa C - lähtekoodi Pi prognoos

Pii prognoos Java koodi, mis sisaldab plaanuri ja reduktor funktsioonid on saadaval kontrolli alla. Plaanuri programmi loob määratud arvu punktide paigutada juhusliku sees üksuse ruut ja seejärel loendab nendes punktides, mis on ringi sees. Reduktor programmi liidetakse loendatud soovitud mappers punktid ja seejärel hindab: valemi 4R, kus R on ringi kokku arvu punkte, mis on ruudu sees arvestatud punktide arvu faktoriaali suhte pii väärtuse.

    /**
    * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements. See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership. The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License. You may obtain a copy of the License at
    *
    * http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or   implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.math.BigDecimal;
    import java.util.Iterator;

    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.BooleanWritable;
    import org.apache.hadoop.io.LongWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Writable;
    import org.apache.hadoop.io.WritableComparable;
    import org.apache.hadoop.io.SequenceFile.CompressionType;
    import org.apache.hadoop.mapred.FileInputFormat;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.MapReduceBase;
    import org.apache.hadoop.mapred.Mapper;
    import org.apache.hadoop.mapred.OutputCollector;
    import org.apache.hadoop.mapred.Reducer;
    import org.apache.hadoop.mapred.Reporter;
    import org.apache.hadoop.mapred.SequenceFileInputFormat;
    import org.apache.hadoop.mapred.SequenceFileOutputFormat;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;


    //A Map-reduce program to estimate the value of Pi
    //using quasi-Monte Carlo method.
    //
    //Mapper:
    //Generate points in a unit square
    //and then count points inside/outside of the inscribed circle of the square.
    //
    //Reducer:
    //Accumulate points inside/outside results from the mappers.
    //Let numTotal = numInside + numOutside.
    //The fraction numInside/numTotal is a rational approximation of
    //the value (Area of the circle)/(Area of the square),
    //where the area of the inscribed circle is Pi/4
    //and the area of unit square is 1.
    //Then, Pi is estimated value to be 4(numInside/numTotal).
    //

    public class PiEstimator extends Configured implements Tool {
    //tmp directory for input/output
    static private final Path TMP_DIR = new Path(
    PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

    //2-dimensional Halton sequence {H(i)},
    //where H(i) is a 2-dimensional point and i >= 1 is the index.
    //Halton sequence is used to generate sample points for Pi estimation.
    private static class HaltonSequence {
    // Bases
    static final int[] P = {2, 3};
    //Maximum number of digits allowed
    static final int[] K = {63, 40};

    private long index;
    private double[] x;
    private double[][] q;
    private int[][] d;

    //Initialize to H(startindex),
    //so the sequence begins with H(startindex+1).
    HaltonSequence(long startindex) {
    index = startindex;
    x = new double[K.length];
    q = new double[K.length][];
    d = new int[K.length][];
    for(int i = 0; i < K.length; i++) {
    q[i] = new double[K[i]];
    d[i] = new int[K[i]];
    }

    for(int i = 0; i < K.length; i++) {
    long k = index;
    x[i] = 0;

    for(int j = 0; j < K[i]; j++) {
    q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
    d[i][j] = (int)(k % P[i]);
    k = (k - d[i][j])/P[i];
    x[i] += d[i][j] * q[i][j];
    }
    }
    }

    //Compute next point.
    //Assume the current point is H(index).
    //Compute H(index+1).
    //@return a 2-dimensional point with coordinates in [0,1)^2
    double[] nextPoint() {
    index++;
    for(int i = 0; i < K.length; i++) {
    for(int j = 0; j < K[i]; j++) {
    d[i][j]++;
    x[i] += q[i][j];
    if (d[i][j] < P[i]) {
    break;
    }
    d[i][j] = 0;
    x[i] -= (j == 0? 1.0: q[i][j-1]);
    }
    }
    return x;
    }
    }

    //Mapper class for Pi estimation.
    //Generate points in a unit square and then
    //count points inside/outside of the inscribed circle of the square.
    public static class PiMapper extends MapReduceBase
    implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

    //Map method.
    //@param offset samples starting from the (offset+1)th sample.
    //@param size the number of samples for this map
    //@param out output {ture->numInside, false->numOutside}
    //@param reporter
    public void map(LongWritable offset,
    LongWritable size,
    OutputCollector<BooleanWritable, LongWritable> out,
    Reporter reporter) throws IOException {

    final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
    long numInside = 0L;
    long numOutside = 0L;

    for(long i = 0; i < size.get(); ) {
    //generate points in a unit square
    final double[] point = haltonsequence.nextPoint();

    //count points inside/outside of the inscribed circle of the square
    final double x = point[0] - 0.5;
    final double y = point[1] - 0.5;
    if (x*x + y*y > 0.25) {
    numOutside++;
    } else {
    numInside++;
    }

    //report status
    i++;
    if (i % 1000 == 0) {
    reporter.setStatus("Generated " + i + " samples.");
    }
    }

    //output map results
    out.collect(new BooleanWritable(true), new LongWritable(numInside));
    out.collect(new BooleanWritable(false), new LongWritable(numOutside));
    }
    }


    //Reducer class for Pi estimation.
    //Accumulate points inside/outside results from the mappers.
    public static class PiReducer extends MapReduceBase
    implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

    private long numInside = 0;
    private long numOutside = 0;
    private JobConf conf; //configuration for accessing the file system

    //Store job configuration.
    @Override
    public void configure(JobConf job) {
    conf = job;
    }


    // Accumulate number of points inside/outside results from the mappers.
    // @param isInside Is the points inside?
    // @param values An iterator to a list of point counts
    // @param output dummy, not used here.
    // @param reporter

    public void reduce(BooleanWritable isInside,
    Iterator<LongWritable> values,
    OutputCollector<WritableComparable<?>, Writable> output,
    Reporter reporter) throws IOException {
    if (isInside.get()) {
    for(; values.hasNext(); numInside += values.next().get());
    } else {
    for(; values.hasNext(); numOutside += values.next().get());
    }
    }

    //Reduce task done, write output to a file.
    @Override
    public void close() throws IOException {
    //write output to a file
    Path outDir = new Path(TMP_DIR, "out");
    Path outFile = new Path(outDir, "reduce-out");
    FileSystem fileSys = FileSystem.get(conf);
    SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
    outFile, LongWritable.class, LongWritable.class,
    CompressionType.NONE);
    writer.append(new LongWritable(numInside), new LongWritable(numOutside));
    writer.close();
    }
    }

    //Run a map/reduce job for estimating Pi.
    //@return the estimated value of Pi.
    public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
    )
    throws IOException {
    //setup job conf
    jobConf.setJobName(PiEstimator.class.getSimpleName());

    jobConf.setInputFormat(SequenceFileInputFormat.class);

    jobConf.setOutputKeyClass(BooleanWritable.class);
    jobConf.setOutputValueClass(LongWritable.class);
    jobConf.setOutputFormat(SequenceFileOutputFormat.class);

    jobConf.setMapperClass(PiMapper.class);
    jobConf.setNumMapTasks(numMaps);

    jobConf.setReducerClass(PiReducer.class);
    jobConf.setNumReduceTasks(1);

    // turn off speculative execution, because DFS doesn't handle
    // multiple writers to the same file.
    jobConf.setSpeculativeExecution(false);

    //setup input/output directories
    final Path inDir = new Path(TMP_DIR, "in");
    final Path outDir = new Path(TMP_DIR, "out");
    FileInputFormat.setInputPaths(jobConf, inDir);
    FileOutputFormat.setOutputPath(jobConf, outDir);

    final FileSystem fs = FileSystem.get(jobConf);
    if (fs.exists(TMP_DIR)) {
     throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
     + " already exists. Please remove it first.");
     }
     if (!fs.mkdirs(inDir)) {
     throw new IOException("Cannot create input directory " + inDir);
     }

     //generate an input file for each map task
     try {
     for(int i=0; i < numMaps; ++i) {
     final Path file = new Path(inDir, "part"+i);
     final LongWritable offset = new LongWritable(i * numPoints);
     final LongWritable size = new LongWritable(numPoints);
     final SequenceFile.Writer writer = SequenceFile.createWriter(
     fs, jobConf, file,
     LongWritable.class, LongWritable.class, CompressionType.NONE);
     try {
     writer.append(offset, size);
     } finally {
     writer.close();
     }
     System.out.println("Wrote input for Map #"+i);
     }

     //start a map/reduce job
     System.out.println("Starting Job");
     final long startTime = System.currentTimeMillis();
     JobClient.runJob(jobConf);
     final double duration = (System.currentTimeMillis() - startTime)/1000.0;
     System.out.println("Job Finished in " + duration + " seconds");

     //read outputs
     Path inFile = new Path(outDir, "reduce-out");
     LongWritable numInside = new LongWritable();
     LongWritable numOutside = new LongWritable();
     SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
     try {
     reader.next(numInside, numOutside);
     } finally {
     reader.close();
     }

     //compute estimated value
     return BigDecimal.valueOf(4).setScale(20)
     .multiply(BigDecimal.valueOf(numInside.get()))
     .divide(BigDecimal.valueOf(numMaps))
     .divide(BigDecimal.valueOf(numPoints));
     } finally {
     fs.delete(TMP_DIR, true);
     }
     }

    //Parse arguments and then runs a map/reduce job.
    //Print output in standard out.
    //@return a non-zero if there is an error. Otherwise, return 0.
     public int run(String[] args) throws Exception {
     if (args.length != 2) {
     System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
     ToolRunner.printGenericCommandUsage(System.err);
     return -1;
     }

     final int nMaps = Integer.parseInt(args[0]);
     final long nSamples = Long.parseLong(args[1]);

     System.out.println("Number of Maps = " + nMaps);
     System.out.println("Samples per Map = " + nSamples);

     final JobConf jobConf = new JobConf(getConf(), getClass());
     System.out.println("Estimated value of Pi is "
     + estimate(nMaps, nSamples, jobConf));
     return 0;
     }

     //main method for running it as a stand alone command.
     public static void main(String[] argv) throws Exception {
     System.exit(ToolRunner.run(null, new PiEstimator(), argv));
     }
     }
     
## <a name="appendix-d---the-10gb-graysort-source-code"></a>Lisa D - 10gb graysort lähtekoodi

Koodi TeraSort MapReduce programmi jaoks on esitatud selle jaotise kontrollimiseks.


    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     *     http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

    package org.apache.hadoop.examples.terasort;

    import java.io.IOException;
    import java.io.PrintStream;
    import java.net.URI;
    import java.util.ArrayList;
    import java.util.List;

    import org.apache.commons.logging.Log;
    import org.apache.commons.logging.LogFactory;
    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.filecache.DistributedCache;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.NullWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.Partitioner;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;

    /**
     * Generates the sampled split points, launches the job,
     * and waits for it to finish.
     * <p>
     * To run the program:
     * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
     */

    public class TeraSort extends Configured implements Tool {
      private static final Log LOG = LogFactory.getLog(TeraSort.class);

      /**
       * A partitioner that splits text keys into roughly equal
       * partitions in a global sorted order.
       */

      static class TotalOrderPartitioner implements Partitioner<Text,Text>{
        private TrieNode trie;
        private Text[] splitPoints;

        /**
         * A generic trie node
         */
        static abstract class TrieNode {
          private int level;
          TrieNode(int level) {
            this.level = level;
          }
          abstract int findPartition(Text key);
          abstract void print(PrintStream strm) throws IOException;
          int getLevel() {
            return level;
          }
        }

        /**
         * An inner trie node that contains 256 children based on the next
         * character.
         */
        static class InnerTrieNode extends TrieNode {
          private TrieNode[] child = new TrieNode[256];

          InnerTrieNode(int level) {
            super(level);
          }
          int findPartition(Text key) {
            int level = getLevel();
            if (key.getLength() <= level) {
              return child[0].findPartition(key);
            }
            return child[key.getBytes()[level]].findPartition(key);
          }
          void setChild(int idx, TrieNode child) {
            this.child[idx] = child;
          }
          void print(PrintStream strm) throws IOException {
            for(int ch=0; ch < 255; ++ch) {
              for(int i = 0; i < 2*getLevel(); ++i) {
                strm.print(' ');
              }
              strm.print(ch);
              strm.println(" ->");
              if (child[ch] != null) {
                child[ch].print(strm);
              }
            }
          }
        }

        /**
         * A leaf trie node that does string compares to figure out where the given
         * key belongs between lower..upper.
         */
        static class LeafTrieNode extends TrieNode {
          int lower;
          int upper;
          Text[] splitPoints;
          LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
            super(level);
            this.splitPoints = splitPoints;
            this.lower = lower;
            this.upper = upper;
          }
          int findPartition(Text key) {
            for(int i=lower; i<upper; ++i) {
              if (splitPoints[i].compareTo(key) >= 0) {
                return i;
              }
            }
            return upper;
          }
          void print(PrintStream strm) throws IOException {
            for(int i = 0; i < 2*getLevel(); ++i) {
              strm.print(' ');
            }
            strm.print(lower);
            strm.print(", ");
            strm.println(upper);
          }
        }


        /**
         * Read the cut points from the given sequence file.
         * @param fs the file system
         * @param p the path to read
         * @param job the job config
         * @return the strings to split the partitions on
         * @throws IOException
         */
        private static Text[] readPartitions(FileSystem fs, Path p,
                                             JobConf job) throws IOException {
          SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
          List<Text> parts = new ArrayList<Text>();
          Text key = new Text();
          NullWritable value = NullWritable.get();
          while (reader.next(key, value)) {
            parts.add(key);
            key = new Text();
          }
          reader.close();
          return parts.toArray(new Text[parts.size()]);  
        }

        /**
         * Given a sorted set of cut points, build a trie that will find the correct
         * partition quickly.
         * @param splits the list of cut points
         * @param lower the lower bound of partitions 0..numPartitions-1
         * @param upper the upper bound of partitions 0..numPartitions-1
         * @param prefix the prefix that we have already checked against
         * @param maxDepth the maximum depth we will build a trie for
         * @return the trie node that will divide the splits correctly
         */
        private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                          Text prefix, int maxDepth) {
          int depth = prefix.getLength();
          if (depth >= maxDepth || lower == upper) {
            return new LeafTrieNode(depth, splits, lower, upper);
          }
          InnerTrieNode result = new InnerTrieNode(depth);
          Text trial = new Text(prefix);
          // append an extra byte on to the prefix
          trial.append(new byte[1], 0, 1);
          int currentBound = lower;
          for(int ch = 0; ch < 255; ++ch) {
            trial.getBytes()[depth] = (byte) (ch + 1);
            lower = currentBound;
            while (currentBound < upper) {
              if (splits[currentBound].compareTo(trial) >= 0) {
                break;
              }
              currentBound += 1;
            }
            trial.getBytes()[depth] = (byte) ch;
            result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                         maxDepth);
          }
          // pick up the rest
          trial.getBytes()[depth] = 127;
          result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                        maxDepth);
          return result;
        }

        public void configure(JobConf job) {
          try {
            FileSystem fs = FileSystem.getLocal(job);
            Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
            splitPoints = readPartitions(fs, partFile, job);
            trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
          } catch (IOException ie) {
            throw new IllegalArgumentException("can't read paritions file", ie);
          }
        }

        public TotalOrderPartitioner() {
        }

        public int getPartition(Text key, Text value, int numPartitions) {
          return trie.findPartition(key);
        }

      }

      public int run(String[] args) throws Exception {
        LOG.info("starting");
        JobConf job = (JobConf) getConf();
        Path inputDir = new Path(args[0]);
        inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
        Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
        URI partitionUri = new URI(partitionFile.toString() +
                                   "#" + TeraInputFormat.PARTITION_FILENAME);
        TeraInputFormat.setInputPaths(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        job.setJobName("TeraSort");
        job.setJarByClass(TeraSort.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(Text.class);
        job.setInputFormat(TeraInputFormat.class);
        job.setOutputFormat(TeraOutputFormat.class);
        job.setPartitionerClass(TotalOrderPartitioner.class);
        TeraInputFormat.writePartitionFile(job, partitionFile);
        DistributedCache.addCacheFile(partitionUri, job);
        DistributedCache.createSymlink(job);
        job.setInt("dfs.replication", 1);
        TeraOutputFormat.setFinalSync(job, true);
        JobClient.runJob(job);
        LOG.info("done");
        return 0;
      }

      /**
       * @param args
       */

      public static void main(String[] args) throws Exception {
        int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
        System.exit(res);
      }
    }














 




[hdinsight-errors]: hdinsight-debug-jobs.md

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md


[powershell-install-configure]: ../powershell-install-configure.md

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
