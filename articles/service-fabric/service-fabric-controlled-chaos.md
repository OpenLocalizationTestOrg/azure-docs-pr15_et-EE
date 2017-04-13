<properties
   pageTitle="Põhjustada kaose teenuse struktuuri rühmades | Microsoft Azure'i"
   description="Viga juhtimise ja kobar analüüsi teenuse API abil saate hallata kaose klaster."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="rsinha"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="motanv"/>

# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Hallatud kaose teenuse struktuuri rühmades esile
Suuremahuliste jaotatud süsteemide nagu cloud infrastruktuuri on potentsiaalselt usaldusväärsed. Azure teenuse struktuuri võimaldab arendajatel kirjutada usaldusväärsed teenused usaldusväärsed infrastruktuur peal. Robustne teenuste kirjutamiseks arendajad peavad saama esile vead sellise usaldusväärsed taristu testida oma teenuste stabiilsuse suhtes.

Viga süsti ja kobar Analysis Service (tuntud ka kui viga analüüsi teenus) võimaldab arendajate esile viga toimingud testimiseks teenused. Siiski suunatud jäljendatud vead saate ainult seni. Võtta katsetamine põhjalikumaks, saate kasutada kaose.

Kaose jäljendab pidev, üksteise vead (graatsiline ja ungraceful) kogu klaster laiendatud aja jooksul. Kui olete kaose määr ja millist vead, saate alustada või peatada C# API-või PowerShelli luua vead klaster ja teenust.

Kaose töötamise annab tulemiks eri sündmusi, mis jäädvustada Käivita hetkel olek. Näiteks sisaldab mõnda ExecutingFaultsEvent täidetakse selle iteratsiooni vead. Mõne ValidationFailedEvent sisaldab tõrge, et kobar valideerimise käigus leiti üksikasju. Saate kasutada GetChaosReportAsync API kaose käivitatakse aruande loomiseks.

## <a name="faults-induced-in-chaos"></a>Vead esile kaose
Kaose genereeritud vead üle kogu teenuse struktuuri kobar ja tihendab vead, kus on näha kuude või aastate sisse mõne tunni pärast. Üksteise vead kõrge viga määr kombinatsiooni leiab nurgas juhtudel, mis on teisiti vastamata. Selle kaose ülesanne põhjustab teenuse kood kvaliteeti märkimisväärselt täiustada.

Kaose põhjustab vead järgmised Kategooriad:

 - Taaskäivitage sõlm
 - Taaskäivitage juurutatud kood pakett
 - Koopia eemaldamine
 - Taaskäivitage koopia
 - Peamine koopia, mis (konfigureeritav) liikumine
 - Teisaldamine teise koopia (konfigureeritav)

Mitme iteratsiooni töötab kaose. Iga iteratsiooni koosneb vead ja kobar valideerimine määratud perioodi kohta. Saate konfigureerida kobar tasakaalustamiseks ja valideerimine õnnestub ajakulu. Kui tõrke on leitud kobar valideerimine, kaose genereeritud ja ei lahene UTC ajatempli ja tõrke üksikasjade lisamine ValidationFailedEvent.

Oletame näiteks, näiteks kaose, mis tund kuni kolm samaaegseid vead käivitamine on häälestatud. Kaose põhjustab kolm vead ja seejärel kinnitab kobar seisund. Seni, kuni eelmise edastab toimingut seni, kuni on konkreetselt peatatud StopChaosAsync API kaudu või üks tund. Kui klaster muutub vigane klõpsake mis tahes iteratsiooni (st see ei stabiliseeri konfigureeritud aja jooksul), kaose genereeritud on ValidationFailedEvent. Sel juhul näitab, et midagi on valesti läinud ja võib-olla lähemalt uurida.

Sellisel kujul, põhjustab kaose ainult turvalised vead. See tähendab, et välise vead puudumisel kvoorumi kaotsimineku või andmekao kunagi ei tekiks.

## <a name="important-configuration-options"></a>Oluliste konfiguratsiooni suvandid
 - **TimeToRun**: kokku kord selle kaose käivitatakse enne selle lõppemist edu. Enne käivitamist TimeToRun perioodi StopChaos API kaudu saate peatada kaose.
 - **MaxClusterStabilizationTimeout**: on maksimaalne aeg ootama, enne selle uuesti kontrollimist saada terve kobar. See ootamine on vähendada klaster samal ajal, kui see on taastamisel. Kontrolle läbi on.
    - Kui kobar seisund on OK
    - Kui teenuse seisund on OK
    - Kui target koopia määratud suuruse saavutamiseni teenuse sektsiooni jaoks
    - Et InBuild koopiad pole olemas
 - **MaxConcurrentFaults**: samaaegseid vead, mis on esile iga iteratsiooni maksimaalne arv. On suurem arv, mida tõhusamaks kaose. Tulemuseks keerukamaid failovers ja ülemineku kombinatsioone. Kaose tagab, et välise vead puudumisel ei ole kvoorumi kaotsimineku või andmete kaotsimineku, olenemata sellest, kuidas kõrge väärtus on selle konfiguratsiooni.
 - **EnableMoveReplicaFaults**: lubamine või keelamine esmane või teisene koopiad liikumiseks põhjustavad vigu. Vaikimisi on keelatud need vead.
 - **WaitTimeBetweenIterations**: aja ootama vahel iteratsioone, mis on pärast ringi vead ja vastavate valideerimine.
 - **WaitTimeBetweenFaults**: aja ootama kaks järjestikust vead iteratsiooni vahel.

## <a name="how-to-run-chaos"></a>Kuidas käivitada kaose
**C#:**

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```
**PowerShelli:**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
