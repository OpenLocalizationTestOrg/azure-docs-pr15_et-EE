<properties
   pageTitle="Teie StorSimple virtuaalse massiiv varukoopia põhjal taastamine"
   description="Lisateavet selle kohta, kuidas taastada oma StorSimple virtuaalse massiiv varukoopia."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="restore-from-a-backup-of-your-storsimple-virtual-array"></a>Teie StorSimple virtuaalse massiiv varukoopia põhjal taastamine

## <a name="overview"></a>Ülevaade 

See artikkel kehtib Microsoft Azure'i StorSimple virtuaalse massiiv (tuntud ka kui StorSimple kohapealse virtuaalse seadme või StorSimple virtuaalse seadme) töötava märts 2016 üldiselt kättesaadav (GA) versiooni või uuem versioon. Selles artiklis kirjeldatakse samm-sammult taastamine oma aktsiate või oma StorSimple virtuaalse massiiv draividel varukoopia kogumist. Artikli üksikasjade ka Üksusetaseme taastamine tööpõhimõte StorSimple virtuaalse massiiv, mis on konfigureeritud failiserverisse.


## <a name="restore-shares-from-a-backup-set"></a>Ühtlasi taastamine varukoopia põhjal määramine


**Enne, kui proovite taastada ühtlasi, veenduge, et teil on piisavalt vaba ruumi seadme selle toimingu lõpuleviimiseks.** Taastamine varukoopia [Azure klassikaline portaalis](https://manage.windowsazure.com/), tehke järgmist.

#### <a name="to-restore-a-share"></a>Ühiskasutusega taastamine

1.  Liikuge sirvides **varundamise kataloogi**. Filtreerimisaluseks soovitud seade ja ajavahemiku varukoopiate otsimiseks. Klõpsake ikooni Otsi ![](./media/storsimple-ova-restore/image1.png) päringu.


1.  Varukoopia komplekti, kuvatakse loendis, klõpsake ja valige teatud varukoopia. Laiendage varukoopia all erinevate osade kuvamiseks. Klõpsake ja valige ühiskasutus, mille soovite taastada.

2.  Klõpsake lehe allosas nuppu **Uus taastamine**.

3.  Alustamiseks käsku **Uus osana taastamine** viisardi. **Määrake nimi ja asukoht** lehel järgmist.


    1.  Kontrollige seadme allika nimi. See peaks olema seade, mis sisaldab ühiskasutus, mille soovite taastada. Seadme valik on tuhm. Valige seade erinevatest allikatest, peate viisardi sulgemiseks ja varunduse uuesti uuesti valida.

    2.  Sisestage nimi, ühiskasutus. Ühiskasutuse nimi peab olema 3 – 127 märki.

    3.  Vaadake üle suurus, -tüübi ja -õigused, mis on seotud ühiskasutus, mille soovite taastada. On võimalik muuta ühiskasutuse atribuudid Windows Exploreri kaudu, kui taastamine on lõpule jõudnud.

    4.  Klõpsake ikooni Otsi ![](./media/storsimple-ova-restore/image1.png).

        ![](./media/storsimple-ova-restore/image9.png)

1.  Pärast töö taastamine on lõpule jõudnud, hakkavad taastamine ja teile kuvatakse uue teatise. Taastamine edenemist jälgida, klõpsake nuppu **Kuva töö**. See viib teid lehele **tööde haldamine** .

2.  Saate taastamine töö edenemist jälgida. Kui taastamine on 100% valmis, liikuge **ühtlasi** lehele oma seadmes.

3.  Nüüd saate vaadata uue taastatud ühiskasutus loendis aktsiate seadme. Pange tähele, et selle taastamine on teha sama tüüpi ühiskasutus. Astmeline ühiskasutus taastatakse nagu mitmetasandilise ja kohalikult kinnitatud ühiskasutus kohalikult kinnitatud osa.

Nüüd on lõpetatud seadme konfiguratsiooni ja õppinud, kuidas varundada või taastada osa. 


## <a name="restore-volumes-from-a-backup-set"></a>Mahud taastamine varukoopia põhjal määramine


Taastamine varukoopia Azure klassikaline portaalis, tehke järgmist. Taastetoimingu taastab varukoopia uus draiv virtuaalse samasse seadmesse; te ei saa taastada mõne muu seadme.

#### <a name="to-restore-a-volume"></a>Taastada maht

1.  Liikuge sirvides **varundamise kataloogi**. Filtreerimisaluseks soovitud seade ja ajavahemiku varukoopiate otsimiseks. Klõpsake ikooni Otsi ![](./media/storsimple-ova-restore/image1.png) päringu.

2.  Varukoopia komplekti, kuvatakse loendis, klõpsake ja valige teatud varukoopia. Laiendage varukoopia, et näha selle all eri mahud. Valige maht, mille soovite taastada. 

5.  Klõpsake lehe allosas nuppu **Uus taastamine**. **Kui uus draiv taastamine** käivitub.

1.  **Määrake nimi ja asukoht** lehel järgmist.


    1.  Kontrollige seadme allika nimi. See peaks olema seade, mis sisaldab helitugevuse, mille soovite taastada. Seadme valik pole saadaval. Valige seade erinevatest allikatest, peate viisardi sulgemiseks ja varunduse uuesti uuesti valida.

    2.  Sisestage uue taastatakse helitugevuse helitugevuse nimi. Helitugevuse nimi peab olema 3 – 127 märki.

    3.  Klõpsake noolt ikooni.

        ![](./media/storsimple-ova-restore/image12.png)

1.  Valige lehel **Määrake hosts, mida saate kasutada seda helitugevuse** ripploendist vastav ACRs.

    ![](./media/storsimple-ova-restore/image13.png)

1.  Klõpsake ikooni Otsi ![](./media/storsimple-ova-restore/image1.png). See alustab taastamine töö ja kuvatakse järgmine teatis, et töö on pooleli.

2.  Pärast töö taastamine on lõpule jõudnud, hakkavad taastamine ja teile kuvatakse uue teatise. Taastamine edenemist jälgida, klõpsake nuppu **Kuva töö**. See viib teid lehele **tööde haldamine** .

3.  Saate taastamine töö edenemist jälgida. Teie seadmes **mahud** lehele liikumine

4.  Nüüd saate vaadata uus taastatud maht mahud loendis seadme. Pange tähele, et selle taastamine on teha sama tüüpi helitugevust. Astmeline helitugevus on taastatud nagu mitmetasandilise ja kohalikult kinnitatud helitugevuse taastatakse nimega kohalikult kinnitatud helitugevust.

5.  Kui helitugevuse kuvatakse loendis mahud online, maht on kasutamiseks saadaval.  ISCSI algataja Host, värskendage loendit eesmärgid iSCSI algataja aken Atribuudid.  Uus sihtkoht, mis sisaldab taastatud maht nimi peaks kuvatama nagu "passiivsed" veerus olek.

6.  Valige siht ja klõpsake nuppu **Ühenda**.   Pärast algataja on ühendatud sihtkohas, olek peaks vahetada **ühendatud**. 

7.  **Ketas halduse** aknas kuvatakse ühendatud mahud nagu on näidatud järgmisel joonisel. Paremklõpsake avastatud helitugevuse (klõpsake ketta nimi) ja seejärel klõpsake nuppu **Veebipildid**.

> [AZURE.IMPORTANT] Kui proovite draivi või osa varukoopia põhjal taastamine seadmine taastamine töökohtade nurjumisel target maht või ühiskasutus endiselt luuakse portaalis. On oluline, et kustutada see target maht või ühiskasutusse anda portaalis minimeerimiseks tulevaste – see element tekkivaid probleeme.

## <a name="item-level-recovery-ilr"></a>Üksusetaseme taastamine (ILR)

See väljaanne tutvustab StorSimple virtuaalse seadme faili server on konfigureeritud Üksusetaseme taastamine (ILR). See funktsioon võimaldab teil teha Varundustöö taastamine failide ja kaustade kõigi aktsiate StorSimple seadme pilve varukoopia põhjal. Kasutajate võib kustutatud failide allalaadimiseks näidise iseteenindusliku Viimatised varukoopiad.

Iga ühiskasutus on *.backups* kausta, mis sisaldab kõige Viimatised varukoopiad. Kasutaja liikuge soovitud varukoopia, kopeerige asjakohased failid ja kaustad Varundamine ja taastada. See kaob kõned administraatorite jaoks failide varukoopiate põhjal taastamine.

1.  Kui läbimiseks on ILR, saate vaadata varukoopiate Windows Exploreri kaudu. Klõpsake vaadata varukoopia jaoks soovitud teatud ühiskasutus. Kuvatakse jaotises ühiskasutus, mis talletab varukoopiate loodud *.backups* kausta. Laiendage *.backups* kausta kuvamiseks varukoopiaid. Seejärel kuvatakse kaust laotusjoonised kogu varukoopia hierarhia. See vaade on loodud nõudmisel ja kulub tavaliselt ainult mõne sekundi loomiseks.

    Viimase 5 varukoopiaid kuvatakse sel viisil ja saab teha mõne üksuse tasandi taastamine. 5 Viimatised varukoopiad sisaldavad ajastatud vaikimisi nii käsitsi varukoopiaid.

    
    -   **Ajastatud varukoopiate** nimeks &lt;seadme nime&gt;DailySchedule AAAAKKPP-HHMMSS UTC.

    -   **Käsitsi varukoopiate** Ad-ajutine-AAAAKKPP-HHMMSS-UTC nimega.
    
        ![](./media/storsimple-ova-restore/image14.png)

1.  Leidke varundamise, mis sisaldab kustutatud faili uusima versiooni. Kuigi kausta nimi sisaldab iga eeltoodud juhtudel UTC ajatempel, kus kausta loomise aeg on varukoopia käivitati tegelik seadme aega. Otsige üles ja tuvastamine varukoopiaid kausta ajatempel abil.

2.  Otsige üles kaust või fail, mille soovite taastada varukoopia, mille te eelmises etapis. Märkme saate vaadata ainult failid või kaustad, millele teil on õigused. Kui teil pole juurdepääsu teatud faile või kaustu, peate pöörduge ühiskasutus administraatori poole, kes saavad kasutada Windows Exploreris ühiskasutus õiguste redigeerimine ja annab teile juurdepääsu teatud faili või kausta. See on soovitatav parimaid tavasid, et ühiskasutus administraator saab kasutaja rühma asemel ühe kasutaja.

3.  Faili või kausta kopeerimiseks kohase osa StorSimple faili serverisse.

![video_icon](./media/storsimple-ova-restore/video_icon.png) **Video saadaval**

Vaadake videot, et näha, kuidas saate luua ühtlasi, ühtlasi varundamine ja taastamine andmete StorSimple virtuaalse massiivi.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet selle kohta, kuidas hallata [Oma StorSimple virtuaalse massiiv kohalik web Kasutajaliidese kaudu](storsimple-ova-web-ui-admin.md).
