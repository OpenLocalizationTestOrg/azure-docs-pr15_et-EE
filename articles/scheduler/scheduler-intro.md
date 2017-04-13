<properties
 pageTitle="Mis on Azure Scheduler? | Microsoft Azure'i"
 description="Azure'i Scheduler võimaldab deklaratiivse kirjeldatud toiminguid käivitamiseks pilveteenuses. See siis koosolekut kavandava ja need toimingud käivitatakse automaatselt."
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
 ms.topic="hero-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="what-is-azure-scheduler"></a>Mis on Azure Scheduler?

Azure'i Scheduler võimaldab deklaratiivse kirjeldatud toiminguid käivitamiseks pilveteenuses. See siis koosolekut kavandava ja need toimingud käivitatakse automaatselt.  Ajasti see [Azure portaali](scheduler-get-started-portal.md), koodi, [REST API -ga](https://msdn.microsoft.com/library/mt629143.aspx)või Azure PowerShelli abil.

Ajasti loob, säilitab ja käivitab ajastatud töö.  Ajasti majutada mis tahes töökoormus või mis tahes koodi käivitada. Ainult _käivitab_ kood mujal majutatud IT-Azure asutusesiseselt, või mõne muu teenusepakkuja juures. See käivitab HTTP, HTTPS, salvestusruumi järjekorda, teenuse siini järjekorda või teenuse siini teema kaudu.

Ajastatud koosolekut kavandava [tööde haldamine](scheduler-concepts-terms.md), hoiab töö täitmise tulemuste ühte saate vaadata ja joonistub ja usaldusväärselt koosolekut kavandava töökoormus käivitada. Azure'i WebJobs (osa veebirakenduste funktsiooni Azure'i rakendust Service) ja muude Azure'i plaanimine võimaluste kasutamine Scheduler taustal. [Ajasti REST API](https://msdn.microsoft.com/library/mt629143.aspx) aitab hallata järgmiste toimingute jaoks teatist. Sellisel kujul Scheduler toetab [keerukate ajakava ja Täpsem Korduvus](scheduler-advanced-complexity.md) hõlpsalt.

On mitu stsenaariumi, mis sobivad Scheduler kasutamist. Näiteks:

+ _Korduv rakenduse toimingud:_ Perioodiliselt andmete kogumine Twitteri kanalisse sisse.
+ _Igapäevane hooldus:_ Logide, varukoopiate ja muude hooldustoiminguid igapäevane kärpimine. Näiteks võite administraator 01:00:00 andmebaasi varundamine iga päev 9 järgmise kuu.

Scheduler võimaldab teil luua, värskendada, kustutada, vaadata ja hallata töö ja [töö saidikogumid](scheduler-concepts-terms.md) programmiliselt, skripte ja portaalis.

## <a name="see-also"></a>Vt ka

 [Azure'i Scheduler põhimõtet, terminoloogia ja üksuse hierarhia](scheduler-concepts-terms.md)

 [Azure'i portaalis Scheduler kasutamise alustamine](scheduler-get-started-portal.md)

 [Lepingud ja arvelduse Azure'i ajasti](scheduler-plans-billing.md)

 [Kuidas luua keerukate ajakavasid ja täpsemalt Korduvus Azure'i Scheduleri abil](scheduler-advanced-complexity.md)

 [Azure'i Scheduler REST API viide](https://msdn.microsoft.com/library/mt629143)

 [Azure'i Scheduler PowerShelli cmdlet-käskude viitamine.](scheduler-powershell-reference.md)

 [Azure'i Scheduler kõrge-saadavus ja usaldusväärsus](scheduler-high-availability-reliability.md)

 [Azure'i Scheduler piirangud, vaikesätted ja kuvatavad tõrkekoodid](scheduler-limits-defaults-errors.md)

 [Azure'i Scheduler väljaminevat autentimist](scheduler-outbound-authentication.md)
