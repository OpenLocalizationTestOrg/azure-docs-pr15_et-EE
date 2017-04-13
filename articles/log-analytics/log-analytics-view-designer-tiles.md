<properties
    pageTitle="Logige Kasutusanalüüsi kuvamine kujundaja paani viide | Microsoft Azure'i"
    description="Kuvakujundaja Log Analytics võimaldab teil luua kohandatud vaateid OMS konsool, mis sisaldavad erinevate visualiseeringute andmete OMS hoidla. Selles artiklis antakse viide sätted iga paanid saadaval kasutada oma kohandatud vaateid."
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

# <a name="log-analytics-view-designer-tile-reference"></a>Log Kasutusanalüüsi kuvamine kujundaja paani viide
Kuvakujundaja Log Analytics võimaldab teil luua kohandatud vaateid OMS konsool, mis sisaldavad erinevate visualiseeringute andmete OMS hoidla. Sellest artiklist leiate sätted iga saadaval kasutada oma kohandatud vaated paanid viite.

Muud artiklid Kuvakujundaja jaoks saadaval on.

- [Kuvakujundaja](log-analytics-view-designer.md) - ülevaade Kuvakujundaja ja toimingute loomise ja redigeerimise kohandatud vaateid.
- [Visualiseeringu osa viide](log-analytics-view-designer-parts.md) – viide sätted iga paanid saadaval kasutada oma kohandatud vaateid. 


Järgmises tabelis on loetletud paanid Designeris vaade saadaval eri tüüpi.  Järgmistes jaotistes kirjeldatakse iga paani tüüp üksikasjad ja nende atribuudid.

| Paan | Kirjeldus |
|:--|:--|
| [Arv](#number-tile) | Ühe arv näitab päringu kirjete arvu. |
| [Kahe arvu](#two-numbers-tile) | Kahe arvu ühe nähtaval kirjete kaks erinevat päringute arv. |
| [Rõngas](#donut-tile) | Rõngasdiagramm Kokkuvõte väärtusega keskel päringul põhinev. |
| [Joondiagramm ja viiktekst](#line-chart-amp-callout-tile) | Päringu ja Kokkuvõte väärtusega viikteksti joondiagramm. |
| [Joondiagramm](#line-chart-tile) | Päringul põhinev joondiagramm. |
| [Kahe ajaskaalad](#two-timelines-tile) | Kaks rida iga eraldi päringul põhinev virntulpdiagramm. |



## <a name="number-tile"></a>Arv paan

Paani **Number** kuvatakse ühe arv näitab Logi päringu ja sildi kirjete arvu.

![Arv paan](media/log-analytics-view-designer/tile-number.png)

| Säte | Kirjeldus |
|:--|:--|
| Nimi        | Paani ülaosas kuvatav tekst. |
| Kirjeldus | Paani nime all kuvatav tekst.    |
| **Paan** |
| Legendi | Klõpsake jaotises väärtuse kuvatav tekst. |
| Päringu | Päringu käivitamiseks.  Kuvatakse päringu tagastatud kirjete arvu. |
| **Täpsemad** |  **> Andmevoo kontrollimine** |
| Lubatud | Valige, kui andmevoo kinnitamine peaks olema lubatud paani.  See pakub alternatiivse meilisõnumi, kui andmed ei ole saadaval paani.  Tavaliselt kasutatakse saate sisestada sõnumi ajutine jooksul, kui vaade on installitud andmed on saadaval. |
| Päringu | Päringu käivitamiseks kontrollida, kui andmed on vaates saadaval.  Kui päring tagastab tulemeid, siis kuvatakse teade asemel peamine päringule väärtuse. |
| Sõnumi | Kui kinnitamine andmevoo päring tagastab andmed kuvatava teate.  Kui annate pole sõnumit, kuvatakse *Läbimiseks hindamine* . |
| **Ajavahemik** |
| Kestus | Kestus tänasest kuupäevast ajavahemiku päringu jaoks kasutada.  Näiteks kui määratud on **7 päeva** , siis päring on piiratud kirjed, mis on loodud 7 päeva tagasi praeguse kuupäeva. |
| Lõpeta andmete offset | Valikuline offset praegusi andmeid ajavahemiku peamised päringu jaoks kasutada.  Näiteks kui **-1 päeva** kasutatakse **lõpp kuupäev offset** ja **7 päeva** kasutada **kestus**, siis päring on piiratud, 8 päeva tagasi, et eile loodud kirjed. |

## <a name="two-numbers-tile"></a>Kahe arvu paan

**Kahe arvu** paanil kuvatakse kahe arvu, mis näitab iga arvu kahe eri log päringuid ja sildi kirjeid.

![Kahe arvu paan](media/log-analytics-view-designer/tile-two-numbers.png)

| Säte | Kirjeldus |
|:--|:--|
| Nimi        | Paani ülaosas kuvatav tekst. |
| Kirjeldus | Paani nime all kuvatav tekst.    |
| **Esimene paan** |
| Legendi | Klõpsake jaotises väärtuse kuvatav tekst. |
| Päringu | Päringu käivitamiseks.  Kuvatakse päringu tagastatud kirjete arvu. |
| **Teine paan** |
| Legendi | Klõpsake jaotises väärtuse kuvatav tekst. |
| Päringu | Päringu käivitamiseks.  Kuvatakse päringu tagastatud kirjete arvu. |
| **Täpsemad** | **> Andmevoo kontrollimine** |
| Lubatud | Valige, kui andmevoo kinnitamine peaks olema lubatud paani.  See pakub alternatiivse meilisõnumi, kui andmed ei ole saadaval paani.  Tavaliselt kasutatakse saate sisestada sõnumi ajutine jooksul, kui vaade on installitud andmed on saadaval. |
| Päringu | Päringu käivitamiseks kontrollida, kui andmed on vaates saadaval.  Kui päring tagastab tulemeid, siis kuvatakse teade asemel peamine päringule väärtuse. |
| Sõnumi | Kui kinnitamine andmevoo päring tagastab andmed kuvatava teate.  Kui annate pole sõnumit, kuvatakse *Läbimiseks hindamine* . |
| **Ajavahemik** |
| Kestus | Kestus tänasest kuupäevast ajavahemiku päringu jaoks kasutada.  Näiteks kui määratud on **7 päeva** , siis päring on piiratud kirjed, mis on loodud 7 päeva tagasi praeguse kuupäeva. |
| Lõpeta andmete offset | Valikuline offset praegusi andmeid ajavahemiku peamised päringu jaoks kasutada.  Näiteks kui **-1 päeva** kasutatakse **lõpp kuupäev offset** ja **7 päeva** kasutada **kestus**, siis päring on piiratud, 8 päeva tagasi, et eile loodud kirjed. |

## <a name="donut-tile"></a>Rõngas paan

Paani **rõngas** kuvab ühe väärtuse veerus log päringu summeeritud arvu.  Funktsiooni rõngas graafiliselt kuvab tulemusi ülemised kolme kirja.

![Rõngas paan](media/log-analytics-view-designer/tile-donut.png)

| Säte | Kirjeldus |
|:--|:--|
| Nimi        | Paani ülaosas kuvatav tekst. |
| Kirjeldus | Paani nime all kuvatav tekst.    |
| **Rõngas** |
| Päringu | Päringu käivitamine on rõngas.  Esimene peaks olema tekstväärtuse ja teine atribuudi arvulise väärtuse.  See on tavaliselt päringu, mis kasutab **mõõt** märksõna summeeritavaid tulemused. |
| **Rõngas** | **> Center** |
| Teksti | Jaotises väärtus sees on rõngas kuvatav tekst. |
| Toiming | Selle toimingu sooritamiseks summeeritavaid on üks väärtus atribuudi väärtus.<br><br>-Summa: Lisada kõik kirjed väärtusega kinnisvara väärtuste.<br>-Protsent: Protsent ületa väärtustega kirjed abil kõik kirjed ületa väärtuste atribuudi väärtust. |
| Tulemus väärtused kasutatud keskmist toiming | Soovi korral klõpsake plussmärki, et lisada ühe või mitu väärtust.  Päringu tulemused piirduvad määrate atribuudi väärtustega kirjed.  Kui ei sisalda väärtusi on lisatud, kui päring on kaasatud kõik kirjed. |
| **Rõngas** | **> Veel suvandeid** |
| Värvid | Värvi iga kolme ülemise atribuutide kuvamiseks.  Kui soovite määrata alternatiivse värvid teatud kinnisvara, siis kasutage Täpsem värvi vastendamise. |
| Täiustatud värvi kaardistamine | Kuvab teatud atribuudi väärtuste jaoks soovitud värv.  Kui teie määratud väärtuse, on top kolm, siis alternatiivse värvi kuvatakse asemel standard värv.  Kui see atribuut pole top kolm, seejärel soovitud värvi ei kuvata. |
| **Täpsemad** | **> Andmevoo kontrollimine** |
| Lubatud | Valige, kui andmevoo kinnitamine peaks olema lubatud paani.  See pakub alternatiivse meilisõnumi, kui andmed ei ole saadaval paani.  Tavaliselt kasutatakse saate sisestada sõnumi ajutine jooksul, kui vaade on installitud andmed on saadaval. |
| Päringu | Päringu käivitamiseks kontrollida, kui andmed on vaates saadaval.  Kui päring tagastab tulemeid, siis kuvatakse teade asemel peamine päringule väärtuse. |
| Sõnumi | Kui kinnitamine andmevoo päring tagastab andmed kuvatava teate.  Kui annate pole sõnumit, kuvatakse *Läbimiseks hindamine* . |
| **Ajavahemik** |
| Kestus | Kestus tänasest kuupäevast ajavahemiku päringu jaoks kasutada.  Näiteks kui määratud on **7 päeva** , siis päring on piiratud kirjed, mis on loodud 7 päeva tagasi praeguse kuupäeva. |
| Lõpeta andmete offset | Valikuline offset praegusi andmeid ajavahemiku peamised päringu jaoks kasutada.  Näiteks kui **-1 päeva** kasutatakse **lõpp kuupäev offset** ja **7 päeva** kasutada **kestus**, siis päring on piiratud, 8 päeva tagasi, et eile loodud kirjed. |

## <a name="line-chart-tile"></a>Rea diagrammi paan

**Joondiagramm** paan kuvab joondiagrammi mitut andmesarja päringust Logi koos aja jooksul.  

![Joondiagramm ja viikteksti paan](media/log-analytics-view-designer/tile-line-chart.png)

| Säte | Kirjeldus |
|:--|:--|
| Nimi        | Paani ülaosas kuvatav tekst. |
| Kirjeldus | Paani nime all kuvatav tekst.    |
| **Joondiagramm** |  
| Päringu | Päringu käivitamiseks joondiagrammil jaoks.  Esimene peaks olema tekstväärtuse ja teine atribuudi arvulise väärtuse.  See on tavaliselt päringu, mis kasutab **mõõt** märksõna summeeritavaid tulemused.  Kui päring kasutab seda **intervalli** märksõna siis diagrammi x-telje kasutab seda ajavahemik.  Kui päring ei sisalda **intervall** märksõna siis tundide järel kasutatakse x-telje. |
| **Joondiagramm** | **> Y-telg** |
| Kasutage logaritmilist skaalat | Valige y-telje muuta logaritmiliseks skaalaks kasutamiseks. |
| Ühikud | Määrake päringu tagastatud väärtused mõõtühikud.  Seda teavet kasutatakse siltide kuvamiseks Failitüübid väärtust, mis näitab, diagrammi ja soovi korral teisendamise väärtused.  **Üksuse tüüp** määrab üksuse kategooria ja määratleb **Praeguse üksuse tüüp** väärtused, mis on saadaval.  Kui valite väärtuse **teisendamine** teisendatakse arvväärtuste **Praeguse üksuse** tüüp, millest **teisendamine** tüüp. |
| Kohandatud sildi | Y telje silt üksuse tüüp kõrval kuvatav tekst.  Kui silti pole määratud, kuvatakse ainult üksuse tüüp. |
| **Täpsemad** | **> Andmevoo kontrollimine** |
| Lubatud | Valige, kui andmevoo kinnitamine peaks olema lubatud paani.  See pakub alternatiivse meilisõnumi, kui andmed ei ole saadaval paani.  Tavaliselt kasutatakse saate sisestada sõnumi ajutine jooksul, kui vaade on installitud andmed on saadaval. |
| Päringu | Päringu käivitamiseks kontrollida, kui andmed on vaates saadaval.  Kui päring tagastab tulemeid, siis kuvatakse teade asemel peamine päringule väärtuse. |
| Sõnumi | Kui kinnitamine andmevoo päring tagastab andmed kuvatava teate.  Kui annate pole sõnumit, kuvatakse *Läbimiseks hindamine* . |
| **Ajavahemik** |
| Kestus | Kestus tänasest kuupäevast ajavahemiku päringu jaoks kasutada.  Näiteks kui määratud on **7 päeva** , siis päring on piiratud kirjed, mis on loodud 7 päeva tagasi praeguse kuupäeva. |
| Lõpeta andmete offset | Valikuline offset praegusi andmeid ajavahemiku peamised päringu jaoks kasutada.  Näiteks kui **-1 päeva** kasutatakse **lõpp kuupäev offset** ja **7 päeva** kasutada **kestus**, siis päring on piiratud, 8 päeva tagasi, et eile loodud kirjed. |


## <a name="line-chart--callout-tile"></a>Joone viikteksti & Diagrammi paan

**Joondiagramm ja viikteksti** paan kuvab joondiagrammi mitut andmesarja päringust Logi koos aja ja viikteksti summeeritud väärtusega.  

![Joondiagramm ja viikteksti paan](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Säte | Kirjeldus |
|:--|:--|
| Nimi        | Paani ülaosas kuvatav tekst. |
| Kirjeldus | Paani nime all kuvatav tekst.    |
| **Joondiagramm** |  
| Päringu | Päringu käivitamiseks joondiagrammil jaoks.  Esimene peaks olema tekstväärtuse ja teine atribuudi arvulise väärtuse.  See on tavaliselt päringu, mis kasutab **mõõt** märksõna summeeritavaid tulemused.  Kui päring kasutab seda **intervalli** märksõna siis diagrammi x-telje kasutab seda ajavahemik.  Kui päring ei sisalda **intervall** märksõna siis tundide järel kasutatakse x-telje. |
| **Joondiagramm** | **> Viiktekst** |
| Viiktekst | Pealkirja kohale viikteksti väärtuse kuvatav tekst. |
| Sarja nimi | Atribuudi väärtus sarja viikteksti väärtuse jaoks kasutada.  Kui ühtegi sarja, kasutatakse päringu kõik kirjed. |
| Toiming | Selle toimingu sooritamiseks summeeritavaid üksikväärtus viikteksti jaoks atribuudi väärtus.<br>– Keskmise: Kõik kirjed alates väärtusest Keskmine.<br><br>-Count: Päringu tagastatavad kõigi kirjete arvu.<br>-Viimase näidis: Kaasatud diagrammi viimase intervalli väärtus.<br>-Max: Kaasatud diagrammi sagedus maksimum väärtus.<br>-Min: Kaasatud diagrammi intervalle väikseim väärtus.<br>-Summa: Kõik kirjed alates väärtusest summa. |
| **Joondiagramm** | **> Y-telg** |
| Kasutage logaritmilist skaalat | Valige y-telje muuta logaritmiliseks skaalaks kasutamiseks. |
| Ühikud | Määrake päringu tagastatud väärtused mõõtühikud.  Seda teavet kasutatakse siltide kuvamiseks Failitüübid väärtust, mis näitab, diagrammi ja soovi korral teisendamise väärtused.  **Üksuse tüüp** määrab üksuse kategooria ja määratleb **Praeguse üksuse tüüp** väärtused, mis on saadaval.  Kui valite väärtuse **teisendamine** teisendatakse arvväärtuste **Praeguse üksuse** tüüp, millest **teisendamine** tüüp. |
| Kohandatud sildi | Y telje silt üksuse tüüp kõrval kuvatav tekst.  Kui silti pole määratud, kuvatakse ainult üksuse tüüp. |
| **Täpsemad** | **> Andmevoo kontrollimine** |
| Lubatud | Valige, kui andmevoo kinnitamine peaks olema lubatud paani.  See pakub alternatiivse meilisõnumi, kui andmed ei ole saadaval paani.  Tavaliselt kasutatakse saate sisestada sõnumi ajutine jooksul, kui vaade on installitud andmed on saadaval. |
| Päringu | Päringu käivitamiseks kontrollida, kui andmed on vaates saadaval.  Kui päring tagastab tulemeid, siis kuvatakse teade asemel peamine päringule väärtuse. |
| Sõnumi | Kui kinnitamine andmevoo päring tagastab andmed kuvatava teate.  Kui annate pole sõnumit, kuvatakse *Läbimiseks hindamine* . |
| **Ajavahemik** |
| Kestus | Kestus tänasest kuupäevast ajavahemiku päringu jaoks kasutada.  Näiteks kui määratud on **7 päeva** , siis päring on piiratud kirjed, mis on loodud 7 päeva tagasi praeguse kuupäeva. |
| Lõpeta andmete offset | Valikuline offset praegusi andmeid ajavahemiku peamised päringu jaoks kasutada.  Näiteks kui **-1 päeva** kasutatakse **lõpp kuupäev offset** ja **7 päeva** kasutada **kestus**, siis päring on piiratud, 8 päeva tagasi, et eile loodud kirjed. |

## <a name="two-timelines-tile"></a>Kahe ajaskaalade paan

**Kahe ajaskaalade** paani kuvatakse kahe logi päringu tulemused aja jooksul tulpdiagrammid.  Viikteksti kuvatakse iga sarja.  

![Kahe ajaskaalade paan](media/log-analytics-view-designer/tile-two-timelines.png)

| Säte | Kirjeldus |
|:--|:--|
| Nimi        | Paani ülaosas kuvatav tekst. |
| Kirjeldus | Paani nime all kuvatav tekst.    |
| Esimene diagramm   
| Legendi | Jaotises viikteksti esimese rea jaoks kuvatav tekst.
| Värvi | Sarja esimese veeru jaoks kasutatavat värvi.
| Diagrammi päring | Päringu käivitamiseks esimese sarja.  Diagrammi tulpadele esindab iga ajavahemiku jooksul kirjete arvu.
| Toiming | Selle toimingu sooritamiseks summeeritavaid üksikväärtus viikteksti jaoks atribuudi väärtus.<br><br>– Keskmise: Kõik kirjed alates väärtusest Keskmine.<br>-Count: Päringu tagastatavad kõigi kirjete arvu.<br>-Viimase näidis: Kaasatud diagrammi viimase intervalli väärtus.<br>-Max: Kaasatud diagrammi sagedus maksimum väärtus.
| **Teise diagrammi** |
| Legendi | Jaotises viikteksti teise rea jaoks kuvatav tekst.
| Värvi | Teise rea veeru jaoks kasutatavat värvi.
| Diagrammi päring | Päringu käivitamiseks teise rea.  Diagrammi tulpadele esindab iga ajavahemiku jooksul kirjete arvu.
| Toiming | Selle toimingu sooritamiseks summeeritavaid üksikväärtus viikteksti jaoks atribuudi väärtus.<br><br>– Keskmise: Kõik kirjed alates väärtusest Keskmine.<br>-Count: Päringu tagastatavad kõigi kirjete arvu.<br>-Viimase näidis: Kaasatud diagrammi viimase intervalli väärtus.<br>-Max: Kaasatud diagrammi sagedus maksimum väärtus. |
| **Täpsemad** | **> Andmevoo kontrollimine** |
| Lubatud | Valige, kui andmevoo kinnitamine peaks olema lubatud paani.  See pakub alternatiivse meilisõnumi, kui andmed ei ole saadaval paani.  Tavaliselt kasutatakse saate sisestada sõnumi ajutine jooksul, kui vaade on installitud andmed on saadaval. |
| Päringu | Päringu käivitamiseks kontrollida, kui andmed on vaates saadaval.  Kui päring tagastab tulemeid, siis kuvatakse teade asemel peamine päringule väärtuse. |
| Sõnumi | Kui kinnitamine andmevoo päring tagastab andmed kuvatava teate.  Kui annate pole sõnumit, kuvatakse *Läbimiseks hindamine* . |
| **Ajavahemik** |
| Kestus | Kestus tänasest kuupäevast ajavahemiku päringu jaoks kasutada.  Näiteks kui määratud on **7 päeva** , siis päring on piiratud kirjed, mis on loodud 7 päeva tagasi praeguse kuupäeva. |
| Lõpeta andmete offset | Valikuline offset praegusi andmeid ajavahemiku peamised päringu jaoks kasutada.  Näiteks kui **-1 päeva** kasutatakse **lõpp kuupäev offset** ja **7 päeva** kasutada **kestus**, siis päring on piiratud, 8 päeva tagasi, et eile loodud kirjed. |

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [log otsingud](log-analytics-log-searches.md) päringute toetamiseks paanid.
- [Visualiseeringu osa](log-analytics-view-designer-parts.md) lisada oma kohandatud vaade.