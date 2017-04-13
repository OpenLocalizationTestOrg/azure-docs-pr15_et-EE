<properties
    pageTitle="JSON väljundi voo Analyticsi | Microsoft Azure'i"
    description="Siit saate teada, kuidas voo Analytics saate suunata Azure'i DocumentDB JSON väljund, andmete arhiveerimise ja latentsusajaga päringud JSON struktureerimata andmed."
    keywords="JSON väljund"
    documentationCenter=""
    services="stream-analytics,documentdb"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="target-azure-documentdb-for-json-output-from-stream-analytics"></a>Sihtrakenduse Azure'i DocumentDB voo Analytics JSON väljundi jaoks

Voo Analytics saate suunata [Azure'i DocumentDB](https://azure.microsoft.com/services/documentdb/) JSON väljund, lubada andmepäringuid arhiveerimise ja latentsusajaga struktureerimata JSON andmete jaoks. Siit saate teada, kuidas kõige paremini rakendada selle integreerimine.

Neile, kes ei tunne DocumentDB, Heitke pilk [DocumentDB's õppeteema](https://azure.microsoft.com/documentation/learning-paths/documentdb/) alustada.

## <a name="basics-of-documentdb-as-an-output-target"></a>Kui ka väljundi target DocumentDB põhialused
Azure'i DocumentDB väljundi voo Analytics võimaldab kirjutamist oma voo töötlemiseks tulemuste JSON väljundina oma DocumentDB collection(s). Voo Analytics ei loo saidikogumid andmebaasi, selle asemel et peaksite need loomine algusest peale. See on nii, et arvelduse DocumentDB saidikogumid on läbipaistev ja nii, et saate häälestada jõudluse, järjepidevuse ja võimsus teie saidikogumite otse kasutades [DocumentDB API -d](https://msdn.microsoft.com/library/azure/dn781481.aspx). Soovitame kasutada ühte DocumentDB andmebaasi iga streaming töö loogiliselt eraldi oma saidikogumid streaming tööd.

DocumentDB saidikogumi funktsioonid on järgmised.

## <a name="tune-consistency-availability-and-latency"></a>Vormindusühtsuse kättesaadavus ning latentsuse häälestamine

Rakenduse vastavad teie DocumentDB võimaldab teil viimistlemiseks häälestada andmebaas ja saidikogumite ja kompromisse vormindusühtsuse kättesaadavus ning latentsuse vahel. Sõltuvalt sellest, millised tasemed loetuks järjepidevuse teie stsenaariumi vajadustele vastu lugemine ja kirjutamine latentsus, saate valida järjepidevuse taseme andmebaasi kontol. Vaikimisi lubab DocumentDB ka sünkroonse indekseerimise iga toimingu CRUD oma saidikogumi kohta. See on kasulik teine võimalus määrata DocumentDB tulemuslikkuse lugemine ja kirjutamine. Lisateavet selle teema kohta vaadake üle [andmebaasi ja päringu järjepidevuse tasemete muutmine](../documentdb/documentdb-consistency-levels.md) artikkel.

## <a name="choose-a-performance-level"></a>Valige jõudluse tase

Tasemel 3 erinevatele (S1, S2 või S3), mis määratlevad, CRUDs jaoks saadaval läbilaskevõime selle saidikogumi loomist DocumentDB saidikogumid. Lisaks mõjutab jõudluse indekseerimise järjepidevuse tasemed teie saidikogumi kohta. Lugege [artiklit](../documentdb/documentdb-performance-levels.md) mõista nende jõudluse tasemete üksikasjad.

## <a name="upserts-from-stream-analytics"></a>Upserts voo Analytics

Voo Analytics integreerimine DocumentDB võimaldab lisada või värskendada kirjeid oma DocumentDB kollektsiooni antud dokumendi ID veeru põhjal. See on ka edaspidi on *Upsert*.

Voo Analytics kasutab loodetav Upsert lähenemisviisi, kus värskendused on ainult valmis lisa nurjub dokumendi ID konflikti tõttu. Selle värskenduse sooritatakse voo Analytics paik, nii, et see võimaldab osaline värskenduste ehk uue atribuudid või asendades olemasoleva atribuudi sooritatakse artistide lisamine dokumenti. Pange tähele, et muutub JSON dokumendi tulemi kogu massiivis saada kirjutatakse üle, st Massiiv massiiv atribuutide väärtused on pole ühendatud.

## <a name="data-partitioning-in-documentdb"></a>Klõpsake DocumentDB eraldamine andmed

DocumentDB saidikogumid võimaldavad partition andmete päringu mustrite ja rakenduse jõudluse vajadustele. Iga saidikogumi võib sisaldada kuni 10GB andmete (maksimum) ja praegu ei ole võimalik kogumi skaalal (või ületäitumise). Jaoks skaala välja voo Analytics võimaldab teil kirjutamise mitme saidikogumid antud eesliitega (vt allpool kasutusteavet). Voo Analytics kasutab partition selle väljundi kirjete ühtsete [Räsi Partition Resolveri](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) strateegia kasutaja esitatud PartitionKey veeru alusel. Alusta voogesituse töö ajal antud eesliitega saidikogumite arvu kasutatakse väljundi partition arv, mille töö kirjutab paralleelselt (DocumentDB saidikogumite = väljundi sektsioonid). Ühe S3 jaoks saidikogumi rongile indekseerimise teinud ainult lisab, 0,4 MB/s kirjutamine läbilaskevõime oodata kohta. Mitme saidikogumid abil võimaldab teil suurema läbilaskevõimega ja suurendatud võimsusega saavutamiseks.

Kui kavatsete tõsta sektsioon count tulevikus, peate oma töö jaotamine andmeid oma olemasoleva saidikogumid uued saidikogumid sisse ja taaskäivitage voo Analytics töö. Lisateavet selle kohta, kasutades PartitionResolver ja uuesti eraldamine koos proovi kood, kaasatakse järeltegevuse postitusele. Artikli [sisse DocumentDB eraldamine](../articles/documentdb-partition-data.md#developing-a-partitioned-application) pakub selle üksikasjad.

## <a name="documentdb-settings-for-json-output"></a>JSON väljundi DocumentDB sätted

Loomise DocumentDB väljundina, mis on voo Analytics loob viip teavet, nagu allpool näha. Selles jaotises antakse selgitus atribuudid määratlus.

![documentdb voo analytics väljund ekraani](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output.png)  

-   **Väljundi pseudonüüm** – pseudonüümi viidata selle väljundi ASA päringus  
-   **Konto nimi** – nimi või lõpp-punkti URI DocumentDB konto.  
-   **Konto võti** – ühiskasutusega kiirklahv DocumentDB konto.  
-   **Andmebaasi** – The DocumentDB andmebaasi nimi.  
-   **Saidikogumi nimi muster** – saidikogumi nimi mustri kasutamiseks saidikogumid. Saidikogumi nimevormingut saate ehitatud abil valikuline {partition} luba, kus sektsioonid alustama 0. Järgnevalt on valimi kehtiv sisendina:  
   1\) MyCollection – ühe saidikogumi nimega "MyCollection" peab olemas.  
   2\) MyCollection {partition} – nt saidikogumid peab olemas – "MyCollection0", "MyCollection1", "MyCollection2" ja jne.  
-   **Sektsiooni võti** – väljundi sündmuste abil saate määrata klahvi jagamine väljundi üle saidikogumid välja nimi. Ühe saidikogumi väljundi, mis tahes suvalise väljund veeru saab kasutada näiteks PartitionId.  
-   **Dokumendi ID** – valikuline. Väljundi sündmuste abil saate määrata, millised lisa või Värskenda põhinevad toimingute primaarvõtme välja nimi.  
