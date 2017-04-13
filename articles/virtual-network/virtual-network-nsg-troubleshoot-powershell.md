<properties 
   pageTitle="Tõrkeotsing võrgu turberühmad – PowerShelli | Microsoft Azure'i"
   description="Saate teada, kuidas tõrkeotsing võrgu turberühmad Azure'i ressursihaldur juurutamise mudeli Azure PowerShelli kaudu."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Tõrkeotsing võrgu turberühmad Azure PowerShelli abil

> [AZURE.SELECTOR]
- [Azure'i portaal](virtual-network-nsg-troubleshoot-portal.md)
- [PowerShelli](virtual-network-nsg-troubleshoot-powershell.md)

Kui arvuti virtuaalne (VM) konfigureeritud võrgu turberühmad (NSGs) ja esineb VM ühenduvusprobleemide, selles artiklis antakse ülevaade diagnostika võimalused NSGs täpsemaks tõrkeotsinguks.

NSGs võimaldavad juhtida liiklust tüüpi selle voo ja sealt virtuaalmasinates (VMs). NSGs saab rakendada alamvõrku Azure virtuaalse võrgu (VNet), võrgu liidesed (NIC) või mõlemad. Efektiivse kohaldatakse Võrguadapter on liitmise reeglid, mis on olemas NSGs, mis on rakendatud Võrguadapter ja alamvõrku, mis on ühendatud. Reeglite üle nende NSGs vahel saate omavahel konflikti ja mõjutada on VM võrguühendus.  

Tõhus turvalisuse reegleid saate vaadata oma NSGs vastavalt oma VM NICs on rakendatud. Selles artiklis kirjeldatakse järgmiste reeglite kasutamine Azure ressursihaldur juurutamise mudeli VM ühenduvusprobleemide tõ. Kui olete tuttav VNet ja NSG põhimõtet, lugeda artikleid [Virtual võrk](virtual-networks-overview.md) ja [võrgu turberühmad](virtual-networks-nsg.md) ülevaade.

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Tõhus turvalisus reeglite kasutamine VM liiklust tõrkeotsing

Stsenaarium, et järgmine on näide levinud Interneti-ühenduse probleemi:

Nimega *VM1* VM on osa on alamvõrgu *Subnet1* sees on VNet nimega *WestUS-VNet1*nimega. VM, kasutades TCP pordi 3389 RDP ühenduse loomise katse nurjus. NSGs rakendatakse NIC *VM1-NIC1* ja alamvõrgu *Subnet1*. TCP-port 3389 liiklust on lubatud NSG, mis on seotud võrgu liidese *VM1-NIC1*, kuid TCP ping-VM1 isiku porti 3389 nurjub.

Kuigi see näide kasutab TCP port 3389, saab määratlemiseks sissetulevate ja väljaminevate ühenduse tõrkeid, mis tahes Port toimige järgmiselt.

## <a name="detailed-troubleshooting-steps"></a>Üksikasjalikud juhised tõrkeotsing
Järgmiste juhiste VM NSGs tõrkeotsing:

1. Azure'i sisse logida ja Azure PowerShelli seansi käivitamine Kui olete tuttav Azure PowerShelli, lugege artiklit [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) kaudu.

2. Sisestage järgmine käsk, et tagastada kõik NSG reeglid, mis on rakendatud Töölehefunktsioon *VM1-NIC1* ressursirühm *RG1*Võrguadapter.

        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Kui te ei tea mõne NIC nimi, sisestage järgmine käsk tuua kõik NICs rühmas ressursside nimed: 
    
    >`Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`

    Järgmine tekst on valimi *VM1-NIC1* NIC tagastada tegelik reeglid väljund.

        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
        
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]

    Võtke arvesse järgmist teavet väljund.

    - On kaks **NetworkSecurityGroup** sektsiooni: üks on seostatud mõne alamvõrgu (*Subnet1*) ja ühte on seostatud mõne NIC (*VM1-NIC1*). Selles näites on NSG on rakendatud iga.
    - **Seos** on kujutatud ressursi (alamvõrgu või NIC) antud NSG on seostatud. Kui NSG ressurss on teisaldatud/lahtiseostatud vahetult enne selle käsu, peate ootama mõne hetke muudatuse kajastamiseks käsu väljund. 
    - Reegli nimed, mis on *defaultSecurityRules*koos sissejuhatava põhjendusega: kui an NSG on loodud, mitu turvalisus vaikereeglit luuakse kogumahtu. Vaikereeglit ei saa eemaldada, kuid neid saab alistada kõrgema prioriteet reeglite abil.
     Lugege lisateavet NSG vaikereeglit turvalisus [NSG ülevaade](virtual-networks-nsg.md#default-rules) artiklit.
    - **ExpandedAddressPrefix** paisub aadress eesliiteid NSG vaikimisi silte. Siltide tähistavad mitu aadressi eesliiteid. Sildid laiendamine võib olla kasulik VM ühenduvuse konkreetne aadress eesliiteid ja sealt tõrkeotsingul. Näiteks koos VNET silmitsemine, VIRTUAL_NETWORK sildi laiendatud piilus VNet eesliiteid eelmise väljund.

        >[AZURE.NOTE] Käsk ainult kuvatakse efektiivse reegleid kui ka NSG on seostatud mõne alamvõrku, on NIC või mõlemad. VM võib olla mitu NICs koos rakendatud erinevad NSGs. Tõrkeotsingu, käivitage käsk iga NIC.
        
3. Lihtne filtreerimine suurema hulga NSG reeglid, sisestage täpsemaks tõrkeotsinguks järgmised käsud: 

        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView

    Koordinaatvõrgu kuvamine, rakendatakse filtri RDP liikluse (TCP port 3389), nagu on näidatud järgmisel pildil:

    ![Reeglite loend](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
    
4. Nagu näete ruudustiku vaate, on mõlemad lubada ja keelata RDP reeglid. Etappi 2 väljund näitab, et *DenyRDP* reegel on NSG, mis on rakendatud alamvõrgu sisse. Sissetulevad reeglid, rakendatakse alamvõrgu NSGs töödeldakse esmalt. Vaste leidmisel NSG, mis on rakendatud võrgu liidese ei töödelda. Sel juhul *DenyRDP* reeglit: alamvõrgu blokeerib RDP VM (**VM1**).

    >[AZURE.NOTE] VM võib olla mitu NICs manustatud. Iga võib olla ühendatud muu alamvõrgu. Kuna eespool kirjeldatud käsud on vastuolus Võrguadapter, on oluline, määrake ühenduvuse jätmine esineb NIC tagamiseks. Kui te pole kindel, saate käivitada käsud alati iga NIC manustatud VM suhtes.

5. RDP VM1 sisse, et muuta *Keelamiseks RDP (3389)* reegli *Luba RDP(3389)* **Subnet1-NSG** NSG sisse. Veenduge, et TCP port 3389 on avatud, avades RDP ühenduse VM või PsPing tööriista abil. Te saate lisateavet PsPing lugedes [PsPing allalaadimine lehe](https://technet.microsoft.com/sysinternals/psping.aspx)

    Saate või reeglid eemaldamine on NSG abil teabe väljund järgmine käsk:

        Get-Help *-AzureRmNetworkSecurityRuleConfig
        

## <a name="considerations"></a>Kaalutlused

Kui ühendusprobleemide tõrkeotsing, võtke arvesse järgmist.

- NSG vaikereeglit kuvatakse sissetuleva juurdepääsu Interneti kaudu ja võimaldab üksnes VNet sissetulev liiklus. Reeglite lisatakse selgesõnaliselt sissetuleva juurdepääsu lubamiseks Internet, vastavalt vajadusele.
- Kui NSG turvalisuse reeglid põhjustab mõne VM võrguühenduse nurjumise, probleem võib olla järgmised.
    - Tulemüüri tarkvara on VM operatsioonisüsteemis töötab
    - Marsruudib virtuaalne seadmed või kohapealse liikluse konfigureeritud. Kohapealse sunnitud tunneling kaudu saate Interneti-liikluse ümber. See säte, olenevalt sellest, kuidas käsitleb kohapealse võrgu riistvara see liiklus ei pruugi RDP/SSH ühendus Interneti kaudu oma VM. Lugege artiklit [Tõrkeotsing marsruudib](virtual-network-routes-troubleshoot-powershell.md) teavet, mida võib takistada voo ja sealt VM liikluse marsruutimiseks probleemide väljaselgitamiseks. 
- Kui teil on piilus VNets, vaikimisi, laiendage VIRTUAL_NETWORK sildi automaatselt kaasatav eesliiteid piilus VNets. Saate vaadata nende eesliiteid loendis **ExpandedAddressPrefix** seotud VNet silmitsemine ühenduvuse seotud probleemide tõrkeotsinguks. 
- Tõhus turvalisuse reeglid kuvatakse vaid siis, kui on on seostatud soovitud VM NIC ja või alamvõrgu NSG. 
- Kui tegemist on pole NSGs seostatud NIC alamvõrgu ja teil on määratud oma VM avaliku IP-aadress, avatakse kõik pordid sissetulevate ja väljaminevate juurdepääsu. Kui VM on avaliku IP-aadressi, rakendades NSGs NIC või alamvõrgu tungivalt soovitatav.  
