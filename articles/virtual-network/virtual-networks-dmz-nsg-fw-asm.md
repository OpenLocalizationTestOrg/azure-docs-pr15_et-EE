<properties
   pageTitle="Näide DMZ-koostada DMZ, tulemüüri ja NSGs rakenduste kaitsta | Microsoft Azure'i"
   description="Koostamine on DMZ, tulemüüri ja võrgu turberühmad (NSG)"
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

# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a>Näide 2 – koostada DMZ, rakendused, tulemüüri ja NSGs kaitsmiseks

[Tagasi lehele Turve piiri head tavad][HOME]

Selles näites loob mõne DMZ tulemüüri, neli windows serverite ja võrgu turberühmad. See ka juhendab iga oluline käsud süvitsi mõistmise iga toimingu esitada. Olemas on ka liikluse stsenaarium jaotise esitada põhjalikult samm-sammult, kuidas liikluse tulu kaitse DMZ kihid läbi. Lõpuks viited jaotis on lõpule viidud kood ja juhiseid, et koostada selles keskkonnas testimiseks ja katsetage erinevaid võimalusi. 

![Sissetulev DMZ Dessandi ja NSG][1]

## <a name="environment-description"></a>Keskkonna kirjeldus
Selles näites on tellimus, mis sisaldab järgmist:

- Kaks pilveteenused: "FrontEnd001" ja "BackEnd001"
- Virtuaalse võrgu "CorpNetwork" koos kahe alamvõrku: "FrontEnd" ja "Kirjutamata"
- Ühe võrgu turberühma, mis on rakendatud nii alamvõrku
- Võrgu virtuaalse seadme selles näites Barracuda NextGen tulemüüri, ühendatud Frontend alamvõrgu
- Windows Server, mis tähistab rakenduse veebiserverisse ("IIS01")
- Kaks windows serverite, mis tähistab rakenduse uuesti end serverid ("AppVM01", "AppVM02")
- Windows server, mis tähistab DNS server ("DNS01")

>[AZURE.NOTE] Kuigi see näide kasutab Barracuda NextGen tulemüüri, saab paljude erinevate võrgu virtuaalne seadmed selle näite puhul.

All viidetega jaotises on PowerShelli skripti, mis põhineb enamik keskkond, mis on kirjeldatud ülal. Koostamise VMs ja virtuaalne võrkude, kuigi on valmis näide skripti, on pole üksikasjalikult kirjeldatud selles dokumendis.
 
Keskkonna koostamiseks:

  1.    Viited jaotises võrku config XML-faili salvestamine (uuendatud nimed, asukoht ja IP-aadressid vastavad antud stsenaarium)
  2.    Värskendage kasutaja muutujate skripti vastavaks script on käivitamiseks suhtes (tellimuste teenuste nimed, jne)
  3.    PowerShelli skripti käivitada

**Märkus**: sellele PowerShelli skripti piirkonna peab vastama piirkonna sellele võrgu konfiguratsiooni XML-failis.

Kui skript käivitub edukalt võidakse pärast skripti järgmist:

1.  Tulemüüri reeglid häälestada, seda käsitletakse allpool olevat jaotist: tulemüüri reeglid.
2.  Soovi korral jaotises viited on kaks skriptide veebiserverisse ja rakenduse serveris, millel on lihtne veebirakenduse testimine selle DMZ konfiguratsiooni lubama häälestamiseks.

Järgmises jaotises selgitatakse enamik skriptide laused suhtes võrgu turberühmad.

## <a name="network-security-groups-nsg"></a>Võrgu turberühmad (NSG)
Selle näite puhul on NSG rühma ehitatud ja seejärel koormatud kuus reeglid. 

>[AZURE.TIP] Üldiselt teatud "Luba" reeglite tuleks esmalt looma ja seejärel viimati rohkem üldine "Keela" reegleid. Mis on määratud prioriteet dikteerib hinnatud esimene. Kui liikluse on leitud kindla reegli rakendamiseks, väärtustatakse pole veel reeglid. Kas sissetulevaid või väljaminevaid suunas (alamvõrgu seisukohast) saate rakendada NSG reeglid.

Deklaratiivse, ehitatakse sissetulev liiklus järgmisi reegleid:

1.  Sisemise DNS-i liikluse (port 53) on lubatud
2.  RDP-liikluse (port 3389) Interneti kaudu mis tahes VM on lubatud
3.  HTTP-liikluse (port 80) Interneti kaudu Dessandi (tulemüür) on lubatud
4.  Mis tahes liikluse (kõik pordid) kaudu IIS01 AppVM1 on lubatud
5.  Mis tahes liikluse (kõik pordid) Interneti kaudu kogu VNet (nii alamvõrku) on keelatud
6.  Mis tahes liiklus (kõik pordid) Frontend alamvõrgu Taustväärtus alamvõrgu on keelatud

Reegleid kindlasti iga alamvõrku, kui HTTP-päring on sissetuleva Interneti kaudu veebiserverisse, nii reeglite 3 (luba) ja 5 (keelamiseks) soovite rakendada, kuid kuna reegli 3 on kõrgem prioriteet ainult seda ja reegli 5 ei tule mängu. Seega Firewall lubatud HTTP-päring. Kui selle sama liikluse üritab DNS01 serverisse, reegel 5 (Keela) oleks esmalt rakendamiseks ja liiklus oleks lubatud edastamiseks server. Reegli 6 (Keela) blokeerib Frontend alamvõrgu räägib Taustväärtus alamvõrgu (v.a lubatud liikluse reeglite 1 – 4), see kaitseb kirjutamata võrgu juhul ründaja kompromisse, mis on Frontend, ründaja veebirakendus on piiratud juurdepääsu kirjutamata "kaitstud" (ainult seostuvatele ressurssidele esitatud AppVM01 serveris).

On vaikimisi Väljaminev reegel, mis lubab liikluse välja Interneti-ühendus. Selle näite puhul me lubamisel väljaminev liiklus ja pole muutmine mis tahes väljaminev reeglid. Lukustada nii juhiseid, kasutaja määratletud marsruutimine on vajalik, see on erinevate näites, mida saate uurida liikluse leitud [peamisi piiri dokumendi][HOME].

Ülaltoodud arutatakse NSG reeglid on väga sarnased NSG reegleid [näide]1 - koostada lihtsaid DMZ koos NSGs[Example1]. Palun vaadake üle NSG kirjeldus selles dokumendis üksikasjaliku vaadata iga NSG reegli ja selle atribuute.

## <a name="firewall-rules"></a>Tulemüüri reeglid
Halduse klient tuleb hallata tulemüüri ja luua vaja kuvatakse arvutis olema installitud. Vaadake, kuidas hallata seadme dokumentatsiooni tulemüüri (või muude Dessandi) tarnija. Ülejäänud selles jaotises kirjeldatakse konfiguratsiooni, tulemüüri, tarnijatele halduse kliendi (nt mitte Azure portaali või PowerShelli) kaudu.

Kliendi alla laadida ja selles näites kasutatakse Barracuda ühenduse loomise juhised leiate siit: [Barracuda NG administraator](https://techlib.barracuda.com/NG61/NGAdmin)

Tulemüüri, edasisaatmise reegleid on vaja luua. Kuna selles näites marsruudib ainult Interneti-liikluse sisse seotud tulemüüri ja seejärel veebiserverisse, ainult üks edasisaatmine reegli NAT on vaja. Selles näites kasutatakse Barracuda NextGen tulemüüri oleks reegli sihtkoha NAT reegli ("Dst NAT") seda liikluse edasi.

Järgmise reegli loomiseks (või olemasoleva vaikereeglit kinnitamine), alates Barracuda NG administraator kliendi armatuurlaua, liikuge menüü konfiguratsiooni, klõpsake jaotises funktsionaalseid konfiguratsiooni Ruleset. Koordinaatvõrgu nimega, "Põhi reeglid" näitab olemasoleva active ja desaktiveeritakse reegleid tulemüüri. See ruudustiku ülemises paremas nurgas on väike, roheline "+" nuppu, klõpsake selle uue reegli loomiseks (Märkus: tulemüüri võib olla "lukustatakse", kui te ei näe nuppu märgitud "Lukustada" ja te ei saa luua või reeglite redigeerimiseks, klõpsake seda nuppu, et "avada" kuvatakse ruleset ja Luba redigeerimist). Kui soovite olemasoleva reegli redigeerimine, valige see reegel, paremklõpsake ja valige reegli redigeerimine.

Uue reegli loomiseks ja sisestage nimi, näiteks "WebTraffic". 

Sihtkoha NAT reegli ikoon näeb välja umbes järgmine: ![Sihtkoha NAT ikoon][2]

Reegli ise peaks välja nägema umbes selline:

![Tulemüüri reegel][3]

Siin mis tahes meiliaadressi, mis tabab tulemüüri sissetulev jõua HTTP (port 80 ja 443 https) saadetakse läbi tulemüüri "DHCP1 kohalik IP" kasutajaliidese ja ümber veebiserver 10.0.1.5 IP-aadressiga. Kuna liiklus tulevad pordi 80 ja pordi 80 veebiserverisse toimub muutmata port oli vaja. Siiski sihtloendis võis 10.0.1.5:8080 kui meie veebiserver kuulda Port 8080, seega tõlkimiseks sissetulev port 80 tulemüüris sissetuleva pordi 8080 veebiserverisse.

Ühendusviisi peaks ka olema sellele, sihtkoha reegli Internetist, "Dünaamiline SNAT" on kõige kohasem. 

Kuigi ainult üks reegel on loodud on olulised, tema prioriteet on õigesti seatud. Kui kõik reeglid tulemüüri ruudustiku selle uue reegli allosas (all "BLOCKALL" reegel) on seda kunagi rakendatakse. Veenduge, võrguliikluse äsja loodud reegel on kohal BLOCKALL reegel.

Pärast reegli loomise viisist, see tuleb lükata tulemüüri ja seejärel aktiveerida, kui seda ei tehta reegli muutmine ei jõustu. Tõuketeatised ja aktiveerimise protsessi on kirjeldatud järgmises jaotises.

## <a name="rule-activation"></a>Reegli aktiveerimine
Ruleset, muuta, et lisada see reegel, kus on ruleset tulemüüri üles laadida ja aktiveerida.

![Tulemüüri reegli aktiveerimine][4]

Kliendi haldus ülemises parempoolses nurgas on kobar nuppudest. Tulemüüri muudetud reegleid saatmiseks nuppu "Saada muudatused" ja seejärel nuppu "Aktiveeri".

Tulemüüri ruleset aktiveerimine selles näites keskkonna koostamine on lõpule viidud. Soovi korral lisada rakendus selle keskkonnas testimiseks saab käivitada postituse koostamine skriptide jaotises viited selle all liikluse stsenaariumid.

>[AZURE.IMPORTANT] See on oluline mõistan, et teil pole tabab veebiserver otse. Kui brauser taotleb mõne lehe HTTP kaudu FrontEnd001.CloudApp.Net, HTTP lõpp-punkti (port 80) edastab selle liikluse tulemüüri pole veebiserverisse. Tulemüüri seejärel selle reegli järgi loodud ülaltoodud NATs, mis taotlus veebiserver.

## <a name="traffic-scenarios"></a>Liikluse stsenaariumid

#### <a name="allowed-web-to-web-server-through-firewall"></a>(Lubatud) Web abil veebiserverisse tulemüüri kaudu
1.  Interneti kasutaja taotlused HTTP lehel FrontEnd001.CloudApp.Net (Internet vastastikuste pilveteenus)
2.  Pilveteenuse teenuse möödub liikluse kaudu avatud lõpp-punkti port 80 tulemüüri kohaliku liides 10.0.1.4:80
3.  Frontend alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (DNS) ei rakendada, järgmise reegli liikumine
  2.    NSG reegli 2 (RDP) ei rakendada, järgmise reegli liikumine
  3.    NSG reegli 3 (Internetist tulemüüri) rakendamine, liiklus on lubatud, peatage reeglite töötlemine
4.  Liikluse tabab sisemise IP-aadress tulemüüri (10.0.1.4)
5.  Tulemüüri reegli vt see on pordi 80 liikluse, suunab ta veebiserver IIS01
6.  IIS01 on kuulamine web liikluse, saab see taotlus ja käivitab päringu töötlemise
7.  IIS01 küsitakse SQL serveri AppVM01 teavet
8.  Väljamineva reegleid Frontend alamvõrgu, liikluse on lubatud
9.  Taustväärtus alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (DNS) ei rakendada, järgmise reegli liikumine
  2.    NSG reegli 2 (RDP) ei rakendada, järgmise reegli liikumine
  3.    NSG reegli 3 (Internet Firewall) ei rakendada, teisaldamine järgmise reegli
  4.    NSG reegli 4 (IIS01, et AppVM01) rakendamine, liikluse on lubatud, reeglite töötlemise lõpetamine
10. AppVM01 saab SQL-päringu ja vastab
11. Kuna puuduvad väljaminev reeglid Taustväärtus alamvõrgu vastus on lubatud
12. Frontend alamvõrgu algab Sissetulev reegel töötlemine:
  1.    Ei ole NSG reegel, mis kehtib sissetulev liiklus Taustväärtus alamvõrgu Frontend alamvõrku, nii et ükski selle NSG reeglite rakendamine
  2.    Vaikimisi süsteemi reegli lubamisel vahel alamvõrku võimaldaks see liiklus nii, et liiklus on lubatud.
13. IIS-i server saab vastuse SQL-i ja lõpetab HTTP vastus ja saadab imperatiivsus päringu
14. Kuna see on NAT seansi tulemüüri, vastuse destination (esialgu) on tulemüüri
15. Tulemüüri saab vastuse veebiserver ja edastab tagasi Interneti kasutajale
16. Kuna seal on reegleid ei väljaminev Frontend alamvõrgu vastus on lubatud ja Internet kasutaja saab veebilehele, mis on nõutav.

#### <a name="allowed-rdp-to-backend"></a>(Lubatud) Põhjused, mida kirjutamata
1.  Serveri administraator Internetis taotleb AppVM01 kohta, kus xxxxx on RDP AppVM01 (määratud port leiate Azure'i portaalis või PowerShelli kaudu) Kui soovite CEIP määratud pordinumber BackEnd001.CloudApp.Net:xxxxx RDP-seanss
2.  Kuna tulemüüri on ainult kuulata FrontEnd001.CloudApp.Net aadressi, see on seotud selle liiklust
3.  Taustväärtus alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (DNS) ei rakendada, järgmise reegli liikumine
  2.    NSG reegli 2 (RDP) rakendamine, liiklus on lubatud, peatage reeglite töötlemine
4.  Väljamineva reegleid ei vaikereeglit rakendada ja tagastada liikluse on lubatud
5.  RDP-seansi on lubatud
6.  AppVM01 küsib kasutajanime ja parooli

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Lubatud) DNS-i serveris web serveri DNS-i otsing
1.  Web Server, IIS01, vajadustele andmekanali, www.data.gov juures, kuid vajadustele aadressi lahendamiseks.
2.  Võrgukonfiguratsioon VNet loendite DNS01 (10.0.2.4 Taustväärtus alamvõrgu) esmane DNS-i server, IIS01 saadab DNS-i taotluse DNS01
3.  Väljamineva reegleid Frontend alamvõrgu, liikluse on lubatud
4.  Taustväärtus alamvõrgu algab Sissetulev reegel töötlemine:
  1.     NSG reegli 1 (DNS-i) rakendamine, liiklus on lubatud, peatage reeglite töötlemine
5.  DNS-serveri saab taotluse
6.  DNS-i server pole vahemälus talletatud aadressi ja palub juur DNS-i serveri Internet
7.  Väljamineva reegleid Taustväärtus alamvõrgu, liikluse on lubatud
8.  Interneti DNS-i server vastab, kuna selle seansi käivitati ettevõttesiseselt, vastus on lubatud
9.  DNS-serveri vahemälu vastus ja algse vastuseks naasmiseks IIS01
10. Väljamineva reegleid Taustväärtus alamvõrgu, liikluse on lubatud
11. Frontend alamvõrgu algab Sissetulev reegel töötlemine:
  1.    Ei ole NSG reegel, mis kehtib sissetulev liiklus kirjutamata alamvõrgu Frontend alamvõrku, nii et ükski selle NSG reeglite rakendamine
  2.    Vaikimisi süsteemi reegli lubamisel vahel alamvõrku võimaldab see liiklus nii, et liiklus on lubatud
12. IIS01 saab vastuse DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(Lubatud) Web serveri juurdepääsu faili AppVM01
1.  IIS01 küsib AppVM01 faili
2.  Väljamineva reegleid Frontend alamvõrgu, liikluse on lubatud
3.  Taustväärtus alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (DNS) ei kehti, teisaldamine järgmise reegli
  2.    NSG reegli 2 (RDP) ei rakendada, järgmise reegli liikumine
  3.    NSG reegli 3 (Internet Firewall) ei rakendada, teisaldamine järgmise reegli
  4.    NSG reegli 4 (IIS01, et AppVM01) rakendamine, liikluse on lubatud, reeglite töötlemise lõpetamine
4.  AppVM01 saab taotluse ja vastab failiga (eeldades, et juurdepääs on lubatud)
5.  Kuna ei ole väljamineva reegleid kirjutamata alamvõrgu vastus on lubatud
6.  Frontend alamvõrgu algab Sissetulev reegel töötlemine:
  1.    Ei ole NSG reegel, mis kehtib sissetulev liiklus Taustväärtus alamvõrgu Frontend alamvõrku, nii et ükski selle NSG reeglite rakendamine
  2.    Vaikimisi süsteemi reegli lubamisel vahel alamvõrku võimaldaks see liiklus nii, et liiklus on lubatud.
7.  IIS-i server saab faili

#### <a name="denied-web-direct-to-web-server"></a>(Keelatud) Web otse veebiserverisse
Kuna veebiserver, IIS01 ja tulemüüri on sama pilveteenuses ühiskasutusse need sama avaliku suunatud IP-aadress. Seetõttu oleks mis tahes HTTP-liikluse suunata tulemüüri. Kuigi taotluse tuleb edukalt kätte, seda ei saa minna otse veebiserverisse, möödunud, nagu on loodud, tulemüüri kaudu esmalt. Lugege teemat esimese stsenaarium selle jaotise jaoks liiklust.

#### <a name="denied-web-to-backend-server"></a>(Keelatud) Web Taustväärtus serveris
1.  Interneti-kasutaja proovib juurde AppVM01 faili BackEnd001.CloudApp.Net teenuse kaudu
2.  Kuna see on avatud faili ühiskasutusse andmine pole lõpp-punkte, seda ei liigu pilveteenusesse ja ei jõua server
3.  Kui mingil põhjusel olid avatud lõpp-punktid, NSG reegli 5 (VNet internetist) blokeeritaks see liiklus

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Keelatud) DNS-i serveris Web DNS-i otsing
1.  Interneti-kasutaja proovib otsingul sisemise DNS-i kirje kohta DNS01 BackEnd001.CloudApp.Net teenuse kaudu
2.  Kuna ei ole lõpp avatud DNS-i, seda ei liigu pilveteenusesse ja ei jõua server
3.  Kui mingil põhjusel olid avatud lõpp-punktid, NSG reegli 5 (VNet internetist) blokeeritaks see liiklus (Märkus: selle reegli 1 (DNS) ei kehti kahel põhjusel, esmalt allikas aadress on Interneti-ühendus, see reegel rakendub ainult kohaliku VNet allikana, ka see on reegli luba nii, et see oleks kunagi keelamiseks liikluse)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(Keelatud) SQL-i juurdepääs läbi tulemüüri Web
1.  Interneti-kasutaja taotleb SQL-i andmete FrontEnd001.CloudApp.Net (Internet vastastikuste pilveteenus)
2.  Kuna ei ole lõpp avatud SQL-i, seda ei liigu pilveteenusesse ja ei jõua tulemüüri
3.  Kui mingil põhjusel olid avatud lõpp-punktid, algab Frontend alamvõrgu Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (DNS) ei rakendada, järgmise reegli liikumine
  2.    NSG reegli 2 (RDP) ei rakendada, järgmise reegli liikumine
  3.    NSG reegli 2 (Internet Firewall) rakendamine, liiklus on lubatud, peatage reeglite töötlemine
4.  Liikluse tabab sisemise IP-aadress tulemüüri (10.0.1.4)
5.  Tulemüüri on edasisaatmise reegleid ei SQL-i ja langeb liiklus

## <a name="conclusion"></a>Kokkuvõte
See on üsna lihtne viis rakenduse tulemüüri kaitsmine ja tagasi end alamvõrgu sissetulev liiklus eraldamiseks.

Veel näiteid ja võrgu turvalisus piirmäärad ülevaate leiate [siit][HOME].

## <a name="references"></a>Viited
### <a name="main-script-and-network-config"></a>Peamised skripte ja võrgu Config
Täielik skripti PowerShelli skripti faili salvestada. Salvestage fail nimega "NetworkConf2.xml" võrgu Config.
Muutke kasutaja määratletud muutujaid vastavalt vajadusele. Skripti, siis järgige tulemüüri reegli häälestamise eespool.

#### <a name="full-script"></a>Täielik skripti
Kuvatakse selle skripti, kasutaja määratletud muutujate põhjal:

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
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet (plus the NVA on the FrontEnd subnet)
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
       myFirewall - 10.0.1.4
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
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 1
        #       - The AppVM1 Server must be VM 2
        #       - The DNS server must be VM 4
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 4 - The DNS Server
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
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: Web traffic goes through the firewall, so we'll need to set up a HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and Port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) to $($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
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
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
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
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Sissetulev DMZ NSG abil"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Sihtkoha NAT ikoon"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Tulemüüri reegel"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Tulemüüri reegli aktiveerimine"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
