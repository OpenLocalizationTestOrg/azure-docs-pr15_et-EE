<properties
   pageTitle="Azure'i turbekeskus ohtu aruanne | Microsoft Azure'i"
   description="Selle dokumendi aitab teil aruannete abil saate Azure'i turvalisus Center ohtu nutikad käigus leida rohkem teavet turvalisus teatise."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/17/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-threat-intelligence-report"></a>Azure'i turbekeskus ohtu aruanne
Selles dokumendis selgitatakse, kuidas Azure'i turvalisus Center ohtu nutikad aruannete saate lisateavet ohtu, loonud turvalisus teatise.

## <a name="what-is-a-threat-intelligence-report"></a>Mis on ohtu ärianalüüsi aruanne?
Turbekeskus ohtude tuvastamise toimib turvalisus teavet oma Azure ressursse, võrgu ja ühendatud partnerite lahenduste jälgimine. See analüüsib seda teavet, sageli tõestusmeetodid teavet ohtude tuvastamiseks mitmest allikast. See toiming on osa turbekeskus [võimaluste avastamiseks](security-center-detection-capabilities.md). 

Kui turbekeskus tuvastab ohtu, see käivitab ka [turbeteates](security-center-managing-and-responding-alerts.md), mis sisaldab üksikasjalikku teavet teatud sündmuse, sh parandamise soovitused. Langeva vastuse meeskondadel uurida ja migreerimisel ning probleemide lahendamisel ohtude abistamiseks turbekeskus sisaldab ohtu ärianalüüsi aruanne, mis sisaldab teavet ohtu, tuvastatud, sh teave on: 

- Ründaja identiteedi või ühendused (kui see teave on saadaval)
- Ründajad eesmärkide
- Praeguse ja ajalooliste rünnak Kampaaniat (kui see teave on saadaval)
- Ründajad taktikaid, tööriistad ja juhised
- Seotud näidikud, nt URL-id ja failiräsidest kompromiss (ROK)
- Viktimoloogiat, mis on valdkonna ja geograafilised esinemissageduse, mis aitavad teil kindlaks teha, kui teie Azure ressursid on risk
- Vähendamiseks ja parandamise teave

>[AZURE.NOTE] Teabe mis tahes kindla aruandes erinevad; üksikasjalikku teavet põhineb selle ründevara tegevuste ja esinemissageduse.

Turbekeskus on kolme tüüpi ohtu aruandeid, mis rünnak erineda. On saadaval aruanded.

- **Tegevuse rühma aruanne**: pakub sügav sukeldub ründajad, nende eesmärkide ja taktikaid.
- **Turunduskampaania aruande**: keskendub teatud rünnak Kampaaniat üksikasjad. 
- **Aruande ohtu Kokkuvõte**: hõlmab kõiki üksusi kaks aruannetes.

Seda tüüpi teavet on väga kasulik [langeva vastuse](security-center-incident-response.md) käigus, kus on pidev juurdlust mõista rünnak, ründaja põhjuste ja mida teha edasi probleemi leevendada. 

## <a name="how-to-access-the-threat-intelligence-report"></a>Kuidas pääseda juurde ohtu ärianalüüsi aruanne?

**Turbeteatiste** paani vaadates saate vaadata oma praeguse teatised. Avage Azure'i portaal ja järgige allpool näha kõiki teatisi rohkem üksikasju.

1. Armatuurlaual turbekeskus kuvatakse paani **Turbeteatiste** .

2. Klõpsake paani avamiseks **Turbeteatiste** tera, mis sisaldab täpsemat teavet teatiste ja seejärel turbeteates, mida soovite saada lisateavet kohta.

    ![Turbeteatiste](./media/security-center-threat-report/security-center-threat-report-fig1.png)

3. Sel juhul **kahtlaste käivitada protsessi** tera kuvatakse teatise üksikasjad, nagu on näidatud alloleval joonisel:

    ![Turvalisus teatis detais](./media/security-center-threat-report/security-center-threat-report-fig2.png)

4.  Saadaval iga turbeteates teabe hulk sõltub tüüpi teatise. **Aruannete** väli on link ohtu ärianalüüsi aruandesse. Klõpsake seda ja muu brauseriaknas kuvatakse PDF-faili abil.

    ![Salvestusruumi valiku](./media/security-center-threat-report/security-center-threat-report-fig3.png)

Siit saate alla laadida PDF-i aruande ja lugege lisateavet tuvastatud probleem turvalisus ja teha toiminguid, mis on esitatud teabe põhjal.

## <a name="see-also"></a>Vt ka

Selles dokumendis te teada, kuidas Azure'i turvalisus Center ohtu nutikad aruannete aitab Turbeteatiste kohta uurimise ajal. Azure'i turbekeskus kohta leiate lisateavet teemast järgmist:

- [Azure'i turbekeskus KKK](security-center-faq.md). Korduma kippuvad küsimused teenuse kasutamise kohta leiate.
- [Langeva vastamise Azure'i turbekeskus kasutamine](security-center-incident-response.md)
- [Azure'i turbekeskus tuvastamise võimalused](security-center-detection-capabilities.md)
- [Azure'i turbekeskus plaanimine ja toimingute juhend](security-center-planning-and-operations-guide.md). Saate teada, kuidas plaanimine ja kujundamine vastu võtta Azure'i turbekeskus mõista.
- [Haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](security-center-managing-and-responding-alerts.md). Saate teada, kuidas hallata ja turbeteatised vastata.
- [Töötlemise turvalisus juhtum Azure'i Security Center](security-center-incident.md)
- [Azure'i turvalisus ajaveebi](http://blogs.msdn.com/b/azuresecurity/). Otsige postitused Azure Turve ja nõuetele vastavuse kohta.
