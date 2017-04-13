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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume"></a>Kasutage StorSimple halduri teenuse klooni maht

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Ülevaade

StorSimple halduri **Varukoopia kataloogi** kuvatakse kõik luuakse käsitsi või automaatse varundamise funktsiooni võtmisel varukoopia komplektid. Kasutage seda lehte varukoopiate varukoopia poliitika või maht, valige loendis või kustutada varukoopiaid või varukoopia abil saate taastada või klooni maht.

![Varukoopia kataloogi leht](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

Selles õppetükis kirjeldatakse, kuidas abil saate määrata ka üksikute helitugevuse klooni varukoopia. Selgitatakse ka *siirdamiseks* ja *püsivate* kloonid vahe. 

## <a name="create-a-clone-of-a-volume"></a>Looge klooni, mille maht

Kohalikule või hetktõmmise pilveteenuse abil saate luua samasse seadmesse, mõnda muud seadet või isegi virtuaalse masina klooni.

#### <a name="to-clone-a-volume"></a>Klooni maht

1. Lehel StorSimple halduri teenuse **varundamise kataloogi** vahekaarti ja valige varukoopia kogum.

2. Laiendage varunduse seotud mahud kuvamiseks. Klõpsake ja valige maht varukoopia määramine.

     ![Klooni maht](./media/storsimple-clone-volume/HCS_Clone.png) 

3. Klõpsake nuppu **klooni** alustamiseks kloonimine valitud maht.

4. Klooni helitugevuse viisardis jaotises **Määra nimi ja asukoht**:

  1. Leidke soovitud seade. See on asukoht, kus luuakse klooni. Saate valida, kas samasse seadmesse või mõnda muud seadet määrata. Kui valite seotud Pilve väliselt maht (mitte Azure), target seadme ripploendis kuvatakse ainult füüsilise seadmed. Te ei saa klooni helitugevuse, mis on seotud Pilve väliselt virtuaalse seadmes.

        >  [AZURE.NOTE] Veenduge, et klooni jaoks vajaliku on väiksem kui target seadme võimsus.
  2. Määrake oma klooni helitugevuse kordumatu nimi. Nimi peab sisaldama 3 kuni 127 märki.
  3. Klõpsake noolt ikooni ![ikooni kõrval olevat noolt](./media/storsimple-clone-volume/HCS_ArrowIcon.png) edasi järgmisele lehele.

5. Klõpsake jaotises **Määrake hosts, mida saate kasutada seda helitugevuse**:

  1. Määrake juurdepääsu juhtelement kirje (ACR) klooni. Saate lisada uue ACR või valige olemasolev loendist.
  2. Klõpsake ikooni Otsi ![sisse-ikoon](./media/storsimple-clone-volume/HCS_CheckIcon.png)selle toimingu lõpuleviimiseks.

6. Klooni tööd alustada ja teid teavitatakse klooni edukalt loomisel. Klõpsake nuppu **Kuva töö** klooni töö lehel **töö** jälgimiseks.

7. Pärast klooni töö on lõpule viidud:

  1. Minge lehele **seadmed** ja valige vahekaart **Helitugevuse ümbriste** . 
  2. Valige helitugevuse ümbris, mis on seotud andmeallika maht, mida te kloonitud. Äsja loodud klooni peaksite nägema mahud loendit.

>[AZURE.NOTE] Vaike- ja varundamise automaatselt keelatud kloonitud maht.

Sellisel viisil loodud klooni on ajutine klooni. Klooni failitüüpide kohta leiate lisateavet teemast [siirdamiseks vs püsivate kloonid](#transient-vs.-permanent-clones).

Selle klooni on nüüd tavaline helitugevust ja kõik toimingud, mis on võimalik maht on võimalik klooni. Peate selle maht, mis tahes varufailide konfigureerimine.

## <a name="transient-vs-permanent-clones"></a>Mööduv vs püsivate kloonid

Ajutine ja püsiv kloonid luuakse ainult siis, kui teil on mõne muu seadme kellelegi kloonimine. Saate klooni teatud helitugevuse määramine mõne muu seadme varukoopia põhjal. Sellisel viisil loodud klooni on *ajutine* klooni. Siirdamiseks klooni on algse köite viiteid ja kasutab seda helitugevuse kohalikult kirjutamise ajal lugeda. 

Pärast pilvepõhise hetktõmmis siirdamiseks klooni teha, on tulemuseks klooni *püsivate* klooni. Püsivate klooni ja ei olla mis tahes viited algse maht, mis see on kloonitud kaudu.  

## <a name="scenarios-for-transient-and-permanent-clones"></a>Ajutine ja püsiv kloonid stsenaariumid

Järgmistes jaotistes kirjeldatakse näide olukordades siirdamiseks ja püsivate kloonid kasutamist.

### <a name="item-level-recovery-with-a-transient-clone"></a>Üksusetaseme siirdamiseks klooni taastamine

Peate taastada faili ühe-aastane Microsoft PowerPointi esitlus. IT-administraator määratleb teatud varukoopia põhjal selle aja jooksul ja seejärel filtreerib maht. Administraator seejärel kloonid helitugevuse, leiab fail, mida otsite, ja pakub seda teile. Selle stsenaariumi korral kasutatakse siirdamiseks klooni. 
 
![Video saadaval](./media/storsimple-clone-volume/Video_icon.png) **Video saadaval**

Vaadake videot, mis näitab, kuidas saate kasutada klooni ja funktsioonide StorSimple taastada kustutatud faile taastada, klõpsake [siin](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Tootmiskeskkonnas koos püsivate klooni testimine

Peate kontrollima tootmiskeskkonda testimise viga. Saate luua klooni mahu tootmiskeskkonda võttes selle klooni hetktõmmise pilve. Kloonitud maht on nüüd sõltumatu. Selle stsenaariumi korral kasutatakse püsivate klooni.

## <a name="next-steps"></a>Järgmised sammud
- Siit saate teada, [StorSimple helitugevuse hulgast varukoopia põhjal](storsimple-restore-from-backup-set.md)taastamine.

- Siit saate teada, [hallata StorSimple seadme StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.

 
