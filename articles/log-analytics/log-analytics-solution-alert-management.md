<properties
   pageTitle="Lahendus toimingud halduse komplekti (OMS) Teavita | Microsoft Azure'i"
   description="Log Analytics lahendus teatiste haldus aitab teil analüüsida kõiki teatisi teie keskkonnas.  Lisaks konsolideerimisele sees OMS teatised, see impordib teatiste ühendatud süsteemi Center toimingute Manager (SCOM) halduse rühmade Log Analytics."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/06/2016"
   ms.author="bwren" />

# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Teatiste haldus lahenduse toimingud halduse komplekti (OMS)

![Teatiseikoon haldus](media/log-analytics-solution-alert-management/icon.png) Lahendus teatiste haldus aitab teil analüüsida kõiki teatisi teie keskkonnas.  Lisaks konsolideerimisele sees OMS teatised, see impordib teatiste ühendatud süsteemi Center toimingute Manager (SCOM) halduse rühmade Log Analytics.  Mitme halduse levirühmadega keskkonnas, teatiste haldamise lahendus annab kokkuvõtlik teatiste üle kõik halduse rühmad.

## <a name="prerequisites"></a>Eeltingimused

- See lahendus on vaja importida teatiste SCOM, OMS tööruumi ja SCOM haldus jaotises kirjeldatud [Ühenduse Toiminguhalduri Log Analytics](log-analytics-om-agents.md)protsessi abil vahelist ühendust.  


## <a name="configuration"></a>Konfigureerimine

Teatiste haldus lahendus lisada OMS tööruumi abil kirjeldatud [Lisa](log-analytics-add-solutions.md)lahendusi.  On pole veel konfigureerimine vajalik.

## <a name="management-packs"></a>Juhtimise tuvastamine

Kui teie SCOM rühma on ühendatud teie OMS tööruumi, siis järgmised halduse paketid installitakse SCOM kui lisate seda lahendust.  Ei ole konfiguratsiooni või halduse komplektid nõutav hooldustööd.  

- Microsoft System Center Advisor teatiste haldus (Microsoft.IntelligencePacks.AlertManagement)

Lahendus halduse paketid värskendamise kohta leiate lisateavet teemast [Ühenduse Toiminguhalduri Log Analytics](log-analytics-om-agents.md).

## <a name="data-collection"></a>Andmete kogumine

### <a name="agents"></a>Agentide

Järgmises tabelis kirjeldatakse ühendatud allikad, mis ei toeta seda lahendust.

| Ühendatud allikas | Tugi | Kirjeldus |
|:--|:--|:--|
| [Windowsi agentide](log-analytics-windows-agents.md) | Ei | Otsest Windows agentide Loo SCOM teatisi. |
| [Linux agentide](log-analytics-linux-agents.md) | Ei | Otsest Linux agentide Loo SCOM teatisi. |
| [SCOM rühma](log-analytics-om-agents.md) | Jah | Teatised, mis on genereeritud SCOM agentide toimetatakse halduse rühma ja seejärel edastatakse Log Analytics.<br><br>Log Analytics SCOM agendi otsene ühendus ei ole vaja. Teatiste andmed edasi OMS hoidla haldus rühmast. |
| [Azure storage konto](log-analytics-azure-storage.md) | Ei | Azure storage kontodel SCOM teadete ei salvestata. |

### <a name="collection-frequency"></a>Saidikogumi sagedus

Sees OMS teatised on võimalik lahendus kohe.  Teatise andmete saadetakse rühmast SCOM halduse Log Analytics iga 3 minutit.  

## <a name="using-the-solution"></a>Lahendus abil

Teatiste haldus lahenduse lisamisel oma OMS tööruumi paani **Teatiste haldus** lisatakse OMS armatuurlauale.  Sellel paanil kuvatakse count ja graafiliselt arvu viimase 24 tunni jooksul tekkinud praegu aktiivne teatised.  Te ei saa muuta selle ajavahemiku.

![Teatiste haldus paan](media/log-analytics-solution-alert-management/tile.png)

Klõpsake paani avamiseks **Teatiste halduse** armatuurlaua **Teatiste haldamine** .  Armatuurlaua sisaldab järgmises tabelis veerud.  Igas veerus ülemise kümme teatiste arvu sobitamine määratud ulatuse ja ajavahemiku kriteeriumid selle veeru järgi.  Saate käivitada log otsingu sisaldava tervet loendit, klõpsates nuppu, et **näha kõiki** veeru allosas või klõpsates selle veeru päist.

| Veerg| Kirjeldus |
|:--|:--|
| Kriitiline teatised | Kõik teatised ja tõsidust kriitiline rühmitatud teatiste nime järgi.  Klõpsake kaustapaanil Teavita nime esitus kõik kirjed, et Teavita log otsingu käivitamiseks. |
| Hoiatus teatised | Kõik teatised ja tõsidust hoiatus rühmitatud teatiste nime järgi.  Klõpsake kaustapaanil Teavita nime esitus kõik kirjed, et Teavita log otsingu käivitamiseks. |
| Aktiivse SCOM teatised | Kõik SCOM teatised, mis tahes maakond erinevalt *suletud* Rühmitusalus teatise loonud allikat. |
| Kõik aktiivse teatised | Kõik teatised koos iga raskusaste rühmitatud teatiste nime järgi. Ainult sisaldab SCOM teatiste abil mujal kui *suletud*.|

Kui kerite paremale, loetletakse armatuurlaud mitme levinud suhtes, mida võite klõpsata teatiste andmete [log otsingu](log-analytics-log-searches.md) tegemiseks.

![Teatiste haldamise armatuurlaud](media/log-analytics-solution-alert-management/dashboard.png)

## <a name="scope-and-time-range"></a>Tähtajad ja ulatus

Vaikimisi on analüüsitud teatiste haldamise lahendus teatisi kõigi ühendatud halduse rühmadest, mis on loodud viimase 7 päeva jooksul.  

![Teatiste haldus ulatus](media/log-analytics-solution-alert-management/scope.png)

- Analüüsi kaasatud rühmade haldus muutmiseks klõpsake **ulatus** armatuurlaua ülaosas.  Võite valida **Global** kõigi ühendatud halduse rühmade või **Haldus jaotises** ühe halduse rühma valimiseks.

- Ajavahemiku teatisi muutmiseks valige **andmete põhjal** armatuurlaua ülaosas.  Saate valida teatised viimase 7 päeva, 1 päeva ja 6 tunni jooksul.  Või saate valida **kohandatud** ja määrata kohandatud ajavahemiku lõikes.

## <a name="log-analytics-records"></a>Kasutusanalüüsi kirjed

Teatiste haldus lahendus analüüsib mõni kirje tüüpi **teatis**.  Lisaks importimine SCOM teatised ja iga tüüpi **teatiste** ja SourceSystem, **OpsManager**vastav kirje loomine.  Järgmises tabelis on need kirjed atribuudid.  

| Atribuut | Kirjeldus |
|:--|:--|
| Tüüp | *Teavita* |
| SourceSystem | *OpsManager* |
| AlertContext | XML-vormingus genereeritakse teatise põhjustanud üksuse andmete üksikasjad. |
| AlertDescription | Teatise üksikasjalik kirjeldus. |
| AlertId | GUID teatise. |
| AlertName | Teatise nime. |
| AlertPriority | Teatise prioriteeditase. |
| AlertSeverity | Raskusaste taseme teatise. |
| AlertState | Viimane eraldusvõime olekus teatise. |
| LastModifiedBy | Muudetud teatise kasutaja nimi. |
| ManagementGroupName | Kus on loodud teatise halduse rühma nime. |
| RepeatCount | Sama teatise loodud sama aja arvu jälgida objekti, sest lahendamist. |
| ResolvedBy | Teatise lahendatud kasutaja nimi. Kui teatise ei ole veel lahendatud tühjendamine. |
| SourceDisplayName | Kuvatav nimi loodud teatise jälgimisega seotud objekti. |
| SourceFullName | Jälgimise objekt, mille loodud teatise täisnimi. |
| TicketId | Piletimüügi ID teatise kui SCOM keskkonnas on integreeritud protsessi määramise piletite kohta teatiste saamiseks.  Tühi pole Piletite ID on määratud. |
| TimeGenerated | Kuupäev ja kellaaeg, mis loodi teatise. |
| TimeLastModified | Kuupäev ja kellaaeg viimati muudetud teatise. |
| TimeRaised | Kuupäev ja kellaaeg, mis on loodud teatise. |
| TimeResolved | Kuupäev ja kellaaeg, teatise lahendab. Kui teate ei ole veel lahendatud tühjendamine. |

## <a name="sample-log-searches"></a>Valimi log otsingud

Järgmises tabelis toodud valimi log otsinguid Teavita kirjete kogutud seda lahendust.  

| Päringu | Kirjeldus |
|:--|:--|
| Tüüp = teatiste SourceSystem = OpsManager AlertSeverity = viga TimeRaised > nüüd 24 tunni | Kriitiline teatiste astmes viimase 24 tunni jooksul |
| Tüüp = teatiste AlertSeverity = hoiatus TimeRaised > nüüd 24 tunni | Hoiatus teatiste astmes viimase 24 tunni jooksul  |
| Tüüp = teatiste SourceSystem = OpsManager AlertState! = suletud TimeRaised > nüüd - 24 TUNNIST & #124; Mõõtke count(), mis Count, SourceDisplayName | Allikate aktiivne teatiste astmes viimase 24 tunni jooksul |
| Tüüp = teatiste SourceSystem = OpsManager AlertSeverity = viga TimeRaised > nüüd - 24-TUNNISE AlertState! = suletud | Kriitiline teatiste esitatud viimase 24 tunni jooksul, mis on endiselt aktiivne |
| Tüüp = teatiste SourceSystem = OpsManager TimeRaised > nüüd - 24-TUNNISE AlertState = suletud | Teatised, mis on esitatud viimase 24 tunni jooksul, mis on nüüd suletud |
| Tüüp = teatiste SourceSystem = OpsManager TimeRaised > nüüd - 1 päeva & #124; Mõõtke count(), mis Count, AlertSeverity | Teatiste astmes rühmitatud raskusaste 1 viimase päeva jooksul |
| Tüüp = teatiste SourceSystem = OpsManager TimeRaised > nüüd - 1 päeva & #124; RepeatCount laskuv sortimine | Teatiste astmes sorditud nende korduste arv väärtus 1 viimase päeva jooksul |

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [teatiste Log Analytics](log-analytics-alerts.md) Log Analytics teatiste loomise kohta.
