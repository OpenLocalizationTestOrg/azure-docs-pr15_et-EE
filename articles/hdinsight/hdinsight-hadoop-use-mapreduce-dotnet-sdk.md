<properties
    pageTitle="Edastab MapReduce tööd Hdinsightiga .NET SDK abil | Microsoft Azure'i"
    description="Saate teada, kuidas edastab MapReduce tööd Azure Hdinsightiga Hadoopi Hdinsightiga .NET SDK abil."
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
   ms.date="10/28/2016"
    ms.author="jgao"/>

# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>MapReduce tööde Hdinsightiga .NET SDK abil

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Saate teada, kuidas esitada MapReduce töid Hdinsightiga .NET SDK abil. Hdinsightiga kogumite tulla jar fail on teatud MapReduce näidised. Jar fail on */example/jars/hadoop-mapreduce-examples.jar*.  Üks on *wordcount*. Arendate C# konsooli rakendus wordcount töö esitada.  Töö loeb */example/data/gutenberg/davinci.txt* faili, ja väljundi */example/data/davinciwordcount*tulemid.  Kui soovite, käivitage rakendus uuesti, peab puhastamiseks väljund kausta.

> [AZURE.NOTE] Selles artiklis toodud juhiseid tuleb teha Windows kliendi. Kasutamine Linux, OS X või Unix kliendi töötamiseks taru kohta lisateabe saamiseks kasutage menüü Vaateselektori, vt artiklit peal.

##<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles artiklis, peab teil olema järgmised:

- **A Hadoopi kobar Hdinsightiga sisse**. Vt [Linux-põhine Hadoopi rakenduses Hdinsightiga kasutamise alustamine](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Visual Studio 2012/2013/2015**.

##<a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>Edastab MapReduce tööd Hdinsightiga .NET SDK abil

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

                    SubmitMRJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };

                    var paras = new MapReduceJobSubmissionParameters
                    {
                        //Content = "wordcount  ",
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the MR job to the cluster...");
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

- Luua klaster ja esitamine taru töö leiate artiklist [alustamine Windows Azure Hdinsightiga](hdinsight-hadoop-linux-tutorial-get-started.md).
- Luua Hdinsightiga kogumite, leiate teemast [loomine Linux-põhine Hadoopi kogumite Hdinsightiga sisse](hdinsight-hadoop-provision-linux-clusters.md).
- Haldamise Hdinsightiga kogumite, leiate teemast [haldamine Hadoopi kogumite Hdinsightiga sisse](hdinsight-administer-use-management-portal.md).
- Õppekeskuse Hdinsightiga .NET SDK, vt [Hdinsightiga .NET SDK viide](https://msdn.microsoft.com/library/mt271028.aspx).
- Jaoks-interaktiivne Azure autentida, vt [autentimist mitte-interaktiivne .NET Hdinsightiga rakenduste loomine](hdinsight-create-non-interactive-authentication-dotnet-applications.md).




