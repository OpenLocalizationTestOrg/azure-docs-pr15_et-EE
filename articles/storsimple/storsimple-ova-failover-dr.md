<properties
   pageTitle="Katastroofi taastamine ja seadme Tõrkesiirde oma StorSimple virtuaalse massiiv"
   description="Lugege lisateavet Tõrkesiirde kohta oma StorSimple virtuaalse massiiv."
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

# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array"></a>Katastroofi taastamine ja seadme Tõrkesiirde oma StorSimple virtuaalse massiiv


## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse avariitaastet oma Microsoft Azure'i StorSimple virtuaalse massiiv (tuntud ka kui StorSimple kohapealse virtuaalse seadme), sh nurjumise üle virtuaalse mõnda muud seadet õnnetuste üksikasjalikud etapid. Tõrkesiirde võimaldab teil oma andmete *Allikas* seadme andmekeskuses siirdamiseks mõnda muud seadet *target* , asuvad samas või geograafiline asukoht on muutunud. Seadme Tõrkesiirde on kogu seade. Ajal Tõrkesiirde cloud andmete allikas seadme muutub omandiõiguse target seadme.

Seadme Tõrkesiirde on orkestreerinud funktsiooni katastroofi taastamine (DR) kaudu ja alustatakse **seadmete** lehe kaudu. Selle lehe Tulemusarvestaja StorSimple seadmete StorSimple halduri teenust ühendatud. Iga seadme jaoks kuvatakse sõbralik nimi, olek, ettevalmistatud ja maksimaalse ametinimetus, tüüp ja mudel.

![](./media/storsimple-ova-failover-dr/image15.png)

See artikkel on mõeldud StorSimple virtuaalse massiivi ainult. Ei õnnestu üle 8000 sarja seadmes, minge [Tõrkesiirde ja avariitaastet StorSimple seadme](storsimple-device-failover-disaster-recovery.md).


## <a name="what-is-disaster-recovery"></a>Mis on avariitaastet?

Katastroof taastamine (DR) stsenaarium, esmane seade lakkab töötamast. Sellisel juhul saate teisaldada pilve andmed seotud nurjunud seadme mõnele muule seadmele esmase seadme abil *Allikas* ja täpsustades mõnda muud seadet, mis *target*. Selle protsessi nimetatakse *Tõrkesiirde*. Ajal Tõrkesiirde kõik mahud või ühtlasi allikas seadme omaniku muutmine ja kantakse target seade. Andmete filtreerimine pole lubatud.

Kui täielik seadme taastamine abil intensiivsuskaardi – kaardipõhiste tiering ja jälitamine on kujundatud DR. Intensiivsuskaardi on määratletud intensiivsuskaardi väärtuse määramisel põhiste andmete lugemine ja kirjutamine mustrite. See intensiivsuskaardi kaart siis astme alumise intensiivsuskaardi andmete tükkideks pilveteenusesse esmalt säilitades kõrge intensiivsuskaardi (viimati kasutatud) andmete tükkideks kohaliku astme. Intensiivsuskaardist kasutatakse ajal DR, taastada ja rehydrate pilvest andmed. Seadme toob kõik mahud/aktsiate Viimati tehtud varukoopia (nagu määratud ettevõttesiseselt) ja teeb selle varukoopia põhjal taastada. Kogu DR protsess on juhitud seade.


## <a name="prerequisites-for-device-failover"></a>Seadme Tõrkesiirde eeltingimused


### <a name="prerequisites"></a>Eeltingimused

Mis tahes seadme Tõrkesiirde, tuleb täita järgmised eeltingimused:

- Andmeallika seade peab olema **deaktiveeritud** olekus.

- Sihtrakenduse seade peab kuvataks **aktiivseks** Azure klassikaline portaalis. Peate ettevalmistamise target virtuaalne seadme sama või suurema mahuga. Saab kasutada kohaliku web UI edukalt virtuaalse seadme registreerimine ja konfigureerimiseks.

    > [AZURE.IMPORTANT] Proovige konfigureerimine registreeritud virtuaalse seadme kaudu, klõpsates nuppu **valmis seadme häälestus**. Teenuse kaudu tehakse pole seadme konfigureerimine.

- Lähte- ja seadme peavad olema sama tüüpi. Teil võib nurjuda ainult üle virtuaalse seadme konfigureeritud faili server, teise faili serverisse. Sama kehtib iSCSI serveri.

- Failiserverisse DR, soovitame, et liitute target seadme allika sama domeen, et ühiskasutusõigused lahendada. Selles väljaandes on toetatud Tõrkesiirde target seadme sama domeeni.

### <a name="other-considerations"></a>Muud kaalutlused

- Soovitame võtta mahud või aktsiate allikas seadme ühenduseta.

- Kui see on kavandatud Tõrkesiirde, soovitame, et võtta seadme varukoopia ja jätkake selle Tõrkesiirde andmekao minimeerimiseks. Kui see on planeerimata Tõrkesiirde, kasutatakse taastada seadme Viimane varukoopia.

- Saadaval target seadmete jaoks DR on seadmeid, millel on sama või suurem võimsus võrreldes allika seade. Ei vasta kriteeriumidele piisavalt ruumi, kuid teenust ühendatud seadmete ei ole saadaval, kui target seadmed.

### <a name="dr-prechecks"></a>DR prechecks

Enne DR algust tehakse prechecks seadmes. Kontrolli aitab tagada, et ilmneb tõrkeid kui DR algab. Funktsiooni prechecks on järgmised.

- Salvestusruumi konto kontrollimine

- Pilveteenuse Ühenduvus Azure kontrollimine

- Vaba ruumi target seadme kontrollimine

- Kontrollimine, kui iSCSI serveri andmeallika seade on sobivad ACR nimed, IQN (kuni 220 märki) ja parool CHAP (12 – 16 märki) seotud mahud

Kui mõne eespool prechecks nurjub, ei saa jätkata DR. Peate nende probleemide lahendamiseks ja proovige siis uuesti DR.

Pärast DR on edukalt lõpule viidud, kantakse cloud andmete allikas seadme omandiõiguse target seade. Andmeallika seade pole enam siis portaalis. Juurdepääs kõik mahud/aktsiad allika seadmes on blokeeritud ja target muutub aktiivne.

> [AZURE.IMPORTANT]
> 
> Kuigi seade pole enam saadaval, virtuaalse masina, mida te ette valmistatud host süsteemi endiselt tarbib ressursse. Kui DR on edukalt lõpule viidud, saate kustutada selle virtuaalse masina host süsteemist.

## <a name="fail-over-to-a-virtual-array"></a>Nurjuda üle virtuaalse massiiv

Soovitame, et teil on mõne muu StorSimple virtuaalse massiiv ette valmistatud ja konfigureeritud UI kohaliku veebi kaudu StorSimple halduri teenuse enne selle protseduuri registreeritud.


> [AZURE.IMPORTANT]
> 
> - Teil pole õigust 1200 virtuaalse seadmesse nurjumise üle StorSimple 8000 sarja seadme kaudu.
> - Üle võib nurjuda Federal teabe töötlemise Standard (FIPS) lubatud virtuaalse seadme juurutatud Government portaalis Azure klassikaline portaali virtuaalse seadme kaudu. Kehtib ka vastupidi.

Järgmiste toimingute target StorSimple virtuaalse seade seadme taastamiseks.

1. Võtta hosti mahud/aktsiate ühenduseta. Vaadake mahud/aktsiate võrguühenduseta hosti operatsioonisüsteemi – konkreetseid juhiseid. Kui juba ühenduseta, peate tegema kõik mahud/aktsiad ühenduseta seadme minnes **seadmed > ühtlasi** (või **seadme > mahud**). Valige ühiskasutus ja mahu ning **vallasrežiimi** lehe allserva. Kinnituse küsimisel nuppu **Jah**. Korrake seda toimingut kõik osakud ja mahud seadmes.

2. **Seadmete** lehel Valige Tõrkesiirde allika seade ja klõpsake nuppu **Desaktiveeri**. 
    ![](./media/storsimple-ova-failover-dr/image16.png)

3. Teil palutakse kinnitust. Seadme desaktiveerimine on püsiv protsess, mida ei saa tagasi võtta. Saate ka peatselt tegemiseks oma aktsiate/mahud ühenduseta Host.

    ![](./media/storsimple-ova-failover-dr/image18.png)

3. Pärast kinnitamist, hakkavad selle desaktiveerimine. Pärast soovitud desaktiveerimine on edukalt lõpule viidud, teavitatakse teid.

    ![](./media/storsimple-ova-failover-dr/image19.png)

4. Klõpsake lehel **seadmed** seadme olek muutub nüüd **deaktiveeritud**.

    ![](./media/storsimple-ova-failover-dr/image20.png)

5. Valige desaktiveeritakse seade ja klõpsake lehe allosas nuppu **Tõrkesiirde**.

6. Kinnita Tõrkesiirde viisardi, mis avab, tehke järgmist.

    1. Valige ripploendist saadaolevate seadmete lisamine **Target seadme.** Ainult seadmetes, mis on piisavalt kuvatakse ripploendis.

    2. Vaadake üksikasjad, nt seadme nimi, koguvõimsus ja nimed üle nurjunud aktsiate allika seadmega seotud.

        ![](./media/storsimple-ova-failover-dr/image21.png)

7. Märkige ruut **I nõustute, et Tõrkesiirde on püsivate toiming ja pärast selle Tõrkesiirde on edukalt lõpule viidud, kustutatakse allika seade**.

8. Klõpsake ikooni Otsi ![](./media/storsimple-ova-failover-dr/image1.png).


9. Tõrkesiirde tööd alustada ja kuvab teate. Klõpsake nuppu **Kuva töö** on tõrkesiire jälgimiseks.

    ![](./media/storsimple-ova-failover-dr/image22.png)

10. **Töö** lehel kuvatakse Tõrkesiirde töökoha allika seade. See töö sooritab DR prechecks.

    ![](./media/storsimple-ova-failover-dr/image23.png)

    Pärast DR prechecks on edukalt, kuvatakse Tõrkesiirde töö kudema taastamine tööd iga ühiskasutus/maht, mis on olemas allika seadmes.

    ![](./media/storsimple-ova-failover-dr/image24.png)

11. Pärast selle Tõrkesiirde on lõpule jõudnud, avage leht **seadmed** .

    lisamine. Valige StorSimple virtuaalse seade, mida kasutati target seadme Tõrkesiirde protsessi.

    b. Liikuge lehele **ühtlasi** (või **mahud** kui iSCSI server). Nüüd olema lisatud kõik aktsiad (mahud) vana seadme kaudu.
    
    ![](./media/storsimple-ova-failover-dr/image25.png)

![](./media/storsimple-ova-failover-dr/video_icon.png)**Video saadaval**

See video näitab, kuidas saate ei õnnestu, üle StorSimple kohapealse virtuaalne seadme virtuaalse teise seadmesse.

> [AZURE.VIDEO storsimple-virtual-array-disaster-recovery]

## <a name="business-continuity-disaster-recovery-bcdr"></a>Järjepidevuse avariitaastet (BCDR)

Business järjepidevuse katastroof taastamine (BCDR) stsenaarium juhul, kui kogu Azure andmekeskuse lakkab töötamast. See võib mõjutada teie haldur StorSimple teenuse ja seotud StorSimple seadmetes.

Kui loendis on just enne on toimunud registreeritud StorSimple seadmete, siis need StorSimple seadmed võib tekkida vajadus kustutada. Pärast katastroofi, saate uuesti luua ja neid seadmete konfigureerimine.

## <a name="errors-during-dr"></a>Tõrgete ajal DR

**Pilveteenuse ühenduvuse katkestuste ajal DR**

Kui pilvepõhist ühenduvust katkestati pärast DR käivitas ja enne seadme taastamine on lõpule viidud, DR ei õnnestu ja kuvab teate. Target seade, mida kasutati DR on märgitud seejärel *kasutuskõlbmatuks.* Kas samasse seadmesse target ei saa kasutada siis tulevaste DRs.

**Puudub ühilduv target seadmed**

Kui saadaval target seadmete pole piisavalt vaba ruumi, kuvatakse tõrge selle kohta, mis on olemas puudub ühilduv target seadmed.

**Precheck tõrked**

Kui üks selle prechecks on täidetud, siis kuvatakse precheck ebaõnnestumist.

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet selle kohta, kuidas hallata [Oma StorSimple virtuaalse massiiv kohalik web Kasutajaliidese kaudu](storsimple-ova-web-ui-admin.md).
