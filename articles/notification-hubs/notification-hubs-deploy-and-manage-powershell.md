<properties 
    pageTitle="Juurutada ja hallata teatis jaoturi PowerShelli abil" 
    description="Kuidas luua ja hallata teatis jaoturi abil PowerShelli automatiseerimine" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Juurutada ja hallata teatis jaoturi PowerShelli abil

##<a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse, kuidas kasutada loomine ja haldamine Azure'i teatis jaoturi PowerShelli kaudu. Järgmised automatiseerimise Põhitoimingud on näidatud selles teemas.

+ Teate jaoturi loomine
+ Mandaadi seadmine

Kui ka peate looma uue teenuse siini nimeruumi oma teatise jaoturi, lugege teemat [PowerShelli abil hallata teenuse siini](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Jaoturi teatiste haldamine ei toeta otseselt kaasas Azure PowerShelli cmdlet-käsud. PowerShelli kaudu parim lahendus on viide Microsoft.Azure.NotificationHubs.dll komplekti. [Microsoft Azure'i teatis jaoturi Nugeti pakett](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)levitatakse selle komplekti.


## <a name="prerequisites"></a>Eeltingimused

Enne alustamist selles artiklis, peab teil olema järgmised:

- Azure'i tellimuse. Azure'i on tellimispõhine platvorm. Tellimuse hankimise kohta lisateabe saamiseks lugege [Ostu suvandid], [Liikme pakub]või [Tasuta prooviversioon].

- Azure'i PowerShelliga arvuti. Juhised leiate teemast [installida ja konfigureerida Azure PowerShelli].

- PowerShelli skriptide, Nugeti pakettide ja .NET Framework üldine mõistmine.


## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>.NET-komplekti jaoks teenuse siini viide

Azure'i teatis jaoturi haldamise pole veel kaasas Azure PowerShelli PowerShelli cmdlet-käsud. Ettevalmistamise teatis jaoturi, saate kasutada [Microsoft Azure'i teatis jaoturi Nugeti pakett](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).net-i klient.

Esmalt veenduge, et leida oma skripti **Microsoft.Azure.NotificationHubs.dll** komplekti, mis on installitud Visual Studio projektis Nugeti paketina. Selleks et paindlik, sooritab skripti järgmist.

1. Määrab, kus käivitati tee.
2. Läbib tee kuni leiab kausta nimega `packages`. Selle kausta loomist Nugeti pakettide Visual Studio projektid installimisel.
3. Rekursiivselt otsingud on `packages` komplekti **Microsoft.Azure.NotificationHubs.dll**nimega kaust.
4. Viited selle komplekti nii, et tüübid on hilisemaks kasutamiseks.

Siin on, kuidas rakendatakse järgmist PowerShelli skripti:

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a>Klassi NamespaceManager loomine

Ettevalmistamise teatis jaoturi, luua SDK [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) klassi eksemplar. 

Saate kaasas Azure PowerShelli cmdleti [Get-AzureSBAuthorizationRule] autoriseerimine reegel, mida kasutatakse ühendusstringi toomiseks. Viite saab salvestada selle `NamespaceManager` astme soovitud `$NamespaceManager` muutujana. Kasutame `$NamespaceManager` ettevalmistamise teate jaoturi.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Ettevalmistamise uus teatis keskus 

Ettevalmistamise uute teate jaoturi, kasutage [.NET API teatis jaoturi].

See osa skripti häälestamist neli kohalikku muutujat. 

1. `$Namespace`: Valige see nimeruumi nimi, kuhu soovite teatise keskuse loomine.
2. `$Path`: Määrake selle tee nimi uue teate jaoturi.  Näiteks "MyHub".    
3. `$WnsPackageSid`: Määrata paketi SID saate Windowsi rakenduse [Windowsi Arenduskeskus](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Määrata salajane võti teie Windowsi rakenduse jaoks [Windowsi Arenduskeskus](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Neid muutujaid kasutatakse ühenduse loomiseks oma nimeruum ja luua uus teatis jaoturi konfigureeritud toime WNS mandaati Windowsi rakenduse Windowsi teatise Services (WNS) teatised. Teavet saada paketi SID ja salajane võti kuvamiseks õpetusega [Alustamine teatis jaoturi](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) . 

+ Skripti koodilõigu kasutab funktsiooni `NamespaceManager` objekti, et kontrollida, kui teate jaoturi tähistatud `$Path` olemas.

+ Kui see pole, skripti loomiseks on `NotificationHubDescription` koos WNS identimisteabe ja edasi, et soovite selle `NamespaceManager` klassi `CreateNotificationHub` meetod.

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a>Lisaressursid

- [Hallata teenuse siini PowerShelliga](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
- [Kuidas luua teenuse järjekorrad, teemade ja tellimuste PowerShelli skripti abil](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Kuidas luua teenuse siini Namespace ja sündmuse jaoturi, mis PowerShelli skripti abil](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Mõned valmis skriptid on ka allalaadimiseks saadaval.
- [Teenus siini PowerShelli skriptide](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)
 

[Ostmise võimalused]: http://azure.microsoft.com/pricing/purchase-options/
[Liikme pakkumised]: http://azure.microsoft.com/pricing/member-offers/
[Tasuta prooviversioon]: http://azure.microsoft.com/pricing/free-trial/
[Installida ja konfigureerida Azure PowerShell]: ../powershell-install-configure.md
[.Net-i API teatis jaoturi]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
 
