<properties
   pageTitle="Analüüsimine ja protsessi JSON dokumente taru rakenduses Hdinsightiga | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada JSON dokumente ja nende abil taru Hdinsightiga sisse."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/22/2015"
   ms.author="rashimg"/>


# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Protsessi ja analüüsida JSON dokumentide taru sisse Hdinsightiga

Saate teada, kuidas töödelda ja analüüsida JSON failide kasutamine rakenduses Hdinsightiga taru. Kasutatakse järgmist JSON dokumendi õpetuses

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

Faili leiate wasbs://processjson@hditutorialdata.blob.core.windows.net/. Azure'i bloobimälu Hdinsightiga kasutamise kohta leiate lisateavet teemast [kasutamine HDFS-ga ühilduva Azure'i bloobimälu koos Hadoopi Hdinsightiga sisse](hdinsight-hadoop-use-blob-storage.md). Kui soovite, saate faili vaikimisi ümbris, klaster kopeerida.

Selles õppetükis saate kasutada taru konsooli.  Juhised taru konsooli avamise kohta leiate artiklist [Kasutamine taru koos Hadoopi Hdinsightiga Kaugtöölaud](hdinsight-hadoop-use-hive-remote-desktop.md).

##<a name="flatten-json-documents"></a>Ühenda JSON-dokumendid

Järgmises jaotises toodud meetodid nõuavad JSON dokumendi ühes reas. Seega peate Ühenda JSON dokumendi string. Kui dokumenti JSON on juba eristamine sõltub, saate selle sammu vahele jätta ja minge otse järgmise jaotise analüüsimine JSON andmete põhjal.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

Töötlemata JSON fail asub **wasbs://processjson@hditutorialdata.blob.core.windows.net/**. Tabeli *StudentsRaw* taru osutab töötlemata un lapik JSON dokument.

Tabeli *StudentsOneLine* taru salvestab andmed Hdinsightiga failisüsteemi jaotises ** /json/õpilased/tee.

Lause INSERT asustada StudentOneLine tabeli lapik JSON andmetega.

SELECT-lause tagastab ainult 1 rida.

Siin on SELECT-lause:

![Lamedamad JSON dokumendi.][image-hdi-hivejson-flatten]

##<a name="analyze-json-documents-in-hive"></a>JSON dokumentide taru analüüs

Taru pakub kolme eri võimaldada päringute sooritamine JSON dokumentidega.

- Kasutage toomine\_JSON\_objekti UDF (kasutaja määratletud funktsioon)
- JSON_TUPLE UDF-i kasutamine
- Kasutage kohandatud SerDe
- Kirjutage UDF Python või muude keelte kasutamine kuulub teile. Lugege [artiklit] [ hdinsight-python] taru koos oma Python koodi käitamise kohta.

### <a name="use-the-getjsonobject-udf"></a>Kasutage toomine\_JSON_OBJECT UDF
Taru pakub sisseehitatud UDF nimega, [saada json objekt](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) , mida saab teha JSON päringu käitamine aja jooksul. See meetod on kaks argumenti – tabeli nimi ja meetodi nime, mis on lapik JSON dokumendi ja JSON välja, mis tuleb sõeluda. Vaatame näiteks teada, kuidas see UDF töötab.

Eesnimi ja perekonnanimi iga õpilase jaoks

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Siin on selle päringu käivitamisel konsooli aknas.

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

On mõned piirangud get-json_object UDF.

- Kuna iga välja päringu jaoks on vaja uuesti sõelumine päringu, mõjutab see jõudlust.
- SAADA\_JSON_OBJECT() tagastab stringi esitus massiivi. Teisendada taru massiivile, on teil asendada nurksulgude regulaaravaldised abil "[" ja "]" ja seejärel ka kõne tükeldatud saada massiiv.


Sellepärast, et taru viki soovitab json_tuple abil.  

### <a name="use-the-jsontuple-udf"></a>JSON_TUPLE UDF-i kasutamine

Teise UDF esitatud taru nimetatakse [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple) täidab [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object)paremini. See meetod võtab võtmed ja JSON-stringi ja annab vastuseks väärtused, mis on ühe funktsiooniga korteeži. Järgmine päring tagastab kool id "ja" klassi JSON dokumendist:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

See skript taru konsooli väljund:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

JSON\_KORTEEŽI kasutab [külgmine vaade](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) süntaks taru, mis võimaldab json\_korteeži rakendades funktsiooni UDT algse tabeli iga rea virtuaalse tabeli loomiseks.  Keerukate JSONs muutuvad liiga kohmakas tõttu KÜLGMINE vaade korduvalt kasutada. Lisaks JSON_TUPLE ei oska pesastatud JSONs.


###<a name="use-custom-serde"></a>Kasutage kohandatud SerDe

SerDe on parim valik sõelumine pesastatud JSON dokumendid, võimaldab määratleda JSON-skeemi ja kasutage skeemiga sõeluda dokumendid. Selles õpetuses te kasutate ühte populaarsemaks SerDe, mis on välja töötatud [rcongiu](https://github.com/rcongiu).

**Kohandatud SerDe kasutamiseks tehke järgmist.**

1. Installige [Java SE arengu Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). Valige soovitud JDK X64 Windowsi versiooni kui kavatsete kasutada Windows kasutuselevõtu Hdinsightiga

    >[AZURE.WARNING] See SerDe JDK 1.8 ei tööta.

    Kui installimine on lõpule jõudnud, lisage uus kasutaja keskkonnas muutujana.

    1. Avage Windows kuval **Kuva täpsemad sätted süsteem** .
    2. Klõpsake **keskkonna muutujate**.  
    3. Saate lisada uue **JAVA_HOME** keskkonna muutujana osutab **C:\Program Files\Java\jdk1.7.0_55** või kus teie JDK on installitud.

    ![Õige config väärtused JDK seadistamine][image-hdi-hivejson-jdk]

2. Installige [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)

    Kausta Prügikast minnes juhtelemendi lisamine oma tee paani--> Redigeeri oma konto Environment muutujate süsteemi muutujaid. Pildil näidatakse, kuidas seda teha.

    ![Kuidas häälestada Maven][image-hdi-hivejson-maven]

3. Klooni projekti [Taru-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github saidi kaudu. Selleks klõpsake nupu "Laadi alla Zip", nagu on näidatud alloleval pildil.

    ![Projekti kloonimine][image-hdi-hivejson-serde]

4: avage kaust, kuhu allalaaditud selle paketi ja tüüp "mvn paketi". See peaks luua vajalikud jar failid, mida saate kopeerida üle klaster.

5: minge sihtkaust jaotises Kui laadisite paketi juurkausta. Json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar faili üleslaadimiseks juht-sõlm klaster. Ma tavaliselt pannakse taru kahendarvu kaustas: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin või midagi sarnast.

6: taru vastava viiba kuvamisel tippige "lisa jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar". Kuna minu puhul on jar C:\apps\dist\hive-0.13.x\bin kausta, saate otse lisada purk nime nagu allpool näidatud:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

    ![Adding JAR to your project][image-hdi-hivejson-addjar]

Nüüd olete valmis päringuid käivitada vastu JSON dokument on SerDe abil.

Järgmise tabeli määratletud skeemi loomine

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

Eesnimi ja perekonnanimi õpilase

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Siin ongi tulemus taru konsooli.

![SerDe päringu 1][image-hdi-hivejson-serde_query1]

Hulgaliselt JSON dokumendi summa arvutamiseks

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

Ülaltoodud päring kasutab [külgmine vaade tükeldamine](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF laiendada, et nad saavad summeeritakse hinded massiiv.

Siin on taru konsooli.

![SerDe päringu 2][image-hdi-hivejson-serde_query2]

Leidmiseks, mis üliõpilane on rohkem kui 80 võrra poolitusjoonega teemade valimine  
      muusikat StudentClassCollection.ClassId kaudu json_table jt külgmine vaade tükeldamine (muusikat Kui tulemus StudentClassCollection.Score) saidikogumi kus Keskmine > 80;

Ülaltoodud päring tagastab taru massiiv erinevalt toomine\_json\_objekti, mis tagastab stringi.

![SerDe päringu 3][image-hdi-hivejson-serde_query3]

Kui soovite skil moonutatud JSON, seejärel, nagu on selgitatud [vikilehe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) selle SerDe võite saavutada mis, tippides alljärgnev kood:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




##<a name="summary"></a>Kokkuvõte
Kokkuvõttes JSON operaator taru, mille valimisel tüüpi oleneb stsenaariumist. Kui teil on lihtne JSON dokumendi ja teil on ainult üks väli-otsimiseks saate kasutada taru UDF-i hankimine\_json\_objekti. Kui teil on rohkem kui üks klahvid otsimiseks saate kasutada json_tuple. Kui teil on pesastatud dokumendi, siis kasutage JSON SerDe.

Vaadake teisi seotud artiklid

- [Kasutage taru ja HiveQL Hadoopi sisse Hdinsightiga Apache log4j näidisfaili analüüsimiseks](hdinsight-use-hive.md)
- [Abil rakenduses Hdinsightiga taru flight viivitus andmete analüüsiks](hdinsight-analyze-flight-delay-data.md)
- [Abil rakenduses Hdinsightiga taru Twitteri andmete analüüsiks](hdinsight-analyze-twitter-data.md)
- [Hadoopi töö DocumentDB ja Hdinsightiga abil](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
