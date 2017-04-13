<properties 
   pageTitle="StorSimple hetktõmmise haldur MMC menüü toimingud | Microsoft Azure'i"
   description="Kirjeldab, kuidas kasutada standard Microsoft Management Console (MMC) menüü toimingud StorSimple hetktõmmise halduris."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/25/2016"
   ms.author="v-sharos" />

# <a name="use-the-mmc-menu-actions-in-storsimple-snapshot-manager"></a>Kasutage menüü toimingud MMC StorSimple hetktõmmise halduris

## <a name="overview"></a>Ülevaade

StorSimple hetktõmmise Manager, näete järgmisi toiminguid, mis on loetletud kõik toiming menüüdes ja kõik muudatused paanil **toimingud** . 

- Vaade
- Uus aken siit 
- Värskendamine 
- Loendi eksportimine 
- Abi 

Neid toiminguid on osa Microsoft Management Console (MMC) ja ei ole teatud StorSimple hetktõmmise haldur. Selle õpetuse neid toiminguid kirjeldatakse ja selgitatakse, kuidas neid kasutada StorSimple hetktõmmise halduris.

## <a name="view"></a>Vaade

Saate **vaates** suvandi **tulemuste** paanil vaate muutmine ja konsooli akna vaate muutmine. 

#### <a name="to-change-the-results-pane-view"></a>Otsingutulemite paani vaate muutmine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. Klõpsake paanil **ulatus** Paremklõpsake mis tahes sõlme või laiendage ja paremklõpsake üksust **tulemuste** paanil ja seejärel suvandit **Vaade** . 

3. Lisada või eemaldada veerge, mis kuvatakse **tulemuste** paanil, klõpsake nuppu **Lisa/Eemalda veerud**. Kuvatakse dialoogiboks **Veergude lisamine või eemaldamine** .

    ![Veergude lisamine või eemaldamine kaudu otsingutulemite paan](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Add_remove_columns.png) 

4. Täitke vorm järgmiselt:

    - Märkige loendis **Saadaolevad** veerud üksused ja lisamiseks klõpsake nuppu **Lisa** need **kuvatakse veergude** loendile. 

    - Klõpsake loendis **kuvatakse veerud** ja klõpsake nuppu **Eemalda** loendist eemaldada. 

    - Valige üksuse loendis **kuvatakse** veerud ja klõpsake nuppu **Nihuta üles** või **Nihuta alla** üksuse loendis üles või alla nihutada. 

    - Klõpsake nuppu **Taasta vaikesätted** naasmiseks vaikimisi **tulemuste** paanil konfigureerimine. 

5. Kui olete soovitud valikud lõpetanud, klõpsake nuppu **OK**. 

#### <a name="to-change-the-console-window-view"></a>Konsooli akna vaate muutmine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. **Ulatus** paanil Paremklõpsake mis tahes sõlme, klõpsake menüüd **Vaade**ja seejärel klõpsake käsku **Kohanda**. Kuvatakse dialoogiboks **kohandamine** .

    ![Konsooli aknas kohandamine](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Customize.png) 

3. Märkige või tühjendage ruudud, et konsooli aknas üksuste kuvamiseks või peitmiseks. Kui olete soovitud valikud lõpetanud, klõpsake nuppu **OK**.

## <a name="new-window-from-here"></a>Uus aken siit

Saate konsooli uues aknas avamiseks suvand **Uus aken siit** .

#### <a name="to-open-a-new-console-window"></a>Uue konsooli akna avamiseks

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. **Ulatus** paanil, paremklõpsake mis tahes sõlm ja seejärel klõpsake nuppu **Uus aken siit**. 

    Kuvatakse uues aknas, mis näitab ainult ulatuse valitud. Näiteks kui paremklõpsate **Varukoopia poliitika** sõlme, uues aknas kuvatakse ainult **Varukoopia poliitikate** sõlm paani **ulatuse** ja loendi määratletud varukoopia poliitika **tulemuste** paanil. Vt järgmist näidet.

    ![Uus aken siit](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_NewWindow.png) 
 
## <a name="refresh"></a>Värskendamine

Saate värskendada konsooli akna **värskendamine** toiming.

#### <a name="to-update-the-console-window"></a>Konsooli akna värskendamine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. **Ulatus** paanil Paremklõpsake mis tahes sõlme või laiendage ja paremklõpsake üksust **tulemuste** paanil ja seejärel klõpsake nuppu **Värskenda**. 

## <a name="export-list"></a>Loendi eksportimine

**Loendi eksportimine** toimingu abil saate loendi komaeraldusega (CSV) faili salvestamine. Näiteks saate eksportida varukoopia poliitikate või varukoopia kataloogi loend. Seejärel saate CSV-faili importida tabelarvutusrakendust analüüsi jaoks.

#### <a name="to-save-a-list-in-a-comma-separated-value-csv-file"></a>Loendi komaeraldusega (CSV) faili salvestamine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur. 

2. Klõpsake paanil **ulatus** Paremklõpsake mis tahes sõlme või laiendage ja paremklõpsake üksust **tulemuste** paanil ja klõpsake **Loendi eksportimine**. 

3. Kuvatakse dialoogiboks **Loendi eksportimine** . Täitke vorm järgmiselt: 

    1. Väljale **faili nimi** tippige CSV-faili nimi või klõpsake noolt ja valige rippmenüüst loendist.

    2. Klõpsake väljal **Salvestustüüp** noolenuppu ja valige failitüüp rippmenüü loendist.

    3. Ainult valitud üksuste salvestamiseks valige read ja seejärel märkige ruut **Salvesta ainult valitud read** . Kõik eksporditud loendid salvestamiseks tühjendage ruut **Salvesta ainult valitud read** .

    4. Klõpsake nuppu **Salvesta**.

    ![Loendi eksportimine CSV-vormingus faili](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Export_List.png) 
 
## <a name="help"></a>Abi

Saate **menüü** saadaval elektroonilise spikri StorSimple hetktõmmise juhataja ja MMC.

#### <a name="to-view-available-online-help"></a>Saadaval elektroonilise spikri kuvamiseks

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. **Ulatus** paanil Paremklõpsake mis tahes sõlme või laiendage ja paremklõpsake üksust **tulemuste** paanil ja seejärel klõpsake nuppu **Spikker**. 

## <a name="next-steps"></a>Järgmised sammud

- Lugege lisateavet [StorSimple hetktõmmise halduri kasutajaliides](storsimple-use-snapshot-manager.md).
- Lisateave [halduris StorSimple hetktõmmise hallata oma StorSimple lahendus](storsimple-snapshot-manager-admin.md).
