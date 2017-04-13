<properties
    pageTitle="Teenuse siini PowerShelliga haldamine | Microsoft Azure'i"
    description="Hallata teenuse siini PowerShelli skriptide abil"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="sethm"/>

# <a name="manage-service-bus-with-powershell"></a>Teenuse siini PowerShelliga haldamine

## <a name="overview"></a>Ülevaade

Microsoft Azure'i PowerShelli on skriptimine keskkonnas, mille abil saate kontrollida ja juurutamise ja haldamise oma töökoormus Azure automatiseerida. Selles artiklis kirjeldatakse, kuidas ette valmistada ja hallata teenuse siini üksustele nagu nimeruumid, järjekorrad ja sündmuse jaoturi abil kohaliku Azure PowerShelli konsooli PowerShelli abil.

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist selles artiklis, peab teil olema järgmised eeltingimused:

- Azure'i tellimuse. Azure'i on tellimispõhine platvorm. Tellimuse hankimise kohta lisateabe saamiseks lugege [Ostu suvandid][], [Liikme pakub][]või [Tasuta prooviversioon][].

- Arvuti Azure PowerShelli abil. Juhised leiate teemast [installida ja konfigureerida Azure PowerShelli][].

- PowerShelli skriptide, Nugeti pakettide ja .NET Framework üldine mõistmine.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>.NET-komplekti jaoks teenuse siini viide

Teatud PowerShelli cmdlet-käsud on saadaval haldamise teenuse siini. Üksused, mis ei ole kokku läbi olemasoleva cmdletid ettevalmistamise, saate .net-i kliendi teenuse siini [teenuse siini Nugeti pakett][].

Esmalt veenduge, et skripti saate üles otsida **Microsoft.ServiceBus.dll** komplekti, mis installitakse koos Nugeti pakett. Selleks, et olla paindlik, sooritab skripti järgmist.

1. Määrab, kus käivitati tee.
2. Läbib tee kuni leiab kausta nimega `packages`. Selle kausta loomist Nugeti pakettide installimisel.
3. Rekursiivselt otsingud on `packages` komplekti **Microsoft.ServiceBus.dll**nimega kaust.
4. Viited selle komplekti nii, et tüübid on hilisemaks kasutamiseks.

Siin on, kuidas rakendatakse järgmist PowerShelli skripti:

```
try
{
    # WARNING: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error "Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script."
}
```

## <a name="provision-a-service-bus-namespace"></a>Sätte teenuse siini nimeruum

Kahe PowerShelli cmdlet-käskude tugi teenuse siini nimeruum toimingud. .NET SDK API, asemel saate kasutada [Get-AzureSBNamespace][] ja [Uus-AzureSBNamespace][].

Selles näites loob mõne kohalikku muutujat skripti; `$Namespace` and `$Location`.

- `$Namespace`on töö soovime teenuse siini nimeruumi nimi.
- `$Location`tuvastab mis skripti sätted nimeruumi data Centeri kaudu.
- `$CurrentNamespace`salvestab viide nimeruumi, mida skript toob (või loob).

Kuvatakse tegelike skripti `$Namespace` ja `$Location` saab edasi parameetrid.

See osa skripti hõlmab järgmisi toiminguid:

1. Katsete toomiseks nimeruumi teenuse siini esitatud nimega.
2. Nimeruumi leidmisel selle aruandeid, mis ei leitud.
3. Kui nimeruumi ei leita, loob nimeruumi ja seejärel toob vastloodud nimeruumi.

    ```
    $Namespace = "MyServiceBusNS"
    $Location = "West US"
    
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
    
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Output "The namespace [$Namespace] already exists in the [$($CurrentNamespace.Region)] region."
    }
    else
    {
        Write-Host "The [$Namespace] namespace does not exist."
        Write-Output "Creating the [$Namespace] namespace in the [$Location] region..."
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace $false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```

Muude teenuste siini üksuste ettevalmistamise, luua SDK [NamespaceManager][] klassi eksemplar.
Saate [Get-AzureSBAuthorizationRule][] cmdlet-käsk Luba reeglit, mida kasutatakse ühendusstringi toomiseks. Viite saab salvestada selle `NamespaceManager` astme soovitud `$NamespaceManager` muutujana. Kasutame `$NamespaceManager` skripti ettevalmistamise muude üksuste hiljem.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the event hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-other-service-bus-entities"></a>Muude teenuste siini üksuste ettevalmistamine

Ettevalmistamise muude üksuste, näiteks järjekorrad, teemasid ja sündmuse jaoturi, et kasutada [.NET API teenuse siini jaoks][]. See artikkel keskendub ainult sündmuse jaoturi, kuid juhiseid muud üksused on sarnased. Lisaks üksikasjalikumat näiteid, sealhulgas muude üksuste viidatud käesoleva artikli lõpus.

See osa skripti loob neli rohkem kohalikku muutujat. Neid muutujaid kasutatakse väärtustada mõne `EventHubDescription` objekti. Skripti hõlmab järgmisi toiminguid:

1. Abil soovitud `NamespaceManager` objekti, vaadake, kui sündmus jaoturi tähistatud `$Path` olemas.
2. Kui see pole olemas, looge mõne `EventHubDescription` ja edasi, et soovite selle `NamespaceManager` klassi `CreateEventHubIfNotExists` meetod.
3. Pärast sündmuse jaoturi kättesaadav, kindlaks teha, luua tarbija rühma abil `ConsumerGroupDescription` ja `NamespaceManager`.

    ```
    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"
        
    # Check to see if the Event Hub already exists
    if ($NamespaceManager.EventHubExists($Path))
    {
        Write-Output "The [$Path] event hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] event hub in the [$Namespace] namespace: PartitionCount=[$PartitionCount] MessageRetentionInDays=[$MessageRetentionInDays]..."
        $EventHubDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.EventHubDescription -ArgumentList $Path
        $EventHubDescription.PartitionCount = $PartitionCount
        $EventHubDescription.MessageRetentionInDays = $MessageRetentionInDays
        $EventHubDescription.UserMetadata = $UserMetadata
        $EventHubDescription.Path = $Path
        $NamespaceManager.CreateEventHubIfNotExists($EventHubDescription);
        Write-Output "The [$Path] event hub in the [$Namespace] namespace has been successfully created."
    }
        
    # Create the consumer group if it doesn't exist
    Write-Output "Creating the consumer group [$ConsumerGroupName] for the [$Path] event hub..."
    $ConsumerGroupDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.ConsumerGroupDescription -ArgumentList $Path, $ConsumerGroupName
    $ConsumerGroupDescription.UserMetadata = $ConsumerGroupUserMetadata
    $NamespaceManager.CreateConsumerGroupIfNotExists($ConsumerGroupDescription);
    Write-Output "The consumer group [$ConsumerGroupName] for the [$Path] event hub has been successfully created."
    ```

## <a name="migrate-a-namespace-to-another-azure-subscription"></a>Migreerimine nimeruumi mõnele muule Azure tellimusele

Järgmised käsud jada viib nimeruumi ühe Azure tellimuselt teisele. Selle toimingu täitmiseks nimeruumi olema juba aktiivne ja PowerShelli käsud kasutaja peab olema nii lähte- ja sihtsaitide tellimuste administraator.

```
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Järgmised sammud

Selles artiklis ette ettevalmistamise teenuse siini üksuste PowerShelli kaudu lihtne ülevaade. Midagi, mida .net-i kliendi teekide abil teha, saate teha ka PowerShelli skripti.

Ajaveebipostituste need on saadaval üksikasjalikumat näited:

- [Kuidas luua teenuse järjekorrad, teemade ja tellimuste PowerShelli skripti abil](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Kuidas luua teenuse siini Namespace ja sündmuse jaoturi, mis PowerShelli skripti abil](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Mõned valmis skriptid on ka allalaadimiseks saadaval.
- [Teenus siini PowerShelli skriptide](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Link references-->
[Ostmise võimalused]: http://azure.microsoft.com/pricing/purchase-options/
[Liikme pakkumised]: http://azure.microsoft.com/pricing/member-offers/
[Tasuta prooviversioon]: http://azure.microsoft.com/pricing/free-trial/
[Installida ja konfigureerida Azure PowerShell]: ../powershell-install-configure.md
[Teenuse siini Nugeti pakett]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Uue AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[.Net-i API teenuse siini jaoks]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.aspx
[NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
