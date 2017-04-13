<properties
   pageTitle="Aruande ja Azure teenuse struktuuri koos seisundi kontrollimine | Microsoft Azure'i"
   description="Siit saate teada, kuidas saata oma teenuse kood Seisundiaruannete ja kuidas saan kontrollida oma teenuse seisund leiate Azure'i teenuse struktuuri seisundi jälgimine tööriistade abil."
   services="service-fabric"
   documentationCenter=".net"
   authors="toddabel"
   manager="mfussell"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/06/2016"
   ms.author="toddabel"/>

# <a name="report-and-check-service-health"></a>Aruande ja teenuse seisundi kontrollimine
Oma teenuste tekib probleeme, vastata ja parandada juhtumite ja katkestuste sõltub teie võimet probleeme kiiresti tuvastada. Kui te aruande probleemid ja tõrked Azure teenuse struktuuri seisund ülemus oma teenuse kood, saate kasutada standard seisundi jälgimine tööriistu, mis pakub oleku seisund teenuse struktuuri.

On kaks võimalust teenuse seisundit saate teatada.

- Kasutage objektide [sektsiooni](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.aspx) või [CodePackageActivationContext](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.aspx) .  
Saate kasutada funktsiooni `Partition` ja `CodePackageActivationContext` objektide teatada seisundi elemente, mis on osa praeguses kontekstis. Näiteks koodi, mis käivitatakse osana koopia saate aruande seisund ainult selle koopia, mis kuulub sektsiooni ja rakendus, mis on osa.

- Kasutage `FabricClient`.   
Saate kasutada `FabricClient` aruande seisund teenuse kood, kui klaster on [turvaline](service-fabric-cluster-security.md) või kui teenus töötab administraatoriõigused. See ei saa true tegelike enamikul juhtudel. Koos `FabricClient`, saab seisundit aruandluseks üksus, mis on osa klaster. Ideaalvariandis mitte siiski teenuse kood tuleks ainult saada aruandeid, mis on seotud oma seisund.

Selles artiklis tutvustatakse näiteks, et aruannete seisund teenuse kood. Näites kuvatakse ka, kuidas tööriistu, mis sisaldab teenuse struktuuri saab seisundit oleku kontrollimine. Selles artiklis peaks olema tutvustuse seisundi jälgimine võimaluste teenuse struktuuri. Üksikasjalikumat teavet, lugege põhjalikumat artiklite seisundi kohta, mis algavad käesoleva artikli lõpus link sari.

## <a name="prerequisites"></a>Eeltingimused
Peab teil olema installitud järgmine:

   * Visual Studio 2015
   * Teenuse struktuuri SDK

## <a name="to-create-a-local-secure-dev-cluster"></a>Luua kohaliku turvaline arendaja kobar
- Avage PowerShelli administraatoriõigused ja käivitage järgmised käsud.

![Käsud, mis näitab, kuidas luua turvalist arendaja kobar](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Rakenduse juurutada ja selle seisundi kontrollimine

1. Avage Visual Studio administraatorina.

2. Projekti loomine **Stateful teenuse** malli abil.

    ![Stateful teenusega teenuse struktuuri rakenduse loomine](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)

3. Vajutage klahvi **F5** käivitage rakendus silumine režiimis. Rakenduse juurutatakse kohaliku klaster.

4. Pärast seda, kui rakendus töötab, paremklõpsake kohalik halduri olekualal ikooni ja valige **Halda kohaliku kobar** kiirmenüü teenuse struktuuri Exploreri avamiseks.

    ![Avage teenus struktuuri Explorer olekualal](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)

5. Rakenduse seisundi peaks olema kuvatud, nagu seda pilti. Sel ajal, olla terve ja tõrkeid.

    ![Terve rakenduse teenuse struktuuri Exploreris](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)

6. Saate vaadata ka seisundi PowerShelli abil. Saate kasutada ```Get-ServiceFabricApplicationHealth``` vaadata rakenduse seisundi ja te saate kasutada ```Get-ServiceFabricServiceHealth``` kontrollida teenuse seisund. Seisundiaruanne PowerShelli sama rakenduse jaoks on sellel pildil.

    ![Terve rakenduse PowerShellis](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Kohandatud seisund sündmuste lisamiseks oma teenuse kood
Visual Studio teenuse struktuuri projekti mallid sisaldavad proovi kood. Järgmised toimingud näitavad, kuidas saavad aruande kohandatud seisund sündmuste oma teenuse kood. Need aruanded kuvatakse automaatselt standard tööriistad teenuse struktuuri pakub nt teenuse struktuuri Explorer, Azure portaali seisund vaade ja PowerShelli seisundi kontrollimiseks.

1. Avage rakenduse Visual Studio varem loodud või luua uue rakenduse Visual Studio **Stateful teenuse** malli abil.

2. Avage fail Stateful1.cs ja otsige soovitud `myDictionary.TryGetValueAsync` sisse helistada on `RunAsync` meetod. Saate vaadata, et seda meetodit tagastab on `result` mis hoiab näidiku praegune väärtus, kuna võtme loogika see rakendus on hoida töötab sõnaarvestuse. Kui see päris rakenduse ja tulem puudumine esindatud tõrke, ei taha lipu sündmusega.

3. Aruande seisund sündmuse kui tulemi puudumine tähistab tõrge, lisage toimige järgmiselt.

    lisamine. Lisage soovitud `System.Fabric.Health` nimeruum Stateful1.cs faili.

    ```csharp
    using System.Fabric.Health;
    ```

    b. Lisage järgmine kood pärast selle `myDictionary.TryGetValueAsync` helistamine

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    Me aruande koopia seisund, kuna see on teatatud stateful teenus. Funktsiooni `HealthInformation` parameetri talletab teavet tervise probleemi, mis on teatatud.

    Kui olete loonud kodakondsuseta teenus, kasutage järgmine kood

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```

4. Kui teie teenus töötab administraatoriõigused või kui klaster pole [turvaline](service-fabric-cluster-security.md), saate ka `FabricClient` aruande seisund, nagu on näidatud toimige järgmiselt.  

    lisamine. Luua selle `FabricClient` astme pärast selle `var myDictionary` deklareerimise.

    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```

    b. Lisage järgmine kood pärast selle `myDictionary.TryGetValueAsync` kõne.

    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.ServiceInitializationParameters.PartitionId,
            this.ServiceInitializationParameters.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```

5. Vaatame simuleerida selle tõrke ja vaadake seisundi jälgimine tööriistad nähtavaks. Simuleerida tõrge, kommentaari välja varem lisatud seisund aruannete koodi esimene rida. Kui te kommentaari välja kõigepealt esimene joon, näeb koodi järgmises näites.

    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
 Järgmine kood nüüd tule selle seisundiaruanne iga kord, kui `RunAsync` aktiveeritakse. Pärast selle muudatuse, vajutage klahvi **F5** käivitage rakendus.

6. Kui rakendus töötab, avage teenuse seisundi kontrollimiseks rakenduse struktuuri Explorer. Sel ajal, teenuse struktuuri Explorer näitab, et rakendus on vigane. Selle põhjuseks on tõrge, mis teatati kood, et lisasime varem.

    ![Vigane rakenduse teenuse struktuuri Exploreris](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)

7. Kui valite puuvaates teenuse struktuuri Exploreri peamine koopia, kuvatakse **Seisundioleku** näitab tõrke, liiga. Teenuse struktuuri Explorer kuvatakse ka seisundi aruande üksikasjad, mis lisati selle `HealthInformation` parameetri kood. Saate vaadata sama Seisundiaruannete PowerShelli ja Azure portaali.

    ![Koopia seisund teenuse struktuuri Exploreris](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Aruande jääb seisund halduri kuni selle mõne muu aruande või selle koopia on kustutatud. Kuna me ei määrata `TimeToLive` selle seisundi aruande selle `HealthInformation` objekti aruande kunagi aegub.

Soovitame seisund esitatakse kõige Varundustöö taseme, mis sel juhul on koopia. Saate teatada ka seisundi kohta `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Klõpsake aruande seisund `Application`, `DeployedApplication`, ja `DeployedServicePackage`, kasutage `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Järgmised sammud
[Sügav veealuste teenuse struktuuri seisundi kohta](service-fabric-health-introduction.md)
