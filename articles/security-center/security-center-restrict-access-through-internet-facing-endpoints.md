<properties
   pageTitle="Azure'i turbekeskus Interneti-ühendusega lõpp-punktid kaudu juurdepääsu piiramise | Microsoft Azure'i"
   description="Selle dokumendi näidatakse, kuidas rakendada Azure'i turbekeskus soovitust **piira juurdepääsu Interneti suunatud lõpp-punkti**."
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

# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Azure'i turbekeskus Interneti-ühendusega lõpp-punktid kaudu juurdepääsu piiramise

Azure'i turbekeskus soovitab, et Interneti-ühendusega lõpp-punktid kaudu juurdepääsu piiramise kui mis tahes võrk turberühmad (NSGs) on sissetulevad reeglid, mis võimaldavad juurdepääsu "kõik" allika IP-aadress. Juurdepääs "mis tahes" avamine võib lubada ründajad juurde pääseda oma ressursse. Turbekeskus soovitab, et redigeerite neid sissetulevad reeglid juurdepääsu piiramiseks allika IP-aadressid, mis tegelikult vajate juurdepääsu.

Soovitusega luuakse – veebi porti, millel on "mis tahes" allikas.

> [AZURE.NOTE] Selle dokumendi tutvustab teenus on näide juurutamise abil. See ei ole samm-sammult juhendi.

## <a name="implement-the-recommendation"></a>Soovituse

1. Valige **soovitused blade** **piira juurdepääsu Interneti suunatud lõpp-punkti**.
![Vastastikuste lõpp-punkti Interneti kaudu juurdepääsu piiramise][1]

2. Avatakse tera **piira juurdepääsu Interneti suunatud lõpp-punkti**. See blade loendid sissetulevad reeglid, mis loomine võimalike turvalisus probleemi virtuaalmasinates (VMs). Valige VM.
![Valige VM][2]

3. **NSG** tera kuvab võrgu turberühma teabe, seotud sissetulevad reeglid ja seotud VM. Valige **Redigeeri sissetulevad reeglid** sissetulevast redigeerimist jätkata.
![Võrgu turberühma blade][3]

4. Valige enne **sissetuleva meili Turve reeglite** redigeerimiseks sissetuleva reegel. Selles näites vaatame valige **AllowWeb**.
![Sissetulev turvalisuse reeglid][4]

  Pange tähele, et võite valida ka **vaikereeglit** vaikereeglit ohjata kõik NSGs kogumi kuvamiseks. Vaikimisi reegleid ei saa kustutada, kuid, kuna need on määratud madalamat prioriteeti, saab ta tühistada reeglite loomist. Lugege lisateavet [vaikereeglit](../virtual-network/virtual-networks-nsg.md#default-rules).
![Vaikereeglit][5]

5. Enne **AllowWeb** , atribuutide redigeerimine Sissetulev reegel, et **Allikas** on IP-aadress või IP-aadresside blokeerimine. Sissetulev reegel atribuutide kohta leiate lisateavet teemast [NSG reeglid](../virtual-network/virtual-networks-nsg.md#nsg-rules).

  ![Sissetuleva reegli redigeerimine][6]

## <a name="see-also"></a>Vt ka

Selles artiklis ilmnes saate kuidas rakendada turbekeskus soovitust "Piira juurdepääsu kaudu Internet suunatud lõpp-punkti." NSGs ja reeglid lubamise kohta lisateabe saamiseks vt järgmist:

- [Mis on võrgu turvalisus jaotises (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Kuidas hallata NSGs Azure'i portaalis](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md)– saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Turvalisus soovitused Azure'i turbekeskus haldamine](security-center-recommendations.md)– siit saate teada, kuidas soovitused aitab teil kaitsta oma Azure ressursse.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md)– saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus koosolekukutsetele](security-center-managing-and-responding-alerts.md)--saate teada, kuidas hallata ja turbeteatised vastata.
- [Jälgimis partnerite lahenduste ja Azure turbekeskus](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md)– Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveebi](http://blogs.msdn.com/b/azuresecurity/)--Azure turvalisus uudiseid ja teavet saada.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
