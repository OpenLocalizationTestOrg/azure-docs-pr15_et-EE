<properties
   pageTitle="Hadoopi siga kasutamine .NET Hdinsightiga | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada .NET SDK jaoks Hadoopi siga tööde Hadoopi Hdinsightiga edastamiseks."
   services="hdinsight"
   documentationCenter=".net"
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Kasutades .NET SDK Hadoopi rakenduses Hdinsightiga siga tööde

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Selles dokumendis on toodud siga tööde Hadoopi Hdinsightiga kobar edastamiseks .NET SDK Hadoopi jaoks kasutamise näide.

Hdinsightiga .NET SDK pakub .net-i kliendi teegid, mis hõlbustab tööta Hdinsightiga kogumite .net-i kaudu. Siga võimaldab teil luua MapReduce toimingute sarja andmete teisendused modelleerimine. Saate teada, kuidas kasutada põhilisi C# rakendus siga töö, mida soovite mõne Hdinsightiga kobar edastada.

## <a name="prerequisites"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist.

* Mõne Windows Azure Hdinsightiga (Hadoopi Hdinsightiga) kobar (kas Windowsi või Linuxi-põhine).
* Visual Studio 2012 või 2013 või 2015.

## <a name="create-the-application"></a>Rakenduse loomine

Hdinsightiga .NET SDK pakub .net-i kliendi teegid, mis hõlbustab tööta Hdinsightiga kogumite .net-i kaudu. 


1. Avage Visual Studio 2012 või 2013
2. Klõpsake menüü **fail** käsk **Uus** ja seejärel valige **projekti**.
3. Uue projekti jaoks tippige või valige järgmised väärtused.

    <table>
    <tr>
    <th>Atribuut</th>
    <th>Väärtus</th>
    </tr>
    <tr>
    <th>Kategooria</th>
    <th>Mallid/Visual C# / Windows</th>
    </tr>
    <tr>
    <th>Mall</th>
    <th>Konsooli rakendus</th>
    </tr>
    <tr>
    <th>Nimi</th>
    <th>SubmitPigJob</th>
    </tr>
    </table>
4. Klõpsake nuppu **OK** , et luua projekt.
5. Klõpsake menüü **Tööriistad** valige **Teegi Package Manager** või **Nugeti Package Manager**ja valige **Package Manager konsooli**.
6. Konsooli .NET SDK pakettide installimiseks käivitage järgmine käsk.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

7. Topeltklõpsake Solution Exploreris **Program.cs** selle avamiseks. Asendage olemasolevat järgmist.

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

                    SubmitPigJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitPigJob()
                {
                    var parameters = new PigJobSubmissionParameters
                    {
                        Query = @"LOGS = LOAD 'wasbs:///example/data/sample.log';
                                    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                    RESULT = order FREQUENCIES by COUNT desc;
                                    DUMP RESULT;"
                    };

                    System.Console.WriteLine("Submitting the Pig job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }


7. Vajutage klahvi **F5** rakenduse käivitamiseks.
8. Vajutage klahvi **ENTER** väljuda rakendus.

## <a name="summary"></a>Kokkuvõte

Nagu näete, .NET SDK Hadoopi jaoks võimaldab teil luua .net-i rakendusi, mis edastab siga tööd on Hdinsightiga kobar ja töö oleku jälgimine.

## <a name="next-steps"></a>Järgmised sammud

Üldist teavet siga Hdinsightiga.

* [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)

Teavet teiste mooduste kohta, saate töötada Hadoopi Hdinsightiga.

* [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

* [Kasutage MapReduce koos Hadoopi Hdinsightiga](hdinsight-use-mapreduce.md) [eelvaade portaali]: https://portal.azure.com/
