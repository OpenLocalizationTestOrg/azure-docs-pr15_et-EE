<properties
 pageTitle="Kuupäevatabelid ja nende kohta Hdinsightiga WebHCat tõrgete lahendamine"
 description="Siit saate teada, kuidas umbes levinud vigade tagastatud WebHCat Hdinsightiga kohta ja nende lahendamiseks ette võtta."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
 tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="09/27/2016"
 ms.author="larryfr"/>

#<a name="understand-and-resolve-errors-received-from-webhcat-templeton-on-hdinsight"></a>Mõista ja lahendada tõrkeid, mis on saadud WebHCat (Templeton), klõpsake Hdinsightiga

Tööta Hdinsightiga, WebHCat (varem Templeton,) abil saate tõrked. Selle dokumendi annab juhiseid levinud vigade – miks need tekivad ja kuidas need lahendada.

##<a name="what-is-webhcat"></a>Mis on WebHCat?

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) on REST API [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), tabeli ja salvestusruumi haldamine kiht Hadoopi jaoks. WebHCat on vaikimisi sisse Hdinsightiga kogumite ja edastab tööd, saada töö oleku jne logimata klaster kasutab mitmesuguseid tööriistu.

##<a name="modifying-configuration"></a>Konfiguratsiooni

> [AZURE.IMPORTANT] Mitut loetletud selle dokumendi tõrkeid ilmneda konfigureeritud maksimum on ületatud. Kui eraldusvõime etappi mainimised väärtuse saate muuta, saate selleks tuleb kasutada ühte järgmistest muutmine:

* Kogumite **Windowsi** jaoks: skripti toimingu abil saate konfigureerida väärtus kobar loomise ajal. Lisateabe saamiseks lugege teemat [arendada skript toimingud](hdinsight-hadoop-script-actions.md).

* Kogumite **Linuxi** jaoks: kasutamine Ambari (web või REST API) väärtust muuta. Lisateabe saamiseks vaadake [Hdinsightiga haldamine kasutamine Ambari](hdinsight-hadoop-manage-ambari.md)

###<a name="default-configuration"></a>Vaikimisi konfigureerimine

Järgnevalt on konfiguratsioon vaikeväärtused, mis võivad mõjutada jõudlust WebHCat või põhjustada tõrkeid, kui need väärtused on ületatud.

| Säte | Mida see tähendab? | Vaikeväärtus |
| ------- | ------------ | ------------- |
| [Yarn.Scheduler.Capacity.Maximum-rakendused][maximum-applications] | Tööd, mis võib olla aktiivne samaaegselt maksimumarv (ootel või töötava) | 10 000 |
| [Templeton.exec.Max-protsessikategooriad][max-procs] | Nõuab, et saate liitmise maksimumarv | 20 |
| [mapreduce.jobhistory.Max-vanus – ms][max-age-ms] | Mis säilitatakse varasem töökogemus päevade arvu. | 7 päeva |

##<a name="too-many-requests"></a>Liiga palju taotlusi

**HTTP olekukoodi**: 429

| Põhjus | Eraldusvõime |
| ----- | ---------- |
| Kui olete ületanud lubatud samaaegseid taotlusi, kätte WebHCat minutis (vaikimisi 20) | Tagada, et te ei esita oma töökoormus mitme samaaegseid taotlusi suurima arvu vähendada või suurendada samaaegseid taotluse limiit muutmine `templeton.exec.max-procs`. Lisateabe saamiseks vaadake [muutes konfigureerimine](#modifying-configuration) |

##<a name="server-unavailable"></a>Server pole saadaval

**HTTP olekukoodi**: 503

| Põhjus | Eraldusvõime |
| ---------------- | ------------------- |
| See esineb tavaliselt Tõrkesiirde klaster jaoks esmaseid ja teiseseid HeadNode vahel | Oodake kaks minutit ja seejärel korrake toimingut |

##<a name="bad-request-content-could-not-find-job"></a>Vigane päring sisu: ei leia töö

**HTTP olekukoodi**: 400

| Põhjus | Eraldusvõime |
| ---------------- | ------------------- |
| Töö üksikasjad on eemaldatud, töö ajalugu puhtamaks | Vaikimisi säilitatakse varasem töökogemus on 7 päeva. Seda saab muuta, muutes `mapreduce.jobhistory.max-age-ms`. Lisateabe saamiseks vaadake [muutes konfigureerimine](#modifying-configuration) |
| Töö on tapetud Tõrkesiirde tõttu | Proovige töö esitamise kuni kaks minutit. |
| Kasutati kehtetu töö ID-d | Kontrollige, kas töö id on õige |

##<a name="bad-gateway"></a>Vale lüüs

**HTTP olek kood**: 502

| Põhjus | Eraldusvõime |
| ---------------- | ------------------- |
| Sisemise Prügikoristus esineb WebHCat käigus | Oodake, kuni Prügikoristus lõpuni või taaskäivitage WebHCat teenus |
| Ajalõpp ResourceManager teenuse vastuse ootamine. See võib juhtuda siis, kui aktiivse taotluste arv läheb konfigureeritud maksimumi (vaikimisi 10 000) | Oodake, kuni praegu töötavate töö lõpuleviimiseks või suurendada samaaegne töö piirangut, muutes `yarn.scheduler.capacity.maximum-applications`. Lisateabe saamiseks vaadake [muutes konfigureerimine](#modifying-configuration)  |
| Kui proovite tuua kõiki töid kaudu [saada /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) kõne ajal `Fields` on seatud`*` | Saate tuua *Kõik* töö üksikasjad, selle asemel kasutada `jobid` tööde suurem ainult teatud töö id üksikasjade toomine. Või kasutage`Fields` |
| WebHCat teenus on alla HeadNode Tõrkesiirde ajal | Oodake, kuni kaks minutit ja proovige uuesti |
| On rohkem kui 500 ootel tööd WebHCat kaudu | Oodake enne esitamist rohkemate lõpetanud pooleli tööde haldamine |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
 
