<properties 
   pageTitle="Log Analytics letid Windows ja Linux täitmiseks | Microsoft Azure'i"
   description="Jõudluse hinnale kogutakse Log Analytics jõudluse kohta Windows ja Linux agentide analüüsimiseks.  Selles artiklis kirjeldatakse, kuidas konfigureerida nii Windowsi jõudlus hinnale kogumine ja Linux agentide need andmed talletatakse OMS hoidla ja kuidas neid analüüsida OMS portaalis."
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Windows ja Linux jõudluse andmeallikate Log Analytics 

Jõudluse hinnale Windows ja Linux annab ülevaate riistvara, operatsioonisüsteemide ja rakenduste jõudlust.  Log Analytics koguda jõudluse hinnale sagedasti lähedal reaalajas (NAR) analüüsi Lisaks liitmise jõudlusandmeid enam termin analüüsi ja aruandluse jaoks.

![Jõudluse hinnale](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Jõudluse hinnale konfigureerimine

Jõudluse hinnale [menüü andmed Log Kasutusanalüüsi sätteid](log-analytics-data-sources.md#configuring-data-sources)konfigureerida.

Windowsi või Linuxi jõudluse hinnale uue OMS tööruumi esmalt konfigureerimisel on esitatud võimalus kiiresti luua mitme levinud hinnale.  Need on loetletud ruut iga abil.  Veenduge, et kõik algselt loodava hinnale on märgitud ja seejärel klõpsake nuppu **Lisa valitud jõudluse hinnale**.

![Windowsi jõudlus hinnale konfigureerimine](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Järgige uue Windowsi jõudlus vastuolus kogumise lisamiseks.

1. Tippige tekst väljale Vorminda *objekti (eksemplari) \counter*näidiku nimi.  Kui alustate tippimist, on esitatud levinud hinnale vastavat loendit.  Võite valida vastuolus loendist või tippige oma pildiga.  Saate naasta ka kõik eksemplarid kindla vastuolus, määrates *object\counter*. 
2. Klõpsake **+** või vajutage klahvi **Enter** , et näidiku loendisse lisada.
3. Kui lisate vastuolus, kasutab vaikimisi 10 sekundit selle **Valimi intervall**.  Kui soovite vähendada mäluruumi kogutud jõudluse andmeid, saate seda suurem väärtus 1800 sekundit (30 minuti) muuta.
4. Kui olete valmis lisamise hinnale, nuppu **Salvesta** salvestada konfiguratsiooni ekraani ülaosas.

![Linux jõudluse hinnale konfigureerimine](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Järgige uue Linuxi jõudluse vastuolus kogumise lisamiseks.

1. Vaikimisi lükata kõik konfiguratsioonimuudatuste automaatselt kõigile.  Fluentd Andmekoguja saadetakse agentide Linuxi jaoks konfigureerimise faili.  Kui soovite muuta selle faili käsitsi iga Linux agent, siis tühjendage ruut *Rakenda all minu Linux masinad konfigureerimine*.
2. Tippige tekst väljale Vorminda *objekti (eksemplari) \counter*näidiku nimi.  Kui alustate tippimist, on esitatud levinud hinnale kattuvad loendiga.  Võite valida vastuolus loendist või tippige oma pildiga.  
2. Klõpsake **+** või vajutage sisestusklahvi **Enter** näidiku lisamiseks muude hinnale objekti loendit.
3. Objekti kõigi hinnale kasutada sama **Proovi intervall**.  Vaikimisi on 10 sekundit.  Saate muuta 1800 sekundit (30 minuti) väärtus suurem kui soovite vähendada mäluruumi kogutud jõudluse andmeid.
4. Kui olete valmis lisamise hinnale, nuppu **Salvesta** salvestada konfiguratsiooni ekraani ülaosas.

## <a name="data-collection"></a>Andmete kogumine

Log Analytics kogub kõik määratud jõudluse hinnale nende valimi määratud intervalliga kõik agentide, mis on vastuolus installitud.  Andmeid ei liidetakse ja töötlemata andmete on saadaval kõigi log otsingu jaoks määratud OMS tellimuse kestuse.


## <a name="performance-record-properties"></a>Jõudluse kirje atribuudid

Jõudluse kirjed peavad tüüpi **täiuslik** ja atribuutide järgmises tabelis.

| Atribuut | Kirjeldus |
|:--|:--|
| Arvuti         | Arvuti, mis on kogutud sündmus. |
| CounterName      | Jõudluse näidiku nimi |
| CounterPath      | Täielik tee vormi näidiku \\ \\ \<arvuti >\\object(instance)\\counter. |
| Põhimõttelinegi     | Näidiku arvulise väärtuse.  |
| InstanceName     | Sündmuse eksemplari nimi.  Kui eksemplari pole tühi. |
| ObjectName       | Jõudluse objekti nimi |
| SourceSystem  | Mis on kogutud andmete tüüp. <br> OpsManager – Windows agent, kas otse ühendust luua või SCOM <br> Linux – kõik Linuxi agentide  <br> AzureStorage – Azure diagnostika |
| TimeGenerated       | Kuupäev ja kellaaeg, andmed on valimisse. |


## <a name="sizing-estimates"></a>Annab vastuseks hinnangulise suuruse muutmine

 Kindla vastuolus kogumiseks 10 sekundi järel ligikaudne on umbes 1 MB päevas eksemplaris.  Saate hinnata kindla vastuolus järgmise valemiga salvestusruumi nõuetele.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Log otsinguid jõudluse kirjed

Järgmises tabelis leiate muu näited log otsinguid, et tuua tegevust.

| Päringu | Kirjeldus |
|:--|:--|
| Tüüp = täiuslik | Kõik jõudlusandmeid |
| Tüüp = täiuslik arvuti = "Minuarvuti" | Kõik jõudlusandmeid arvutist |
| Tüüp = täiuslik CounterName = "Praegune ketas järjekorra pikkus" | Kõigi tulemustega seotud andmete jaoks teatud vastuolus |
| Tüüp = täiuslik (ObjectName = protsessor) CounterName = "% protsessori aja" InstanceName = _Total & #124; mõõta Avg(Average) AVGCPU arvuti | Keskmise CPU kasutamine üle kõik arvutid |
| Tüüp = täiuslik (CounterName = "% protsessor Time") & #124;  Mõõt max(Max) arvuti | Suurim lubatud CPU kasutuse üle kõik arvutid |
| Tüüp = täiuslik ObjectName = loogiline ketas CounterName = "Praegune kettale järjekorra pikkus" arvuti = "MyComputerName" & #124; Mõõt Avg(Average) InstanceName järgi | Keskmine üle kõik eksemplarid antud arvuti praegune kuhjuda ketta pikkus |
| Tüüp = täiuslik CounterName = "DiskTransfers/sec" & #124; Mõõt percentile95(Average) arvuti | 95 protsentiili, ketta edastamine sekundis üle kõik arvutid |
| Tüüp = täiuslik CounterName = "% protsessori aja" InstanceName = "_Total" & #124; mõõta avg(CounterValue) arvuti intervall 1 tund | Kord tunnis keskmise CPU hõivatus üle kõik arvutid. |
| Tüüp = täiuslik arvuti = "Minuarvuti" CounterName = % * InstanceName = _Total & #124; mõõta percentile70(CounterValue) CounterName intervall 1 tund | Kord tunnis 70 protsentiili alati % protsendimäärana counter arvutist |
| Tüüp = täiuslik CounterName = "% protsessori aja" InstanceName = "_Total" (arvuti = "Minuarvuti") & #124; mõõta min(CounterValue) avg(CounterValue), percentile75(CounterValue), max(CounterValue) arvuti intervall 1 tund | Kord tunnis Keskmine, miinimum, maksimum ja 75 protsentiil CPU hõivatus kindla arvutiga |

## <a name="viewing-performance-data"></a>Tulemustega seotud andmete vaatamine

Log otsingu tulemustega seotud andmete käivitamisel kuvatakse vaikimisi **Logi** vaadet.  Graafilisel kujul andmete kuvamiseks klõpsake nuppu **mõõdikute**.  Graafilise üksikasjalik vaade, klõpsake selle **+** vastu kõrval.  

![Ahendatud mõõdikute kuvamine](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

Kui olete valinud kellaaja vahemik on 6 tunni või vähem, siis graafik värskendatakse iga paari sekundi järel.  Reaalajas andmed kuvatakse paremal pool helesinine graafik.

![Reaalajas andmed laiendatud mõõdikute kuvamine](media/log-analytics-data-sources-performance-counters/metricsexpanded.png)

Log otsingu tulemustega seotud andmete liitmine kohta leiate teemast [nõudmisel argumendil liitmise ja visualiseerimine OMS](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/).

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [log otsingud](log-analytics-log-searches.md) andmeallikate ja lahenduste kogutud andmete analüüsimiseks.  
- Kogutud andmete eksportimine [Power BI](log-analytics-powerbi.md) täiendavad visualiseerimine ja analüüs.