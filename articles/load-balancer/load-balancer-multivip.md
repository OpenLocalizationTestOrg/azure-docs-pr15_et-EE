<properties
   pageTitle="Mutiple VIP jaoks pilveteenus"
   description="MultiVIP ja kuidas panna mitu VIP pilveteenus ülevaade"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-multiple-vips-for-a-cloud-service"></a>Mitme VIP pilvepõhise teenuse konfigureerimine

Pääsete Azure pilveteenustega avaliku Interneti kaudu, kasutades Azure esitatud IP-aadress. Selle avaliku IP-aadress on edaspidi VIP (virtuaalne IP) Kuna see on seotud Azure laadi koormusetasakaalustusteenuse ja pole virtuaalse masina (VM) eksemplarid teenuses pilve. Ühe VIP abil pääsete juurde mis tahes VM eksemplari mõnda pilveteenusesse.

Kuid on stsenaariumid, kus peate mitu VIP kirjena osutage sama pilveteenuses. Näiteks võib teie pilveteenuses majutada mitme veebisaidid, mis nõuab SSL-i ühenduvuse vaikimisi pordi 443, iga saidi majutab erinevad kliendi jaoks või rentniku. Selle stsenaariumi korral peate olema erinevad avaliku suunatud IP-aadress iga veebisaidi jaoks. Alloleval joonisel on näidatud tüüpiline mitme rentniku web hosting, kellel on vaja mitut SSL-sertide sama avaliku Port.

![Mitme VIP SSL-i stsenaarium](./media/load-balancer-multivip/Figure1.png)

Ülaltoodud näites kõik VIP kasutada sama avaliku port (443) ja on liikluse ümber üks või rohkem koormusetasakaalustusega VMs kordumatu privaatne pordi majutusteenuse kõik veebilehed pilveteenusesse sisemise IP-aadress.

>[AZURE.NOTE] Teine olukord on vaja kasutada mitut VIP majutab mitme SQL-i AlwaysOn kättesaadavus rühma kuulajatele Virtuaalmasinates samu kohta.

VIP on dünaamiline vaikimisi, mis tähendab, et tegelik IP-aadress on pilveteenusesse võivad aja jooksul muutuda. Et vältida mis juhtub, saate reserveerida VIP teie teenuse jaoks. Reserveeritud VIP kohta leiate lisateavet teemast [Reserveeritud avaliku IP](../virtual-network/virtual-networks-reserved-public-ip.md).

>[AZURE.NOTE] Palun vaadake [hinnad IP-aadressi](https://azure.microsoft.com/pricing/details/ip-addresses/) teavet klõpsake VIP hinnad ja reserveeritud IP-d.

Saate kontrollimine VIP, mis kasutavad teie pilveteenustega PowerShelli abil kui ka lisada ja eemaldada VIP, seostada VIP lõpp- ja konfigureerida laadimine kohta teatud VIP tasakaalustamiseks.

## <a name="limitations"></a>Piirangud

Sel ajal, mitme VIP funktsioonid on piiratud järgmistel juhtudel:

- **Ainult IaaS**. Mitme VIP saate lubada vaid pilveteenuseid, mis sisaldavad VMs. Ei saa kasutada mitme VIP PaaS stsenaariumid rolli aknad.
- **PowerShelli ainult**. PowerShelli abil saate hallata ainult mitme VIP.

Need piirangud on ajutine ja võib igal ajal muuta. Veenduge, et selle lehe tulevaste muudatuste kinnitamiseks uuesti.


## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Kuidas lisada VIP pilveteenus

Lisada VIP teenust, käivitage PowerShelli järgmine käsk:

    Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

See käsk kuvab tulemi, mis on sarnane järgmises näites:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>Kuidas eemaldada VIP pilveteenus

Eeltoodud näites teenust lisatud VIP eemaldamiseks käivitage PowerShelli käsk:

    Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

>[AZURE.IMPORTANT] VIP saate eemaldada ainult siis, kui tal pole sellega seotud lõpp-punkte.

## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Kuidas hankida teavet VIP pilveteenus

Tuua VIP, mis on seotud pilveteenus, käivitage järgmine PowerShelli skripti:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Skripti, kuvatakse tulemus on sarnane järgmises näites:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

Selles näites on pilveteenusesse 3 VIP:

- **Vip1** on vaikimisi VIP, teate, et kuna IsDnsProgrammedName väärtus on seatud väärtusele tõene.
- **VIP2** ja **Vip3** ei kasutata nagu nad ei saa IP-aadressid. Need kasutatakse ainult Kui seote lõpp VIP abil.

>[AZURE.NOTE] Teie tellimus on ainult tasuline eest VIP kui need on seostatud lõpp. Hinnad kohta leiate lisateavet teemast [hinnad IP-aadress](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Kuidas siduda VIP-lõpp

Pilvepõhise teenuse lõpp-VIP siduda käivitage PowerShelli käsk:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 `
  	| Update-AzureVM

See käsk loob lingitud nimega *Vip2* pordi *80*ja seda VM nimega *myVM1* pilveteenuses, nimega *myService* abil *TCP* port *8080*lingid VIP lõpp.

Kontrollige konfiguratsiooni, käivitage PowerShelli järgmine käsk:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Väljund näeb välja järgmine näide:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Laadi kohta teatud VIP tasakaalustamiseks lubamine

Saate ühe VIP seostada mitme virtuaalmasinates koormuse tasakaalustamiseks. Näiteks peate nimega *myService*pilveteenus ja kahe virtuaalmasinates nimega *myVM1* ja *myVM2*. Ja oma pilveteenuses on mitu VIP, üks neist nimega *Vip2*. Kui soovite tagada, et kogu liikluse pordi *81* kohta *Vip2* saabuv *myVM1* ja *myVM2* pordi *8181*, käivitage järgmine PowerShelli skripti kohta:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

    Get-AzureVM -ServiceName myService -Name myVM2 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

Saate uuendada oma laadi koormusetasakaalustusteenuse kasutada eri VIP. Näiteks kui käivitate PowerShelli käsu alla, saate muuta laadi tasakaalustamiseks kasutada nimega Vip1 VIP määramine:

    Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1

## <a name="next-steps"></a>Järgmised sammud

[Log analüüsiteabe Azure laadi saldo](load-balancer-monitor-log.md)

[Internet suunatud laadi koormusetasakaalustusteenuse ülevaade](load-balancer-internet-overview.md)

[Vastastikuste laadi koormusetasakaalustusteenuse Internetis alustamine](load-balancer-get-started-internet-arm-ps.md)

[Virtuaalse võrgu ülevaade](../virtual-network/virtual-networks-overview.md)

[Reserveeritud IP REST API-d](https://msdn.microsoft.com/library/azure/dn722420.aspx)