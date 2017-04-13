<properties
   pageTitle="Endpoint Protection installige Azure'i turbekeskus | Microsoft Azure'i"
   description="Selle dokumendi näidatakse, kuidas rakendada Azure'i turbekeskus soovitust **Installida Endpoint Protection**."
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
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="install-endpoint-protection-in-azure-security-center"></a>Installige Azure'i turbekeskus Endpoint Protection

Azure'i turbekeskus soovitada, et olete ette programmis ründevaratõrje Azure'i virtuaalmasinates (VMs) Kui ründevaratõrje pole veel lubatud. See soovitus kehtib ainult Windows VMs.

> [AZURE.NOTE] Selle dokumendi tutvustab teenus on näide juurutamise abil.  See ei ole samm-sammult juhendi.

## <a name="implement-the-recommendation"></a>Soovituse

1. Valige **soovitused** tera, **Installige Endpoint Protection**.
![Valige installi Endpoint Protection][1]

2. **Endpoint Protection installida** tera avaneb VMs loendi kuvamiseks ilma ründevaratõrje lubatud. Valige loendist VMs, mida soovite installida ründevaratõrje ja klõpsake **VMs installimine**.
![Valige VMs installida Ründevaratõrje][2]

3. **Valige Endpoint Protection** tera avaneb, mis lubavad teil valida ründevaratõrje lahenduse, mida soovite kasutada. Selles näites vaatame valige **Microsofti ründevaratõrje**.
![Valige Endpoint Protection][3]

4. Ründevaratõrje lahenduse kohta lisateavet ei kuvata. Valige **Loo**.
![Ründevaratõrje lahenduse loomine][4]

5. Sisestage nõutav konfiguratsioon sätete **Lisamine laiend** enne ja seejärel klõpsake **nuppu OK**. Sätete konfigureerimise kohta leiate lisateavet teemast [vaike- ja kohandatud ründevaratõrje konfigureerimine](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[Microsofti ründevaratõrje](../azure-security-antimalware.md) on nüüd aktiivne valitud VMs.

## <a name="see-also"></a>Vt ka

Selles artiklis ilmnes saate kuidas rakendada turbekeskus soovitust "Installi Endpoint Protection." Azure programmis ründevaratõrje lubamise kohta lisateabe saamiseks vaadake järgmist:

- [Microsofti ründevaratõrje pilveteenustega ja Virtuaalmasinates](../azure-security-antimalware.md) – saate teada, kuidas Microsofti ründevaratõrje juurutamine.

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – siit saate teada, kuidas konfigureerida turbepoliitikate.
- [Azure'i turbekeskus soovitused haldamine turvalisuse kohta](security-center-recommendations.md) – siit saate teada, kuidas soovitused aitab teil kaitsta oma Azure ressursse.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus koosolekukutsetele](security-center-managing-and-responding-alerts.md) --saate teada, kuidas hallata ja turbeteatised vastata.
- [Jälgimis partnerite lahenduste ja Azure turbekeskus](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveebi](http://blogs.msdn.com/b/azuresecurity/) --Otsi ajaveebi postituste Azure Turve ja nõuetele vastavuse kohta.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
