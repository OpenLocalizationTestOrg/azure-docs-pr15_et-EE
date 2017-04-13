<properties
   pageTitle="StorSimple virtuaalse massiiv ülevaade | Microsoft Azure'i"
   description="Kirjeldatakse StorSimple virtuaalse massiiv integreeritud salvestusruumi lahenduse, mida haldab Microsoft Azure'i salvestusruumi ja kohapealse virtuaalse seadme salvestusruumi tasks."
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
   ms.date="10/06/2016"
   ms.author="alkohli" />

# <a name="introduction-to-the-storsimple-virtual-array"></a>Sissejuhatus StorSimple virtuaalse massiiv

## <a name="overview"></a>Ülevaade

Tere tulemast rakendusse Microsoft Azure'i StorSimple virtuaalse massiiv, integreeritud salvestusruumi lahenduse, haldab salvestusruumi tasks kohapealse virtuaalse seade töötab hypervisor ja Microsoft Azure'i salvestusruumi. Virtuaalne massiiv (tuntud ka kui StorSimple kohapealse virtuaalse seadme) on tõhusa, kulutõhus ja hõlpsasti hallatava failiserverisse või iSCSI serveri lahenduse, mis välistab paljude probleemide ja ettevõtte salvestusruumi ja andmekaitsega seotud. Virtuaalne massiiv on eriti sobivad hästi office remote/haru office (ROBO) stsenaariumid.

See ülevaade keskendub virtuaalse massiiv. 

- Ülevaate StorSimple 8000 sarja, minge [StorSimple 8000 sarja: hübriid pilve lahendus](storsimple-overview.md). 

- Lisateabe saamiseks StorSimple 5000/7000 sarja seade, avage [StorSimple spikker](http://onlinehelp.storsimple.com/).

Virtuaalne massiiv toetab iSCSI või Server sõnum Block (SMB) protokolli. See töötab hypervisor infrastruktuuri ja pakub tiering cloud pilve varundamise fast taastada, Üksusetaseme taastamine ja katastroofi taastamine funktsioonid.

Järgmises tabelis on kokkuvõte virtuaalse massiiv olulisi funktsioone.

| Funktsioon | Virtuaalne massiiv |
| ------- | ------------- |
|Installinõuded | Kasutab virtualization taristu (Hyper-V või VMware)|
| Kättesaadavus | Ühe sõlm |
| Koguvõimsus (sh cloud) |Kuni 64 TB kasutatav võimsus virtuaalse seadme kohta |
| Kohaliku võimsus | 390 GB 6,4 TB kasutuskõlblikud osas iga virtuaalse seadme (vaja ettevalmistamise 500 8 TB kettaruumi GB)|
| Kohalikke Protokollid | iSCSI või SMB |
| Taastamise aeg eesmärk (RTO) | iSCSI: vähem kui 2 minutit sõltumata suurus |
| Taastamine punkti eesmärk (RPO) | Igapäevane varukoopiate ja vajadusel varukoopiate |
| Salvestusruumi tiering | Intensiivsuskaardi kasutab vastenduse määrata, millised andmed sõltuvad sisse või välja |
| Tugi | Office'i Klõpskäivituse taristu tarnija ei toeta |
| Jõudluse | Erineb sõltuvalt aluseks oleva taristu |
| Andmete mobility | Kas samasse seadmesse saab taastada või Üksusetaseme taastamine (faili server) |
| Storage astme | Kohaliku hypervisor salvestus- ja pilveteenuses |
| Ühiskasutus suurus |Mitmetasandilise: kuni 20 TB; Kohalik kinnitatud: 2 TB |
| Maht |Mitmetasandilise: kuni 5 TB; Kohalik kinnitatud: kuni 500 GB |
| Hetktõmmiseid | Ühtse krahhi |
| Üksusetaseme taastamine | Jah; Kasutajad saavad taastamine aktsiad |

## <a name="why-use-storsimple"></a>Miks kasutada StorSimple?

StorSimple loob kasutajate ja serverite Azure storage minutites koos rakenduse muutmine.

Järgmises tabelis kirjeldatakse lahenduse virtuaalse massiivi sisaldava peamised eelised.

| Funktsioon | Kasu |
|---------|---------|
| Läbipaistva integreerimine | Virtuaalne massiiv toetab iSCSI või SMB protokolli. Andmete liikumine kohaliku taseme ja taseme pilveteenuse vahel on sujuvalt ja kasutajale läbipaistev.|
| Vähendatud salvestusruumi kulud | StorSimple, mille olete ette piisavalt kohaliku salvestusruumi täita praeguse nõudmistele enim kasutatud saadaolevad andmed. Salvestusruumi vajadused kasvavad, StorSimple astme külma andmete rakendusse kulutõhus salvestusruum pilveteenuses. Andmed on deduplicated ja tihendatud enne saatmist pilveteenusesse vähendada salvestusruumi nõuded ja kulud.|
| Lihtsustatud mäluhaldus | StorSimple pakub tsentraliseeritud haldamine mitmes seadmes StorSimple halduri abil pilveteenuses.| 
| Täiustatud katastroofiabi ja nõuetele vastavus | StorSimple hõlbustab kiirem avariitaastet metaandmete kohe taastamine ja andmete taastamine vastavalt vajadusele. See tähendab, et toiminguid saate jätkata minimaalsete segadusi vältida.|
| Andmete mobility | Andmete mitmetasandilise pilveteenusesse pääseb muude saitide taastamine ja migreerimise eesmärkidel. Pange tähele, et saate taastada andmed ainult algse virtuaalse massiiv. Siiski kasutada katastroofi taastamine funktsioonid taastada kogu virtuaalse massiiv teise virtuaalse massiiv.|

## <a name="storsimple-workload-summary"></a>StorSimple töökoormus Kokkuvõte

Allpool on esitatud tabelina Kokkuvõte toetatud StorSimple töökoormus.

| Stsenaarium                | Töökoormus              | Toetatud |  Piirangud                                  | Versioon              |
|-------------------------|-----------------------|-----------|------------------------------------------------|----------------------|
| ROBO koostöö      | Failide ühiskasutus          | Jah       | Vaata [faili serveri ülempiirid](storsimple-ova-limits.md). <br>Lugege teemat [toetatud SMB versiooni süsteeminõuded](storsimple-ova-system-requirements.md).   | Kõik versioonid      |


## <a name="workflows"></a>Töövood

StorSimple virtuaalse massiiv sobib eelkõige järgmised töövood:

- [Pilvepõhist salvestusruumi haldamine](#cloud-based-storage-management)
- [Asukoha sõltumatul varundamine](#location-independent-backup)
- [Andmete kaitse ja katastroofi taastamine](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>Pilvepõhist salvestusruumi haldamine

Azure'i klassikaline portaalis töötab StorSimple halduri teenuse abil saate haldamine mitmes seadmes ja mitmes asukohas talletatud andmed. See on eriti kasulik jaotatud haru stsenaariumid. Pange tähele, et peate looma eraldi eksemplarid StorSimple halduri teenuse haldamiseks virtuaalse massiivid ja füüsilise StorSimple seadmed. 

![pilvepõhist salvestusruumi haldamine](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Asukoha sõltumatul varundamine

Virtuaalne massiiv cloud hetktõmmiseid annavad asukoht sõltumatul, punkti /-kellaaja koopia helitugevust ja ühiskasutusse anda. Pilveteenuse hetktõmmiseid on vaikimisi sisse lülitatud ja ei saa keelata. Kõik mahud ja ühtlasi on varukoopia üles ühe päeva varukoopia poliitika kaudu samal ajal ja täiendavad adhoc varukoopiate vajaduse korral saate teha.

### <a name="data-protection-and-disaster-recovery"></a>Andmete kaitse ja katastroofi taastamine

Virtuaalne massiiv toetab järgmised andmed kaitse ja katastroofi taastamise stsenaariumid:

- **Helitugevuse ja ühiskasutusse anda taastamine** – uue töövoo kasutamine taastamine taastada helitugevust ja ühiskasutusse anda. Seda moodust abil saate taastada kogu ja ühiskasutusse anda.
- **Üksusetaseme taastamine** – aktsiad luba lihtsustatud juurdepääsu Viimatised varukoopiad. Üksiku faili saab taastada hõlpsalt pilveteenuses saadaval teisiti .backup kausta. See taastamine võimalus on kasutajapõhine ja pole haldus sekkumine on vajalik.
- **Avariitaastet** – kasutage Tõrkesiirde võimalus taastada kõigi mahud või aktsiate uue virtuaalse massiiv. Saate luua uue virtuaalse massiivi ja registreerida StorSimple halduri teenuse, siis ei õnnestu üle virtuaalse algse massiiv. Uue virtuaalse massiivi seejärel endale ettevalmistatud ressursid. 

## <a name="virtual-array-components"></a>Virtuaalne massiivikomponentide

Virtuaalne massiiv sisaldab järgmised komponendid.

- [Virtuaalne massiiv](#virtual-array) – hübriid pilve mäluseadmesse virtuaalse masina ette valmistatud virtualiseeritud keskkonnas või hypervisor põhjal.  
- [StorSimple halduri teenuse](#storsimple-manager-service) – laiend Azure klassikaline portaali, mis võimaldab teil ühe või mitme StorSimple seadmete haldamine ühe web interface, millele pääsete erinevates kohtades. Saate luua ja hallata teenuste, vaadata ja seadmed ja teatiste haldamine ja hallata maht, osakud ja olemasolevaid pilte StorSimple halduri teenuse.
- [Kohalik web kasutajaliidese](#local-web-user-interface) -Veebipõhine kasutajaliides, mis saab konfigureerida nii, et see ühenduse loomine kohaliku võrgu ja seejärel registreerida StorSimple halduri teenuse seade seadme. 
- [Käsurea liides](#command-line-interface) – Windows PowerShelli kasutajaliidese, mida saate kasutada virtuaalse massiivi tugi seansi käivitamine.
Järgmistes jaotistes kirjeldatakse iga nende komponentide täpsemalt ning selgitavad, kuidas lahendus korraldab andmeid, määrab mälu ja hõlbustab mäluhaldus ja andmekaitse.

### <a name="virtual-array"></a>Virtuaalne massiiv

Ühe sõlme salvestusruumi lahenduse, mis pakub esmane, haldab salvestusruumi suhtlemine ja aitab tagada turvalisuse ja konfidentsiaalsusega kõik andmed, mis on talletatud seadme virtuaalse massiiv.

Virtuaalne massiiv on esitatud ühe mudelit, mis on allalaadimiseks saadaval. Salvestusruumi massiiv on maksimaalne maht on 6,4 TB seadme (koos mõne aluseks salvestusruumi nõue 8 TB) ja 64 TB salvestusruumi cloud. 

Virtuaalne massiiv sisaldab järgmisi funktsioone.

- See on tõhus. See muudab teie virtualization taristu kasutada, ja saab juurutada olemasoleva Hyper-V või VMware hypervisor.
- See asub andmekeskuses ja saab konfigureerida mõne iSCSI serveri või faili serverisse. 
- See on integreeritud pilve.
- Varukoopiate talletatakse pilves, mis võivad lihtsustada avariitaastet ja lihtsustada Üksusetaseme taastamine (ILR). 
- Saate rakendada värskenduste virtuaalse massiiv, nagu sooviksite neid rakendamine füüsilise seadmega.

>[AZURE.NOTE] Virtuaalne massiiv ei saa laiendada. Seetõttu on oluline ettevalmistamise piisavalt salvestusruumi virtuaalse seadme loomisel. 

### <a name="storsimple-manager-service"></a>StorSimple halduri teenuse

Microsoft Azure'i StorSimple sisaldab veebipõhise kasutajaliidese (StorSimple halduri teenus), mis võimaldab teil ühes keskses kohas hallata andmekeskuse ja salvestusruum pilveteenuses. StorSimple halduri teenuse abil saate teha järgmisi toiminguid:

- Ühe teenuse kaudu hallata mitut StorSimple virtuaalse massiivi. 
- Konfigureerimine ja haldamine turbesätted StorSimple seadmete jaoks. (Krüptimise pilveteenuses on sõltuv Microsoft Azure'i API-d).
- Salvestusruumi konto identimisteave ja atribuutide konfigureerimine.
- Konfigureerimine ja haldamine mahud või aktsiad.
- Varundamise ja taastamise andmete maht või aktsiad.
- Jõudluse jälgimist.
- Vaadake üle süsteemisätetest ja tuvastada võimalikke probleeme.

StorSimple halduri teenuse abil saate teha oma virtuaalse massiivi iga päev haldamine.

Lisateabe saamiseks lugege [kasutamine StorSimple halduri teenuse haldamiseks StorSimple seadme](storsimple-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Kohaliku web kasutajaliides

Virtuaalne massiiv sisaldab veebipõhine UI, et kasutatakse ühekordse konfigureerimine ja StorSimple halduri teenuse seadme registreerimise. Saate selle sulgeda ja uuesti virtuaalse massiiv diagnostika analüüse, tarkvara, seadme administraatori parooli muutmine, vaatamine süsteemi logid ja ühendust Microsofti Support esitada teenusetaotluse. 

Veebipõhise Kasutajaliidese kasutamise kohta leiate teavet minge [kasutamine hallata oma StorSimple virtuaalse massiiv Veebipõhine kasutajaliides](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Käsurea liides

Kaasatud Windows PowerShelli kasutajaliidese võimaldab teil nii, et nad saaksid aidata teil tõrkeotsing ja lahendada probleeme, mis võivad ilmneda virtuaalse seadme tugi seansi Microsoft Support algatada.

## <a name="storage-management-technologies"></a>Salvestusruumi haldamine tehnoloogiad

Lisaks virtuaalse massiiv ja muud komponendid, StorSimple lahendus kasutab järgmist tarkvara tehnoloogiad kiireks juurdepääsuks olulisi andmeid sisestada, vähendada salvestusruumi ja kaitsta oma virtuaalse massiiv salvestatud andmed:

- [Automaatse salvestusruumi tiering](#automatic-storage-tiering) 
- [Kohalik kinnitatud osakud ja mahud](#locally-pinned-shares-and-volumes)
- [Korduste eemaldamise ja andmete tihendamise mitmetasandilise või pilveteenusesse varundatud](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
- [Ajastatud ja vajadusel varukoopiate](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Automaatse salvestusruumi tiering

Virtuaalne massiiv kasutab uue tiering süsteemi üle virtuaalse massiiv ja pilves talletatud andmete haldamine. On vaid kahe astme: kohaliku virtuaalse massiiv ja Azure pilve salvestusruum. StorSimple virtuaalse massiiv korraldab andmete automaatselt sisse astme, mis põhineb intensiivsuskaart, mis jälgib praeguse kasutuse, vanus ja seosed muudele andmetele. Andmed, mis on aktiivne (kuumim) on talletatud kohalik, samal ajal vähem aktiivne ja passiivne andmete migreeritakse automaatselt pilveteenusesse. (Kõik varukoopiate talletatakse pilves). StorSimple reguleerib ja korraldab andmed ümber ning salvestusruumi ülesanded nimega mustreid muuta. Näiteks osa teabest võib muutuda vähem aktiivne aja jooksul. Kui see on järk-järgult vähem aktiivne, on see pilveteenusesse välja mitmetasandilise. Kui sama andmeid uuesti aktiivseks, on see salvestusruumi massiivile rakenduses mitmetasandilise.

Andmete kindla astmeline ühiskasutus või helitugevus on tagatud oma kohaliku taseme ruumi. (10% kokku ettevalmistatud ruumi ühiskasutus või helitugevuse). Kui see vähendab saadaolevat virtuaalse seadme ühiskasutus või helitugevuse, see tagab, et ühele ühiskasutus või helitugevuse tiering ei mõjuta tiering vajadustega muude aktsiate või maht. Seega on väga hõivatud töökoormus ühes ühiskasutus või maht ei saa jõustada muude töökoormus pilveteenusesse. 

![automaatse salvestusruumi tiering](./media/storsimple-ova-overview/automatic-storage-tiering.png)

>[AZURE.NOTE] Saate määrata nii kohalikult kinnitatud maht, sellisel juhul andmeid hoitakse virtuaalse massiivi ja pole kunagi mitmetasandilise pilveteenusesse. Lisateabe saamiseks minge [kohalikult kinnitatud osakud ja maht](#locally-pinned-shares-and-volumes).

### <a name="locally-pinned-shares-and-volumes"></a>Kohalik kinnitatud osakud ja mahud

Saate luua vastav osakud ja mahud kohaliku kinnitatud. See funktsioon tagab nõutud kriitiliste rakenduste andmete jääb massiivis virtuaalse ja pole kunagi mitmetasandilise pilveteenusesse. Kohalik kinnitatud osakud ja mahud kuuluvad järgmised funktsioonid. 

- Ta ei kuulu cloud latentsused või ühenduvusprobleemide.
- Siiski kasu StorSimple pilve varundus ja katastroofi taastamise funktsioone.

Saate taastada kohalikult kinnitatud ühiskasutus või mis on mitmetasandilise või astmeline ühiskasutus või helitugevuse kohaliku kinnitatud. 

Lugege lisateavet kohalikult kinnitatud mahud [kasutamine StorSimple halduri teenuse haldamiseks maht](storsimple-manage-volumes-u2.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Korduste eemaldamise ja andmete tihendamise mitmetasandilise või pilveteenusesse varundatud

StorSimple kasutab korduste eemaldamise ja andmete tihendamise täpsemaks vähenda pilveteenuses. Korduste eemaldamise üldine vähendab kõrvaldamise koondamise salvestatud andmehulgas säilitatavate andmetega. Teave muutub, StorSimple ignoreerib samaks andmed ja salvestab ainult soovitud muudatused. Lisaks StorSimple vähendab salvestatud andmeid ja koolitus dubleeritud teavet. 

>[AZURE.NOTE] Salvestatud virtuaalse massiiv andmed on deduplicated ega tihendatud. Kõik deduplication ja tihendamise ilmneb just enne seda, kui andmed on saadetud pilveteenusesse.

### <a name="scheduled-and-on-demand-backups"></a>Ajastatud ja vajadusel varukoopiate

StorSimple andmete kaitse funktsioonid võimaldavad teil luua nõudmisel varukoopiad. Lisaks varukoopia vaikeajakava tagab, et andmed on varundatud iga päev. Varukoopiate võetakse suureneva pilte, mis talletatakse pilves vorm. Pärast viimast varundamist ainult muudatuste salvestamiseks hetktõmmiseid saab luua ja kiiresti taastada. Nende näidete võib olla kriitilise tähtsusega Avariijärgne taaste stsenaariumid, sest need asendamine teisene salvestusruumi süsteemide (nt lint varukoopia) ja võimaldab teil andmeid oma andmekeskuse või alternatiivse saitide vajadusel taastada.

## <a name="next-steps"></a>Järgmised sammud

Siit saate teada, kuidas [valmistada virtuaalse massiiv portaali](storsimple-ova-deploy1-portal-prep.md).


