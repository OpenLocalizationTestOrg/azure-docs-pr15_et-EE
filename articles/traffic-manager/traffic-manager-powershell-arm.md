<properties
    pageTitle="Azure'i ressursihaldur liikluse haldurit toetatakse | Microsoft Azure'i "
    description="Jaoks liikluse haldur Azure ressursihaldur PowerShelli abil"
    services="traffic-manager"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="azure-resource-manager-support-for-azure-traffic-manager"></a>Azure'i ressursihaldur tugi Azure liikluse haldur

Azure'i ressursihaldur on eelistatud haldamise liidest Azure teenuste jaoks. Azure'i ressursihaldur vastavalt API-d ja tööriistade abil saab hallata Azure liikluse haldur profiilid.

## <a name="resource-model"></a>Ressursi mudel

Azure'i liikluse haldur on konfigureeritud kogumi nimega liikluse haldur profiili sätete abil. Seda profiili sisaldab DNS-i sätted, liikluse marsruutimise sätted, lõpp-punkti jälgimisega seotud sätted ja teenuse lõpp-punktid, kus liiklus marsruuditakse loendit.

Ressursi tüüpi 'TrafficManagerProfiles' esindab iga liikluse haldur profiili. Tasemel REST API URI iga reegli puhul on järgmine:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="comparison-with-the-azure-traffic-manager-classic-api"></a>Azure'i liikluse haldur klassikaline API võrdlus

Azure'i ressursihaldur liikluse haldurit toetatakse kasutab eri terminoloogia kui klassikaline juurutamise mudel. Järgmine tabel sisaldab sõnu ressursihaldur ja klassikaline erinevused.

| Ressursihaldur termin | Klassikaline termin |
|-----------------------|--------------|
| Liikluse marsruutimist meetod | Koormust tasakaalustavad meetod |
| Priority (prioriteet) meetod | Tõrkesiirde meetod |
| Kaalutud meetod | Round-jaan meetod |
| Jõudluse meetod | Jõudluse meetod |

Klientide tagasiside põhjal oleme muutnud selgust ja vähendamiseks levinud arusaamatusi terminid. Ei erine funktsioone.

## <a name="limitations"></a>Piirangud

Kui viitate lõpp-punkti tüüp 'AzureEndpoints' Web Appi, ainult viitamiseks liikluse haldur lõpp-punktid vaikimisi (tootmine) [Web Appi pesa](../app-service-web/web-sites-staged-publishing.md). Kohandatud slots ei toetata. Lahendusena saate konfigureerida kohandatud slots 'ExternalEndpoints' tüüp.

## <a name="setting-up-azure-powershell"></a>Azure'i PowerShelli seadistamine

Need juhised kasutada Microsoft Azure PowerShelli. Järgmises artiklis selgitatakse, kuidas installida ja konfigureerida Azure PowerShelli.

- [Kuidas installida ja konfigureerida Azure PowerShell](../powershell-install-configure.md)

Selle artikli näited endale ressursi olemasolevasse rühma. Saate luua ressursirühma abil järgmine käsk:

```powershell
    New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

>[AZURE.NOTE] Azure'i ressursihaldur eeldab, et kõigi asukoht. Selle asukoha on vaikimisi kasutatakse ressursse, mis on loodud selle ressursirühma. Kuna liikluse haldur profiili ressursid on globaalne, mitte piirkondliku, ressursside rühma asukoha valik on siiski ei mõjuta Azure'i liikluse haldur.

## <a name="create-a-traffic-manager-profile"></a>Liikluse haldur profiili loomine

Liikluse haldur profiili loomine kasutage cmdletti New-AzureRmTrafficManagerProfile:

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

Järgmises tabelis kirjeldatakse parameetrid.

| Parameetri | Kirjeldus |
|-----------|-------------|
| Nimi | Ressursinimi liikluse haldur profiili ressursi. Sama ressursi jaotises Profiilid peab olema kordumatu nimi. See nimi on eraldi DNS-i päringuid DNS-i nimi.|
| ResourceGroupName | Profiili ressurssi sisaldava ressursirühma nimi.|
| TrafficRoutingMethod | Saate määrata, millist lõpp-punkti tagastatakse vastuse DNS-i päringu määratlemiseks liikluse marsruutimist meetod. Võimalikud väärtused on "Jõudlus", "Kaalutud" või "Prioriteet".|
| RelativeDnsName | Saate määrata DNS-i nimi, mis on esitatud selle liikluse haldur profiili hostname osa. See väärtus on koos DNS-i domeeni nimi, mida kasutatakse Azure liikluse halduri vormi profiili täielik domeeninimi (FQDN). Näiteks "contoso" väärtuse seadmisel muutub "contoso.trafficmanager.net."|
| TTL | Määrab selle DNS-i aeg-to-Live (TTL), sekundites. See TTL teatab kohaliku DNS-i selsüünid ja DNS-i kliendid, kui kaua vahemälu DNS-i vastuste selle liikluse haldur profiili.|
| MonitorProtocol | Määrab protokolli abil lõpp-punkti seisundi jälgimine. Võimalikud väärtused on "HTTP" ja "HTTPS".|
| MonitorPort | Saate määrata TCP pordi kasutatakse lõpp-punkti seisundi jälgimine.|
| MonitorPath | Saate määrata tee lõpp-punkti domeeni nimi, mida kasutatakse probe lõpp-punkti seisundi suhtes.|

Cmdlet loob liikluse haldur profiili Azure ja tagastab vastava profiili objekti PowerShelli. Selles etapis ei sisalda profiili mis tahes lõpp-punktid. Lõpp-punktid liikluse haldur profiili lisamise kohta leiate lisateavet teemast [Lisamist liikluse haldur lõpp-punktid](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Liikluse haldur profiili hankimine

Olemasoleva liikluse haldur profiili objekti toomiseks kasutada Get-AzureRmTrafficManagerProfle cmdlet-käsk:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Selle cmdlet-käsk tagastab liikluse haldur profiili objekt.

## <a name="update-a-traffic-manager-profile"></a>Liikluse haldur profiili värskendamine

3-sammult muutmine liikluse haldur profiilid järgmiselt:

1. Get-AzureRmTrafficManagerProfile abil profiili tuua või kasutada tagastatud New-AzureRmTrafficManagerProfile profiili.
2. Profiili muutmiseks. Saate lisada ja eemaldada lõpp-punktid või lõpp-punkti või profiili parameetrite muutmine. Need muudatused on ühenduseta toimingud. Muudate ainult kohaliku objekti mällu, mis tähistab profiili.
3. Cmdlet-käsu Set-AzureRmTrafficManagerProfile abil muudatuste kinnitamiseks.

Kõik kasutajaprofiili atribuute saab muuta profiili RelativeDnsName peale. Funktsiooni RelativeDnsName muutmiseks peate kustutama profiili ja uue nimega uus profiil.

Järgmises näites näitab, kuidas muuta profiili TTL:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    $profile.Ttl = 300
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

On kolme tüüpi liikluse haldur lõpp-punktid.

1. **Azure'i lõpp-punktid** on majutatud Azure teenused
2. **Väliste lõpp-punktid** on majutatavale Azure'i teenused
3. Pesastatud hierarhiad liikluse haldur profiilide ehitada kasutatakse **pesastatud lõpp-punktid** . Pesastatud lõpp-punktid lubada täpsema liikluse marsruutimist konfiguratsioone keerukate rakenduste.

Kõigi kolme juhtudel lõpp-punktid saab lisada kahel viisil:

1. 3-sammult eespool kirjeldatud abil. Selle meetodi eeliseid on, et mitu lõpp-punkti muudatusi saate teha ühte värskendus.
2. Cmdlet-käsu New-AzureRmTrafficManagerEndpoint abil. Selle cmdlet-käsu lisab lõpp liikluse haldur olemasolevale profiilile ühekordne.

## <a name="adding-azure-endpoints"></a>Azure'i lõpp-punktid lisamine

Azure'i lõpp-punktid viide Azure teenuseid. Kolme tüüpi Azure lõpp-punktid on toetatud:

1. Azure'i Web Apps
2. "Klassikaline" cloud services (mis võib sisaldada PaaS teenuse või IaaS virtuaalmasinates)
3. Azure'i PublicIpAddress ressursid (mis võivad olla seotud laadi-koormusetasakaalustusteenuse või virtuaalse masina NIC). Funktsiooni PublicIpAddress peab olema määratud kasutada liikluse haldur DNS-i nimi.

Iga kord:

- Teenus on määratud lisa-AzureRmTrafficManagerEndpointConfig või New-AzureRmTrafficManagerEndpoint parameetriga "targetResourceId".
- "Target" ja 'EndpointLocation' Vaikimisi on TargetResourceId.
- Täpsustatakse "kaal" pole kohustuslik. Kaalu kasutatakse ainult siis, kui profiili on konfigureeritud kasutama "Kaalutud" liikluse marsruutimist meetod. Muul juhul need ignoreeritakse. Kui määratud, väärtus peab olema arv vahemikus 1 – 1000. Vaikeväärtus on '1'.
- Täpsustatakse 'prioriteet"pole kohustuslik. Prioriteedid kasutatakse ainult siis, kui profiili on konfigureeritud kasutama "Prioriteet" liikluse marsruutimist meetod. Muul juhul need ignoreeritakse. Sobivad väärtused on 1 1000 kõrgem prioriteet, mis näitab, lower väärtustega. Kui määratud üks lõpp-punkti, peab olema määratud kõigi lõpp-punktide jaoks. Kui välja jäetud, rakendatakse alates '1' vaikeväärtused, et lõpp-punktid on ära toodud järjestuses.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Näide 1: Lisamise abil lisa-AzureRmTrafficManagerEndpointConfig Web Appi lõpp-punktid

Selles näites liikluse haldur profiili loomine ja lisamine kahe Web Appi lõpp-punktid lisa-AzureRmTrafficManagerEndpointConfig cmdlet-käsu abil.

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $webapp1 = Get-AzureRMWebApp -Name webapp1
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
    $webapp2 = Get-AzureRMWebApp -Name webapp2
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-a-classic-cloud-service-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Näide 2: Lisamine on "klassikalisel" pilve teenuse lõpp-punkti New-AzureRmTrafficManagerEndpoint abil

Selles näites on "klassikalisel" pilve teenuse lõpp-punkti liikluse haldur profiilile lisanud. Selles näites me määratud profiil profiili- ja ressursihalduse rühma nime abil, mitte läbides profiili objekti. Mõlema lähenemisel on toetatud.

    $cloudService = Get-AzureRmResource -ResourceName MyCloudService -ResourceType "Microsoft.ClassicCompute/domainNames" -ResourceGroupName MyCloudService
    New-AzureRmTrafficManagerEndpoint -Name MyCloudServiceEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $cloudService.Id -EndpointStatus Enabled

### <a name="example-3-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Näide 3: Lisamise publicIpAddress lõpp-punkti New-AzureRmTrafficManagerEndpoint abil

Selles näites on lisatud avaliku IP-aadressi ressursi liikluse haldur profiili. Avaliku IP-aadressi peab olema konfigureeritud DNS-i nimi ja saate seotud NIC VM või laadi koormusetasakaalustusteenuse.

    $ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled

## <a name="adding-external-endpoints"></a>Väliste lõpp-punktide lisamine

Liikluse haldur kasutab väliste lõpp-punktide juurde majutatavale Azure'i teenustele-liikluse suunamiseks. Sarnaselt Azure lõpp-punktid, saab lisada väliste lõpp-punktid, kasutades lisa-AzureRmTrafficManagerEndpointConfig järgneb Set-AzureRmTrafficManagerProfile või New-AzureRMTrafficManagerEndpoint.

Kui soovite määrata välise lõpp-punktid:

- Lõpp-punkti domeeninimi peab olema määratud parameetriga "Target"
- Kui "Jõudlus" liikluse marsruutimist meetodi kasutamise 'EndpointLocation' on nõutav. Muul juhul pole kohustuslik. Väärtus peab olema [lubatud Azure regiooni nimi](https://azure.microsoft.com/regions/).
- "Kaal" ja "Prioriteet" on valikulised.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Näide 1: Lisamise väliste lõpp-punktide lisa-AzureRmTrafficManagerEndpointConfig ja Set-AzureRmTrafficManagerProfile abil

Selles näites me liikluse haldur profiili loomine, lisage kaks väliste lõpp-punktid ja muudatuste kinnitamine.

    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Näide 2: Lisamise väliste lõpp-punktide New-AzureRmTrafficManagerEndpoint abil

Selles näites lisame väliste lõpp olemasolevale profiilile. Profiili on määratud profiili- ja ressursihalduse rühma nime abil.

    New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled

## <a name="adding-nested-endpoints"></a>Lisades 'Pesastatud' lõpp-punktid

Iga liikluse haldur profiili saate määrata ühe liikluse marsruutimist meetod. Kuid on stsenaariumid, mis nõuavad keerukamaid liikluse marsruutimist suurem marsruudi liikluse haldur üks profiil. Saate pesastada liikluse haldur profiilid ühendada mitu liikluse marsruutimist meetodit kasu. Pesastatud profiilid võimaldavad alistada tugi suuremad ja keerukamaid rakenduse kasutuselevõttu liikluse haldur vaikekäitumise. Üksikasjalikumat näiteid leiate [pesastatud liikluse haldur profiilid](traffic-manager-nested-profiles.md).

Pesastatud lõpp-punktid on konfigureeritud ema profiili, kindla lõpp-punkti tüüp, "NestedEndpoints". Kui soovite määrata pesastatud lõpp-punktid:

- Lõpp-punkti peab olema määratud parameetriga "targetResourceId"
- Kui "Jõudlus" liikluse marsruutimist meetodi kasutamise 'EndpointLocation' on nõutav. Muul juhul pole kohustuslik. Väärtus peab olema [lubatud Azure regiooni nimi](http://azure.microsoft.com/regions/).
- "Kaal" ja "Prioriteet" on valikuline, nagu Azure lõpp-punktid.
- Parameetri "MinChildEndpoints" pole kohustuslik. Vaikeväärtus on '1'. Kui see piirmäärast saadaval lõpp-punktide arv, leiab ema profiili lapse profiili 'halvenenud ja muude lõpp-punktid profiili ema-liikluse suunab.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Näide 1: Lisamise pesastatud lõpp-punktid lisa-AzureRmTrafficManagerEndpointConfig ja Set-AzureRmTrafficManagerProfile abil

Selles näites me looma uue liikluse haldur laps ja vanem profiilid, lisada lapse pesastatud lõpp-punkti ema ja muudatuste kinnitamine.

    $child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

Lühike selles näites, ei ole lisame mis tahes lõpp-punktid lapse või ema profiilid.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Näide 2: Lisamise abil uus-AzureRmTrafficManagerEndpoint pesastatud lõpp-punktid

Selles näites me lisada lapse olemasolevale profiilile pesastatud lõpp-punkti ema olemasolevale profiilile. Profiili on määratud profiili- ja ressursihalduse rühma nime abil.

    $child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2

## <a name="update-a-traffic-manager-endpoint"></a>Liikluse haldur Endpoint värskendamine

On kaks võimalust värskendada olemasoleva liikluse haldur lõpp.

1. Saada liikluse haldur profiili abil Get-AzureRmTrafficManagerProfile, lõpp-punkti atribuutide sees profiili värskendamine ja Set-AzureRmTrafficManagerProfile abil muudatuste kinnitamine. See meetod on see eelis, ei saaks seda rohkem kui üks lõpp-punkti ühekordne värskendada.
2. Saada liikluse haldur lõpp-punkti abil Get-AzureRmTrafficManagerEndpoint, lõpp-punkti atribuutide värskendamine ja Set-AzureRmTrafficManagerEndpoint abil muudatuste kinnitamine. See meetod on lihtsam, kuna see ei nõua indekseerimise üheks profiili massiiv lõpp-punktid.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Näide 1: Värskendamine lõpp-punktid Get-AzureRmTrafficManagerProfile ja Set-AzureRmTrafficManagerProfile abil

Selles näites me muuta kahe lõpp-punktid sees olemasolevale profiilile prioriteet.

    $profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
    $profile.Endpoints[0].Priority = 2
    $profile.Endpoints[1].Priority = 1
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Näide 2: Värskendamine lõpp Get-AzureRmTrafficManagerEndpoint ja Set-AzureRmTrafficManagerEndpoint abil

Selles näites me muuta ühe lõpp-punkti sisse olemasolevale profiilile paksuse.

    $endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
    $endpoint.Weight = 20
    Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Lubamine ja keelamine lõpp-punktid ja profiilid

Liikluse haldur võimaldab üksikute lõpp-punktid lubatud ja keelatud, samuti lubamisel lubamine ja keelamine kogu profiilid.
Need muudatused saate tehtud töö/värskendamine/säte lõpp-punkti või profiili ressursid. Neid levinud toiminguid täiustamiseks need on toetatud ka asjakohast cmdlettide kaudu.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Näide 1: Lubamine ja keelamine profiili liikluse haldur

Liikluse haldur profiili, kasutage luba-AzureRmTrafficManagerProfile. Profiili saab määrata profiili objekti abil. Profiili objekti saab edasi tulemas kaudu või kasutades funktsiooni "-TrafficManagerProfile" parameeter. Selles näites me määramiseks profiil profiili- ja ressursihalduse rühma nime.

```powershell
    Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Liikluse haldur profiili keelamiseks tehke järgmist.

```powershell
    Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Keela-AzureRmTrafficManagerProfile cmdlet, mis palub kinnitust. See viip võib olla rõhutud abil soovitud "-Jõusta" parameeter.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Näide 2: Lubamine ja keelamine liikluse haldur endpoint

Kasutage liikluse haldur endpoint lubamiseks luba-AzureRmTrafficManagerEndpoint. On kaks võimalust, et määrata lõpp-punkti

1. Kasutades TrafficManagerEndpoint objekt, kaudu tulemas on möödas või selle '-TrafficManagerEndpoint' parameeter
2. Lõpp-punkti nimi, lõpp-punkti tüüp, profiili nimi ja ressursside rühma nime abil:

```powershell
    Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Samuti liikluse haldur endpoint keelamine:

```powershell
     Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Keela AzureRmTrafficManagerProfile, sarnaselt Keela-AzureRmTrafficManagerEndpoint cmdlet, mis palub kinnitust. See viip võib olla rõhutud abil soovitud "-Jõusta" parameeter.

## <a name="delete-a-traffic-manager-endpoint"></a>Mõne liikluse haldur lõpp-punkti kustutamine

Kui soovite eemaldada üksikuid lõpp-punktid, kasutage Eemalda-AzureRmTrafficManagerEndpoint cmdletti:

```powershell
    Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Selle cmdlet, mis palub kinnitust. See viip võib olla rõhutud, kasutades funktsiooni "-Jõusta ' parameeter.

## <a name="delete-a-traffic-manager-profile"></a>Liikluse haldur profiili kustutamine

Liikluse haldur profiili kustutamiseks cmdletiga Eemalda-AzureRmTrafficManagerProfile, täpsustades profiili- ja ressursihalduse rühma nimed:

```powershell
    Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Selle cmdlet, mis palub kinnitust. See viip võib olla rõhutud abil soovitud "-Jõusta" parameeter.

Profiili kustutada saab määrata ka profiili objekti abil:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Samuti saate järjestusel veetorustiku:

```powershell
    Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Järgmised sammud

[Liikluse haldur jälgimine](traffic-manager-monitoring.md)

[Liikluse haldur jõudluse huvides](traffic-manager-performance-considerations.md)
