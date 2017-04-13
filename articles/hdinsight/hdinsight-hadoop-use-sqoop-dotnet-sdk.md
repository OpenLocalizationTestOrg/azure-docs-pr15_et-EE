<properties
    pageTitle="Hadoopi Sqoop kasutamine Hdinsightiga | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Hdinsightiga .NET SDK käivitamiseks Sqoop importimine ja eksportimine mõnda Hadoopi kobar ja Azure SQL-andmebaasi vahel."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
   ms.date="09/14/2016"
    ms.author="jgao"/>

#<a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>Kasutades .NET SDK Hadoopi sisse Hdinsightiga Sqoop tööde

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Saate teada, kuidas kasutada Hdinsightiga .NET SDK Hdinsightiga importimiseks ja eksportimiseks Hdinsightiga kobar ja Azure SQL-andmebaasi või SQL serveri andmebaasi vahelise Sqoop tööde käitamine.

> [AZURE.NOTE] Selles artiklis toodud juhiseid saab kasutada kas Windowsi-põhiste või Linuxi-põhine Hdinsightiga klaster; neid juhiseid vaid töötab Windows kliendi. Menüü Vaateselektori peal artiklit abil saate valida ka muid võimalusi.

###<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

- **A Hadoopi kobar Hdinsightiga sisse**. Lugege teemat [loomine kobar ja SQL-andmebaasi](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="run-sqoop-using-net-sdk"></a>Kasutades .NET SDK Sqoop käivitamine

Hdinsightiga .NET SDK pakub .net-i kliendi teegid, mis hõlbustab tööta Hdinsightiga kogumite .net-i kaudu. Selles jaotises loote C# konsooli rakendus on hivesampletable õpetustes varasemas versioonis loodud SQL-andmebaasi tabeli eksportida.

**Esitada Sqoop töö**

1. Luua Visual Studio C# konsooli rakendus.
2. Visual Studio Package Manager konsooli, järgmine käsk Nugeti paketi importida.

        Install-Package Microsoft.Azure.Management.HDInsight.Job
        
3. Kasutage Program.cs faili järgmine kood:

        using System.Collections.Generic;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
        
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
        
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
        
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
        
                    SubmitSqoopJob();
        
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
        
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
        
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
        
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
        
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
        
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
        
4. Vajutage klahvi **F5** programmi käivitamiseks. 

##<a name="limitations"></a>Piirangud

* Hulgi ekspordi - ja Linux-põhine Hdinsightiga, kasutatakse Microsoft SQL Server või Azure'i SQL-andmebaasi andmete eksportimine Sqoop konnektor ei toeta praegu hulgi lisab.

* Partiide - ja Linux-põhine Hdinsightiga, kasutades funktsiooni `-batch` vahetada, kui lisatakse, Sqoop täidab mitme lisab asemel partiide lisa toimingud.

##<a name="next-steps"></a>Järgmised sammud

Nüüd olete õppinud, kuidas kasutada Sqoop. Lisateavet leiate järgmistest teemadest.

- [Kasutage Oozie koos Hdinsightiga](hdinsight-use-oozie.md): kasutamine Sqoop toimingu Oozie töövoos.
- [Analüüsi viivitus lennuandmetega abil Hdinsightiga](hdinsight-analyze-flight-delay-data.md): flight analüüsimiseks kasutada taru viivitamine andmed ja Azure SQL-andmebaasi andmete eksportimine Sqoop abil.
- [Hdinsightiga andmed üles laadida](hdinsight-upload-data.md): Otsige muid viise andmete üleslaadimiseks Hdinsightiga/Azure'i bloobimälu.


