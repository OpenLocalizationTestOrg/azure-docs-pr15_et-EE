<properties
   pageTitle="Azure'i turbekeskus ja Azure SQL-andmebaasi teenuse | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas turbekeskus aitab teil secure oma andmebaasi Azure SQL-andmebaasis."
   services="sql-database"
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
   ms.date="10/18/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-and-azure-sql-database-service"></a>Azure'i turbekeskus ja Azure SQL-andmebaas

[Azure'i turbekeskus](https://azure.microsoft.com/documentation/services/security-center/) aitab vältida, avastada ja ohtude vastata. Integreeritud turvalisus jälgimine ja rühmapoliitika haldus pakub oma Azure tellimustes, aitab tuvastada ohud, mis võivad muidu märkamata ja töötab laialdane ökosüsteemi turvalisus lahendusi.

Selles artiklis kirjeldatakse, kuidas turbekeskus aitab teil secure oma andmebaasi Azure SQL-andmebaasis.

## <a name="why-use-security-center"></a>Miks kasutada turbekeskus?

Turbekeskus aitab teil kaitsta SQL-andmebaasis andmete esitada nähtavus turvalisus serverid ja andmebaasid. Turbekeskus, saate teha järgmist.

- SQL-andmebaasi krüptimine ja auditeerimine määratleda.
- SQL-andmebaasi ressursid jälgida kõiki oma tellimustes.
- Kiiresti tuvastada ja migreerimisel ning probleemide lahendamisel turvalisus.
- Teatiste [Azure'i SQL-andmebaasi ohtude tuvastamise](../sql-database/sql-database-threat-detection-get-started.md)integreerida.

Lisaks aitab kaitsta teie SQL-andmebaasi ressursse, turbekeskus annab turvalisus jälgimise ja halduse Azure'i virtuaalmasinates, pilveteenustega, rakenduse teenused, virtuaalse võrgu ja. Lisateavet turbekeskus [siin](security-center-intro.md).

## <a name="prerequisites"></a>Eeltingimused

Alustamine turbekeskus, peab teil olema Microsoft Azure'i tellimust. Teie tellimus on lubatud turbekeskus tasuta astme. Security Center tasuta ja Standard astme kohta leiate lisateavet teemast [Turvalisus Center hinnad](https://azure.microsoft.com/pricing/details/security-center/).

Turbekeskus toetab Rollipõhine juurdepääsu. Rollipõhine juurdepääsu reguleerimine (RBAC) Azure kohta leiate lisateavet teemast [Azure Active Directory rolli vastavalt juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md). Turvalisus Center KKK annab teavet [Kuidas Security Center käsitletakse õigused](security-center-faq.md#how-are-permissions-handled-in-azure-security-center).

## <a name="access-security-center"></a>Turvalisus Hõlbustuskeskus

Pääsete turbekeskus [Azure portaali](https://azure.microsoft.com/features/azure-portal/). [Logige sisse portaali](https://portal.azure.com/) ja **turbekeskuse suvandi**valimine.

![Turbekeskus suvand][1]

Avaneb tera **Turbekeskus** .
![Turbekeskus blade][2]

## <a name="set-security-policy"></a>Poliitika määramine

Turvalisuse poliitika määratleb juhtelemendid, mis on soovitatav ressursid määratud tellimuse või ressursirühma. Turbekeskus, saate määratleda oma tellimuste või ressursside rühmad vastavalt oma ettevõtte turvalisus vajadustele ja rakenduste tüübi või tundlikkust iga tellimuse andmed.

Saate seada poliitika soovitused auditeerimine SQL-i ja SQL-i läbipaistev andmete krüptimine (TDE) kuvamiseks.

- **SQL-i auditeerimine ja ohtude tuvastamise**sisselülitamisel soovitab turbekeskus, et Azure'i andmebaasile pääsevad juurde auditeerimine lubatud nõuetele vastavuse tagamiseks Täpsemad tuvastamise ja uurimise eesmärgil.
- Kui lülitate **SQL-i läbipaistvaid andmete krüptimist**, turbekeskus soovitab selle krüptimist ja ülejäänud olema lubatud oma Azure'i SQL-andmebaasi, seotud varukoopiate ja tehingulogi faile.

Turvalisuse poliitika, valige turbekeskus enne **poliitika** paani. Enne **turbepoliitika** , valige tellimus, millele soovite lubada turbepoliitika. Valige **vältimise poliitika** ja turvalisuse soovitusi, mida soovite kasutada selle tellimuse **sisse** lülitada.
![Turbepoliitika][3]

Lisateavet leiate teemast [turbepoliitikate seadmine](security-center-policies.md).

## <a name="manage-security-recommendation"></a>Turvalisuse soovitus haldamine

Turbekeskus analüüsib perioodiliselt teie Azure turvalisus seisundi. Kui turbekeskus tuvastab võimalikud turvahaavatavusi, loob see soovitusi. Soovitused juhatavad teid protsessi konfigureerimine vajalik juhtelement.

Kui määrate turvalisuse poliitika, turbekeskus analüüsib oma ressursse, mis tähistavad võimaliku turvalisus olek. Soovitused kuvatakse tabelina, kus iga rida tähistab ühe kindla soovituse. Kasutage järgmises tabelis, et aidata teil mõista saadaval soovitused Azure'i SQL-andmebaasi ja mida iga soovitus ei, kui rakendate selle viitena. Valige soovituse suunab teid artikkel, mis selgitab, kuidas rakendada soovitust Security Center.

| Soovitused | Kirjeldus |
| ----- | ----- |
| [Valemiauditi ja ohtu tuvastamine SQL-i serverite lubamine](security-center-enable-auditing-on-sql-servers.md) | Soovitab sisselülitamine auditeerimine ja ohtu tuvastamine SQL-andmebaasi server. (Ainult SQL-andmebaasi teenus. Ei sisalda Microsoft SQL Server töötab teie virtuaalmasinates.) |
| [Valemiauditi ja ohtu tuvastamine SQL andmebaase lubamine](security-center-enable-auditing-on-sql-databases.md) | Soovitab sisselülitamine auditeerimine ja ohtu tuvastamine SQL-andmebaasi andmebaaside jaoks. (Ainult SQL-andmebaasi teenus. Ei sisalda Microsoft SQL Server töötab teie virtuaalmasinates.) |
| [Läbipaistva andmete krüptimine](security-center-enable-transparent-data-encryption.md) | Soovitab krüptimise SQL-i andmebaaside jaoks lubate. (Ainult SQL-andmebaasi teenus.) |

Soovitused oma Azure ressursse vaatamiseks valige turbekeskus enne paani **soovitusi** . **Soovitused** enne, valige soovituse üksikasjade kuvamiseks. Selles näites vaatame valige **Luba auditi ja ohtude tuvastamise SQL-i serverites**.

![Soovitused][4]

Nagu allpool näha, turbekeskus kuvatakse SQL-i serverid kus auditeerimine ja ohtu tuvastamine on lubatud. Kui lülitate auditeerimine, saate konfigureerida ohtude tuvastamise sätete ja e-posti sätete turvalisus teatiste saamiseks. Ohtude tuvastamise annab märku, kui tuvastab, et Anomaalne andmebaasi tegevusi, mis näitavad ohtude andmebaasi. Armatuurlaua turbekeskus kuvatakse teatisi.
![Auditi- ja ohtu tuvastamine][5]

Järgige [Alustamine SQL-i andmebaasi ohtude tuvastamise](../sql-database/sql-database-threat-detection-get-started.md) sisse lülitada ja konfigureerimiseks ohtude tuvastamise ja konfigureerimiseks kirju, mis saadetakse Turbeteatiste avastamisel Anomaalne tegevuste loendit.

Soovitused kohta leiate lisateavet teemast [soovitused haldamine turvalisuse kohta](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Kuvari turvalisus seisund

Kui olete lubanud [turbepoliitikate](security-center-policies.md) ressursid on tellimus, analüüsib turbekeskus oma ressursse, mis tähistavad võimaliku turvalisus.  Saate vaadata oma turvalisus seisundi **ressursside turvalisus seisund** paani. Paani **ressursside turvalisus seisundi** **andmete** klõpsamisel avaneb **Andmete ressursid** tera SQL-i soovitused küsimustega Auditeerimispoliitika ja läbipaistev andmete krüptimine pole lubatud. Samuti on andmebaasi üldine seisundioleku soovitused.
![Ressursside turvalisus seisund][6]

Lisateavet leiate jaotisest [Turvalisus seisundi jälgimine](security-center-monitoring.md).

## <a name="manage-and-respond-to-security-alerts"></a>Haldamine ja turbeteatised vastamine

Turbekeskus automaatselt kogub, analüüsib ja ühendab logiandmed [Azure SQL-i ohtude tuvastamise](../sql-database/sql-database-threat-detection-get-started.md), samuti muud Azure ressursid tuvasta ohud ja vähendamiseks vale-positiivsed. Tähtsuse Turbeteatiste loend kuvatakse turbekeskus koos kiiresti uurida probleemi ja kuidas migreerimisel ning probleemide lahendamisel rünnaku soovitused vajaliku teabe.

Teatiste vaatamiseks valige **Turbeteatiste** paan turbekeskus enne. **Turbeteatiste** enne, valige teatise Lisateavet sündmuste kohta, mis vallandanud teatise ja mida, kui mis tahes, peate tegema, et migreerimisel ning probleemide lahendamisel rünnaku juhiseid. Selles näites vaatame valige **võimalike SQL süst**.
![Turbeteatiste][7]

Nagu allpool näha, turbekeskus pakub täiendavaid üksikasju, mis pakuvad ülevaate, mis käivitab teatise, target ressurss, kui see on rakendatav allika IP-aadress ja soovitusi migreerimisel ning probleemide lahendamisel.
![Võimalikud SQL süst][8]

Lisateavet leiate teemast [haldamine ja turbeteatised vastuvõtmist](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Järgmised sammud

- [Turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Turvalisuse plaanimine ja toimingute keskjuhik](security-center-planning-and-operations-guide.md) - jälgi komplekt, toimingud ja ülesanded optimeerida turbekeskus kasutamise oma asutuse turbenõuetele ja cloud juhtimine mudelit põhjal.
- [Turvalisus Center andmete turvalisuse](security-center-data-security.md) – siit saate teada, kuidas turbekeskus kogub ja töötleb oma Azure ressursse, sh konfiguratsiooniteavet, metaandmete, sündmuselogide, krahh gigabaiti ja kohta.
- [Töötlemise turvalisus juhtumite](security-center-incident.md) – saate teada, kuidas kasutada turvalisus teatiste võimalus turbekeskus aidata teil töötlemise turvalisus juhtumite.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
