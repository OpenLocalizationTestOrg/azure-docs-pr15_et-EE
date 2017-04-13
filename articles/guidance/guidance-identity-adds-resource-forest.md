<properties
   pageTitle="Azure'i arhitektuur viide - IaaS: Loomine Azure Active Directory ressursside kogumis | Microsoft Azure'i"
   description="Kuidas luua Azure Active Directory usaldatud domeeni."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="creating-a-active-directory-directory-services-adds-resource-forest-in-azure"></a>Azure Active Directory Directory Services (LISAB) ressursi mets loomine

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Selles artiklis kirjeldatakse, kuidas Active Directory domeeni Azure, mis on eraldi, kuid usaldusväärsete, luua oma kohapealse mets domeene.

> [AZURE.NOTE] Azure'i on kaks erinevat juurutamise mudelit: [Ressursihaldur] [ resource-manager-overview] ja klassikaline. See viide arhitektuur kasutab ressursihaldur, mis soovitab Microsoft kasutada uut juurutuste.

Ettevõte, mis töötab kohapealse Active Directory (AD) võib olla mets, mis sisaldab palju erinevaid domeene. Näiteks võite luua üksikute domeenide erinevate osakondade või suborganizations või uue domeenide lisatud tuua või muude organisatsioonide ühinemise tulemusena. Domeenide abil saate hoida eraldi turvalisuse põhjustel võib-olla otstarbekas alade vahelise eraldustaseme esitada, kuid vahel domeenide usalda seosed luues saate ühiskasutusse anda.

Ettevõte, mis kasutab eraldi domeenid saate ära Azure'i paigutades ühte või mitut domeenid pilveteenusesse. Teise võimalusena ettevõtte võib soovi säilitada kõik cloud ressursid loogiliselt erinevad need hoitakse kohapeal ja talletada teavet cloud ressursid oma domeeni ka pilveteenuses hoitavate kataloogi.

Saate rakendada Active Directory Azure mitmel viisil, nagu on kirjeldatud teemades [Ulatub Active Directory Azure] [ extending-ad-to-azure] ja [Rakendamise Azure Active Directory][implementing-aad]. See dokument keskendub ühe konkreetse stsenaariumi: domeeni pilveteenuses hoitavate kohapealse domeene erineb, kuid mis võib olla usaldusseos kohapealse domeenide loomine. 

Tüüpilised kasutamine karpide see arhitektuur on järgmised.

- Turvalisus eraldamine objekte ja identiteedid pilveteenuses hoitavate haldamine.

- Migreerimine versioonist kohapealse üksikute domeenide pilve.

## <a name="architecture-diagram"></a>Arhitektuur skeem

Järgmine diagramm tõstab esile see arhitektuur (*suurendamiseks klõpsake*) olulised komponendid. Värvi välja elementide kohta lisateabe saamiseks lugege [rakendamise turvaline hübriid võrgu arhitektuur Azure] [ implementing-a-secure-hybrid-network-architecture] ja [rakendamiseks turvaline hübriid võrgu arhitektuur Interneti-juurdepääsu Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **Kohapealse võrgu.** Kohapealse võrgu sisaldab omaette AD mets ja domeenid.

- **AD serverites.** Need on domeenikontrollerid rakendamise directory services (AD DS) töötab VMs pilveteenuses. Järgmistes serverites majutada mets, mis sisaldab ühe või mitu domeeni, asutusesisesest need asuvad eraldi.

- **Ühesuunalise usaldusseos.** Näites joonisel on kuvatud ühesuunalise usalda domeeni, et kohapealse domeeni. Selle seosega võimaldab kohapealse kasutajatele juurdepääs ressursid pilves, kuid mitte teistpidi Domeen. See on levinud juhtumi. Siiski saate luua kahesuunaline usalduskeskuse kui pilveteenuste kasutajad on vaja ka kohapealse ressurssidele.

- **Active Directory alamvõrgu.** AD DS-i serverid on hostitud eraldi alamvõrgu. NSG reeglid AD DS-i serverid kaitse ja saate sisestada tulemüüri vastu liikluse ootamatud allikatest.

- **Web taseme alamvõrku**, **Business taseme alamvõrgu**ja **andmete taseme alamvõrgu**. Nende alamvõrku majutada serverid ja kasutada rakendusi pilves komponendid. Lisateabe saamiseks lugege teemat [Töötab VMs jaoks N-astme arhitektuur Azure][running-VMs-for-an-N-tier-architecture-on-Azure]. Ressursside ja selle alamvõrgu VMs sisalduvad pilvepõhise domeeni.

- **Azure'i lüüsi**. Azure'i lüüsi pakub kohapealse võrgu ja Azure VNet vahelist ühendust. See võib olla [VPN-ühendus] [ azure-vpn-gateway] või [Azure'i ExpressRoute][azure-expressroute]. Lisateabe saamiseks lugege teemat [rakendamise turvaline hübriid võrgu arhitektuur Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Soovitused

Sellest jaotisest leiate loendi olulised osad vajalikud põhilised arhitektuur soovitusi. Soovitused hõlmavad:

- LIIDAB sätted ja

- Usalda seose konfigureerimine.

Täiendavad või erinevad nõuded: siinkirjeldatud võib olla. Saate üksused selles jaotises lähtepunktina arutate, kuidas kohandada oma süsteemi arhitektuur.

### <a name="adds-recommendations"></a>LIIDAB soovitused

Teatud soovitused rakendamise Active Directory pilves, vaadake dokumendi [Ulatub Active Directory Azure][extending-ad-to-azure]. Artikli [juhised juurutamine Windows Server Active Directory Azure'i Virtuaalmasinates] [ ad-azure-guidelines] sisaldab täiendavat üksikasjalikku teavet.

### <a name="trust-recommendations"></a>Usalda soovitused

Kohapealse domeeni sisalduvad erinevate mets domeenide pilveteenuses. Pilveteenuste kasutajad kohapealse autentimise lubamiseks peab domeenide pilveteenuses usaldada sisselogimise domeeni kohapealse mets. Samamoodi kui pilveteenuses pakub sisselogimise domeeni väliste kasutajate jaoks, võib olla vajalik asutusesiseses kogumis, et usaldada domeeni pilve.

Saate luua usalduste tasemel mets [mets usalduste]loomisega[creating-forest-trusts], või domeeni tasemel [välise usalduste]loomisega[creating-external-trusts]. Mets taseme usalda loob kaks kogumit kõik domeene vahel seose. Mõne välise domeeni taseme usalda ainult loob kaks määratud domeeni vahel seose. Ainult loote välise domeeni taseme usalduste domeene erinevate metsade vahel.

Usalduste võib olla Ühesuunalised (ühesuunalise) või kahesuunaline (kahesuunaline):

- Ühesuunalise usalda võimaldab kasutajatel ühe domeeni või mets (tuntud *sissetulevate* domeeni või mets) juurde pääseda olevad teise ressursside ( *Väljamineva* domeeni või mets). 

- Kahesuunalise usalda võimaldab kasutajatel juurdepääs teistes ressursside domeeni või mets.

Järgmises tabelis on toodud mõned lihtsad stsenaariumid usalda konfiguratsioone:

| Stsenaarium | Kohapealse usalda | Pilveteenuse usalda |
|----------|-------------------|-------------|
| Kohapealse kasutajad vajavad juurdepääsu ressursid pilves, kuid mitte vastupidi | Ühesuunalise, sissetuleva | Ühesuunalise, väljamineva |
| Pilveteenuste kasutajad kasutamiseks on vaja juurdepääsu kohapealse varundatakse, kuid mitte vastupidi | Ühesuunalise, väljamineva | Ühesuunalise, sissetuleva |
| Pilveteenuses ja kohapealsete kasutajate jaoks on vaja juurdepääsu kohapealse ja pilveteenuse ressursside | Kahesuunaline, sissetulevad ja väljaminevad | Kahesuunaline, sissetulevad ja väljaminevad |

## <a name="security-considerations"></a>Turvalisuse alused

Mets taseme usalduste on sihiliste. Kui loote mets taseme usalda vahel on kohapealse mets ja mets pilves, laiendada selle usalda muud uued domeenid loodud kas mets. Kui kasutate domeenid, et turvalisuse huvides, kaaluge usalduste ainult domeeni tasemel. Domeeni taseme usalduste on sihiliste.

AD kohased turvakaalutlused, jaotisest *turvakaalutlused* [ulatub Active]Directory Azure[extending-ad-to-azure].

## <a name="availability-considerations"></a>Kättesaadavus kaalutlused

Rakendada vähemalt kaks domeenikontrollerid iga domeeni puhul. See võimaldab automaatse dispersioonanalüüs meiliserverite vahel. Saadavus VMs abistas AD serverid töötlemise iga domeeni jaoks luua. Tagada vähemalt kaks serverid määramine. 

Pidage silmas ka määrata ühe või mitme serveri iga domeeni nimega [Valige toimingud meistrid] [ standby-operations-masters] juhuks, kui ühenduvust serveriga abistas FSMO rolli nurjus.

## <a name="scalability-considerations"></a>Kaalutlused skaleeritavus.

AD on automaatselt scalable domeenikontrollerid, mis on osa sama domeeni. Taotlusi levitatakse üle kõik andmetöötlusalased domeeni. Saate lisada mõne muu domeenikontrolleri ja selle sünkroonib automaatselt domeeniga. Konfigureerige eraldi laadi koormusetasakaalustusteenuse domeeni kontrollerid liikluse suunamiseks. Tagatakse, et kõik domeenikontrollerid on domeeni andmebaasi käsitlema piisavate mälu ja salvestusruumi. Muuta kõik selle domeenikontrolleri VMs sama suur.

## <a name="management-and-monitoring-considerations"></a>Juhtimis- ja kasutuse jälgimine

Haldus ja jälgimine kaalutluste kohta leiate teavet teemast võrdväärse jaotiste [Laiendamine Active Directory Azure][extending-ad-to-azure]. 

Lisateabe saamiseks lugege teemat [Jälgimise Active Directory][monitoring_ad]. Saate installida [Microsoft süsteemide keskus] näiteks[ microsoft_systems_center] jälgimisega seotud server management alamvõrgu abil teha järgmisi toiminguid. 

## <a name="solution-components"></a>Lahenduse komponendid

Proovi lahenduse skripti [Deploy-ReferenceArchitecture.ps1][solution-script], on saadaval, saate rakendada selles artiklis kirjeldatud soovitused järgneva arhitektuur. See skript kasutab Azure ressursihaldur mallid. Mallid on saadaval kogumina olulise koosteüksuste, mis teeb teatud toimingu, näiteks on VNet luua või konfigureerida on NSG. Skripti eesmärk on korraldab malli juurutamine.

Mallid on parameetriga säilitatakse eraldi JSON parameetritega. Saate muuta need failid oma nõuetele vastavuse juurutamist konfigureerimiseks parameetrid. Pole vaja muuta ise malle. Ei tohi muuta skeemid objektide parameetri failid.

Mallide redigeerimisel luua objekte, mis nimereeglid, [On soovitatav nimetamise põhimõtted Azure ressursse]kirjeldatud järgige[naming-conventions].

Lahendus näidis loob ja konfigureerib pilves, mis rakendab domeeni nimega *treyresearch.com*keskkonnas. See keskkond, mis hõlmab lisatakse alamvõrgu ja serverid DMZ, web taseme, business taseme ja Accessi taseme andmekomponendid VPN-lüüsi ja juhtimise taseme. Valimi lahendus ka valikuline loomiseks oma domeeni *contoso.com*jäljendatud kohapealse keskkonna konfiguratsioon. Lahendus sisaldab skriptide, mille usaldusseos need domeenides, mis võimaldab kohapealse kasutajatele juurdepääs *treyresearch.com* domeeni pilveteenuses.

Järgmistes jaotistes kirjeldada asutusesiseses elementide ja Pilveteenusest konfiguratsioone.

### <a name="on-premises-components"></a>Kohapealse komponendid

>[AZURE.NOTE] Järgmised komponendid pole dokumendis kirjeldatud arhitektuur keskmes ja pakutakse lihtsalt testimiseks cloud keskkonnas turvaliselt, reaal tootmiskeskkonna kasutamise asemel võimalust. Seetõttu Kokkuvõte selles jaotises ainult võtme parameetri failid. Saate muuta sätteid, näiteks IP-aadresside või VMs suurused, kuid on soovitatav paljude parameetrid, muutmata jätta.

See keskkond, mis hõlmab ka AD mets domeeni *contoso.com* . Domeeni sisaldab LIIDAB kaks serverite IP-aadressid 192.168.0.4 ja 192.168.0.5. Nende kahe serverid käivitada ka DNS-i teenus. Kohaliku administraatorikonto nii VMs nimetatakse `testuser` parooliga `AweS0me@PW`. Lisaks häälestab konfiguratsiooni VPN-lüüsi ühenduse VNet pilveteenuses. Saate muuta konfiguratsiooni, redigeerides järgmised JSON failid, mis asub [**parameetrite/valikul Asutusesisene**] [ on-premises-folder] kaust:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. See fail määratleb võrgu aadressiruumi kohapealse keskkonna jaoks.

- ** [virtualMachines-adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. See fail sisaldab kohapealse VMs lisatakse majutusteenuste konfigureerimine. Vaikimisi luuakse kaks *Standard-DS3-v2* VMs.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** ja ** [connection.parameters.json][on-premises-connection-parameters]**. Nende failide hoidke Azure'i VPN-lüüsi VPN-ühenduse sätete pilves, sh ühiskasutusega võti liikluse liiklevad lüüsi kaitsmiseks kasutada.

Ülejäänud failide kausta sisaldavad kohapealse domeeni (*contoso.com*), kasutades selle taristu loomiseks kasutatud konfiguratsiooniteavet. Nende abil lisatakse häälestamine DNS-i, installige ja kohapealse mets loomine.

Lahendus ka kasutab järgmist skripti, nimega [sissetuleva meili-trust.ps1][incoming-trust], mis töötab kohapealse domeeni arvutisse:

```Powershell
# Run the following powershell script in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)

$TrustedDomainName = "contoso.com"
#$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

$TrustingDomainName = "treyresearch.com"
$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustingDomainDnsIpAddresses
$ForwardDomainName = $TrustingDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

See skript lisab AD DS-i serverid IP-aadresside pilves (leiate järgmisest jaotisest) Kohalik DNS-i teenus, ja seejärel kasutab [netdom] [ netdom] käsku Loo on sissetulevate ühesuunalise usalda domeeni pilves (*treyresearch.com*).

### <a name="cloud-components"></a>Pilveteenuse komponendid

Nende komponentide vormi tuum see arhitektuur. Need taristu *treyresearch.com* domeeni häälestamise ja luua seoseid usalda kohapealse domeeni *contoso.com* . [**Parameetrite/Azure'i**] [ azure-folder] kaust sisaldab järgmisi parameetri faile konfigureerida järgmised komponendid:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. See fail määratleb struktuuri on VNet VMs ja muud komponendid pilveteenuses. See sisaldab sätteid, näiteks nimi, aadressiruumi, alamvõrku ja aadressid, mis tahes DNS-serverid nõutav. DNS-i aadressid näidatud selles näites viide kohapealse DNS-serverid ja vaikimisi Azure'i DNS-i server IP-aadressid. Need aadressid viitamiseks oma DNS-i häälestamise, kui te ei kasuta valimi kohapealse keskkonna muutmiseks tehke järgmist.
    
    ```json
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg",
        "addressPrefixes": [
          "10.0.0.0/16"
        ],
        "subnets": [
          {
            "name": "dmz-private-in",
            "addressPrefix": "10.0.0.0/27"
          },
          {
            "name": "dmz-private-out",
            "addressPrefix": "10.0.0.32/27"
          },
          {
            "name": "dmz-public-in",
            "addressPrefix": "10.0.0.64/27"
          },
          {
            "name": "dmz-public-out",
            "addressPrefix": "10.0.0.96/27"
          },
          {
            "name": "mgmt",
            "addressPrefix": "10.0.0.128/25"
          },
          {
            "name": "GatewaySubnet",
            "addressPrefix": "10.0.255.224/27"
          },
          {
            "name": "web",
            "addressPrefix": "10.0.1.0/24"
          },
          {
            "name": "biz",
            "addressPrefix": "10.0.2.0/24"
          },
          {
            "name": "data",
            "addressPrefix": "10.0.3.0/24"
          },
          {
            "name": "adds",
            "addressPrefix": "10.0.4.0/27"
          }
        ],
        "dnsServers": [
          "10.0.4.4",
          "10.0.4.5",
          "168.63.129.16"
        ]
      }
    }
    ```

- ** [virtualMachines-adds.parameters.json ] [ virtualmachines-adds-parameters] **. Selle faili konfigureerib VMs töötab lisatakse pilveteenuses. Konfiguratsiooni koosneb kahest VMs. Administraatori kasutajanime ja parooli muutmine soovitud `virtualMachineSettings` jaotist ja saate soovi korral suurust saab muuta VM domeeni nõuetele vastavus:

    Lisateavet leiate teemast [Ulatub Active Directory Azure][extending-ad-to-azure].
    
    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-ad",
        "computerNamePrefix": "aad",
        "size": "Standard_DS3_v2",
        "osType": "Windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "adds",
            "privateIPAllocationMethod": "Static",
            "startingIPAddress": "10.0.4.4",
            "enableIPForwarding": false,
            "dnsServers": [
            ],
            "isPrimary": "true"
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 127,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [
        ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-as"
        }
      }
    },
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg"
      }
    },
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ```

- ** [lisamine – lisab-domeeni-controller.parameters.json][add-adds-domain-controller-parameters]**. See fail sisaldab loomise kestvad lisatakse serverid *treyresearch.com* domeeni sätted. Kasutab kohandatud domeeni luua ja lisada sellele lisatakse serverid laiendid. Kui loote täiendavad lisatakse serverid (sel juhul tuleks lisada neid on `vms` massiiv), muuta nende nimed vaikimisi või soovite seda luua domeeni erineva nimega ei pea te selle faili muutmiseks.

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. See fail sisaldab saab luua ühenduse loomiseks kohapealse võrgu pilveteenuses Azure'i VPN-lüüsi sätted. Muutke soovitud `sharedKey` väärtus on `connectionsSettings` jaotis kohapealse VPN seadme vastavaks. Lisateabe saamiseks lugege teemat [rakendamise hübriid võrgu arhitektuur Azure ja kohapealse VPN][hybrid-azure-on-prem-vpn].

- ** [dmz-private.parameters.json] [ dmz-private-parameters] ** ja ** [dmz-public.parameters.json ] [ dmz-public-parameters] **. Nende failide konfigureerimine sissetulevate (avaliku) ja väljaminevate (privaatne) külgedel DMZ, kaitsmine pilveteenuses serverid moodustavate VMs. Järgmisi elemente ja nende konfigureerimise kohta leiate lisateavet teemast [rakendada mõne DMZ Azure ja Interneti vahel][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer-web.parameters.json][loadBalancer-web-parameters]**, ** [loadBalancer-biz.parameters.json][loadBalancer-biz-parameters]**, ja ** [loadBalancer-data.parameters.json][loadBalancer-data-parameters]**. Järgmiste parameetrite failid sisaldavad veebi-, äri- ja juurdepääsu astme VM kirjeldused ja konfigureerige koormus soolise iga taseme jaoks. Need on VMs, mis majutada veebirakenduste ja andmebaaside ja teha business töökoormus asutuse jaoks. Web taseme VM lisatakse domeeni pilves, kasutades määratud sätted soovitud ** [web vm-domeeni join.parameters.json] [ web-vm-domain-join-parameters] ** faili.

    Iga fail sisaldab kahte tüüpi konfiguratsiooni parameetrid. Funktsiooni `virtualMachineSettings` jaotis määratleb ADFS-i teenuse pilveteenuses majutavad VMs. Vaikimisi skript loob kaks neist VMs sama kättesaadavus määramine. Järgmised fragmendid Kuva *loadBalancer-web.parameters.json* faili oluline osa:

    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-web",
        "computerNamePrefix": "web",
        "size": "Standard_DS1_v2",
        "osType": "windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "web",
            "privateIPAllocationMethod": "Dynamic",
            "isPrimary": "true",
            "enableIPForwarding": false,
            "dnsServers": [ ]
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 128,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [ ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-web-vm-as"
        }
      }
    },
    ...
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ````
    Saate muuta suurused ja iga taseme vms arv vastavalt oma vajadustele.

    Funktsiooni `loadBalancerSettings` jaotises antakse kirjeldus laadi koormusetasakaalustusteenuse need VMs. Laadi koormusetasakaalustusteenuse edastab liikluse, mis kuvatakse pordi 80 (HTTP) ja pordi 443 (HTTPS) ühe või teise VMs. 

    >[AZURE.NOTE] Pordi 80 reegel kasutab TCP-ühenduse, mitte HTTP. See on Kuna IIS-i installimine web taseme on konfigureeritud toetama ainult Windowsi autentimist. Anonüümne autentimine on keelatud. *Ping* katsel pordi 80 HTTP-ühenduse kaudu nurjub 401 (volitamata) viga, arvestades TCP-ühenduse aadressil tuvastab, kas port on aktiivne:

    ```json
    "loadBalancerSettings": {
      "value": {
        "name": "ra-adtrust-web-lb",
        "frontendIPConfigurations": [
          {
            "name": "ra-adtrust-web-lb-fe",
            "loadBalancerType": "internal",
            "internalLoadBalancerSettings": {
              "privateIPAddress": "10.0.1.254",
              "subnetName": "web"
            }
          }
        ],
        "backendPools": [
          {
            "name": "ra-adtrust-web-lb-bep",
            "nicIndex": 0
          }
        ],
        "loadBalancingRules": [
          {
            "name": "http-rule",
            "frontendPort": 80,
            "backendPort": 80,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "http-probe",
            "enableFloatingIP": false
          },
          {
            "name": "https-rule",
            "frontendPort": 443,
            "backendPort": 443,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "https-probe",
            "enableFloatingIP": false
          }
        ],
        "probes": [
          {
            "name": "http-probe",
            "port": 80,
            "protocol": "Tcp",
            "requestPath": null
          },
          {
            "name": "https-probe",
            "port": 443,
            "protocol": "Tcp",
            "requestPath": null
          }
        ],
        "inboundNatRules": [ ]
      }
    }
    ```

- ** [virtualMachines-mgmt.parameters.json][virtualMachines-mgmt-parameters]**. See fail sisaldab hüpata ruut/halduse VMs konfigureerimine. Ainult on võimalik saada sisse- ja toetatavatele välja hüpata VM veebi-, äri- ja andmete astme. Skripti loob ühe *Standard_DS1_v2* VM vaikimisi, kuid saate muuta selle faili loomiseks suuremaks või täiendava VMs kui halduse töökoormus on tõenäoliselt oluline.

Konfiguratsiooni kasutab ka [väljaminevat trust.ps1] [ outgoing-trust] skripti loomiseks ühesuunalise väljamineva usalda domeeni *contoso.com* allpool näidatud:

```powershell
# prerequiste: 
# You need to first run incoming-trust.ps1 in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)
# Then,
# Run the following powershell script in ra-adtrust-ad-vm1 (ip 10.0.4.4)

$TrustedDomainName = "contoso.com"
$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

#$TrustingDomainName = "treyresearch.com"
#$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustedDomainDnsIpAddresses
$ForwardDomainName = $TrustedDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

#netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

See skript on sarnane ülalkirjeldatud *sissetuleva meili-trust.ps1* skripti. See lisab asutusesisese IP-aadresside AD DS-i serverid kohalik DNS-i teenus, ja seejärel kasutab [netdom] [ netdom] käsk väljamineva usalda seose loomine.

## <a name="solution-deployment"></a>Lahenduse juurutamine

Lahendus eeldab järgmine kohustuslik tarkvara:

- Teil on, kus saate luua ressursi rühmade Azure olemasolevale tellimusele.

- On alla laadida ja installida Azure PowerShell viimase koostamine. Leiate [siit] [ azure-powershell-download] juhised.

Skripti, mis kasutab lahenduse käivitamiseks tehke järgmist.

1. Kohalikus arvutis mugav kausta teisaldada ja järgmised alamkaustade loomine.

    - Skriptide

    - Skriptide/parameetrid

    - Skriptide/parameetrid/valikul Asutusesisene

    - Azure'i/skriptide/parameetrid

2. Laadige alla [Deploy-ReferenceArchitecture.ps1] [ solution-script] skriptide kausta fail

3. [Parameetrite/valikul Asutusesisene] sisu alla laadida[ on-premises-folder] skriptide/parameetrid/valikul Asutusesisene kausta:

4. [Parameetrite/Azure'i] sisu alla laadida[ azure-folder] skriptide/parameetrid/Azure kausta.

5. Skriptide kausta Deploy-ReferenceArchitecture.ps1 faili redigeerida ja muuta järgmised read määramiseks ressursi rühmad, mida tuleks luua või kasutada hoidke ressursse, mis on loodud skripti:

    ```powershell
    # Azure Onpremise Deployments
    $onpremiseNetworkResourceGroupName = "ra-adtrust-onpremise-rg"

    # Azure ADDS Deployments
    $azureNetworkResourceGroupName = "ra-adtrust-network-rg"
    $workloadResourceGroupName = "ra-adtrust-workload-rg"
    $securityResourceGroupName = "ra-adtrust-security-rg"
    $addsResourceGroupName = "ra-adtrust-adds-rg"
    ```

6. Redigeerige parameetri skriptide/parameetrid/valikul Asutusesisene ja skriptide/parameetrid/Azure kaustade failid. Värskendage selle ressursi rühma viited need failid vastavad määratud muutujate Deploy-ReferenceArchitecture.ps1 faili ressursside rühmade nimed. Järgmises tabelis on näidatud, parameetri faile, mis viitavad mis ressursirühma. *Ra adfs-töökoormus rg*, *ra adfs-turvalisus rg*, *ra-ADFS-i lisab-rg*, *ra adfs-i rg*ja *ra adfs-puhverserveri rg* ressursi rühmade kasutatakse ainult PowerShelli skripti ja parameetri failid ei esine.

  	|Ressursirühm|Parameetri failid|
  	|--------------|--------------|
  	|RA adtrust-valikul Asutusesisene rg|parameters\onpremise\connection.parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.parameters.JSON<br />parameters\onpremise\virtualNetwork.parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON
  	|RA adtrust-võrgu rg|parameters\onpremise\connection.parameters.JSON<br />parameters\azure\dmz-Private.parameters.JSON<br />parameters\azure\dmz-Public.parameters.JSON<br />parameters\azure\loadBalancer-biz.parameters.JSON<br />parameters\azure\loadBalancer-Data.parameters.JSON<br />parameters\azure\loadBalancer-Web.parameters.JSON<br />parameters\azure\virtualMachines-ADDS.parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.parameters.JSON<br />parameters\azure\virtualNetwork-ADDS-DNS.parameters.JSON<br />parameters\azure\virtualNetwork.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON (*kaks sündmuste*)

    Lisaks asutusesisese konfiguratsiooni seadmine ja cloud komponendid, nagu on kirjeldatud [Lahenduse komponendid] [ solution-components] jaotis.

7. Azure'i PowerShelli akna avamine skriptide kausta teisaldada ja käivitage järgmine käsk:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Asendage `<subscription id>` oma Azure tellimuse ID-ga.

    Jaoks `<location>`, määrake Azure piirkond, näiteks `eastus` või `westus`.

    Funktsiooni `<mode>` parameeter võib olla üks järgmistest väärtustest:

    - `Onpremise`, jäljendatud kohapealse keskkonna loomiseks.

    - `Infrastructure`, luua VNet taristu ja pilveteenuses väljale liikumine.

    - `CreateVpn`, luua Azure virtuaalse võrgu lüüsi ja ühendage see kohapealse võrgu.

    - `AzureADDS`, ehitada VMs abistas lisatakse serverid, juurutada Active Directory need VMs ja domeeni loomine pilveteenuses.

    - `WebTier`, mis loob veebis taseme VMs ja laadimine koormusetasakaalustusteenuse.

    - `Prepare`, mis teeb eelmise tööülesanded. **See on soovitatav suvand, kui on loomine on täiesti uue juurutamise ja teil pole kohapealse infrastruktuuri.**

    - `Workload`luua business ja andmete taseme VMs ja koormuse tasakaalustajad toimingut. Need VMs ei kaasata ka selle `Prepare` suvand.

    >[AZURE.NOTE] Kui kasutate funktsiooni `Prepare` suvand skripti aega võtab mitu tundi.

8.  Kui kasutate valimi asutusesisese konfiguratsiooni:

    1. Ühenduse loomine välja hüpata (*ra adtrust-mgmt vm1* *ra adtrust – turvalisus rg* ressursi jaotises). *Testuser* parooliga sisse logida *AweS0me@PW*.

    2.  Siirdeloendi ruut esimene VM RDP seansi avamine domeeni *contoso.com* (kohapealse domeeni). See VM on IP-aadress 192.168.0.4. Kasutajanimi on parooliga *contoso\testuser* *AweS0me@PW*.

    3. Laadige alla [sissetuleva meili-trust.ps1] [ incoming-trust] skript ja käivitage see *treyresearch.com* domeeni sissetulevate usalda loomiseks.

9. Kui kasutate oma kohapealse infrastruktuur:

    1. Laadige alla [sissetuleva meili-trust.ps1] [ incoming-trust] skripti.

    2. Skripti redigeerida ja asendada väärtus on `$TrustedDomainName` muutuv oma domeeni nimega.

    3. Käivitage skript.

10. Siirdeloendi väljale ühenduse esimese VM *treyresearch.com* domain (Domeen pilveteenuses). See VM on IP-aadress 10.0.4.4. Kasutajanimi on parooliga *treyresearch\testuser* *AweS0me@PW*.

11. Laadige alla [väljaminevat trust.ps1] [ outgoing-trust] skript ja käivitage see *treyresearch.com* domeeni sissetulevate usalda loomiseks. Kui kasutate oma kohapealse masinad, redigeerige esmalt skripti. Määrata selle `$TrustedDomainName` muutuv kohapealse domeeni nimi ja määrake selle domeeni AD DS-i serverid IP-aadressid on `$TrustedDomainDnsIpAddresses` muutuja.

12. Kohapealse arvutis teha [kinnitamine on usalda] artiklis kirjeldatud juhised[ verify-a-trust] määrata, kas selle usaldusseos on õigesti konfigureeritud *contoso.com* ja *treyresearch.com* domeenide vahel. Võib-olla peate oodake mõni minut pärast eelmisi toiminguid enne usalda saab kinnitada.

Valikuline juhiseid näitab, kuidas kindlaks teha, kas Domeen usalda töötab ootuspäraselt. Nende toimingute jaoks on installitud Visual Studio arengu arvuti.

1.  Azure'i PowerShelli aknas, käivitage järgmine käsk web taseme loomiseks, kui te pole seadistanud seda varem (, kasutades funktsiooni `Prepare` suvand):

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> WebTier
    ```

    See käsk loob web taseme ja lisab VMs *treyresearch.com* domeeni.

2. ASP.net-i veebirakenduse nimega *TreyResearchWebApp*Visual Studio abil arvutis arengu, luua. .NET Frameworki 4.5.2 kasutada.

3. Valige mall *MVC* ja muuta autentimise *Windowsi autentimist*. Ärge looge rakenduse teenuse pilveteenuses.

3. Koostamine ja käivitage rakendus testimiseks autentimine. Veenduge, et teie praeguse Windowsi kasutajanime kuvatakse paremale klõpsake lehe ülaosas menüüriba.

4. Sulgege Internet Explorer.

5. *Lahendus Exploreri* aknas, paremklõpsake TreyResearchWebApp projekt, klõpsake nuppu *Avalda*.

6. *Veebis avaldamine* aknas, klõpsake nuppu *kohandatud*. Saate luua kohandatud profiili nimega *TreyResearchWebApp*.

7. Klõpsake lehel *ühenduse* määratud *Avalda meetod* *Failisüsteemis* ja määrake kausta nimega *TreyResearchWebApp*, mugav asukohas arengu arvutis.

8. Seadke lehel *sätted* *väljaanne* *konfigureerimine* .

9. *Eelvaates* leheküljele, klõpsake nuppu *Avalda*.

10. Ühenduse iga VM web astme omakorda (kaudu välja hüpata) ja tehke järgmist. Veebirakenduse IP-aadresside taseme VMs on 10.0.1.4 ja 10.0.1.5. Nii vms kasutajanimi on parooliga *treyresearch\testuser* *AweS0me@PW*:

    1. Kopeerige *TreyResearchWebApp* kausta ja selle sisu arvutist arengu *C:\inetpub* kausta.

    2. Teenusekomplekti Internet Information Services (IIS) Manager konsooli, liikuge *Sites\Default veebisaidi* arvutis.

    3. Paanil *toimingud* nuppu *Põhisätted*ja muuta veebisaidi füüsilise tee *%SystemDrive%\inetpub\TreyResearchWebApp*.

    4. Topeltklõpsake paanil *Funktsioonid vaade* *autentimist*. Veenduge, et *Windowsi autentimine* on lubatud ja *Anonüümne autentimine* on keelatud.

11. kohapealse arvutist, avage Internet Explorer ja liikuge sirvides veebilehel http://10.0.1.254 (see on web taseme laadi koormusetasakaalustusteenuse aadress).

12. Sisestage dialoogiboksis *Windowsi Turve* kohapealse domeeni kasutaja mandaat. Määrake kasutajanimi, mida pole olemas *treyresearch* Domeen. Kui kasutate jäljendatud kohapealse keskkonna *contoso* domeeni kasutaja esmalt loomine ja määrake selle kasutaja mandaat.

13. Avalehe kuvamisel kontrollige õige domeen ja kasutajanimi kuvatakse paremale klõpsake lehe ülaosas menüüriba.

## <a name="next-steps"></a>Järgmised sammud

- Lugege heade tavade [domeeni kohapealse lisatakse Azure] pikendamiseks[adds-extend-domain]

- Lugege heade tavade loomise [ADFS-i infrastruktuur] [ adfs] Azure.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[implementing-aad]: ./guidance-identity-aad.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[running-VMs-for-an-N-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[creating-forest-trusts]: https://technet.microsoft.com/library/cc816810(v=ws.10).aspx
[creating-external-trusts]: https://technet.microsoft.com/library/cc816837(v=ws.10).aspx
[naming-conventions]: ./guidance-naming-conventions.md
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[verify-a-trust]: https://technet.microsoft.com/library/cc753821.aspx
[netdom]: https://technet.microsoft.com/library/cc835085.aspx
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://github.com/mspnp/reference-architectures/blob/master/guidance-identity-adds-trust/parameters/onpremise/connection.parameters.json
[incoming-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/incoming-trust.ps1
[outgoing-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/outgoing-trust.ps1
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-adds.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/add-adds-domain-controller.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-public.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-mgmt.parameters.json
[web-vm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/web-vm-domain-join.parameters.json
[solution-components]: [#solution_components]
[0]: ./media/guidance-identity-aad-resource-forest/figure1.png "Turvaline hübriid võrgu arhitektuur eraldi AD domeenide"