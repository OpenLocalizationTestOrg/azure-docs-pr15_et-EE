<properties
    pageTitle="Logige Analytics Kuvakujundaja | Microsoft Azure'i"
    description="Kuvakujundaja Log Analytics võimaldab teil luua kohandatud vaateid OMS konsool, mis sisaldavad erinevate visualiseeringute andmete OMS hoidla. Käesolevast artiklist leiate ülevaate Kuvakujundaja ja toimingute loomise ja redigeerimise kohandatud vaateid."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer"></a>Log Analytics Kuvakujundaja
Kuvakujundaja Log Analytics võimaldab teil luua kohandatud vaateid OMS konsool, mis sisaldavad erinevate visualiseeringute andmete OMS hoidla. Käesolevast artiklist leiate ülevaate Kuvakujundaja ja toimingute loomise ja redigeerimise kohandatud vaateid.

Muud artiklid Kuvakujundaja jaoks saadaval on.

- [Paani viide](log-analytics-view-designer-tiles.md) – viide sätted iga paanid saadaval kasutada oma kohandatud vaateid. 
- [Visualiseeringu osa viide](log-analytics-view-designer-parts.md) – viide sätted iga paanid saadaval kasutada oma kohandatud vaateid. 


## <a name="concepts"></a>Mõisted
Vaadete kuvamine kujundaja loodud sisaldama järgmises tabelis.

| Veebiosa | Kirjeldus |
|:--|:--|
| Paan | Peamised Log Analytics ülevaate armatuurlaual kuvatud.  Sisaldab kohandatud vaates sisalduva teabe summeerimine visuaalse.  Paani tüüpide pakuvad erinevate visualiseeringute kirjete OMS hoidla.  Klõpsake paani kohandatud vaate avamiseks. |
| Kohandatud vaate | Kuvatakse, kui kasutaja klõpsab paani.  Ühe või mitme visualiseeringu osa sisaldab. |
| Visualiseeringu osa | Visualiseeringu andmete põhjal ühe või mitme [log otsingud](log-analytics-log-searches.md)OMS hoidla.  Enamik osa sisaldab päise, mis pakub kõrge visualiseerimine ja ülemise tulemite loend.  Osa eri tüüpi pakuvad erinevate visualiseeringute kirjete OMS hoidla.  Elementide osa esitada üksikasjalikud andmed log otsimiseks klõpsake nuppu. |

![Vaate kujundaja ülevaade](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Kuvakujundaja lisamine tööruumi
Kuvakujundaja on eelvaade, peate lisama selle oma tööruumi, valides **Eelvaatefunktsioonide** OMS portaali jaotises **sätted** .

![Luba eelvaade](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Loomise ja redigeerimise vaated

### <a name="create-a-new-view"></a>Uue vaate loomine
Avage uue vaate **Kuvakujundaja** klõpsates paani Kuvakujundaja peamised OMS armatuurlaual.

![Vaate Designer paan](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Olemasoleva vaate redigeerimine
Olemasolevat vaadet Designeris vaate redigeerimiseks avada vaade klõpsates selle paani peamise OMS armatuurlaua.  Klõpsake vaate avamiseks Designeris vaate **redigeerimine** nuppu.

![Vaate redigeerimine](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Klooni olemasolev vaade
Kui te klooni vaate, loob uue vaate ja avatakse see vaade Designer.  Uus vaade on sama nimi koos algse nimega "Kopeeri" lisatud lõpuni.  Klooni vaate, avage olemasolevat vaadet, valides selle paani peamise OMS armatuurlaua.  Klõpsake nuppu **klooni** Designeris vaade vaate avamiseks.

![Vaate klooni](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Kustutage olemasolev vaade
Olemasoleva vaate kustutamiseks avamiseks klõpsata oma peamist OMS armatuurlaua paanil vaade.  Seejärel Designeris vaade vaate avamiseks nuppu **Redigeeri** ja klõpsake nuppu **Kustuta vaade**.

![Vaate kustutamine](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Olemasolev vaade eksportimine
Saate eksportida vaate JSON faili, mida saate importida teise tööruumi või [Azure ressursihaldur malli](../resource-group-authoring-templates.md)kasutada.  Avada eksportimiseks olemasolevat vaadet, klõpsates selle paani peamise OMS armatuurlaua vaade.  Seejärel nuppu **ekspordi** faili brauseri allalaadimine kausta loomiseks.  Faili nimi on nimi laiend *omsview*vaade.

![Eksportida vaate](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Olemasoleva vaate importimine
Saate importida eksporditud rühmast teise halduse *omsview* -fail.  Importimine olemasolevat vaadet, peate esmalt looma uue vaate.  Klõpsake nuppu **impordi** ja valige *omsview* fail.  Otsingukonfiguratsiooni failis kopeeritakse olemasolevat vaadet.

![Eksportida vaate](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Töötamine Kuvakujundaja
Vaate Designer on kolm paani.  **Kujunduse** paani tähistab kohandatud vaate.  Kui lisate paanid ja osa paanilt **juhtelemendi** **kujunduse** paani need lisatakse vaatesse.  Paanil **Atribuudid** kuvatakse paan või valitud veebiosa atribuute.

![Kuvakujundaja](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfigureerige vaate paan
Kohandatud vaate võib olla ainult ühe paani.  Valige vahekaart **paani** vaatamiseks praegusele paanile või valige alternatiivse paanil **juhtelement** .  Paanil **Atribuudid** kuvatakse atribuutide praegusele paanile.  Üksikasjalikku teavet vastavalt paani atribuudid konfigureerida [Paani viide](log-analytics-view-designer-tiles.md) ja klõpsake nuppu **Rakenda** muudatuste salvestamiseks.

### <a name="configure-visualization-parts"></a>Visualiseeringu osa konfigureerimine
Vaade võib sisaldada mis tahes arv visualiseeringu osa.  Valige menüü **Vaade** ja seejärel visualiseeringu osa vaate lisada.  Paanil **Atribuudid** kuvatakse valitud veebiosa atribuute.  [Visualiseeringu osa viide](log-analytics-view-designer-parts.md) konfigureerimine üksikasjalikku teavet vastavalt atribuutide kuvamine ja klõpsake nuppu **Rakenda** muudatuste salvestamiseks.

### <a name="delete-a-visualization-part"></a>Visualiseeringu osa kustutamine
Visualiseeringu osa eemaldamiseks vaate osa paremas ülanurgas nuppu **X** .

### <a name="rearrange-visualization-parts"></a>Visualiseeringu osa ümberkorraldamine
Vaated on ainult ühe rea visualiseeringu osa.  Olemasoleva osade vaates ümberkorraldamine, klõpsates ja lohistades neid uude asukohta.


## <a name="next-steps"></a>Järgmised sammud

- Saate lisada oma kohandatud vaate [paanid](log-analytics-view-designer-tiles.md) .
- [Visualiseeringu osa](log-analytics-view-designer-parts.md) lisada oma kohandatud vaade.
