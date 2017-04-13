
<properties
   pageTitle="Azure'i juhised | mustrite ja tavad | Microsoft Azure'i"
   description="Azure'i viide Arhitektuurides"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="christb"/>

# <a name="azure-reference-architectures"></a>Azure'i viide arhitektuur

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

_See sisu on aktiivne arendustegevus. See on kasulik täna, et teeme selle kättesaadavaks eelvaade. Me väärtus teie tagasisidet._

Meie viide arhitektuurides on korraldatud stsenaariumi järgi rühmitatuna mitme seotud arhitektuurides.
Iga üksiku arhitektuur pakub tavadega ja kirjeldavad juhised, samuti käivitatava osa, mis hõlmab soovitusi.
Paljud selle arhitektuur on järk-järgult; koostamise peal eelnev arhitektuurides, millel on vähem nõuetele.

## <a name="designing-your-infrastructure-for-resiliency"></a>Oma taristu paindlikkust kujundamine

Selle sarja Soovitatavad tavad optimaalse VM konfiguratsiooni algab ja kulmineerub mitme piirkonna juurutuse Tõrkesiirde abil.

- [Opsüsteemi Windows VM Azure](guidance-compute-single-vm.md)
- [Töötab Linux VM Azure](guidance-compute-single-vm-linux.md)
- [Töötab mitu VMs skaleeritavus ja kättesaadavuseks.](guidance-compute-multi-vm.md)
- [Opsüsteemi Windows VMs N-astme arhitektuur](guidance-compute-n-tier-vm.md)
- [Opsüsteemi Linux VMs N-astme arhitektuur](guidance-compute-n-tier-vm-linux.md)
- [Opsüsteemi Windows VMs mitme piirkondade jaoks kõrge-saadavus](guidance-compute-multiple-datacenters.md)
- [Mitme piirkonna jaoks kõrge-saadavus Linux VMs töötab?](guidance-compute-multiple-datacenters-linux.md)

## <a name="connecting-your-on-premises-network-to-azure"></a>Kohapealse võrgu ühenduse Azure'i

See sari algab näidates olemasoleva võrgu ühenduse Azure'i võimalust. Klõpsake laiendatud katta ja turvalisus.

- [Hübriidjuurutuse võrgu arhitektuur Azure rakendamise ja kohapealsete VPN](guidance-hybrid-network-vpn.md)
- [Hübriidjuurutuse võrgu arhitektuur koos Azure'i ExpressRoute rakendamine](guidance-hybrid-network-expressroute.md)
- [Rakendamise tugevalt saadaval hübriid võrgu arhitektuur](guidance-hybrid-network-expressroute-vpn-failover.md)

## <a name="securing-your-hybrid-network"></a>Hübriidjuurutuse võrgu turvamine

See sari hõlmab järeleproovitud tavade loomine DMZ Azure turvalist ühendust oma kohapealse andmekeskuse ja Interneti-ühendus.

- [Azure'i ja teie asutusesisese andmekeskuse vahelise on DMZ rakendamine](guidance-iaas-ra-secure-vnet-hybrid.md)
- [Azure'i ja Interneti-ühendus on DMZ rakendamine](guidance-iaas-ra-secure-vnet-dmz.md)

## <a name="providing-identity-services"></a>Identiteedi teenuste

Selle sarja käivitub, näidates, kuidas kasutada kasutaja autentimise Azure Azure Active Directory. Klõpsake laiendatud kataks keeruliste stsenaariumide ulatub oma lisatakse taristu Azure ja delegeerimine ADFS-i abil.

- [Azure Active Directory rakendamine](./guidance-identity-aad.md)
- [Active Directory kataloogiteenuste ulatub (LISAB) Azure](./guidance-identity-adds-extend-domain.md)
- [Azure Active Directory Directory Services (LISAB) ressursi mets loomine](./guidance-identity-adds-resource-forest.md)
- [Azure Active Directory Federation Services (ADFS) rakendamine](./guidance-identity-adfs.md)

## <a name="architecting-scalable-web-application-using-azure-paas"></a>Architecting scalable veebirakenduse Azure'i PaaS

See sari hõlmab soovitused scalable ja tugevalt saadaval veebirakenduste loomine. 

- [Lihtsa veebirakenduse](guidance-web-apps-basic.md)
- [Veebirakenduse skaleeritavus parandamine](guidance-web-apps-scalability.md)
- [Veebirakenduse kõrge kättesaadavus](guidance-web-apps-multi-region.md)
