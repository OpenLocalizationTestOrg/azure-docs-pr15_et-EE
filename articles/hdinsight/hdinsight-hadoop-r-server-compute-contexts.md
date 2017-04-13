<properties
   pageTitle="Arvutage kontekstis suvandid R server (eelvaade) Hdinsightiga | Microsoft Azure'i"
   description="Lisateavet erinevate Arvuta kontekstis saadaolevad suvandid kasutajatele R serveriga Hdinsightiga (eelvaade)"
   services="HDInsight"
   documentationCenter=""
   authors="jeffstokes72"
   manager="jhubbard"
   editor="cgronlun"
/>

<tags
   ms.service="HDInsight"
   ms.devlang="R"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-services"
   ms.date="10/18/2016"
   ms.author="jeffstok"
/>

# <a name="compute-context-options-for-r-server-on-hdinsight-preview"></a>Arvutage kontekstis suvandid R Server Hdinsightiga (eelvaade)

Microsoft Azure Hdinsightiga (eelvaade) R Server pakub uusimate võimaluste Analyticsi R-põhine. Seda kasutatakse andmeid, mis on talletatud HDFS nõus [Azure'i bloobimälu](../storage/storage-introduction.md "Azure'i bloobimälu") salvestusruumi konto või kohaliku Linuxi failisüsteemi. Kuna R Server on ehitatud avatud allika R, R-põhise taotluste koostate saate kasutada mis tahes avatud allika R pakettide 8000 +. Samuti saate ära kasutada funktsiooni praktikaid [mastaabimuundur](http://www.revolutionanalytics.com/revolution-r-enterprise-scaler "Revolutsiooni Analytics mastaabimuundur"), Microsofti suur andmete analytics paketi, mis on kaasas R Server.  

Serva sõlme Premium kobar pakub on mugav koht ühenduse klaster ja oma R skriptide käitamiseks. Soovitud serva sõlm teil võimalik töötab mastaabimuundur 's parallelized jaotatud funktsioone üle serva sõlm serveri soonte. Teil on olemas ka võimalus käivitada kogu klaster sõlmed abil mastaabimuundur 's Hadoopi kaarti vähendada või säde arvutada kontekstides.

## <a name="compute-contexts-for-an-edge-node"></a>Arvutage kontekstides jaoks soovitud serva sõlm

Üldiselt R skripti, serva sõlm R serveris töötavad töötab sees R Tõlgi sõlme. Erandiks on need toimingud, mis mastaabimuundur funktsiooni. Kõnede mastaabimuundur käivitage Arvuta keskkonnas, kus on määratud häälestusest mastaabimuundur Arvuta kontekstis.  R skripti käivitamisel kuvatakse serva sõlm Arvuta konteksti võimalikud väärtused on kohalik järjestikku ("kohaliku"), kohaliku samal ajal (localpar"), kaardi vähendamiseks ja säde.

Suvandi "kohaliku" ja "localpar" erinevad ainult kuidas rxExec kõned on täidetud. Mõlemad käivitada rx-funktsioon kõnede paralleelselt viisil üle kõik need on saadaval tuuma kui teisiti mastaabimuundur numCoresToUse võimalust, nt rxOptions(numCoresToUse=6) kaudu. Järgmiselt on toodud Arvuta kontekstis erinevaid võimalusi

| Arvutage kontekst  | Kuidas määrata                      | Täitmise kontekstis                                                                     |
|------------------|---------------------------------|---------------------------------------------------------------------------------------|
| Kohalik järjestikune | rxSetComputeContext('local')    | Parallelized täitmise üle südamikud serva sõlm server, välja arvatud rxExec kõned, mis on täidetud seeriatoodanguna |
| Kohaliku samal ajal   | rxSetComputeContext('localpar') | Parallelized täitmise üle südamikud serva sõlm server                                 |
| Säde            | RxSpark()                       | Parallelized jaotatud täitmise kaudu säde üle sõlmed HDI kobar      |
| Kaardi vähendamine       | RxHadoopMR()                    | Parallelized jaotatud täitmise kaudu kaardi vähendada üle sõlmed HDI kobar |


Eeldades, et soovite parallelized täitmise täitmiseks, siis on kolm võimalust. Laadi töid Kasutusanalüüsi ja suuruse ja asukoha andmete sõltub millise variandi valite.

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Juhised sobiva Arvuta kontekst

Praegu pole valem, mis ütleb teile, mis arvutada kontekstis kasutada. Siiski on mõned peamised põhimõtted, mis aitavad teil teha õige valik või vähemalt aitavad teil kitsendada oma valikuid, enne kui käivitate kriteerium. Suunavad põhimõtted järgmised:

1.  Kohaliku Linuxi failisüsteemi on kiirem kui HDFS.
2.  Korduvate analüüside on kiirem kui andmed on kohalik ja kui see on XDF.
3.  On soovitatav voona vähehaaval andmete teksti andmeallikast; kui suurt hulka andmeid on suurem, muuta see XDF enne analüüsi.
4.  Funktsiooni pea kohal kopeerimine või andmete analüüsimiseks serva sõlm streaming muutub väga suurte andmehulkade käsitamatu.
5.  Säde on kiiremini kaardi vähendada Hadoopi analüüsimiseks.

Anda need põhimõtted, on mõned üldised reeglid arv pöial valimise Arvuta kontekstis.

### <a name="local"></a>Kohaliku

- Kui andmete analüüsimiseks hulk on väike ja ei nõua korduvate analüüsi, taustal alla laadida otse tavapärase analüüsi ja kasutada "kohaliku" või "localpar".
- Kui andmete analüüsimiseks on väike või keskmise suurusega ja nõuab korduvate analüüsi, siis kopeerige see kohaliku failisüsteemi, importige see XDF, ja analüüsida "kohaliku" või "localpar" kaudu.

### <a name="hadoop-spark"></a>Hadoopi säde

- Kui andmete analüüsimiseks hulk on suur, importige see XDF HDFS sisse (kui salvestusruum pole probleemi), ja analüüsida "Säde" kaudu.

### <a name="hadoop-map-reduce"></a>Hadoopi kaardi vähendamine

- Kasutage ainult siis, kui teil tekib ületamatu probleem kasutamisega säde Arvuta kontekstis üldiselt juhul aeglasem.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Tekstisisene abi rxSetComputeContext

Vaadake teavet ja mastaabimuundur Arvuta konteksti näited, teksti sees, aitab r rxSetComputeContext meetodit, näiteks:

    > ?rxSetComputeContext

Saate ka viidata "Mastaabimuundur jaotatud arvutuste juhend", mis on saadaval [MSDN-i R Server](https://msdn.microsoft.com/library/mt674634.aspx "R Server MSDN-is") teegist.


## <a name="next-steps"></a>Järgmised sammud

Selles artiklis saate teada, kuidas luua uus Hdinsightiga klaster, mis sisaldab R Server. Soovite õpitut ka olulisema teabe R konsooli SSH seanss. Nüüd saate lugeda avastada töötamise R serveris Hdinsightiga muul viisil järgmistest artiklitest:

- [R Server Hadoopi ülevaade](hdinsight-hadoop-r-server-overview.md)
- [R server Hadoopi jaoks kasutamise alustamine](hdinsight-hadoop-r-server-get-started.md)
- [Hdinsightiga Premium RStudio serveri lisamine](hdinsight-hadoop-r-server-install-r-studio.md)
- [Azure'i talletamise võimalused R Server Hdinsightiga Premium](hdinsight-hadoop-r-server-storage.md)
