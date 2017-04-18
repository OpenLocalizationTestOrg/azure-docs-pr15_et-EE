<properties
   pageTitle="Näide DMZ-koostada DMZ, tulemüüri, UDR ja NSG võrkude kaitsmiseks | Microsoft Azure'i"
   description="Koostamine on DMZ tulemüüri, kasutaja määratletud marsruutimise (UDR) ja võrgu turberühmad (NSG)"
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

# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a>Näide 3 – koostada DMZ, tulemüüri, UDR ja NSG võrkude kaitsmiseks

[Tagasi lehele Turve piiri head tavad][HOME]

Selles näites loob mõne DMZ tulemüüri, neli windows serverite, kasutaja määratletud marsruutimine, IP edastamine ja võrgu turberühmad. See ka juhendab iga oluline käsud süvitsi mõistmise iga toimingu esitada. Olemas on ka liikluse stsenaarium jaotise esitada põhjalikult samm-sammult, kuidas liikluse tulu kaitse DMZ kihid läbi. Lõpuks viited jaotis on lõpule viidud kood ja juhiseid, et koostada selles keskkonnas testimiseks ja katsetage erinevaid võimalusi. 

![Kahesuunalise DMZ Dessandi, NSG ja UDR][1]

## <a name="environment-setup"></a>Keskkonna häälestamine
Selles näites on tellimus, mis sisaldab järgmist:

- Kolm pilveteenused: "SecSvc001", "FrontEnd001" ja "BackEnd001"
- Virtuaalse võrgu "CorpNetwork" koos kolme alamvõrku: "SecNet", "FrontEnd" ja "Kirjutamata"
- Võrgu virtuaalse seadme selles näites tulemüüri, ühendatud SecNet alamvõrgu
- Windows Server, mis tähistab rakenduse veebiserverisse ("IIS01")
- Kaks windows serverite, mis tähistab rakenduse uuesti end serverid ("AppVM01", "AppVM02")
- Windows server, mis tähistab DNS server ("DNS01")

All viidetega jaotises on PowerShelli skripti, mis põhineb enamik keskkond, mis on kirjeldatud ülal. Koostamise VMs ja virtuaalne võrkude, kuigi on valmis näide skripti, on pole üksikasjalikult kirjeldatud selles dokumendis.

Keskkonna koostamiseks:

  1.    Viited jaotises võrku config XML-faili salvestamine (uuendatud nimed, asukoht ja IP-aadressid vastavad antud stsenaarium)
  2.    Värskendage kasutaja muutujate skripti vastavaks script on käivitamiseks suhtes (tellimuste teenuste nimed, jne)
  3.    PowerShelli skripti käivitada

**Märkus**: sellele PowerShelli skripti piirkonna peab vastama piirkonna sellele võrgu konfiguratsiooni XML-failis.

Kui skript käivitub edukalt võidakse pärast skripti järgmist:

1.  Tulemüüri reeglid häälestada, seda käsitletakse allpool olevat jaotist: tulemüüri reegli kirjeldust.
2.  Soovi korral jaotises viited on kaks skriptide veebiserverisse ja rakenduse serveris, millel on lihtne veebirakenduse testimine selle DMZ konfiguratsiooni lubama häälestamiseks.

Kui skript käivitub edukalt tulemüüri reeglid tuleb lõpule viia, seda käsitletakse jaotises pealkirjaga: tulemüüri reeglid.

## <a name="user-defined-routing-udr"></a>Kasutaja määratletud marsruutimise (UDR)
Vaikimisi on järgmine süsteemi marsruudib määratletud järgmiselt:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

Funktsiooni VNETLocal on alati määratletud aadress prefix(es), VNet selle teatud võrgu (st see muutub VNet VNet sõltuvalt sellest, kuidas iga teatud VNet on määratletud) jaoks. Ülejäänud süsteemi marsruudib on staatiline ja vaikimisi nagu eespool kirjeldatud.

Priority (prioriteet), mis marsruudib töödeldakse meetodil pikima eesliite Match (LPM), nii kõige marsruutimiseks tabelis kehtiksid antud Sihtaadress.

Seetõttu oleks üle VNet sihtkohta tõttu 10.0.0.0/16 marsruutimiseks marsruutida liikluse (nt DNS01 server 10.0.2.4) mõeldud kohtvõrgu (10.0.0.0/16). Teisisõnu, 10.0.2.4, 10.0.0.0/16 marsruutimiseks on kõige marsruutimiseks, ehkki 10.0.0.0/8 ja 0.0.0.0/0 ka võivad rakendada, kuid kuna need on väiksem teatud need ei mõjuta see liiklus. Seega 10.0.2.4 liiklust on mõne järgmise sõnumihüppe kohta, pärast, kohaliku VNet ja lihtsalt marsruutimine sihtkohta.

Kui liikluse on mõeldud 10.1.1.1 näiteks 10.0.0.0/16 marsruutimiseks ei rakendada, kuid selle 10.0.0.0/8 oleks kõige teatud ja see liiklus oleks lähevad ("must holed"), kuna selle järgmise sõnumihüppe kohta, pärast on Null. 

Kui sihtkoht ei rakendada mõne eesliiteid Null või VNETLocal eesliiteid ja järgige selle oleks vähemalt teatud marsruutimine, 0.0.0.0/0 ja Interneti-ühendus on järgmine sõnumihüppe kohta, pärast ja seega välja Azure internet serva marsruutida.

Kui marsruudi tabelis on kaks identse eesliidete tähised, on järjestuse eelistused marsruudib "allikas" atribuudi alusel:

1.  "VirtualAppliance" = kasutaja määratletud marsruutimiseks käsitsi lisatud tabel
2.  "VPNGateway" = dünaamiline marsruutimiseks (BGP kasutamisel koos hübriid võrke) lisatud dünaamiline Võrguprotokollide, lennuliinide võivad aja jooksul muutuda nagu dünaamiline protokoll automaatselt kajastab piilus võrgus
3.  "Vaikimisi" = süsteemi marsruudib, kohaliku VNet ja staatilise kirjeid ülaltoodud tabelis marsruutimiseks.

>[AZURE.NOTE] Nüüd saate kasutaja määratletud marsruutimist (UDR) ExpressRoute ja VPN lüüside jõustamine rist – eeldusel sissetuleva ja väljamineva liikluse marsruutida võrgu virtuaalse seadme (Dessandi).

#### <a name="creating-the-local-routes"></a>Kohaliku marsruudib loomine

Selles näites on vaja kahe marsruudi tabelid, üks iga alamvõrku Frontend ja Taustväärtus. Staatilise protsesside vastav antud alamvõrgu laaditakse iga tabeli. Selleks, et selles näites on iga tabeli kolme marsruudib.

1. Kohaliku alamvõrgu liiklus järgmise Hop määratletud kohaliku alamvõrgu liiklust tulemüüri lubama
2. Virtuaalne võrguliiklust järgmise Hop määratletud tulemüüri, alistab see on vaikimisi reegel, mis lubab kohaliku VNet liikluse marsruutimiseks otse
3. Kõigi ülejäänud liikluse (0/0) koos järgmise Hop määratletud tulemüüri

Kui marsruutimise tabelid on loodud nad on seotud oma alamvõrku. Jaoks Frontend alamvõrgu marsruutimise tabel, kui loodud ja seotud alamvõrgu peaks välja nägema selline:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


Selles näites kasutatakse järgmised käsud marsruutimiseks tabeli koostamine, kasutaja määratletud protsessi lisamiseks ja seejärel siduda marsruutimiseks tabeli lisamine alamvõrgu (Märkus; kõik üksused, mis algavad sõnadega dollarimärk all (nt: $BESubnet) on kasutaja määratletud muutujate skripti viide jaotises selle dokumendi):

1.  Kõigepealt tuleb luua marsruutimise põhitabel. See koodilõigu kuvatakse Taustväärtus alamvõrgu tabeli loomine. Skript, vastava tabeli on loodud Frontend alamvõrgu.

        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"

2.  Kui marsruutimiseks tabel on loodud, saate teatud kasutaja määratletud marsruudib lisada. Selles snipped, suunatakse kõik liikluse (0.0.0.0/0) kaudu virtuaalse seadme (muutuja, $VMIP [0], kasutatakse läbida määratud virtuaalse seadme loomisel eelnevalt kirjeldatud skripti IP-aadress). Skript, luuakse vastava reegli ka Frontend tabelis.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

3. Ülaltoodud marsruutimiseks kirje alistab vaikimisi "0.0.0.0/0" marsruudi, kuid vaikimisi 10.0.0.0/16 reegli veel olemas, mis võimaldaks liikluse marsruutimiseks otse sihtkohta, mitte võrgu virtuaalse seadme VNet. Õige seda käitumist reegli järgi tuleb lisada.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

4. On selles etapis tuleb valida. Ülaltoodud kaks protsesside kuvatakse kõik liikluse marsruutida tulemüüri, paarislehekülgedele liikluse ühe alamvõrgu hindamiseks. See võib soovitud aga alamvõrku, marsruutimiseks kohalik ilma tulemüüri kolmanda liikluse lubamiseks väga kindla reegli saab lisada. Selles, et kõik aadressi suunama kohaliku alamvõrgu saate lihtsalt seal marsruutimine otse (NextHopType = VNETLocal).

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal

5.  Lõpuks loodud ja kasutaja määratletud marsruudib eeltäidetakse marsruutimise tabel, tabel peab nüüd olema seotud on alamvõrgu. Skript, esiosa marsruutimiseks tabel on seotud ka Frontend alamvõrgu. Siin on tagasi end alamvõrgu sidumine skripti.

        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
           -SubnetName $BESubnet `
           -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>IP ümbersuunamine
Funktsioon companion UDR, on IP edastamine. See on virtuaalse seadme, mis võimaldab saada liikluse konkreetselt adresseeritud seade ja seejärel edastab selle liikluse lõpliku säte.

Näiteks kui liikluse aadressilt AppVM01 taotleb DNS01 server, UDR oleks marsruutida see tulemüüri. Koos IP edastamine lubatud, liikluse DNS01 sihtkoha (10.0.2.4) aktsepteeritakse seadme (10.0.0.4), ja seejärel edastatakse ultimate sihtkohta (10.0.2.4). Ilma IP edastamine tulemüüri sisse lülitatud, liikluse ei võeta seadet, ehkki marsruutimiseks tabelil on nimega kuvatakse järgmise sõnumihüppe kohta, pärast tulemüüri. 

>[AZURE.IMPORTANT] On oluline meeles pidada, et lubada IP edastamine koos kasutaja määratletud marsruutimist.

Häälestamise IP edastamine on ühe käsu ja saab teha VM loomise ajal. Selles näites voo koodilõigu skripti lõpus on ja rühmitatud UDR käsud.

1.  Kõne VM eksemplari, mis on teie virtuaalse seadme tulemüüri sel juhul ja luba IP edastamine (Märkus; mõnda üksust punane algavad sõnadega dollarimärk (nt: $VMName[0]) on kasutaja määratletud muutuja viide jaotises selle dokumendi käsikirjaga. Null sulgudes [0], tähistab esimese VM VMs näide skripti töötamiseks muutmata massiivis, esimese VM (VM 0) peab olema tulemüüri):

        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
           Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Võrgu turberühmad (NSG)
Selles näites on NSG rühma ehitatud ja seejärel laadida ühe reegli abil. Selles rühmas on seotud siis ainult Frontend ja Taustväärtus alamvõrku (mitte SecNet). Deklaratiivse ehitatakse järgmise reegli:

1.  Mis tahes liikluse (kõik pordid) Interneti kaudu kogu VNet (kõik alamvõrku) on keelatud

Kuigi selles näites kasutatakse NSGs, on see peamine eesmärk teisene kiht vastu käsitsi vale konfiguratsioon. Soovime blokeeri kogu sissetulev liiklus Interneti kaudu ühte on Frontend või Taustväärtus alamvõrku, liikluse peaks ainult kaudu SecNet alamvõrgu Firewall flow (ja seejärel vajadusel Frontend või Taustväärtus alamvõrku). Lisaks UDR reegleid kohas, kus kõik liiklus, mis ei tee see Frontend või Taustväärtus alamvõrku oleks suunata välja tulemüüri (tänu UDR). Tulemüüri näeksid see on asümmeetriline kulgemist ja langeb väljaminev liiklus. Seega on kolme kihti turvalisus kaitsmine Frontend ja Taustväärtus alamvõrku; 1) pole avatud lõpp-punkte FrontEnd001 ja BackEnd001 pilveteenused, (2) NSGs keelamisega seotud liikluse Internetist, 3) tulemüüri langetamine asümmeetriline liiklust.

Ühe huvipakkuva punkti kohta võrgu turberühma selles näites on see sisaldab ainult üks reegel, kuvatakse allpool, mis on Interneti-liikluse kogu virtuaalse võrku, mis sisaldab turvalisus alamvõrgu keelamiseks. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Kuna selle NSG on ainult seotud Frontend ja Taustväärtus alamvõrku, reegel ei töödelda, liikluse sissetuleva meili Turve alamvõrgu. Selle tulemusena isegi siis, kui reegel NSG ütleb puudub Interneti-liikluse iga aadressi VNet, kuna selle NSG kunagi seotud turbe alamvõrgu, liikluse on flow alamvõrgu turvalisus.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName
    
    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Tulemüüri reeglid
Tulemüüri, edasisaatmise reegleid on vaja luua. Kuna tulemüüri blokeerimine või ümbersuunamine kogu Sissetulev, Väljaminev ja sisene-VNet liikluse palju tulemüüri reeglid on vaja. Ka kogu sissetulev liiklus tabab turvalisuse teenuse avaliku IP-aadress (erinevad pordid), tulemüüri töötlemiseks. Hea tava on diagramm loogilise puhul enne selle alamvõrku häälestamist ja tulemüüri reeglid vältimiseks töödelda hiljem. Järgmisel joonisel on loogiline vaade tulemüüri reegleid, näiteks:
 
![Loogika vaate tulemüüri reeglid][2]

>[AZURE.NOTE] Võrgu virtuaalse seadme kasutatud põhjal, erinevad pordid haldus. Selles näites on viidatud Barracuda NextGen tulemüüri, mis kasutab pordid 22, 801 ja 807. Pöörduge seadme tarnija dokumentatsiooni haldamiseks kasutatava seadme täpse pordid leidmiseks.

### <a name="logical-rule-description"></a>Loogika reegli kirjeldus
Loogika ülaltoodud joonisel, Turve alamvõrgu ei kuvata, kuna tulemüüri on ainult ressurss selle alamvõrgu ja diagramm näitab tulemüüri reeglid ja kuidas need loogiliselt lubada või keelata liikluse puhul ja mitte tegelik suunatud tee. Ka välise valitud RDP liikluse on suurem ulatus pordid (8014 – 8026) ja valitud mõnevõrra joondamiseks kaks viimast octets kohalik IP-aadressi lihtsam loetavuse (nt kohaliku serveri aadress 10.0.1.4 on seotud välise pordi 8014), aga kõrgema vastuoluliste pordid saaks kasutada.

Selle näite puhul läheb vaja 7 tüüpi reegleid, reegli järgmist tüüpi kirjeldatakse järgmiselt:

- Välised reeglid (sissetulev liiklus):
  1.    Tulemüüri haldus reegel: See rakendus Redirect reegel võimaldab liikluse halduse pordid võrgu virtuaalse seadme edasi.
  2.    (Iga windows server) RDP reeglid: võimaldab nelja reegleid (üks iga server) kaudu RDP üksikute serverite haldus. See võib kombineeritud sisse ühe reegli sõltuvalt kasutatava võrgu virtuaalse seadme võimalustest.
  3.    Rakenduse liikluse reegleid: On kaks rakenduse liikluse reeglid, esiosa web liikluse esimene ja teine tagasi end liikluse (nt veebiserver soovite andmete esimese taseme). Konfiguratsiooni reegleid, on sõltuvad võrgu arhitektuur (kui teie serverid suunatakse) ja liiklus puhul (liikluse puhul mis pordid kasutatakse, millises suunas).
      - Esimese reegli võimaldab tegeliku rakenduse liikluse rakenduse serverisse. Turvalisus, haldus jne lubada muude reeglitega, rakenduse reeglite ajal mis luba väliskasutajad või teenuste juurde pääseda soovitud rakendustes. Selle näite puhul on ühe veebiserverisse port 80, seega ühe tulemüüri rakenduse reegli suunab sissetulev liiklus väline IP, web servers sisemise IP-aadress. Liikluse ümber seansi peaks olema NAT tuli sisemise serveri.
      - Teine rakenduse liikluse reegel on tagasi end reegel mis tahes pordi veebiserver rääkida AppVM01 server (kuid mitte AppVM02) lubamiseks.
- Sise-eeskirjad (Sisene-VNet liikluse)
  4.    Väljamineva Internet reeglit: see reegel võimaldab liikluse mis tahes võrk edastamiseks valitud võrgu kaudu. See reegel on tavaliselt vaikimisi reegel juba tulemüüri, kuid keelatud olekusse. Selles näites see reegel peaks olema lubatud.
  5.    DNS-i reegel: See reegel võimaldab ainult DNS-i (port 53) liiklus DNS server edasi. Selles keskkonnas enamik on Frontend abil soovitud Taustväärtus liikluse on blokeeritud, see reegel võimaldab konkreetselt DNS-i mis tahes kohaliku alamvõrgu.
  6.    Alamvõrgu abil alamvõrgu reeglit: see reegel on lubada mis tahes serveri tagasi end alamvõrgu serveriga mis tahes esiosa alamvõrgu (kuid mitte vastupidi).
- Tõrkekindel reeglit (liikluse, mis ei vasta mõne eespool):
  7.    Keelamiseks kõik liikluse reegel: See tuleb alati lõplik reegli (prioriteet) nii ja sellisena kui liiklus liigub sujuvalt ei sobi ühegi eelmiste reeglite see kõrvaldatakse selle reegliga. See on vaikimisi reegel ja tavaliselt aktiveeritud, muutmata on üldiselt vaja.

>[AZURE.TIP] Rakenduse liikluse teise reegli, mis tahes port on lubatud lihtne käesoleva näite reaal stsenaariumi kõige kindla pordi ja aadresside vahemikud kasutada, et vähendada rünnak pind see reegel.

<br />

>[AZURE.IMPORTANT] Kui kõik ülaltoodud reeglid on loodud, on oluline üle vaadata iga tagada liikluse on lubatud või keelatud vastavalt soovile reegli prioriteedi. Selle näite puhul on reegleid tähtsuse järjekorras. See on lihtne olla lukustades tulemüüri tõttu valesti järjestatud reeglid. Vähemalt, veenduge halduse jaoks ise tulemüüri on alati absoluutne kõrgeim eelistus reegel.

### <a name="rule-prerequisites"></a>Reegli eeltingimused
Ühe töötab tulemüüri virtuaalse masina nõutav on avalik lõpp-punktid. Tulemüüri töödelda liikluse vastav avaliku lõpp-punktid peab olema avatud. On kolme tüüpi liikluse selles näites; 1) halduse liiklust juhtelemendi tulemüüri ja tulemüüri reeglid, 2) RDP liikluse määrata windows serverite ja 3) rakenduse liikluse. Need on kolme tüüpi liikluse ülemises veerud pool loogilise vaate tulemüüri reeglid kohal.

>[AZURE.IMPORTANT] Siin on võtme takeway on meeles pidada, et **Kõik** liikluse tulevad tulemüüri kaudu. Nii kaugtöölaua IIS01 server, isegi juhul, kui see on ees End pilveteenuses ja esiosa alamvõrgu, sellele serverile juurdepääsu me ei vaja RDP port 8014 tulemüüri ja tulemüüri RDP taotluse ettevõttesiseselt marsruutimiseks IIS01 RDP Port ja seejärel lubada. Azure'i portaalis "Connect" nupp ei tööta sellepärast, et IIS01 pole otsene RDP tee (kui portaalis saate vaadata). See tähendab, et kõik ühendused Interneti kaudu ei saa turvalisuse teenuse ja portide, nt secscv001.cloudapp.net:xxxx (viide eespool diagramm kaardistada väline Port ja sisemine IP ja Port).

Lõpp-punkti saab avada kas VM loomise ajal või postituse koostamine on tehtud näide skripti ja selle koodilõigu allpool (Märkus; mõni üksus alustades dollarimärk (nt: $VMName[$i]) on kasutaja määratletud muutuja viide jaotises selle dokumendi käsikirjaga. "$i" sulgudes [$i] tähistab massiiv arvu teatud VM massiivi VMs):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Ehkki mitte selgelt näidatud siin kasutamisega muutujaid, kuid lõpp-punktid on **ainult** avada turvalisus pilveteenuses. See on, et tagada, et kogu sissetulev liiklus (marsruutida, NAT oli, lähevad) tulemüür.

Halduse klient tuleb hallata tulemüüri ja luua vaja kuvatakse arvutis olema installitud. Vaadake, kuidas hallata seadme dokumentatsiooni tulemüüri (või muude Dessandi) tarnija. Ülejäänud selles jaotises ja järgmises jaotises, tulemüüri reeglite loomine, kirjeldada konfiguratsiooni, tulemüüri, tarnijatele halduse kliendi (nt mitte Azure portaali või PowerShelli) kaudu.

Kliendi alla laadida ja selles näites kasutatakse Barracuda ühenduse loomise juhised leiate siit: [Barracuda NG administraator](https://techlib.barracuda.com/NG61/NGAdmin)

Pärast sisselogimist peale tulemüüri, kuid enne tulemüüri reeglite loomine, on kaks eelnevalt nõutud objekti klassi, mida saab teha loomise reegleid lihtsamaks; Võrgu ja teenuse objektid.

Selle näite puhul kolm nimega võrgu objekti peaks olema määratletud (üks Frontend alamvõrgu ja Taustväärtus alamvõrku, samuti võrgu objekti DNS-i serveri IP-aadress). Luua nimega võrgu; alates kliendi Barracuda NG administraatori armatuurlauale, liikuge vahekaarti konfigureerimine, funktsionaalseid konfigureerimine jaotises nuppu Ruleset, siis "Võrgud" tulemüüri objektide menüü, klõpsake käsku Uus menüü Redigeeri võrkude. Võrgu objekti saab nüüd luua, nimi ja eesliite lisamisega.

![FrontEnd võrgu objekti loomine][3]
 
See loob FrontEnd alamvõrgu nimega võrk, on sarnane objekt, tuleks luua ka Taustväärtus alamvõrgu. Nüüd on alamvõrku saab hõlpsam viidata tulemüüri reeglid nime järgi.

DNS-i Server objekti:

![DNS-i Server objekti loomine][4]

See ühe IP address viide kasutatud dokumendis hiljem reeglis DNS-i.

Teine eelnevalt nõutud objektid on teenuste objekte. Need RDP ühenduse pordid esindab iga server. Kuna olemasolev RDP teenuse objekt on seotud default RDP port, 3389, uusi teenuseid saab luua välise pordid (8014 8026) kaudu liikluse lubamiseks. Uue pordid võib lisada ka olemasoleva RDP teenuse, kuid hõlpsamaks tutvustamise, saab luua reegel iga server. RDP uue reegli loomiseks server; alates Barracuda NG administraator kliendi armatuurlaua, liikuge vahekaarti konfigureerimine funktsionaalseid konfigureerimine jaotises nuppu Ruleset, seejärel klõpsake menüüs tulemüüri objektid "Teenused", alla teenuste loend ja valige "RDP" teenus. Paremklõpsake ja valige Kopeeri, seejärel paremklõpsake ja klõpsake siis käsku Kleebi. Nüüd on RDP-Copy1 objekti, mida saab redigeerida. Paremklõpsake RDP-Copy1 ja valige käsk Redigeeri, Redigeeri teenuse objekti akna pop kuni, nagu on näidatud siin.

![Koopia Default RDP reegel][5]

Väärtused saab redigeerida siis tähistada konkreetse server RDP teenus. AppVM01 jaoks tuleb muuta ülaltoodud default RDP reegel on uus teenus nimi, kirjeldus ja välise RDP Port 8-kujund diagrammis kasutatud kajastamiseks (Märkus: pordid muudetakse, 3389 RDP vaikimisi kasutatava selle kindla serveri välise pordi, AppVM01 puhul on välised pordi 8025) muudetud teenus on allpool näidatud :

![AppVM01 reegel][6]
 
Seda toimingut korrata luua RDP teenuste jaoks ülejäänud serverid; AppVM02, DNS01 ja IIS01. Nende teenuste loomine teeb reegli loomise lihtsamaks ja selgemaks järgmises jaotises.

>[AZURE.NOTE] Teenuse RDP tulemüüri jaoks pole vaja kahel põhjusel; 1) esimese tulemüüri VM on Linux põhise pilt nii, et kasutataks SSH port 22 VM halduse RDP ja 2) port 22 asemel ja kahe halduse pordid on lubatud halduse Ühenduvus allpool kirjeldatud halduse esimese reegli.

### <a name="firewall-rules-creation"></a>Tulemüüri reeglite loomine
On kolme tüüpi tulemüüri reeglid selles näites kasutatakse, need kõik on erinevate ikoonid:

Rakenduse ümber suunata reeglit: ![Ümber suunata ikoon][7]

Sihtkoha NAT reeglit: ![Sihtkohta NAT ikoon][8]

Liigu reeglit: ![Edastavad ikoon][9]

Lisateabe saamiseks klõpsake järgmisi reegleid leiate veebisaidilt Barracuda.

Järgmiste reeglite loomiseks (või olemasoleva vaikereeglit kinnitamine), alates Barracuda NG administraator kliendi armatuurlaua, liikuge menüü konfiguratsiooni, klõpsake jaotises funktsionaalseid konfiguratsiooni Ruleset. Koordinaatvõrgu nimega, "Põhi reeglid" näitab olemasoleva active ja desaktiveeritakse reegleid seda tulemüüri. See ruudustiku ülemises paremas nurgas on väike, roheline "+" nuppu, klõpsake selle uue reegli loomiseks (Märkus: teie tulemüür võib olla "lukustatakse", kui kuvatakse nupp märgitud "Lukustada" ja te ei saa luua või reeglite redigeerimiseks, klõpsake seda nuppu, et "Ava" reegli määramine ja Luba redigeerimist). Kui soovite olemasoleva reegli redigeerimine, valige see reegel, paremklõpsake ja valige reegli redigeerimine.

Pärast reeglite loodud ja/või muudetud, tuleb lükata tulemüüri ja seejärel aktiveerida, kui see on võimalik muuta reegleid ei jõustu. Tõuketeatised ja aktiveerimise protsessi on kirjeldatud allpool üksikasjad reegli kirjeldus.

Üksikasjad iga reegel täitma selles näites kirjeldatakse järgmiselt:

- **Reegli tulemüüri haldus**: selle rakenduse ümber suunata reegel võimaldab liikluse edastamiseks halduse pordid võrgu virtuaalse seadme selles näites Barracuda NextGen tulemüüri. Juhtimise pordid 801 807 ja soovi korral 22. Sise- ja pordid on samad (st pole pordi tõlge). See reegel, HÄÄLESTUS-MGMT-juurdepääsu, on vaikimisi reegel ja (Barracuda NextGen tulemüüri versiooni 6.1) vaikimisi lubatud.

    ![Tulemüüri haldus reegel][10]

>[AZURE.TIP] Andmeallika aadressiruumi see reegel on olemas, kui halduse IP-aadresside vahemikud, see ulatus oleks vähendamine ka rünnak pind halduse pordid.

- **RDP reegleid**: võimaldab need sihtkoha NAT reeglite haldamine kaudu RDP üksikute serverid.
On selle reegli loomiseks vajalik kriitiline neljal.
  1.    Andmeallika – lubama RDP igalt poolt, on viide "" kasutatakse väljal Allikas.
  2.    Teenuse – kasutage sobivat teenuse objekti varem loodud sel juhul "AppVM01 RDP", välise pordid suunata serverid kohalik IP-aadress ja pordi 3386 (default RDP port). Selle kindla reegli on RDP juurdepääsuks AppVM01.
  3.    Siht-peaks olema *kohaliku porti tulemüüri*, "DCHP 1 kohalik IP" või eth0 kasutamisel staatiline IP-d. Järjenumber (eth0, eth1 jne) võib olla erinev, kui teie võrgu seade on mitu kohaliku liidesed. See on pordi kaudu tulemüüri saatmisel (võib olla sama vastuvõtmise pordi), tegelik suunatud sihtkoht on Sihtloend väli.
  4.    Ümbersuunamine – selles teemas kirjeldatakse virtuaalse seadme, kus see liiklus lõpuks suunata. Lihtsaim ümbersuunamine on IP ja Port (valikuline) väljale Sihtloend. Kui porti kasutatakse sissetuleva taotluse sihtkoha Port kasutatud (st pole tõlge), kui pordi on määratud pordi on ka NAT koos IP käsitleks.

    ![Tulemüüri RDP reegel][11]

    Kokku neli RDP reeglid on vaja luua: 

  	|   Reegli nimi    | Server  |   Teenus   |  Sihtloend  |
  	|----------------|---------|-------------|---------------|
  	| RDP-IIS01   | IIS01   | IIS01 RDP   | 10.0.1.4:3389 |
  	| RDP-DNS01   | DNS01   | DNS01 RDP   | 10.0.2.4:3389 |
  	| RDP-AppVM01 | AppVM01 | AppVM01 RDP | 10.0.2.5:3389 |
  	| RDP-AppVM02 | AppVM02 | AppVm02 RDP | 10.0.2.6:3389 |
  
>[AZURE.TIP] Lähte-kui ka teenuse väljade ulatuse täpsustamine vähendab rünnak pind. Kõige väiksema ulatuse, mis võimaldab funktsioone kasutada.

- **Rakenduse liikluse reegleid**: on kaks rakenduse liikluse reeglid, esiosa web liikluse esimene ja teine tagasi end liikluse (nt veebiserver soovite andmete esimese taseme). Reegleid ei sõltuvad võrgu arhitektuur (kui teie serverid suunatakse) ja liiklus puhul (liikluse puhul mis pordid kasutatakse, millises suunas).

    Esimest korda arutatakse, on võrguliikluse esiosa reegel:

    ![Tulemüüri Web reegel][12]
 
    See reegel sihtkoha NAT võimaldab tegeliku rakenduse liikluse rakenduse serverisse. Turvalisus, haldus jne lubada muude reeglitega, rakenduse reeglite ajal mis luba väliskasutajad või teenuste juurde pääseda soovitud rakendustes. Selle näite puhul on ühe veebiserverisse port 80, seega ühe tulemüüri rakenduse reegel suunab sissetulev liiklus väline IP, web servers sisemise IP-aadress.

    **Märkus**: et pole määratud sihtloendis väljal port, seega kasutatakse sissetuleva pordi 80 (või teenuse valitud 443) veebiserver ümbersuunamine. Kui veebiserver on kuulamine porti, näiteks port 8080, sihtloendis välja võib ajakohastada 10.0.1.4:8080 pordi ümbersuunamise ka lubama.

    Järgmise rakenduse liikluse reegel on tagasi end reegli lubamiseks veebiserver rääkima AppVM01 server (kuid mitte AppVM02) mis tahes teenuse kaudu:

    ![Tulemüüri AppVM01 reegel][13]

    See reegel Pass võimaldab IIS-serveris Frontend alamvõrgu saavutamiseks on AppVM01 (IP-aadress 10.0.2.5) mis tahes pordi abil protokolli Accessi veebirakenduse vajalikud andmed.

    See ekraanipilt on "\<konkreetsete dest\>" kasutatakse sihtvälja tähenda 10.0.2.5 sihtkohana. See võib olla kas konkreetsete, nagu on näidatud või nimega võrgu objekti (nagu tehtud eeltingimused DNS-server). See sõltub sellest, millist meetodit kasutada tulemüüri administraator. Lisamiseks 10.0.2.5 on Explict Desitnation, topeltklõpsake esimese tühja rea jaotises \<konkreetsete dest\> ja sisestage aadress Kuvatavas aknas.

    Liigu reegliga selle pole NAT on vaja, kuna see on sisemine liikluse, et ühenduse meetodit saate seada "No SNAT".

    **Märkus**: selle reegli allikas võrk on mis tahes ressursi FrontEnd alamvõrgu, kui seal on ainult üks või web serverites teadaolevad teatud arvu võrgu objekti ressursi saanud luua täpsemalt, et need täpse IP-aadresside asemel kogu Frontend alamvõrgu.

>[AZURE.TIP] See reegel kasutab teenust "Kõik" teha valimi rakenduste häälestamine ja kasutamine lihtsamaks, see võimaldab ICMPv4 (ping) ühe reegli. Siiski ei ole soovitatav tava. Võimalikud miinimum, mis võimaldab rakenduse töö vähendamiseks rünnak pind üle selle piiri tuleb vähendada pordid ja protokollid ("teenused").

<br />

>[AZURE.TIP] Kuigi see reegel kuvatakse on kasutusel konkreetsete dest viide, võib kasutada kogu tulemüüri konfigureerimine ühtse lähenemisviisi. On soovitatav, et nimega võrgu objekti kasutada kogu lihtsam loetavuse ja klienditoe. Konkreetsete dest kasutatakse ainult mõne alternatiivse meetod kuvamiseks siin ja ei ole üldiselt soovitatav (eriti keerukate konfiguratsioone).

- **Väljaminev Internet reeglil**: Andke see reegel võimaldab liikluse mis tahes allika võrgust valitud sihtkoha võrke edasi. See reegel on vaikimisi reegel tavaliselt juba Barracuda NextGen tulemüüri, kuid on keelatud. Paremklõpsake selle reegli pääsevad käsk Aktiveeri reegel. Reegli joonisel on muudetud lisamiseks kaks kohaliku alamvõrku, mis on loodud viited eelnevalt nõutud jaotises selle dokumendi allika atribuut see reegel.

    ![Tulemüüri Väljaminev reegel][14]

- **DNS-i reegli**: Andke see reegel võimaldab ainult DNS-i (port 53) liiklus DNS server edasi. Selles keskkonnas enamik on FrontEnd abil soovitud Taustväärtus liikluse on blokeeritud, võimaldab see reegel konkreetselt DNS-i.

    ![Tulemüüri DNS-i reegel][15]

    **Märkus**: Kuva kuvatõmmis ühenduse meetod on lisatud. Kuna see reegel on sisemine IP sisemise IP address liiklus, pole mis tegeleb on vaja, selle ühenduse meetod on seatud "No SNAT" selle reegli edastamiseks.

- **Alamvõrgu abil alamvõrgu reegli**: Andke see reegel on vaikimisi reegli, mis on aktiveeritud ning muuta, et lubada mis tahes serveri tagasi end alamvõrgu esiosa alamvõrgu mis tahes serveriga ühenduse loomiseks. See reegel on kõik sise-liikluse nii, et ühenduse meetodit saate seada ei SNAT.

    ![Tulemüüri sisene-VNet reegel][16]

    **Märkus**: kahesuunalise ruut on märgitud (ega oleks märgitud enamik reeglid), see on oluline selle reegli, et see muudab see reegel "ühe suunamata", ühenduse saab algatada tagasi end alamvõrgu esiosa võrgus, kuid mitte vastupidi. Kui see ruut on märgitud, võimaldaks see reegel kahesuunalise liikluse, mida meie loogilise diagrammist on soovitud.

- **Keela kõik liikluse reegel**: see tuleb alati lõplik reegli (prioriteet) osas ja sellisena kui liikluse juhtimine ei sobi ühegi eelmise reeglite, see kõrvaldatakse selle reegli alusel. See on vaikimisi reegel ja tavaliselt aktiveeritud, muutmata on üldiselt vaja. 

    ![Tulemüüri keelamiseks reegel][17]

>[AZURE.IMPORTANT] Kui kõik ülaltoodud reeglid on loodud, on oluline üle vaadata iga tagada liikluse on lubatud või keelatud vastavalt soovile reegli prioriteedi. Selle näite puhul on need peaks kuvatama põhi ruudustiku edasisaatmise reegleid Barracuda halduse klientrakenduses järjestuses reegleid.

## <a name="rule-activation"></a>Reegli aktiveerimine
Ruleset, muuta, et määratlus loogika diagrammi, kus on ruleset tulemüüri üles laadida ja seejärel aktiveeritud.

![Tulemüüri reegli aktiveerimine][18]
 
Kliendi haldus ülemises parempoolses nurgas on kobar nuppudest. Tulemüüri muudetud reegleid saatmiseks nuppu "Saada muudatused" ja seejärel nuppu "Aktiveeri".
 
Tulemüüri ruleset aktiveerimine selles näites keskkonna koostamine on lõpule viidud.

## <a name="traffic-scenarios"></a>Liikluse stsenaariumid
>[AZURE.IMPORTANT] Võtme takeway on meeles pidada, et **Kõik** liikluse tulevad tulemüüri kaudu. Nii kaugtöölaua IIS01 server, isegi juhul, kui see on ees End pilveteenuses ja esiosa alamvõrgu, sellele serverile juurdepääsu me ei vaja RDP port 8014 tulemüüri ja tulemüüri RDP taotluse ettevõttesiseselt marsruutimiseks IIS01 RDP Port ja seejärel lubada. Azure'i portaalis "Connect" nupp ei tööta sellepärast, et IIS01 pole otsene RDP tee (kui portaalis saate vaadata). See tähendab, et kõik ühendused Interneti kaudu ei saa turvalisuse teenuse ja portide, nt secscv001.cloudapp.net:xxxx.

Need stsenaariumid tulemüüri järgmised reeglid peavad olema kohas:

1.  Tulemüüri haldus
2.  Põhjused, mida IIS01
3.  Põhjused, mida DNS01
4.  Põhjused, mida AppVM01
5.  Põhjused, mida AppVM02
6.  Rakenduse liikluse veebis
7.  Rakenduse liikluse AppVM01
8.  Väljamineva meili Interneti-ühendus
9.  Frontend DNS01
10. Siseste alamvõrgu liikluse (tagasi end, et ainult esiosa)
11. Keela kõik

Tegelik tulemüüri ruleset tõenäoliselt on lisaks palju reegleid, on antud tulemüüri reegleid ka muu prioriteet numbrid, kui need siin loetletud. See loendist ja seotud arvud on anda ainult need üksteist reeglid ja nende hulgas tähtsuse vahel. Teisisõnu; tegelik tulemüüri, võib "RDP-IIS01" reegli arvu 5, kuid kui see on all reegli "Tulemüüri haldus" ja "RDP DNS01" reegli kohal soovite joondada eesmärgiga selles loendis. Loendis kuvatakse ka abi selle all stsenaariumid, mis võimaldab lühike; nt "FW reegli 9 (DNS)". Ka jaoks lühike, neli RDP reegleid saab ühiselt nimeks "RDP reeglite" kui liikluse stsenaarium on seotud RDP.

Ka meelde, et võrgu turberühmad on kohapealne jaoks sissetuleva Interneti-liikluse Frontend ja Taustväärtus alamvõrku kohta.

#### <a name="allowed-internet-to-web-server"></a>(Lubatud) Interneti veebiserverisse
1.  Interneti kasutaja taotlused HTTP lehel SecSvc001.CloudApp.Net (Internet vastastikuste pilveteenus)
2.  Pilveteenuse teenuse möödub liikluse kaudu avatud lõpp-punkti port 80 tulemüüri liides 10.0.0.4:80
3.  Pole NSG määratud turvalisus alamvõrku, seega süsteemi NSG reeglite luba liiklust tulemüüri
4.  Liikluse tabab sisemise IP-aadress tulemüüri (10.0.1.4)
5.  Tulemüüri algab reeglite töötlemine:
  1.    FW reegli 1 (FW Mgmt) ei rakendada, järgmise reegli liikumine
  2.    FW reeglid 2 – 5 (RDP reeglid) ei rakendada, järgmise reegli liikumine
  3.    FW reegli 6 (rakenduse: Web) rakendamine liikluse on lubatud, tulemüüri NATs seda 10.0.1.4 (IIS01)
6.  Frontend alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (Blokeeri Internet) ei kehti (see liiklus oli NAT oleks tulemüür, seega allikas aadress on nüüd tulemüüri, mis on alamvõrgu turvalisus ja näha Frontend alamvõrgu NSG teel, et "kohalikud" liikluse ja on seega lubatud), teisaldamine järgmise reegli
  2.    Alamvõrgu abil alamvõrgu liikluse lubamiseks NSG Vaikereeglit, liikluse on lubatud, NSG reeglite töötlemise lõpetamine
7.  IIS01 on kuulamine web liikluse, saab see taotlus ja käivitab päringu töötlemise
8.  IIS01 üritab käivitab FTP seansi Taustväärtus alamvõrgu AppVM01 abil
9.  UDR marsruutimiseks Frontend alamvõrgu teeb tulemüüri kuvatakse järgmise sõnumihüppe kohta, pärast
10. Väljamineva reegleid Frontend alamvõrgu, liikluse on lubatud
11. Tulemüüri algab reeglite töötlemine:
  1.    FW reegli 1 (FW Mgmt) ei rakendada, järgmise reegli liikumine
  2.    FW reegel 2 – 5 (RDP reeglid) ei rakendada, järgmise reegli liikumine
  3.    FW reegli 6 (rakenduse: Web) ei kehti, teisaldamine järgmise reegli
  4.    FW reegli 7 (rakenduse: kirjutamata) rakendamine liikluse on lubatud, tulemüüri edastab liikluse 10.0.2.5 (AppVM01)
12. Taustväärtus alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (Blokeeri Internet) ei rakendada, järgmise reegli liikumine
  2.    Alamvõrgu abil alamvõrgu liikluse lubamiseks NSG Vaikereeglit, liikluse on lubatud, NSG reeglite töötlemise lõpetamine
13.  AppVM01 saab taotluse ja käivitab seanss ja vastab
14. UDR marsruutimiseks kirjutamata alamvõrgu teeb tulemüüri kuvatakse järgmise sõnumihüppe kohta, pärast
15. Kuna seal on NSG reegleid ei väljamineva Taustväärtus alamvõrgu vastus on lubatud
16. Kuna see on tagasi liikluse on loodud seansi tulemüüri edastab vastuse veebiserver (IIS01)
17. Frontend alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (Blokeeri Internet) ei rakendada, järgmise reegli liikumine
  2.    Alamvõrgu abil alamvõrgu liikluse lubamiseks NSG Vaikereeglit, liikluse on lubatud, NSG reeglite töötlemise lõpetamine
18. IIS-i server saab vastuse, tehing koos AppVM01 on lõpule viidud ja seejärel lõpetab koostamise HTTP vastuse, HTTP-vastus saadetakse imperatiivsus päringu
19. Kuna seal on Frontend alamvõrgu vastus väljaminev NSG reegleid ei on lubatud
20. HTTP vastuse tabab tulemüüri ja kuna see on loodud NAT-seansiga vastus aktsepteerib tulemüüri
21. Tulemüüri seejärel suunab vastuse Interneti kasutaja
22. Kuna seal on ei väljaminev NSG reeglid või UDR humala Frontend alamvõrgu vastus on lubatud ja kasutajale Interneti saab nõutav veebilehele.

#### <a name="allowed-internet-rdp-to-backend"></a>(Lubatud) Interneti-kirjutamata RDP
1.  Serveri administraator Internetis taotleb RDP-seansi AppVM01 kaudu SecSvc001.CloudApp.Net:8025, kus 8025 on kasutaja määratud pordi number "RDP AppVM01" tulemüüri reegli abil
2.  Pilveteenusesse edastab liikluse kaudu avatud lõpp-punkti Port 8025 tulemüüri liides 10.0.0.4:8025
3.  Pole NSG määratud turvalisus alamvõrku, seega süsteemi NSG reeglite luba liiklust tulemüüri
4.  Tulemüüri algab reeglite töötlemine:
  1.    FW reegli 1 (FW Mgmt) ei rakendada, järgmise reegli liikumine
  2.    FW reegli 2 (RDP IIS) ei rakendada, järgmise reegli liikumine
  3.    FW reegli 3 (RDP DNS01) ei rakendada, järgmise reegli liikumine
  4.    FW reegli 4 (RDP AppVM01) rakendamine, liikluse on lubatud, tulemüüri NATs seda 10.0.2.5:3386 (RDP-porti AppVM01)
5.  Taustväärtus alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (Blokeeri Internet) ei kehti (see liiklus oli NAT oleks tulemüür, seega allikas aadress on nüüd tulemüüri, mis on alamvõrgu turvalisus ja näinud Taustväärtus alamvõrgu NSG olema "Kohalik" liikluse ja on seega lubatud), teisaldamine järgmise reegli
  2.    Alamvõrgu abil alamvõrgu liikluse lubamiseks NSG Vaikereeglit, liikluse on lubatud, NSG reeglite töötlemise lõpetamine
6.  AppVM01 on listening RDP-liikluse ja vastab
7.  Väljamineva NSG reegleid ei vaikereeglit rakendada ja tagastada liikluse on lubatud
8.  UDR marsruudib väljaminev liiklus Firewall on järgmine sõnumihüppe kohta, pärast nimega
9.  Kuna see on tagasi liikluse on loodud seansi tulemüüri edastab vastuse Interneti kasutaja
10. RDP-seansi on lubatud
11. AppVM01 küsib kasutajanime ja parooli

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Lubatud) DNS-i serveris web serveri DNS-i otsing
1.  Web Server, IIS01, vajadustele andmekanali, www.data.gov juures, kuid vajadustele aadressi lahendamiseks.
2.  Võrgukonfiguratsioon VNet loendite DNS01 (10.0.2.4 Taustväärtus alamvõrgu) esmane DNS-i server, IIS01 saadab DNS-i taotluse DNS01
3.  UDR marsruudib väljaminev liiklus Firewall on järgmine sõnumihüppe kohta, pärast nimega
4.  Väljamineva NSG reegleid ei on seotud Frontend alamvõrku, liikluse on lubatud
5.  Tulemüüri algab reeglite töötlemine:
  1.    FW reegli 1 (FW Mgmt) ei rakendada, järgmise reegli liikumine
  2.    FW reegel 2 – 5 (RDP reeglid) ei rakendada, järgmise reegli liikumine
  3.    FW reeglid 6 ja 7 (rakenduse reeglid) ei rakendada, järgmise reegli liikumine
  4.    FW reegel 8 (Internet) ei rakendada, järgmise reegli liikumine
  5.    FW reegli 9 (DNS-i) rakendamine, liikluse on lubatud, tulemüüri edastab liikluse 10.0.2.4 (DNS01)
6.  Taustväärtus alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (Blokeeri Internet) ei rakendada, järgmise reegli liikumine
  2.    Alamvõrgu abil alamvõrgu liikluse lubamiseks NSG Vaikereeglit, liikluse on lubatud, NSG reeglite töötlemise lõpetamine
7.  DNS-serveri saab taotluse
8.  DNS-i server pole vahemälus talletatud aadressi ja palub juur DNS-i serveri Internet
9.  UDR marsruudib väljaminev liiklus tulemüüri nimega kuvatakse järgmise sõnumihüppe kohta, pärast
10. Väljamineva NSG reegleid ei Taustväärtus alamvõrgu, liikluse on lubatud
11. Tulemüüri algab reeglite töötlemine:
  1.    FW reegli 1 (FW Mgmt) ei rakendada, järgmise reegli liikumine
  2.    FW reegel 2 – 5 (RDP reeglid) ei rakendada, järgmise reegli liikumine
  3.    FW reeglid 6 ja 7 (rakenduse reeglid) ei rakendada, järgmise reegli liikumine
  4.     FW reegel 8 (Internet) rakendamine, liikluse on lubatud, seansi on SNAT välja juur DNS-i serveri Internet
12. Interneti DNS-i server vastab, kuna selle seansi algusest tulemüüri, vastuse aktsepteerib tulemüüri
13. See on on loodud seansi, tulemüüri edastab vastuse algatanud server, DNS01
14. Taustväärtus alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (Blokeeri Internet) ei rakendada, järgmise reegli liikumine
  2.    Alamvõrgu abil alamvõrgu liikluse lubamiseks NSG Vaikereeglit, liikluse on lubatud, NSG reeglite töötlemise lõpetamine
15. DNS-i server saab vastuse vahemälu ja seejärel vastuseks algse naasmiseks IIS01
16. UDR marsruutimiseks kirjutamata alamvõrgu teeb tulemüüri kuvatakse järgmise sõnumihüppe kohta, pärast
17. Väljamineva NSG reegleid ei olemas Taustväärtus alamvõrku, liikluse on lubatud
18. See on on loodud seansi tulemüüri, vastus edastatakse tulemüür uuesti IIS-i server
19. Frontend alamvõrgu algab Sissetulev reegel töötlemine:
  1.    Ei ole NSG reegel, mis kehtib sissetulev liiklus Taustväärtus alamvõrgu Frontend alamvõrku, nii et ükski selle NSG reeglite rakendamine
  2.    Vaikimisi süsteemi reegli lubamisel vahel alamvõrku võimaldab see liiklus nii, et liiklus on lubatud
20. IIS01 saab vastuse DNS01

#### <a name="allowed-backend-server-to-frontend-server"></a>(Lubatud) Frontend Taustväärtus serverist
1.  AppVM02 kaudu RDP sisse logitud administraatorina taotleb faili otse IIS01 serveri kaudu Windowsi file Exploreris
2.  UDR marsruutimiseks kirjutamata alamvõrgu teeb tulemüüri kuvatakse järgmise sõnumihüppe kohta, pärast
3.  Kuna seal on NSG reegleid ei väljamineva Taustväärtus alamvõrgu vastus on lubatud
4.  Tulemüüri algab reeglite töötlemine:
  1.    FW reegli 1 (FW Mgmt) ei rakendada, järgmise reegli liikumine
  2.    FW reegel 2 – 5 (RDP reeglid) ei rakendada, järgmise reegli liikumine
  3.    FW reeglid 6 ja 7 (rakenduse reeglid) ei rakendada, järgmise reegli liikumine
  4.    FW reegel 8 (Internet) ei rakendada, järgmise reegli liikumine
  5.    FW reegli 9 (DNS) ei rakendada, järgmise reegli liikumine
  6.    FW reegli 10 (Sisene alamvõrgu) rakendamine, liikluse on lubatud, tulemüüri liikluse läbib 10.0.1.4 (IIS01)
5.  Frontend alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (Blokeeri Internet) ei rakendada, järgmise reegli liikumine
  2.    Alamvõrgu abil alamvõrgu liikluse lubamiseks NSG Vaikereeglit, liikluse on lubatud, NSG reeglite töötlemise lõpetamine
6.  Eeldades, et proper autentimise ja luba, IIS01 taotluse vastu võtab ja vastab
7.  UDR marsruutimiseks Frontend alamvõrgu teeb tulemüüri kuvatakse järgmise sõnumihüppe kohta, pärast
8.  Kuna seal on Frontend alamvõrgu vastus väljaminev NSG reegleid ei on lubatud
9.  Kui see on mõne olemasoleva seansi tulemüüris see vastus on lubatud ja tulemüüri annab vastuseks AppVM02
10. Taustväärtus alamvõrgu algab Sissetulev reegel töötlemine:
  1.    NSG reegli 1 (Blokeeri Internet) ei rakendada, järgmise reegli liikumine
  2.    Alamvõrgu abil alamvõrgu liikluse lubamiseks NSG Vaikereeglit, liikluse on lubatud, NSG reeglite töötlemise lõpetamine
11. AppVM02 saab vastuse

#### <a name="denied-internet-direct-to-web-server"></a>(Keelatud) Interneti otse veebiserverisse
1.  Interneti-kasutaja proovib juurdepääs veebiserverile, IIS01, FrontEnd001.CloudApp.Net teenuse kaudu
2.  Kuna ei ole lõpp HTTP liikluse avatud, see ei oleks läbida pilveteenusesse ja ei jõua server
3.  Kui mingil põhjusel olid avatud lõpp-punktid, NSG (Blokeeri Internet) Frontend alamvõrgu blokeeritaks see liiklus
4.  Lõpuks Frontend alamvõrgu UDR marsruutimiseks soovite saata mis tahes väljaminev liiklus IIS01 nimega kuvatakse järgmise sõnumihüppe kohta, pärast tulemüüri ja tulemüüri oleks on see asümmeetriline liikluse ja kukutage Väljamineva meili vastust, seega on vähemalt kolme sõltumatu kihti kaitsevallid vahel Interneti- ja IIS01 kaudu oma pilveteenuses sobimatut/volitamata juurdepääsu vältimiseks.

#### <a name="denied-internet-to-backend-server"></a>(Keelatud) Internet Taustväärtus serveris
1.  Interneti-kasutaja proovib juurde AppVM01 faili BackEnd001.CloudApp.Net teenuse kaudu
2.  Kuna see on avatud faili ühiskasutusse andmine pole lõpp-punkte, seda ei liigu pilveteenusesse ja ei jõua server
3.  Kui mingil põhjusel olid avatud lõpp-punktid, NSG (Blokeeri Internet) blokeeritaks see liiklus
4.  Lõpuks UDR marsruutimiseks soovite saata mis tahes väljaminev liiklus AppVM01 nimega kuvatakse järgmise sõnumihüppe kohta, pärast tulemüüri ja tulemüüri oleks on see asümmeetriline liikluse ja kukutage Väljamineva meili vastust, seega on vähemalt kolme sõltumatu kihti kaitsevallid vahel Interneti- ja AppVM01 kaudu oma pilveteenuses sobimatut/volitamata juurdepääsu vältimiseks.

#### <a name="denied-frontend-server-to-backend-server"></a>(Keelatud) Kirjutamata frontend serverist
1.  Oletame IIS01 turvalisust on rikutud ja proovite skannida Taustväärtus alamvõrgu serverid pahatahtlik kood töötab.
2.  Frontend alamvõrgu UDR marsruutimiseks soovite saata mis tahes väljaminev liiklus kaudu IIS01 Firewall on järgmine sõnumihüppe kohta, pärast. See pole midagi, mida saate muuta rikutud VM.
3.  Tulemüüri töötleb liikluse, kui taotlus oli AppVM01 või DNS-i server DNS otsingud, mis liikluse potentsiaalselt olema lubatud tulemüür (tõttu FW reeglid 7 ja 9). Kõik muud liikluse soovite blokeerida FW reegli 11 (Keela kõik).
4.  Kui Täpsemad ohtude tuvastamise on lubatud tulemüüris (mis ei kuulu selles dokumendis, dokumentatsioonist tarnija oma võrgu seadme Täpsemad ohtu võimaluste) isegi liikluse, mida oleks lihtne ümbersuunamine reeglid, mida käsitletakse selles dokumendis lubatud oleks ära hoida, kui liiklus teadaolevad allkirjad või lipuga reegli täpsemalt ohtu mustreid.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Keelatud) DNS-i serveris Interneti DNS-i otsing
1.  Interneti-kasutaja proovib otsingul sisemise DNS-i kirje kohta DNS01 BackEnd001.CloudApp.Net teenuse kaudu 
2.  Kuna ei ole lõpp DNS-i liikluse avatud, see ei oleks läbida pilveteenusesse ja ei jõua server
3.  Kui mingil põhjusel olid avatud lõpp-punktid, NSG reegli (Blokeeri Internet) Frontend alamvõrgu blokeeritaks see liiklus
4.  Lõpuks kirjutamata alamvõrgu UDR marsruutimiseks soovite saata mis tahes väljaminev liiklus DNS01 nimega kuvatakse järgmise sõnumihüppe kohta, pärast tulemüüri ja tulemüüri oleks on see asümmeetriline liikluse ja kukutage Väljamineva meili vastust, seega on vähemalt kolme sõltumatu kihti kaitsevallid vahel Interneti- ja DNS01 kaudu oma pilveteenuses sobimatut/volitamata juurdepääsu vältimiseks.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Keelatud) SQL-i juurdepääs läbi tulemüüri Internet
1.  Interneti-kasutaja taotleb SQL-i andmete SecSvc001.CloudApp.Net (Internet vastastikuste pilveteenus)
2.  Kuna ei ole lõpp avatud SQL-i, seda ei liigu pilveteenusesse ja ei jõua tulemüüri
3.  Kui mingil põhjusel olid avatud SQL-i lõpp-punktid, algaks tulemüüri reeglite töötlemine:
  1.    FW reegli 1 (FW Mgmt) ei rakendada, järgmise reegli liikumine
  2.    FW reeglid 2 – 5 (RDP reeglid) ei rakendada, järgmise reegli liikumine
  3.    FW reegli 6 ja 7 (rakenduse reeglid) ei rakendada, järgmise reegli liikumine
  4.    FW reegel 8 (Internet) ei rakendada, järgmise reegli liikumine
  5.    FW reegli 9 (DNS) ei rakendada, järgmise reegli liikumine
  6.    FW reegli 10 (Sisene alamvõrgu) ei rakendada, järgmise reegli liikumine
  7.    FW reegli 11 (Keela kõik) rakendamine, liikluse on blokeeritud, peatage reeglite töötlemine


## <a name="references"></a>Viited
### <a name="main-script-and-network-config"></a>Peamised skripte ja võrgu Config
Täielik skripti PowerShelli skripti faili salvestada. Salvestage fail nimega "NetworkConf2.xml" võrgu Config.
Muutke kasutaja määratletud muutujaid vastavalt vajadusele. Skripti, siis järgige tulemüüri reegli häälestamise eespool.

#### <a name="full-script"></a>Täielik skripti
Kuvatakse selle skripti, kasutaja määratletud muutujate põhjal:

1.  Ühenduse loomine Azure tellimuse
2.  Mäluruumi uue konto loomine
3.  Uue VNet ja kolme alamvõrku, nagu on määratletud võrgu Config faili loomine
4.  Koostage viis virtuaalmasinates; tulemüüri 1 ja 4 windows server VMs
5.  Konfigureerige UDR, sh:
  1.    Kaks uut marsruutimiseks tabelite loomine
  2.    Marsruudib tabelite lisamine
  3.    Tabelite sidumiseks sobiv alamvõrku
6.  Funktsiooni Dessandi IP saatmise lubamine
7.  Saate konfigureerida, sh NSG.
  1.    Mõne NSG loomine
  2.    Reegli lisamine
  3.    Sobiv alamvõrku on NSG sidumine

See PowerShelli skripti peaks töötama kohalikult sisse Interneti ühendatud arvuti või serveri.

>[AZURE.IMPORTANT] See skript käitamisel võib olla hoiatused või muud informatiivsed teated, mis PowerShellis pop. Punase tooniga ainult tõrketeated on muret.

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwading from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA
    
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.
    
      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4
     
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4
     
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
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
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
    
    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}
    
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
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
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
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }
    
    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan
    
      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"
    
      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal
    
      # Assoicate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName
    
     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
    
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the Frontend and Backback subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
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
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
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
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Kahesuunalise DMZ Dessandi, NSG ja UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Loogika vaate tulemüüri reeglid"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "FrontEnd võrgu objekti loomine"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "DNS-i Server objekti loomine"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Koopia Default RDP reegel"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 reegel"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Redirect ikoon"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Sihtkoha NAT ikoon"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Liigu ikoon"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Tulemüüri haldus reegel"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "Tulemüüri RDP reegel"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Tulemüüri Web reegel"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Tulemüüri AppVM01 reegel"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Tulemüüri Väljaminev reegel"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "Tulemüüri DNS-i reegel"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Tulemüüri sisene-VNet reegel"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Tulemüüri keelamiseks reegel"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Tulemüüri reegli aktiveerimine"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
