<properties
   pageTitle="Power BI Log Analytics andmete eksportimine | Microsoft Azure'i"
   description="Power BI on pilvepõhine business analytics teenus Microsoft, mis sisaldab erinevaid andmete analüüsimiseks rikkalike visualiseeringute ja aruandeid.  Log Analytics saate pidevalt ekspordi andmed OMS hoidla Power BI nii, et saate kasutada oma visualiseerimine ja andmeanalüüsi tööriistad.  Selles artiklis kirjeldatakse, kuidas konfigureerida päringute Logi Analytics, mis automaatselt eksportimine Power BI intervalliga."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="export-log-analytics-data-to-power-bi"></a>Power BI Log Analytics andmete eksportimine

[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) on pilvepõhine business analytics teenus Microsoft, mis sisaldab erinevaid andmete analüüsimiseks rikkalike visualiseeringute ja aruandeid.  Log Analytics saab automaatselt ekspordi andmed OMS hoidla Power BI nii, et saate kasutada oma visualiseerimine ja andmeanalüüsi tööriistad.

Power BI konfigureerimisel Log Analytics loote Logi päringud, mis nende tulemite eksportimine vastavate andmekomplektide Power BI.  Päringu ja ekspordi endiselt automaatselt käivitada ajakava määratlevad ajakohastamine andmekomplekti Log Analytics kogutud uute andmetega.

![Log Analytics Power BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Power BI ajakava

*Power BI ajakava* sisaldab log otsing, mis ekspordib andmed OMS hoidla vastavate andmekomplekti Power BI ja ajakava, mis määratleb, kui sageli käivitatakse andmekomplekti ajakohastage see otsing.

Andmekomplekti väljad vastavad kirjed Logi otsinguga atribuutide.  Kui otsing tagastab kirjed eri tüüpi seejärel andmekomplekti kaasatakse kõik iga kaasatud kirjetüüpide atribuudid.  

> [AZURE.NOTE] See on parim kasutada log otsingu päring, mis tagastab toorandmetega asemel läbimiseks konsolideerimise näiteks [mõõt](log-analytics-search-reference.md#measure)käskude abil.  Saate teha mõnda liitmise ja arvutused Power BI on lähteandmed.

## <a name="connecting-oms-workspace-to-power-bi"></a>Power BI OMS tööruumi ühenduse

Enne kui saate eksportida Logi Analytics Power BI, tuleb ühendada OMS tööruumi Power BI kontole juurdepääsemine järgmist.  

1. Klõpsake paani **sätted** OMS konsooli.
2. Valige **kontod**.
3. Klõpsake jaotise **Tööruumi teabe** **Power BI kontoga ühenduse loomine**.
4. Sisestage mandaat Power BI konto jaoks.

## <a name="create-a-power-bi-schedule"></a>Power BI ajakava loomine

Luua Power BI ajakava iga andmekomplekti järgmiste toimingute abil.

1. Klõpsake OMS konsooli **Log otsingu** paani.
2. Tippige uue päringu või valige salvestatud otsingu, mis tagastab andmed, mida soovite eksportida **Power**bi.  
3. **Power BI** dialoogiboksi avamiseks lehe ülaosas nuppu **Power BI** .
4. Järgmises tabelis teave ja klõpsake nuppu **Salvesta**.

| Atribuut | Kirjeldus |
|:--|:--|
| Nimi | Ajasta kindlaks teha, kui kuvate Power BI ajakava loendi nimi. |
| Salvestatud otsingu | Log otsingu käivitamiseks.  Saate valida praeguse päringu või valige olemasoleva salvestatud otsingu rippmenüüst. |
| Ajakava | Kui tihti käivitate salvestatud otsingu ja Power BI andmekomplekti eksportimine.  Väärtus peab olema 15 minutit kuni 24 tundi. |
| Andmekomplekti nimi | Power BI andmekomplekti nimi.  See luuakse siis, kui seda pole ja värskendada, kui see on olemas. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Vaatamine ja Power BI ajakavasid eemaldamine

Saate vaadata olemasolevaid Power BI ajakavasid järgmist loendit.

1. Klõpsake paani **sätted** OMS konsooli.
2. Valige **Power BI**.

Lisaks Ajasta üksikasju, kuvatakse mitu korda, viimase nädala käivitatud ajakava ja viimase sünkroonimise olek.  Kui sünkroonimine ilmnenud tõrkeid, võite klõpsata linki log otsingu kirjete töötada tõrke üksikasjad.

**Eemaldage veerg** **X** klõpsates saate eemaldada ajakava.  Saate keelata, valides **välja**ajakava.  Ajakava muutmiseks peate selle eemaldada ja looge see uuesti uued sätted.

![Power BI ajakava](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Valimi kiirtutvustus
Järgmises jaotises antakse ülevaade olulisematest punktidest Power BI ajakava loomine ja selle andmehulga abil saate luua lihtsa aruande näide.  Selles näites arvutite jõudluse kõik andmed on eksporditud Power BI ja seejärel joone graafiku luuakse kuvamiseks protsessori kasutamine.

### <a name="create-log-search"></a>Log otsingu loomine
Alustame luues log soovime saata andmekomplekti andmeid otsida.  Selles näites kasutame päring, mis tagastab kõik jõudlusandmeid arvutite puhul nimi, mis algab *srv*.  

![Power BI ajakavasid](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Power BI otsinguterminite loomine
Me nuppu **Power BI** avada dialoogiboksi Power BI ja sisestage nõutav teave.  Soovime selle otsingu käivitamiseks üks kord tunnis ja luua nimega *Contoso täiuslik*Andmekomplekt.  Kuna meil juba on otsing avamine, mis loob andmete soovime, soovime säilitada vaikimisi *kasutada* otsingu praeguse **Salvestatud otsingu**jaoks.

![Power BI otsinguterminite](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Power BI otsinguterminite kinnitamine
Veenduge, et saaksime Ajasta õigesti loomise, me vaadata Power BI otsinguid loend jaotises **sätted** paani OMS armatuurlaud.  Me oodake mõni minut ja värskendage seda vaadet seni, kuni see aru, et sünkroonimine käivitada.

![Power BI otsinguterminite](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a>Power BI andmekomplekti kinnitamine
Me [powerbi.microsoft.com](http://powerbi.microsoft.com/) ja liikuge **andmekomplektide** allosas vasakul paanil meie kontosse sisse logida.  Me näeme, et *Contoso täiuslik* andmekomplekti on loetletud, mis näitab, meie ekspordi käivitada edukalt.

![Power BI andmekomplekti](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Andmekomplekti põhjal aruande loomine
Me valige **Contoso täiuslik** andmekomplekti ja seejärel klõpsake **tulemite** vaatamiseks väljad, mis on osa sellest andmekomplekti paremal paanil **väljad** .  Näitab iga arvuti protsessori kasutamine rea diagrammi loomiseks soovime järgmisi toiminguid.

1. Valige rida diagrammi visualiseering.
2. Lohistage **ObjectName** **taseme Aruandefiltrid** ja märkige ruut **protsessor**.
3. Lohistage **CounterName** **taseme Aruandefiltrid** ja märkige ruut **% protsessori aja**.
4. Lohistage **põhimõttelinegi** **väärtused**.
5. Lohistage **arvuti** **Legend**.
6. Lohistage **TimeGenerated** **telg**.

Me näeme, et tulemiks oleva rea graafik kuvatakse meie andmekomplekti andmetega.

![Power BI rea graafik](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a>Aruande salvestamine
Me salvestate aruande, klõpsates ekraani ülaosas nuppu Salvesta ja kinnitage on nüüd loetletud vasakul paanil jaotises aruanded.

![Power BI aruanded](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [log otsingute](log-analytics-log-searches.md) koostamiseks suhtes, mida saab eksportida Power BI.
- Lugege lisateavet [Power BI](http://powerbi.microsoft.com) visualiseeringute Log Analytics ekspordi põhjal koostada.
