<properties
    pageTitle="Muutuste jälituse lahenduse Log Analytics | Microsoft Azure'i"
    description="Saate konfiguratsiooni muutuste jälitamise lahendus Log Analytics aidata teil hõlpsasti kindlaks teha, tarkvara ja keskkonna muutuste Windows Services – need konfiguratsioonimuudatuste tuvastamise aitavad teil Pinpointi küsimusi."
    services="operations-management-suite"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="operations-management-suite"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="change-tracking-solution-in-log-analytics"></a>Muutmine Logi Analytics jälgimise lahendus


Saate konfiguratsiooni muutuste jälitamise lahendus Log Analytics aidata teil hõlpsasti kindlaks teha, tarkvara ja teenuste Windows muutusi ja Linux daemon muutuste keskkonna – need konfiguratsioonimuudatuste tuvastamise aitavad teil Pinpointi küsimusi.

Installida lahenduse värskendamiseks installitud tüüp. Muudatuste installitud tarkvara Windows services jälgida serverites on lugemine ja seejärel andmed saadetakse Log Analytics teenuse pilveteenuses töötlemiseks. Loogika on saadud andmetele rakendatud ja pilveteenusesse andmed. Muutuste leidmisel serverite muudatused on näidatud muutuste jälitamise armatuurlaud. Muutuste jälitamise armatuurlaual toodud teabe abil näete oma serveri infrastruktuuri tehtud muudatused.

## <a name="installing-and-configuring-the-solution"></a>Installimine ja konfigureerimine lahendus
Kasutage järgmist teavet, installida ja konfigureerida lahendus.

- Toiminguhalduri jaoks on vaja muutuste jälitamise lahendus.
- Peate igas arvutis, kuhu soovite jälgida muudatusi Windows või Toiminguhalduri agent.
- Muutuste jälitamise lahendus lisada oma OMS tööruumi [lisada Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md)kirjeldatud protsessi abil.  On pole veel konfigureerimine vajalik.


## <a name="change-tracking-data-collection-details"></a>Jälgimise andmete kogumise üksikasjade muutmine

Muutuste jälituse kogub tarkvara varude ja Windows teenus metaandmete abil agentide, mida teil on lubatud.

Järgmises tabelis on andmete kogumise meetodite ja muud üksikasjad kohta, kuidas andmeid kogutakse muutuste jälitamise.

| platvorm | Otsest Agent | SCOM agent | Azure'i salvestusruum | SCOM on nõutav? | Rühma kaudu saadetud SCOM agendi andmed | saidikogumi sagedus |
|---|---|---|---|---|---|---|
|Windows|![Jah](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Jah](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Ei](./media/log-analytics-change-tracking/oms-bullet-red.png)|            ![Ei](./media/log-analytics-change-tracking/oms-bullet-red.png)|![Jah](./media/log-analytics-change-tracking/oms-bullet-green.png)| tunni|

## <a name="use-change-tracking"></a>Muutuste jälitamise kasutamine

Kui lahendus on installitud, saate vaadata muudatuste kokkuvõte teie jälgitud serverite OMS lehel **Ülevaade** paani **Muutuste jälituse** abil.

![Muutuste jälitamise paani pilt](./media/log-analytics-change-tracking/oms-changetracking-tile.png)

Muudatuste taristu ja seejärel drill üheks üksikasjad järgmiste kategooriate vaatamine

- Konfiguratsiooni muutusi tippige tarkvara ja teenuste Windowsi jaoks
- Rakenduste ja üksikute serverite värskenduste tarkvara muudatused
- Tarkvara muudatused iga rakenduse arv
- Windowsi teenusemuudatusest üksikute serverite jaoks

![Muutuste jälitamise armatuurlaua pilt](./media/log-analytics-change-tracking/oms-changetracking01.png)

![Muutuste jälitamise armatuurlaua pilt](./media/log-analytics-change-tracking/oms-changetracking02.png)

### <a name="to-view-changes-for-any-change-type"></a>Vaadata muudatusi, mis tahes tüübi muutmine

1. Klõpsake lehe **Ülevaade** **Muutuste jälitamise** paani.
2. Armatuurlaual **Muuta jälgimise** ühes muudatuse tüüp labad Kokkuvõte teave üle ja seejärel klõpsake ühte vaadata üksikasjalikku teavet lehel **log** .
3. Mis tahes Logi otsing, saate vaadata tulemusi aeg, üksikasjalikud tulemused ja ajaloo log otsing. Samuti saate filtreerida omadustega tulemeid piiritleda.

## <a name="next-steps"></a>Järgmised sammud

- [Log otsingud Log Analytics](log-analytics-log-searches.md) abil saate vaadata üksikasjalikku jälituse andmed.
