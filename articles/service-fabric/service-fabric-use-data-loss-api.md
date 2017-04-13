<properties
   pageTitle="Kuidas rakendada andmete kaotsimineku teenuse struktuuri Services | Microsoft Azure'i"
   description="Kirjeldab, kuidas kasutada andmete kaotsimineku api"
   services="service-fabric"
   documentationCenter=".net"
   authors="LMWF"
   manager="rsinha"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="lemai"/>
   
# <a name="how-to-invoke-data-loss-on-services"></a>Kuidas rakendada andmete kaotsimineku Services

>[AZURE.WARNING] Selles dokumendis kirjeldatakse, kuidas põhjustada andmekadu teenuste ja tuleks kasutada hoolikalt.

## <a name="introduction"></a>Sissejuhatus
Saate kasutada andmete kaotsimineku sektsiooni oma struktuuri teenus, helistades StartPartitionDataLossAsync().  See api kasutab viga süsti ja analüüsi teenuse põhjustada andmete kaotsimineku korral tööd teha.

## <a name="using-the-fault-injection-and-analysis-service"></a>Viga süsti ja analüüsi teenuse abil

Viga süsti ja analüüsi teenuse toetab praegu järgmised API-de allolevat tabelit.  Diagrammi paremas servas kuvatakse vastava PowerShelli cmdlet-käsk.  MSDN-i dokumentatsiooni iga API iga kohta lisateabe saamiseks vaadake.

|           C# API                    |         PowerShelli cmdlet-käsk                      |
|-------------------------------------|-----------------------------------------------:|
|[StartPartitionDataLossAsync] [dl]   |[Algus-ServiceFabricPartitionDataLoss] [psdl]   |
|[StartPartitionQuorumLossAsync] [ql] |[Algus-ServiceFabricPartitionQuorumLoss] [psql] |
|[StartPartitionRestartAsync] [rp]    |[Algus-ServiceFabricPartitionRestart] [psrp]    |

## <a name="conceptual-overview-of-running-a-command"></a>Käsu kontseptuaalne ülevaade

Viga süsti ja analüüsi teenuse kasutab asünkroonne mudelit, mis kus on käsk Alusta ühe API-ga, "Start" API selles dokumendis edaspidi, siis kontrollib selle käsu abil "GetProgress" API, kuni see on jõudnud terminal state, või tühistate selle edenemist.
Käsu käivitamiseks helistada vastava API API "Start".  See API järgmistel juhtudel tagastab vea süsti ja analüüsi teenus on kutse aktsepteerinud.  Siiski ei Näita, kui palju on käsu käivitada, või isegi siis, kui see on veel alanud.  Käsu edenemise kontrollimiseks kõne "GetProgress" API, mis vastab varem nimetati API "Start".  "GetProgress" API tagastab objekti, mis näitab praeguse oleku kuvamiseks käsku atribuudi olekusse sees.  Käsu töötab lõputult kuni:

1.  See on lõpule jõudnud edukalt.  Kui helistate "GetProgress" seda sel juhul, poolelioleva objekt riik valmis.
2.  See esineb parandamatu viga.  Kui helistate "GetProgress" seda sel juhul, poolelioleva objekt riik kuvatakse tõrge
3.  Tühistate kaudu [CancelTestCommandAsync]  [ cancel] API või [Peata-ServiceFabricTestCommand]  [ cancelps] PowerShelli cmdlet-käsk.  Kui helistate "GetProgress" seda sel juhul, olla edenemise objekti olekus tühistatud või ForceCancelled, sõltuvalt selle API argumendina.  Dokumentatsioonist [CancelTestCommandAsync]  [ cancel] rohkem üksikasju.


## <a name="details-of-running-a-command"></a>Käsu üksikasjad

Käsu käivitamiseks kõne alustamine oodatud argumendiga API.  Kõigi käivitamine API-de on nimega operationId Guid argument.  Saate peaks silma peal hoida argumendi operationId, kuna seda kasutatakse selle käsu täitmise edenemist jälgida.  See peab kantakse "GetProgress" API käsu edenemise jälgimiseks.  Funktsiooni operationId peab olema kordumatu.

Pärast edukalt kutsudes API alustada, tuleks GetProgress API nimetatakse esitatavaks tagastatud edenemise objekti olekus atribuudi lõpetamiseni.  Kõik [FabricTransientException's]  [ fte] ja OperationCanceledException isiku tuleks uuesti proovida.
Kui käsk ei jõudnud terminal riik (lõpule viidud, Faulted või tühistatud), on tagastatud edenemise objekti tulemi atribuudi täiendavat teavet.  Kui olek on lõpule viidud, sisaldab Result.SelectedPartition.PartitionId valitud sektsiooni ID-d.  Result.Exception on null.  Kui olek on tõrge, Result.Exception on põhjus viga süstimist ja analüüsi teenuse tehtud käsu.  Result.SelectedPartition.PartitionId on valitud sektsiooni id.  Mõnel juhul käsk võib ei on asus piisavalt valige sektsiooni.  Sel juhul on PartitionId on 0.  Kui olek on tühistatud, Result.Exception on tühi.  Nagu Faulted juhul, Result.SelectedPartition.PartitionId on valitud sektsiooni ID-d, kuid kui käsk toiminud pole piisavalt seda teha, siis on see 0.  Vaadake ka valimi allpool.

Proovi kood allpool näitab, kuidas alustada, siis märkige ruut käsu põhjustada andmekao kindlasse sektsiooni edenemist.

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // The id of the target partition inside the target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have the chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

Allpool valimi näitab, kuidas funktsiooni PartitionSelector abil saate valida määratud teenuse juhusliku sektsiooni:

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have the Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## <a name="history-and-truncation"></a>Ajalugu ja kärpimine

Pärast käsu ei jõudnud terminal state, metaandmete jääb viga süsti ja analüüsi teenuse teatud aja jooksul, enne kui see eemaldatakse ruumi säästmiseks.  Kui "GetProgress" nimetatakse abil operationId käsk pärast on eemaldatud, see on FabricException abil kuvatakse tõrkekood KeyNotFound tagasi.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
