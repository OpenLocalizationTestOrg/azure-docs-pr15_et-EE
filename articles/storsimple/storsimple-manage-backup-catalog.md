<properties 
   pageTitle="Hallata oma StorSimple varukoopia kataloogi | Microsoft Azure'i"
   description="Selgitab, kuidas kasutada lehe StorSimple halduri teenuse varukoopia kataloogi loend, valida ja kustutada varukoopia Komplektid maht."
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
   ms.date="04/28/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>StorSimple halduri teenuse abil saate hallata oma varukoopia kataloog

## <a name="overview"></a>Ülevaade

StorSimple halduri **Varukoopia kataloogi** lehel kuvatakse kõik luuakse käsitsi ja ajastatud varukoopiaid võtmisel varukoopia komplektid. Kasutage seda lehte varukoopiate varukoopia poliitika või maht, valige loendis või kustutada varukoopiaid või varukoopia abil saate taastada või klooni maht.

Selle õpetuse selgitatakse loend, valige ja kustutamine varukoopia. Saate teada, kuidas oma seadme taastamine varukoopia põhjal, avage [seadme hulgast varukoopia põhjal taastamine](storsimple-restore-from-backup-set.md). Saate teada, kuidas klooni maht, minge [klooni StorSimple helitugevust](storsimple-clone-volume.md).

![Varukoopia kataloogi](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

**Varukoopia kataloogi** lehelt leiate päringu varukoopia määramine valiku piiritlemine. Saate filtreerida varukoopia komplekti, mis laaditakse järgmiste parameetrite põhjal:

- **Seadme** -seade, mis loodi varukoopia määramine.

- **Varukoopia poliitika või maht** – varukoopia poliitika või helitugevuse seostatud see varukoopia komplekt.

- **Saatja ja adressaat** – kuupäev ja kellaaeg vahemiku varukoopia määramine loomisel.

Filtreeritud varukoopia komplektid on siis tabelis põhjal järgmisi atribuute:

- **Nimi** – nimi varukoopia poliitika või helitugevuse seostatud varukoopia määramine.

- **Suurus** – varukoopiad määrata tegeliku suurusega.

- **Loomisaeg** – kuupäev ja kellaaeg, millal on loodud varukoopiaid. 

- **Tippige** – varundamise komplekti saab kohaliku pilte või pilvepõhises hetktõmmiseid. Kohaliku hetktõmmise on kõik oma seadmes talletatud kohalik helitugevuse andmete varukoopia pilve hetktõmmise viitab elavate pilveteenuses andmete varundamine. Kohaliku hetktõmmiseid juurdepääsu kiirem cloud hetktõmmiseid on valida andmete paindlikkust.

- **Algataja** – varukoopiaid saab algatada automaatselt ajakava või käsitsi kasutaja. Varukoopia poliitika abil saate ajastada varukoopiad. Teise võimalusena saate **teha varukoopia** suvand käsitsi varukoopia tegemiseks.

## <a name="list-backup-sets-for-a-volume"></a>Loendi varundamise määrab maht
 
Tehke loendis varukoopiate maht, tehke järgmist.

#### <a name="to-list-backup-sets"></a>Kui soovite loendi varukoopia komplektid

1. Klõpsake lehel StorSimple halduri teenuse vahekaarti **varundamise kataloogi** .

2. Filtri Valikud järgmiselt:

    1. Valige soovitud seade.

    2. Valige ripploendist, mahu vaatamiseks vastava varukoopiaid.

    3. Määrake soovitud ajavahemiku.

    4. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) päringu.
 
    Varukoopiate, mis on seotud valitud maht peaks kuvatama varukoopia komplekti loendis.

## <a name="select-a-backup-set"></a>Valige varukoopia määramine

Järgmiste juhiste valimiseks varukoopia ruumala või varukoopia poliitika määramine.

#### <a name="to-select-a-backup-set"></a>Valige varukoopia määramine

1. Klõpsake lehel StorSimple halduri teenuse vahekaarti **varundamise kataloogi** .

2. Filtri Valikud järgmiselt:

    1. Valige soovitud seade.

    2. Valige ripploendist, helitugevuse või varundamise poliitika varundamiseks, mida soovite valida.

    3. Määrake soovitud ajavahemiku.

    4. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) päringu.

    Valitud helitugevuse seostatud varukoopiaid või varukoopia poliitika peaks kuvatama varukoopia komplekti loendis.

3. Valige ja laiendage varukoopiad määrata. Klõpsake lehe allservas kuvatakse **taastada** ja **kustutada** suvandid. Kas neid toiminguid saate sooritada valitud varukoopia Määrake.

## <a name="delete-a-backup-set"></a>Varukoopia kustutamine

Kui te ei soovi enam seotud andmete säilitamise, kustutada varukoopia. Järgmiste toimingute kustutamiseks varukoopiad määrata.

#### <a name="to-delete-a-backup-set"></a>Kustutamiseks varukoopia määramine

1. Klõpsake lehel StorSimple halduri teenuse **varundamise kataloogi menüü**.

2. Filtri Valikud järgmiselt:

    1. Valige soovitud seade.

    2. Valige ripploendist, helitugevuse või varundamise poliitika varundamiseks, mida soovite valida.

    3. Määrake soovitud ajavahemiku.

    4. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) päringu.

    Valitud helitugevuse seostatud varukoopiaid või varukoopia poliitika peaks kuvatama varukoopia komplekti loendis.

3. Valige ja laiendage varukoopiad määrata. Klõpsake lehe allservas kuvatakse **taastada** ja **kustutada** suvandid. Klõpsake nuppu **Kustuta**.

4. Kui kustutamine on pooleli ja kui see on edukalt lõpule jõudnud, teavitatakse teid sellest. Kui kustutamine on lõpule jõudnud, klõpsake selle lehe päringut värskendada. Kustutatud varukoopia määramine ei kuvata enam loendist varukoopia komplektid.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas [kasutada seadme hulgast varukoopia põhjal taastamine varukoopia põhjal kataloogi](storsimple-restore-from-backup-set.md).

- Siit saate teada, [hallata StorSimple seadme StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.
