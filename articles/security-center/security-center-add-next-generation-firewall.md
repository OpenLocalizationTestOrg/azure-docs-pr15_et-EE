<properties
   pageTitle="Lisada järgmise genereerimine tulemüüri Azure'i turbekeskus | Microsoft Azure'i"
   description="Selle dokumendi näitab, kuidas rakendada Azure'i turbekeskus **lisamine järgmise genereerimine tulemüür** ja **marsruutimiseks traffice NGFW ainult kaudu**."
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
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Azure'i turbekeskus järgmise genereerimine tulemüüri lisamine

Azure'i turbekeskus soovitada lisada oma turvalisus kaitstud suurendamiseks mõnelt Microsofti partnerilt järgmise genereerimine tulemüüri (NGFW). Selle dokumendi juhatab teid läbi näide sellest, kuidas seda teha.

> [AZURE.NOTE] Selle dokumendi tutvustab teenus on näide juurutamise abil.  See ei ole samm-sammult juhendi.

## <a name="implement-the-recommendation"></a>Soovituse

1. **Soovitused** tera, valige käsk **Lisa järgmise genereerimine tulemüüri**.
![Järgmise genereerimine tulemüüri lisamine][1]

2. Valige **Lisa järgmise genereerimine tulemüüri** labale lõpp.
![Valige lõpp][2]

3. Teine **Lisa järgmise genereerimine tulemüüri** tera avaneb. Soovi korral saate kasutada olemasolevat lahendust, kui see on saadaval või saate luua uue. Selles näites on olemasolevaid lahendusi pole saadaval nii loome uue NGFW.
![Looge uus järgmise genereerimine tulemüüri][3]

4. Uue NGFW loomiseks valige lahenduse integreeritud partnerite loendist. Selles näites me valib **Sisse punkti**.
![Valige järgmine genereerimine tulemüüri lahendus][4]

5. **Kontrolli punkti** tera avaneb teile pakkuda partneri lahenduse teavet. Valige **Loo** teabe tera.
![Tulemüüri teabe blade][5]

6. **Loo virtuaalse masina** tera avaneb. Siin saate sisestada tööasendisse virtuaalse masina (VM) käivitate selle NGFW vajalik teave. Järgige juhiseid ja sisestage nõutav teave NGFW. Valige OK, et rakendada.
![Virtuaalse masina käivitamiseks NGFW loomine][6]

## <a name="route-traffic-through-ngfw-only"></a>Marsruutimiseks liiklus läbi NGFW ainult

Naaske **soovitused** tera. Uue kirje on loodud pärast saate lisada mõne NGFW kaudu turbekeskus, nimetatakse **NGFW ainult liikluse marsruutimiseks**. Soovitusega luuakse ainult siis, kui olete installinud oma NGFW turbekeskus kaudu. Kui teil on Interneti-ühendusega lõpp-punktid, soovitada turbekeskus võrgu turberühma reeglid, mis jõustada sissetulev liiklus teie VM kaudu oma NGFW konfigureerida.

1. Valige **soovitused blade** **NGFW ainult liikluse marsruutimiseks**.
![Marsruutimiseks liiklus läbi NGFW ainult][7]

2. Avatakse tera **marsruutida liikluse kaudu NGFW ainult** mis loetleb VMs, mida te saate marsruutida liiklust. Valige loendist VM.
![Valige VM][8]

3. Höövlitera valitud VM avaneb, kus on kuvatud seotud sissetulevad reeglid. Kirjeldus annab teile rohkem teavet võimalikke edasiseid juhiseid. Valige **Redigeeri sissetulevad reeglid** sissetulevast redigeerimist jätkata. Eeldatakse, et **Allikas** on seatud **mis tahes** jaoks soovitud NGFW seotud Interneti-ühendusega lõpp-punktid. Sissetulev reegel atribuutide kohta leiate lisateavet teemast [NSG reeglid](../virtual-network/virtual-networks-nsg.md#nsg-rules).
![Konfigureerige reeglid piirata juurdepääsu][9]
![Redigeeri Sissetulev reegel][10]

## <a name="see-also"></a>Vt ka

Selle dokumendi ilmnes saate kuidas rakendada turbekeskus soovitust "Add järgmise genereerimine tulemüüri." NGFWs ja sisse punkti partnerite lahenduste kohta lisateabe saamiseks vaadake järgmist:

- [Tulemüüri edasi-genereerimine](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
- [Punkti vSEC kontrollimine](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – siit saate teada, kuidas konfigureerida turbepoliitikate.
- [Azure'i turbekeskus soovitused haldamine turvalisuse kohta](security-center-recommendations.md) – siit saate teada, kuidas soovitused aitab teil kaitsta oma Azure ressursse.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus koosolekukutsetele](security-center-managing-and-responding-alerts.md) --saate teada, kuidas hallata ja turbeteatised vastata.
- [Jälgimis partnerite lahenduste ja Azure turbekeskus](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveebi](http://blogs.msdn.com/b/azuresecurity/) --Otsi ajaveebi postituste Azure Turve ja nõuetele vastavuse kohta.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
