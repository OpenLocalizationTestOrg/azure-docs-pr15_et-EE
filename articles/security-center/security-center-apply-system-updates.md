<properties
   pageTitle="Rakendada süsteemi uuendused Azure'i turbekeskus | Microsoft Azure'i"
   description="Selle dokumendi näitab, kuidas rakendada Azure'i turbekeskus **süsteemi värskendused** ja **taaskäivitage pärast süsteemi värskendused**."
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

# <a name="apply-system-updates-in-azure-security-center"></a>Azure'i turbekeskus süsteemi uuendused rakendamine

Azure'i turbekeskus jälgib iga päev Windows ja Linux virtuaalmasinates (VM) puuduvate operatsioonisüsteemi uuendused. Turbekeskus toob loendi saadaval turbe- ja kriitilised värskendused Windows Update või Windows Server Update Services (WSUS), olenevalt sellest, milline teenus on konfigureeritud Windows VM.  Turbekeskus kontrollib ka uusimaid värskendusi Linuxi jaoks. Kui teie VM pole süsteemi värskenduse, turbekeskus soovitab süsteemi värskenduste rakendamist

> [AZURE.NOTE] Selle dokumendi tutvustab teenus on näide juurutamise abil.  See ei ole samm-sammult juhendi.

## <a name="implement-the-recommendation"></a>Soovituse

1. **Soovitused** tera, valige **Rakenda süsteemi värskendused**.
![Süsteemi värskenduste rakendamine][1]

2. **Värskenduste rakendamine süsteemi** tera avaneb loendit kõigist VMs puuduvad süsteemi värskendused. Valige VM.
![Valige VM][2]

3. Tera avab, kuvab selle VM jaoks puuduvad värskendused. Valige süsteem värskendus. Selles näites vaatame valige KB3156016.
![Puuduvate turbevärskendusi][3]

4. Järgige puuduvad värskenduse tera **Turvalisus värskendada** .
![Värskendus][4]

## <a name="reboot-after-system-updates"></a>Taaskäivitage pärast süsteemi värskendused

5. Naaske **soovitused** tera. Uue kirje on loodud pärast teie rakendatud süsteemi värskendused, nimetatakse **taaskäivitage pärast süsteemi värskendused**. Siit saate teada, peate taaskäivitage süsteem värskenduste rakendamine protsessi lõpuleviimiseks VM.
![Taaskäivitage pärast süsteemi värskendused][5]

6. Valige **taaskäivitage pärast süsteemi värskendused**. Avatakse **taaskäivitus on ootel süsteemi värskenduste lõpuleviimiseks** blade loendit kõigist VMs, mida peate taaskäivitama Rakenda süsteemi värskendused lõpule.
![Taaskäivitage ootel][6]

Taaskäivitage VM Azure lõpule viia.

## <a name="see-also"></a>Vt ka

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Azure'i turbekeskus soovitused haldamine turvalisuse kohta](security-center-recommendations.md) – siit saate teada, kuidas soovitused aitab teil kaitsta oma Azure ressursse.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](security-center-managing-and-responding-alerts.md) --saate teada, kuidas hallata ja turbeteatised vastata.
- [Jälgimis partnerite lahenduste ja Azure turbekeskus](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveebi](http://blogs.msdn.com/b/azuresecurity/) --Otsi ajaveebi postituste Azure Turve ja nõuetele vastavuse kohta.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
