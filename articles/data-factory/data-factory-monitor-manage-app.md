<properties 
    pageTitle="Jälgimine ja haldamine Azure'i andmed Factory torujuhtmete" 
    description="Saate teada, kuidas jälgimine ja haldamine Azure'i andmed tehased ja torujuhtmed jälgimine ja haldamine rakenduse abil." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="spelluru"/>

# <a name="monitor-and-manage-azure-data-factory-pipelines-using-new-monitoring-and-management-app"></a>Jälgimine ja haldamine Azure'i andmed Factory torujuhtmete uue jälgimine ja haldamine rakenduse abil
> [AZURE.SELECTOR]
- [Azure'i portaalis/Azure PowerShelli abil](data-factory-monitor-manage-pipelines.md)
- [Kasutades jälgimise ja halduse rakenduse](data-factory-monitor-manage-app.md)

Selles artiklis kirjeldatakse, kuidas jälgida, hallata ja oma torujuhtmete silumine ja luua teatisi, et saada teada, **jälgimine ja haldamine rakenduse**tõrked. Saate vaadata ka jälgimine ja haldamine rakenduse kasutamise kohta leiate järgmisest videost.
   

> [AZURE.VIDEO azure-data-factory-monitoring-and-managing-big-data-piplines]
      
## <a name="launching-the-monitoring-and-management-app-a"></a>Käivitamine, jälgimine ja haldamine rakenduse lisamine
Jälgimine ja haldamine rakenduse käivitamiseks klõpsake **jälgimine ja haldamine** paani **Andmete FACTORY** enne oma andmete Factory.

![Paani andmete Factory avalehel jälgimine](./media/data-factory-monitor-manage-app/MonitoringAppTile.png) 

Peaksite nägema jälgimine ja haldamine App käivitatud eraldi aknas menüü /.  

![Jälgimise ja halduse rakenduse](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [AZURE.NOTE] Kui näete, et veebibrauseri on ummikus "Lubatakse...", keelamine ja tühjendage ruut **Blokeeri kolmanda osapoole küpsised ja saidi andmed** säte (või) jätta lubatud ja **login.microsoftonline.com** erand luua ja proovige rakenduse uuesti käivitamist.


Kui tegevuse windows allosas loendis ei kuvata, klõpsake tööriistaribal loendi värskendamiseks nuppu **Värskenda** . Lisaks seada õige väärtused filtrite **alguskellaaeg** ja **lõppkellaaeg** .  


## <a name="understanding-the-monitoring-and-management-app"></a>Mõistmine jälgimise ja halduse rakenduse
Klõpsake vasakus servas on kolm vahekaarti (**Resource Explorer**, **Vaadete jälgimine**ja **teatised**) ja esimest vahekaarti (Resource Explorer) on vaikimisi valitud. 

### <a name="resource-explorer"></a>Ressursi Explorer
Näete järgmist: 

- Ressursi Exploreri **puuvaate** vasakul paanil.
- **Diagrammi vaate** ülaosas.
- **Tegevuste Windows** loendi allservas keskmisel paanil.
- **Atribuutide**/**Tegevuse akna Exploreri** vahekaardid parempoolsel paanil. 

Ressursi Exploreris, näete kõigi andmete factory puuvaates ressursside (torustike, andmekomplektide, lingitud teenused). Objekti valimisel Resource Explorer märkate järgmist: 

- seotud andmete Factory üksus on esile tõstetud diagrammivaates.
- seotud tegevuse windows (klõpsake [siin](data-factory-scheduling-and-execution.md) tegevuse windows kohta) on esile tõstetud tegevuse Windows loendi allservas.  
- valitud objekti atribuutide aknas parempoolsel paanil atribuudid. 
- Valitud objekti võimalusel JSON määratlus. Näide: lingitud teenuse või andmekomplekt või müügivõimaluste. 

![Ressursi Explorer](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Tegevuste aknas kontseptuaalne lisateabe saamiseks vt [plaanimine ja täitmise](data-factory-scheduling-and-execution.md) artikkel. 

### <a name="diagram-view"></a>Diagrammivaate
Diagrammi vaate andmete factory pakub ühe paani klaas jälgida ja andmete hankimise ja varade haldamine. Kui valite andmete Factory üksus (andmekomplekti/müügivõimaluste) diagrammivaates, märkate järgmist:
 
- andmete factory üksus on valitud puuvaates
- seotud tegevuse windows on esile tõstetud tegevuse Windows loendis.
- valitud objekti atribuutide aknas atribuudid

Kui tulemas on lubatud (not peatatud olekus), siis kuvatakse rohelise joonega. 

![Müügivõimaluste töötab](./media/data-factory-monitor-manage-app/PipelineRunning.png)

Märkate, et on kolm käsunuppu müügivõimaluste diagrammivaates jaoks. Teise nupu abil saate viige tulemas. Pausi lõpetada praegu töötab tegevuste ja temaga lõpetamise jätkata. Kolmas nupp peatab tulemas ja lõpeb oma olemasolevaid käivitamisel tegevusi. Esimene nuppu uuesti tulemas. Kui teie kohaletoimetamisel on peatatud, märkate värvi muutmine tulemas paani järgmiselt.

![Peata/Jätka paani](./media/data-factory-monitor-manage-app/SuspendResumeOnTile.png)

Saate mitme valimine kahe või enama torujuhtmete (kasutades CTRL) ja peata/Jätka mitme torujuhtmete korraga Käsuriba nuppude abil.

![Klõpsake käsku Peata/Jätka](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

Saate vaadata kõiki tegevusi ettevalmistamisel, paremklõpsates paani müügivõimaluste ja klõpsake nuppu **Ava kohaletoimetamisel**.

![Müügivõimaluste menüü Ava](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

Avatud müügivõimaluste vaates kuvatakse kõik tegevuste teel. Selles näites on ainult üks tegevuse: Kopeeri tegevus. Eelmisse vaatesse naasmiseks klõpsake andmete factory nime lingirea menüü ülaosas.

![Avatud müügivõimaluste](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

Müügivõimaluste vaates on väljundi andmekomplekti või millal te Viige hiirekursor väljundi andmekomplekti, klõpsamisel näete selle andmekogumi hüpikaknas tegevuse Windows.

![Tegevuste Windowsi hüpik](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

Võite klõpsata ka tegevuse akna **atribuutide** aknas parempoolsel paanil seda üksikasjade kuvamiseks. 

![Tegevuse aken Atribuudid](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

Parempoolsel paanil täpsema teabe kuvamiseks **Tegevuse akna Exploreri** vahekaardi aktiveerimine.

![Tegevuste aknas Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png) 

Kuvatakse ka **lahendatud muutujate** iga tegevuste käivitamine **katsete** jaotises katse. 

![Lahendatud muutujat](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Valitud objekti JSON skript definitsiooni näha **skript** vahekaardi aktiveerimine.   

![Skripti menüü](./media/data-factory-monitor-manage-app/ScriptTab.png)

Saate vaadata tegevuse windows kolmes kohas:

- Tegevuste Windowsi hüpik diagrammivaates (keskmisel paanil).
- Tegevuste aknas Explorer parempoolsel paanil.
- Tegevuste Windows loendi alumise paani.

Tegevuste Windows hüpikakende ja tegevuste aknas Explorer, saate kerida eelmise nädala ja järgmisel nädalal, vasak ja parem noolte abil.

![Tegevuste aknas Exploreri vasakule/paremale nooled](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

Diagrammi vaate allosas näete nuppu suumi sisse suumida välja Suum mahutamiseks, suumi 100%, lukustada paigutus. Paigutus nuppu Lukusta takistab kogemata teisaldamist tabelid ja torujuhtmete diagrammivaates ja on vaikimisi sees. Saate välja lülitada ja üksuste liikumine skeem. Kui lülitate selle välja, saate viimase nupp automaatselt paigutamiseks tabelid ja torujuhtmete. Saate ka suurendada / vähenda kerimisratta abil.

![Diagrammi vaate suurendamine käsud](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)


### <a name="activity-windows-list"></a>Tegevuste Windows loend
Tegevuste windows loendi allservas keskmisel paanil kuvatakse kõik tegevuse Windowsi jaoks valitud diagrammivaate või ressursside Exploreri andmekomplekti. Vaikimisi on loendis laskuvas järjestuses, mis tähendab, et näete uusima tegevuse akna ülaosas. 

![Tegevuste Windows loend](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Selles loendis automaatselt värskendada, seega kasutage nupp Värskenda tööriistaribal käsitsi värskendamiseks.  


Tegevuste windows võib olla üks järgmistest olekutest:

<table>
<tr>
    <th align="left">Olek</th><th align="left">SubStatus</th><th align="left">Kirjeldus</th>
</tr>
<tr>
    <td rowspan="8">Ootel</td><td>ScheduleTime</td><td>On aeg pole aken tegevuste käivitamiseks.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Varustava sõltuvused ei ole valmis.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Arvuta ressursid pole saadaval.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Kõik tegevuse eksemplarid on hõivatud muu tegevuse Windowsi opsüsteemi.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Tegevuste on peatatud ja ei saa käivitada windows tegevuse seni, kuni see jätkatakse.</td>
</tr>
<tr>
<td>Proovige</td><td>Tegevuste täitmise uuesti proovida.</td>
</tr>
<tr>
<td>Valideerimine</td><td>Valideerimine pole veel alanud.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Ootamine valideerimise abil uuesti proovida.</td>
</tr>
<tr>
<tr
<td rowspan="2">InProgress</td><td>Kontrollimine</td><td>Valideerimine on pooleli.</td>
</tr>
<td></td>
<td>Tegevuste akna töötlemise.</td>
</tr>
<tr>
<td rowspan="4">Nurjus.</td><td>Ajalõppe</td><td>Täitmise võttis oodatust, mis on lubatud tegevuse.</td>
</tr>
<tr>
<td>Tühistatud</td><td>Kasutaja toiming tühistada.</td>
</tr>
<tr>
<td>Valideerimine</td><td>Valideerimine nurjus.</td>
</tr>
<tr>
<td></td><td>Luua ja/või kinnitage tegevuse akna nurjus.</td>
</tr>
<td>Kas olete valmis</td><td></td><td>Tegevuste aken on kasutusvalmis.</td>
</tr>
<tr>
<td>Vahele</td><td></td><td>Tegevuste akna ei töödelda.</td>
</tr>
<tr>
<td>Ükski</td><td></td><td>Tegevuste aknas, mida kasutada mõne muu seisundiga on olemas, kuid on lähtestatud.</td>
</tr>
</table>


Kui klõpsate loendis soovitud tegevuse aknas, näete seda üksikasjad, **Atribuudid** või **Tegevuse Windows Exploreri** aknas paremal.

![Tegevuste aknas Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Tegevuste Windowsi värskendamine  
Üksikasjad on automaatselt värskendatud, et kasutada **värskendamine** (teine nupp) tegevuse windows loendi käsitsi värskendamiseks käsuribal nuppu.  
 

### <a name="properties-window"></a>Aken Atribuudid
Aken Atribuudid on kõige parempoolsele paanil rakenduse jälgimine ja haldamine. 

![Aken Atribuudid](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Ressursi explorer (puuvaade) (või) diagrammi vaade (või) tegevuse windows loendis valitud üksuse atribuutide kuvamine 

### <a name="activity-window-explorer"></a>Tegevuste aknas Explorer

**Tegevuste aknas Exploreri** aknas on kõige parempoolsele paanil jälgimine ja haldamine App. Tegevuste Windows hüpikakende või tegevuse Windows loendist valitud tegevuse akna üksikasjade kuvamine 

![Tegevuste aknas Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

Mida saab vahetada mõne muu tegevuse akna ülaosas kalendrivaates klõpsates. Saate kasutada ka **vasaknool**/**paremnool** nuppude kohal näha tegevuse Windowsi eelmine/järgmine nädal.

Saate kasutada tööriistariba nupud alumise paani **uuesti** tegevuse akna või **värskendamine** üksikasjade paanil. 

### <a name="script"></a>Skripti 
**Skripti** vahekaardil JSON määratluse andmete Factory valitud üksuse (lingitud teenuse andmekomplekti ja müügivõimaluste) kuvamiseks. 

![Skripti menüü](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="using-system-views"></a>Süsteemi vaadete kasutamine
Jälgimine ja haldamine App sisaldab valmismallide süsteemi vaateid (**viimaste tegevuse windows**, **nurjunud tegevuse windows**, **Pooleli tegevuse windows**), mis võimaldab teil vaadata oma andmeid Factory tehtud/nurjus/pooleli tegevuse windows. 

Vasakul **Jälgimise vaadete** vahekaardi aktiveerimine klõpsake seda. 

![Vaadete menüü jälgimine](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Praegu on kolm süsteemi vaated toetatud. Valige suvand näha tehtud tegevuse windows (või) nurjunud tegevuse Windowsi (või) pooleli tegevuse Windowsi tegevuse Windows loendis (keskmisel paanil allosas). 

**Tehtud tegevuse windows** suvandi valimisel kuvatakse kõik tehtud tegevuse windows laskuvas järjestuses, **viimase katse ajal**. 

Saate kõik nurjunud tegevuse windows loendi kuvamiseks **nurjunud tegevuse windows** vaade. Valige loendis, et näha üksikasjalikku teavet selle **atribuutide** aknas (või) **Tegevuse akna Exploreri**nurjunud tegevuse akna. Samuti saate alla laadida mis tahes logid nurjunud tegevuse akna. 


## <a name="sorting-and-filtering-activity-windows"></a>Sortimine ja filtreerimine tegevuse windows
Filtri tegevuse Windowsi käsuriba **alguskellaaeg** ja **lõppkellaaeg** sätete muutmine Kui olete muutnud alguskellaaeg ja lõppkellaaeg, kõrval nuppu lõppkellaaeg tegevuse Windows loendi värskendamiseks.

![Algus- ja lõppaeg](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [AZURE.NOTE] Praegu on kogu aeg jälgimine ja haldamine rakenduse UTC-vormingus. 

**Tegevuste Windowsi loendis**, klõpsake veeru nimi (nt: olek). 

![Tegevuste Windows loendi veeru menüü](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

Saate teha järgmist:

- Sordi tõusvas järjestuses.
- Sordi laskuvas järjestuses.
- Ühe või mitme väärtusega (valmis, ootel, jne) alusel filtreerimine

Kui määrate filtri veergu, näete nuppu filter selle veeru väärtuste veerg on filtreeritud väärtuste jaoks lubatud. 

![Veerus tegevuse Windows loendi filtreerimine](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

Saate sama hüpikakna tühjendamise. Kõik filtrid tegevuse windows loendi tühjendamiseks klõpsake käsuribal nuppu Tühjenda filter. 

![Tühjendage kõik filtrid tegevuse Windows loendis](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)


## <a name="performing-batch-actions"></a>Paketi toimingute

### <a name="rerun-selected-activity-windows"></a>Käivitage uuesti valitud tegevuse windows
Valige soovitud tegevus aken, klõpsake allanoolt esimese käsunupu riba ja valige **Käivita uuesti** / **uuesti koos enne kohaletoimetamisel**. Kui valite suvandi **abil uuesti enne teel** , see reruns kõik ka varustava tegevuse windows. 
    ![Käivitage uuesti aken soovitud tegevus](./media/data-factory-monitor-manage-app/ReRunSlice.png)

Samuti saate valida mitme tegevuse windows loendis ja uuesti, samal ajal. Soovite filtreerida tegevuse windows oleku põhjal (nt: **Failed**) ja seejärel käivitage uuesti windows nurjunud tegevuse pärast tegevuse windows nurjumise põhjustava probleemi lahendamist. Järgmised jaotisest filtreerimine tegevuse windows loendi üksikasjad.  

### <a name="pauseresume-multiple-pipelines"></a>Peata/Jätka mitme torujuhtmete
Saate mitme valimine kahe või enama torujuhtmete (kasutades CTRL) ja peata/Jätka neid korraga käsuriba nupud (esile tõstetud punane ristkülik järgmisel pildil) abil.

![Peatada/jätkata käsuriba](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="creating-alerts"></a>Teatiste loomine 
Teatiste lehel saate teatise, vaade/kustutage olemasolev teatiste loomine. Te saate ka/Keela teatise. Lehe teatiste vaatamiseks klõpsake vahekaarti teatised.

![Vahekaarti teatised](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a>Teatise loomine

1. Klõpsake nuppu **Lisa teate** teatise lisada. Kuvatakse lehel üksikasjad. 

    ![Teatiste - üksikasjade lehe loomine](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
1. Määrake **nimi** ja **Kirjeldus** teatise jaoks, ja klõpsake nuppu **edasi**. Peaksite nägema **filtrid** lehele.

    ![Teatiste - filtrid lehe loomine](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)

2. Valige **sündmus**, **olek**ja **substatus** (valikuline) soovitud andmed Factory teenuse märku, ja klõpsake nuppu **edasi**. Peaksite nägema **adressaatide** lehe.

    ![Teatiste - adressaatide lehe loomine](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png) 
3. Valige suvand **e-posti tellimuse administraatorid** ja/või sisestage **täiendavad administraatori e-posti**ja klõpsake nuppu **valmis**. Peaksite nägema teatise loendis. 
    
    ![Teatiste loend](./media/data-factory-monitor-manage-app/AlertsList.png)

Teatiste loendis nuppudega seostatud hoiatusteate Redigeeri/Kustuta / / Keela teatise. 

### <a name="eventstatussubstatus"></a>Sündmuse/olek/substatus
Järgmises tabelis on ära toodud loendis Saadaolevad sündmused ja olekuid (ja substatuses).

Sündmuse nimi | Olek | Sub olek
-------------- | ------ | ----------
Tegevuste käivitamine alustamine | Alustamine | Käivitamine
Tegevuste käivitamine lõpetanud | Õnnestus | Õnnestus 
Tegevuste käivitamine lõpetanud | Nurjus.| Nurjunud ressursi eraldus<br/><br/>Nurjunud täitmise<br/><br/>Aegus<br/><br/>Valideerimine nurjus<br/><br/>Hüljatud
Nõudmisel HDI kobar loomine alustamine | Alustamine | &nbsp; |
Nõudmisel HDI kobar loodud | Õnnestus | &nbsp; |
Kustutatud nõudmisel HDI kobar | Õnnestus | &nbsp; |
### <a name="to-editdeletedisable-an-alert"></a>Redigeeri/Kustuta/keelata teatise


![Teatiste nupud](./media/data-factory-monitor-manage-app/AlertButtons.png)



    
 


