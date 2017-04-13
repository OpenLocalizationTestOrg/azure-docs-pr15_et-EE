<properties
   pageTitle="Lõpp-punkti kaitse seisundi teatised Azure'i turbekeskus lahendamine | Microsoft Azure'i"
   description="Selle dokumendi näidatakse, kuidas rakendada Azure'i turbekeskus soovitust **lahendamiseks Endpoint Protection seisund teatiste**."
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

# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Lõpp-punkti kaitse seisundi teatised Azure'i turbekeskus lahendamine

Azure'i turbekeskus soovitada lahendada tuvastati lõpp-punkti kaitse seisundi teatised.  Turbekeskus võimaldab teil näha, milline virtuaalmasinates (VM) on lõpp-punkti kaitse tõrkeid ja kui palju tõrkeid.

> [AZURE.NOTE] Selle dokumendi tutvustab teenus on näide juurutamise abil. See ei ole samm-sammult juhendi.

## <a name="implement-the-recommendation"></a>Soovituse

1. Valige **soovitused blade** **lahendamiseks Endpoint Protection seisund teatiste**.
![Endpoint protection seisund teatiste lahendamine][1]

2. Avatakse tera **Endpoint Protection tõrge** jaoks iga VM loendeid VMs tõrkeid ja ebaõnnestumiste arv. Valige loendist VM.
![Endpoint protection tõrge][2]

3. **Tõrgete loend** blade avab valitud VM, tõrgete loendit. Valige loendist Lisateavet tõrke. Avatakse tera valitud nurjumise kohta lisateabe saamiseks.
![Tõrgete loend][3]
  ![tõrge sündmuse][4]

## <a name="see-also"></a>Vt ka

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md)– saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Turvalisus soovitused Azure'i turbekeskus haldamine](security-center-recommendations.md)– siit saate teada, kuidas soovitused aitab teil kaitsta oma Azure ressursse.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md)– saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus koosolekukutsetele](security-center-managing-and-responding-alerts.md)--saate teada, kuidas hallata ja turbeteatised vastata.
- [Jälgimis partnerite lahenduste ja Azure turbekeskus](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md)– Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveebi](http://blogs.msdn.com/b/azuresecurity/)--Azure turvalisus uudiseid ja teavet saada.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
