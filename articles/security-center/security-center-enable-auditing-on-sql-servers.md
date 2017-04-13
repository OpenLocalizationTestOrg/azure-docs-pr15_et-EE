<properties
   pageTitle="Luba auditi serverites SQL Azure'i turbekeskus | Microsoft Azure'i"
   description="Selle dokumendi näidatakse, kuidas rakendada **Luba auditi serverites SQL**Azure'i turbekeskus soovitust."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-auditing-on-sql-servers-in-azure-security-center"></a>Luba auditi serverites SQL Azure'i turbekeskus

Azure'i turbekeskus soovitab lülitada auditeerimine kõigile andmebaasidele Azure SQL-i serveritesse kui auditeerimine pole veel lubatud. Auditi aitavad teil säilitada nõuetele vastavuse, mõista andmebaasi tegevus ja saada aimu erinevused ja kõrvalekaldeid, mis võib viidata business probleemid või arvatav turvalisus rikkumised.

Kui olete sisse lülitanud auditeerimine saate konfigureerida ohtude tuvastamise sätete ja meilisõnumite vastuvõtmiseks Turbeteatiste. Ohtude tuvastamise tuvastab Anomaalne andmebaasi tegevuste ohtude andmebaasiga, mis näitab. See võimaldab teil tuvastada ja neile vastamine esinevad võimalike ohtudega.

See soovitus kehtib Azure SQL-i teenuse ainult; See ei sisalda SQL Server töötab teie virtuaalmasinates Azure'i taristu teenused (Azure'i IaaS).

> [AZURE.NOTE] Selle dokumendi tutvustab teenus on näide juurutamise abil.  See ei ole samm-sammult juhendi.

## <a name="implement-the-recommendation"></a>Soovituse

1. **Soovitused** tera, valige **Luba auditi SQL-i serverites**.  Avatakse tera **Luba auditi SQL-i serverites** .
![Luba auditi SQL-i serverid][1]

2. Valige SQL server lubamiseks klõpsake audit. Avatakse tera **Auditeerimine sätted** .
![Auditeerimissätted][2]
3. **Auditi sätted** enne, valige jaotises **auditeerimine** **ON** .
![Auditeerimissätted sisselülitamine][3]

4. Järgige [Alustamine SQL-andmebaasi auditi](../sql-database/sql-database-auditing-get-started.md) konfigureerimine salvestusruumi oma auditilogide talletamiseks. Selle tellimuse salvestusruumi andmekogumise on vaikimisi salvestusruumi konto.

5. Järgige [Alustamine SQL-i andmebaasi ohtude tuvastamise](../sql-database/sql-database-threat-detection-get-started.md) sisse lülitada ja konfigureerida ohtude tuvastamise ja konfigureerida kirju, mis saadetakse Turbeteatiste avastamisel Anomaalne tegevuste loendit.

## <a name="see-also"></a>Vt ka

Selles artiklis ilmnes saate kuidas rakendada turbekeskus soovitust "Luba auditi SQL-i serverid." SQL-andmebaasi turvalisemaks muutmise kohta lisateabe saamiseks vaadake järgmist:

- [SQL-andmebaasi turvamine](../sql-database/sql-database-security.md)

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Azure'i turbekeskus soovitused haldamine turvalisuse kohta](security-center-recommendations.md) – siit saate teada, kuidas soovitused aitab teil kaitsta oma Azure ressursse.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](security-center-managing-and-responding-alerts.md) --saate teada, kuidas hallata ja turbeteatised vastata.
- [Jälgimis partnerite lahenduste ja Azure turbekeskus](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveebi](http://blogs.msdn.com/b/azuresecurity/) --Azure turvalisus uudiseid ja teavet saada.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]:./media/security-center-enable-auditing-on-sql-server/enable-auditing.png
[3]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
