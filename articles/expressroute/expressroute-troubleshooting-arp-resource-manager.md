<properties 
   pageTitle="ExpressRoute tõrkeotsing juhend - saada ARP tabelid | Microsoft Azure'i"
   description="See leht sisaldab juhiseid saada soovitud ARP tabelid on ExpressRoute ringi"
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

#<a name="expressroute-troubleshooting-guide---getting-arp-tables-in-the-resource-manager-deployment-model"></a>ExpressRoute tõrkeotsing juhend - ressursihaldur juurutamise mudeli tabelite ARP otsimine

> [AZURE.SELECTOR]
[PowerShelli - ressursihaldur](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShelli – klassikaline](expressroute-troubleshooting-arp-classic.md)

Selles artiklis tutvustatakse ARP tabelite jaoks oma ExpressRoute ringi lugege juhiseid. 

>[AZURE.IMPORTANT] See dokument on mõeldud aitavad diagnoosimine ja lihtsat seotud probleemide lahendamine. See on mõeldud Microsofti tugiteenuste Asenda. Kui te ei saa allpool kirjeldatud juhised probleemi lahendamiseks tuleb teil avada tugi Piletite koos [Microsofti tugiteenuste](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Aadresside eraldusvõime Protocol (ARP) ja ARP tabelid
Aadress eraldusvõime Protocol (ARP) on määratletud [RFC 826](https://tools.ietf.org/html/rfc826)layer 2 protokoll. ARP kasutatakse vastendama Ethernet aadress (MAC aadress) ip-aadressiga.

ARP tabelis on ära toodud ipv4 aadress ja MAC-aadress kindla silmitsemine kaardistada. ARP tabelist ExpressRoute ringi silmitsemine pakub järgmist teavet iga kasutajaliidese (esmaseid ja teiseseid)

1. Kohapealse ruuteri kasutajaliidese ip-aadressi MAC-aadressi vastenduse
2. ExpressRoute ruuteri kasutajaliidese ip-aadressi MAC-aadressi vastenduse
3. Vanuse kaardistamine

ARP tabelid aitavad layer 2 konfiguratsiooni valideerimiseks ja tõrkeotsingu lihtsa kiht 2 ühenduvusprobleemide. 

Näide ARP tabel: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Järgmine jaotis annab teavet kuvamise näinud ExpressRoute serva ruuterid ARP tabelid. 

## <a name="prerequisites-for-learning-arp-tables"></a>Õppekeskuse ARP tabelid eeltingimused

Veenduge, et enne, kui te edu täpsemaks on järgmine

 - Kehtivate ExpressRoute ringi vähemalt üks silmitsemine konfigureeritud. Ringi ühenduvuse pakkuja peab olema täielikult konfigureeritud. (Või ühenduvuse pakkuja) peavad on konfigureeritud vähemalt üks peerings (Azure'i privaatne, Azure avalik ja Microsofti) selle ringi.
 - IP-aadresside vahemikud kasutatakse konfigureerimise peerings (Azure'i privaatne, Azure avalik ja Microsoft). Vaadake üle ip address ülesande näidetes [ExpressRoute marsruutimise nõuded lehe](expressroute-routing.md) mõista, kuidas ip-aadressid on vastendatud liideste küljel ja ExpressRoute pool. Saate teavet silmitsemine konfiguratsiooni, vaadates [ExpressRoute silmitsemine konfiguratsiooni lehele](expressroute-howto-routing-arm.md).
 - Teave teie võrgu meeskond / ühenduvuse pakkuja kohta MAC-aadressid liideste kasutada järgmisi IP-aadressid.
 - Peate uusima PowerShelli mooduli Azure (1.50 või uuem versioon).

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>Saada soovitud ARP tabelid oma ExpressRoute ringi
See jaotis sisaldab juhiseid kuvamise ARP tabelid kohta silmitsemine PowerShelli kaudu. Teie või teie ühendus pakkuja on konfigureeritud, silmitsemine enne areneb edasi. Iga ringi on kaks teed (esmaseid ja teiseseid). Saate vaadata iga tee ARP tabelist sõltumatult.

### <a name="arp-tables-for-azure-private-peering"></a>Azure'i privaatne silmitsemine ARP tabelid
Järgmine cmdlet-käsk pakub soovitud ARP tabelid Azure privaatne silmitsemine

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary
        
        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Väljundi on allpool üks teed

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Azure'i avaliku silmitsemine ARP tabelid
Järgmine cmdlet-käsk pakub soovitud ARP tabelid Azure avaliku silmitsemine

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary
        
        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Väljundi on allpool üks teed

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Microsoft silmitsemine ARP tabelid
Järgmine cmdlet-käsk pakub soovitud ARP tabelid Microsoft silmitsemine

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary
        
        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Väljundi on allpool üks teed

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Kuidas kasutada seda teavet
ARP tabeli lisamine silmitsemine saab kasutada määratlemiseks valideerimiseks layer 2 konfigureerimine ja Ühenduvus. Selles jaotises antakse ülevaade ARP tabelid ilme erinevate stsenaariumide.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>Kui soovitud ringi on funktsionaalseid olekus (oodatud riik) ARP tabel

 - ARP tabel on kirje kohapealse pool lubatud IP-aadress ja MAC-aadress ja sarnase suunamise Microsoft pool. 
 - Kohapealse ip-aadressi viimase oktettide alati on paaritu arv.
 - Microsofti ip-aadressi viimase oktettide alati on paarisarv.
 - Sama MAC-aadress kuvatakse Microsoft pool kõik 3 peerings (esmane / teisene). 


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP tabelist, kui kohapealse / ühenduvuse pakkuja pool on probleeme

 - ARP tabelis kuvatakse ainult üks kirje. See näitab kaardistamine MAC-aadress ja IP-aadressi kasutatakse Microsofti poole. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Avage oma ühenduvuse teenusepakkuja silumine küsimusi esitada tugiteenuse taotluse. 


### <a name="arp-table-when-microsoft-side-has-problems"></a>ARP tabel, kui Microsoft side on probleeme

 - Ei kuvata ARP tabel, mis näidatud silmitsemine kui on probleeme Microsofti pool. 
 -  Avage [Microsofti tugiteenuste](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)tugi Piletite. Saate määrata, et teil on probleeme layer 2 Ühenduvus. 

## <a name="next-steps"></a>Järgmised sammud

 - Kinnitage oma ExpressRoute ringi konfiguratsioone Layer 3
     - Saada marsruutimiseks Kokkuvõte BGP seansside oleku määratlemine 
     - Saada marsruutimiseks tabeli abil saate määratleda, millist eesliidete tähised on avaldatud üle ExpressRoute
 - Kinnitage andmeedastus vaadates baiti /
 - Kui teil on endiselt probleeme, avage tugi Piletite koos [Microsofti tugiteenuste](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
