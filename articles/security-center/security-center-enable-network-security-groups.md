<properties
   pageTitle="Luba võrgus turberühmad Azure'i Security Center | Microsoft Azure'i"
   description="Selle dokumendi näidatakse, kuidas rakendada Azure'i turbekeskus soovitust **Lubada võrgu turberühmad**."
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

# <a name="enable-network-security-groups-in-azure-security-center"></a>Luba võrgus turberühmad Azure'i Security Center

Azure'i turbekeskus soovitada lubada võrgu turberühma (NSG), kui üks pole veel lubatud. NSGs sisaldavad lubada või keelata võrguliikluse VM eksemplaride virtuaalse võrgu juurdepääsu juhtimine loend (ACL) reeglite loendi. NSGs võib olla seotud alamvõrku või üksikud VM eksemplarid selle alamvõrgu sees. Mõne NSG on seostatud mõne alamvõrku, ACL reeglite rakendamine VM juhtudel selle alamvõrgu. Lisaks on üksikuid VM-liikluse võib olla piiratud veel mõne NSG otse, et selle VM seostamise kaudu. Lisateavet leiate teavet [mis on võrgu turvalisus jaotises (NSG)?](../virtual-network/virtual-networks-nsg.md)

Kui teil pole lubatud NSGs, turbekeskus esitab kahe soovitused teile: lubamine võrgu turberühmad alamvõrku ja lubada võrgu turvalisuse rühmad virtuaalmasinates. Saate valida taseme, alamvõrgu või VM NSGs rakendada.


> [AZURE.NOTE] Selle dokumendi tutvustab teenus on näide juurutamise abil.  See ei ole samm-sammult juhendi.

## <a name="implement-the-recommendation"></a>Soovituse

1. **Soovitused** tera, valige **Luba võrgu turberühmad** alamvõrku või virtuaalmasinates.
![Luba võrgus turberühmad][1]

2. Avatakse tera **Konfigureerimine puuduvad võrgu turberühmad** alamvõrku või virtuaalmasinates, sõltuvalt teie valitud soovitus. Valige soovitud alamvõrgu või virtuaalse masina konfigureerimiseks on NSG.

  ![Alamvõrgu NSG konfigureerimine][2]

  ![VM NSG konfigureerimine][3]
3. **Valige võrgu turberühma** enne valige mõne olemasoleva NSG või luua uue NSG.

  ![Valige võrgu turberühm][4]

Kui loote uue NSG, järgige [haldamine NSGs Azure'i portaalis](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) luua mõne NSG ja turvalisuse reeglid loonud.

## <a name="see-also"></a>Vt ka

Selles artiklis ilmnes saate kuidas rakendada alamvõrku või virtuaalmasinates turbekeskus soovitust "Lubamine võrgu turberühmad". NSGs lubamise kohta lisateabe saamiseks vaadake järgmist:

- [Mis on võrgu turvalisus jaotises (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Kuidas hallata NSGs Azure'i portaalis](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Azure'i turbekeskus soovitused haldamine turvalisuse kohta](security-center-recommendations.md) – siit saate teada, kuidas soovitused aitab teil kaitsta oma Azure ressursse.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus koosolekukutsetele](security-center-managing-and-responding-alerts.md) --saate teada, kuidas hallata ja turbeteatised vastata.
- [Jälgimis partnerite lahenduste ja Azure turbekeskus](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveebi](http://blogs.msdn.com/b/azuresecurity/) --Azure turvalisus uudiseid ja teavet saada.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
