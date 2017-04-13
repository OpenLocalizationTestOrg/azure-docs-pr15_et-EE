<properties
   pageTitle="Luba VM Agent Azure turbekeskus | Microsoft Azure'i"
   description="Selle dokumendi näidatakse, kuidas rakendada Azure'i turbekeskus soovitust **Lubada VM Agent**."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="enable-vm-agent-in-azure-security-center"></a>Luba VM Agent Azure Security Center

VM Agent peab olema installitud virtuaalmasinates (VM) kohta, et [lubada andmete kogumine](security-center-enable-data-collection.md).  Azure'i turbekeskus võimaldab teil näha, milline VMs nõua VM Agent ja soovitame teil lubada VM Agent need sisse.

Vaikimisi installitakse VM Agent vms Azure'i turuplatsilt juurutatud. Artikli [VM Agent ja laiendid – osa 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) teave VM agendi installimise kohta.


> [AZURE.NOTE] Selle dokumendi tutvustab teenus on näide juurutamise abil. See ei ole samm-sammult juhendi.

## <a name="implement-the-recommendation"></a>Soovituse

1. **Soovitused blade**, valige **Luba VM Agent**.
![Luba VM Agent][1]

2. Avatakse tera **VM Agent on puudu või ei reageeri**. See blade loetletud VMs, mis nõuavad VM Agent. Enne VM agent installimiseks järgige juhiseid.
![VM Agent on puudu][2]

## <a name="see-also"></a>Vt ka

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md)– saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Turvalisus soovitused Azure'i turbekeskus haldamine](security-center-recommendations.md)– siit saate teada, kuidas soovitused aitab teil kaitsta oma Azure ressursse.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md)– saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](security-center-managing-and-responding-alerts.md)--saate teada, kuidas hallata ja turbeteatised vastata.
- [Jälgimis partnerite lahenduste ja Azure turbekeskus](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md)– Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveebi](http://blogs.msdn.com/b/azuresecurity/)--Azure turvalisus uudiseid ja teavet saada.

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
