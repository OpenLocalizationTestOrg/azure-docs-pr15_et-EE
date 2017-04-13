<properties
    pageTitle="Luua kohandatud armatuurlaua Log Analytics | Microsoft Azure'i"
    description="Sellest juhendist aitab teil mõista, kuidas Log Analytics armatuurlaudade saate visualiseerida kõik salvestatud log otsingud, annab teile ühe lens keskkonna kuvamiseks."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="create-a-custom-dashboard-in-log-analytics"></a>Log Analytics kohandatud Armatuurlaua loomine

Selle juhendi abil teil mõista, kuidas Log Analytics armatuurlaudade visualiseerida kõik salvestatud log otsingud, võimaldades ühe lens keskkonna kuvamiseks.

![Näidisarmatuurlaual](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

Kohandatud armatuurlaudade, OMS portaali loodud saadaolevaid OMS Mobile'i rakenduses. Lisateavet rakenduste järgmiste lehtede kuvamine

- [Microsoft Store OMS mobiilirakenduse kaudu](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
- [Apple iTunes OMS mobiilirakenduse kaudu](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobiilse armatuurlaud](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Kuidas luua oma armatuurlaua?

Alustamiseks avage leht OMS ülevaade. **Minu armatuurlaua** paanil kuvatakse vasakul. Klõpsake seda armatuurlauale süvitsiminek.

![Ülevaade](./media/log-analytics-dashboards/oms-dashboards-overview.png)


## <a name="adding-a-tile"></a>Paan lisamine

Armatuurlaudade, paanid on tootja salvestatud log otsingusse. OMS sisaldab palju eelnevalt tehtud salvestatud log otsinguid, et saaksite hakata kohe. Tehke järgmist, et määrata, kuidas alustada.

Armatuurlaua minu vaates lihtsalt **kohandamine** sisestamiseks klõpsake nuppu Kohanda režiim.

![Kujunduselemendid](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Paneeli, mis avab lehe paremas servas kuvatakse kõik teie tööruumi salvestatud log otsingud. Visualiseerida salvestatud log otsingu paanina, viige kursor salvestatud otsingu ja klõpsake **pluss** sümbol.

![Paanide 1 lisamine](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

**Lisaks** sümbol klõpsamisel kuvatakse uue paani minu armatuurlaua vaates.

![Paanide 2 lisamine](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)


## <a name="edit-a-tile"></a>Paani redigeerimine

Armatuurlaua minu vaates lihtsalt **kohandamine** sisestamiseks klõpsake nuppu Kohanda režiim. Klõpsake paani, mida soovite redigeerida. Õige ekraani muudatused redigeerimiseks ja annab valiku suvanditest:

![Paani redigeerimine](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Paani redigeerimine](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Paani visualiseeringud#
On kolme tüüpi paani visualiseeringud valida.

|Diagrammi tüüp|mida see tähendab?|
|---|---|
|![Lintdiagramm](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png)|Kuvab salvestatud log Otsingu tulemused ajaskaala lintdiagrammi või tulemused välja sõltuvalt loendi kui log otsingu liitmise tulemused välja või mitte.
|![meetermõõdustik](./media/log-analytics-dashboards/oms-dashboards-metric.png)|Kuvab teie kokku log Otsingu tulem tabamust paanil arvuna. Argumendil paanid võimaldavad teil määrata, mis toob esile paani piirmäära jõudmisel.|
|![rea](./media/log-analytics-dashboards/oms-dashboards-line.png)|Kuvab teie salvestatud log Otsingu tulem tabamust väärtustega ajaskaala joondiagramm.|

### <a name="threshold"></a>Lävi
Paani argumendil visualiseeringu abil saate luua läve. Valige paanil läve loomiseks. Valige, kas paani esile tõsta, kui väärtus on valitud lävi alla või üle, siis allpool läviväärtuse väärtus.

## <a name="organizing-the-dashboard"></a>Armatuurlaua korraldamine
Armatuurlaua korraldada, liikuge Kuva minu armatuurlaua ja klõpsake nuppu **Kohanda** sisestamiseks kohandada režiim. Klõpsake ja lohistage paani, mille soovite teisaldada ja teisaldage see, kuhu soovite oma paani.

![Armatuurlaua korraldamine](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Paani eemaldamine
Paani eemaldamine, liikuge Kuva minu armatuurlaua ja klõpsake nuppu **Kohanda** sisestamiseks kohandada režiim. Valige paan, mille soovite eemaldada ja seejärel valige paremal **Eemaldada paani**.

![Paani eemaldamine](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Järgmised sammud

- [Teatiste](log-analytics-alerts.md) loomine Log Analytics luua teatised ja migreerimisel ning probleemide lahendamisel probleeme.
