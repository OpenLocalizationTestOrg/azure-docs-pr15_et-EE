<properties
   pageTitle="Konfigureerige laadi koormusetasakaalustusteenuse SQL alati | Microsoft Azure'i"
   description="Laadi koormusetasakaalustusteenuse töötamiseks SQL alati ja kuidas võimendada PowerShelli loomiseks laadi koormusetasakaalustusteenuse rakendamiseks SQL-i konfigureerimine"
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

# <a name="configure-load-balancer-for-sql-always-on"></a>Konfigureerige laadi koormusetasakaalustusteenuse SQL alati

Nüüd saate käivitada SQL serveri Kättesaadavusrühmad ILB. Kättesaadavus rühma on SQL serveri lipulaev lahendus kõrge kättesaadavus ja Avariijärgne taaste. Kättesaadavus rühma kuulajale võimaldab kliendi rakenduste peamine koopia, olenemata sellest, mitme kujundusmuudatusi konfiguratsiooni sujuvalt ühendada.

Kuulajale (DNS) nimi on vastendatud koormusetasakaalustusega IP-aadress ja Azure laadi koormusetasakaalustusteenuse suunab Sissetuleva liikluse ainult esmane server koopia määramine.

Saate ILB tugi SQL serveri AlwaysOn (kuulajale) lõpp-punktid. Nüüd on kuulajale hõlbustusfunktsioonide kontroll ja saate valida koormusetasakaalustusega IP-aadressi teatud alamvõrgu virtuaalse võrku (VNet).

Kasutades ILB kuulajale, SQL serveri lõpp-punkti (nt serveri = tcp:ListenerName, 1433; andmebaasi = DatabaseName) on kättesaadav ainult:

- Teenuste ja VMs Virtual samasse võrku
- Teenuste ja VMs ühendatud kohapealse võrgu kaudu
- Teenuste ja VMs kaudu ühendatud VNets

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Joonis 1 - SQL-i AlwaysOn konfigureeritud Interneti-ühendusega laadi koormusetasakaalustusteenuse

## <a name="add-internal-load-balancer-to-the-service"></a>Sisemise laadi koormusetasakaalustusteenuse teenuse lisamine

1. Järgmises näites configure Virtual võrk, mis sisaldab alamvõrku, mis on nimega "Alamvõrgu-1":

        Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc

2. Laadi tasakaalustatud lõpp-punktid ILB iga VM lisamine

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –
        DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Ülaltoodud näites olete 2 VM nimega "sqlsvc1" ja "sqlsvc2" töötab pilveteenuses teenuse "Sqlsvc". Pärast "DirectServerReturn" koos selle ILB loomist saate lisada koormus tasakaalustatud lõpp-punktid ILB lubama SQL-i konfigureerimine kuulajatele kättesaadavus rühmade jaoks.

SQL-i AlwaysOn kohta leiate lisateavet teemast [Azure kättesaadavusrühma jaoks on sisemine laadi koormusetasakaalustusteenuse konfigureerimine](../virtual-machines/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Vt ka

[Töö alustamine Interneti vastastikuste laadi koormusetasakaalustusteenuse konfigureerimine](load-balancer-get-started-internet-arm-ps.md)

[Alustamine on sisemine laadi koormusetasakaalustusteenuse konfigureerimine](load-balancer-get-started-ilb-arm-ps.md)

[Laadi koormusetasakaalustusteenuse jaotuse režiimi konfigureerimine](load-balancer-distribution-mode.md)

[Oma laadi koormusetasakaalustusteenuse jõudeolekus TCP ajalõpp sätete konfigureerimine](load-balancer-tcp-idle-timeout.md)
