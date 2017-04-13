<properties
   pageTitle="Migreerimisel ning probleemide lahendamisel OS nõrkade kohtade Azure'i Security Center | Microsoft Azure'i"
   description="Selle dokumendi näidatakse, kuidas rakendada Azure'i turbekeskus soovitust **migreerimisel ning probleemide lahendamisel OS nõrkade kohtade**."
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

# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Migreerimisel ning probleemide lahendamisel OS nõrkade kohtade Azure'i Security Center

Azure'i turbekeskus analüüsib iga päev virtuaalse masina (VM) operatsioonisüsteem (OS) konfiguratsioone, mis võiksid VM haavatavamaks rünnakutele ja soovitab konfiguratsioonimuudatuste tegeleda nende nõrkade. Jälgitava teatud kuvatakse kohta leiate lisateavet teemast [soovitatav konfiguratsioon reeglite loendi](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Turbekeskus soovitab nõrkade kohtade lahendada kui teie VM OS konfiguratsiooni ei sobi soovitatav konfiguratsioon reegleid.

> [AZURE.NOTE] Selle dokumendi tutvustab teenus on näide juurutamise abil.  See ei ole samm-sammult juhendi.

## <a name="implement-the-recommendation"></a>Soovituse

1. Valige **soovitused** labale **migreerimisel ning probleemide lahendamisel OS nõrkade kohtade**.
![Migreerimisel ning probleemide lahendamisel OS nõrkade kohtade][1]

    **Migreerimisel ning probleemide lahendamisel OS nõrkade kohtade** tera avaneb ja on loetletud teie VMs OS konfiguratsioone, mis vastavad soovitatav konfiguratsioon reegleid.  Jaoks iga VM tuvastab tera:

   - **Reeglite nurjus** --arvu reeglid, mis on VM OS nurjus.
   - **Viimane SKANNIMINE aega** – kuupäev ja kellaaeg, et turbekeskus skannitud viimase soovitud VM OS konfigureerimine.
   - **Riigi** --haavatavuse praegune olek:

        - Avatud: Haavatavuse ei ole arvesse veel
        - Poolelioleva: Praegu ei rakendata haavatavuse on, midagi on vaja teie poolt
        - Lahendatud: Haavatavuse oli juba adresseeritud (kui probleem on lahendatud, kirje on halli värvi)
  - **Raskusaste** --kõik nõrkade kohtade määratakse tõsidust madal, s.t haavatavuse kõrvaldada, kuid ei nõua kohest tähelepanu.

Valige VM. Höövlitera selle VM avab ja kuvab reeglid, mis ei ole.
   ![Konfiguratsiooni reeglid, mis ei ole][2]

Valige reegel. Selles näites võimaldab valida **parool peab vastama keerukuse nõuetele**. Tera avaneb, mis kirjeldab nurjunud reegli ja selle mõju. Vaadake üksikasjad üle ja kaaluge operatsioonisüsteemi konfiguratsioone rakendamise viisi.
  ![Nurjunud reegli kirjeldus][3]

  Kordumatud tunnused reeglid turbekeskus kasutab levinud konfiguratsiooni loendamine (CCE). See tera on esitatud järgmine teave:

  - NIMI--Reegli nimi
  - RASKUSASTE--CCE raskusaste väärtus kriitiline, oluline või Hoiatus
  - CCIED--Reegli CCE ainuidentifikaator
  - KIRJELDUS – Kirjeldust reegel
  - HAAVATAVUSE--Selgituse haavatavuse või kui reeglit pole rakendatud
  - MÕJU--Business mõju reegli rakendamisel
  - OODATAV väärtus--Väärtus õigesti, kui turbekeskus analüüsib selle reegliga konfiguratsioonile VM OS
  - REEGLI toiming--Reegli toiming kasutatavaid turbekeskus teie VM OS konfiguratsiooni selle reegliga analüüsi käigus
  - TEGELIK väärtus--Pärast selle reegliga konfiguratsioonist VM OS analüüsi tagastatakse väärtus
  - HINDAMISE tulemus –-analüüsi tulemus: edastama, Fail


## <a name="see-also"></a>Vt ka

Selles artiklis ilmnes saate kuidas rakendada turbekeskus soovitust "Remediate OS nõrkade kohtade." Saate vaadata konfiguratsiooni reeglikomplekti [siin](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Kordumatud tunnused reeglid turbekeskus kasutatakse CCE (levinud konfiguratsiooni loendamine). Külastage lisateabe saamiseks [CCE](http://cce.mitre.org) saidile.

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Azure'i turbekeskus soovitused haldamine turvalisuse kohta](security-center-recommendations.md) – siit saate teada, kuidas soovitused aitab teil kaitsta oma Azure ressursse.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](security-center-managing-and-responding-alerts.md) --saate teada, kuidas hallata ja turbeteatised vastata.
- [Jälgimis partnerite lahenduste ja Azure turbekeskus](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveebi](http://blogs.msdn.com/b/azuresecurity/) --Otsi ajaveebi postituste Azure Turve ja nõuetele vastavuse kohta.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
