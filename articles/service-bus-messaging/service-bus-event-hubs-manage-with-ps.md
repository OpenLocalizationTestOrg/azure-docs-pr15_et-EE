<properties
    pageTitle="PowerShelli abil saate hallata teenuse siini ja sündmuse jaoturi ressursid | Microsoft Azure'i"
    description="PowerShelli kaudu, luua ja hallata teenuse siini ja sündmuse jaoturi ressursid"
    services="service-bus,event-hubs"
    documentationCenter=".NET"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="use-powershell-to-manage-service-bus-and-event-hubs-resources"></a>PowerShelli abil saate hallata teenuse siini ja sündmuse jaoturi ressursse

Microsoft Azure'i PowerShelli on skriptimine keskkonnas, mille abil saate kontrollida ja automatiseerida juurutus- ja Azure teenused. Selles artiklis kirjeldatakse, kuidas ette valmistada ja hallata teenuse siini üksustele nagu nimeruumid, järjekorrad ja sündmuse jaoturi abil kohaliku Azure PowerShelli konsooli PowerShelli abil.

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist peate järgmist.

- Azure'i tellimuse. Azure'i on tellimispõhine platvorm. Tellimuse hankimise kohta leiate lisateavet teemast [ostmise võimalused][], [liikme pakub][]või [tasuta konto][].

- Arvuti Azure PowerShelli abil. Juhised leiate teemast [installida ja konfigureerida Azure PowerShelli][].

- PowerShelli skriptide, Nugeti pakettide ja .NET Framework üldine mõistmine.

## <a name="include-a-reference-to-the-net-assembly-for-service-bus"></a>Viite siini teenuse jaoks .NET-komplekti

Teatud PowerShelli cmdlet-käsud on saadaval haldamise teenuse siini. Ettevalmistamise üksused, mis ei ole kokku olemasoleva cmdletid kaudu, saate kasutada .net-i kliendi teenuse siini sees PowerShelli kaudu jaoks poolt [teenuse siini Nugeti pakett]viitamine.

Esmalt veenduge, et skripti saate üles otsida **Microsoft.ServiceBus.dll** komplekti, mis installitakse koos Nugeti pakett. Selleks, et olla paindlik, sooritab skripti järgmist.

1. Määratleb, millest käivitati tee.
2. Läbib tee kuni leiab kausta nimega `packages`. Selle kausta loomist Nugeti pakettide installimisel.
3. Rekursiivselt otsingud on `packages` komplekti **Microsoft.ServiceBus.dll**nimega kaust.
4. Viited selle komplekti nii, et tüübid on hilisemaks kasutamiseks.

Siin on, kuidas rakendatakse järgmist PowerShelli skripti:

```powershell

try
{
    # IMPORTANT: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}

```

## <a name="provision-a-service-bus-namespace"></a>Sätte teenuse siini nimeruum

Teenuse siini nimeruumid töötamisel on kaks .NET SDK asemel saate kasutada cmdlet-käsud: [Get-AzureSBNamespace][] ja [Uus-AzureSBNamespace][].

Selles näites loob mõne kohalikku muutujat skripti; `$Namespace` and `$Location`.

- `$Namespace`nimi on selle teenuse siini nimeruumi töö soovime.
- `$Location`tuvastab andmekeskuse, kus näidatakse me ettevalmistamise nimeruumi.
- `$CurrentNamespace`salvestab viide nimeruumi, mida me tuua (või luua).

Kuvatakse tegelike skripti `$Namespace` ja `$Location` saab edasi parameetrid.

See osa skripti teeb järgmist.

1. Katsete toomiseks nimeruumi teenuse siini esitatud nimega.
2. Nimeruumi leidmisel selle aruandeid, mis ei leitud.
3. Kui nimeruumi ei leita, loob nimeruumi ja seejärel toob vastloodud nimeruumi.

    ``` powershell

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
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```
Muude teenuste siini üksuste ettevalmistamise, eksemplari loomine soovitud `NamespaceManager` objekti SDK. Saate [Get-AzureSBAuthorizationRule][] cmdlet-käsk Luba reeglit, mida kasutatakse ühendusstringi toomiseks. Selles näites talletab viide on `NamespaceManager` astme soovitud `$NamespaceManager` muutujana. Skript hiljem kasutab `$NamespaceManager` muude üksuste ette valmistada.

    ``` powershell
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    # Create the NamespaceManager object to create the Event Hub
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
    ```

## <a name="provisioning-other-service-bus-entities"></a>Muude teenuste siini üksuste ettevalmistamine

Ettevalmistamise muude üksuste, näiteks järjekorrad, teemasid ja sündmuse jaoturi, saate [Teenuse siini jaoks .net-i API -ga][]. Üksikasjalikumat näiteid, sealhulgas muude üksuste viidatud käesoleva artikli lõpus.

### <a name="create-an-event-hub"></a>Kuvatakse sündmuse jaoturi loomine

See osa skripti loob neli rohkem kohalikku muutujat. Neid muutujaid kasutatakse väärtustada mõne `EventHubDescription` objekti. Skripti teeb järgmist.

1. Abil soovitud `NamespaceManager` objekti, vaadake, kui sündmus jaoturi tähistatud `$Path` olemas.
2. Kui see pole olemas, looge mõne `EventHubDescription` ja edasi, et soovite selle `NamespaceManager` klassi `CreateEventHubIfNotExists` meetod.
3. Pärast sündmuse jaoturi kättesaadav, kindlaks teha, luua tarbija rühma abil `ConsumerGroupDescription` ja `NamespaceManager`.

    ``` powershell

    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"

    # Check if the Event Hub already exists
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

### <a name="create-a-queue"></a>Järjekorra loomiseks

Järjekorda või teema loomiseks tehke nimeruum sisse nagu eelmises punktis.

    if ($NamespaceManager.QueueExists($Path))
    {
        Write-Output "The [$Path] queue already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] queue in the [$Namespace] namespace..."
        $QueueDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.QueueDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $QueueDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $QueueDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $QueueDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $QueueDescription.EnableBatchedOperations = $EnableBatchedOperations
        $QueueDescription.EnableDeadLetteringOnMessageExpiration = $EnableDeadLetteringOnMessageExpiration
        $QueueDescription.EnableExpress = $EnableExpress
        $QueueDescription.EnablePartitioning = $EnablePartitioning
        $QueueDescription.ForwardDeadLetteredMessagesTo = $ForwardDeadLetteredMessagesTo
        $QueueDescription.ForwardTo = $ForwardTo
        $QueueDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        if ($LockDuration -gt 0)
        {
            $QueueDescription.LockDuration = [System.TimeSpan]::FromSeconds($LockDuration)
        }
        $QueueDescription.MaxDeliveryCount = $MaxDeliveryCount
        $QueueDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $QueueDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        $QueueDescription.RequiresSession = $RequiresSession
        if ($EnablePartitioning)
        {
            $QueueDescription.SupportOrdering = $False
        }
        else
        {
            $QueueDescription.SupportOrdering = $SupportOrdering
        }
        $QueueDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateQueue($QueueDescription);
    }

### <a name="create-a-topic"></a>Teema loomine

    if ($NamespaceManager.TopicExists($Path))
    {
        Write-Output "The [$Path] topic already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] topic in the [$Namespace] namespace..."
        $TopicDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.TopicDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $TopicDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $TopicDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $TopicDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $TopicDescription.EnableBatchedOperations = $EnableBatchedOperations
        $TopicDescription.EnableExpress = $EnableExpress
        $TopicDescription.EnableFilteringMessagesBeforePublishing = $EnableFilteringMessagesBeforePublishing
        $TopicDescription.EnablePartitioning = $EnablePartitioning
        $TopicDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        $TopicDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $TopicDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        if ($EnablePartitioning)
        {
            $TopicDescription.SupportOrdering = $False
        }
        else
        {
            $TopicDescription.SupportOrdering = $SupportOrdering
        }
        $TopicDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateTopic($TopicDescription);
    }

## <a name="next-steps"></a>Järgmised sammud

Selles artiklis ette saate lihtsa töövoo ettevalmistamise teenuse siini üksuste PowerShelli kaudu. Kuigi on teatud PowerShelli cmdlet-käskude haldamise teenuse siini sõnumside üksused, viidates Microsoft.ServiceBus.dll komplekti, peaaegu kõik toimingud, saate teha abil saate teha ka PowerShelli skripti .net-i kliendi teegid.

See on saadaval järgmiste ajaveebipostituste üksikasjalikumat näited:

- [Kuidas luua teenuse järjekorrad, teemade ja tellimuste PowerShelli skripti abil](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Kuidas luua teenuse siini Namespace ja sündmuse jaoturi, mis PowerShelli skripti abil](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Mõned valmis skriptid on ka allalaadimiseks saadaval.

- [Teenus siini PowerShelli skriptide](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[ostmise võimalused]: http://azure.microsoft.com/pricing/purchase-options/
[liikme pakub]: http://azure.microsoft.com/pricing/member-offers/
[tasuta konto]: http://azure.microsoft.com/pricing/free-trial/
[Teenuse siini Nugeti pakett]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Uue AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[.Net-i API teenuse siini jaoks]: https://msdn.microsoft.com/en-us/library/azure/mt419900.aspx
[Installida ja konfigureerida Azure PowerShell]: ../powershell-install-configure.md
