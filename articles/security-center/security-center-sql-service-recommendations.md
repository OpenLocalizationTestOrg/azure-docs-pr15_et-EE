<properties
   pageTitle="Azure SQL-i teenuse Azure turbekeskus kaitsmine | Microsoft Azure'i"
   description="Selle dokumendi aadressid soovitused Azure'i turbekeskus, mis aitavad teil kaitsta Azure SQL-i teenuse ja vastavalt turbepoliitikate jääda."
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
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-azure-sql-service-in-azure-security-center"></a>Azure SQL-i teenuse Azure turbekeskus kaitsmine

Azure'i turbekeskus analüüsib oma Azure turvalisus seisundi. Kui turbekeskus tuvastab võimalikud turvahaavatavusi, loob see soovitusi, mis juhendab teid protsessi konfigureerimine vajalik juhtelement.  Soovitused Azure'i ressursi tüüpi: networking, SQL-i ja rakenduste virtuaalmasinates (VMs).

Selles artiklis käsitletakse soovitusi, mis rakenduvad Azure SQL-i teenus.  Azure SQL-i soovitused teeninduskeskuse ümber lubamisega auditeerimine Azure SQL-i serverid ja andmebaasid ja lubada krüptimise SQL-i andmebaaside jaoks.  Kasutage järgmises tabelis, et aidata teil mõista Saadaolevad SQL-i teenuse soovitused ja mida igat teha, kui rakendate selle viitena.

## <a name="available-sql-service-recommendations"></a>Saadaolevad SQL-i teenuse soovitused

|Soovitused|Kirjeldus|
|-----|-----|
|[Luba auditi SQL server](security-center-enable-auditing-on-sql-servers.md)|Soovitab lülitada auditeerimine Azure SQL-i serverite (Azure SQL-i teenuse; ei kaasata ainult teie virtuaalmasinates töötab SQL-i).|
|[Luba auditi SQL-i andmebaas](security-center-enable-auditing-on-sql-databases.md)|Soovitab lülitada auditeerimine Azure SQL-i andmebaasid (Azure SQL-i teenuse; ei kaasata ainult teie virtuaalmasinates töötab SQL-i).|
|[Läbipaistva andmete krüptimine SQL andmebaase lubamine](security-center-enable-transparent-data-encryption.md)|Soovitab, et lubate krüptimise SQL-i andmebaasid (ainult Azure SQL-i teenus).|

## <a name="see-also"></a>Vt ka

Soovitusi, mida rakendada muid Azure ressursside kohta lisateabe saamiseks vaadake järgmist:

- [Azure'i Security Center oma virtuaalmasinates kaitsmine](security-center-virtual-machine-recommendations.md)
- [Azure'i turbekeskus rakendused kaitsmine](security-center-application-recommendations.md)
- [Võrgu Azure'i turbekeskus kaitsmine](security-center-network-recommendations.md)

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](security-center-managing-and-responding-alerts.md) --saate teada, kuidas hallata ja turbeteatised vastata.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
