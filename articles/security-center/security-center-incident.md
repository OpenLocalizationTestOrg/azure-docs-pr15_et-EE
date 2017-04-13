<properties
   pageTitle="Töötlemise turvalisus juhtum Azure'i Security Center | Microsoft Azure'i"
   description="Selle dokumendi abil saate kasutada Azure turbekeskus võimaluste hallata turvalisus juhtumile."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="handling-security-incident-in-azure-security-center"></a>Töötlemise turvalisus juhtum Azure'i Security Center 
Vigade sõelumine ja turbeteatised uurida võib olla aeganõudev isegi kõige kogenud turvalisus ärianalüütikud ja paljudele on raske isegi teada, kust alustada. Teave erinevate [Turbeteatiste](security-center-managing-and-responding-alerts.md)vahel ühenduse [analytics](security-center-detection-capabilities.md) abil turbekeskus annavad teile ühte vaatesse on rünnak turunduskampaania ja kõik seotud teatised – saate kiiresti aru, milliseid toiminguid ründaja võttis ja ressursse, mis olid mõjutab.

Selles dokumendis kirjeldatakse turvalisuse teatiste võimalus kasutamine turbekeskus aidata teil töötlemise turvalisus juhtumite.


## <a name="what-is-a-security-incident"></a>Mis on turvalisus juhtum?

Security Center, on turvalisus juhtum mitmest kõik teatised, ressursi mustriga [tappa kettäärisega](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) joondada. [Turbeteatiste](security-center-managing-and-responding-alerts.md) paani ja tera kuvada juhtumid. Juhtum paljastavad loendit seotud teatised, mis võimaldab teil saada lisateavet iga kord.

## <a name="managing-security-incidents"></a>Turvalisus juhtumite haldamine

Saate vaadata oma praeguse turvalisus juhtumite vaadates paani turvalisus teatised. Azure'i portaali ja järgige juhiseid saate vaadata täpsemat teavet iga turvalisus juhtum.

1. Armatuurlaual turbekeskus kuvatakse paani **Turbeteatiste** .

    ![Turbeteatiste paani Security Center](./media/security-center-incident/security-center-incident-fig1.png)

2.  Klõpsake selle paani laiendada seda ja kui tuvastatakse turvalisus juhtum, kuvatakse see jaotises Turve teatiste graafik nagu allpool näidatud:

    ![Turvalisus juhtum](./media/security-center-incident/security-center-incident-fig2.png)

3.  Märgata turvalisus langeva kirjeldus on võrreldes teiste teatiste teistsuguse ikooni. Seda, et see juhtum üksikasjade kuvamiseks klõpsake nuppu.

    ![Turvalisus juhtum](./media/security-center-incident/security-center-incident-fig3.png)

4.  **Juhtum** blade kuvatakse rohkem andmeid turvalisus juhtum, mis sisaldab selle täielik kirjeldus selle tõsidust (mis sel juhul on kõrge), selle praeguses olekus (sel juhul on endiselt *aktiivne*, mis tähendab, et kasutajal pole tehtud toimingu *hülgamiseks* -seda saab teha õigus klõpsates juhtum **Turbeteatiste** tera) , ründasid ressursi (see juhul *VM1*), puhastamiseks juhiseid juhtum ja alumise paani peate teatised, mis sisaldusid selle juhtumi. Kui soovite saada lisateavet iga teatisega, avab selle ja teise blade klõpsake lihtsalt, nagu allpool näidatud:

    ![Turvalisus juhtum](./media/security-center-incident/security-center-incident-fig4.png)

Teave selle tera sõltub teatise. Lugege lisateavet selle kohta, kuidas neid teatiste haldamine [haldamine ja turbeteatised Azure'i turbekeskus koosolekukutsetele](security-center-managing-and-responding-alerts.md) . Mõned olulised kaalutluste kohta seda võimalust:

- Uus filter võimaldab teil kohandada vaadet, et ainult juhtum, teatiste ainult või mõlemad. 
- Sama teatise võib olla juhtum (vajadusel), ning samuti nähtav autonoomse hoiatusteate osana. 
- Sulgemisega juhtum ei lahenda seotud teatised.

## <a name="see-also"></a>Vt ka

Selles dokumendis soovite õpitut turbekeskus turvalisus langeva võimalus kasutamine. Lisateavet turbekeskus, leiate järgmistest:

- [Haldamine ja turbeteatised Azure'i turbekeskus reageerimine](security-center-managing-and-responding-alerts.md)
- [Azure'i turbekeskus tuvastamise võimalused](security-center-detection-capabilities.md)
- [Azure'i turbekeskus plaanimine ja toimingute juhend](security-center-planning-and-operations-guide.md)
- [Haldamine ja turbeteatised Azure'i turbekeskus reageerimine](security-center-managing-and-responding-alerts.md)
- [Azure'i turvalisus Center KKK](security-center-faq.md)– Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveebi](http://blogs.msdn.com/b/azuresecurity/)--Otsi ajaveebi postituste Azure Turve ja nõuetele vastavuse kohta.
