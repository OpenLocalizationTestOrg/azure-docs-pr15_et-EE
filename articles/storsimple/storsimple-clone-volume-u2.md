<properties
   pageTitle="Teie StorSimple helitugevuse klooni | Microsoft Azure'i"
   description="Kirjeldatakse klooni eri tüüpi ja millal neid kasutada, ja selgitatakse, kuidas abil saate määrata ka üksikute helitugevuse klooni varukoopia."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/27/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a>Kasutage StorSimple halduri teenuse klooni maht (Värskenda 2)

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Ülevaade

StorSimple halduri **Varukoopia kataloogi** kuvatakse kõik luuakse käsitsi või automaatse varundamise funktsiooni võtmisel varukoopia komplektid. Kasutage seda lehte varukoopiate varukoopia poliitika või maht, valige loendis või kustutada varukoopiaid või varukoopia abil saate taastada või klooni maht.

![Varukoopia kataloogi leht](./media/storsimple-clone-volume-u2/backupCatalog.png)  

Selles õppetükis kirjeldatakse, kuidas abil saate määrata ka üksikute helitugevuse klooni varukoopia. Selgitatakse ka *siirdamiseks* ja *püsivate* kloonid vahe.

>[AZURE.NOTE] 
>
>Kohalik kinnitatud helitugevus on kloonitud nimega astmeline helitugevust. Kui peate olema kinnitatud kohalikult kloonitud kogus, saate teisendada klooni kohalikult kinnitatud helitugevuse pärast selle klooni on edukalt lõpule viidud. Lisateabe saamiseks astmeline helitugevuse teisendamine kohalikult kinnitatud helitugevust, minge [Helitugevuse tüübi muutmine](storsimple-manage-volumes-u2.md#change-the-volume-type).
>
>Kui proovite teisendamise kloonitud kaudu kohalikult kinnitatud kohe pärast kloonimine (kui see on endiselt siirdamiseks klooni), et mitmetasandilise tulemus ei õnnestu ja kuvatakse järgmine tõrketeade:
>
>`Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
>
>See tõrge on saanud ainult juhul, kui teil on mõne muu seadme kellelegi kloonimine. Saate teisendada edukalt helitugevuse soovite kohalikult kinnitatud, kui teisendate siirdamiseks klooni esmalt püsivate klooni. Püsivate klooni siirdamiseks klooni teisendamiseks cloud hetktõmmis seda teha.

## <a name="create-a-clone-of-a-volume"></a>Looge klooni, mille maht

Saate luua klooni samasse seadmesse, mõnda muud seadet või isegi virtuaalse masina abil kohalikule või pilveteenuse hetktõmmise.

#### <a name="to-clone-a-volume"></a>Klooni maht

1. Klõpsake lehel StorSimple halduri teenuse **varundamise kataloogi** vahekaarti ja valige varukoopia kogum.

2. Laiendage varunduse seotud mahud kuvamiseks. Klõpsake ja valige maht varukoopia määramine.

     ![Klooni maht](./media/storsimple-clone-volume-u2/CloneVol.png) 

3. Klõpsake nuppu **klooni** alustamiseks kloonimine valitud maht.

4. Klooni helitugevuse viisardis jaotises **Määra nimi ja asukoht**:

  1. Leidke soovitud seade. See on asukoht, kus luuakse klooni. Saate valida, kas samasse seadmesse või mõnda muud seadet määrata. Kui valite seotud Pilve väliselt maht (mitte Azure), target seadme ripploendis kuvatakse ainult füüsilise seadmed. Te ei saa klooni seostatud Pilve väliselt virtuaalse seadme maht.

        >[AZURE.NOTE] Veenduge, et klooni jaoks vajaliku on väiksem kui target seadme võimsus.

  2. Määrake oma klooni helitugevuse kordumatu nimi. Nimi peab sisaldama 3 kuni 127 märki. 
    
        >[AZURE.NOTE] Väli **Klooni helitugevuse nimega** saab **testimiste** isegi juhul, kui teil on kloonimine kohalikult kinnitatud helitugevust. Te ei saa muuta selle sätte; juhul, kui teil on vaja ka kohalik kinnitatud kloonitud kogus, saate teisendada klooni kohalikult kinnitatud helitugevuse pärast loomist edukalt klooni. Lisateabe saamiseks astmeline helitugevuse teisendamine kohalikult kinnitatud helitugevust, minge [Helitugevuse tüübi muutmine](storsimple-manage-volumes-u2.md#change-the-volume-type).

        ![Klooni viisardi 1](./media/storsimple-clone-volume-u2/clone1.png) 

  3. Klõpsake noolt ikooni ![ikooni kõrval olevat noolt](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) edasi järgmisele lehele.

5. Klõpsake jaotises **Määra hosts, mida saate kasutada seda helitugevuse**:

  1. Määrake juurdepääsu juhtelement kirje (ACR) klooni. Saate lisada uue ACR või valige olemasolev loendist.

        ![Klooni viisardi 2](./media/storsimple-clone-volume-u2/clone2.png) 

  2. Klõpsake ikooni Otsi ![sisse-ikoon](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)selle toimingu lõpuleviimiseks.

6. Klooni tööd alustada ja teid teavitatakse klooni edukalt loomisel. Klõpsake nuppu **Kuva töö** klooni töö lehel **töö** jälgimiseks. Kui klooni töö on valmis, kuvatakse järgmine teade:

    ![Klooni sõnum](./media/storsimple-clone-volume-u2/CloneMsg.png) 

7. Pärast klooni töö on lõpule viidud:

  1. Minge lehele **seadmed** ja valige vahekaart **Helitugevuse ümbriste** . 
  2. Valige helitugevuse ümbris, mis on seotud andmeallika maht, mida te kloonitud. Äsja loodud klooni peaksite nägema mahud loendis.

>[AZURE.NOTE] Vaike- ja varundamise automaatselt keelatud kloonitud maht.

Sellisel viisil loodud klooni on ajutine klooni. Klooni failitüüpide kohta leiate lisateavet teemast [siirdamiseks vs püsivate kloonid](#transient-vs.-permanent-clones).

Selle klooni on nüüd tavaline helitugevust ja kõik toimingud, mis on võimalik maht on võimalik klooni. Peate selle maht, mis tahes varufailide konfigureerimine.

## <a name="transient-vs-permanent-clones"></a>Mööduv vs püsivate kloonid

Siirdamiseks kloonid luuakse ainult siis, kui teil on muu seadme kloonimine. Saate klooni teatud helitugevuse määramine mõne muu seadme hallatavate StorSimple halduri varukoopia põhjal. Siirdamiseks klooni on viited andmetele algse köite ja kasutab andmete lugemine ja kirjutamine kohalikult target seadmes. 

Pärast pilvepõhise hetktõmmis siirdamiseks klooni teha, on tulemuseks klooni *püsivate* klooni. Selle käigus andmete koopia luuakse pilve ja kopeerige andmed aega määratakse mahu andmed ja Azure latentsused, (see on Azure'i Azure eksemplar). Selle protsessi võib kuluda kuni nädala päeva. Siirdamiseks klooni muutub püsivalt klooni sellisel viisil ja pole viiteid algse köite andmed, mida see oli kloonitud kaudu. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Ajutine ja püsiv kloonid stsenaariumid

Järgmistes jaotistes kirjeldatakse näide olukordades siirdamiseks ja püsivate kloonid kasutamist.

### <a name="item-level-recovery-with-a-transient-clone"></a>Üksusetaseme siirdamiseks klooni taastamine

Peate taastada faili ühe-aastane Microsoft PowerPointi esitlus. IT-administraator määratleb teatud varukoopia põhjal selle aja jooksul ja seejärel filtreerib maht. Administraator seejärel kloonid maht, otsib fail, mida otsite, ja pakub teile. Selle stsenaariumi korral kasutatakse siirdamiseks klooni. 
 
![Video saadaval](./media/storsimple-clone-volume-u2/Video_icon.png) **Video saadaval**

Vaadake videot, mis näitab, kuidas saate kasutada klooni ja funktsioonide StorSimple taastada kustutatud faile taastada, klõpsake [siin](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Tootmiskeskkonnas koos püsivate klooni testimine

Peate kontrollima tootmiskeskkonda testimise viga. Loote klooni mahu tootmiskeskkonda ja seejärel selle klooni loomiseks on sõltumatu kloonitud helitugevuse cloud hetktõmmise tegemiseks. Selle stsenaariumi korral kasutatakse püsivate klooni.  

## <a name="next-steps"></a>Järgmised sammud
- Siit saate teada, [StorSimple helitugevuse hulgast varukoopia põhjal](storsimple-restore-from-backup-set-u2.md)taastamine.

- Siit saate teada, [StorSimple halduri teenuse haldamiseks StorSimple seadme kasutamise](storsimple-manager-service-administration.md)kohta.

 
