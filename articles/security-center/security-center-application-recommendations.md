<properties
   pageTitle="Azure'i turbekeskus rakendused kaitsmine | Microsoft Azure'i"
   description="Selle dokumendi aadressid soovitused Azure'i turbekeskus, mis aitavad teil kaitsta oma Azure rakendused ja vastavalt turbepoliitikate jäämise."
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

# <a name="protecting-your-applications-in-azure-security-center"></a>Rakenduste Azure Security Center kaitsmine

Azure'i turbekeskus analüüsib oma Azure turvalisus seisundi. Kui turbekeskus tuvastab võimalikud turvahaavatavusi, loob see soovitusi, mis juhendab teid protsessi konfigureerimine vajalik juhtelement.  Soovitused Azure'i ressursi tüüpi: networking, SQL-i ja rakenduste virtuaalmasinates (VMs).

Selles artiklis käsitletakse soovitusi, mis kehtivad rakenduste.  Rakenduse soovitused keskenduvad web rakenduse tulemüüri kasutuselevõtt.  Kasutage järgmises tabelis, et aidata teil mõista saadaolevate rakenduste Soovitused ja mida igat teha, kui rakendate selle abimaterjalina.

## <a name="available-application-recommendations"></a>Saadaolevate rakenduste Soovitused

|Soovitused|Kirjeldus|
|-----|-----|
|[Tulemüüri web rakenduse lisamine](security-center-add-web-application-firewall.md)|Soovitab juurutada web rakenduse tulemüüri (WAF) web lõpp-punktid. Mitme veebirakenduste turbekeskus saate kaitsta, lisades oma olemasoleva WAF juurutuste need rakendused. WAF seadmed (ressursihaldur juurutamise mudeli abil loodud) peab töötama eraldi virtuaalse võrku. WAF seadmed (klassikaline juurutamise mudeli abil loodud) on piiratud turberühma võrgu kaudu. See tugi laiendatakse tulevikus täielikult kohandatud juurutusega on WAF seadme (klassikaline).|
|[Kaitse viimistlemine](security-center-add-web-application-firewall.md#finalize-application-protection)|Mõne WAF seadistamiseks peab liikluse ümber WAF seadmele. Pärast soovitusega täiendavad häälestamise vajalikud muudatused.|

## <a name="see-also"></a>Vt ka

Soovitusi, mida rakendada muid Azure ressursside kohta lisateabe saamiseks vaadake järgmist:

- [Azure'i Security Center oma virtuaalmasinates kaitsmine](security-center-virtual-machine-recommendations.md)
- [Võrgu Azure'i turbekeskus kaitsmine](security-center-network-recommendations.md)
- [Teie teenuse Azure SQL Azure'i turbekeskus kaitsmine](security-center-sql-service-recommendations.md)

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Haldamine ja turbeteatised Azure'i turbekeskus koosolekukutsetele](security-center-managing-and-responding-alerts.md) --saate teada, kuidas hallata ja turbeteatised vastata.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
