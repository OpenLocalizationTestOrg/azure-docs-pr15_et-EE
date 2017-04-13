<properties
    pageTitle="Logige Analytics Kuvakujundaja | Microsoft Azure'i"
    description="Kuvakujundaja Log Analytics võimaldab teil luua kohandatud vaateid OMS konsool, mis sisaldavad erinevate visualiseeringute andmete OMS hoidla. Selles artiklis antakse viite sätete iga visualiseeringu osa saadaval kasutada oma kohandatud vaateid."
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
    ms.date="10/20/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-visualization-part-reference"></a>Logige Analytics Kuvakujundaja visualiseeringu osa viide
Kuvakujundaja Log Analytics võimaldab teil luua kohandatud vaateid OMS konsool, mis sisaldavad erinevate visualiseeringute andmete OMS hoidla. Selles artiklis antakse viite sätete iga visualiseeringu osa saadaval kasutada oma kohandatud vaateid.

Muud artiklid Kuvakujundaja jaoks saadaval on.

- [Kuvakujundaja](log-analytics-view-designer.md) - ülevaade Kuvakujundaja ja toimingute loomise ja redigeerimise kohandatud vaateid.
- [Paani viide](log-analytics-view-designer-tiles.md) – viide sätted iga paanid saadaval kasutada oma kohandatud vaateid. 

Järgmises tabelis kirjeldatakse erinevat tüüpi saadaval Designeris View paanid.  Järgmistes jaotistes kirjeldatakse iga paani tüüp üksikasjad ja nende atribuudid.

| Vaate tüüp | Kirjeldus |
|:--|:--|
| [Päringute loend](#list-of-queries-part) | Kuvatakse loend log otsingupäringuid.  Kasutaja klõpsata iga päringu tulemuste kuvamiseks.  |
| [Arvu ja loend](#number-amp-list-part) | Päis on ühe arvu kuvav count log otsingupäringu kirjeid.  Loend kuvatakse graafik arvuveerg või selle muutmine aja jooksul suhteline väärtus, mis näitab, kus ülemise kümme päringu tulemusi. |
| [Kahe arvude ja loend](#two-numbers-amp-list-part) | Päis on nähtaval arv kirjeid eraldi log otsingupäringuid kahe arvu.  Loend kuvatakse graafik arvuveerg või selle muutmine aja jooksul suhteline väärtus, mis näitab, kus ülemise kümme päringu tulemusi. |
| [Rõngas ja loend](#donut-amp-list-part) | Päis kuvatakse summeeritud väärtus veerus log päringu ühe arv.  Funktsiooni rõngas graafiliselt kuvab tulemusi ülemised kolme kirja. |
| [Kahe ajaskaalade ja loend](#two-timelines-amp-list-part) | Päis kuvatakse kahe logi päringu tulemused aja jooksul tulpdiagrammid koos viikteksti, kuvades väärtuse veerus log päringu summeeritud ühe arvu.  Loend kuvatakse graafik arvuveerg või selle muutmine aja jooksul suhteline väärtus, mis näitab, kus ülemise kümme päringu tulemusi. |   
| [Teave](#information-part) | Päis kuvatakse staatiliseks tekstiks ja mõni valikuline link.  Loend kuvatakse ühe või mitme üksuse tiitel ja staatiliseks tekstiks. |
| [Joondiagrammi, viikteksti ja loend](#line-chart-callout-amp-list-part) | Aja ja viikteksti summeeritud väärtusega kuvab päise joondiagrammi mitut andmesarja Logi päringu abil.  Loend kuvatakse graafik arvuveerg või selle muutmine aja jooksul suhteline väärtus, mis näitab, kus ülemise kümme päringu tulemusi. |
| [Joondiagramm ja loend](#line-chart-amp-list-part) | Päis kuvatakse koos mitut andmesarja Logi päringu joondiagramm aja jooksul.  Loend kuvatakse graafik arvuveerg või selle muutmine aja jooksul suhteline väärtus, mis näitab, kus ülemise kümme päringu tulemusi. |
| [Rea diagrammide osa virnas](#stack-of-line-charts-part) | Kuvab kolm eraldi Joondiagrammid mitut andmesarja päringust Logi koos aja jooksul. |


## <a name="list-of-queries-part"></a>Päringute osas

Kuvatakse loend log otsingupäringuid.  Kasutaja klõpsata iga päringu tulemuste kuvamiseks.  Vaate sisaldab vaikimisi ühe päringu ja võite klõpsata nuppu **+ päringu** lisada täiendavad päringud.

![Päringute vaade loend](media/log-analytics-view-designer/view-list-queries.png)

| Säte | Kirjeldus |
|:--|:--|
| **Üldine** |
| Pealkiri | Vaate ülaosas kuvatav tekst. |
| Uus rühm | Saate luua uue rühma alates praeguse vaate vaates. |
| Eelnevalt valitud filtrid | Komaga eraldatud loend filter vasakul paanil kaasata, kui kasutaja valib päringu atribuudid. |
| Renderdada režiim | Esialgne vaade kuvatakse, kui päring on valitud.  Kasutaja saab valida kõik saadaolevad vaated pärast päringu avamist. |
| **Päringud** |
| Otsingupäringu | Päringu käivitamiseks. |
| Sõbralik nimi | Päringu kuvamiseks kasutajale kirjeldav nimi. |


## <a name="number--list-part"></a>Arvu ja loendi veebiosa

Päis on ühe arvu kuvav count log otsingupäringu kirjeid.  Loend kuvatakse graafik arvuveerg või selle muutmine aja jooksul suhteline väärtus, mis näitab, kus ülemise kümme päringu tulemusi.


![Päringute vaade loend](media/log-analytics-view-designer/view-number-list.png)

| Säte | Kirjeldus |
|:--|:--|
| **Üldine** |
| Rühma tiitel | Vaate ülaosas kuvatav tekst. |
| Uus rühm | Saate luua uue rühma alates praeguse vaate vaates. |
| Ikoon | Pildifaili kõrval päises tulemi kuvamiseks.
| Kasutage ikoon | Valige ikoon kuvada. |
| **Pealkiri** |
| Legendi | Päise ülaosas kuvatav tekst. |
| Päringu | Päringu käivitamiseks päise.  Kuvatakse päringu tagastatud kirjete arvu. |
| **Loend** |
| Päringu | Päringu käivitamiseks loendi.  Kuvatakse kaks esimest atribuudid tulemuste esimesed kümme kirjete jaoks.  Esimene peaks olema tekstväärtuse ja teine atribuudi arvulise väärtuse.  Ribad luuakse automaatselt arvväärtusega veeru suhtelise väärtuse.<br><br>Kirjete loendi sortimiseks kasutada päringus käsku Sordi.  Kasutaja saab nuppu Vaata kõiki päringu käivitamiseks ja tagastab kõik kirjed. |
| Graafik peitmine | Valige keelamiseks graafik arvuline veerg paremal. |
| Minigraafika lubamine | Minigraafika asemel horisontaalriba kuvamiseks valige.  Üksikasjad leiate [Levinud sätted](#sparklines) . |
| Värvi | Ribad või minigraafika värvi. |
| Nime ja väärtuse eraldaja | Ühe märgi eraldaja, kui soovite teksti atribuudi sõeluda mitu väärtust.  Üksikasjad leiate [Levinud sätted](#name-value-separator) . |
| Päringu navigeerimine | Päringu käivitamiseks, kui kasutaja valib üksuse loendis.  Üksikasjad leiate [Levinud sätted](#navigation-query) . |
| **Loend** | **> Veerutiitleid** |
| Nimi | Loendi esimese veeru ülaosas kuvatav tekst. |
| Väärtus | Teises veerus loendi ülaosas kuvatav tekst. |
| **Loend** | **> Piirmäärad** |
| Luba piirmäärad | Valige luba lävede.  Üksikasjad leiate [Levinud sätted](#thresholds) . |


## <a name="two-numbers--list-part"></a>Kahe arvu ja Loendivaate veebiosa

Päis on nähtaval arv kirjeid eraldi log otsingupäringuid kahe arvu.  Loend kuvatakse graafik arvuveerg või selle muutmine aja jooksul suhteline väärtus, mis näitab, kus ülemise kümme päringu tulemusi.

![Kahe arvu ja loendivaates](media/log-analytics-view-designer/view-two-numbers-list.png)

| Säte | Kirjeldus |
|:--|:--|
| **Üldine** |
| Rühma tiitel | Vaate ülaosas kuvatav tekst. |
| Uus rühm | Saate luua uue rühma alates praeguse vaate vaates. |
| Ikoon | Pildifaili kõrval päises tulemi kuvamiseks.
| Kasutage ikoon | Valige ikoon kuvada. |
| **Pealkiri** |
| Legendi | Päise ülaosas kuvatav tekst. |
| Päringu | Päringu käivitamiseks päise.  Kuvatakse päringu tagastatud kirjete arvu. |
| **Loend** |
| Päringu | Päringu käivitamiseks loendi.  Kuvatakse kaks esimest atribuudid tulemuste esimesed kümme kirjete jaoks.  Esimene peaks olema tekstväärtuse ja teine atribuudi arvulise väärtuse.  Ribad luuakse automaatselt arvväärtusega veeru suhtelise väärtuse.<br><br>Kirjete loendi sortimiseks kasutada päringus käsku Sordi.  Kasutaja saab nuppu Vaata kõiki päringu käivitamiseks ja tagastab kõik kirjed. |
| Graafik peitmine | Valige keelamiseks graafik arvuline veerg paremal. |
| Minigraafika lubamine | Minigraafika asemel horisontaalriba kuvamiseks valige.  Üksikasjad leiate [Levinud sätted](#sparklines) . |
| Värvi | Ribad või minigraafika värvi. |
| Toiming | Toimingu teostamine minigraafikat.  Üksikasjad leiate [Levinud sätted](#sparklines) . |
| Nime ja väärtuse eraldaja | Ühe märgi eraldaja, kui soovite teksti atribuudi sõeluda mitu väärtust.  Üksikasjad leiate [Levinud sätted](#name-value-separator) . |
| Päringu navigeerimine | Päringu käivitamiseks, kui kasutaja valib üksuse loendis.  Üksikasjad leiate [Levinud sätted](#navigation-query) . |
| **Loend** | **> Veerutiitleid** |
| Nimi | Loendi esimese veeru ülaosas kuvatav tekst. |
| Väärtus | Teises veerus loendi ülaosas kuvatav tekst. |
| **Loend** | **> Piirmäärad** |
| Luba piirmäärad | Valige luba lävede.  Üksikasjad leiate [Levinud sätted](#thresholds) . |

## <a name="donut--list-part"></a>Rõngas ja loendi veebiosa

Päis kuvatakse summeeritud väärtus veerus log päringu ühe arv.  Funktsiooni rõngas graafiliselt kuvab tulemusi ülemised kolme kirja.

![Rõngas ja loendi kuvamine](media/log-analytics-view-designer/view-donut-list.png)

| Säte | Kirjeldus |
|:--|:--|
| **Üldine** |
| Rühma tiitel | Paani ülaosas kuvatav tekst. |
| Uus rühm | Saate luua uue rühma alates praeguse vaate vaates. |
| Ikoon | Pildifaili kõrval päises tulemi kuvamiseks. |
| Kasutage ikoon | Valige ikoon kuvada. |
| **Päis** |
| Pealkiri | Päise ülaosas kuvatav tekst.
| Alapealkiri | Jaotise päis ülaosas kuvatav tekst.
| **Rõngas** |
| Päringu | Päringu käivitamine on rõngas.  Esimene peaks olema tekstväärtuse ja teine atribuudi arvulise väärtuse. |
| **Rõngas** |  **> Keskele** |
| Teksti | Jaotises väärtus sees on rõngas kuvatav tekst. |
| Toiming | Selle toimingu sooritamiseks summeeritavaid on üks väärtus atribuudi väärtus.<br><br>-Summa: Kõik kirjed väärtuste liitmiseks.<br>-Protsent: Protsent tagastatud väärtused **tulemus väärtused kasutatud keskmist toiming** kokku kirjete päringu kirjed. |
| Tulemus väärtused kasutatud keskmist toiming | Soovi korral klõpsake plussmärki, et lisada ühe või mitu väärtust.  Päringu tulemused piirduvad määrate atribuudi väärtustega kirjed.  Kui ei sisalda väärtusi on lisatud, on kõik kirjed päringu kaasatud. |
| **Lisavõimalused** | **> Värvid** |
| Värvi 1<br>Värvi 2<br>Värvi 3 | Iga soovitud rõngas kuvatud väärtuste jaoks valige soovitud värv. |
| **Lisavõimalused** | **> Täpsemad värvi kaardistamine** |
| Välja väärtus | Tippige kuvamiseks eri värvi, kui see on kaasatud on rõngas välja nimi. |
| Värvi | Valige soovitud värv kordumatu välja. |
| **Loend** |
| Päringu | Päringu käivitamiseks loendi.  Kuvatakse päringu tagastatud kirjete arvu. |
| Graafik peitmine | Valige keelamiseks graafik arvuline veerg paremal. |
| Minigraafika lubamine | Minigraafika asemel horisontaalriba kuvamiseks valige.  Üksikasjad leiate [Levinud sätted](#sparklines) . |
| Värvi | Ribad või minigraafika värvi. |
| Toiming | Toimingu teostamine minigraafikat.  Üksikasjad leiate [Levinud sätted](#sparklines) . |
| Nime ja väärtuse eraldaja | Ühe märgi eraldaja, kui soovite teksti atribuudi sõeluda mitu väärtust.  Üksikasjad leiate [Levinud sätted](#name-value-separator) . |
| Päringu navigeerimine | Päringu käivitamiseks, kui kasutaja valib üksuse loendis.  Üksikasjad leiate [Levinud sätted](#navigation-query) . |
| **Loend** | **> Veerutiitleid** |
| Nimi | Loendi esimese veeru ülaosas kuvatav tekst. |
| Väärtus | Teises veerus loendi ülaosas kuvatav tekst. |
| **Loend** | **> Piirmäärad** |
| Luba piirmäärad | Valige luba lävede.  Üksikasjad leiate [Levinud sätted](#thresholds) . |

## <a name="two-timelines--list-part"></a>Kahe ajaskaalade ja loendi veebiosa

Päis kuvatakse kahe logi päringu tulemused aja jooksul tulpdiagrammid koos viikteksti, kuvades väärtuse veerus log päringu summeeritud ühe arvu.  Loend kuvatakse graafik arvuveerg või selle muutmine aja jooksul suhteline väärtus, mis näitab, kus ülemise kümme päringu tulemusi.

![Kahe ajaskaalade ja loendi kuvamine](media/log-analytics-view-designer/view-two-timelines-list.png)

| Säte | Kirjeldus |
|:--|:--|
| **Üldine** |
| Rühma tiitel | Paani ülaosas kuvatav tekst. |
| Uus rühm | Saate luua uue rühma alates praeguse vaate vaates. |
| Ikoon | Pildifaili kõrval päises tulemi kuvamiseks. |
| Kasutage ikoon | Valige ikoon kuvada. |
| **Esmalt diagramm<br>teine diagramm** |
| Legendi | Jaotises viikteksti esimese rea jaoks kuvatav tekst. |
| Värvi | Sarja veergude jaoks kasutatavat värvi. |
| Päringu | Päringu käivitamiseks esimese sarja.  Diagrammi tulpadele esindab iga ajavahemiku jooksul kirjete arvu. |
| Toiming | Selle toimingu sooritamiseks summeeritavaid üksikväärtus viikteksti jaoks atribuudi väärtus.<br><br>-Summa: Kõik kirjed alates väärtusest summa.<br>– Keskmise: Kõik kirjed alates väärtusest Keskmine.<br>-Viimase näidis: Kaasatud diagrammi viimase intervalli väärtus.<br>-Esimene näidis: Kaasatud diagrammi esimese intervalli väärtus.<br>-Count: Päringu tagastatavad kõigi kirjete arvu.|
| **Loend** |
| Päringu | Päringu käivitamiseks loendi.  Kuvatakse päringu tagastatud kirjete arvu. |
| Graafik peitmine | Valige keelamiseks graafik arvuline veerg paremal. |
| Minigraafika lubamine | Minigraafika asemel horisontaalriba kuvamiseks valige.  Üksikasjad leiate [Levinud sätted](#sparklines) . |
| Värvi | Ribad või minigraafika värvi. |
| Toiming | Toimingu teostamine minigraafikat.  Üksikasjad leiate [Levinud sätted](#sparklines) . |
| Päringu navigeerimine | Päringu käivitamiseks, kui kasutaja valib üksuse loendis.  Üksikasjad leiate [Levinud sätted](#navigation-query) . |
| **Loend** | **> Veerutiitleid** |
| Nimi | Loendi esimese veeru ülaosas kuvatav tekst. |
| Väärtus | Teises veerus loendi ülaosas kuvatav tekst. |
| **Loend** | **> Piirmäärad** |
| Luba piirmäärad | Valige luba lävede.  Üksikasjad leiate [Levinud sätted](#thresholds) . |

## <a name="information-part"></a>Teavet osa

Päis kuvatakse staatiliseks tekstiks ja mõni valikuline link.  Loend kuvatakse ühe või mitme üksuse tiitel ja staatiliseks tekstiks.

![Teabe kuvamine](media/log-analytics-view-designer/view-information.png)

| Säte | Kirjeldus |
|:--|:--|
| **Üldine** |
| Rühma tiitel | Paani ülaosas kuvatav tekst. |
| Uus rühm | Saate luua uue rühma alates praeguse vaate vaates. |
| Värvi | Päise taustavärvi. |
| **Päis** |
| Pilt | Pildifaili päises kuvada. |
| Sildi | Klõpsake päises kuvatav tekst. |
| **Päis** | **> Link** |
| Sildi | Lingi tekst. |
| URL-i | Lingi URL. |
| **Teavet üksused** |
| Pealkiri | Pealkirja iga üksuse jaoks kuvatav tekst. |
| Sisu | Iga üksuse jaoks kuvatav tekst. |


## <a name="line-chart-callout--list-part"></a>Joondiagrammi, viikteksti ja Loendivaate veebiosa

Aja ja viikteksti summeeritud väärtusega kuvab päise joondiagrammi mitut andmesarja Logi päringu abil.  Loend kuvatakse graafik arvuveerg või selle muutmine aja jooksul suhteline väärtus, mis näitab, kus ülemise kümme päringu tulemusi.

![Joondiagrammi, viikteksti ja loendivaates](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Säte | Kirjeldus |
|:--|:--|
| **Üldine** |
| Rühma tiitel | Paani ülaosas kuvatav tekst. |
| Uus rühm | Saate luua uue rühma alates praeguse vaate vaates. |
| Ikoon | Pildifaili kõrval päises tulemi kuvamiseks. |
| Kasutage ikoon | Valige ikoon kuvada. |
| **Päis** |
| Pealkiri | Päise ülaosas kuvatav tekst. |
| Alapealkiri | Jaotise päis ülaosas kuvatav tekst. |
| **Joondiagramm** |
| Päringu | Päringu käivitamiseks joondiagrammil jaoks.  Esimene peaks olema tekstväärtuse ja teine atribuudi arvulise väärtuse.  See on tavaliselt päringu, mis kasutab **mõõt** märksõna summeeritavaid tulemused.  Kui päring kasutab seda **intervalli** märksõna siis diagrammi x-telje kasutab seda ajavahemik.  Kui päring ei sisalda **intervall** märksõna siis tundide järel kasutatakse x-telje. |
| **Joondiagramm** | **> Viiktekst** |
| Viikteksti tiitel | Viikteksti väärtus ülaltoodud kuvatav tekst. |
| Sarja nimi | Atribuudi väärtus sarja viikteksti väärtuse jaoks kasutada.  Kui ühtegi sarja, kasutatakse päringu kõik kirjed. |
| Toiming | Selle toimingu sooritamiseks summeeritavaid üksikväärtus viikteksti jaoks atribuudi väärtus.<br><br>– Keskmise: Kõik kirjed alates väärtusest Keskmine.<br>-Päringu tagastatavad Count kõigi kirjete arv.<br>-Viimase näidis: Kaasatud diagrammi viimase intervalli väärtus.<br>-Max: Kaasatud diagrammi sagedus maksimum väärtus.<br>-Min: Kaasatud diagrammi sagedus väikseim väärtus.<br>-Summa: Kõik kirjed alates väärtusest summa. |
| **Joondiagramm** | **> Y-telg** |
| Kasutage logaritmilist skaalat | Valige y-telje muuta logaritmiliseks skaalaks kasutamiseks. |
| Ühikud | Määrake päringu tagastatud väärtused mõõtühikud.  Seda teavet kasutatakse siltide kuvamiseks Failitüübid väärtust, mis näitab, diagrammi ja soovi korral teisendamise väärtused.  Üksuse tüüp määrab üksuse kategooria ja määratleb praeguse üksuse tüüp väärtused, mis on saadaval.  Kui valite väärtuse teisendamine siis praeguse üksuse tüüp, millest teisendatakse arvväärtuste tippige Teisenda. |
| Kohandatud sildi | Y telje silt üksuse tüüp kõrval kuvatav tekst.  Kui silti pole määratud, kuvatakse ainult üksuse tüüp. |
| **Loend** |
| Päringu | Päringu käivitamiseks loendi.  Kuvatakse päringu tagastatud kirjete arvu. |
| Graafik peitmine | Valige keelamiseks graafik arvuline veerg paremal. |
| Minigraafika lubamine | Minigraafika asemel horisontaalriba kuvamiseks valige.  Üksikasjad leiate [Levinud sätted](#sparklines) . |
| Värvi | Ribad või minigraafika värvi. |
| Toiming | Toimingu teostamine minigraafikat.  Üksikasjad leiate [Levinud sätted](#sparklines) . |
| Nime ja väärtuse eraldaja | Ühe märgi eraldaja, kui soovite teksti atribuudi sõeluda mitu väärtust.  Üksikasjad leiate [Levinud sätted](#name-value-separator) . |
| Päringu navigeerimine | Päringu käivitamiseks, kui kasutaja valib üksuse loendis.  Üksikasjad leiate [Levinud sätted](#navigation-query) . |
| **Loend** | **> Veerutiitleid** |
| Nimi | Loendi esimese veeru ülaosas kuvatav tekst. |
| Väärtus | Teises veerus loendi ülaosas kuvatav tekst. |
| **Loend** | **> Piirmäärad** |
| Luba piirmäärad | Valige luba lävede.  Üksikasjad leiate [Levinud sätted](#thresholds) . |

## <a name="line-chart--list-part"></a>Rea loendi & Diagrammi veebiosa

Päis kuvatakse koos mitut andmesarja Logi päringu joondiagramm aja jooksul.  Loend kuvatakse graafik arvuveerg või selle muutmine aja jooksul suhteline väärtus, mis näitab, kus ülemise kümme päringu tulemusi.

![Rea diagrammi ja loendi kuvamine](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Säte | Kirjeldus |
|:--|:--|
| **Üldine** |
| Rühma tiitel | Paani ülaosas kuvatav tekst. |
| Uus rühm | Saate luua uue rühma alates praeguse vaate vaates. |
| Ikoon | Pildifaili kõrval päises tulemi kuvamiseks. |
| Kasutage ikoon | Valige ikoon kuvada. |
| **Päis** |
| Pealkiri | Päise ülaosas kuvatav tekst. |
| Alapealkiri | Jaotise päis ülaosas kuvatav tekst. |
| **Joondiagramm** |
| Päringu | Päringu käivitamiseks joondiagrammil jaoks.  Esimene peaks olema tekstväärtuse ja teine atribuudi arvulise väärtuse.  See on tavaliselt päringu, mis kasutab **mõõt** märksõna summeeritavaid tulemused.  Kui päring kasutab seda **intervalli** märksõna siis diagrammi x-telje kasutab seda ajavahemik.  Kui päring ei sisalda **intervall** märksõna siis tundide järel kasutatakse x-telje. |
| **Joondiagramm** | **> Y-telg** |
| Kasutage logaritmilist skaalat | Valige y-telje muuta logaritmiliseks skaalaks kasutamiseks. |
| Ühikud | Määrake päringu tagastatud väärtused mõõtühikud.  Seda teavet kasutatakse siltide kuvamiseks Failitüübid väärtust, mis näitab, diagrammi ja soovi korral teisendamise väärtused.  Üksuse tüüp määrab üksuse kategooria ja määratleb praeguse üksuse tüüp väärtused, mis on saadaval.  Kui valite väärtuse teisendamine siis praeguse üksuse tüüp, millest teisendatakse arvväärtuste tippige Teisenda. |
| Kohandatud sildi | Y telje silt üksuse tüüp kõrval kuvatav tekst.  Kui silti pole määratud, kuvatakse ainult üksuse tüüp. |
| **Loend** |
| Päringu | Päringu käivitamiseks loendi.  Kuvatakse päringu tagastatud kirjete arvu. |
| Graafik peitmine | Valige keelamiseks graafik arvuline veerg paremal. |
| Minigraafika lubamine | Minigraafika asemel horisontaalriba kuvamiseks valige.  Üksikasjad leiate [Levinud sätted](#sparklines) . |
| Värvi | Ribad või minigraafika värvi. |
| Toiming | Toimingu teostamine minigraafikat.  Üksikasjad leiate [Levinud sätted](#sparklines) . |
| Nime ja väärtuse eraldaja | Ühe märgi eraldaja, kui soovite teksti atribuudi sõeluda mitu väärtust.  Üksikasjad leiate [Levinud sätted](#name-value-separator) . |
| Päringu navigeerimine | Päringu käivitamiseks, kui kasutaja valib üksuse loendis.  Üksikasjad leiate [Levinud sätted](#navigation-query) . |
| **Loend** | **> Veerutiitleid** |
| Nimi | Loendi esimese veeru ülaosas kuvatav tekst. |
| Väärtus | Teises veerus loendi ülaosas kuvatav tekst. |
| **Loend** | **> Piirmäärad** |
| Luba piirmäärad | Valige luba lävede.  Üksikasjad leiate [Levinud sätted](#thresholds) . |

## <a name="stack-of-line-charts-part"></a>Rea diagrammide osa virnas

Kuvab kolm eraldi Joondiagrammid mitut andmesarja päringust Logi koos aja jooksul.

![Joondiagrammide virnas](media/log-analytics-view-designer/view-stack-line-charts.png)

| Säte | Kirjeldus |
|:--|:--|
| **Üldine** |
| Rühma tiitel | Paani ülaosas kuvatav tekst. |
| Uus rühm | Saate luua uue rühma alates praeguse vaate vaates. |
| Ikoon | Pildifaili kõrval päises tulemi kuvamiseks. |
| **Diagramm 1<br>diagramm 2<br>3 diagramm** | **> Päis** |
| Pealkiri | Diagrammi ülaosas kuvatav tekst. |
| Alapealkiri | Diagrammi ülaosas pealkirja kuvatav tekst. |
| **Diagramm 1<br>diagramm 2<br>3 diagramm** | **Joondiagramm** |
| Päringu | Päringu käivitamiseks joondiagrammil jaoks.  Esimene peaks olema tekstväärtuse ja teine atribuudi arvulise väärtuse.  See on tavaliselt päringu, mis kasutab **mõõt** märksõna summeeritavaid tulemused.  Kui päring kasutab seda **intervalli** märksõna siis diagrammi x-telje kasutab seda ajavahemik.  Kui päring ei sisalda **intervall** märksõna siis tundide järel kasutatakse x-telje. |
| **Diagrammi** | **> Y-telg** |
| Kasutage logaritmilist skaalat | Valige y-telje muuta logaritmiliseks skaalaks kasutamiseks. |
| Ühikud | Määrake päringu tagastatud väärtused mõõtühikud.  Seda teavet kasutatakse siltide kuvamiseks Failitüübid väärtust, mis näitab, diagrammi ja soovi korral teisendamise väärtused.  Üksuse tüüp määrab üksuse kategooria ja määratleb praeguse üksuse tüüp väärtused, mis on saadaval.  Kui valite väärtuse teisendamine siis praeguse üksuse tüüp, millest teisendatakse arvväärtuste tippige Teisenda. |
| Kohandatud sildi | Y telje silt üksuse tüüp kõrval kuvatav tekst.  Kui silti pole määratud, kuvatakse ainult üksuse tüüp. |

## <a name="common-settings"></a>Levinud sätted
Järgmistes jaotistes kirjeldatakse mitu visualiseeringu osa ühised sätted.

### <a name="name-value-separator">Nime ja väärtuse eraldaja</a>
Kui soovite mitme väärtuste loendi päringu atribuudi teksti sõeluda üksikmärki eraldaja.  Kui määrate eraldaja, saate sisestada nimed iga välja eraldatud sama eraldaja väljale nimi.

Näiteks Kujutage nimega *asukohta* , mis sisaldab väärtusi, nt *Redmond suurendamine 41* ja *Bellevue-Building12*atribuut.  Saate määrata – nime ja väärtuse eraldaja ja *Linna hoone* nimi.  See oleks iga väärtuse sõeluda nimega *linn* ja *Building*kaks atribuudid. 

### <a name="navigation-query">Päringu navigeerimine</a>
Päringu käivitamiseks, kui kasutaja valib üksuse loendis.  *{Valitud üksuse}* abil saate lisada üksus, mille kasutaja valitud süntaks.

Näiteks kui päring on veerg nimega *arvuti* ja navigeerimise päring on *{valitud üksuse}*, päringu nagu *arvuti = "Minuarvuti"* läheks, kui kasutaja on valitud arvuti.  Kui navigeerimine päring on *Tüüp = {valitud üksuse} sündmuse* seejärel päringu *Tüüp = sündmuse arvuti = "Minuarvuti"* soovite käivitada.

### <a name="sparklines">Minigraafika</a>
Minigraafika on väike joondiagramm, mis kujutab loendi kirje väärtus aja jooksul.  Visualiseeringu osa loendiga, saate valida, kas kuvada horisontaalse riba tähistab suhteline väärtus arvuveerg või minigraafikusse, mis näitab selle väärtus aja jooksul.

Järgmises tabelis kirjeldatakse minigraafika sätteid.

| Säte | Kirjeldus |
|:--|:--|
| Minigraafika lubamine | Minigraafika asemel horisontaalriba kuvamiseks valige. |
| Toiming | Kui minigraafika on lubatud, on selle toimingu sooritamiseks iga atribuudi loendis minigraafikat väärtuste arvutamiseks.<br><br>-Viimase näide: Üle ajavahemik sarja viimase väärtuse.<br>-Max: Sarja üle ajavahemik maksimum.<br>-Min: Sarja üle ajavahemik väikseim väärtus.<br>-Summa: Üle ajavahemik sarja väärtuste summa.<br>-Kokkuvõte: Kasutab sama mõõt käsk päringu päises. |

### <a name="thresholds">Piirmäärad</a>
Lävede võimaldavad kuvamine värviline ikooni iga üksuse kõrval, võimaldades kiire visuaalne näidik üksused, mis ületavad kindla väärtuse või on teatud vahemikus.  Näiteks võib kuvada roheline ikoon on väärtus, kollane, kui väärtus on vahemikus, mis näitab hoiatus ja punane üksuste jaoks, kui see ületab veaväärtuse.

Kui lubate läved alumise osa, peate määrama ühe või mitme lävede.  Kui üksuse väärtus on suurem läve ja järgmise lävi väärtusest väiksem, kasutatakse seda värvi.  Kui üksus on suurem kui siis kõige läve, seatakse selle värvi.   

Lävi igas kogumis on üks lävi väärtus on **vaikimisi**.  See on värv, kui ei sisalda muid väärtusi on ületatud.  Saate lisada või eemaldada lävede, klõpsates soovitud **+** või nuppu **x** .

Järgmises tabelis kirjeldatakse piirmäärade sätteid.

| Säte | Kirjeldus |
|:--|:--|
| Luba piirmäärad | Valige vasakul iga väärtuse, mis näitab selle seisundi suhtes määratud lävede värvi ikooni kuvamiseks. |
| Nimi | Piirmäärast nimi. |
| Lävi | Piirmäära väärtus.  Iga loendiüksust värvi seisund on seatud värvi üksuse väärtuse alusel ületab läve kõrgeim väärtus.  On ühe vaikimisi piirmäära, mis on värv, kui on ületatud pole läviväärtuse liugureid. |
| Värvi | Piirmäärast värvi. |


## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [log otsingud](log-analytics-log-searches.md) päringute toetamiseks visualiseeringu osa.