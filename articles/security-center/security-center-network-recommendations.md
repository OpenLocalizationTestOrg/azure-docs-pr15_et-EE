<properties
   pageTitle="Võrgu Azure'i turbekeskus kaitsmine | Microsoft Azure'i"
   description="Selle dokumendi aadressid soovitused Azure'i turbekeskus, mis aitavad teil kaitsta Azure võrgu ja vastavalt turbepoliitikate jääda."
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

# <a name="protecting-your-network-in-azure-security-center"></a>Võrgu Azure'i turbekeskus kaitsmine

Azure'i turbekeskus analüüsib oma Azure turvalisus seisundi. Kui turbekeskus tuvastab võimalikud turvahaavatavusi, loob see soovitusi, mis juhendab teid protsessi konfigureerimine vajalik juhtelement.  Soovitused Azure'i ressursi tüüpi: networking, SQL-i ja rakenduste virtuaalmasinates (VMs).

Selles artiklis käsitletakse soovitusi, mis kehtida teie võrgu.  Võrgu soovitused keskenduvad järgmise genereerimine tulemüürid, võrgu turberühmad, seadistamine sissetulev liiklus reeglid ja muu.  Kasutage järgmises tabelis aidata teil mõista võrgu soovitusi ja mida igat teha, kui rakendate selle abimaterjalina.

## <a name="available-network-recommendations"></a>Võrgu soovitused

|Soovitused|Kirjeldus|
|-----|-----|
|[Järgmise genereerimine tulemüüri lisamine](security-center-add-next-generation-firewall.md)|Soovitab lisada oma turvalisus kaitstud suurendamiseks mõnelt Microsofti partnerilt järgmise genereerimine tulemüüri (NGFW).|
|[Marsruutimiseks liiklus läbi NGFW ainult](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Soovitab võrgu turvalisus jaotises (NSG) reeglid, mis jõustada sissetulev liiklus teie VM kaudu oma NGFW konfigureerida.|
|[Luba võrgus turberühmad alamvõrku või virtuaalmasinates](security-center-enable-network-security-groups.md)|Soovitab, et lubada NSGs alamvõrku või VMs.|
|[Vastastikuste lõpp-punkti Interneti kaudu juurdepääsu piiramise](security-center-restrict-access-through-internet-facing-endpoints.md)|Soovitab sissetulev liiklus reeglite konfigureerimine NSGs.|

## <a name="see-also"></a>Vt ka

Soovitusi, mida rakendada muid Azure ressursside kohta lisateabe saamiseks vaadake järgmist:

- [Azure'i Security Center oma virtuaalmasinates kaitsmine](security-center-virtual-machine-recommendations.md)
- [Rakenduste Azure Security Center kaitsmine](security-center-application-recommendations.md)
- [Teie teenuse Azure SQL Azure'i turbekeskus kaitsmine](security-center-sql-service-recommendations.md)

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Haldamine ja turbeteatised Azure'i turbekeskus koosolekukutsetele](security-center-managing-and-responding-alerts.md) --saate teada, kuidas hallata ja turbeteatised vastata.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
