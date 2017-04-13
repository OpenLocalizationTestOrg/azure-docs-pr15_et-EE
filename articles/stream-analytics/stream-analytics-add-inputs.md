<properties
    pageTitle="Andmete sisestamine oma voo Analytics projektidele lisamine | Microsoft Azure'i"
    description="Saate teada, kuidas ühendada oma voo Analytics töö nagu andmesisestuse sündmuse jaoturi või viite andmete põhjal ajaveebi salvestusruumi streaming andmeallikas."
    keywords="streaming andmed sisestada, andmeid"
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


# <a name="add-a-streaming-data-input-or-reference-data-to-a-stream-analytics-job"></a>Voo Analytics töö voogesituse andmete sisestamine või viite andmete lisamine

Saate teada, kuidas ühendada oma voo Analytics töö nagu andmesisestuse sündmuse jaoturi või viite andmete põhjal bloobimälu streaming andmeallikas.

Azure'i voo Analytics töö saab ühendada ühe andmete sisestamine või rohkem, millest igaüks määratleda ühenduse olemasolevas andmeallikas. Andmeallika saadetakse andmed, on see tarbitud voo Analytics töö ja töödeldud nimega streaming andmete reaalajas. Voo Analytics on esimese klassi integreerimine [Azure sündmuse jaoturi](https://azure.microsoft.com/services/event-hubs/) ja [Azure'i bloobimälu](../storage/storage-dotnet-how-to-use-blobs.md) nii ja väljaspool projekti tellimus.

Selles artiklis on samm [voo Analytics õppeteema](/documentation/learning-paths/stream-analytics/).

## <a name="data-input-streaming-data-and-reference-data"></a>Andmesisestuse: Streaming andmed ja lähteandmed

On kaks erinevat tüüpi sisendina voo Analytics: andmevoogu ja andmed.

- **Andmevoogu**: voo Analytics töö peab sisaldama vähemalt ühte andmesisestuse voo consumed ja kujundab töö. Azure'i bloobimälu ja Azure sündmuse jaoturi on toetatud andmeallikate voo Sisestuskeel nimega. Azure'i sündmuse jaoturi kasutatakse sündmuse voogu kogumine ühendatud seadmete, teenused ja rakendused. Azure'i bloobimälu saab kasutada Sisestuskeel allikas söömisega voo hulgi andmed.  
- **Lähteandmed**: voo Analytics toetab teist tüüpi abistava Sisestuskeel nimega andmed.  Liikumistee andmete, mitte need andmed on staatiline või aeglustumas muutmine.  Seda kasutatakse tavaliselt look-ups ja andmevoogu koos korrelatsiooni loomiseks rikkalikumat andmehulgas.  Azure'i bloobimälu pole praegu toetatud Sisestuskeel ainuke viide andmete jaoks.  

Oma töö voo Analytics sisend lisamiseks tehke järgmist.

1. Azure'i portaalis valige **sisendina** ja seejärel käsku **Lisa sisend** voo Analytics oma töö.

    ![Azure'i klassikaline portaali - sisend lisada.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  

    Klõpsake Azure portaali **sisendina** paan voo Analytics oma töö.  

    ![Azure'i portaal - lisada sisestatud andmed.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  

2. Määrake sisend: kas **andmevoos** või **viite andmed**.

    ![Õigete andmete sisend voona või viidata lisamine](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  

    ![Lisage õigete andmete sisestusmeetodi voona või viitamine.](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  

3. Kui andmevoos sisendit loomist määrata sisestamise andmeallika tüüp.  Seda toimingut saavad vahele ainult bloobimälu salvestusruumi on toetatud seekord nagu viide andmete loomise ajal.

    ![Andmesisestuse voo andmete lisamine](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  

    ![Andmesisestuse voo andmete lisamine](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  

4. Sisestage sõbralik nimi selle sisendi välja Sisestuskeel alias (pseudonüüm).  Selle nime kasutatakse teie töö päringu hiljem sisend viidata.

    Täitke ülejäänud nõutavad ühenduse atribuudid ühenduse andmeallikaga. Need väljad erineda sisestus- ja allika tüüp ja on määratletud üksikasjalikult [siin](stream-analytics-create-a-job.md).  

    ![Lisage sündmuse jaoturi andmete sisestamine](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  

5. Sisendandmete sariväljaanne sätete määramiseks tehke järgmist.
    - Veendumaks, et teie päringud töötavad eeldatult, määrake **Sündmuse serialiseerimisvormingus** sissetulevad andmed.  Toetatud sariväljaanne vormingud lähevad JSON, CSV ja Avro.
    - Kontrollige **kodeering** andmete jaoks.  UTF-8 on ainus toetatud kodeerimise vorming sel ajal.

    ![Andmete sariväljaanne sätete sisestada andmeid](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  

    ![Andmete sariväljaanne sätete sisestada andmeid](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  

6. Pärast Sisestuskeel loomine, kuvatakse voo Analytics veenduge, et see saab ühenduda sisendandmete allikas.  Testi ühendust toimingu olekut saate vaadata teate jaoturi.

    ![Testi ühendust voogesituse andmete sisend](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  

    ![Testi ühendust voogesituse andmete sisend](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Siit saate teavet andmete sisendina streaming
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
