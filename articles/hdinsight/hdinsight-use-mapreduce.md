<properties
   pageTitle="Hadoopi Hdinsightiga koos MapReduce | Microsoft Azure'i"
   description="Saate teada, kuidas käivitada MapReduce töö Hadoopi Hdinsightiga rühmades. Te käivitate põhilised word count toiming rakendatud Java MapReduce tööd."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>Hadoopi Hdinsightiga MapReduce kasutamine

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Sellest artiklist saate teada, kuidas käivitada MapReduce töö Hadoopi Hdinsightiga rühmades. Põhilised word count toiming rakendada Java MapReduce töö käivitamine

##<a id="whatis"></a>Mis on MapReduce?

Hadoopi MapReduce on tarkvara raamistik kirjutamiseks tööd, mis on suure hulga andmete töötlemine. Sisendandmete jagatakse sõltumatu tükkideks, siis töödeldud samal ajal soovitud klaster sõlmed üle. MapReduce töö koosneb kahest funktsioonid:

* **Plaanuri**: tarbib sisendandmete, analüüsib seda (tavaliselt koos filtreerimine ja sortimine toimingud) ja eraldab kordseid (võti ja väärtuse paarideks)
* **Reduktor**: tarbib kordseid kiiratava on plaanuri ja teeb kokkuvõtte toiming, mis loob tulemus on väiksem, kombineeritud plaanuri andmete põhjal

Põhilised word count MapReduce töö näites on kujutatud järgmisel joonisel:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

Selle töö väljund on arv, mitu korda teksti, mis on analüüsida toimunud iga sõna.

* Funktsiooni plaanuri võtab iga rea sisendväärtuste tekstile sisendina ja jaotab selle sõnad. See eraldab võtme/väärtuse paari iga kord, kui sõna esineb sõna järgneb on 1. Väljund sorditakse enne saatmist reduktor.

* Funktsiooni reduktor liidab nende üksikute loendab iga sõna ja eraldab paari üksikväärtus klahvi, mis sisaldab selle sündmuste summa, millele järgneb sõna.

MapReduce saab rakendada erinevaid keeli. Java on kõige levinum rakendamist ja kasutatakse tutvustamise otstarbeks selles dokumendis.

### <a name="hadoop-streaming"></a>Hadoopi Streaming

Keelte või raamistik, mis põhinevad Java ja Java virtuaalse masina (nt Scalding või Cascading,) saate parandusfunktsiooni otse nimega MapReduce töö sarnane Java rakendus. Teised, nagu C# või Python või autonoomse täitmisfailid, peate kasutama Hadoopi streaming.

Hadoopi streaming suhtleb plaanuri ja reduktor STDIN ja STDOUT - plaanuri ja reduktor andmeid lugeda rea haaval stdin ja kirjutada väljund STDOUT. Iga rea lugeda või plaanuri ja reduktor tekitatud peab olema paari /-väärtuse, lõigumärkidega menüü charaacter vormingus:

    [key]/t[value]

Lisateavet leiate teemast [Hadoopi Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html).

Hadoopi voogesitus Hdinsightiga kasutamise näited leiate järgmistest:

* [Arendamise Python MapReduce tööde haldamine](hdinsight-hadoop-streaming-python.md)

##<a id="data"></a>Näidisandmete kohta

Selles näites jaoks näidisandmete, kasutage märkmikud Leonardo Da Vinci, mis on antud Hdinsightiga klaster dokumendi teksti.

Azure'i bloobimälu, mis kasutab Hdinsightiga failisüsteemi Hadoopi kogumite talletatakse näidisandmeid. Hdinsightiga pääsevad bloobimälu **wasb** eesliite abil salvestatud failid. Näiteks sample.log failile juurdepääsu, saate kasutada järgmist süntaksit:

    wasbs:///example/data/gutenberg/davinci.txt

Kuna Azure'i bloobimälu on vaikimisi salvestusruumi jaoks Hdinsightiga, võite kasutada ka faili **/example/data/gutenberg/davinci.txt**abil.

> [AZURE.NOTE] Eelmise süntaksit, **wasbs: / / /** kasutatakse klaster Hdinsightiga jaoks vaikimisi salvestusruumi ümbrises salvestatud failidele juurde pääseda. Kui määrasite täiendavat salvestusruumi kontod, kui teil on ette valmistatud klaster ning soovite nende kontodega salvestatud failidele juurde pääseda, pääsete andmed, määrates container nimi ja salvestusruumi konto aadress. Näiteks **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/gutenberg/davinci.txt**.

##<a id="job"></a>Näide MapReduce kohta

Selles näites kasutatakse MapReduce töö asub **wasbs://example/jars/hadoop-mapreduce-examples.jar**ja need on varustatud Hdinsightiga klaster. See sisaldab sõna count näide vastu **davinci.txt**töötavad.

> [AZURE.NOTE] Hdinsightiga 2.1 kogumite, on **wasbs:///example/jars/hadoop-examples.jar**faili asukoht.

Viide, on word count MapReduce töö Java kood:

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

Kirjutage oma MapReduce töö [arendamise Java MapReduce programmid Hdinsightiga](hdinsight-develop-deploy-java-mapreduce-linux.md)leiate juhised.

##<a id="run"></a>Funktsiooni MapReduce käivitamine

Hdinsightiga saab käitada HiveQL tööd, kasutades mitmesuguseid viise. Järgmise tabeli abil saate otsustada, milline viis teile sobib, siis järgige lühiülevaade linki.

| **Kasutage seda**...                                                    | **..) (.selle toiming**                                       | .. ei ole see **kobar operatsioonisüsteem** | .. .minu **Kliendi operatsioonisüsteem** |
|:-------------------------------------------------------------------|:--------------------------------------------------------|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md)                       | Hadoopi käsuga **SSH**                  | Linux                                     | Linux, Unix, Mac OS X või Windows        |
| [Curl](hdinsight-hadoop-use-mapreduce-curl.md)                     | Esitage töö eemalt **ülejäänud** abil               | Linux või Windows                          | Linux, Unix, Mac OS X või Windows        |
| [Windows PowerShelli](hdinsight-hadoop-use-mapreduce-powershell.md) | Esitage töö kaugühenduse teel **Windows PowerShelli** abil | Linux või Windows                          | Windows                                  |
| [Kaugtöölaua](hdinsight-hadoop-use-mapreduce-remote-desktop)    | Hadoopi käsuga kaudu **Kaugtöölaua**       | Windows                                   | Windows                                  |

##<a id="nextsteps"></a>Järgmised sammud

Kuigi MapReduce pakub võimsaid diagnostika oskusi, võib olla veidi keerulisem juhtslaid. On mitu Java-põhine raamistiku, mis hõlbustavad MapReduce rakendusi nagu siga ja taru, mis võimaldab lihtsam viis andmete kasutamine Hdinsightiga määratlemine. Lisateabe saamiseks lugege järgmisi artikleid:

* [Töötada Java MapReduce programmide Hdinsightiga](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Arendamise Python streaming MapReduce programmide Hdinsightiga](hdinsight-hadoop-streaming-python.md)

* [Arendamise põletushaavad MapReduce töökohtade Apache Hadoopi Hdinsightiga](hdinsight-hadoop-mapreduce-scalding.md)

* [Hdinsightiga taru kasutamine][hdinsight-use-hive]

* [Kasutage siga Hdinsightiga][hdinsight-use-pig]

* [Käivitage Hdinsightiga näidised][hdinsight-samples]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[powershell-install-configure]: ../powershell-install-configure.md

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
