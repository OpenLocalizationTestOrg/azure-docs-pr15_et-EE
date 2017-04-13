<properties
   pageTitle="ExpressRoute tõrkeotsingu juhend: saada ARP tabelid | Microsoft Azure'i"
   description="Sellelt lehelt leiate juhised toomiseks soovitud ARP tabelid on ExpressRoute ringi."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carolz"
   editor="tysonn"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="expressroute-troubleshooting-guide-getting-arp-tables-in-the-classic-deployment-model"></a>ExpressRoute tõrkeotsingu juhend: saada ARP tabelid klassikaline juurutamise mudeli

> [AZURE.SELECTOR]
[PowerShelli - ressursihaldur](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShelli – klassikaline](expressroute-troubleshooting-arp-classic.md)

Selles artiklis tutvustatakse aadress eraldusvõime Protocol (ARP) tabelite jaoks oma Azure'i ExpressRoute ringi toomiseks vajalikud toimingud.

>[AZURE.IMPORTANT] See dokument on mõeldud aitavad diagnoosimine ja lihtsat seotud probleemide lahendamine. See on mõeldud Microsofti tugiteenuste Asenda. Kui see probleemi ei lahenda, kasutades järgmisi juhiseid, avage esitada tugiteenuse taotluse [Microsoft Azure'i abi](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)+ tugi.

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Aadresside eraldusvõime Protocol (ARP) ja ARP tabelid
ARP on kiht 2 protokoll, mis on määratletud [RFC 826](https://tools.ietf.org/html/rfc826). ARP saab vastendada IP-aadress aadressi Ethernet (MAC aadress).

Mõne ARP tabelis on ära toodud IPv4 aadress ja MAC-aadress kindla silmitsemine kaardistada. ARP tabelist ExpressRoute ringi silmitsemine pakub iga liidese (esmaseid ja teiseseid) järgmine teave:

1. Kohapealse ruuteri kasutajaliidese IP-aadressi MAC-aadressi vastenduse
2. ExpressRoute ruuteri kasutajaliidese IP-aadressi MAC-aadressi vastenduse
3. Vanus kaardistamine

ARP tabelid aitab kiht 2 konfiguratsiooni valideerimiseks ja põhilised kiht 2 ühenduvusprobleemide tõrkeotsing.

Järgmine on näide ARP tabeli:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Järgmises jaotises kirjeldatakse ARP tabelid, mis on näinud ExpressRoute serva ruuterid kuvamiseks.

## <a name="prerequisites-for-using-arp-tables"></a>ARP tabelite kasutamise eeltingimused

Veenduge, et enne jätkamist on järgmine:

 - Kehtivate ExpressRoute ringi, mis on konfigureeritud vähemalt üks silmitsemine. Ringi ühenduvuse pakkuja peab olema täielikult konfigureeritud. (Või ühenduvuse pakkuja) tuleb konfigureerida vähemalt üks peerings (Azure'i privaatne, Azure avalik või Microsofti) selle ringi.

 - IP-aadresside vahemikud kasutatavate konfigureerimise peerings (Azure'i privaatne, Azure avalik ja Microsoft). Vaadake üle IP address ülesande näidetes [ExpressRoute marsruutimise nõuded lehe](expressroute-routing.md) mõista, kuidas IP-aadressid on vastendatud liideste oma aise ja ExpressRoute küljel. Saate teavet silmitsemine konfiguratsiooni, vaadates [ExpressRoute silmitsemine konfiguratsiooni lehele](expressroute-howto-routing-classic.md).

 - Teave teie võrgu meeskonnatöö või ühenduvust pakkuja MAC-aadressid liideste, mida kasutatakse koos IP-aadressid.

 - Uusima Windows PowerShelli moodul Azure (1.50 või uuem versioon).

## <a name="arp-tables-for-your-expressroute-circuit"></a>Oma ExpressRoute ringi ARP tabelid
Sellest jaotisest leiate juhised selle kohta, kuidas vaadata ARP tabelid igat tüüpi silmitsemine PowerShelli abil. Enne jätkamist, teie või teie ühenduvuse pakkuja peab konfigureerimine on silmitsemine. Iga ringi on kaks teed (esmaseid ja teiseseid). Saate vaadata iga tee ARP tabelist sõltumatult.

### <a name="arp-tables-for-azure-private-peering"></a>Azure'i privaatne silmitsemine ARP tabelid
Järgmine cmdlet-käsk pakub soovitud ARP tabelid Azure privaatne silmitsemine:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Järgmine on näide väljundi jaoks üks teed:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Azure'i avaliku silmitsemine ARP tabelid:
Järgmine cmdlet-käsk pakub soovitud ARP tabelid Azure avaliku silmitsemine:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Järgmine on näide väljundi jaoks üks teed:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Järgmine on näide väljundi jaoks üks teed:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Microsoft silmitsemine ARP tabelid
Järgmine cmdlet-käsk pakub soovitud ARP tabelid Microsoft silmitsemine:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Väljundi on näidatud allpool mõni aadressidel:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Kuidas kasutada seda teavet
ARP tabeli lisamine silmitsemine saab valideerimiseks kiht 2 konfigureerimine ja Ühenduvus. Selles jaotises antakse ülevaade ARP tabelid erinevates stsenaariumides ilme.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>Kui soovitud ringi toimiv (oodatud) riik ARP tabel

 - ARP tabelis on kirje kohapealse side lubatud IP- ja MAC-aadress ja sarnase kirje Microsoft poolel.
 - Viimase oktettide kohapealse IP-aadress on alati paaritu arv.
 - Viimase oktettide Microsofti IP-aadress on alati paarisarv.
 - Kõigi kolme peerings (esmane/teisene) küljel Microsoft kuvatakse sama MAC-aadress.


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>Kui see on kohapealse või kui Ühenduvus-pakkuja side on probleeme ARP tabel

 ARP tabelis kuvatakse ainult üks kirje. See näitab kaardistamine MAC-aadress ja IP-aadress, mida kasutatakse Microsoft pool.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Kui teil tekib probleeme selliselt, avage esitada tugiteenuse taotluse selle probleemi lahendamiseks ühenduvuse teenusepakkuja juures.


### <a name="arp-table-when-the-microsoft-side-has-problems"></a>ARP tabel, kui Microsoft side on probleeme

 - Ei kuvata ARP tabel, mis näidatud silmitsemine kui on probleeme Microsofti pool.
 -  Avage [Microsoft Azure'i abi](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)+ tugi esitada tugiteenuse taotluse. Saate määrata, et teil on probleeme kiht 2 Ühenduvus.

## <a name="next-steps"></a>Järgmised sammud

 - Kinnitage oma ExpressRoute ringi konfiguratsioone Layer 3:
     - Saada marsruudi Kokkuvõte BGP seansside oleku määratlemine.
     - Saan kindlaks teha, millised eesliidete tähised on avaldatud üle ExpressRoute marsruutimiseks tabeli.
 - Kinnitage andmeedastus vaadates baiti sisse ja välja.
 - Kui teil on endiselt probleeme, avage esitada tugiteenuse taotluse [Microsoft Azure'i abi](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) + tugi.
