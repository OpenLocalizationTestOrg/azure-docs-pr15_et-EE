<properties
   pageTitle="ExpressRoute kliendi ruuteri konfiguratsiooni näidised | Microsoft Azure'i"
   description="Sellelt lehelt leiate ruuteri config näidised Cisco ja kadakamarja ruuterid."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="router-configuration-samples-to-setup-and-manage-routing"></a>Ruuteri konfigureerimine näidised häälestamise ja haldamise marsruutimine

Sellelt lehelt leiate kasutajaliidese ja marsruutimine konfigureerimine näidised Cisco IOS-XE ja kadakas MX sarja ruuterid. Need on mõeldud kasutamiseks ainult suunavad näidiseid ja ei tohi kasutada olemasoleval. Saate töötada oma võrgu jaoks sobiv konfiguratsioone tulla tarnijaga. 

>[AZURE.IMPORTANT] Sellel lehel näidised on mõeldud puhtalt juhised. Tuleb töötada oma tarnija müük / tehniline meeskonnatöö ja võrgu meeskonnatöö tulla asjakohased konfiguratsioon teie vajadustele. Microsoft ei toeta konfiguratsioone loendis selle lehe seotud probleemid. Pöörduge oma seadme tarnija tugi probleeme.

Kõik peerings rakendada marsruuteri konfiguratsiooni proovide allpool. Marsruutimise kohta lisateabe saamiseks vaadake [ExpressRoute peerings](expressroute-circuit-peerings.md) ja [ExpressRoute marsruutimise nõuetele](expressroute-routing.md) .

## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS-XE vastavalt ruuterid

Selles jaotises näidised taotleda ruuteri opsüsteemi IOS-XE OS pere.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. liideste ja sub liideste seadistamine

Vajate sub kasutajaliidese silmitsemine iga ruuteri loote Microsoft kohta. Sub kasutajaliidese tähistatud VLAN ID või paari virnastatud VLAN ID ja IP-aadress.

#### <a name="dot1q-interface-definition"></a>Dot1Q kasutajaliidese määratlus

See näide pakub sub kasutajaliidese definitsiooni sub kasutajaliidese ühe VLAN ID-ga. VLAN ID on kordumatu silmitsemine kohta. Viimase oktettide aadressi IPv4 alati on paaritu arv.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

#### <a name="qinq-interface-definition"></a>QinQ kasutajaliidese määratlus

See näide pakub sub kasutajaliidese definitsiooni sub kasutajaliidese koos kahe VLAN ID-d. Väline VLAN ID (s-silt), kui kasutatakse jääb samaks üle kõik peerings. Sisemine VLAN ID (c-silt) on kordumatu silmitsemine kohta. Viimase oktettide aadressi IPv4 alati on paaritu arv.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>
    
### <a name="2-setting-up-ebgp-sessions"></a>2. eBGP seanssi luua

Te peate setup Microsoft BGP seansi jaoks iga silmitsemine. Allpool valimi võimaldab Microsoft BGP seansi häälestamine. Kui kasutasite sub kasutajaliidese IPv4 aadress on a.b.c.d, BGP naaber (Microsoft) IP-aadress on a.b.c.d+1. Viimase oktettide BGP naabri IPv4 aadress alati on paarisarv.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. häälestamise eesliiteid üle BGP seansi reklaamida

Saate konfigureerida oma marsruuteri valige eesliidete Microsoft reklaami. Saate seda teha allpool valimi abil.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. kaartide marsruutimiseks

Saate kasutada marsruutimiseks-kaardid ja eesliide, mis on loetletud filter eesliiteid levitatud teie võrku. Saate selle ülesande valimi allpool. Veenduge, et vastav eesliide loendite häälestus on.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>Kadakas MX sarja ruuterid 

Selles jaotises näidised rakendada mis tahes kadakas MX sarja ruuterid.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. liidesed ja sub liidesed seadistamine

#### <a name="dot1q-interface-definition"></a>Dot1Q kasutajaliidese määratlus

See näide pakub sub kasutajaliidese definitsiooni sub kasutajaliidese ühe VLAN ID-ga. VLAN ID on kordumatu silmitsemine kohta. Viimase oktettide aadressi IPv4 alati on paaritu arv.

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


#### <a name="qinq-interface-definition"></a>QinQ kasutajaliidese määratlus

See näide pakub sub kasutajaliidese definitsiooni sub kasutajaliidese koos kahe VLAN ID-d. Väline VLAN ID (s-silt), kui kasutatakse jääb samaks üle kõik peerings. Sisemine VLAN ID (c-silt) on kordumatu silmitsemine kohta. Viimase oktettide aadressi IPv4 alati on paaritu arv.

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2. luua eBGP seansid

Te peate setup Microsoft BGP seansi jaoks iga silmitsemine. Allpool valimi võimaldab Microsoft BGP seansi häälestamine. Kui kasutasite sub kasutajaliidese IPv4 aadress on a.b.c.d, BGP naaber (Microsoft) IP-aadress on a.b.c.d+1. Viimase oktettide BGP naabri IPv4 aadress alati on paarisarv.

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. häälestamise eesliiteid üle BGP seansi reklaamida

Saate konfigureerida oma marsruuteri valige eesliidete Microsoft reklaami. Saate seda teha allpool valimi abil.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4. kaartide marsruutimiseks

Saate kasutada marsruutimiseks-kaardid ja eesliide, mis on loetletud filter eesliiteid levitatud teie võrku. Saate selle ülesande valimi allpool. Veenduge, et vastav eesliide loendite häälestus on.

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>Järgmised sammud

Vt lisateavet [ExpressRoute KKK](expressroute-faqs.md) .
