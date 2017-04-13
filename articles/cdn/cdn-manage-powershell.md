<properties
    pageTitle="Azure'i CDN PowerShelliga haldamine | Microsoft Azure'i"
    description="Saate teada, kuidas hallata Azure CDN Azure PowerShelli cmdlet-käskude abil."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="casoper"/>


# <a name="manage-azure-cdn-with-powershell"></a>Azure'i CDN PowerShelliga haldamine

PowerShelli pakub kõige paindlik meetodite abil hallata oma Azure'i CDN profiilid ja lõpp-punktid.  Saate PowerShelli interaktiivseks või kirjutades skriptide haldamisega seotud toiminguid automatiseerida.  Selle õpetuse näitab, mitu enamlevinud tööülesanded saate täita PowerShelli abil hallata oma Azure'i CDN profiilid ja lõpp-punktid.

## <a name="prerequisites"></a>Eeltingimused

PowerShelli abil saate hallata oma Azure'i CDN profiilid ja lõpp-punktid, peab teil olema installitud Azure PowerShelli moodul.  Saate teada, kuidas installida Azure PowerShelli ja ühenduse Azure'i abil soovitud `Login-AzureRmAccount` cmdlet-käsk, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

>[AZURE.IMPORTANT] Peate sisse logima `Login-AzureRmAccount` enne täidate Azure PowerShelli cmdlet-käsud.

## <a name="listing-the-azure-cdn-cmdlets"></a>Loetelu Azure'i CDN cmdlet-käsud

Saate loetleda kõik Azure'i CDN cmdlet-käskude abil soovitud `Get-Command` cmdlet-käsk.

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a>Abi otsimine

Saate mis tahes nende cmdlettide kasutamise abi selle `Get-Help` cmdlet-käsk.  `Get-Help`kasutus- ja süntaks sisaldab ja soovi korral kuvab näited.

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Loendi olemasoleva Azure'i CDN profiilid

Funktsiooni `Get-AzureRmCdnProfile` cmdlet ilma parameetrid toob kõik olemasoleva CDN profiilid.

```powershell
Get-AzureRmCdnProfile
```

Selle väljundi võib veetorustiku cmdletid loendamine jaoks.

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

Saate naasta ka üks profiil profiili nimi ja ressursside rühma määramisega.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

>[AZURE.TIP] On võimalik on mitu CDN profiili sama nimega nii kaua, kui nad on erinevate ressursside rühmad.  Jättes selle `ResourceGroupName` parameetri tagastab kõik profiilid kattuvad nimega.

## <a name="listing-existing-cdn-endpoints"></a>Loendi olemasoleva CDN lõpp-punktid

`Get-AzureRmCdnEndpoint`saate tuua üksikute lõpp-punkti või kogu selle lõpp-punktid profiilis.  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>CDN profiilid ja lõpp-punktid loomine

`New-AzureRmCdnProfile`ja `New-AzureRmCdnEndpoint` loomiseks CDN profiilid ja lõpp-punktid.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Lõpp-punkti nimi saadavuse vaatamine

`Get-AzureRmCdnEndpointNameAvailability`tagastab objekti, mis näitab, kas mõni lõpp-punkti nimi on saadaval.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Kohandatud domeeni lisamist

`New-AzureRmCdnCustomDomain`lisab kohandatud domeeninime olemasoleva lõpp.

>[AZURE.IMPORTANT] CNAME-i peab häälestada nii, nagu on kirjeldatud [Kuidas kaardil sisu kohaletoimetamise võrk (CDN) lõpp-punkti kohandatud domeeni](./cdn-map-content-to-custom-domain.md)DNS-i teenusepakkuja juures.  Vastenduse enne muutmine lõpp-punkti abil saate testida `Test-AzureRmCdnCustomDomain`.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Lõpp-punkti muutmine

`Set-AzureRmCdnEndpoint`muudab olemasoleva lõpp.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Puhastamine/Pre-loading CDN varad

`Unpublish-AzureRmCdnEndpointContent`vahemälus talletatud likvideerimised varad, samal ajal `Publish-AzureRmCdnEndpointContent` eelnevalt laadib vara toetatud lõpp-punktid.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Käivitamine ja peatamine CDN lõpp-punktid
`Start-AzureRmCdnEndpoint`ja `Stop-AzureRmCdnEndpoint` saab kasutada käivitamine ja peatamine üksikute lõpp-punktid või rühmad, lõpp-punktid.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>CDN ressursid kustutamine

`Remove-AzureRmCdnProfile`ja `Remove-AzureRmCdnEndpoint` saab eemaldada profiilid ja lõpp-punktid.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Järgmised sammud

Saate teada, kuidas Azure'i CDN [.net-i](./cdn-app-dev-net.md) või [Node.js](./cdn-app-dev-node.md)automatiseerimiseks.

CDN funktsioonide kohta leiate teemast [CDN ülevaade](./cdn-overview.md).


