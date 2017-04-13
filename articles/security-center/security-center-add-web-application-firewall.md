<properties
   pageTitle="Azure'i turbekeskuse tulemüüri web rakenduse lisamine | Microsoft Azure'i"
   description="Selle dokumendi näitab, kuidas rakendada Azure'i turbekeskuse **tulemüüri web rakenduste lisamine** ja **Finalize kaitse**."
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

# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Azure'i turbekeskuse tulemüüri web rakenduse lisamine

Azure'i turbekeskus soovitada lisamine oma veebirakenduste tagamiseks mõnelt Microsofti partnerilt web rakenduse tulemüüri (WAF). Selle dokumendi juhatab teid läbi näide sellest, kuidas seda teha.

> [AZURE.NOTE] Selle dokumendi tutvustab teenus on näide juurutamise abil.  See ei ole samm-sammult juhendi.

## <a name="implement-the-recommendation"></a>Soovituse

1. Valige **soovitused** labale **Secure veebirakenduse web rakenduse tulemüüri kaudu**.
![Turvaline web rakendus][1]

2. Valige veebirakenduse labale **Secure web rakenduse tulemüüri oma veebirakendusi** . **Lisa Web rakenduse tulemüüri** tera avaneb.
![Tulemüüri web rakenduse lisamine][2]
3. Soovi korral saate kasutada mõne olemasoleva web rakenduse tulemüüri, kui see on saadaval või saate luua uue. Selles näites on olemasolevaid WAFs pole saadaval nii loome uue WAF.

4. Uue WAF loomiseks valige lahenduse integreeritud partnerite loendist. Selles näites me valib **Barracuda Web rakenduse tulemüüri**.
5. **Barracuda Web rakenduse tulemüüri** tera avaneb teile pakkuda partneri lahenduse teavet. Valige **Loo** teabe tera.
![Tulemüüri teabe blade][3]

6. **Uus Web rakenduse tulemüüri** tera avaneb, kus saate **VM konfiguratsiooni** toimingute ja **WAF**teavet. Valige **VM konfigureerimine**.

7. **VM konfiguratsiooni** tera saab sisestada tööasendisse virtuaalse masina, mis töötab WAF vajalik teave.
![VM konfigureerimine][4]
8. **Uus Web rakenduse tulemüüri** tera naasmiseks valige **WAF teavet**. **WAF teabe** tera saate konfigureerida WAF ise. Juhis 7 võimaldab virtuaalse masina WAF käivitavad ja samm 8 võimaldab ettevalmistamise WAF ise konfigureerida.

## <a name="finalize-application-protection"></a>Kaitse viimistlemine

1. Naaske **soovitused** tera. Kui olete loonud WAF, nimetatakse **Finalize kaitse**on loodud uue kirje. Siit saate teada, peate tegelikult juhtmestik WAF Azure virtuaalse võrgustikus häälestamine nii, et see saate kaitsta rakendus protsessi lõpuleviimiseks.
![Kaitse viimistlemine][5]

2. Valige **Finalize kaitse**. Avatakse uus laba. Näete, et on veebirakendus, mis peab olema selle liikluse ümber.
3. Valige veebirakenduse. Tera avaneb, mis annab teile juhised selle veebirakenduse tulemüüri seadistus lõpuleviimine. Juhiseid ja valige **piira liikluse**. Turbekeskus tuleb teha siis üles-kaabeldus teie eest.
![Piira liikluse][6]

> [AZURE.NOTE] Mitme veebirakenduste turbekeskus saate kaitsta, lisades oma olemasoleva WAF juurutuste need rakendused. WAF seadmed (ressursihaldur juurutamise mudeli abil loodud) peab töötama eraldi virtuaalse võrku. WAF seadmed (klassikaline juurutamise mudeli abil loodud) on piiratud turberühma võrgu kaudu. See tugi laiendatakse tulevikus täielikult kohandatud juurutusega on WAF seadme (klassikaline). Lisateavet selle [klassikaline ja ressursihaldur juurutamise mudelite](../azure-classic-rm.md) Azure ressursse.

Logide kaudu selle WAF on nüüd täielikult integreeritud. Turbekeskus saate alustada automaatselt kogumiseks ja analüüsimiseks logid nii, et teile oluliste Turbeteatiste saab kuvada.

## <a name="see-also"></a>Vt ka

Selle dokumendi ilmnes saate kuidas rakendada turbekeskus soovitust "Add veebirakenduse." Web rakenduse tulemüüri konfigureerimise kohta lisateabe saamiseks vaadake järgmist:

- [Rakenduse teenuse keskkonnas Web rakenduse tulemüüri (WAF) konfigureerimine](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus koosolekukutsetele](security-center-managing-and-responding-alerts.md) --saate teada, kuidas hallata ja turbeteatised vastata.
- [Azure'i turbekeskus soovitused haldamine turvalisuse kohta](security-center-recommendations.md) – siit saate teada, kuidas soovitused aitab teil kaitsta oma Azure ressursse.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveebi](http://blogs.msdn.com/b/azuresecurity/) --Otsi ajaveebi postituste Azure Turve ja nõuetele vastavuse kohta.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
