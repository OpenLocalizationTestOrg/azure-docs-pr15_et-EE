<properties 
    pageTitle="Viivitus lennuandmetega taru Linux-põhine Hdinsightiga kohta koos analüüsida | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada taru flight Hdinsightiga Linuxi-põhiste andmete analüüsimine ja seejärel andmed eksportida SQL-andmebaasi Sqoop abil." 
    services="hdinsight" 
    documentationCenter="" 
    authors="Blackmist" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="larryfr"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Abil rakenduses Hdinsightiga taru flight viivitus andmete analüüsiks

Saate teada, kuidas analüüsida viivitus lennuandmetega taru kasutamine Linux-põhine Hdinsightiga seejärel Azure'i SQL-andmebaasi kasutatakse Sqoop andmed eksportida.

> [AZURE.NOTE] Üksikute osade seda dokumenti saate kasutada Windowsi-põhiste Hdinsightiga kogumite (Python ja taru näiteks), on palju juhiseid teatud kogumite Linux-põhine. Mis töötab koos Windowsi-põhiste kobar juhised leiate teemast [analüüsi viivitus lennuandmetega taru rakenduses Hdinsightiga abil](hdinsight-analyze-flight-delay-data.md)

###<a name="prerequisites"></a>Eeltingimused

Enne alustamist selle õpetuse, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __An Hdinsightiga kobar__. Juhised luua uue Linuxi-põhiste Hdinsightiga klaster teemast [Hadoopi taru rakenduses Hdinsightiga Linux koos kasutamise alustamine](hdinsight-hadoop-linux-tutorial-get-started.md) .

- __Azure'i SQL-andmebaas__. Kasutage Azure'i SQL-andmebaasiga sihtkoha andmete säilitamiseks. Kui teil on SQL-andmebaasi juba, lugege teemat [SQL-andmebaasi õpetus: SQL-i andmebaasi loomine minutiga](../sql-database/sql-database-get-started.md).

- __Azure'i CLI__. Kui te pole Azure'i CLI installinud, vt [installida ja konfigureerida Azure CLI](../xplat-cli-install.md) lisajuhiseid.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]


##<a name="download-the-flight-data"></a>Flight andmete allalaadimine

1. Liikuge sirvides [uurimistöö ja uuendusliku tehnoloogiaga Administreerimine Bureau transport statistika][rita-website].
2. Klõpsake lehel Valige järgmised väärtused:

  	| Nimi | Väärtus |
  	| ---- | ---- |
  	| Filter aasta | 2013 |
  	| Filtreerimine perioodi kohta. | Jaanuar |
  	| Väljad | Aasta, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. Tühjendage kõik muud väljad |

3. Klõpsake nuppu **Laadi alla**. 

##<a name="upload-the-data"></a>Andmete üleslaadimine

1. Järgmise käsu abil saate zip-faili üleslaadimine Hdinsightiga kobar pea sõlme:

        scp FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Asendage zip-faili nimi __faili nimi__ . Asendage Hdinsightiga kobar SSH kasutajanimi __kasutajanimi__ . Asendage CLUSTERNAME Hdinsightiga kobar nime.
    
    > [AZURE.NOTE] Kui kasutate oma SSH login autentida parooli, küsitakse teilt, kas soovite parooli. Kui kasutasite avalik võti, peate kasutama funktsiooni `-i` parameeter ja määrake kattuvad privaatvõti teed. Näiteks `scp -i ~/.ssh/id_rsa FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Kui üleslaadimine on lõppenud, ühendada klaster SSH abil.

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Lisateavet kasutades SSH Linux-põhine Hdinsightiga leiate järgmistest artiklitest:
    
    * [Kasutada SSH Linux-põhine Hadoopi Hdinsightiga Linux, Unix või OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Kasutada SSH Linux-põhine Hadoopi Windows Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows.md)
    
3. Kui ühendus on loodud, kasutage järgmist pakkige see lahti, ZIP-faili:

        unzip FILENAME.zip
    
    See väljavõte CSV-faili, mis on umbes 60MB suurus.
    
4. Järgmine abil saate luua uue kausta WASB (jaotatud andmesalve kasutatavaid Hdinsightiga) ja kopeerige fail.

    hdfs dfs - mkdir -p /tutorials/flightdelays/data hdfs dfs – sellele FILENAME.csv/õpetused/flightdelays/andmete /
    
##<a name="create-and-run-the-hiveql"></a>Loomine ja käivitamine on HiveQL

Järgmiste juhiste abil saate andmete importimine CSV-fail nimega __viivitused__taru tabelisse.

1. Luua ja redigeerida uus fail nimega __flightdelays.hql__kasutage järgmist:

        nano flightdelays.hql
        
    Kasutage seda faili sisu järgmine:
    
        DROP TABLE delays_raw;
        -- Creates an external table over the csv file
        CREATE EXTERNAL TABLE delays_raw (
            YEAR string,
            FL_DATE string,
            UNIQUE_CARRIER string,
            CARRIER string,
            FL_NUM string,
            ORIGIN_AIRPORT_ID string,
            ORIGIN string,
            ORIGIN_CITY_NAME string,
            ORIGIN_CITY_NAME_TEMP string,
            ORIGIN_STATE_ABR string,
            DEST_AIRPORT_ID string,
            DEST string,
            DEST_CITY_NAME string,
            DEST_CITY_NAME_TEMP string,
            DEST_STATE_ABR string,
            DEP_DELAY_NEW float,
            ARR_DELAY_NEW float,
            CARRIER_DELAY float,
            WEATHER_DELAY float,
            NAS_DELAY float,
            SECURITY_DELAY float,
            LATE_AIRCRAFT_DELAY float)
        -- The following lines describe the format and location of the file
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE
        LOCATION '/tutorials/flightdelays/data';
        
        -- Drop the delays table if it exists
        DROP TABLE delays;
        -- Create the delays table and populate it with data
        -- pulled in from the CSV file (via the external table defined previously)
        CREATE TABLE delays AS
        SELECT YEAR AS year,
            FL_DATE AS flight_date,
            substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
            substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
            substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
            ORIGIN_AIRPORT_ID AS origin_airport_id,
            substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
            substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
            substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
            DEST_AIRPORT_ID AS dest_airport_id,
            substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
            substring(DEST_CITY_NAME,2) AS dest_city_name,
            substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
            DEP_DELAY_NEW AS dep_delay_new,
            ARR_DELAY_NEW AS arr_delay_new,
            CARRIER_DELAY AS carrier_delay,
            WEATHER_DELAY AS weather_delay,
            NAS_DELAY AS nas_delay,
            SECURITY_DELAY AS security_delay,
            LATE_AIRCRAFT_DELAY AS late_aircraft_delay
        FROM delays_raw;
        
2. Faili salvestamiseks kasutage __klahvikombinatsiooni Ctrl + X__ja seejärel __Y__ .

3. __Flightdelays.hql__ faili käivitada taru ja kasutage järgmist:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -f flightdelays.hql
        
    > [AZURE.NOTE] Selles näites `localhost` kasutatakse, kuna olete loonud ühenduse Hdinsightiga kobar, mis on, kus töötab HiveServer2 pea sõlme.

4. Kasutage interaktiivse Linnulennutee seansi avamine järgmine käsk:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

5. Kui teile on `jdbc:hive2://localhost:10001/>` Küsi, kasutage järgmisi andmete toomiseks imporditud lennuandmetega viivitust.

        INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        SELECT regexp_replace(origin_city_name, '''', ''),
            avg(weather_delay)
        FROM delays
        WHERE weather_delay IS NOT NULL
        GROUP BY origin_city_name;

    See linna, et kogenud ilm viivitusi, Keskmine viivitusaeg, koos loendi toomiseks ja salvestage see `/tutorials/flightdelays/output`. Hiljem Sqoop andmeid lugeda, selle asukoha ja eksportimine Azure'i SQL-andmebaasi.

6. Sisestage Linnulennutee väljumiseks `!quit` kaudu.

## <a name="create-a-sql-database"></a>SQL-i andmebaasi loomine

Kui teil on juba SQL-andmebaasiga, saate serveri nime. Selle leidmiseks [Azure portaali](https://portal.azure.com) , valides __SQL andmebaase__, ja seejärel filtreerimine nime andmebaas, mida soovite kasutada. Serveri nimi on kirjas veerus __SERVER__ .

Kui teil on juba SQL-andmebaasiga, kasutage teave [SQL-andmebaasi õpetus: SQL-i andmebaasi loomine minutiga](../sql-database/sql-database-get-started.md) loomist. Peate salvestamiseks kasutatud andmebaasi serveri nimi.

##<a name="create-a-sql-database-table"></a>SQL-andmebaasi tabeli loomine

> [AZURE.NOTE] Seal on palju võimalusi tabeli loomiseks SQL-i andmebaasiga ühenduse loomiseks. Järgmiste juhiste kasutamiseks [FreeTDS](http://www.freetds.org/) Hdinsightiga kobar.

1. Ühenduse Linux-põhine Hdinsightiga kobar SSH abil ja käivitage järgmised toimingud SSH seanss.

3. Järgmise käsu abil saate installida FreeTDS:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Kui FreeTDS on installitud, kasutage järgmist käsku SQL-i andmebaasiserveriga ühenduse loomiseks. Asendage __serverinimi__ SQL-andmebaasi serveri nimi. Asendage __adminLogin__ ja __adminPassword__ login SQL-i andmebaasi. Asendage __databaseName__ andmebaasi nimi.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>

    Saate väljundi umbes järgmine:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Juures olevat `1>` Küsi, sisestage järgmised read:

        CREATE TABLE [dbo].[delays](
        [origin_city_name] [nvarchar](50) NOT NULL,
        [weather_delay] float,
        CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
        ([origin_city_name] ASC))
        GO

    Kui soovitud `GO` esitatakse, eelmise laused hinnatakse. See loob uue tabeli nimega __viivitused__, rühmitatud indeks (nõutav SQL-andmebaasi).

    Veenduge, et tabel on loodud kasutada järgmist:

        SELECT * FROM information_schema.tables
        GO

    Peaksite nägema väljundi umbes järgmine:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        databaseName       dbo     delays      BASE TABLE

8. Sisestage `exit` juures olevat `1>` kiiret tsql kasuliku väljumiseks.
    
##<a name="export-data-with-sqoop"></a>Sqoop andmete eksportimine

2. Veenduge, et Sqoop näevad SQL-andmebaasi järgmise käsu abil:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    See peaks tagastama andmebaaside, sh viivitused tabeli varem loodud andmebaasi loendit.

3. Kasutage andmete eksportimine hivesampletable mobiledata tabelisse järgmine käsk:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir 'wasbs:///tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1

    See teeb Sqoop ühenduse SQL-andmebaasiga, viivitused tabeli, mis sisaldab andmebaasi ja andmete eksportimine on wasbs: / / / õpetused/flightdelays/väljundi (kus meil salvestatud taru päringu varasemas versioonis, väljundi) viivitused tabelisse.

4. Pärast käsu on lõpule jõudnud, kasutades TSQL andmebaasiga ühenduse loomiseks järgmist kasutamine

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Kui ühendus on loodud, kasutage kinnitamaks, et andmed on eksporditud mobiledata tabelisse järgmistest:
    
        SELECT * FROM delays
        GO

    Peaksite nägema loetletakse tabelis andmeid. Tippige `exit` tsql kasuliku sulgemiseks.

##<a id="nextsteps"></a>Järgmised sammud

Nüüd aru Azure'i bloobimälu faili üleslaadimine, kuidas asustamiseks taru tabeli andmed Azure'i bloobimälu abil, käivitamise taru päringute ja andmete eksportimine HDFS Azure SQL-andmebaasi Sqoop kasutamise kohta. Lisateabe saamiseks lugege järgmisi artikleid:

* [Hdinsightiga töötamise alustamine][hdinsight-get-started]
* [Hdinsightiga taru kasutamine][hdinsight-use-hive]
* [Hdinsightiga Oozie kasutamine][hdinsight-use-oozie]
* [Hdinsightiga Sqoop kasutamine][hdinsight-use-sqoop]
* [Kasutage siga Hdinsightiga][hdinsight-use-pig]
* [Töötada Java MapReduce programmide Hdinsightiga][hdinsight-develop-mapreduce]
* [Arendamise Python Hadoopi streaming programmide Hdinsightiga][hdinsight-develop-streaming]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx


 
