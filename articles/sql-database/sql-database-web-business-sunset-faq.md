<properties
   pageTitle="Azure SQL-i andmebaasi Web ja Business Edition ajalise piirangu KKK | Microsoft Azure'i"
   description="Siit saate teada kui Azure SQL-i veebi ja andmebaaside pensionile ning te saate tutvuda funktsioonid ja funktsionaalsus uus teenus astme."
   services="sql-database"
   documentationCenter="na"
   authors="stevestein"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/08/2016"
   ms.author="sstein" />

# <a name="web-and-business-edition-sunset-faq"></a>Web ja Business Edition ajalise piirangu KKK

Nüüd on kasutuselt kõrvaldatud Azure SQL-i veebi ja andmebaasid. Basic, Standard, Premium ja elastne astme asendada pensionile veebi ja andmebaasid.

Teid aidata üleminekut veebi ja andmebaaside, SQL-andmebaasi teenuse soovitab on asjakohane taseme- ja jõudluse tase (hinnad taseme) iga andmebaasi vastavalt oma ajalooliste töökoormus.

**Kui soovite saada hinnad taseme soovitusi:**

- [Portaalis Azure SQL-i andmebaasi V12 versioonile](sql-database-upgrade-server-portal.md)
- [Versioonitäienduse SQL-i andmebaasi v12 PowerShelli abil](sql-database-upgrade-server-powershell.md)
- [Andmebaasi veebi- ja hinnakirjad taseme muutmine](sql-database-service-tier-advisor.md)



## <a name="why-does-the-azure-portal-show-my-web-and-business-edition-databases-as-retired"></a>Miks Azure portaali Kuva minu veebi ja väljaande andmebaaside kasutuselt kõrvaldatud?

Kuna veebi ja väljaande andmebaasid on kättesaadavad pärast mai 2015, märgib portaali kasutuselt kõrvaldatud veebi ja andmebaase. Endine sildi pakub meelde, et kõik andmebaasid veebi ja tuleks uuendada Standard-, põhi-või Premium. Olemasoleva veebi- ja andmebaaside versiooniks uus teenus astme kohta leiate üksikasjalikumat teavet teemast [Azure SQL-i andmebaasi V12 versioonile](sql-database-upgrade-server-portal.md).

## <a name="which-new-service-tier-is-the-best-choice-to-upgrade-my-existing-web-or-business-database-to"></a>Millised uue teenuse kiht on parim valik minu olemasoleva veebi- ja andmebaasi versioonile?

Teatud funktsioonide ja jõudluse nõuded rakenduse valimine uue teenuse taseme- ja jõudluse tasemele olemasoleva andmebaasi veebi- ja sõltub.

Kasutage hinnakirjad taseme soovitusi kirjeldatud kohale või üksikasjalikku teavet leiate [Azure'i SQL-i andmebaasi V12 versioonile](sql-database-upgrade-server-portal.md)aidata teil valida sobiv uue teenuse kiht.

## <a name="why-is-microsoft-introducing-new-service-tiers"></a>Miks on Microsoft kasutusele uue teenuse astme?

Klientide tagasiside põhjal Azure'i SQL-andmebaasi tutvustus uus teenus astme kliendid rohkem tuge relatsiooniandmebaasist töökoormus. Esitamisel prognoositavad jõudluse kõigi on raske kaalu selgituseks rakenduse nõudmistele kerge seitse tasemete on loodud uue astme. Lisaks uue astme pakub mitmesuguseid äri järjepidevus funktsioonid, tugevam sees SLA, suurem andmebaasi suurus vähem raha ja arvelduse parem kogemus.

## <a name="where-can-i-learn-more-about-the-new-service-tiers"></a>Kust saada lisateavet uus teenus astme?

Üksikasjalikku teavet uute teenuse astme ja jõudluse mudeli, vt [teenuse astme](sql-database-service-tiers.md). Üksikasjalik uus teenus astme hinnateavet, leiate teemast [SQL-andmebaasi hinnad](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="what-features-or-functionality-will-not-be-available-in-basic-standard-and-premium"></a>Milliseid funktsioone või funktsioonid ei ole saadaval, Basic, Standard ja Premium?

Funktsiooni ühendused pensionile veebi ja versioonid. Kliendid, kellel on vaja skaala-out nende andmebaaside soovitatakse selle asemel kasutada või [Azure'i SQL-andmebaasi](sql-database-elastic-scale-get-started.md), mis lihtsustab loomiseks ja haldamiseks rakendus kasutab sharding [elastne](sql-database-elastic-scale-get-started.md) Andmebaasiriistad migreerida. .Net-i kliendi teek võimaldab määratleda, kuidas andmed on vastendatud shards ja marsruudib OLTP taotluste vastav andmebaaside rakendused. Haldamise toiminguid, mis taaskonfigureerimine, kuidas andmeid jaotab shards toetamiseks mõni Azure pilveteenuse teenuse mall on kaasatud, et saate majutada oma Azure'i tellimus. Lisaks [elastne Andmebaasiriistad](sql-database-elastic-scale-get-started.md), Microsoft jätkuvalt luua ja avaldada [kohandatud sharding mustrite ja tavade juhised](https://msdn.microsoft.com/library/azure/dn764977.aspx) õpetused sügav kliendi kokkulepete põhjal. Uutele klientidele, kes vajavad skaala funktsioonid välja peaks väljamöllimine [elastne Andmebaasiriistad](sql-database-elastic-scale-get-started.md) ja/või ühendust Microsofti Support hinnata nende suvandid.

Microsoft muudab ka andmebaasi Kopeeri kogemus Premium andmebaasidega. ANDMEBAASI loomine varem premium andmebaasi piirmäär on piiratud... T-SQL loodud KOOPIANA tekstivormingus peatatud Premium andmebaasi ilma reserveeritud ressursse, mis on sama määra Business andmebaasi. Kui Premiumi kvoodi on nüüd vabalt veel olemas, peatatud Premium enam ei toetata. Andmebaasi koopiate luuakse nüüd sama väljaande ja jõudlusega allikana ja arve vastavalt sellele. Alandada kopeeritud andmebaaside muu teenuse taseme või jõudluse tasemele vähendada nende soovi korral saate valida kliendid. Peatatud Premium olemasolevaid andmebaase teisendatakse Business edition osana selles versioonis. On tõenäoline, kasutuselevõtu [Punkti õigel ajal taastada](sql-database-recovery-using-backups.md#point-in-time-restore) vähendab vajadust andmebaasi varundada.

## <a name="how-does-basic-standard-and-premium-improve-my-billing-experience"></a>Kuidas Basic, Standard, ja minu arvelduse kasutusvõimalusi parandada?

Tavaline, Standard, Premium Azure SQL-i andmebaasid on arve alusel ja teil on võimalus mastaapimiseks iga andmebaasi üles või alla. Teil on arve alusel kõrgeima taseme- ja jõudluse teenusetaseme valite iga fikseeritud määra. Lisaks jõudluse tasemeid (näide: Basic, S1 ja P2) on katki bill oleks lihtsam näha andmebaasi päeva/tunde, saate tehtud ühe kuu, iga taseme jaoks. Veebi ja andmebaaside jätkuvalt toimub abil andmebaasi üksust andmebaasi mahtu. Külastage [SQL-andmebaasi hinnad lehe](https://azure.microsoft.com/pricing/details/sql-database/) Lisateavet hinnad ja uus teenus astme erinevused.


## <a name="see-also"></a>Vt ka

[Azure'i SQL-andmebaas](https://azure.microsoft.com/documentation/services/sql-database/)

[Veebi ja Business hinnad](https://azure.microsoft.com/pricing/details/sql-database/web-business/)

[Teenuse astme](sql-database-service-tiers.md)

[Varasema versiooni täiendamine versiooniks SQL Azure'i andmebaasi V12](sql-database-upgrade-server-portal.md)
