<properties
   pageTitle="Hübriidjuurutuse võrgu arhitektuur Azure'i ja kohapealsete VPN rakendamine | Microsoft Azure'i"
   description="Kuidas rakendada-saidilt võrgu arhitektuur, mis hõlmavad on Azure virtuaalse võrgu ja kohapealse võrgu, mis ühendatud VPN-i abil."
   services=""
   documentationCenter="na"
   authors="RohitSharma-pnp"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="roshar"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-and-on-premises-vpn"></a>Hübriidjuurutuse võrgu ülesehituse Azure ja kohapealse VPN rakendamine

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

See artikkel kirjeldab tavade pikendamiseks on kohapealse võrgu peale Azure-saidilt virtuaalse privaatvõrgu (VPN) abil. Liiklus liigub sujuvalt kohapealse võrgu ja muu Azure virtuaalse kaudu (VNet) kaudu on IPSec VPN tunneliga vahel. See arhitektuur sobib hübriid rakenduste järgmised omadused:

- Rakenduse osades käivitage kohapealse ajal teistega käivitada Azure.

- Omavahel kohapealse riistvara ja pilveteenuses tõenäoliselt lihtversioon või on kasulik nõuded on veidi laiendatud latentsus paindlikkust ja töötlemine power pilve.

- Laiendatud võrgu kujutab suletud süsteem. On pole otsene tee Azure'i VNet Interneti kaudu.

- Kasutajate ühenduse kohapealse võrgu majutatud Azure teenuste kasutamiseks. Selle seose kohapealse võrgu ja Azure töötavate teenuste vahel on läbipaistev kasutajatele.

Stsenaariumid, mis sobivad seda profiili näited.

- Ärivaldkonna rakendusi kasutada ettevõttes, kus osa funktsioone viiakse pilveteenusesse.

- Arengu/katselabori töökoormus.

> [AZURE.NOTE] Azure'i on kaks erinevat juurutamise mudelit: [Azure'i ressursihaldur] [ resource-manager-overview] ja klassikaline. See näidis kasutab ressursihaldur, mis soovitab Microsoft kasutada uut juurutuste.

## <a name="architecture-diagram"></a>Arhitektuur skeem

Järgmine diagramm komponentide arhitektuuri selle olulisemad toimingud.

> Visio dokumendi, mis sisaldab arhitektuur diagramm on saadaval alla laadida [Microsoft allalaadimiskeskuse][visio-download]. See diagramm on "hübriid võrgu - VPN" leht.

![[0]][0]

- **Kohapealse võrgu.** See on võrgu arvutites ja seadmetes, kuvatakse ettevõttes töötavate kohaliku piirkonna privaatvõrgu kaudu ühendatud.

- **VPN-seade.** See on seadme või teenus, mis sisaldab välise ühenduvuse kohapealse võrku. VPN-seade võib olla riistvara või see võib olla näiteks marsruutimine ja Remote Access Service (RRAS) Windows Server 2012 tarkvara lahendus.

> [AZURE.NOTE] Loendi toetatud VPN-ja teavet valitud VPN seadmete ühenduse Azure'i VPN-lüüsi konfigureerimise kohta leiate teemast juhised soovitud seade [ei toeta Azure VPN seadmete loendi][vpn-appliance].

- **N-astme pilve rakendus.** See on majutatud Azure rakendus. See võib sisaldada mitmetasandiline, mitu alamvõrku Azure koormus soolise kaudu ühendatud abil. Iga alamvõrgu liiklus võib-olla maksma reeglid, mis on määratletud [Azure'i võrgu turberühmad (NSGs)]abil[azure-network-security-group]. Lisateabe saamiseks vaadake teemat [Alustamine Microsoft Azure'i turvalisusega][getting-started-with-azure-security].

> [AZURE.NOTE] Selles artiklis kirjeldatakse ühtse tervikuna pilves rakendus. Töötab [N-astme arhitektuur Azure] [ implementing-a-multi-tier-architecture-on-Azure] üksikasjalikku teavet.

- **[Virtuaalse võrgu (VNet)][azure-virtual-network].** Pilve rakendus ja Azure VPN lüüsi komponendid asuma samas VNet.

- **[Azure'i VPN-lüüsi][azure-vpn-gateway].** VPN-lüüsi teenuse võimaldab teil on VNet ühenduse kohapealse võrgu VPN seadme kaudu. Lisateavet leiate teemast [ühenduse loomine mõne kohapealse võrgu loomine Microsoft Azure virtuaalse][connect-to-an-Azure-vnet]. VPN-lüüsi hõlmab järgmisi elemente:

  - **Virtuaalse võrgu lüüsi.** See on ressurss, mis pakub virtuaalse VPN-seadme jaoks soovitud VNet. See vastutab marsruutimise liikluse kohapealse võrgu kaudu soovitud VNet.

  - **Lüüsi kohalikus võrgus.** See on kõrvaldamise kohapealse VPN seade. Pilve rakendus kohapealse võrgu võrguliikluse marsruuditakse selle lüüsi kaudu.

  - **Ühendus.** Ühendus on atribuudid, mis määravad ühenduse tüüp (IPSec) ja kohapealse VPN seadme ühiskasutusse klahvi liikluse krüptimiseks.

  - **Lüüsi alamvõrgu.** Virtuaalse võrgu lüüsi toimub omaette alamvõrku, mis on erinevate nõudeid, nagu on kirjeldatud allpool jaotises soovitusi.

- **Sisemise koormuse koormusetasakaalustusteenuse.** Lüüsi VPN-i võrguliikluse suunatakse pilve rakendus on sisemine laadi koormusetasakaalustusteenuse kaudu. Laadi koormusetasakaalustusteenuse asub ees alamvõrgu rakenduse.

## <a name="recommendations"></a>Soovitused

Azure'i pakub palju erinevaid ressursse ja ressursside tüübid, nii, et see viide arhitektuur võib olla ette valmistatud mitmel erineval viisil. Oleme pakkunud viide arhitektuur soovituste järgneva installimiseks on Azure ressursihaldur malli. Kui soovite luua oma viite arhitektuur peaksite nende soovituste täitmisel juhul, kui teil on konkreetse nõude, mis ei sobi soovitus.

### <a name="vnet-and-gateway-subnet"></a>VNet ja lüüsi alamvõrgu

Luua ka Azure VNet hoides komponendid pilveteenuses. Azure'i VNet aadressiruumi peab olema piisavalt suur, et aadressid, mida kasutatakse VMs ja selle VNet alamvõrku. Tagada, et VNet aadressiruumi on piisavalt ruumi kasvu, kui täiendavad VMs võivad olla tulevikus vaja. Funktsiooni VNet aadressiruumi peab ei kattu kohapealse võrgu. Näiteks ülaltoodud diagrammi kasutab aadress ruumi 10.20.0.0/16 on VNet.

Looge alamvõrku, mis on nimega _GatewaySubnet_, mis aadressi hulk /27. Selle alamvõrgu nõuab virtuaalse võrgu lüüsi ja eraldamise 32-aadressid selles alamvõrgu aitab vältida teil tulevikus võimalik lüüsi mahupiirangud vastu töötab. Vältige selle alamvõrgu aadress ruumi keskele. Hea tava on aadressiruumi jaoks lüüsi alamvõrgu VNet aadressiruumi ülemises otsas. Skemaatilise diagrammi siinse näide kasutab 10.20.255.224/27.  Kiirülevaate protseduuri arvutamiseks soovitud CIDR on järgmine:

1. Määrake muutuv bittide VNet 1 kuni bittide on kasutatud lüüsi alamvõrgu, siis ülejäänud bittide väärtuseks 0 aadressiruumi.

2. Tulemuseks bittide teisendamine kümnendarvuks ja kiire selle nimega aadressiruumi eesliite pikkus on seatud mahu lüüsi alamvõrgu.

Näiteks VNet, mis IP address hulk 10.20.0.0/16, rakendades samm #1 ülaltoodud muutub 10.20.0b11111111.0b11100000.  Teisendamine koma ja selle avaldada, nagu aadressiruumi annab 10.20.255.224/27

> [AZURE.WARNING] Juurutada muude virtuaalmasinates või rolli aknad lüüsi alamvõrgu. Ka ei anna soovitud NSG selle alamvõrgu see põhjustab lüüsi enam töötada.

### <a name="virtual-network-gateway"></a>Virtuaalse võrgu lüüsi

Eraldada virtuaalse võrgu lüüsi avaliku IP-aadressi.

Lüüsi alamvõrgu virtuaalse võrgu lüüsi loomine ja selle määramine äsja eraldatud avaliku IP-aadressi. Kasutage lüüsi tüüp, mis sobib kõige paremini teie vajadustele ja mis on lubanud seadme VPN:

[Poliitika-põhise lüüsi] loomine[ policy-based-routing] kui teil on vaja hoolikalt määrata, kuidas taotlused marsruuditakse. näiteks aadressi eesliiteid poliitika kriteeriumide alusel. Poliitika-põhise lüüside kasutada staatiline marsruutimine ja töötavad ainult saidilt ühendused.

Luua [lüüsi marsruutimiseks-põhine] [ route-based-routing] kui loote ühenduse kohapealse võrgu kaudu RRAS, mitme saidi või rist-piirkond ühenduste toetamiseks või rakendada VNet-VNet ühendused (sh marsruudib, mis läbida mitme VNets). Lüüside marsruutimiseks vastavalt kasutada dünaamiline marsruutimine otse liikluse vahel. Need saate parem staatilise marsruudib, kui võrguteed tõrked luba, kuna need võite proovida viise. Lüüside marsruutimiseks vastavalt saate vähendada ka halduse pea kohal, kuna marsruudib võib pole vaja käsitsi värskendada, kui aadressidele muutmine.

Toetatud VPN seadmete loendi leiate teemast [kohta VPN-saidilt VPN-lüüsi ühendused seadmete][vpn-appliances].

> [AZURE.NOTE] Pärast lüüsi loomist ei saa muuta ilma kustutamine ja uuesti luua lüüsi lüüsi vahel.

Valige Azure VPN lüüsi SKU soovitule kõige täpsemini vastava oma läbilaskevõime nõuete. Azure'i VPN lüüs on näidatud järgmises tabelis kolmest SKU-des saadaval. Iga SKU pakub erinevaid läbilaskevõime, [võetakse tasu] [ azure-gateway-charges] põhjal aeg, et lüüsi on ette valmistatud ja saadaval.

| SKU-GA | VPN-i jõudlus | Max IPSec tunnelid|
|-----|----------------|------------------|
| Tavaline | 100 Mbps | 10 |
| Standard | 100 Mbps | 10 |
| Suure jõudlusega | 200 Mbps | 30 |

> [AZURE.NOTE] Tavaline SKU ei ühildu Azure'i ExpressRoute. Saate [muuta selle SKU] [ changing-SKUs] pärast lüüsi loomist.

Saate luua marsruutimise reeglid lüüsi alamvõrgu selle otse rakenduse liiklust ühest lüüsist sisemise koormuse koormusetasakaalustusteenuse mitte võimaldades taotlusi edasi, et rakendus VMs.

### <a name="on-premises-network-connection"></a>Kohapealse võrguühendus

Saate luua lüüsi kohalikus võrgus. Määrake avaliku IP-aadress kohapealse VPN seade ja aadressiruumi kohapealse võrgu. Pange tähele, et kohapealse VPN seade peab pääseb kohtvõrgu lüüsi Azure'i VPN-lüüsi avaliku IP-aadress. VPN-seade ei leita taga NAT seade.

Virtuaalse võrgu lüüsi ja kohtvõrgu lüüsi saidilt ühenduse luua. Valige saidi saidi (IPSec) ühenduse tüüp ja Määrake ühiskasutusega võti. Saitide krüptimise abil Azure'i VPN-lüüsi põhineb eelnevalt jagatud klahvide abil autentimise IPSec protokolli. Azure'i VPN-lüüsi loomisel Määrake võti. Tuleb konfigureerida kohapealse sama võti töötab VPN seade. Muud autentimise menetlustele praegu ei toetata.

Veenduge, et kohapealse marsruutimise infrastruktuur on konfigureeritud edasi taotlusi mõeldud aadressid Azure'i VNet VPN seadmele.

Avage mis tahes pordid nõuab cloud kohapealse võrku.

Veenduge, et ühenduse testimiseks tehke järgmist.

- Kohapealse VPN seade õigesti marsruudib liikluse pilve rakendus Azure'i VPN lüüsi kaudu.

- Funktsiooni VNet õigesti marsruudib liikluse tagasi kohapealse võrgu.

- Keelatud liikluse mõlemas on blokeeritud õigesti.

## <a name="scalability-considerations"></a>Kaalutlused skaleeritavus.

Suure jõudlusega VPN SKU Basic või Standard VPN lüüsi SKU-de jaoks teisaldades võite saavutada piiratud vertikaalne skaleeritavus.

Oodatav suuremahulise VPN-liikluse VNets, on soovitatav eri töökoormus üheks eraldi väiksem VNets levitamise ja neid VPN-lüüsi konfigureerimise.

Horisontaalselt või vertikaalselt, saate selle VNet partition. Sektsiooni horisontaalselt, et liiguvad mõnel juhul VM iga taseme alamvõrku, uue VNet. Tulem on, et iga VNet on sama struktuuri ja funktsioonid. Sektsiooni vertikaalselt, ümberkujundamiseks iga taseme funktsioonid jagada eri loogilise alasid (nt tellimuste töötlemine, arvete, kliendi konto haldamine jne). Iga funktsionaalne ala siis oma VNet paigutada.

Imitatsiooniga on kohapealse Active Directory domeenikontrolleri on VNet ja DNS-i rakendamisel VNet, aitab vähendada mõne seotud turbe- ja halduskulud liiklus tulenevad kohapealse pilveteenusesse.

## <a name="availability-considerations"></a>Kättesaadavus kaalutlused

Kui teil on vaja tagada, et kohapealse võrgu saadaval Azure VPN-lüüsi, rakendada tõrkesiirdeklastrite kohapealse VPN-lüüsi.

Kui teie ettevõttel on mitu kohapealse saidid, [mitme saidi ühenduste] loomine[ vpn-gateway-multi-site] ühe või mitme Azure'i VNets abil. Seda moodust nõuab dünaamiline (marsruutimiseks-põhine) marsruutimine, seega veenduge, et kohapealse VPN-lüüsi toetab seda funktsiooni.

Vt [SLA VPN Gateway] [ sla-for-vpn-gateway] VPN lüüsi SLA üksikasjad.

## <a name="manageability-considerations"></a>Kaalutlused hallatavus

Kuvari diagnostikateave kohapealse VPN seadmete kaudu. Selle protsessi sõltub VPN seadme saadaolevad funktsioonid. Näiteks kui kasutate Windows Server 2012 marsruutimine ja Remote Access Service, tuleks lubada [RRAS logimine][rras-logging].

Kasutage [Azure'i VPN-lüüsi diagnostika] [ gateway-diagnostic-logs] jäädvustada teavet ühenduvusprobleemide kohta. Neid logisid saab jälgida lähte- ja sihtkohad ühenduse taotlusi, millist protokolli kasutati, ja kuidas ühendus on loodud (või miks katse nurjus) näiteks järgmist teavet.

Azure'i portaalis auditilogide abil jälgida Azure'i VPN lüüsi funktsionaalseid logid. Eraldi logid kohtvõrgu lüüsi, Azure võrgu lüüsi ja ühenduse saadaval. Seda teavet saab kasutada mis tahes lüüsi tehtud muudatuste jälitamine ja võib olla kasulik, kui varem toimiva lüüsi lakkab töötamast mingil põhjusel.

![[2]][2]

Ühenduvus jälgimine ja ühenduvuse tõrge sündmustel silma peal hoidmiseks. Saate kasutada jälgimise pakett nagu [Nagios] [ nagios] jäädvustada ja teatavad, et seda teavet.

## <a name="security-considerations"></a>Turvalisuse alused

Ühiskasutusega tähtsamad iga VPN-lüüsi jaoks luua. Keeruka ühiskasutusega klahvi abil brute-Jõusta eest seisma.

> [AZURE.NOTE] Praegu ei saa kasutada Azure klahvi Vault eelnevalt jagatud võtmed Azure'i VPN-lüüsi.

Veenduge, et kohapealse VPN seadme kasutab krüptimismeetod, mis ühildub [Azure'i VPN-lüüsi][vpn-appliance-ipsec]. Poliitika-põhise marsruutimiseks Azure'i VPN-lüüsi toetab AES256, AES128 ja 3DES krüptimise algoritmid. AES256 ja 3DES marsruutimiseks vastavalt lüüside toega.

Kui teie kohapealse VPN seadme perimeetri võrgus, mis on tulemüür vahel perimeetri võrk ja Internet, peate konfigureerida [täiendavaid tulemüüri reeglid] [ additional-firewall-rules] lubamiseks-saidilt VPN-ühendus.

Kui soovitud VNet rakenduse saadab andmed Interneti-ühendus, võiksite [jõustatud tunneling] [ forced-tunneling] kohapealse võrgu kaudu kogu seotud Interneti-liikluse marsruutimiseks. Seda moodust võimaldab rakenduse taristu kohapealse väljaminevate taotlusi audit.

> [AZURE.NOTE] Jõustatud tunneling võib mõjutada Ühenduvus Azure Services (selle salvestusruumi teenus) ja Windows litsentsi Manager.

## <a name="troubleshooting-considerations"></a>Kaalutlused tõrkeotsing

> [AZURE.NOTE] Külastage marsruutimine ja Remote Access ajaveebi üldist teavet [Levinumate tõrgete tõrkeotsingu sooritamiseks VPN-seotud][troubleshooting-vpn-errors].

Kui liikluse ei saa läbida VPN-ühendus, siis kontrollige järgmist.

### <a name="on-premises-vpn-appliance"></a>Kohapealse VPN seadme:

**Kohapealse VPN seade töötab õigesti?**

Märkige ruut mis tahes logifailid loodud VPN seadme vead ja ebaõnnestumist. Selle teabe asukoha sõltuvalt oma seade. Näiteks kui kasutate RRAS Windows Server 2012, saate PowerShelli käsk Kuva sündmuse tõrketeabe RRAS teenuse:

```
Get-EventLog -LogName System -EntryType Error -Source RemoteAccess | Format-List -Property *
```

Iga kirje atribuudi _sõnumi_ kirjeldatakse viga. Mõned levinud näited:

- Ei ole võimalik, et ühendus tõttu Azure'i VPN lüüsi RRAS VPN võrgukonfiguratsioon kasutajaliidese jaoks määratud vale IP-aadress:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {41, 3, 0, 0}
Index              : 14231
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: The network connection between your computer and
                     the VPN server could not be established because the remote server is not responding. This could
                     be because one of the network devices (e.g, firewalls, NAT, routers, etc) between your computer
                     and the remote server is not configured to allow VPN connections. Please contact your
                     Administrator or your service provider to determine which device may be causing the problem.
Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, The network connection between
                     your computer and the VPN server could not be established because the remote server is not
                     responding. This could be because one of the network devices (e.g, firewalls, NAT, routers, etc)
                     between your computer and the remote server is not configured to allow VPN connections. Please
                     contact your Administrator or your service provider to determine which device may be causing the
                     problem.}
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:26:02 PM
TimeWritten        : 3/18/2016 1:26:02 PM
UserName           :
Site               :
Container          :
```

- RRAS VPN võrgu kasutajaliidese konfiguratsioonis määratud on vale ühiskasutusega võti:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {233, 53, 0, 0}
Index              : 14245
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: IKE authentication credentials are unacceptable

Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, IKE authentication credentials are
                     unacceptable
                     }
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:34:22 PM
TimeWritten        : 3/18/2016 1:34:22 PM
UserName           :
Site               :
Container          :
```

Saate hankida sündmuselogi teavet katsete ühenduse RRAS teenuse kaudu, kasutades PowerShelli käsk:

```
Get-EventLog -LogName Application -Source RasClient | Format-List -Property *
```

Korral ei saa ühendust luua, sisaldab see Logi tõrkeid, mis näevad välja järgmine:

```
EventID            : 20227
MachineName        : on-prem-vm
Data               : {}
Index              : 4203
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : CoId={B4000371-A67F-452F-AA4C-3125AA9CFC78}: The user SYSTEM dialed a connection named
                     AzureGateway which has failed. The error code returned on failure is 809.
Source             : RasClient
ReplacementStrings : {{B4000371-A67F-452F-AA4C-3125AA9CFC78}, SYSTEM, AzureGateway, 809}
InstanceId         : 20227
TimeGenerated      : 3/18/2016 1:29:21 PM
TimeWritten        : 3/18/2016 1:29:21 PM
UserName           :
Site               :
Container          :
```

**On VPN seade õigesti marsruutimise liikluse Azure'i VPN lüüsi kaudu?**

Tööriist, näiteks [Pspingi] kasutamine[ psping] ühenduvuse ja marsruutimise kogu VPN-lüüsi kinnitamiseks. Näiteks Ühenduvus on kohapealse arvutist veebiserverile, mis asub selle VNet testimiseks käivitage järgmine käsk (asendamine `<<web-server-address>>` veebiserver aadress):

```
PsPing -t <<web-server-address>>:80
```

Kui arvuti kohapealse saate marsruutida liikluse veebiserverisse, peaksite nägema väljund sarnaneb järgmisega:

```
D:\PSTools>psping -t 10.20.0.5:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.0.5:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.0.5:80 (warmup): 6.21ms
Connecting to 10.20.0.5:80: 3.79ms
Connecting to 10.20.0.5:80: 3.44ms
Connecting to 10.20.0.5:80: 4.81ms

  Sent = 3, Received = 3, Lost = 0 (0% loss),
  Minimum = 3.44ms, Maximum = 4.81ms, Average = 4.01ms
```

Kui arvuti kohapealse ei saa suhelda määratud sihtkohta, kuvatakse sõnumite umbes järgmine:

```
D:\PSTools>psping -t 10.20.1.6:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.1.6:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.1.6:80 (warmup): This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80:
  Sent = 3, Received = 0, Lost = 3 (100% loss),
  Minimum = 0.00ms, Maximum = 0.00ms, Average = 0.00ms
```

**Kas kohapealne tulemüür võimaldab VPN-liikluse läbib? On õiged pordid avatud?**

Veenduge, et kohapealse VPN seadme kasutab krüptimismeetod, mis ühildub [Azure'i VPN-lüüsi][vpn-appliance].

Poliitika-põhise marsruutimiseks Azure'i VPN-lüüsi toetab AES256, AES128 ja 3DES krüptimise algoritmid. AES256 ja 3DES marsruutimiseks vastavalt lüüside toega.

### <a name="azure-vnet-gateway"></a>Azure'i VNet lüüsi:

[Azure'i VPN-lüüsi diagnostikalogid] uurida[ gateway-diagnostic-logs] võimalike probleemide kohta.

**On Azure VPN-lüüsi ja kohapealse VPN seadme konfigureeritud sama ühiskasutusega autentimist võti?**

Saate vaadata ühiskasutusega võti talletatud Azure'i VPN-lüüsi, kasutades Azure CLI järgmine käsk:

```
azure network vpn-connection shared-key show <<resource-group>> <<vpn-connection-name>>
```

Oma kohapealse VPN seadme vastav käsu abil saate kuvada selle seadme jaoks konfigureeritud ühiskasutuses võti.

Veenduge, et _GatewaySubnet_ alamvõrku, hoides Azure'i VPN-lüüsi pole seostatud mõne NSG.

Azure'i CLI järgmise käsu abil saate kuvada alamvõrgu üksikasjad:

```
azure network vnet subnet show -g <<resource-group>> -e <<vnet-name>> -n GatewaySubnet
```

Tagada andmete väli puudub nimega _võrgu turberühma ID-d_. Järgmises näites on kujutatud näiteks tulemuste _GatewaySubnet_ , mis sisaldab on määratud NSG (_VPN-lüüsi-rühma_). See võib põhjustada vältida lüüsi töötab õigesti, kui seal on see NSG jaoks määratletud reeglid:

```
C:\>azure network vnet subnet show -g profx-prod-rg -e profx-vnet -n GatewaySubnet
    info:    Executing command network vnet subnet show
    + Looking up virtual network "profx-vnet"
    + Looking up the subnet "GatewaySubnet"
    data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/virtualNetworks/profx-vnet/subnets/GatewaySubnet
    data:    Name                            : GatewaySubnet
    data:    Provisioning state              : Succeeded
    data:    Address prefix                  : 10.20.3.0/27
    data:    Network Security Group id       : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/networkSecurityGroups/VPN-Gateway-Group
    info:    network vnet subnet show command OK
```

**Virtuaalmasinates rakenduses Azure'i VNet konfigureeritakse lubada liiklust väljaspool olevat VNet pärit? Märkige ruut mis tahes seostatud alamvõrku, mis sisaldab järgmisi virtuaalmasinates NSG reeglid.**

Azure'i CLI järgmise käsu abil saate vaadata kõiki NSG reegleid:

```
azure network nsg show -g <<resource-group>> -n <<nsg-name>>
```

**Azure'i VPN-lüüsi on välja lülitatud?**

Azure PowerShelli käsk abil saate praeguse oleku Azure'i VPN-ühendus. Funktsiooni `<<connection-name>>` parameeter on Azure VPN-ühendus, mis ühendab virtuaalse võrgu lüüsi ja kohalik lüüsi nimi.

```
Get-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> - ResourceGroupName <<resource-group>>
```

Järgmised pikad esiletõstmiseks loodud kui lüüsi on ühendatud (nt esimese) ja väljundi (teine näide) tehke järgmist.

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : Connected
EgressBytesTransferred     : 55254803
IngressBytesTransferred    : 32227221
ProvisioningState          : Succeeded
...
```

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection2 -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : NotConnected
EgressBytesTransferred     : 0
IngressBytesTransferred    : 0
ProvisioningState          : Succeeded
...
```

### <a name="host-vm-configuration-network-bandwidth-utilization-and-application-performance"></a>Hostikonfiguratsioon VM, võrgu läbilaskevõime kasutuse ja rakenduse jõudlus.

Kontrollige tulemüüri Külastajate operatsioonisüsteemi Azure VMs alamvõrgu töötab õigesti konfigureeritud: kohapealse IP-aadresside vahemikud lubatud liikluse lubamiseks.

**Kas liiklus ribalaiust piirmäära lähedale Azure'i VPN-lüüsi?**

Instrumentaarium sõltub teie kohapealse töötab VPN-seade saadaval vahendid. Näiteks kui kasutate RRAS Windows Server 2012, saate jõudluse kuvari hulga andmete ei saanud ja edastatav teave üle VPN-ühendus Valige _Saadud baiti sekundis_ ja _Edastatav teave baiti sekundis_ hinnale _RAS kokku_ objekti abil:

![[3]][3]

Mida tuleks võrrelda tulemusi VPN-lüüsi (100 Mbps Basic ja Standard SKU-de jaoks ja suure jõudlusega SKU 200 Mbps) saadaval läbilaskevõime:

![[4]][4]

**Mis tahes virtuaalmasinates rakenduses Azure'i VNet töötab aeglaselt? Kas nad ülekoormatud, on piisavalt neist laadi käsitlema, on kõik koormus soolise õigesti konfigureeritud?**

Selleks on vaja [talletamiseks ja analüüsimiseks diagnostikateave][azure-vm-diagnostics]. Saate uurida Azure'i portaalis tulemusi, kuid paljud muude tootjate tööriistad on ka saadaval, mida saate sisestada üksikasjaliku ülevaate tulemustega seotud andmete.

**On tegemist tõhusa rakendus kasutada cloud ressursside?**

Seadme kood rakendus töötab iga VM määrata, kas rakendused on parim ära ressursid. Saate kasutada näiteks [Rakenduse ülevaated] [ application-insights] abil teha järgmisi toiminguid.

## <a name="solution-deployment"></a>Lahenduse juurutamine

Kui teil on juba konfigureeritud VPN seadme olemasoleva kohapealse infrastruktuuri, saate järgmiste juhiste järgi juurutada arhitektuur viide:

1. Paremklõpsake nuppu ja valige kas "Ava link uus vahekaart" või "Ava link uues aknas":  
[![Azure'i juurutamine](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn%2Fazuredeploy.json)

2. Oodake, kuni link peaks avama Azure'i portaalis, siis tehke järgmist: 
    - **Ressursirühm** nimi on juba määratletud parameeter faili, seega valige **Loo uus** ja sisestage `ra-hybrid-vpn-rg` tekstiväljale.
    - Valige piirkonnale **asukoht** rippmenüüst.
    - **Malli Root Uri** või **Parameetri Root Uri** tekstivälju ei saa redigeerida.
    - Tingimused läbi ja seejärel märkige ruut **nõustun eespool tingimused** .
    - Klõpsake nuppu **osta** .

3. Oodake, kuni juurutuse lõpuleviimiseks.

## <a name="next-steps"></a>Järgmised sammud

- [Täiendav domeenikontrollerid kohapealse Active Directory domeeni VMs kasutamine on Azure VNet installimise][installing-ad].

- [Windows Serveri Active Directory Azure VMs juurutamise juhised][deploying-ad].

- [DNS-i server on VNet loomine][creating-dns].

- [Konfigureerimise ja haldamise DNS-i lisamine VNet][configuring-dns].

- [Kohapealse Stormshield võrgu turvalisus seadmete abil üle VPN][stormshield].

- [Rakendamise hübriid võrgu arhitektuur koos Azure ExpressRoute][expressroute].

<!-- links -->

[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[arm-templates]: ../resource-group-authoring-templates.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-portal]: ../azure-portal/resource-group-portal.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[azure-virtual-network]: ../virtual-network/virtual-networks-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-network-security-group]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: ../vpn-gateway/vpn-gateway-multi-site.md
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/v1_0/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[azure-vpn-gateway-diagnostics]: http://blogs.technet.com/b/keithmayer/archive/2014/12/18/diagnose-azure-virtual-network-vpn-connectivity-issues-with-powershell.aspx
[ping]: https://technet.microsoft.com/library/ff961503.aspx
[tracert]: https://technet.microsoft.com/library/ff961507.aspx
[psping]: http://technet.microsoft.com/sysinternals/jj729731.aspx
[nmap]: http://nmap.org
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: http://blogs.technet.com/b/keithmayer/archive/2015/12/07/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs.aspx
[troubleshooting-vpn-errors]: https://blogs.technet.microsoft.com/rrasblog/2009/08/12/troubleshooting-common-vpn-related-errors/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[create-on-prem-network]: https://technet.microsoft.com/library/dn786406.aspx#routing
[create-azure-vnet]: ../virtual-network/virtual-networks-create-vnet-classic-cli.md
[azure-vm-diagnostics]: https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/
[application-insights]: ../application-insights/app-insights-overview-usage.md
[forced-tunneling]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-forced-tunneling/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[vpn-appliances]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[installing-ad]: ../active-directory/active-directory-install-replica-active-directory-domain-controller.md
[deploying-ad]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[creating-dns]: https://blogs.msdn.microsoft.com/mcsuksoldev/2014/03/04/creating-a-dns-server-in-azure-iaas/
[configuring-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[stormshield]: https://azure.microsoft.com/marketplace/partners/stormshield/stormshield-network-security-for-cloud/
[vpn-appliance-ipsec]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec-parameters
[expressroute]: ./guidance-hybrid-network-expressroute.md
[naming conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/deploy-reference-architecture.sh
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetwork.parameters.json
[virtualNetworkGateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetworkGateway.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/blueprints/hybrid-network-vpn.png "Hübriid ülesehituse network kestvad asutusesisese ja cloud infrastruktuur"
[1]: ./media/guidance-hybrid-network-vpn/partitioned-vpn.png "Eraldamine VNet, parandamiseks skaleeritavus."
[2]: ./media/guidance-hybrid-network-vpn/audit-logs.png "Auditilogid Azure'i portaalis"
[3]: ./media/guidance-hybrid-network-vpn/RRAS-perf-counters.png "Jõudluseloendurite jälgida VPN-i võrguliiklust."
[4]: ./media/guidance-hybrid-network-vpn/RRAS-perf-graph.png "Näide VPN võrgu jõudluse graafik"