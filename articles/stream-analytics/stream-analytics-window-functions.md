<properties
    pageTitle="Sissejuhatus voo Analytics akna funktsioonid | Microsoft Azure'i"
    description="Siit saate teada, voo Analytics (langema, hopping, libistades) kolm akna funktsioonide kohta."
    keywords="langema aknas libistades aknas hopping aken"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="introduction-to-stream-analytics-window-functions"></a>Voo Analytics akna funktsioonide tutvustus

Reaalajas palju streaming stsenaariumid, tuleb toiminguid ainult ajaline windows leiduvatele andmetele. Kohalikke akendesüsteemide funktsioonide tugi on klahv Azure'i voo Analytics, mis liigub nõel arendaja tootlikkuse loome keerukate voo töötlemise tööd. Voo Analytics võimaldab arendajatel [**langema**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) ja [**libiseva**](https://msdn.microsoft.com/library/dn835051.aspx) Windowsi abil voogesituse andmete ajaline toiminguid. Tuleb märkida, et [Kõik toimingud akendega](https://msdn.microsoft.com/library/dn835019.aspx) väljasta tulemused akna **lõpuni** . Akna väljund on ühe sündmuse liitväärtuse funktsiooni kasutatakse põhjal. Sündmuse on ajatempel akna lõppu ja kõik akna funktsioonid on määratletud fikseeritud pikkusega. Lõpuks on oluline, et kõik akna funktsioonid [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) -klauslit kasutada.

![Voo Analytics akna funktsioonid mõisted](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Pyllähdys aken

Langema funktsioonide segmendi andmete voo erinevate aja lõikudeks ja teha funktsiooni nende vastu, nagu järgmises näites kasutatakse akent. Võtme differentiators langema akna on, et nad korrata, ei kattuks ja sündmuse ei saa kuuluvad rohkem kui üks langema aken.

![Sissejuhatav langema voo Analytics akna funktsioonid](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Sagedushüplemise aken

Kindla ajavahemiku järgi hopping ajas akna funktsioonid sõnumihüppe kohta, pärast edasi. See võib olla lihtne mõtlema neid langema aknad, milles saate kattuvad, nii, et sündmused saate kuuluvad rohkem kui üks Hopping akna tulemite hulk. Teha sama, mis on langema Hopping akna aknas ühte oleks lihtsalt määrata sõnumihüppe kohta, pärast sama akna suuruse. 

![Sissejuhatav hopping voo Analytics akna funktsioonid](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Libistades aken

Libistades akna funktsioonid erinevalt langema või Hopping windows, andes mõne väljundi **ainult** sündmuse ilmnemisel. Iga aknas on vähemalt üks sündmus ja akna pidevalt liigub edasi mõne € (epsilon). Nagu Hopping Windows, saate sündmuste kuuluvad libistades rohkem kui üks aken.

![Sissejuhatav libistades voo Analytics akna funktsioonid](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Akna funktsioonid abi saamine

Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
