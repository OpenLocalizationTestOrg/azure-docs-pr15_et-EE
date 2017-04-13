<properties
    pageTitle="Töötada Java MapReduce programmide Linux-põhine Hdinsightiga | Microsoft Azure'i"
    description="Saate teada, kuidas Java MapReduce programmide arendamise ja juurutama Linux-põhine Hdinsightiga."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="Blackmist"
    documentationCenter=""
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight-linux"></a>Java MapReduce programmide arendamise Hadoopi Hdinsightiga Linuxi jaoks

See dokumentide juhendab teid Apache Maven abil luua MapReduce rakenduse, siis juurutada ja käivitage see Linux-põhine Hadoopi Hdinsightiga kobar.

##<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

- [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 või uuem versioon (või samaväärne, nt OpenJDK)

- [Apache Maven](http://maven.apache.org/)

- **Azure'i tellimuse**

- **Azure'i CLI**

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="configure-environment-variables"></a>Keskkonna muutujate konfigureerimine

Java ja selle JDK installimisel määrata järgmised keskkonna muutujat. Siiski peaksite, et need on olemas ja sisaldaksid teie süsteemi õigete väärtuste.

* Kuhu on installitud Java runtime keskkond (JRE) osutama **JAVA_HOME** . Näiteks OS X, Unix või Linuxi süsteemis peaks olema umbes väärtus `/usr/lib/jvm/java-7-oracle`. Klõpsake Windowsi oleks väärtus, mis on sarnane`c:\Program Files (x86)\Java\jre1.7`

* **Tee** - peaks sisaldama järgmine tee:

    * **JAVA_HOME** (või samaväärne tee)

    * **JAVA_HOME\bin** (või samaväärne tee)

    * Kuhu on installitud Maven

##<a name="create-a-new-maven-project"></a>Maven uue projekti loomine

1. Terminal seansi või käsurea sisse oma arenduskeskkond, muuta kataloogide asukohta, kuhu soovite salvestada selle projekti.

3. Käsk __mvn__ , mis installitakse koos Maven abil saate luua projekti tellinguid.

        mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    See loob uue kataloogi praegust kausta, millele nimi on määratud __artifactID__ parameetri (käesoleva näite**wordcountjava** .) See kaust sisaldab järgmist:

    * __pom.xml__ - [Projekti objekti mudeli programmiga](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) , mis sisaldab projekti koostamiseks kasutatud andmed ja konfiguratsiooni üksikasjad.

    * __src__ - kataloog, mis sisaldab kataloogi __põhi/java/organisatsiooniskeemi/apache/Hadoopi/näited__ , kus näidatakse Autor rakenduse.

3. Kustutage __src/test/java/org/apache/hadoop/examples/apptest.java__ faili, kui seda ei kasutata selles näites.

##<a name="add-dependencies"></a>Lisage sõltuvused

1. __Pom.xml__ faili redigeerida ja lisada järgmised sees on `<dependencies>` jaotis:

        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-examples</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-client-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>

    See Maven ütleb, et projekt nõuab teekide (loetletud &lt;artifactId\>) kindla versiooniga (loetletud &lt;versioon\>). Kompileerimise ajal laaditakse see vaikimisi Maven hoidla. Saate vaadata rohkem [Maven hoidla otsing](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) .

    Funktsiooni `<scope>provided</scope>` ütleb Maven, et neid järgnevusi peaks olema pakitud rakenduse nagu nad pakutav Hdinsightiga kobar käitusajal.

2. Lisage järgmine __pom.xml__ faili. See peab asuma selle `<project>...</project>` siltide faili; näiteks `</dependencies>` ja `</project>`.

        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
              </configuration>
              <executions>
                <execution>
                  <phase>package</phase>
                    <goals>
                      <goal>shade</goal>
                    </goals>
                </execution>
              </executions>
            </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
               <source>1.7</source>
               <target>1.7</target>
              </configuration>
            </plugin>
          </plugins>
        </build>

    Esimese lisandmooduli konfigureerib [Maven varju lisandmoodul](http://maven.apache.org/plugins/maven-shade-plugin/), mis kasutatakse koostamiseks on uberjar (nimetatakse ka fatjar), mis sisaldab sõltuvuste nõutud rakendus. See välistab ka kattuvat litsentside jar pakett, mis võib põhjustada probleeme mõnes süsteemis sees.

    Teine lisandmoodul konfigureerib Maven koostaja, mille abil saate seada nõutav selle rakendusega kasutada Hdinsightiga kobar versiooni Java versiooni.

3. Salvestage fail __pom.xml__ .

##<a name="create-the-mapreduce-application"></a>MapReduce rakenduse loomine

1. Minge __wordcountjava/src/põhi/java/organisatsiooniskeemi/apache/Hadoopi/näiteid__ kataloogi ja __WordCount.java__ __App.java__ faili ümber nimetada.

2. Avage __WordCount.java__ fail tekstiredaktoris ja asendage sisu järgmist:

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

    Pange tähele, et paketi nimi on **org.apache.hadoop.examples** ja selle tunni nime **WordCount**. Kasutage nende nimed, kui annate MapReduce töö.

3. Salvestage fail.

##<a name="build-the-application"></a>Rakenduse koostamine

1. Muuta __wordcountjava__ kataloogi, kui te ei ole juba olemas.

2. Järgmise käsu abil saate koostada JAR faili, mis sisaldab rakenduse:

        mvn clean package

    See puhastada mis tahes eelmise Koosta esemeid, sõltuvusi, mis on ei ole juba installitud, ja koostamine ja paketti rakenduse alla laadida.

3. Kui käsk lõpetab, sisaldab fail nimega __Wordcountjava-1,0-SNAPSHOT.jar__ __wordcountjava/siht__ kataloogi.

    > [AZURE.NOTE] __Wordcountjava-1,0-SNAPSHOT.jar__ fail on mõni uberjar, mis sisaldab mitte ainult WordCount töö, vaid ka sõltuvusi töö nõuab Runtime.


##<a id="upload"></a>Laadige jar

Järgmise käsu abil Hdinsightiga headnode jar faili üles laadida:

    scp wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

Selle valiku puhul kopeeritakse failid kohalik süsteem pea sõlme.

> [AZURE.NOTE] Kui kasutasite parooli konto SSH, küsitakse teilt, kas soovite parooli. Kui olete kasutanud mõnda SSH võti, peate kasutama funktsiooni `-i` parameetri ja privaatvõti teed. Näiteks `scp -i /path/to/private/key wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

##<a name="run"></a>Töö MapReduce

1. Ühenduse loomine Hdinsightiga SSH abil, nagu on kirjeldatud järgmistes artiklites:

    - [Kasutada SSH Linux-põhine Hadoopi Hdinsightiga Linux, Unix või OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Kasutada SSH Linux-põhine Hadoopi Windows Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Kasutage SSH seansi, MapReduce rakenduse käivitamiseks järgmine käsk:

        yarn jar wordcountjava.jar org.apache.hadoop.examples.WordCount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/wordcountout

    See fail davinci.txt sõnade loendamine WordCount MapReduce rakenduse abil ja tulemid talletada __wasbs: / / / näide/andmete/wordcountout__. Nii Sisestuskeel faili- ja väljundi salvestatakse vaikimisi mäluruumi klaster jaoks.

3. Pärast töö lõpulejõudmist kasutada tulemuste vaatamiseks järgmist:

        hdfs dfs -cat wasbs:///example/data/wordcountout/*

    Peaksite nägema sõnade ja loendab sarnaneb järgmisega väärtustega loendit:

        zeal    1
        zelus   1
        zenith  2

##<a id="nextsteps"></a>Järgmised sammud

Selles dokumendis te teada, kuidas arendada Java MapReduce töö. Vaadake muud võimalused tööta Hdinsightiga järgmised dokumendid.

- [Hdinsightiga taru kasutamine][hdinsight-use-hive]
- [Kasutage siga Hdinsightiga][hdinsight-use-pig]
- [Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)

Lisateabe saamiseks vt ka [Java Arenduskeskus](https://azure.microsoft.com/develop/java/).

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

