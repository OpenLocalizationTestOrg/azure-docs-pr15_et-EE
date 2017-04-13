<properties
   pageTitle="Näide DMZ-koostada lihtsaid DMZ koos NSGs | Microsoft Azure'i"
   description="Koostamine on DMZ koos võrgu turberühmad (NSG)"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor;sivae"/>

# <a name="example-1--build-a-simple-dmz-with-nsgs"></a>Näide 1 – koostada lihtsaid DMZ NSGs abil

[Tagasi lehele Turve piiri head tavad][HOME]

Selles näites loob lihtsa DMZ neli windows serverite ja võrgu turberühmad. See ka juhendab iga oluline käsud süvitsi mõistmise iga toimingu esitada. Olemas on ka liikluse stsenaarium jaotise esitada põhjalikult samm-sammult, kuidas liikluse tulu kaitse DMZ kihid läbi. Lõpuks viited jaotis on lõpule viidud kood ja juhiseid, et koostada selles keskkonnas testimiseks ja katsetage erinevaid võimalusi. 

![Sissetulev DMZ NSG abil][1]

## <a name="environment-description"></a>Keskkonna kirjeldus
Selles näites on tellimus, mis sisaldab järgmist:

- Kaks pilveteenused: "FrontEnd001" ja "BackEnd001"
- Virtuaalse võrk, "CorpNetwork" koos kahe alamvõrku; "FrontEnd" ja "Taustväärtus"
- Mis on rakendatud nii alamvõrku võrgu turberühma
- Windows Server, mis tähistab rakenduse veebiserverisse ("IIS01")
- Kaks windows serverite, mis tähistab rakenduse uuesti end serverid ("AppVM01", "AppVM02")
- Windows server, mis tähistab DNS server ("DNS01")

All viidetega jaotises on PowerShelli skripti, mis põhineb enamik keskkond, mis on kirjeldatud ülal. Koostamise VMs ja virtuaalne võrkude, kuigi on valmis näide skripti, on pole üksikasjalikult kirjeldatud selles dokumendis. 

Koostamiseks keskkonnale;

  1.    Viited jaotises võrku config XML-faili salvestamine (uuendatud nimed, asukoht ja IP-aadressid vastavad antud stsenaarium)
  2.    Värskendage kasutaja muutujate skripti vastavaks script on käivitamiseks suhtes (tellimuste teenuste nimed, jne)
  3.    PowerShelli skripti käivitada

**Märkus**: sellele PowerShelli skripti piirkonna peab vastama piirkonna sellele võrgu konfiguratsiooni XML-failis.

Kui skript käivitatakse edukalt lisatoimingud valikuline võib võtta Viited jaotise on kaks skriptide veebiserverisse ja rakenduse serveris, millel on lihtne veebirakendus testimine selle DMZ konfiguratsiooni lubama häälestamiseks.

Järgmistes jaotistes on võrgu turberühmad ja kuidas need funktsioon selle näite puhul kõndides võtme jooned PowerShelli skripti kaudu üksikasjalikku kirjeldust.

## <a name="network-security-groups-nsg"></a>Võrgu turberühmad (NSG)
Selle näite puhul on NSG rühma ehitatud ja seejärel koormatud kuus reeglid. 

>[AZURE.TIP] Üldiselt teatud "Luba" reeglite tuleks esmalt looma ja seejärel viimati rohkem üldine "Keela" reegleid. Mis on määratud prioriteet dikteerib hinnatud esimene. Kui liikluse on leitud kindla reegli rakendamiseks, väärtustatakse pole veel reeglid. Kas sissetulevaid või väljaminevaid suunas (alamvõrgu seisukohast) saate rakendada NSG reeglid.

Deklaratiivse, ehitatakse sissetulev liiklus järgmisi reegleid:

1.  Sisemise DNS-i liikluse (port 53) on lubatud
2.  RDP-liikluse (port 3389) Interneti kaudu mis tahes VM on lubatud
3.  HTTP-liikluse (port 80) Interneti kaudu veebiserverisse (IIS01) on lubatud
4.  Mis tahes liikluse (kõik pordid) kaudu IIS01 AppVM1 on lubatud
5.  Mis tahes liikluse (kõik pordid) Interneti kaudu kogu VNet (nii alamvõrku) on keelatud
6.  Mis tahes liiklus (kõik pordid) Frontend alamvõrgu Taustväärtus alamvõrgu on keelatud

Reegleid kindlasti iga alamvõrku, kui HTTP-päring on sissetuleva Interneti kaudu veebiserverisse, nii reeglite 3 (luba) ja 5 (keelamiseks) soovite rakendada, kuid kuna reegli 3 on kõrgem prioriteet ainult seda ja reegli 5 ei tule mängu. Seega veebiserverisse lubatud HTTP-päring. Kui selle sama liikluse üritab DNS01 serverisse, reegel 5 (Keela) oleks esmalt rakendamiseks ja liiklust oleks lubatud edastamiseks server. Reegli 6 (Keela) blokeerib Frontend alamvõrgu räägib kirjutamata alamvõrgu (v.a lubatud liikluse reeglite 1 – 4), see kaitseb kirjutamata võrgu juhul ründaja kompromisse, mis on Frontend, ründaja veebirakenduse on piiratud juurdepääsu kirjutamata "kaitstud" (ainult seostuvatele ressurssidele esitatud AppVM01 serveris).

On vaikimisi Väljaminev reegel, mis lubab liikluse välja Interneti-ühendus. Selle näite puhul me lubamisel väljaminev liiklus ja pole muutmine mis tahes väljaminev reeglid. Lukustada liikluse mõlemast suunast, kasutaja määratletud marsruutimine on nõutavad, see on uurida "Näite 3" all.

Iga reegli puhul käsitletakse üksikasjalikumalt järgmiselt (**Märkus**: mõnda üksust selle all loendi alguses koos dollarimärk (nt: $NSGName) on kasutaja määratletud muutuja skripti viide jaotises selle dokumendi):

1. Kõigepealt tuleb võrgu turberühma ehitada ootelepanekuks reeglite:

        New-AzureNetworkSecurityGroup -Name $NSGName `
            -Location $DeploymentLocation `
            -Label "Security group for $VNetName subnets in $DeploymentLocation"
 
2.  Selles näites esimese reegli võimaldab DNS-i liikluse vahel kõik sisemise DNS-i server alamvõrgu taustväärtus. Reegel on mõned olulised parameetrid.
  - "Tüüp", tähendab see, millises suunas liikluse selle reegli jõustub; See on alamvõrgu või virtuaalse masina seisukohast (sõltuvalt sellest, kui seob selle NSG). Seega kui tüüp on "Sissetulev" ja liiklust on sisestamise alamvõrgu, soovite reeglit rakendada ja ei mõjuta liiklust alamvõrgu jättes selle reegli alusel.
  - "Prioriteet" märkige liikluse kulgemist hinnatakse. Madalam arv, mida suurem prioriteet. Kui reegel rakendub teatud liiklust, pole veel reegleid töödeldakse. Seega kui reegli prioriteedi 1 võimaldab liikluse, ja reegli prioriteedi 2 keelab liikluse, ja nii reeglite rakendamine liikluse siis liiklus lubatud flow (kuna reegli 1 olnud kulus efekti ja pole veel reeglid on rakendatud).
  - "Toiming" tähendab, et kui liikluse mõjuta see reegel on blokeeritud või lubatud.

            Get-AzureNetworkSecurityGroup -Name $NSGName | `
                Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
                -Type Inbound -Priority 100 -Action Allow `
                -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
                -DestinationAddressPrefix $VMIP[4] `
                -DestinationPortRange '53' `
                -Protocol *

3.  See reegel võimaldab RDP-liikluse tehingust Interneti kaudu mis tahes server on VNET kummagi alamvõrgu RDP-porti. See reegel kasutab kahte teisiti tüüpi aadress eesliiteid; "VIRTUAL_NETWORK" ja "INTERNET". See on lihtne viis aadress aadress eesliiteid suuremat kategooria.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" `
            -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '3389' `
            -Protocol *

4.  See reegel võimaldab sissetuleva Interneti-liikluse tabas veebiserver. See ei muuda marsruutimise käitumine; See võimaldab liikluse mõeldud IIS01 edasi. Seega kui liikluse Interneti kaudu oli see reegel oleks lubada ja Lõpeta reeglite töötlemine sihtkohta veebiserver. (Selle reegli prioriteet 140 kõik muud sissetulevate Interneti-liikluse on blokeeritud). Kui teil on ainult HTTP liikluse, võib see reegel täpsemaks piiratud lubama ainult sihtkoha Port 80.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable Internet to $VMName[0]" `
            -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] `
            -DestinationPortRange '*' `
            -Protocol *

5.  See reegel võimaldab liiklus kõik muud Frontend kirjutamata liikluse AppVM01 server, hiljem reegli plokid serverist IIS01 edasi. See reegel parandada, kui pordi on teada, mis tuleb lisada. Näiteks kui IIS-i server on pihta ainult SQL serveri AppVM01, sihtkoha Port vahemikus muuta oma "*" (kõik) abil 1433 (SQL-i port), võimaldades väiksema sissetulevate rünnak pinna AppVM01 kohta veebirakenduse kunagi kahjustada.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] to $VMName[2]" `
            -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
 
6.  Selle reegliga saab liikluse Interneti kaudu mis tahes serverite võrgu. Kombinatsioonis reegli juures prioriteet 110 ja 120 võimaldab ainult sissetuleva Interneti-liikluse tulemüüri ja RDP pordid teiste serverite ja plokid veel. 

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $VNetName VNet from the Internet" `
            -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '*' `
            -Protocol *
 
7.  Lõplik reegel keelab liikluse kaudu Frontend alamvõrgu Taustväärtus alamvõrgu. Kuna see on ainult sissetuleva reegli, vastupidise liikluse on lubatud (Taustväärtus, et on Frontend).

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" `
            -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix `
            -DestinationPortRange '*' `
            -Protocol * 

## <a name="traffic-scenarios"></a>Liikluse stsenaariumid
#### <a name="allowed-web-to-web-server"></a>(*Lubatud*) Web veebiserver
1.  Interneti kasutaja taotlused HTTP lehel FrontEnd001.CloudApp.Net (Internet vastastikuste pilveteenus)
2.  Pilvepõhine teenus läbib liikluse kaudu avatud lõpp-punkti port 80 IIS01 suunas (veebiserver)
3.  Frontend alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (DNS) ei rakendada, järgmise reegli liikumine
  2.    NSG reegli 2 (RDP) ei rakendada, järgmise reegli liikumine
  3.    NSG reegli 3 (IIS01 internetist) rakendamine, liiklus on lubatud, peatage reeglite töötlemine
4.  Liikluse tabab sisemise IP-aadress veebiserver IIS01 (10.0.1.5)
5.  IIS01 on kuulamine web liikluse, saab see taotlus ja käivitab päringu töötlemise
6.  IIS01 küsitakse SQL serveri AppVM01 teavet
7.  Väljamineva reegleid Frontend alamvõrgu, liikluse on lubatud
8.  Taustväärtus alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (DNS) ei rakendada, järgmise reegli liikumine
  2.    NSG reegli 2 (RDP) ei rakendada, järgmise reegli liikumine
  3.    NSG reegli 3 (Internet Firewall) ei rakendada, teisaldamine järgmise reegli
  4.    NSG reegli 4 (IIS01, et AppVM01) rakendamine, liikluse on lubatud, reeglite töötlemise lõpetamine
9.  AppVM01 saab SQL-päringu ja vastab
10. Kuna ei ole väljamineva reegleid kirjutamata alamvõrgu vastus on lubatud
11. Frontend alamvõrgu algab Sissetulev reegel töötlemine:
  1.    Ei ole NSG reegel, mis kehtib sissetulev liiklus Taustväärtus alamvõrgu Frontend alamvõrku, nii et ükski selle NSG reeglite rakendamine
  2.    Vaikimisi süsteemi reegli lubamisel vahel alamvõrku võimaldaks see liiklus nii, et liiklus on lubatud.
12. IIS-i server saab vastuse SQL-i ja lõpetab HTTP vastus ja saadab imperatiivsus päringu
13. Kuna seal on reegleid ei väljaminev Frontend alamvõrgu vastus on lubatud ja Internet kasutaja saab veebilehele, mis on nõutav.

#### <a name="allowed-rdp-to-backend"></a>(*Lubatud*) Põhjused, mida kirjutamata
1.  Serveri administraator Internetis taotleb AppVM01 kohta, kus xxxxx on RDP AppVM01 (määratud port leiate Azure'i portaalis või PowerShelli kaudu) Kui soovite CEIP määratud pordinumber BackEnd001.CloudApp.Net:xxxxx RDP-seanss
2.  Taustväärtus alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (DNS) ei rakendada, järgmise reegli liikumine
  2.    NSG reegli 2 (RDP) rakendamine, liiklus on lubatud, peatage reeglite töötlemine
3.  Väljamineva reegleid ei vaikereeglit rakendada ja tagastada liikluse on lubatud
4.  RDP-seansi on lubatud
5.  AppVM01 küsib kasutajanime ja parooli

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(*Lubatud*) DNS-i serveris web serveri DNS-i otsing
1.  Web Server, IIS01, vajadustele andmekanali, www.data.gov juures, kuid vajadustele aadressi lahendamiseks.
2.  Võrgukonfiguratsioon VNet loendite DNS01 (10.0.2.4 kirjutamata alamvõrgu) esmane DNS-i server, IIS01 saadab DNS-i taotluse DNS01
3.  Väljamineva reegleid Frontend alamvõrgu, liikluse on lubatud
4.  Kirjutamata alamvõrgu algab Sissetulev reegel töötlemine:
  1.     NSG reegli 1 (DNS-i) rakendamine, liiklus on lubatud, peatage reeglite töötlemine
5.  DNS-serveri saab taotluse
6.  DNS-i server pole vahemälus talletatud aadressi ja palub juur DNS-i serveri Internet
7.  Väljamineva reegleid kirjutamata alamvõrgu, liiklust on lubatud
8.  Interneti DNS-i server vastab, kuna selle seansi käivitati ettevõttesiseselt, vastuse on lubatud
9.  DNS-serveri vahemälu vastus ja algse vastuseks naasmiseks IIS01
10. Väljamineva reegleid Taustväärtus alamvõrgu, liikluse on lubatud
11. Frontend alamvõrgu algab Sissetulev reegel töötlemine:
  1.    Ei ole NSG reegel, mis kehtib sissetulev liiklus Taustväärtus alamvõrgu Frontend alamvõrku, nii et ükski selle NSG reeglite rakendamine
  2.    Vaikimisi süsteemi reegli lubamisel vahel alamvõrku võimaldab see liiklus nii, et liiklus on lubatud
12. IIS01 saab vastuse DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Lubatud*) Web serveri juurdepääsu faili AppVM01
1.  IIS01 küsib AppVM01 faili
2.  Väljamineva reegleid Frontend alamvõrgu, liiklust on lubatud
3.  Kirjutamata alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (DNS) ei rakendada, järgmise reegli liikumine
  2.    NSG reegli 2 (RDP) ei rakendada, järgmise reegli liikumine
  3.    NSG reegli 3 (IIS01 internetist) ei rakendada, järgmise reegli liikumine
  4.    NSG reegli 4 (IIS01, et AppVM01) rakendamine, liikluse on lubatud, reeglite töötlemise lõpetamine
4.  AppVM01 saab taotluse ja vastab failiga (eeldades, et juurdepääs on lubatud)
5.  Kuna ei ole väljamineva reegleid kirjutamata alamvõrgu vastus on lubatud
6.  Frontend alamvõrgu algab Sissetulev reegel töötlemine:
  1.    Ei ole NSG reegel, mis kehtib sissetulev liiklus Taustväärtus alamvõrgu Frontend alamvõrku, nii et ükski selle NSG reeglite rakendamine
  2.    Vaikimisi süsteemi reegli lubamisel vahel alamvõrku võimaldaks see liiklus nii, et liiklus on lubatud.
7.  IIS-i server saab faili

#### <a name="denied-web-to-backend-server"></a>(*Keelatud*) Web kirjutamata serveris
1.  Interneti-kasutaja proovib juurde AppVM01 faili BackEnd001.CloudApp.Net teenuse kaudu
2.  Kuna see on avatud faili ühiskasutusse andmine pole lõpp-punkte, seda ei liigu pilveteenusesse ja ei jõua server
3.  Kui mingil põhjusel olid avatud lõpp-punktid, NSG reegli 5 (VNet internetist) blokeeritaks see liiklus

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(*Keelatud*) DNS-i serveris Web DNS-i otsing
1.  Interneti-kasutaja proovib otsingul sisemise DNS-i kirje kohta DNS01 BackEnd001.CloudApp.Net teenuse kaudu
2.  Kuna ei ole lõpp avatud DNS-i, seda ei liigu pilveteenusesse ja ei jõua server
3.  Kui mingil põhjusel olid avatud lõpp-punktid, NSG reegli 5 (VNet internetist) blokeeritaks see liiklus (Märkus: selle reegli 1 (DNS) ei kehti kahel põhjusel, esmalt allikas aadress on Interneti-ühendus, see reegel rakendub ainult kohaliku VNet allikana, ka see on reegli luba nii, et see oleks kunagi keelamiseks liikluse)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(*Keelatud*) SQL-i juurdepääs läbi tulemüüri Web
1.  Interneti-kasutaja taotleb SQL-i andmete FrontEnd001.CloudApp.Net (Internet vastastikuste pilveteenus)
2.  Kuna ei ole lõpp avatud SQL-i, seda ei liigu pilveteenusesse ja ei jõua tulemüüri
3.  Kui mingil põhjusel olid avatud lõpp-punktid, algab Frontend alamvõrgu Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (DNS) ei rakendada, järgmise reegli liikumine
  2.    NSG reegli 2 (RDP) ei rakendada, järgmise reegli liikumine
  3.    NSG reegli 3 (IIS01 internetist) rakendamine, liiklus on lubatud, peatage reeglite töötlemine
4.  Liikluse tabab sisemise IP-aadress on IIS01 (10.0.1.5)
5.  IIS01 pole listening port 1433, seega ei ole vastusest

## <a name="conclusion"></a>Kokkuvõte
See on üsna lihtne ja otse edasi viis tagasi end alamvõrgu sissetulev liiklus eraldamiseks.

Veel näiteid ja võrgu turvalisus piirmäärad ülevaade leiate [siit][HOME].

## <a name="references"></a>Viited
### <a name="main-script-and-network-config"></a>Peamised skripte ja võrgu Config
Täielik skripti PowerShelli skripti faili salvestada. Salvestage fail nimega "NetworkConf1.xml" võrgu Config.
Muutke kasutaja määratletud muutujaid vastavalt vajadusele. Skripti, siis järgige tulemüüri reegli häälestamine sisalduvad Ülaltoodud näites 1.

#### <a name="full-script"></a>Täielik skripti
Kuvatakse selle skripti, põhineb kasutaja määratletud muutujate;

1.  Ühenduse loomine Azure tellimuse
2.  Mäluruumi uue konto loomine
3.  Uue VNet ja kahe alamvõrku, nagu on määratletud võrgu Config faili loomine
4.  4 windows server VMs koostamine
5.  Saate konfigureerida, sh NSG.
  - Mõne NSG loomine
  - Ilma vaevata luua reeglite abil
  - Sobiv alamvõrku on NSG sidumine

See PowerShelli skripti peaks töötama kohalikult sisse Interneti ühendatud arvuti või serveri.

>[AZURE.IMPORTANT] See skript käitamisel võib olla hoiatused või muud informatiivsed teated, mis PowerShellis pop. Punase tooniga ainult tõrketeated on muret.


    <# 
     .SYNOPSIS
      Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared
      
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.
    
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.5
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
      
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 0
        #       - The AppVM1 Server must be VM 1
        #       - The DNS server must be VM 3
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
        # VM 1 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 2 - The Second Application Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 3 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #   

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                Remove-AzureEndpoint -Name "PowerShell" | `
                New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Note: A Remote Desktop endpoint is automatically created when each VM is created.
            $i++
        }
        # Add HTTP Endpoint for IIS01
        Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) to $($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
            -Protocol *
        
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *
    
        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
      

#### <a name="network-config-file"></a>Võrgu Config fail
Salvestage see XML-fail värskendatud asukoht ja lisada lingi selle faili $NetworkConfigFile muutuja ülaltoodud skripti.
    
    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Rakenduse näidisskriptid
Kui soovite selle ja muude DMZ näited valimi rakenduse installimiseks, üks on esitatud järgmisel lingil: [Rakenduse skripti näide][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Sissetulev DMZ NSG abil"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

