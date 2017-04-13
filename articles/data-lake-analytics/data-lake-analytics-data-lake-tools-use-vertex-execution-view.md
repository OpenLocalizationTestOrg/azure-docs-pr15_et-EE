<properties 
   pageTitle="Andmetööriistad Lake tipp täitmise vaate kasutamine Visual Studio | Microsoft Azure'i" 
   description="Saate teada, kuidas kasutada tipp täitmise vaadet serdikontrolli Lake andmeanalüüsi projektidele." 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/13/2016"
   ms.author="jgao"/>

# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Visual Studio Andmetööriistad Lake tipp täitmise vaate kasutamine

Saate teada, kuidas kasutada tipp täitmise vaadet serdikontrolli Lake andmeanalüüsi projektidele.

## <a name="prerequisites"></a>Eeltingimused

- Mõned põhilised teadmised arendamise U-SQL-i skripti Data Lake Tools for Visual Studio abil.  Vt [õpetus: arendamise U-SQL-i skriptide abil andmete Lake tööriistad Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-the-vertex-execution-view"></a>Tipp täitmise vaatamiseks avage

Töö, võite klõpsata linki "Tipp täitmise View" alumises vasakus nurgas. Võidakse teil paluda laadida profiilid esmalt ja see võib võtta aega olenevalt teie võrguühendus.

![Tööriistade andmeanalüüsi Lake tipp täitmise kuvamine](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Mõista tipp täitmise kuvamine

Pärast sisestamine tipp täitmise vaadet, on kolm osa.

![Tööriistade andmeanalüüsi Lake tipp täitmise kuvamine](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

- Tipp Vaateselektori: vasakul on tipp Vaateselektori.  Saate valida tipud funktsioonid (nt ülemine 10 andmete lugemine, või valige etappi).

    Üks kõige levinum kasutatud filtrid on tipud kriitiline rada kohta. Kriitiline rada on pikima tee U-SQL-i töö. See on kasulik optimeerida oma tööd, märkides ruudu tipp, mis kestab kauem.

- Paani ülaosas:

    ![Tööriistade andmeanalüüsi Lake tipp täitmise kuvamine](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

    Siin kuvatakse kõik tipud töötava olekut. See muudab aeg vastavalt teie kohalikus arvutis ja kuvab erinevate olek eri värvidega.

- Alumine keskmisel paanil:

    ![Tööriistade andmeanalüüsi Lake tipp täitmise kuvamine](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

    - Protsessi nimi: Tipp eksemplari nimi. See koosneb erinevate osade egode | VertexName | VertexRunInstance. Näiteks SV7_Split [62] .v1 tipp tähistab teise käivitatud eksemplari (.v1, index, alates 0) etapi SV7_Split tipp numbri 62.
    - Kokku andmete lugemis-ja kirjutamisõigusega: Andmed on lugeda/kirjutada selle tipp.
    - Riigi-ja väljumis-olek: Lõplik olek kui tippu on lõpetatud.
    - Välju kood/tõrge tüüp: Kuvatakse tõrge, kui tippu nurjus.
    - Loomise põhjus: Miks on tipp on loodud.
    - Ressursi latentsus/protsessi latentsus/PN järjekorda latentsus: andmete töötlemiseks ja püsimiseks järjekorda tipp ootama ressursid, kuluv aeg.
    - Protsessi/looja GUID: GUID praeguse töötava tipp või looja.
    - Versioon: N-nda eksemplarile töötava tipp, (süsteemi võib kavandada uue eksemplari tipp, jaoks mitmel põhjusel, näiteks Tõrkesiirde arvutada koondamise jne.)
    - Versioonis loodud aeg.
    - Protsessi loomine Start kord, protsessi ootel kord, protsessi Start kord, protsessi lõpuleviimine aega: tipp protsessi käivitamisel loomine; tipp protsessi käivitamisel järjekorda; teatud tipp protsessi käivitamisel; Kui teatud tipp on lõpule viidud.

## <a name="next-steps"></a>Järgmised sammud

- Andmeanalüüsi Lake ülevaate saamiseks vt [Azure'i andmeanalüüsi Lake ülevaade](data-lake-analytics-overview.md).
- Alustamiseks U-SQL-i rakenduste arendamise, lugege teemat [arendada U-SQL-i skriptide abil andmete Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- A-SQL-is leiate teemast [Azure andmete Lake Analytics U-SQL-i keele kasutamise alustamine](data-lake-analytics-u-sql-get-started.md).
- Haldamise toiminguid, vt [haldamine Azure'i Lake andmeanalüüsi abil Azure portaali](data-lake-analytics-manage-use-portal.md).
- Logiteave diagnostika vt [juurdepääs diagnostika logid Azure'i Lake andmeanalüüsi jaoks](data-lake-analytics-diagnostic-logs.md)
- Keerukama päringu, leiate artiklist [analüüsi veebisaidi logid Azure'i Lake andmeanalüüsi abil](data-lake-analytics-analyze-weblogs.md).
- Töö üksikasjade vaatamiseks lugege teemat [kasutamine töö brauseri ja Azure andmeanalüüsi lake töö töö vaatamine](data-lake-analytics-data-lake-tools-view-jobs.md)