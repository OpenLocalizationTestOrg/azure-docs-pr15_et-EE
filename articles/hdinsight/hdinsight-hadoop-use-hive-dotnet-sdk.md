<properties
    pageTitle="Käivitage taru päringute abil Hdinsightiga .NET SDK | Microsoft Azure'i"
    description="Saate teada, kuidas edastab Hadoopi tööd Azure Hdinsightiga Hadoopi Hdinsightiga .NET SDK abil."
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

# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>Kasutades Hdinsightiga .NET SDK taru päringute sooritamine

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


Saate teada, kuidas taru päringute abil Hdinsightiga .NET SDK edastada.

> [AZURE.NOTE] Selles artiklis toodud juhiseid tuleb teha Windows kliendi. Kasutamine Linux, OS X või Unix kliendi töötamiseks taru kohta lisateabe saamiseks kasutage menüü Vaateselektori, vt artiklit peal.

##<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles artiklis, peab teil olema järgmised:

- **A Hadoopi kobar Hdinsightiga sisse**. Vt [Linux-põhine Hadoopi rakenduses Hdinsightiga kasutamise alustamine](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Visual Studio 2012/2013/2015**.

##<a name="submit-hive-queries-using-hdinsight-net-sdk"></a>Esitage taru päringute Hdinsightiga .NET SDK abil

Hdinsightiga .NET SDK pakub .net-i kliendi teegid, mis hõlbustab tööta Hdinsightiga kogumite .net-i kaudu. 

**Esitada tööde haldamine**

1. Luua Visual Studio C# konsooli rakendus.
2. Nugeti Package Manager konsool, käivitage järgmine käsk.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

2. Kasutage järgmine kood:

        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
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

                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitHiveJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "ravi" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);

                    System.Console.WriteLine("Waiting for the job completion ...");

                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }

                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure

                    System.Console.WriteLine("Job output is: ");

                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }

5. Vajutage klahvi **F5** käivitage rakendus.


## <a name="next-steps"></a>Järgmised sammud

Selles artiklis on õppinud loomiseks Hdinsightiga kobar on mitu võimalust. Lisateabe saamiseks lugege järgmisi artikleid:

* [Alustamine Windows Azure Hdinsightiga][hdinsight-get-started]
* [Loomine Hadoopi kogumite Hdinsightiga][hdinsight-provision]
* [Hadoopi kogumite rakenduses Hdinsightiga abil Azure portaali haldamine](hdinsight-administer-use-management-portal.md)
* [Hdinsightiga .NET SDK viide](https://msdn.microsoft.com/library/mt271028.aspx)
* [Kasutage siga Hdinsightiga](hdinsight-use-pig.md)
* [Hdinsightiga Sqoop kasutamine](hdinsight-use-sqoop-mac-linux.md)
* [Interaktiivne autentimine .NET Hdinsightiga rakenduste loomine](hdinsight-create-non-interactive-authentication-dotnet-applications.md)


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md


