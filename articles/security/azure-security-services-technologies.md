<properties
   pageTitle="Azure'i turvalisust ja tehnoloogiate | Microsoft Azure'i"
   description="See artikkel pakub Azure'i turvalisus ja tehnoloogia eelkoostatud loendit."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="yurid"/>

# <a name="azure-security-services-and-technologies"></a>Azure'i turvalisust ja tehnoloogiad

Küsitakse meie aruteludes praegustele ja tulevastele Azure klientidega, sageli "Kas teil on loendi kõigi turvalisus ja tehnoloogiaid, mis on Azure pakkuda?"
 
Me aru, et pilveteenuses teenuse pakkuja tehnilise suvandid võrdlemisel on kasulik on saadaval, saate minna sellise loendi alla süvitsi aeg on õige aeg.

Järgnevalt on meie algse peegeldav anda loendi. Aja jooksul selle loendi muutmine ja kasvada, nagu Azure'i teeb. Loendis on liigitatud ja ajaga kasvab ka kategooriat. Veenduge, et kontrollida selle lehe regulaarselt kursis meie teenuste seotud turvalisus ja tehnoloogia. 

## <a name="azure-security---general"></a>Azure'i turbe - üldine
- [Azure'i turbekeskus](https://azure.microsoft.com/documentation/services/security-center/)
- [Azure'i võtme Vault](https://azure.microsoft.com/documentation/services/key-vault/)
- [Azure'i ketta krüptimine](azure-security-disk-encryption.md)
- [Log Analytics](../log-analytics/log-analytics-overview.md)
- [Azure'i arendaja katse Labs](https://azure.microsoft.com/documentation/services/devtest-lab/)

## <a name="azure-storage-security"></a>Azure'i salvestusruumi turvalisus
- [Azure'i salvestusruumi teenuse krüptimist](../storage/storage-service-encryption.md)
- [StorSimple krüptitud hübriid salvestusruum](https://azure.microsoft.com/documentation/services/storsimple/)
- [Azure'i kliendipoolne krüptimist](../storage/storage-client-side-encryption.md)
- [Azure'i salvestusruumi ühiskasutusse Accessi allkirjad](../storage/storage-dotnet-shared-access-signature-part-1.md)
- [Azure'i salvestusruumi konto võtmed](../storage/storage-create-storage-account.md)
- [Azure'i faili jagab SMB 3.0 krüptimine](../storage/storage-dotnet-how-to-use-files.md)
- [Azure'i salvestusruumi Analytics](https://msdn.microsoft.com/library/hh343270.aspx)

## <a name="azure-database-security"></a>Azure'i andmebaasi turvalisus
- [SQL Azure'i tulemüüri](../sql-database/sql-database-firewall-configure.md)
- [SQL Azure'i lahtri taseme krüptimine](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/)
- [Azure SQL-i ühenduse krüptimine](../sql-database/sql-database-security-guidelines.md)
- [SQL Azure'i autentimine](../sql-database/sql-database-security-guidelines.md)
- [SQL Azure'i alati krüptimine](https://msdn.microsoft.com/library/mt163865.aspx)
- [Azure SQL-i veeru taseme krüptimine](https://msdn.microsoft.com/library/ms179331.aspx)
- [SQL Azure'i läbipaistvaid andmete krüptimine](https://msdn.microsoft.com/library/dn948096.aspx)
- [Azure'i SQL-andmebaasi kontrollimine](../sql-database/sql-database-auditing-get-started.md)

## <a name="azure-identity-and-access-management"></a>Azure identiteedi ja juurdepääsu juhtimine
- [Azure'i rollipõhise juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md)
- [Azure Active Directory](../active-directory/active-directory-whatis.md)
- [Azure Active Directory B2C](../active-directory-b2c/active-directory-b2c-get-started.md)
- [Azure Active Directory domeeniteenused](https://azure.microsoft.com/documentation/services/active-directory-ds/)
- [Azure'i Mitmikautentimise](../multi-factor-authentication/multi-factor-authentication.md)

## <a name="backup-and-disaster-recovery"></a>Varundus- ja katastroofiabi
- [Azure'i varukoopiad](https://azure.microsoft.com/documentation/services/backup/)
- [Azure'i saidi taastamine](https://azure.microsoft.com/documentation/services/site-recovery/)

## <a name="azure-networking"></a>Azure'i võrgunduse
- [Võrgu turberühmad](../virtual-network/virtual-networks-nsg.md)
- [Azure'i VPN-lüüsi](../vpn-gateway/vpn-gateway-about-vpngateways.md)
- [Azure'i rakenduse lüüsi](../application-gateway/application-gateway-introduction.md)
- [Azure'i laadi koormusetasakaalustusteenuse](../load-balancer/load-balancer-overview.md)
- [Azure'i ExpressRoute](../expressroute/expressroute-introduction.md)
- [Azure'i liikluse haldur](../traffic-manager/traffic-manager-overview.md)
- [Azure'i rakenduse puhverserver](../active-directory/active-directory-application-proxy-enable.md)
