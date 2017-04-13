<properties
   pageTitle="Testability toimingu | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse Microsoft Azure teenuse struktuuri leitud testability toimingud."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="timlt"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/03/2016"
   ms.author="motanv;heeldin"/>

# <a name="testability-actions"></a>Testability toimingud
Selleks, et simuleerida usaldusväärsed infrastruktuur, Azure teenuse struktuuri pakub teile, arendaja, viisid simuleerida erinevate tegelike tõrgete ja oleku üleminekuid. Need on esitatud testability toimingud. Toimingud on madala API-d, mis põhjustavad teatud viga süsti, oleku üleminek või valideerimine. Neid toiminguid kombineerides kirjutamise täielik testi stsenaariumid teenused.

Teenuse struktuuri pakub testi mõne levinud stsenaariumi, mis koosneb neid toiminguid. Soovitame, et kasutada nende sisseehitatud stsenaariumi, mis on hoolikalt valitud testimiseks levinud oleku üleminekuid ja tõrge juhtudel. Siiski saab luua kohandatud testi stsenaariumid, kui soovite lisada hõlmatud stsenaariumid, mis ei hõlma sisseehitatud stsenaariumid veel või mida on kohandatud just rakenduse toimingud.

C# rakenduste toimingud on leitud System.Fabric.dll komplekteerimine. Süsteemi struktuuri PowerShelli moodul on leitud Microsoft.ServiceFabric.Powershell.dll komplekti. Käitusaja installimise käigus, on installitud ServiceFabric PowerShelli mooduli kasutamise hõlbustamiseks.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Graatsiline vs ungraceful viga toimingud
Testability toimingud on kaks põhilist ämbrid liigitatud:

* Ungraceful vead: need vead simuleerida tõrkeid, nagu seadme taaskäivitamist ja protsessi krahh. Sellisel juhul tõrkeid täitmise raames lõpetab järsku. See tähendab, et pole Kettapuhastus riigi saate käivitada enne rakendus käivitub uuesti.

* Graatsiline vead: need vead simuleerida graatsiline toimingud nagu koopia viib ja käivitab koormusetasakaalustuseks langeb. Sellisel juhul teenuse saab teatise Sule ja puhastada olek enne väljumist.

Parem kvaliteedi kontrollimiseks käivitage teenus ja äri töökoormus ajal tekitavate erinevate graatsiline ja ungraceful vead. Ungraceful vead kasutamise stsenaariumid, kus teenuse protsessi väljub järsku keskel mõned töövoog. See testib tee taastamine, kui koopia teenus on taastatud teenuse struktuuri. See aitab andmete järjepidevuse ja seda, kas teenuse olek on säilitada õigesti pärast ebaõnnestumist. Muude määramine tõrkeid (graatsiline ebaõnnestumist) testida, et teenuse reageerib õigesti koopiad ei teisaldataks, teenuse struktuuri. See testib käsitsemise tühistamise RunAsync meetod. Teenuse tuleb otsida määramine, õigesti salvestada seisu ei luba tühistamine ja sulgege RunAsync meetod.

## <a name="testability-actions-list"></a>Testability toimingute loend

| Toiming | Kirjeldus | Hallatav API | PowerShelli cmdlet-käsk | Graatsiline/ungraceful vead |
|---------|-------------|-------------|-------------------|------------------------------|
|CleanTestState| Eemaldab kobar korral halb sulgumist testi juhi testi olek. | CleanTestStateAsync | Eemalda – ServiceFabricTestState | Pole rakendatav |
| InvokeDataLoss | Andmete kaotsimineku põhjustab teenuse sektsiooni sisse. | InvokeDataLossAsync | Autonoomsest ServiceFabricPartitionDataLoss | Graatsiline |
| InvokeQuorumLoss | Teenusega stateful sektsiooni asetab kvoorumi kaotus. | InvokeQuorumLossAsync | Autonoomsest ServiceFabricQuorumLoss | Graatsiline |
| Esmane teisaldamine | Viib määratud peamine koopia stateful teenuse määratud kobar sõlm. | MovePrimaryAsync | Teisaldamine – ServiceFabricPrimaryReplica | Graatsiline |
| Teisese teisaldamine | Viib praeguse teise koopia stateful teenuse erinevate kobar sõlm. | MoveSecondaryAsync | Teisaldamine – ServiceFabricSecondaryReplica | Graatsiline |
| RemoveReplica | Jäljendab koopia tõrke, eemaldades klaster koopia. Selle koopia suletakse ja on selle rolli üleminek "Pole", kõik seisu eemaldamine klaster. | RemoveReplicaAsync | Eemalda – ServiceFabricReplica | Graatsiline |
| RestartDeployedCodePackage | Jäljendab taaskäivitamine kood paketi juurutatud sõlme klaster kood paketi protsessi tõrge. See käsk kood paketi protsess, mis selle protsessi kõigi kasutaja teenuse koopiate majutatud uuesti. | RestartDeployedCodePackageAsync | Uuesti-ServiceFabricDeployedCodePackage | Ungraceful |
| RestartNode | Jäljendab taaskäivitamine sõlm teenuse struktuuri kobar sõlm tõrge. | RestartNodeAsync | Uuesti-ServiceFabricNode | Ungraceful |
| RestartPartition | Jäljendab andmekeskuse elektrikatkestus või kobar elektrikatkestus stsenaarium taaskäivitamine sektsiooni mõned või kõik koopiad. | RestartPartitionAsync | Uuesti-ServiceFabricPartition | Graatsiline |
| RestartReplica | Jäljendab koopia tõrge klaster nõutud koopia taaskäivitada, sulgege koopia ja seejärel avage see uuesti. | RestartReplicaAsync | Uuesti-ServiceFabricReplica | Graatsiline |
| StartNode | Käivitab sõlme klaster, mis on juba lõpetanud. | StartNodeAsync | Algus-ServiceFabricNode | Pole rakendatav |
| StopNode | Lõpetades sõlme klaster jäljendab sõlm tõrge. Sõlme jäävad allapoole, kuni StartNode nimetatakse. | StopNodeAsync | Lõpeta – ServiceFabricNode | Ungraceful |
| ValidateApplication | Kättesaadavus ja kõigi teenuse struktuuri teenuste rakenduses, tavaliselt pärast tekitavate mõne vea süsteemi seisundi kinnitatakse. | ValidateApplicationAsync | Test-ServiceFabricApplication | Pole rakendatav |
| ValidateService | Tavaliselt pärast tekitavate mõne vea süsteemi kinnitatakse kättesaadavust ja teenuse struktuuri teenuse seisund. | ValidateServiceAsync | Test-ServiceFabricService | Pole rakendatav |

## <a name="running-a-testability-action-using-powershell"></a>Töötab testability toimingu, PowerShelli abil

Selle õpetuse näitab, kuidas käivitada testability toimingu PowerShelli abil. Saate teada, kuidas kohaliku (üks kast) kobar või mõne Azure kobar testability toimingu käivitamiseks. Microsoft.Fabric.Powershell.dll--teenuse struktuuri PowerShelli mooduli--installitakse automaatselt, kui installite Microsoft teenuse struktuuri MSI. Mooduli laaditakse automaatselt, kui avate PowerShelli käsuviibas.

Õpetus segmente:

- [Toimingu vastuolus ühe-karbi kobar](#run-an-action-against-a-one-box-cluster)
- [Toimingu vastuolus on Azure kobar](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Toimingu vastuolus ühe-karbi kobar

Kohaliku kobar vastu testability toimingu käivitamiseks esmalt ühenduse klaster ja avage PowerShelli käsuviibas administraatoriõigustes. Vaadakem toimingu **Uuesti-ServiceFabricNode** .

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Siin on käivitanud toimingu **Uuesti-ServiceFabricNode** sõlm nimega "Node1". Lõpetamise režiimi saate määrata, et see peaks kinnitada, kas toimingu uuesti sõlme tegelikult õnnestus. Kui "Kontrollida" põhjustab seda, et kontrollida, kas toimingu uuesti tegelikult õnnestus, mis määrab lõpetamise režiimi. Mitte otse täpsustades sõlme oma nime, saate selle määrata sektsiooni võti ja koopia, millist kaudu järgmiselt:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Uuesti-ServiceFabricNode** tuleks kasutada klaster teenuse struktuuri sõlm taaskäivitama. See toiming peatab Fabric.exe protsess, mille kõik süsteemi teenuseid ja kasutajale teenuse kujundusmuudatusi majutatud sõlme uuesti. See API testimiseks teenust aitab avastada vead mööda Tõrkesiirde taastamine teed. See aitab simuleerida klaster sõlm ebaõnnestumist.

Järgmine pilt kuvatakse **Uuesti-ServiceFabricNode** testability käsu toimingu.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

Esimese **Get-ServiceFabricNode** (cmdlet-käsu kaudu teenuse struktuuri PowerShelli moodul) väljund näitab, et kohaliku kobar on viis sõlmed: Node.1 Node.5 abil. Pärast toimingu testability (cmdlet) **Uuesti-ServiceFabricNode** käivitatakse sõlme, nimega Node.4, näeme, et selle sõlm sees on lähtestatud.

### <a name="run-an-action-against-an-azure-cluster"></a>Toimingu vastuolus on Azure kobar

Töötab testability toimingu (PowerShelli abil) vastu ka Azure kobar on sarnane töötab toimingu kohaliku kobar suhtes. Ainus erinevus on, et enne käivitada toimingu, mitte ühenduse loomine kohaliku kobar, peate esmalt Azure kobar ühenduse.

## <a name="running-a-testability-action-using-c35"></a>Töötab testability toimingu, kasutades C & #35;

Käivitage testability toimingu abil C#, peate ühenduse klaster FabricClient abil. Hankima toimingu käivitamiseks nõutavad parameetrid. Erinevate parameetrite saab kasutada sama toimingu käivitamiseks.
Vaadates RestartServiceFabricNode toimingu, käivitage see üheks võimaluseks on klaster sõlm teave (sõlme nimi ja sõlm eksemplari ID) abil.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Parameetri selgitus:

- **CompleteMode** saate määrata, et režiimi tuleks kontrollida, kas toimingu uuesti tegelikult õnnestus. Kui "Kontrollida" põhjustab seda, et kontrollida, kas toimingu uuesti tegelikult õnnestus, mis määrab lõpetamise režiimi.  
- **OperationTimeout** määrab aja toimingu lõpuleviimiseks enne TimeoutException erand.
- **CancellationToken** võimaldab ootel kõne tühistada.

Asemel otse täpsustades sõlme oma nime, saate selle sektsiooni võti ja millist koopia kaudu määrata.

Lisateabe saamiseks vt [PartitionSelector ja ReplicaSelector](#partition_replica_selector).


```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>PartitionSelector ja ReplicaSelector

### <a name="partitionselector"></a>PartitionSelector
PartitionSelector on esitatud testability abimees ja seda kasutatakse valida mõne kindla jaotise testability toimingute sooritamiseks. See saab valida kindla sektsiooni, kui sektsiooni ID on eelnevalt teada. Või saate sisestada sektsiooni võti ja toiming lahendab sektsiooni ID ettevõttesiseselt. Teil on ka valida juhuslik sektsiooni.

See helper kasutamiseks PartitionSelector objekti loomine ja valige sektsioon, kasutades ühte järgmistest meetoditest valige *. Seejärel edastada API, mis nõuab PartitionSelector objekti. Kui valitud on suvand puudub, vaikimisi juhusliku sektsiooni.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>ReplicaSelector
ReplicaSelector on esitatud testability abimees ja kasutatakse valige mis tahes testability toiminguid teha koopia. Ta saab valida kindla koopia, kui koopia ID on eelnevalt teada. Lisaks on teil valida peamine koopia või juhusliku teisese. ReplicaSelector tuleneb PartitionSelector, seega peate valima koopia nii sektsiooni, kuhu soovite toimingu testability.

See helper kasutamiseks ReplicaSelector objekti loomine ja seadke soovitud viisil koopia ja sektsiooni valimiseks. Seejärel saate andke seda API, mis nõuab sisse. Kui valitud on suvand puudub, vaikimisi juhusliku koopia ja juhusliku partition.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Järgmised sammud

- [Testability stsenaariumid](service-fabric-testability-scenarios.md)
- Kuidas testida teenuse
   - [Simuleerida tõrkeid teenuse töökoormus ajal](service-fabric-testability-workload-tests.md)
   - [Teenusest side tõrked](service-fabric-testability-scenarios-service-communication.md)
