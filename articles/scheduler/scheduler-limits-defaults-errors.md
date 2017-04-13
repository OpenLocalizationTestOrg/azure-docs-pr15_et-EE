<properties
 pageTitle="Ajasti piirangud ja vaikesätted"
 description="Ajasti piirangud ja vaikesätted"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-limits-and-defaults"></a>Ajasti piirangud ja vaikesätted

## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Ajasti piirmäära, piirangud, vaikesätted ja pidurdab

[AZURE.INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>Päise x-ms-taotlus-id

Iga taotluse vastu toiminguajasti teenus tagastab vastuse päise**x-ms-taotlus-id**nimega. See päis sisaldab läbipaistmatu väärtus, mis tuvastab kordumatult kutse.

Kui taotlus nurjub süsteemne ja kinnitanud, et kutse on õigesti koostatud, võib selle väärtuse abil teatage tõrkest Microsoftile. Sisaldavad teie aruande väärtuse x-ms-taotlus-id, ligikaudne aeg, et taotlus, identifikaator tellimus, töö saidikogumi, ja/või töö ja toiming, mille kutse proovitakse tüüp.

## <a name="see-also"></a>Vt ka


 [Mis on ajasti?](scheduler-intro.md)

 [Azure'i Scheduler põhimõtet, terminoloogia ja üksuse hierarhia](scheduler-concepts-terms.md)

 [Azure'i portaalis Scheduler kasutamise alustamine](scheduler-get-started-portal.md)

 [Lepingud ja arvelduse Azure'i ajasti](scheduler-plans-billing.md)

 [Azure'i Scheduler REST API viide](https://msdn.microsoft.com/library/mt629143)

 [Azure'i Scheduler PowerShelli cmdlet-käskude viitamine.](scheduler-powershell-reference.md)

 [Azure'i Scheduler kõrge-saadavus ja usaldusväärsus](scheduler-high-availability-reliability.md)

 [Azure'i Scheduler väljaminevat autentimist](scheduler-outbound-authentication.md)
