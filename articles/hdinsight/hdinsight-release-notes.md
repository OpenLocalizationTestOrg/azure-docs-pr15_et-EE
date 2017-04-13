<properties
    pageTitle="Vabastage märkmete Hadoopi komponendid Windows Azure Hdinsightiga | Microsoft Azure'i"
    description="Viimane väljalaskemärkmed ja versiooni Windows Azure Hdinsightiga Hadoopi komponendid. Hankige Hadoopi, Apache Storm ja HBase arengu näpunäiteid ja üksikasjad."
    services="hdinsight"
    documentationCenter=""
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="nitinme"/>


# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Väljalaskemärkmed – Hadoopi komponendid Windows Azure Hdinsightiga

## <a name="notes-for-10262016-release-of-r-server-on-hdinsight"></a>Märkmete 10/26/2016 versiooni R Server Hdinsightiga

- On lihtne, R Server Hdinsightiga kobar ettevalmistamise kohta.
- R serveris Hdinsightiga on nüüd saadaval tavaline Hdinsightiga "R Server" klaster tüüp ja eraldi Hdinsightiga rakendus pole enam installitud. Serva sõlm ja R Server binaarkujul on nüüd ette valmistatud R Server kobar juurutamise käigus. See parandab kiiruse ja usaldusväärsuse ettevalmistamise. Hinnakirjad mudeli R server on ajakohastada.
- R Server kobar tüüp hind on nüüd alusel Standard taseme hind pluss R Server lisatasu eest hind. Premium taseme nüüd on mõeldud lisafunktsioonidele saadaval üle kobar eri tüüpi- ja R Server kobar Type ei kasutata. See muudatus ei mõjuta efektiivse hinnad R serveri, muudab ainult olevate kulude esitamise arve. Kõik olemasolevad R Server kogumite tööd jätkata ja ARM Mallid jätkub kuni taunimine funktsioon. **Kuigi värskendama oma skriptitud juurutuste uue ARM malli kasutamiseks on soovitatav.**


## <a name="notes-for-08302016-release-of-r-server-on-hdinsight"></a>Märkmete 30-08-2016 versiooni R Server Hdinsightiga

Linux-põhine Hdinsightiga kogumite täisversioon numbrid juurutatud selles versioonis:

|HDI |HDI kobar versioon   |HDP |HDP koostamine   |Ambari koostamine |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.8268980    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8268980    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8269383    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

Windowsi-põhiste Hdinsightiga kogumite täisversioon numbrid juurutatud selles versioonis:

|HDI |HDI kobar versioon   |HDP |HDP koostamine     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1033.2559206   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1033.2559206    |2.0 |2.0.13.0-2117 |
|3,1 |3.1.4.1033.2559206    |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.1033.2559206    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1033.2559206    |2.3 |2.3.3.1-25    |

## <a name="notes-for-08172016-release-of-r-server-on-hdinsight"></a>Märkmete 08/17/2016 versiooni R Server Hdinsightiga

- R Server 8.0.5 – peamiselt vigu parandada väljaanne. [R Server väljalaskemärkmed](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) lisateabe saamiseks vt. 
- AzureML paketi serva sõlme – [see R pakett](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) võimaldab R mudelite avaldatud ja tarbitud Azure'i ML web teenust.  Lisateavet meie ["ülevaade, R Server klõpsake Hdinsightiga"](hdinsight-hadoop-r-server-overview.md) artikli jaotisest ["kiireks mudel"](hdinsight-hadoop-r-server-overview.md#operationalize-a-model) .
- Linux sõltuvused [100 ülemist populaarsemaid R paketid](https://github.com/metacran/cranlogs) – need pakett sõltuvuste nüüd eelinstallitud Linux.  
- Võimalus kasutada CRAN repo lisamise R paketid andmete sõlmi. Lisateavet meie ["Kasutamise alustamine R Server Hdinsightiga"](hdinsight-hadoop-r-server-get-started.md) artikli jaotisest ["Installi R paketid"](hdinsight-hadoop-r-server-get-started.md#install-r-packages) .
- Täiustatud usaldusväärsuse R Server ettevalmistamise kogumite loomisel.


## <a name="notes-for-08012016-release-of-hdinsight"></a>Märkmete 08/01/2016 versiooni Hdinsightiga

Linux-põhine Hdinsightiga kogumite täisversioon numbrid juurutatud selles versioonis:

|HDI |HDI kobar versioon   |HDP |HDP koostamine   |Ambari koostamine |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.8028416    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8028416    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8053402    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

Windowsi-põhiste Hdinsightiga kogumite täisversioon numbrid juurutatud selles versioonis:

|HDI |HDI kobar versioon   |HDP |HDP koostamine     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1005.2488842   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1005.2488842    |2.0 |2.0.13.0-2117 |
|3,1 |3.1.4.1005.2488842    |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.1005.2488842    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1005.2488842    |2.3 |2.3.3.1-25    |

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Kobar tüüp (nt säde, Hadoop, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Muudatuste Hdinsightiga 3.4 kogumite | Järgmised taru vaikeväärtuse muudetakse parema jõudluse <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul>| Teenus    | Kõik| N/A|
| Probleemi lahendamiseks sisalduvad selles versioonis | TARU 13632 HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933| Teenus    | Kõik| N/A

## <a name="notes-for-07142016-release-of-hdinsight"></a>Märkmete 14-07-2016 versiooni Hdinsightiga

Linux-põhine Hdinsightiga kogumite täisversioon numbrid juurutatud selles versioonis:

|HDI |HDI kobar versioon   |HDP |HDP koostamine   |Ambari koostamine |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.7932505    |2.2 |2.2.9.1-11  |2.2.1.12-2   |
|3.3 |3.3.1000.0.7932505    |2.3 |2.3.3.1-18  |2.2.1.12-2   |
|3.4 |3.4.1000.0.7933003    |2.4 |2.4.2.0     |2.2.1.12-2   |

Windowsi-põhiste Hdinsightiga kogumite täisversioon numbrid juurutatud selles versioonis:

|HDI |HDI kobar versioon   |HDP |HDP koostamine     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.989.2441725    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.989.2441725     |2.0 |2.0.13.0-2117 |
|3,1 |3.1.4.989.2441725     |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.989.2441725     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.989.2441725     |2.3 |2.3.3.1-21    |

## <a name="notes-for-07072016-release-of-hdinsight"></a>Märkmete 07/07/2016 versiooni Hdinsightiga

Linux-põhine Hdinsightiga kogumite täisversioon numbrid juurutatud selles versioonis:

|HDI |HDI kobar versioon   |HDP |HDP koostamine   |
|----|----------------------|----|------------|
|3,2 |3.2.1000.0.7864996    |2.2 |2.2.9.1-11  |
|3.3 |3.3.1000.0.7864996    |2.3 |2.3.3.1-18  |
|3.4 |3.4.1000.0.7861906    |2.4 |2.4.2.0     |

Windowsi-põhiste Hdinsightiga kogumite täisversioon numbrid juurutatud selles versioonis:

|HDI |HDI kobar versioon   |HDP |HDP koostamine     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.977.2413853    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.977.2413853     |2.0 |2.0.13.0-2117 |
|3,1 |3.1.4.977.2413853     |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.977.2413853     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.977.2413853     |2.3 |2.3.3.1-21    |

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Kobar tüüp (nt säde, Hadoop, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| [Huvitav Hdinsightiga tööriistad](hdinsight-apache-spark-intellij-tool-plugin.md) | Huvitav idee lisandmooduli jaoks Hdinsightiga säde kogumite on nüüd integreeritud Azure tööriistakomplekt huvitav. See toetab Azure SDK v2.9.1 uusim Java SDK-d, ja kõiki autonoomse Hdinsightiga lisandmooduli jaoks huvitav funktsioone.| Tööriistad    | Säde| N/A|
| [Eclipse Hdinsightiga tööriistad](hdinsight-apache-spark-eclipse-tool-plugin.md) | Azure'i tööriistakomplekt Eclipse toetab nüüd Hdinsightiga säde kogumite. See võimaldab järgmisi funktsioone. <ul><li>Loomine ja kirjutada säde rakenduse hõlpsalt Scala ja Java esimese klassi loome tugi IntelliSense'i, automaatne vormindamine, veakontroll jne.</li><li>Testige säde rakenduse kohalikult.</li><li>Esitage töö, et Hdinsightiga säde klaster ja toomiseks.</li><li>Azure'i sisse logida ja kasutada säde rühmad Azure tellimusega seostatud.</li><li>Saate liikuda kõigi Hdinsightiga säde klaster seotud salvestusruumi ressursid.</li></ul>| Tööriistad    | Säde| N/A

Alustades selles versioonis, oleme muutnud Külastajate OS lappimine poliitika Linux-põhine Hdinsightiga kogumite. Uue poliitika eesmärk on taaskäivitamisega tõttu lappimine märgatavalt vähendada. Uue poliitika jätkab paik virtuaalmasinates (VM) Linux kogumite iga esmaspäev või Neljapäev, alustades kell 12 UTC järkjärguline mood üle kõik antud kobar sõlmed. Siiski kuvatakse kõik antud VM ainult taaskäivitage kõige rohkem kui 30 päeva tõttu Külastajate OS lappimine. Lisaks on esimene taaskäivitage arvuti jaoks äsja loodud kobar ei juhtu varasemaks kobar loomiskuupäeva 30 päeva.

>[AZURE.NOTE] Need muudatused rakenduvad ainult äsja loodud kogumite või suurem selle versiooni.

## <a name="notes-for-06062016-release-of-hdinsight"></a>Märkmete 06-06-2016 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

|HDP    |HDI versioon    |Säde versioon  |Ambari järgu Number    |HDP järgu Number|
|-------|---------------|---------------|-----------------------|----------------|
|2.3    |3.3.1000.0.7702215|    1.5.2|  2.2.1.8-2|  2.3.3.1-18|
|2.4    |3.4.1000.0.7702224|    1.6.1|  2.2.1.8-2|  2.4.2.0|


See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Kobar tüüp (nt säde, Hadoop, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Klõpsake Hdinsightiga säde on üldiselt kättesaadav | See väljaanne toob täiustused kättesaadavus, skaleeritavus ja tootlikkuse allika Apache Spark klõpsake Hdinsightiga avamiseks. <ul><li>Valdkonna ees-saadavus SLA 99,9%, mis sobib nõuavad enterprise töökoormus.</li><li>Skaleeritav salvestusruumi kiht Azure'i andmesalve Lake abil.</li><li>Andmete uurimine ja iga etapi abivahendid. Kohandatud säde tuum Jupyter märkmikke lubamine interaktiivne andmete uurimine, BI armatuurlauad nagu Power BI, sellele ja Qlik integreerimine on hea andmete ühiskasutus ja pidev aruandlus, huvitav lisandmoodul on usaldusväärne suvand pikaajaline kood artefakt arengu ja silumine.</li></ul>| Teenus    | Säde| N/A|
| Huvitav Hdinsightiga tööriistad | See on mõni huvitav idee lisandmooduli Hdinsightiga säde kogumite jaoks. See võimaldab järgmisi funktsioone.<ul><li>Loomine ja kirjutada säde rakenduse hõlpsalt Scala ja Java esimese klassi loome tugi IntelliSense'i, automaatne vormindamine, veakontroll jne.</li><li>Testige säde rakenduse kohalikult.</li><li>Esitage töö, et Hdinsightiga säde klaster ja toomiseks.</li><li>Azure'i sisse logida ja kasutada säde rühmad Azure tellimusega seostatud.</li><li>Liikuge kõik klaster Hdinsightiga säde seotud salvestusruumi ressursid.</li><li>Liikuge kõik töö ajalugu ja klaster Hdinsightiga säde töö kohta.</li><li>Silumine säde töö eemalt arvutist.</li></ul>| Tööriistad    | Säde| N/A

## <a name="notes-for-05132016-release-of-hdinsight"></a>Märkmete 05-13-2016 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga (Windows) 3.1.4.922.2266903 (HDP 2.1.15.0-2374 - muutmata)
* Hdinsightiga (Windows) 3.2.7.922.2266903 (HDP 2.2.9.1-11)
* Hdinsightiga (Windows) 3.3.0.922.2266903 (HDP 2.3.3.1-18)
* Hdinsightiga (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* Hdinsightiga (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* Hdinsightiga (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Kobar tüüp (nt säde, Hadoop, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Säde versiooniuuendus ja muud veaparandusi | 1.6.1 Hdinsightiga kobar säde versiooni uuendused selles versioonis ja lahendab muid vigu| Teenus    | Säde| N/A

## <a name="notes-for-04112016-release-of-hdinsight"></a>Märkmete 04-11-2016 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga (Windows) 2.1.10.889.2191206 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga (Windows) 3.0.6.889.2191206 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga (Windows) 3.1.4.889.2191206 (HDP 2.1.15.0-2374 - muutmata)
* Hdinsightiga (Windows) 3.2.7.889.2191206 (HDP 2.2.9.1-10)
* Hdinsightiga (Windows) 3.3.0.889.2191206 (HDP 2.3.3.1-16-samaks)
* Hdinsightiga (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* Hdinsightiga (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* Hdinsightiga (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* SDK 1.5.8

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Klaster tüüp (nt Hadoopi, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Kohandatud metastore versioonitäiendusprobleemide HDI 3.4 | Kobar loomine nurjub, kui kasutasite kohandatud metastore, varem kasutatud teise Hdinsightiga kobar alumise versiooni. Selle põhjuseks on skripti versiooniuuendustõrge, mis on nüüd kinnitatud| Kobar loomine    | Kõik | N/A
| Liviuse krahh taastamine | Töö oleku paindlikkust pakub mis tahes töö Liviuse kaudu | Töökindluse | Säde Linux| N/A
| Jupyter sisu HA | Pakub võimalust salvestada ja laadida Jupyter märkmiku sisu ja seotud klaster salvestusruumi konto. Lisateavet leiate teemast [tuumad Jupyter märkmike jaoks saadaval](hdinsight-apache-spark-jupyter-notebook-kernels.md).| Märkmike | Säde Linux| N/A
| HiveContext Jupter märkmike eemaldamine | Kasutage `%%sql` asemel maagiline `%%hive` maagiline. SqlContext on võrdne hiveContext. Lisateabe saamiseks lugege teemat [tuumad Jupyter märkmike puhul saadaval](hdinsight-apache-spark-jupyter-notebook-kernels.md)| Märkmike    | Säde kogumite Linux| N/A
| Taunimine säde varasemad versioonid | Varasem versioon säde 1.3.1 eemaldatakse teenuse 5/31 | Teenus | Säde kogumite Windowsis | N/A

## <a name="notes-for-03292016-release-of-hdinsight"></a>Märkmete 03/29/2016 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - muutmata)
* Hdinsightiga (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 - muutmata)
* Hdinsightiga (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* Hdinsightiga (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - muutmata)
* Hdinsightiga (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - muutmata)
* Hdinsightiga (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* SDK 1.5.8

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Klaster tüüp (nt Hadoopi, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Lisatud Hdinsightiga 3.4 versiooni ja värskendatud HDP versiooni jaoks kõik Hdinsightiga kogumite | Selles versioonis lisatud Hdinsightiga v3.4 (vastavalt HDP 2.4) ja värskendatud ka muude versioonide HDP. HDP 2.4 väljalaskemärkmed on saadaval [siin](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) ja Lisateavet Hdinsightiga versioonid võivad olla leitud [siin](hdinsight-component-versioning.md).| Teenus    | Kõik Linux kogumite| N/A
| Hdinsightiga Premium | Hdinsightiga on nüüd saadaval kahest järgmisest kategooriast - Standard- ja Premium. Hdinsightiga Premium on praegu saadaval ainult Hadoopi eelvaade ja ja säde kogumite Linux. Lisateabe saamiseks vaadake [siin](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium).| Teenus    | Hadoopi ja Linux säde| N/A
| Microsoft R Server | Hdinsightiga Premium pakub Microsoft R Server, mis võivad olla kaasas Hadoopi ja säde kogumite Linux. Lisateabe saamiseks vaadake teemat [Ülevaade R Server Hdinsightiga kohta](hdinsight-hadoop-r-server-overview.md).| Teenus    | Hadoopi ja Linux säde| N/A
| Säde 1.6.0 | Nüüd ka Hdinsightiga 3.4 kogumite säde 1.6.0| Teenus    | Säde kogumite Linux| N/A
| Jupyter märkmiku täiustused | Jupyter märkmike saadaval säde kogumite nüüd anda täiendavad säde tuumad. Need sisaldavad ka täiustusi nagu kasutamine %% maagiline, automaatne visualiseerimine ja integreerimine Python visualiseeringu teekide (nt matplotlib). Lisateavet leiate teemast [tuumad Jupyter märkmike jaoks saadaval](hdinsight-apache-spark-jupyter-notebook-kernels.md). | Teenus | Säde kogumite Linux | N/A

## <a name="notes-for-03222016-release-of-hdinsight"></a>Märkmete 03/22/2016 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - muutmata)
* Hdinsightiga (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 - muutmata)
* Hdinsightiga (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* Hdinsightiga (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - muutmata)
* Hdinsightiga (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - muutmata)
* SDK 1.5.8

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Klaster tüüp (nt Hadoopi, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Kõik Hdinsightiga kogumite Hdinsightiga uuendatud | Selles versioonis, on värskendatud Hdinsightiga versioonide jaoks kõik Hdinsightiga kogumite| Teenus    | Kõik| N/A


## <a name="notes-for-03102016-release-of-hdinsight"></a>Märkmete 03/10/2016 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga (Windows) 2.1.10.859.2123216 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga (Windows) 3.0.6.859.2123216 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga (Windows) 3.1.4.859.2123216 (HDP 2.1.15.0-2374 - muutmata)
* Hdinsightiga (Windows) 3.2.7.859.2123216 (HDP 2.2.9.1-7)
* Hdinsightiga (Windows) 3.3.0.859.2123216 (HDP 2.3.3.1-5 - muutmata)
* Hdinsightiga (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* Hdinsightiga (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* SDK 1.5.8

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Klaster tüüp (nt Hadoopi, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Kõik Hdinsightiga kogumite Hdinsightiga uuendatud | Selles versioonis, on värskendatud Hdinsightiga versioonide jaoks kõik Hdinsightiga kogumite| Teenus    | Kõik| N/A

## <a name="notes-for-01272016-release-of-hdinsight"></a>Märkmete 2016/01/27 Hdinsightiga versiooni

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga (Windows) 2.1.10.817.2028315 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga (Windows) 3.0.6.817.2028315 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga (Windows) 3.1.4.817.2028315 (HDP 2.1.15.0-2374 - muutmata)
* Hdinsightiga (Windows) 3.2.7.817.2028315 (HDP 2.2.9.1-1)
* Hdinsightiga (Windows) 3.3.0.817.2028315 (HDP 2.3.3.1-5 - muutmata)
* Hdinsightiga (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* Hdinsightiga (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* SDK 1.5.8

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Klaster tüüp (nt Hadoopi, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Kõik Hdinsightiga kogumite Hdinsightiga uuendatud | Selles versioonis, on värskendatud Hdinsightiga versioonide jaoks kõik Hdinsightiga kogumite| Teenus    | Kõik| N/A

## <a name="notes-for-12022015-release-of-hdinsight"></a>Märkmete 12/02/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga (Windows) 2.1.10.763.1931434 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga (Windows) 3.0.6.763.1931434 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga (Windows) 3.1.4.763.1931434 (HDP 2.1.15.0-2374 - muutmata)
* Hdinsightiga (Windows) 3.2.7.763.1931434 (HDP 2.2.7.1-34 - muutmata)
* Hdinsightiga (Windows) 3.3.1000.0 (HDP 2.3.3.1-5)
* Hdinsightiga (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34 - muutmata)
* Hdinsightiga (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* SDK 1.5.8

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Klaster tüüp (nt Hadoopi, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Lisatud Hdinsightiga 3.3 versiooni ja värskendatud HDP versiooni jaoks kõik Hdinsightiga kogumite | Selles versioonis lisatud Hdinsightiga v3.3 (vastavalt HDP 2.3) ja värskendatud ka muude versioonide HDP. HDP 2.3 väljalaskemärkmed on saadaval [siin](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) ja Lisateavet Hdinsightiga versioonid võivad olla leitud [siin](hdinsight-component-versioning.md).| Teenus    | Kõik| N/A

## <a name="notes-for-11302015-release-of-hdinsight"></a>Märkmete 30-11-2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga (Windows) 2.1.10.757.1923908 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga (Windows) 3.0.6.757.1923908 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga (Windows) 3.1.4.757.1923908 (HDP 2.1.15.0-2374 - muutmata)
* Hdinsightiga (Windows) 3.2.7.757.1923908 (HDP 2.2.7.1-34)
* Hdinsightiga (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* SDK 1.5.8

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Klaster tüüp (nt Hadoopi, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Hdinsightiga uuendatud kõigi Hdinsightiga kogumite ja HDP versioonide jaoks Hdinsightiga 3,2 kogumite (Windows ja Linux) | Selles väljaandes värskendatud Hdinsightiga ja HDP versioonid | Teenus    | Kõik| N/A


## <a name="notes-for-10272015-release-of-hdinsight"></a>Märkmete 10/27/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga (Windows) 2.1.10.726.1866228 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga (Windows) 3.0.6.726.1866228 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga (Windows) 3.1.4.726.1866228 (HDP 2.1.15.0-2374 - muutmata)
* Hdinsightiga (Windows) 3.2.7.726.1866228 (HDP 2.2.7.1-33)
* Hdinsightiga (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* SDK 1.5.8

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Klaster tüüp (nt Hadoopi, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Hdinsightiga uuendatud kõigi Hdinsightiga kogumite (Windows ja Linux) | Selles väljaandes värskendatud Hdinsightiga ja HDP versioonid | Teenus    | Kõik| N/A
| Fikseeritud Jupyter Windows säde kogumite suurtähed kogumite abil | Kogumite, mis oli määratud suurtähtedega DNS-i nimed olnud probleeme Jupyter märkmike tõttu on origin taotluse sisse. Lahendus on muuta alumise Jupyter kasutaja konfiguratsioon DNS-i nimi. | Teenus    | Hdinsightiga säde (Windows)| N/A


## <a name="notes-for-10202015-release-of-hdinsight"></a>Märkmete 10/20/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.716.1846990 (Windows) (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga 3.0.6.716.1846990 (Windows) (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* Hdinsightiga 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* Hdinsightiga 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* SDK 1.5.8

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Klaster tüüp (nt Hadoopi, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDP vaikeversiooniks HDP 2.2 muutunud | HDP 2.2 vaikeversiooniks Hdinsightiga Windowsi kogumite jaoks on muutunud. Hdinsightiga versioon 3,2 (HDP 2.2) on üldiselt saadaval alates veebruar 2015. Selle muudatuse ainult ümberpööramisel liigub kaasa vaikeversiooniks kobar, kui ka konkreetsete valiku ei ole esitatud ajal ettevalmistamise Azure portaali, PowerShelli cmdlet-käskude või SDK klaster. | Teenus    | Kõik| N/A                  |
|Muudatuste juurutamine mitme Hdinsightiga Linux VM nimevormingut kogumite ühe virtuaalse võrgus | Mitme Hdinsightiga Linux kogumite ühe virtuaalse võrku juurutamine tugi lisatakse see väljaanne. Osa, virtuaalse masina nimevormingu klaster on muutunud headnode\*, workernode\* ja zookeepernode\* abil hn\*, wn\*, ja zk\* vastavalt. See ei ole soovitatav teha otse sõltuvus virtuaalse masina nimevormingu, kuna see on muutuda. Kasutage loendit ja hakkab majutama komponendid hosts vastendust määratlemiseks "hostname -f" kohalikus arvutis või Ambari API-d. Võite leida rohkem teavet [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) ja [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md). | Teenus | Hdinsightiga kogumite Linux | N/A |
| Konfiguratsioonimuudatuste | Hdinsightiga 3,1 kogumite, on nüüd lubatud järgmised: <ul><li>Tez.Yarn.ATS.Enabled ja yarn.log.server.url. See võimaldab rakenduste ajaskaala serveri ja sisse logida server saaks kasutada välja logid.</li></ul>Hdinsightiga 3,2 kogumite, muudetud järgmised: <ul><li>mapreduce.fileoutputcommitter.algorithm.Version on seatud 2. See võimaldab kasutada funktsiooni FileOutputCommitter V2 versiooni.</li></ul> | Teenus | Kõik | N/A |


## <a name="notes-for-09092015-release-of-hdinsight"></a>Märkmete 09/09/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.675.1768697 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga 3.0.6.675.1768697 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga 3.1.4.675.1768697 (HDP 2.1.15.0-2334 - muutmata)
* Hdinsightiga 3.2.6.675.1768697 (HDP 2.2.6.1-0012 - muutmata)
* SDK 1.5.8

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Klaster tüüp (nt Hadoopi, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Kõik Hdinsightiga kogumite Hdinsightiga uuendatud | Selles väljaandes värskendatud Hdinsightiga versioonid | Teenus    | Kõik| N/A                  |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Märkmete 07/31/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.640.1695824 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga 3.0.6.640.1695824 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga 3.1.4.640.1695824 (HDP 2.1.15.0-2334 - muutmata)
* Hdinsightiga 3.2.6.640.1695824 (HDP 2.2.6.1-0012 - muutmata)
* SDK 1.5.8

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Klaster tüüp (nt Hadoopi, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Säde kobar sõlm uuesti kujutise töövoo parandamine | Fikseeritud viga, mis põhjustas säde kobar sõlmed pole pärast uuesti piltide taastamine | Teenus    | Säde| N/A                  |


## <a name="notes-for-07312015-release-of-hdinsight"></a>Märkmete 07/31/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.635.1684502 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga 3.0.6.635.1684502 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga 3.1.4.635.1684502 (HDP 2.1.15.0-2334 - muutmata)
* Hdinsightiga 3.2.6.635.1684502 (HDP 2.2.6.1-0012 - muutmata)
* SDK 1.5.8

See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Klaster tüüp (nt Hadoopi, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Kõik Hdinsightiga kogumite Hdinsightiga uuendatud | Selles väljaandes värskendatud Hdinsightiga versioonid | Teenus    | Kõik| N/A                  |


## <a name="notes-for-07072015-release-of-hdinsight"></a>Märkmete 07/07/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.610.1630216 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga 3.0.6.610.1630216 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga 3.1.4.610.1630216 (HDP 2.1.15.0-2334 - muutmata)
* Hdinsightiga 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* SDK 1.5.8


See väljaanne sisaldab järgmised värskendused.

| Pealkiri                                           | Kirjeldus                                          | Kannatavas ala (nt teenuse, komponent või SDK) | Klaster tüüp (nt Hadoopi, HBase või torm) | JIRA (vajadusel) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDP uuendatud Hdinsightiga 3,2 kogumite | Selles versioonis, kasutab Hdinsightiga 3,2 HDP 2.2.6.1-0012 | Teenus    | Kõik                                                 | N/A                  |


## <a name="notes-for-06262015-release-of-hdinsight"></a>Märkmete 26-06-2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.601.1610731 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga 3.0.6.601.1610731 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga 3.1.4.601.1610731 (HDP 2.1.15.0-2334 - muutmata)
* Hdinsightiga 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* SDK 1.5.8


See väljaanne sisaldab järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Kannatavas ala (nt teenuse, komponent või SDK)</p></th>
<th>Klaster tüüp (nt Hadoopi, HBase või torm)</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>
<td>HDP uuendatud Hdinsightiga 3,2 kogumite</td>
<td>Selles versioonis, kasutab Hdinsightiga 3,2 HDP 2.2.6.1</td>
<td>Teenus</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>Märkmete 18-06-2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.596.1601657 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga 3.0.6.596.1601657 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* Hdinsightiga 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* SDK 1.5.8


See väljaanne sisaldab järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Kannatavas ala (nt teenuse, komponent või SDK)</p></th>
<th>Klaster tüüp (nt Hadoopi, HBase või torm)</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>
<td>Avatud täiendavate HTTPS-pordid</td>
<td>Pilveteenusesse nüüd avaneb 5 kobar nt 8001 abil 8005 pordid veebisaidil https://<clustername>.azurehdinsight.net:8001/. Idest taotluste autentimise kasutades sama elementaarautentimine parooli süsteem nimega pordi 443. Aktiivse headnode sama porti siduda järgmised pordid. Skripti toimingud saab teha klienditeeninduse kuulavad headnode ja marsruudi väljaspool klaster järgmised pordid.</td>
<td>Pilveteenuses</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>Vahelduva MapReduce kaartide segamise probleemi Hdinsightiga 3,2</td>
<td>Lahendus harva, vahelduva võistluse tingimuse kaardi vähendada kaartide segamise suurtes rühmades tulemuseks aeg-ajalt tööülesande tõrgete kohta. Lisateabe saamiseks vaadake <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE-6361</a> .</td>
<td>Hadoopi Core</td>
<td>Kõik</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE-6361</a></td>
</tr>

<tr>
<td>Liikumine uusim Azure Java Hdinsightiga 3,2 SDK 2.2</td>
<td>Java kasutatavaid WASB draiver teisaldada Azure'i SDK uusim versioon. Viimane SDK on mõned parandused ja väljalaskemärkmed sama on saadaval veebisaidil https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt.</td>
<td>Hadoopi Core</td>
<td>Kõik</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOPI-11959</a></td>
</tr>

<tr>
<td>Liikumine HDP 2.1.15 Hdinsightiga 3,1 kogumite jaoks</td>
<td>Hortonworks väljalaskemärkmed vabastamiseks on saadaval <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">siin</a>.</td>
<td>HDP</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>Märkmete 04-06-2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.583.1575584 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga 3.0.6.583.1575584 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga 3.1.3.583.1575584 (HDP 2.1.12.1-0003 - muutmata)
* Hdinsightiga 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* SDK 1.5.8


See väljaanne sisaldab järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Kannatavas ala (nt teenuse, komponent või SDK)</p></th>
<th>Klaster tüüp (nt Hadoopi, HBase või torm)</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>
<td>502 vale lüüs tõrke Storm kogumite Fix</td>
<td>See väljaanne parandused mõjutamata töö esitamise API, mis veebisaidi pärast uuesti alla olevat põhjustatud viga.</td>
<td>Teenus</td>
<td>Torm</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>Märkmete 06/01/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.577.1563827 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga 3.0.6.577.1563827 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga 3.1.3.577.1563827 (HDP 2.1.12.1-0003 - muutmata))
* Hdinsightiga 3.2.4.577.1563827 (HDP 2.2.6.0-2800 - muutmata)
* SDK 1.5.8


See väljaanne sisaldab järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Kannatavas ala (nt teenuse, komponent või SDK)</p></th>
<th>Klaster tüüp (nt Hadoopi, HBase või torm)</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>
<td>Erinevate veaparandusi</td>
<td>See väljaanne parandused kobar ettevalmistamise seotud vigu.</td>
<td>Teenus</td>
<td>Kõigi kobar</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>Märkmete 05/27/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* Muud kobar versioonid ja SDK on juurutatud selles versioonis osana.


See väljaanne sisaldab järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Kannatavas ala (nt teenuse, komponent või SDK)</p></th>
<th>Klaster tüüp (nt Hadoopi, HBase või torm)</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>
<td>HDP 2.2 värskendamine</td>
<td>Selle versiooni Hdinsightiga 3,2 sisaldab HDP 2.2.6 ja toob Hdinsightiga mitu olulist veaparandused. Täielik väljalaskemärkmed on saadaval veebisaidil <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 vabastage märkmete</a>.</td>
<td>HDP</td>
<td>Kõigi kobar</td>
<td>N/A</td>
</tr>

<tr>
<td>Vaikimisi lõng Container mälu konfiguratsiooni muutmine</td>
<td>Selle värskenduse vaikimisi saadaval mälu LÕNG ümbriste (yarn.nodemanager.resource.memory-mb ja yarn.scheduler.maximum-eraldatud-mb), et käivitatud sõlm Manager, suurendatakse 5632 MB. Varem see vähendatud 4608 MB, kuid erinevad töö käivitatakse põhjal, on uus väärtus peab pakkumine paremini töökindlust ja jõudlust enamik projektidele, seega on parem vaikimisi. Tavaliselt, kui teil on kriitiline sõltuvus on see mälu konfiguratsiooni, seadke konkreetselt klaster loomisel.</td>
<td>HDP</td>
<td>Kõigi kobar</td>
<td>N/A</td>
</tr>

<tr>
<td>Torm kogumite ja HBase Config võrdse vaikeväärtus</td>
<td>Selle värskenduse taastab Hbase ja torm kogumite kasutamiseks Hadoopi kogumite LÕNG configs samad väärtused. Seda saab teha võrdse üle kõik kobar tüübid.</td>
<td>HDP</td>
<td>HBase torm</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>Märkmete 05/20/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.564.1542093 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga 3.0.6.564.1542093 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* Hdinsightiga 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* SDK 1.5.8

See väljaanne sisaldab järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Kannatavas ala (nt teenuse, komponent või SDK)</p></th>
<th>Klaster tüüp (nt Hadoopi, HBase või torm)</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>
<td>SCP.NET EventHub tugi</td>
<td>Värskendatud kobar pakettide Hdinsightiga Storm tuua SCP.NET uusi funktsioone. Teil on nüüd uus API topoloogia Builder, mis hõlbustavad EventHubSpout või Java Spouts juurdepääsu. Peate värskendama oma SCP.NET kliendi SDK töötamiseks uue kogumite, nagu lepingud on värskendatud. Uue API-de kohta täpsema teabe saamiseks kasutus- ja Väljalaske märkmed (sh veaparandused) vaadake Readme SCP.NET Nugeti paketis.</td>
<td>VS instrumentaarium</td>
<td>Torm Hdinsightiga 3,2 kogumite</td>
<td>N/A</td>
</tr>

<tr>
<td>JDBC draiveri värskendus</td>
<td>Toetatud SQL serveri versiooni sqljdbc_4.1.5605.100 juht värskendada.</td>
<td>Metastore</td>
<td>Kõik</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>Märkmete 04-27-2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.537.1486660 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga 3.0.6.537.1486660 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga 3.1.3.537.1486660 (HDP 2.1.12.0-2329 - muutmata)
* Hdinsightiga 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* SDK 1.5.8

See väljaanne sisaldab järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Kannatavas ala (nt teenuse, komponent või SDK)</p></th>
<th>Klaster tüüp (nt Hadoopi, HBase või torm)</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>
<td>DLL sõltuvus parandamine</td>
<td>Eemaldab Hdinsightiga sõltuvus ühiku Test raames.</td>
<td>SDK</td>
<td>Hadoopi</td>
<td>N/A</td>
</tr>

<tr>
<td>Viga lahendus võistluse tingimus</td>
<td>Klaster loomine taotluse nüüd ootab panna taotluse vastu enne küsitlused oleku kohta</td>
<td>SDK</td>
<td>Hadoopi</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>Märkmete 14-04-2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - muutmata)
* Hdinsightiga 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK 1.5.6

See väljaanne sisaldab järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Kannatavas ala (nt teenuse, komponent või SDK)</p></th>
<th>Klaster tüüp (nt Hadoopi, HBase või torm)</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>
<td>Tez veaparandusi</td>
<td>Selle versiooni HDI 3,2 kaasatakse parandustega Apache TEZ 2214 ja TEZ 1923. Need on vajalikud teatud päringute taru Tez mis segamine palju andmeid on vaja.
</td>
<td>HDP</td>
<td>Hadoopi</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>Märkmete 04-06-2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - muutmata)
* Hdinsightiga 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - muutmata)
* Hdinsightiga 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - muutmata)
* Hdinsightiga 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK 1.5.6

See väljaanne sisaldab järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Kannatavas ala (nt teenuse, komponent või SDK)</p></th>
<th>Klaster tüüp (nt Hadoopi, HBase või torm)</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>
<td>Hdinsightiga .NET SDK 1.5.6</td>
<td>Värskenduste eemaldada teatud sisemise klassi Hdinsightiga Linuxi jaoks.</td>
<td>SDK</td>
<td>Hadoopi</td>
<td>N/A</td>
</tr>

<tr>
<td>Avro teegi 1.5.6</td>
<td>Lisatud <b>loendisse KnownTypeAttribute</b> <b>GetAllKnownTypes</b>meetod. Fikseeritud NullReferenceException kui tüüp on null GetAllKnownTypes meetod.</td>
<td>SDK</td>
<td>Hadoopi</td>
<td>N/A</td>
</tr>

<tr>
<td>Veaparandusi</td>
<td>Erinevate veaparandusi teenus</td>
<td>Teenus</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-04012015-release-of-hdinsight"></a>Märkmete 04/01/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* Hdinsightiga 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* Hdinsightiga 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* Hdinsightiga 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* SDK 1.5.5

See väljaanne sisaldab järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Kannatavas ala (nt teenuse, komponent või SDK)</p></th>
<th>Klaster tüüp (nt Hadoopi, HBase või torm)</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>
<td>Võimalus lubada või keelata Remote'i töölaua mandaati Windows kogumite .NET SDK kaudu</td>
<td>Programmiline toe lubamine või keelamine Windowsi kogumite RDP mandaat.</td>
<td>SDK</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>Võimalus lubada Remote'i töölaua mandaat kogumite, samal ajal, kui nad ettevalmistatud</td>
<td>Programmiline toe lubamine Remote'i töölaua mandaati klaster loomisel. See eemaldab kontrollimiseks, esmalt ettevalmistamise klaster ja seejärel lubada Kaugtöölaud.</td>
<td>SDK</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>Uuele versioonile üle viidud Python 2.7.8 abil</td>
<td>Uuele versioonile üle viidud Python Python 2.7.8, et Hdinsightiga kogumite, mis sisaldab mõned olulised turvalisus Hdinsightiga versioonide 2.1, 3.0, 3,1 ja 3,2 parandused</td>
<td>Teenus</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>LÕNG konfiguratsiooni muutmine</td>
<td>Muudetud LÕNG konfiguratsiooni yarn.resourcemanager.max-lõpetatud-rakenduste 1000 Hdinsightiga versioonid 3,1 ja 3,2 kobar kõigi jaoks. See väärtus määrab ainult LÕNG UI lõplikus rakenduste loendit. Enne UI kuvatakse programmide loendis esitatud rakenduste kohta teabe saamiseks, saate otse minna ajalugu Server.</td>
<td>LÕNG</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>Mõne HBase kobar sõlmed suuruse muutmine</td>
<td>HBase kogumite nüüd luba suurust sõlmed (üles) Hdinsightiga versioonid 3,1 ja 3,2</td>
<td>Teenus</td>
<td>HBase</td>
<td>N/A</td>
</tr>

<tr>
<td>JDBC versioonitäiendus</td>
<td>SQL-i JDBC draiveri täiendatakse versioon sqljdbc_4.0.2206.100 Hdinsightiga versioon 3,2. See versioon sisaldab olulist turbeparandusi.</td>
<td>HDP</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>TÖÖTAB konfiguratsiooni värskendamine</td>
<td>Värskendatud töötab konfiguratsiooni networkaddress.cache.ttl 300 sekundi -1 Hdinsightiga versioonid 3,1 ja 3,2 vaikeväärtust. See väärtus määrab vahemällu poliitika eduka nime otsingud teenuse nimi. See lahendab kasvab ja kahanemine HBase kogumite seotud viga.</td>
<td>Teenus</td>
<td>HBase</td>
<td>N/A</td>
</tr>

<tr>
<td>Varasema versiooni täiendamine versiooniks Azure Storage SDK Java 2.0</td>
<td>Hdinsightiga versioon 3,2 täiendamist kasutada Java Azure'i salvestusruumi SDK uusim versioon. See sisaldab mitu olulist veaparandused üle praeguse 0.6.0 versioon.</td>
<td>HDP</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>Versioonile Apache WASB lähtekoodi</td>
<td>Hdinsightiga 3,2 nüüd kasutab WASB failisüsteemi juhi Apache Hadoop-koodi uusim versioon. Selle muudatuse koos WASB draiver on nüüd pakitud nimega eraldi jar. See on puhtalt pakendit muutmine ja käitumine WASB juhi muudatusi ei sisalda. Selle JAR faili nimi on hadoop-azure-2.6.0.jar.</td>
<td>HDP</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>Jar Hdinsightiga 3,2 failinime värskendused</td>
<td>Selle värskenduse Hdinsightiga versioon 3,2 sisaldab mitmeid veaparandusi ja uuendamist mõne sisemise purgid pakitud HDP osana. Pange tähele, et need JAR failid sisemise HDP paketi ja mitte kliendi rakenduste otseseks kasutamiseks. Rakenduste peaks oma versiooni purgid paketti nii, et HDP sisemise purgid versiooniuuendus Murra kliendi rakendused.</td>
<td>HDP</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-03032015-release-of-hdinsight"></a>Märkmete 03/03/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.488.1375841 (HDP 1.3.9.0-01351 - muutmata)
* Hdinsightiga 3.0.6.488.1375841 (HDP 2.0.9.0-2097 - muutmata)
* Hdinsightiga 3.1.3.488.1375841 (HDP 2.1.10.0-2290 - muutmata)
* Hdinsightiga 3.2.3.488.1375841 (HDP-2.2.10.0-2340 - muutmata)
* SDK 1.5.0 (muutmata)

See väljaanne sisaldab järgmisi update.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Kannatavas ala (nt teenuse, komponent või SDK)</p></th>
<th>Klaster tüüp (nt Hadoopi, HBase või torm)</th>
<th>JIRA</th>
</tr>


<tr>
<td>Töökindluse täiustamine</td>
<td>Oleme tehtud parandused, mis võimaldavad teenuse mastaapimiseks paremini suurema koormusega suhtes kobar loomine.</td>
<td>Teenus</td>
<td>Kõik</td>
<td>N/A</td>
</tr>



</table>
<br>

## <a name="notes-for-02182015-release-of-hdinsight"></a>Märkmete 02/18/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.471.1342507 (HDP 1.3.9.0-01351 - muutmata)
* Hdinsightiga 3.0.6.471.1342507 (HDP 2.0.9.0-2097 - muutmata)
* Hdinsightiga 3.1.3.471.1342507 (HDP 2.1.10.0-2290 - muutmata)
* Hdinsightiga 3.2.3.471.1342507 (HDP-2.2.10.0-2340)
* SDK 1.5.0

See väljaanne sisaldab järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Kannatavas ala (nt teenuse, komponent või SDK)</p></th>
<th>Klaster tüüp (nt Hadoopi, HBase või torm)</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>
<td>Hdinsightiga 3,2 kogumite</td>
<td>Hadoopi 2.6/HDP2.2 on saadaval Hdinsightiga 3,2 kogumite. See sisaldab peamised versiooniuuendused kõigile avatud lähtekoodi komponendid. Lisateavet leiate teemast, mis on uut Hdinsightiga ja <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">HDP 2.2.0.0 väljalaskemärkmed</a>.</td>
<td>Avatud allikaga tarkvara</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>Hdinsightiga Linux (eelvaade)</td>
<td>Kogumite loovad töötab Ubuntu Linux. Leiate üksikasjalikumat teavet teemast alustamine Hdinsightiga Linux.</td>
<td>Teenus</td>
<td>Hadoopi</td>
<td>N/A</td>
</tr>

<tr>
<td>Torm üldine kättesaadavus</td>
<td>Apache Storm kogumite tavaliselt on saadaval. Täpsema teabe saamiseks vt alustamine Storm kasutamine Hdinsightile.</td>
<td>Teenus</td>
<td>Torm</td>
<td>N/A</td>
</tr>

<tr>
<td>Virtuaalse masina suurus</td>
<td>Azure Hdinsightiga on saadaval rohkem virtuaalse masina tüübid ja suurused. Hdinsightiga saab kasutada A2 A7 suurused ehitatud üldine eesmärgil; D-sarja sõlmed, mis sisaldavad välkdraivide (SSD) ja 60 protsenti kiirem protsessor; ja A8 ja A9 suurused, millel on InfiniBand tugi kiiresti Networking.</td>
<td>Teenus</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>Skaleerimist kobar</td>
<td>Saate muuta andmete sõlmed töötava Hdinsightiga kobar arv ilma kustutada või selle uuesti luua. Praegu ainult Hadoopi päringu ja Apache Storm kobar tüübid on seda võimalust, kuid tugi Apache HBase kobar tüüp on kiiresti jälgimiseks. Lisateabe saamiseks vt haldamine Hdinsightiga kogumite.</td>
<td>Teenus</td>
<td>Hadoopi torm</td>
<td>N/A</td>
</tr>

<tr>
<td>Visual Studio instrumentaarium</td>
<td>Lisaks Apache Storm tööriistasid lõpule jõudnud, on tööriistasid Apache taru Visual Studio on värskendatud lause lõpetamise, kohaliku valideerimise ja täiustatud silumine tugi. Lisateavet leiate teemast alustamine Hdinsightiga Hadoopi tööriistad Visual Studio.</td>
<td>Instrumentaarium</td>
<td>Hadoopi</td>
<td>N/A</td>
</tr>

<tr>
<td>Hadoopi DocumentDB konnektor</td>
<td>Hadoopi konnektor DocumentDB, kus saate teha keerukate liitmised, analüüsi ja toiminguid üle skeemi-vähem JSON dokumendid on salvestatud DocumentDB saidikogumite üle või üle andmebaasi kontod. Lisateabe saamiseks ja õpetus leiate teemast käivitada Hadoopi töö DocumentDB ja Hdinsighti kaudu.</td>
<td>Teenus</td>
<td>Hadoopi</td>
<td>N/A</td>
</tr>

<tr>
<td>Veaparandusi</td>
<td>Oleme teinud eri väikesed veaparandused Hdinsightiga teenuste jaoks. Kliendi-ühendusega käitumine muudatusi oodatakse.</td>
<td>Teenus</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-02062015-release-of-hdinsight"></a>Märkmete 02-06-2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.463.1325367 (HDP 1.3.9.0-01351 - muutmata)
* Hdinsightiga 3.0.6.463.1325367 (HDP 2.0.9.0-2097 - muutmata)
* Hdinsightiga 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* SDK N/A

See väljaanne sisaldab järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Kannatavas ala (nt teenuse, komponent või SDK)</p></th>
<th>Klaster tüüp (nt Hadoopi, HBase või torm)</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>
<td>Veaparandusi</td>
<td>Oleme teinud eri väikesed veaparandused Hdinsightiga teenuste jaoks. Kliendi-ühendusega käitumine muudatusi oodatakse.</td>
<td>Teenus</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>HDP 2.1 hooldustööd värskendamine</td>
<td>Hdinsightiga 3,1 värskendatakse HDP 2.1.10.0 juurutamiseks. Lisateavet leiate teemast <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">väljalaskemärkmed HDP-2.1.10</a>. </td>
<td>Avatud allikaga tarkvara</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>HDP binaarsed värskendused</td>
<td>Millist faili on värskendatud nimed HBase on mõned JAR failid. Nende failide JAR kasutatakse ettevõttesiseselt HBase, mistõttu on tõenäoline, et kliendid on sõltuv nende JAR failide nimed. Järgmised:
<ul>
<li>./lib/Jetty-6.1.26.hwx.jar</li>
<li>./lib/Jetty-sslengine-6.1.26.hwx.jar</li>
<li>./lib/Jetty-util-6.1.26.hwx.jar</li>
</ul>
</td>
<td>Avatud allikaga tarkvara</td>
<td>HBase</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-1292015-release-of-hdinsight"></a>Märkmete 1/29/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.455.1309616 (HDP 1.3.9.0-01351 - muutmata)
* Hdinsightiga 3.0.6.455.1309616 (HDP 2.0.9.0-2097 - muutmata)
* Hdinsightiga 3.1.2.455.1309616 (HDP 2.1.9.0-2196 - muutmata)
* SDK N/A

See väljaanne sisaldab järgmisi update.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Kannatavas ala (nt teenuse, komponent või SDK)</p></th>
<th>Klaster tüüp (nt Hadoopi, HBase või torm)</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>

<td>Veaparandusi</td>
<td>Oleme teinud mõned olulised veaparandusi, mis uuendamine Azure Hdinsightiga rühmad usaldusväärsuse parandamiseks.</td>
<td>Teenus</td>
<td>Kõik</td>
<td>N/A</td>
</tr>



</table>
<br>

## <a name="notes-for-152015-release-of-hdinsight"></a>Märkmete 1/5/2015 versiooni Hdinsightiga

Täisversioon numbrid Hdinsightiga kogumite juurutatud selles versioonis:

* Hdinsightiga 2.1.10.420.1246118 (HDP 1.3.9.0-01351 - muutmata)
* Hdinsightiga 3.0.6.420.1246118 (HDP 2.0.9.0-2097 - muutmata)
* Hdinsightiga 3.1.2.420.1246118 (HDP 2.1.9.0-2196 - muutmata)


See väljaanne sisaldab järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Komponent</th>
<th>Kobar tüüp</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>
<td>Proovide Twitteri trend ja Mahout-põhise filmi soovitused</td>
<td><p>See väljaanne Hdinsightiga päringu konsooli on kaks täiendavad proovi.</p>

<p><b>Twitteri trendi analüüs</b><br>
Avaliku API-de poolt saitidel nagu Twitter on kasulik andmete analüüsimine ja populaarsed trendide mõistmine. Selles õppetükis saate teada, kuidas kasutada taru saada enamik tweets, mis sisaldab kindla sõna saadetud Twitteri kasutajate loendit. </p>

<p><b>Mahout filmi soovitus</b><br>
Apache Mahout on Apache Hadoop arvutisse, mis Õppekeskuse teek. Mahout sisaldab algoritmide kohta (nt filtreerimine, liigitamine ja rühmitamise) andmete töötlemiseks. Selles õpetuses abil soovitus Mootor filmid, mida teie sõbrad näinud filmi soovitusi.</p></td>
<td>Päringu konsooli</td>
<td>Hadoopi</td>
<td>N/A</td>
</tr>

<tr>
<td>Vaikeväärtuse taru konfiguratsiooni muutmine: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>Selle konfiguratsiooni suurus kehtib automaatselt teisendatud kaardi ühendused. Väärtus tähistab suuruse, tabelid, mis saab teisendada räsi kaardid, mis sobivad mällu summa. Eelnevate versioonis see väärtus kasvanud vaikeväärtust, milleks on 10 MB 128 MB. Siiski põhjustas uus väärtus 128 MB töö nurjumise tähtaja puudumise mälu. See väljaanne taastatakse vaikeväärtuse tagasi 10 MB. Klientide saate endiselt selle valiku tühistada kobar loomise ajal, päringuid ja tabeli suurust. Selle sätte ja kuidas seda alistada kohta leiate lisateavet teemast <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">Optimeerimine automaatse liitumise teisendamine</a> Hortonworks dokumentatsiooni. </p></td>
<td>Taru</td>
<td>Hadoopi Hbase</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-12232014-release-of-hdinsight"></a>Märkmete 12/23/2014 versiooni Hdinsightiga

Selles versioonis juurutatud Hdinsightiga kogumite jaoks täisversioon arvud on:

* Hdinsightiga 2.1.10.420.1246783 (HDP versioon ei muutu)
* Hdinsightiga 3.0.6.420.1246783 (HDP versioon ei muutu)
* Hdinsightiga 3.1.1.420.1246783 (HDP versioon ei muutu)

See väljaanne sisaldab järgmisi update.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Komponent</th>
<th>Kobar tüüp</th>
<th>JIRA (vajadusel)</th>
</tr>


<tr>
<td>Vahelduva kobar loomine tõrgete tõttu liigse laadimine</td>
<td><p>Täiustatud algoritmi allalaadimiseks HDP pakettide kobar loomise ajal võimaldab senisest käepärasemad käsitlemine tõrgete tõttu liigse laadi.</p></td>
<td>Teenus</td>
<td>Hadoopi Hbase, torm</td>
<td>N/A</td>
</tr>



</table>
<br>

## <a name="notes-for-12182014-release-of-hdinsight"></a>Märkmete 12/18/2014 versiooni Hdinsightiga

See väljaanne sisaldab järgmisi komponent update.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Komponent</th>
<th>Kobar tüüp</th>
<th>JIRA (vajadusel)</th>
</tr>

<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">Kobar kohandamine üldine Avalability</a></td>
<td><p>Kohandamine pakub võimalust teil kohandada oma Windows Azure Hdinsightiga kogumite projektidega, mis on saadaval Apache Hadoop ökosüsteemi. Selle uue funktsiooniga saate katsetamiseks ja Juurutage Hadoopi projektide Windows Azure Hdinsightiga. See on lubatud – funktsiooni **Skripti toimingu** abil saate muuta Hadoopi kogumite suvalise viisil kohandatud skriptide abil. See kohandus on saadaval igat tüüpi Hdinsightiga kogumite, sh Hadoopi, HBase ja torm. See funktsioon võimsuse näitamiseks me on dokumenteerida installimiseks populaarsed <a href = "hdinsight-hadoop-spark-install.md" target="_blank">säde</a>, <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solri</a>ja <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a> moodulid protsess. See väljaanne lisab ka võimalus jaoks määrata oma kohandatud skript toimingu Azure portaali kaudu leiate juhised ja heade tavade kohta, kuidas luua kohandatud skript toimingute abil helper ja leiate juhised, kuidas testi skripti toimingu. </p></td>
<td>Funktsioon üldine kättesaadavus</td>
<td>Kõik</td>
<td>N/A</td>
</tr>


</table>
<br>

## <a name="notes-for-12052014-release-of-hdinsight"></a>Märkmete 12/05/2014 versiooni Hdinsightiga

Selles versioonis juurutatud Hdinsightiga kogumite jaoks täisversioon arvud on:

* Hdinsightiga 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* Hdinsightiga 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* Hdinsightiga 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* Hdinsightiga SDK N/A

See väljaanne sisaldab komponent järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Komponent</th>
<th>Kobar tüüp</th>
<th>JIRA (vajadusel)</th>
</tr>

<tr>
<td>Vea parandamine: vahelduva viga suure hulga sektsioonid taru DDL tabeli lisamine </td>
<td><p>Kui palju sektsioonide taru tabeli lisamisel on vahelduva ühenduse tõrge taru metastore andmebaasi, võib nurjuda taru DDL. Kui see tõrge ilmneb on taru tõrkelogi näha järgmine tekst: </p><p>"Tõrge [põhi]: ql. Draiveri (SessionState.java:printError(547)) - nurjus: saatja koodi 1 org.apache.hadoop.hive.ql.exec.DDLTask täitmise tõrge. MetaException (message:java.lang.RuntimeException: commitTransaction kutsuti, kuid openTransactionCalls = 0. Seda tõenäoliselt näitab, et on tasakaalustamata kõned openTransaction/commitTransaction)"</p></td>
<td>Taru</td>
<td>Hadoopi Hbase</td>
<td>TARU-482 (see on ka sisemise JIRA nii, et seda ei saa kommenteeritud väliselt. Märkida Reference.)</td>
</tr>

<tr>
<td>Vea parandamine: aeg-ajalt hanguda Hdinsightiga päringu konsooli</td>
<td>Kui see juhtub, näha WebHCat Logi WebHCat käivitit töö järgmine tekst: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper | org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): ei saa laadida ajaloo faili {ajaloo faili wasb URL-i} "</p></td>
<td>WebHCat</td>
<td>Hadoopi</td>
<td>TARU-482 (see on ka sisemise JIRA nii, et seda ei saa kommenteeritud väliselt. Märkida Reference.)</td>
</tr>

<tr>
<td>Vea parandamine: aeg-ajalt kühvli rakenduses latentsuse Hbase päringud</td>
<td>Sel juhul kasutajad sisse Hbase päringute latentsus teade ka aeg-ajalt kühvli 3 sekundit. </td>
<td>Lüüsi Hdinsightiga kobar</td>
<td>HBase</td>
<td>N/A</td>
</tr>

<tr>
<td>HDP JAR faili nime muudatused</td>
<td>Jaoks HDI klaster versiooni 3.0, on mitu installitud HDP sisemise JAR failide muudatustest. Jetty-6.1.26.jar on asendatud jetty-6.1.26.hwx.jar. Jetty-util-6.1.26.jar on asendatud jetty-util-6.1.26.hwx.jar. Need muudatused rakendada Hadoopi, Mahout, WebHCat ja Oozie projektid.</td>
<td>Hadoopi Mahout WebHCat Oozie</td>
<td>Hadoopi HBase</td>
<td>N/A</td>
</tr>

</table>
<br>


## <a name="notes-for-11212014-release-of-hdinsight"></a>Märkmete 11-21-2014 versiooni Hdinsightiga

Selles versioonis juurutatud Hdinsightiga kogumite jaoks täisversioon arvud on:

* Hdinsightiga 2.1.9.382.1169709 (arvutusalas 11/14/2014)
* Hdinsightiga 3.0.5.382.1169709 (arvutusalas 11/14/2014)
* Hdinsightiga 3.1.1.382.1169709 (arvutusalas 11/14/2014)
* Hdinsightiga SDK 1.4.0

See väljaanne sisaldab komponent järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Komponent</th>
<th>Kobar tüüp</th>
<th>JIRA (vajadusel)</th>
</tr>

<tr>
<td>Juurdepääsul logid</td>
<td>Võimalus programmiliselt loetlemine rakendusi, mis on käivitatud oma kogumite ning asjakohaste rakenduste kohased või container kohased logid aidata silumine probleemne rakenduste allalaadimiseks.</td>
<td>SDK</td>
<td>Hadoopi</td>
<td>N/A</td>
</tr>

<tr>
<td>Võimalus määrata regiooni nimi IHdInsightClient.DeleteCluster </td>
<td>Azure Hdinsightiga SDK pakub võimalus määrata regiooni nimi **DeleteCluster**kasutamisel. See aitab klientidele, kes oli kaks ressursid sama nimega eri regioonide ja ei saa kustutada üks neist blokeeringu tühistada.</td>
<td>SDK</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>ClusterDetails.DeploymentId</td>
<td>Objekti **ClusterDetails** tagastab **DeploymentID** välja, mis tähistab klaster ainuidentifikaator. See on tagatud olema kordumatu üle kobar loomise katsed sama nime.</td>
<td>SDK</td>
<td>Kõik</td>
<td>N/A</td>
</tr>
</table>
<br>

## <a name="notes-for-11142014-release-of-hdinsight"></a>Märkmete 11/14/2014 versiooni Hdinsightiga

Selles versioonis juurutatud Hdinsightiga kogumite jaoks täisversioon arvud on:

* Hdinsightiga 2.1.9.382.1169709
* Hdinsightiga 3.0.5.382.1169709
* Hdinsightiga 3.1.1.382.1169709

See väljaanne sisaldab järgmisi uusi funktsioone, komponendi värskendused ja veaparandused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Komponent</th>
<th>Kobar tüüp</th>
<th>JIRA (vajadusel)</th>
</tr>

<tr>
<td>Skripti toimingu (eelvaade)</td>
<td>Eelvaate kobar kohandamise funktsiooni, mis võimaldab Hadoopi muutmine kogumite suvalise viisil kohandatud skriptide abil. Kasutajad saavad selle funktsiooni eksperimenteerida ja projektide, mis on saadaval ökosüsteemi Apache Hadoop-Windows Azure Hdinsightiga kogumite juurutamine. Funktsioon kohandus on saadaval igat tüüpi Hdinsightiga kogumite, sh Hadoopi, HBase ja torm.</td>
<td>Uus funktsioon</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>Varemkoostatud tööd Azure veebisaitide ja salvestusruumi logige analüüs</td>
<td>Hdinsightiga päringu konsooli on alustamine Galerii, mis toetab lahendusi, mis töötavad oma andmete põhjal või näidisandmeid.
<p>**Lahendusi, mis töötavad oma andmete põhjal**:<br>
Oleme koostanud töö mõne enamlevinud andmete analüüsi stsenaariumid saada luua oma lahendusi. Töö andmete abil saate teada, kuidas see töötab. Kui olete valmis, kasutage seejärel on õpitu lahenduse, mis on kujundatud pärast Varemkoostatud töö loomiseks.</p>
<p>**Lahendusi, mis töötada Näidisandmete**:<br>
Saate teada, kuidas töötada Hdinsightiga kõndides läbi mõne lihtsa stsenaariumi (nt web logid ja andur andmete analüüsimine). Saate teada, Hdinsightiga kasutamine andmete analüüsimiseks ja kuidas saate luua ühenduse muude rakenduste ja teenuste andmed. Seda moodust power näide ühendatud Microsoft Excelis andmete visualiseerimine pakub.</p></td>
<td>Päringu konsooli</td>
<td>Hadoopi</td>
<td>N/A</td>
</tr>

<tr>
<td>Mälu leke Paranda Templeton</td>
<td>Põhjus rakenduses Templeton, mis kliendid, kellel oli kaua töötavad kogumite või olid esitamise 100s töö mõjutatud taotleb teine on adresseeritud. Probleemi väljendub Templeton 5xx tõrgete ja lahendus on teenuse taaskäivitamiseks. Lahendus ei ole enam vaja.</td>
<td>Templeton</td>
<td>Kõik</td>
<td>https://Issues.Apache.org/jira/Browse/HADOOP-11248</td>
</tr>
</table>
<br>


**Märkus**: uute võimaluste kobar kohandamine tehtud näitamaks kohta skripti toimingu abil saate installida säde ja R moodulid klaster toiminguid. Lisateabe saamiseks vt

* [Installimine ja kasutamine säde 1.0 Hdinsightiga kogumite](hdinsight-hadoop-spark-install.md)
* [Installimine ja kasutamine R Hdinsightiga Hadoopi kogumite](hdinsight-hadoop-r-scripts.md)




## <a name="notes-for-11072014-release-of-hdinsight"></a>Märkmete 11/07/2014 versiooni Hdinsightiga

Selles versioonis juurutatud Hdinsightiga kogumite täisversioon numbrid on:

* Hdinsightiga 2.1 2.1.9.374.1153876
* Hdinsightiga 3.0 3.0.5.374.1153876
* Hdinsightiga 3,1 3.1.1.374.1153876

See väljaanne sisaldab komponent järgmised värskendused.

<table border="1">
<tr>
<th>Pealkiri</th>
<th>Kirjeldus</th>
<th>Komponent</th>
<th>Kobar tüüp</th>
<th>JIRA (vajadusel)</th>
</tr>

<tr>
<td>HDP 2.1.7</td>
<td>Klõpsake Hortonworks andmete platvormi (HDP) 2.1.7 aluseks on selles versioonis. Lisateavet leiate teemast <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">HDP 2.1.7 väljalaskemärkmed</a>.</td>
<td>HDP</td>
<td>Kõik</td>
<td>N/A</td>
</tr>

<tr>
<td>LÕNG ajaskaala Server</td>
<td>Vaikimisi on lubatud LÕNG ajaskaala Server (tuntud ka kui üldise rakenduse ajalugu Server). Ajaskaala Server pakub üldist teavet lõpuleviidud rakenduste (nt rakenduse ID, rakenduse nimi, rakenduse olek, rakenduse esitamise aja ja rakenduse lõpetamise kellaaeg).

Selle rakenduse teabe võib laadida pea sõlme juurdepääs URI http://headnodehost:8188 või LÕNG käsu: lõng rakenduse – – appStates loendis kõik.

Selle teabe saab ka tuua eemalt kuigi REST API veebisaidil https://{ClusterDnsName}. azurehdinsight.net/ws/v1/applicationhistory/.

Üksikasjalikumat teavet leiate teemast <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">LÕNG ajaskaala Server</a>.</td>
<td>Teenuse LÕNG</td>
<td>Hadoopi HBase</td>
<td>N/A</td>
</tr>

<tr>
<td>Kobar juurutamise ID</td>
<td>Alustades SDK versioon 1.3.3.1.5426.29232, kasutajad pääsevad ainu-ID iga kobar, Hdinsightiga andnud. See võimaldab klientide kordumatu eksemplari kogumite välja selgitada, kui DNS-i nimi on uuesti kasutatakse kogu loomine või langev stsenaariumid.</td>
<td>SDK</td>
<td>Kõik</td>
<td>N/A</td>
</tr>
</table>
<br>

**Märkus**: viga, et vältida täisversioon arvu kuvamine portaalis või tagastatavate SDK või Windows PowerShelli on fikseeritud selles versioonis.

## <a name="notes-for-10152014-release"></a>Märkmete lubamise 10/15/2014

See paik väljaanne fikseeritud põhjus Templeton, keda Templeton raske kasutajad. Mõnel juhul näeksid kasutajad, kes kasutanud Templeton tugevalt tõrgete väljendub 500 tõrkekoodid Kuna taotlused ei oleks piisavalt mälu. Lahendus selle probleemi oli Templeton teenust uuesti. See probleem on lahendatud.


## <a name="notes-for-1072014-release"></a>Märkmete lubamise 10/7/2014

* Kui kasutate Ambari lõpp-punkti, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" *host_name* välja annab sõlme asemel ainult hostinimi täielik domeeninimi (FQDN). Näiteks tagastamise "**headnode0**", asemel saate FQDN "**headnode0. {} .Net ClusterDNS} .azurehdinsight**". See muudatus on hõlbustamiseks stsenaariumid, kus mitu kobar tüüpi (nt HBase ja Hadoopi) saab kasutada ühte virtuaalse võrku. See juhtub näiteks, kui kasutada HBase tagaandmebaas Platvormi Hadoopi.

* Oleme andnud uued mälu sätted Hdinsightiga kobar kasutuselevõtuks vaikimisi. Eelmise mälu vaikesätted ei pole piisavalt kontole CPU valdkond kasutusele arvu juhised. Uute mälu sätete peaks andma parema vaikesätted (ühe Hortonworks soovitused). Neid muuta, vaadake selle SDK dokumentides kobar konfiguratsiooni muutmise kohta. Järgmises tabelis on loetletud uued mälu sätted, mida kasutatakse vaikimisi 4 CPU core (8 container) Hdinsightiga kobar. (Enne selle väljaande kasutada väärtused on olemas ka parenthetically.)

<table border="1">
<tr><th>Komponent</th><th>Mälu jaotamine</th></tr>
<tr><td> Yarn.Scheduler.Minimum jaotamise</td><td>768 MB (varem 512 MB)</td></tr>
<tr><td> Yarn.Scheduler.Maximum jaotamise</td><td>6144 MB (muutmata)</td></tr>
<tr><td>Yarn.nodemanager.Resource.Memory</td><td>6144 MB (muutmata)</td></tr>
<tr><td>mapreduce.map.Memory</td><td>768 MB (varem 512 MB)</td></tr>
<tr><td>mapreduce.map.Java.opts</td><td>valib = Xmx512m (varem – Xmx410m)</td></tr>
<tr><td>mapreduce.reduce.Memory</td><td>1536 MB (varem 1024 MB)</td></tr>
<tr><td>mapreduce.reduce.Java.opts</td><td>valib = Xmx1024m (varem – Xmx819m)</td></tr>
<tr><td>Yarn.app.mapreduce.am.Resource</td><td>768 MB (varem 1024 MB)</td></tr>
<tr><td>Yarn.app.mapreduce.am.Command</td><td>valib = Xmx512m (varem – Xmx819m)</td></tr>
<tr><td>mapreduce.Task.IO.sort</td><td>256 MB (varem 200 MB)</td></tr>
<tr><td>Tez.am.Resource.Memory</td><td>1536 MB (muutmata)</td></tr>

</table><br>

LÕNG ja MapReduce Hortonworks andmete platvormi Hdinsightiga kasutatavat mälu konfiguratsiooni sätete kohta lisateabe saamiseks vaadake [määratleda HDP mälu](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html). Hortonworks olemas tööriista arvutamiseks proper mälu sätted.

Azure'i PowerShelli ja Hdinsightiga SDK tõrketeade: "*HTTP services juurdepääsu pole konfigureeritud kobar*":

* See tõrge on teadaolev [ühilduvusprobleemi](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) , mis võivad olla tingitud vahet Hdinsightiga SDK või Azure PowerShelli versiooni ja klaster versiooni. Kogumite loodud 8/15 või uuem versioon on uue ettevalmistamise võimalus üheks virtuaalse võrgu tugi. Kuid see funktsioon ei ole õigesti tõlgendada varasemate versioonide Hdinsightiga SDK või Azure PowerShelli. Tulem on mõned töö esitamine toimingutega tõrke. Kui kasutate Hdinsightiga SDK API-d või Azure PowerShelli cmdlet-käskude (**Kasuta-AzureRmHDInsightCluster** või **Invoke-AzureRmHDInsightHiveJob**) esitada töid, nende toimingute võib nurjuda tõrketeatega "*kobar <clustername> pole konfigureeritud HTTP services juurdepääsu*." Või (olenevalt toimingu), võite saada muid tõrketeateid, nt "*ei saa ühendust luua kobar*".

* Nende ühilduvusprobleemide lahendatakse Hdinsightiga SDK ja Azure PowerShelli uusimad versioonid. Soovitame värskendamine Hdinsightiga SDK versioon 1.3.1.6 või uuem versioon ja Azure PowerShelli tööriistad versiooni 0.8.8 või uuem versioon. Saate Accessi uusima Hdinsightiga SDK kaudu [](http://nuget.codeplex.com/wikipage?title=Getting%20Started) ja kuidas [installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md)Azure PowerShelli tööriistu.



## <a name="notes-for-9122014-release-of-hdinsight-31"></a>Märkmete 9/12/2014 versiooni Hdinsightiga 3,1

* Klõpsake Hortonworks andmete platvormi (HDP) 2.1.5 aluseks on selles versioonis. Loendi vead fikseeritud selles versioonis, [see väljaanne püsiv](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) lehelt leiate Hortonworks saidil.
* Klõpsake kaustas siga teekide "avro-mapred-1.7.4.jar" fail on muutunud "avro-mapred-1.7.4-hadoop2.jar." Selle faili sisu sisaldab mõnevõrra vigu parandada, mis on serveris. Soovitatav on kliendid teha otse sõltuvus vältida leheküljepiiride, kui failid on ümber nimetatud JAR faili nime.


## <a name="notes-for-8212014-release"></a>Märkmete lubamise 8/21/2014

* Lisame pärast WebHCat konfiguratsioon (TARU 7155), mis määrab Templeton kontrolleril töö vaikimisi mälu 1 GB. (Eelmise vaikeväärtust, milleks on 512 MB).

     Templeton.Mapper.Memory.MB (= 1024)

    * Selle muudatuse aadressid mis teatud taru päringute oli tekib tõttu alumise mälu piirangud järgmine tõrketeade: "Container töötab üle füüsilise mälu."
    * Vana vaikesätete taastamiseks saate määrata selle väärtus 512 Azure PowerShelli kaudu kobar loomise ajal järgmise käsu abil.

        Lisage AzureRmHDInsightConfigValues-Core@{"templeton.mapper.memory.mb"="512";}


* Hosti nimi zookeeper rolli muudeti *zookeeper*. See mõjutab nimelahendus klaster sees, kuid see ei mõjuta välise REST API-d. Kui teil on komponendid, mis kasutavad *zookeepernode* hosti nimi, peate värskendama neid kasutada uus nimi. Uute nimede kolme zookeeper sõlmed on:
    * zookeeper0
    * zookeeper1
    * zookeeper2
* HBase versiooni tugi maatriks on värskendatud. Hdinsightiga versioon 3,1 (HBase versioonist 0,98) on toetatud jaoks HBase töökoormus. Versiooni 3.0 (mis on saadaval eelvaade) ei toetata jooksva edasi.

## <a name="notes-about-clusters-created-prior-to-8152014"></a>Märkmete kohta kogumite loodud enne 8/15/2014

Azure'i PowerShelli või Hdinsightiga SDK tõrketeate, "kobar <clustername> pole konfigureeritud HTTP services juurdepääsu" (või toimingu, olenevalt muude veateateid näiteks: "Ei saa ühendust kobar") võivad tekkida Azure PowerShelli või Hdinsightiga SDK ja klaster versiooni vahe tõttu. Kogumite loodud 8/15 või uuem versioon on uue ettevalmistamise võimalus üheks virtuaalse võrgu tugi. See funktsioon pole õigesti tõlgendada varasemate versioonide Azure PowerShelli või Hdinsightiga Tarkvaraarenduskomplektist, mille tulemuseks on töö esitamise toimingute ebaõnnestumist. Kui kasutate esitada töid Hdinsightiga SDK API-d või Azure PowerShelli cmdlet-käskude (nt kasutamine – AzureRmHDInsightCluster või Invoke-AzureRmHDInsightHiveJob), ei pruugi mõne tõrketeated, mis on kirjeldatud need toimingud.

Nende ühilduvusprobleemide lahendatakse Hdinsightiga SDK ja Azure PowerShelli uusimad versioonid. Soovitame värskendamine Hdinsightiga SDK versioon 1.3.1.6 või uuem versioon ja Azure PowerShelli tööriistad versiooni 0.8.8 või uuem versioon. Saate avada access uusim Hdinsightiga SDK kaudu [Nugeti][nuget-link]. Azure'i PowerShelli tööriistad [Microsoft Web platvormi Installeri]abil pääsete[webpi-link].


## <a name="notes-for-7282014-release"></a>Märkmete lubamise 7/28/2014

* **Uue piirkondades saadaval Hdinsightiga**: me laiendatud Hdinsightiga kohaloleku kolme piirkondadele. Hdinsightiga kliendid saavad luua kogumite järgmistes regioonides:
    * Ida-Aasia
    * Põhja Kesk-USA
    * Lõuna-, Kesk-USA
* Hdinsightiga versioon 1,6 (HDP 1.1 ja Hadoopi 1.0.3) ja Hdinsighti versioon 2.1 (HDP1.3 ja Hadoopi 1.2) kõrvaldatakse Azure portaali. Saate jätkata loomine Hadoopi kogumite nende versioonide kasutades Azure PowerShelli cmdleti [New-AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx) või [Hdinsightiga SDK](http://msdn.microsoft.com/library/azure/dn469975.aspx)abil. Lugege lisateavet [Hdinsightiga komponendi Versioonimine](hdinsight-component-versioning.md) lehe.
* Hortonworks andmete platvormi (HDP) muutused selles versioonis:

<table border="1">
<tr><th>HDP</th><th>Muudatused</th></tr>
<tr><td>HDP 1.3 / HDI 2.1</td><td>Muudatusi</td></tr>
<tr><td>HDP 2.0 / HDI 3.0</td><td>Muudatusi</td></tr>
<tr><td>HDP 2.1 / 3,1 HDI</td><td>zookeeper: ["3.4.5.2.1.3.0-1948]-['3.4.5.2.1.3.2-0002] ></td></tr>


</table><br>

## <a name="notes-for-6242014-release"></a>Märkmete lubamise 6/24/2014

See väljaanne sisaldab teenuse Hdinsightiga täiustused:

* **HDP 2.1-saadavus**: Hdinsightiga 3,1 (mis sisaldab HDP 2.1) on üldiselt kättesaadav ja jaoks uus kogumite vaikeversiooniks.
* **HBase – Azure portaali täiustused**: teeme HBase kogumite eelvaates saadaval. Saate luua HBase kogumite portaalist vaid mõne hiireklõpsuga. 

HBase, kus saate koostada mitmesuguseid reaalajas töökoormus Hdinsightiga, klõpsake töötamine suurte andmekogumite Services miljoneid lõpp-punkti sensor ja telemeetria andmete salvestamiseks interaktiivse veebisaitidel. Järgmiseks oleks nende töökoormus Hadoopi töö andmete analüüsimiseks ja see on võimalik Hdinsightiga Azure PowerShelli ja taru kobar armatuurlaua kaudu.

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>Apache Mahout eelinstallitud Hdinsightiga 3,1

 [Mahout](http://hortonworks.com/hadoop/mahout/) on eelinstallitud Hdinsightiga 3,1 Hadoopi kogumite, siis saate kasutada Mahout töö ilma vajaduseta täiendavate kobar konfigureerimine. Näiteks saate remote on Hadoopi kobar sisse, kasutades kaugtöölaua protokolli (RDP) ja täiendavad juhised leiate ilma käivitada Tere, maailm Mahout järgmine käsk:

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L  

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Selle toimingu täielikuma kirjelduse, dokumentatsioonist [Breiman näide](https://mahout.apache.org/users/classification/breiman-example.html) Apache Mahout veebisaidil.


### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>Taru päringud saate kasutada Tez Hdinsightiga 3,1

Taru 0,13 on saadaval Hdinsightiga 3,1 ja see suudab käitada päringute abil Tez, mida saate kasutada olulisi jõudlustäiustusi.
Tez ei luba vaikimisi taru päringuid. Selle kasutamiseks peab valite. Saate lubada Tez, käitades järgmine koodilõigu.

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

Hortonworks on avaldatud üksikasjalik jaotus taru päringu jõudlustäiustused Tez standard kriteeriumid on esitatud. Lisateavet leiate teemast [võrdlemine Apache taru 13 Enterprise Hadoopi jaoks](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/).

Tez taru kasutamise kohta leiate lisateavet teemast [taru Tez kohta](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

###<a name="global-availability"></a>Üld-saadavus
Väljaandes Hdinsightiga kohta Hadoopi 2.2, Microsoft on käsutusse Hdinsightiga kõik põhi kaugemad, kus Azure on saadaval. Täpsemalt Lääne Euroopa ja Kagu-Aasia andmekeskuste on toodud võrgus. See võimaldab klientide leidmiseks kogumite andmekeskuses, mis on lähedal ja potentsiaalselt tsoon sarnaseid nõuetele.


###<a name="dos--donts-between-cluster-versions"></a>Käsud ja ära kobar erinevused

**Oozie metastores kasutada ka Hdinsightiga 3,1 kobar on Hdinsightiga 2.1 kogumite, kus pole tagasiühilduvad ja neid ei saa kasutada seda eelmise versiooni**.

Kohandatud Oozie metastore andmebaasi juurutatud koos mõne Hdinsightiga 3,1 kobar ei saa koos mõne Hdinsightiga 2.1 kobar taaskasutada. See kehtib ka siis, kui selle metastore, mis on pärit on Hdinsightiga 2.1 kobar. See stsenaarium ei toetata, kuna metastore skeemiga saab kasutamisel koos mõne Hdinsightiga 3,1 kobar, seega pole enam ühildub metastore, nõutud Hdinsightiga 2.1 rühmad. Iga katse uuesti kasutada Oozie metastore, on Hdinsightiga 3,1 kobar koos kasutatud muudab Hdinsightiga 2.1 kobar kasutuks.

**Oozie metastores ei saa üle kogumite ühiskasutusse anda.**

Oozie metastores on lisatud teatud kogumite ja nad ei saa ühiskasutusse anda kogumite üle.

###<a name="breaking-changes"></a>Suurte muudatuste

**Eesliite süntaks**: ainult selle "wasbs: / /" süntaks on Hdinsightiga 3,1 ja 3.0 kogumite toetatud. Vanemate "asv: / /" süntaks on toetatud Hdinsightiga 2.1 ja 1,6 kogumite, kuid see ei toeta Hdinsightiga 3,1 või 3.0 kogumite. See tähendab, et töökohtade mõne Hdinsightiga 3,1 või 3.0 kobar konkreetselt kasutavad funktsiooni "asv: / /" süntaks nurjub. Funktsiooni "wasbs: / /" süntaks tuleb kasutada. Lisaks, mis tahes Hdinsightiga 3,1 või 3.0 kogumite, mis on loodud olemasoleva metastore, konkreetsete viited abil sisaldava tööd on "asv: / /" süntaks nurjub. Nende metastores vaja uuesti luua, kasutades funktsiooni "wasbs: / /" ressursid aadressi süntaks.


**Pordid**: pordid, Hdinsightiga teenus on muutunud. Pordinumbrid, mida kasutati olid efemeersed port vahemikus Windowsi operatsioonisüsteemi. Pordid jagatakse eelmääratletud efemeersed vahemiku lühiajaline Internet Protocol (protokoll)-põhise side automaatselt. Uus komplekt lubatud Hortonworks andmete platvormi (HDP) teenuse pordinumbrid on väljaspool seda vahemikku konfliktide, mis võivad tekkida koos kasutatavate teenuste pea sõlme töötavate portide, et vältida. Uue pordinumbrid ei tohi põhjustada mis tahes server muudatused. Kasutatud arvud on järgmised:

 **Hdinsightiga 1,6 (HDP 1.1)**
<table border="1">
<tr><th>Nimi</th><th>Väärtus</th></tr>
<tr><td>DFS.http.Address</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.Secondary.http.Address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.Job.tracker.http.Address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.Task.tracker.http.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>mapreduce.History.Server.http.Address</td><td>0.0.0.0:31111</td></tr>
<tr><td>Templeton.Port</td><td>30111</td></tr>
</table><br>

 **Hdinsightiga 3,1 ja 3.0 (HDP 2.1 ja 2.0)**
<table border="1">
<tr><th>Nimi</th><th>Väärtus</th></tr>
<tr><td>DFS.namenode.http-aadress</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.namenode.https-aadress</td><td>headnodehost:30470</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.namenode.Secondary.http-aadress</td><td>0.0.0.0:30090</td></tr>
<tr><td>Yarn.nodemanager.WebApp.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>Templeton.Port</td><td>30111</td></tr>
</table><br>

###<a name="dependencies"></a>Sõltuvused

Järgmised sõltuvused lisati Hdinsightiga 3.x (HDP2.x):

* guice-servlet
* optiq-core
* javax.inject
* aktiveerimine
* jsr305
* Geronimo-jaspic_1.0_spec
* juuli-slf4j
* Java-xmlbuilder
* Ant
* Commonsi-koostaja
* JDO-api
* Commonsi-math3
* paranamer
* jaxb-impl
* stringtemplate
* eigenbase-erarannaga
* Jersey-servlet
* Commonsi-exec
* jaxb-api
* Jetty-kõik-server
* janino
* xercesImpl
* optiq-avatica
* Jetta
* eigenbase-atribuudid
* tore kõik
* hamcrest-core
* e-posti
* linq4j
* jpam
* Jersey-kliendi
* aopalliance
* Geronimo-annotation_1.0_spec
* Ant-käiviti
* Jersey-guice
* XML-API-d
* Stax-api
* ASM-Commonsi
* ASM-puu
* wadl
* Geronimo-jta_1.1_spec
* guice
* leveldbjni-all
* kiirus
* põhjustab
* käre java
* Jetty-all
* Commonsi-dbcp

Järgmised sõltuvused pole enam olemas Hdinsightiga 3.x (HDP2.x):

* jdeb
* KFS
* sqlline
* Ivy
* aspectjrt
* JSON
* Core
* jdo2-api
* Avro-mapred
* datanucleus-ja
* jsp
* Commonsi-logimine-api
* Matemaatika-Commonsi
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* käre

###<a name="version-changes"></a>Versiooni muudatused

Vahel Hdinsightiga tehtud järgmised versiooni muudatused 2.x (HDP1.x) ja Hdinsighti 3.x (HDP2.x):

* mõõdikute tuumaga: ["2.1.2]-["3.0.0] >
* derbynet: ["10.4.2.0]-['10.10.1.1] >
* datanucleus: ["rdbms-3.0.8'] -> [" rdbms-3.2.9']
* Jasper-koostaja: ["5.5.12]-['5.5.23] >
* log4j: ["1.2.15", "1.2.16"] -> ["1.2.16", "1.2.17"]
* derbyclient: ["10.4.2.0]-['10.10.1.1] >
* httpcore: ["4.2.4]-["4.2.5] >
* hsqldb: ["1.8.0.10]-["2.0.0] >
* jets3t: ["0.6.1]-["0.9.0] >
* protobuf-java: ["2.4.1]-["2.5.0] >
* Derby: ["10.4.2.0]-['10.10.1.1] >
* Jasper: ["käitusaja-5.5.12'] -> [" käitusaja-5.5.23']
* Commonsi-daemon: ["1.0.1]-["1.0.13] >
* datanucleus-core: ["3.0.9]-["3.2.10] >
* datanucleus-api-jdo: ["3.0.7]-["3.2.6] >
* zookeeper: ["3.4.5.1.3.9.0-01320]-['3.4.5.2.1.3.0-1948] >
* bonecp: ["0.7.1.RELEASE] -> ["
* 0.8.0.RELEASE "]


### <a name="drivers"></a>Draiverid
Java andmebaasi Connnectivity (JDBC) draiveri SQL serveri korral kasutatakse ettevõttesiseselt Hdinsightiga ja väliste toimingute puhul ei kasutata. Kui soovite ühenduse Hdinsightiga abil avatud andmebaasipöörduse (ODBC), kasutage Microsoft taru ODBC-draiver. Lisateabe saamiseks vt [Ühenduse loomine Exceli Hdinsightiga Microsoft taru ODBC-draiver](hdinsight-connect-excel-hive-odbc-driver.md).


### <a name="bug-fixes"></a>Veaparandusi

Selles versioonis, on meil värskendada järgmised Hdinsightiga versioonid koos mitme veaparandused:

* Hdinsightiga 2.1 (HDP 1.3)
* Hdinsightiga 3.0 (HDP 2.0)
* Hdinsightiga 3,1 (HDP 2.1)


## <a name="hortonworks-release-notes"></a>Hortonworks vabastage märkmed

Väljalaskemärkmed – Hortonworks andmete platvormid (HDPs), mis kasutavad Hdinsightiga versioon kogumite on saadaval järgmistes kohtades.

* Hdinsightiga versioon 3,1 kasutab Hadoopi jaotuse, mis põhineb [Hortonworks andmete platvormi 2.1.7][hdp-2-1-7]. See on vaikimisi Hadoopi kobar loodud Azure portaali kasutamisel pärast 11/7/2014. Enne 11/7/2014 põhinevad [Hortonworks andmete platvormi 2.1.1] loodud Hdinsightiga 3,1 kogumite[hdp-2-1-1]

* Hdinsightiga versiooni 3.0 kasutab Hadoopi normaaljaotust, [Hortonworks andmete platvormi 2.0]põhineva[hdp-2-0-8].

* Hdinsightiga versioon 2.1 kasutab Hadoopi normaaljaotust, [Hortonworks andmete platvormi 1.3]põhineva[hdp-1-3-0].

* Hdinsightiga versioon 1,6 kasutab Hadoopi normaaljaotust, [Hortonworks andmete platvormi 1.1]põhineva[hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
 
