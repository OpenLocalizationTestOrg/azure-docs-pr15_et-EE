<properties 
    pageTitle="Kuidas jälgida pilveteenus | Microsoft Azure'i" 
    description="Saate teada, kuidas jälgida pilveteenustega Azure klassikaline portaalis." 
    services="cloud-services" 
    documentationCenter="" 
    authors="rboucher" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="robb"/>


# <a name="how-to-monitor-cloud-services"></a>Kuidas jälgida pilveteenustega

[AZURE.INCLUDE [disclaimer](../../includes/disclaimer.md)]

Saate jälgida `key` jõudluse mõõdikud oma pilveteenustega Azure klassikaline portaalis. Saate määrata minimaalne ja iga teenuse rolli Paljusõnaline jälgimine ja saate kohandada jälgimise kuvatakse. Salvestusruumi konto, millele pääseb väljaspool portaali talletatakse Paljusõnaline jälgimisega seotud andmeid. 

Jälgimise kuvab Azure'i klassikaline portaalis on hästi konfigureeritav. Soovi korral saate lehe **jälgimine** loendis mõõdikute jälgitava mõõdikud ja saate valida, millised mõõdikute diagrammile mõõdikute diagrammides lehe **jälgimine** ja armatuurlaud. 

## <a name="concepts"></a>Mõisted

Vaikimisi minimaalsete jälgimise jaoks on esitatud uue pilveteenus abil kogutud rollid eksemplaride (virtuaalmasinates) host operatsioonisüsteemi jõudluse hinnale. Minimaalne mõõdikute on piiratud CPU protsent, andmete rakendustes, andmed välja, ketta lugemine läbilaskevõimet ja ketta kirjutamine läbilaskevõimet. Konfigureerimisega Paljusõnaline jälgida, saate täiendavaid mõõdikute põhjal jõudlusandmeid sees virtuaalmasinates (rolli aknad). Paljusõnaline mõõdikute lubada täpsema analüüsi probleemid, mis ilmnevad teenuserakenduse toimingud.

Vaikimisi on jõudluse andmete põhjal rolli aknad valimisse ja 3-minutilise intervalliga rolli eksemplari üle. Kui Paljusõnaline jälgimine, liidetakse selle töötlemata jõudluse andmete iga rolli eksemplari ja üle rolli aknad iga rolli 5 minutit, 1 tund ja 12 tunni järel. Kokkuvõtlike andmete langetatakse 10 päeva pärast.

Kui olete lubanud Paljusõnaline jälgimine, liidetud andmed on salvestatud konto salvestusruumi tabelid. Paljusõnaline jälgimise rolli lubamiseks tuleb konfigureerida diagnostika ühendusstringi lingitud konto salvestusruumi. Saate erinevate salvestusruumi kontod erinevad rollid.

Pange tähele, et Paljusõnaline jälgimist võimaldavad suurendada oma andmete salvestamine, andmete edastamiseks ja salvestusruumi tehingud salvestusruumi kulud. Minimaalne jälgimine ei nõua salvestusruumi konto. Mõõdikute, mis on esitatud minimaalsete jälgimisega seotud tasemel andmeid on talletatud teie salvestusruumi konto, isegi juhul, kui määrate jälgimisega seotud taseme Paljusõnaline.


## <a name="how-to-configure-monitoring-for-cloud-services"></a>Kuidas: konfigureerimine pilveteenustega jälgimine

Järgmistest toimingutest abil saate konfigureerida Paljusõnaline või minimaalsete jälgimise Azure klassikaline portaalis. 

### <a name="before-you-begin"></a>Enne alustamist

- Looge konto salvestusruumi jälgimisega seotud andmete talletamiseks. Saate erinevate salvestusruumi kontod erinevad rollid. Lisateavet leiate teemast **Salvestusruumi kontode**kohta, või vaadake, [Kuidas Loo konto salvestusruumi](/manage/services/storage/how-to-create-a-storage-account/).

- Azure'i diagnostika lubamine oma pilvepõhise teenuse rollid. Vt [diagnostika puhul pilveteenuste konfigureerimine](https://msdn.microsoft.com/library/azure/dn186185.aspx#BK_EnableBefore).

Veenduge, et diagnostika ühendusstring on rolli konfiguratsiooni Esita. Te ei saa jälgimist sisse lülitada Paljusõnaline kuni lubada Azure diagnostika ja kaasata diagnostika ühendusstringi rolli konfigureerimine.   

> [AZURE.NOTE] Azure'i SDK 2,5 suunatud projektide automaatselt pole kaasanud diagnostika ühendusstringi projekti malli. Projektide peate rolli konfiguratsiooni diagnostika ühendusstringi käsitsi lisada.

**Diagnostika ühendusstringi käsitsi lisamine rolli konfigureerimine**

1. Avage projekt pilveteenusesse Visual Studio
2. Topeltklõpsake **rolli** avage designer rolli ja valige vahekaart **sätted**
3. Otsige üles säte nimega **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**. 
4. Kui see säte pole seejärel klõpsake nuppu **Lisada sätte** lisamine konfiguratsiooni ja **ConnectionString** tüüp uue sätte muutmine
5. Ühendusstringi jaoks väärtuse määrata selle, klõpsates nuppu **…** . See avab dialoogiboksi võimaldab valida salvestusruumi konto.

    ![Visual Studio sätted](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a>Muuta Paljusõnaline või minimaalsete jälgimine

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com/)avage lehe juurutamiseks pilvepõhise teenuse **konfigureerimine** .

2. **Tase**, klõpsake **Verbose** või **minimaalsete**. 

3. Klõpsake nuppu **Salvesta**.

Kui lülitate Paljusõnaline jälgimine, peab algama, kuvatakse andmed Azure klassikaline portaalis tunni jooksul.

Töötlemata jõudluse andmete ja liidetud jälgimisega seotud andmed on salvestatud tabelite kvalifitseeritud juurutamise ID rollide salvestusruumi konto. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Kuidas: pilveteenuste teenuse mõõdikute teatiste vastuvõtmine

Võite saada teatisi oma pilveteenuses jälgimise mõõdikute põhjal. Azure'i klassikaline portaalis lehel **Halduse teenused** saate luua reegli, mis käivitab teatise, kui valite mõõdiku jõuab teie määratud väärtuse. Võite ka on saadetud teatise käivitamisel. Lisateavet leiate teemast [kohta: teatiste vastuvõtmine ja haldamine teatiste reegleid Azure](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Kuidas: mõõdikute mõõdikute tabeli lisamine

1. Avage [Azure'i klassikaline portaali](http://manage.windowsazure.com/) **kuvari** leht pilveteenusesse.

    Vaikimisi kuvatakse tabeli mõõdikute saadaval mõõdikute alamhulka. Joonisel on vaikimisi Paljusõnaline mõõdikute pilveteenuses, mis on piiratud tõrjumiseks Memory\Available megabaiti jõudluse rolli tasemel andmete jaoks. Valige täiendavad liitväärtuse ja rolli taseme mõõdikute jälgimiseks Azure klassikaline portaalis **Lisamine mõõdikute** abil.

    ![Paljusõnaline kuvamine](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
 
2. Mõõdikute mõõdikute tabeli lisamine

    1. Klõpsake **Mõõdikute lisamine** avamiseks **Valige mõõdikute**, allpool näidatud.

        Esimese saadaval meetermõõdustik on laiendatud saadaolevate suvandite kuvamiseks. Iga meetermõõdustik ülemine suvand kuvatakse kõikide rollide liidetud andmetele. Lisaks saate valida üksikuid rollid andmete kuvamiseks.

        ![Lisage mõõdikud](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)

    2. Valige mõõdikute kuvamiseks

        - Meetermõõdustik jälgimise suvandi laiendamiseks klõpsake allanoolt.
        - Märkige ruut iga jälgimise suvandi, mida soovite kuvada.

        Saate kuvada kuni 50 mõõdikute mõõdikute tabelis.

        > [AZURE.TIP] Paljusõnaline jälgida mõõdikute loend võib sisaldada kümneid mõõdikute. Kui soovite kuvada kerimisriba, viige kursor dialoogiboksi paremas servas. Loendi filtreerimiseks klõpsake ikooni Otsi ja sisestage otsinguväljale tekst, nagu allpool näidatud.
    
        ![Lisage mõõdikute otsing](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)


3. Kui olete mõõdikute valimise lõpetanud, klõpsake nuppu OK (märge).

    Valitud mõõdikute lisatakse tabelisse mõõdikute, nagu allpool näidatud.

    ![kuvari mõõdikud](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)

 
4. Mõõdiku mõõdikute tabeli kustutamiseks klõpsake seda valimiseks meetermõõdustik ja klõpsake **Meetermõõdustik kustutada**. (Ainult näete **Meetermõõdustik kustutamine** kui teil on valitud mõõdiku.)

### <a name="to-add-custom-metrics-to-the-metrics-table"></a>Lisada kohandatud mõõdikute mõõdikute tabel

**Verbose** jälgimise tase sisaldab loendit vaikimisi mõõdikute, mida saate jälgida portaalis. Lisaks saate jälgida mis tahes kohandatud mõõdikute või jõudluse hinnale määratletud rakenduse portaali kaudu.

Järgmised toimingud endale on sisse lülitatud **Verbose** tasemel jälgimise ja koguda ja edastamine kohandatud jõudluse hinnale oma rakenduse konfigureerimine on. 

Kohandatud jõudluse hinnale portaalis kuvamiseks peate värskendama wad-juhtelemendi-ümbrises konfiguratsiooni:
 
1. Avage wad-juhtelemendi-container bloobimälu diagnostika salvestusruumi kontole. Visual Studio või muude salvestusruumi Exploreri abil saate seda teha.

    ![Visual Studio Server Explorer](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)

2. Liikuge bloobimälu tee mustri **RoleInstance/DeploymentId/RoleName** abil otsida oma rolli eksemplari konfigureerimine. 

    ![Visual Studio salvestusruumi Explorer](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. Laadige oma rolli eksemplari konfiguratsioonifail ja värskendage seda, et kaasata kõik kohandatud jõudluseloendurid. Näiteks jälgida *kirjutada ketta baiti sekundis* *C-ketas* lisada järgmised **PerformanceCounters\Subscriptions** sõlme all

    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. Muudatuste salvestamiseks ja konfigureerimise faili üleslaadimine tagasi ülekirjutamise olemasolev fail soovitud bloobimälu samasse asukohta.
5. Tavalise Paljusõnaline režiim Azure klassikaline konfiguratsiooni portaal. Kui Paljusõnaline režiimis juba on teil sisse-või väljalülitamiseks minimaalne ja et Paljusõnaline.
6. Kohandatud jõudluse näidiku kohe on saadaval dialoogiboksi **Mõõdikute lisada** . 

## <a name="how-to-customize-the-metrics-chart"></a>Kuidas: mõõdikute diagrammi kohandamine

1. Valige tabelis mõõdikute kuni 6 mõõdikute mõõdikute diagrammile kantav väärtus. Mõõdiku valimiseks märkige ruut selle vasakul küljel. Diagrammi mõõdikute mõõdiku eemaldamiseks tühjendage selle märkeruut mõõdikute tabelis.

    Kui valite mõõdikute mõõdikute tabelis, lisatakse mõõdikud mõõdikute diagrammi. Kitsas kuval sisaldab **rohkem n** ripploendist argumendil päised, mis ei sobi kuvamise.

 
2. Asemel väljatulemid suhteline väärtused (ainult jaoks iga mõõdiku lõppväärtus) absoluutse väärtuste (Y-telg kuvatud), valige käsk või absoluutne diagrammi kohal.

    ![Absoluutne ja suhteline](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)

3. Ajavahemiku muutmine mõõdikute diagramm kuvatakse, valige 1 tund, 24 tundi või 7 päeva diagrammi kohal.

    ![Kuvari Kuva perioodi kohta.](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)

    Diagrammi armatuurlaua mõõdikud erineb joonestamist mõõdikute meetod. Standardsete mõõdikute on saadaval ja mõõdikute lisada või eemaldada, valides argumendil päis.

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a>Armatuurlaual mõõdikute diagrammi kohandamine

1. Avage armatuurlaualt pilveteenusesse.

2. Lisage või mõõdikute diagrammilt eemaldada.

    - Valige uus mõõdiku joonestamiseks diagrammi päised mõõdiku kõrval olev ruut. Kitsas kuval klõpsake allanoolt, ** *n*??metrics** diagrammile mõõdiku päiseala diagrammi ei saa kuvada.

    - Mõõdiku, mis on kantakse diagrammi kustutamiseks tühjendage ruut selle päise järgi.

3. Kuvab **absoluutne** ja **suhteline** vaheldumisi aktiveerimine.

4. Valige 1 tund, 24 tundi või 7 päeva andmete kuvamiseks.

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a>Kuidas: Accessi Paljusõnaline väljaspool Azure klassikaline portaali andmete

Tabelite salvestusruumi kontod iga rolli määratud talletatakse Paljusõnaline jälgimisega seotud andmeid. Iga pilve juurutamiseks, luuakse kuus tabelite roll. Kahe tabeli luuakse iga (5 minutit, 12 tundi ja 1 tund). Üks nende tabelite talletab rolli taseme liitmised; teise tabeli talletab liitmised rolli eksemplarid. 

Tabelinimede on järgmises vormingus:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

kui:

- *deploymentID* on määratud pilvepõhise teenuse juurutamise GUID

- *aggregation_interval* = 5 M, 1 H või 12 H

- roll taseme liitmised = R

- liitmised rolli eksemplaride = RI

Näiteks salvestada järgmistes tabelites Paljusõnaline kokkuvõtliku 1-tunnise intervalliga andmeid:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
