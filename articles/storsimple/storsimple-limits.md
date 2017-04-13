<properties 
   pageTitle="StorSimple süsteemi piirangud | Microsoft Azure'i"
   description="Kirjeldatakse süsteemi piirangud ja Soovitatavad suurused StorSimple 8000 sarja komponendid ja ühendused."
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

# <a name="storsimple-system-limits"></a>StorSimple süsteemi piirangud

## <a name="overview"></a>Ülevaade

StorSimple pakub teie andmekeskuse scalable ja paindlikus salvestusruumi. Kuid on mõned piirangud, mis teil peaks meeles pidama nagu kavandamine, juurutada ja kasutada oma StorSimple lahendus. Järgmises tabelis on kirjeldatud need piirangud ja leiate mõned soovitused, et pääsete oma StorSimple lahenduse parimal viisil kasutada.

| Piiratud identifikaator | Limiit | Kommentaarid |
|----------------- | ------|--------- |
| Suurim lubatud arv salvestusruumi konto identimisteave | 64 | |
| Helitugevuse ümbriste arvu ülempiir | 64 | |
| Suurim lubatud arv mahud | 255 | |
| Kohalik kinnitatud mahud arvu ülempiir | 32 | |
| Suurim lubatud arv ajakava läbilaskevõimet malli kohta | 168 | Ajakava iga tund iga päev nädala (24 * 7). |
| Astmeline helitugevuse füüsilise seadmetes maksimaalne maht | 64 TB 8100 ja 8600 | 8100 ja 8600 on füüsilise seadmed. |
| Astmeline helitugevuse Azure virtuaalse seadmetes maksimaalne maht | 30 TB 8010 jaoks <br></br> 64 TB 8020 jaoks | 8010 ja 8020 on virtuaalse seadmete Azure Standard ja Premium vastavalt kasutada. |
| Kohalik kinnitatud helitugevuse füüsilise seadmetes maksimaalne maht | 8,5 TB 8100 jaoks <br></br> 22,5 TB 8600 jaoks | 8100 ja 8600 on füüsilise seadmed. |
| Suurim lubatud arv iSCSI ühendused | 512 | |
| Algataja iSCSI Ühenduste maksimumarv | 512 | |
| Juurdepääsu juhtimine kirjete seadme kohta | 64 | |
| Maksimumarv mahud varukoopia poliitika kohta. | 24 | |
| Varukoopiate kohta (varukoopia poliitika) ajakava säilitatakse arvu ülempiir | 64 | |
| Suurim arv ajakavasid varukoopia poliitika kohta. | 10 | |
| Tüüp, mida saab säilitada ruumala hetktõmmiste arvu ülempiir | 256 | See number sisaldab kohaliku hetktõmmiseid ja Pilveteenusest hetktõmmiseid luua. |
| Suurim lubatud arv pilte, mis võib olla mis tahes seadme | 10 000 | |
| Maksimaalne maht, mida saab töödelda paralleelselt varundada, taastada või klooni arv | 16 |<ul><li>Kui loendis on rohkem kui 16 maht, ei töödelda järjest kui töötlemine slots saadaval.</li><li>Enne selle toimingu lõpetamist, ei saa tekkida uue varukoopiaid on kloonitud või taastatud astmeline helitugevust. Kuid kohaliku maht, varukoopiate on lubatud pärast maht on võrguühendus.</li></ul>|
| Taastamine ja klooni taastamine aega astmeline mahud | < kaks minutit | <ul><li>Helitugevus on kättesaadav olenemata helitugevuse suuruse taastamine või klooni toimingute 2 minuti jooksul.</li><li>Helitugevuse esialgu võib aeglasem kui tavaline, nagu enamik andmed ja metaandmed asub endiselt pilveteenuses. Nimega andmete liikumine pilvest StorSimple seadme jõudlust suurendada.</li><li>Kogu aeg alla laadida metaandmete sõltub eraldatud maht. Metaandmete automaatne välisandmevahemik seadme taustal moodustab 5 minutit TB eraldatud helitugevuse andmete kohta. See määr võib mõjutada Interneti-läbilaskevõime pilveteenusesse.</li><li>Taastada või klooni toiming on lõpule jõudnud, kui kõik metaandmete on seadmes.</li><li>Varukoopia ei saa toiminguid kuni taastamine või klooni toiming on täielikult lõpule viidud.|
| Kohalik kinnitatud mahud taastamine aega taastamine | < kaks minutit | <ul><li>Helitugevus on kättesaadav taastetoimingu sõltumata mahu suurus 2 minuti jooksul.</li><li>Helitugevuse esialgu võib aeglasem kui tavaline, nagu enamik andmed ja metaandmed asub endiselt pilveteenuses. Jõudluse võib suurendada vastavalt andmevahetuse pilvest StorSimple seadmele.</li><li>Kogu aeg alla laadida metaandmete sõltub eraldatud maht. Metaandmete automaatne välisandmevahemik seadme taustal moodustab 5 minutit TB eraldatud helitugevuse andmete kohta. See määr võib mõjutada Interneti-läbilaskevõime pilveteenusesse.</li><li>Astmeline maht, kohalikult kinnitatud maht, erinevalt helitugevuse andmed laaditakse kohalikult seadmes. Taastetoimingu on lõpule jõudnud, kui kõik helitugevuse andmed on toodud seadmele.</li><li>Taasta toimingud võib olla pikk. Kogu aeg lõpuleviimiseks taastamine sõltub mahu ettevalmistatud kohaliku maht, Interneti-läbilaskevõime ja seadme olemasolevad andmed. Varukoopia toiminguid kohalikult kinnitatud maht on lubatud taastetoimingu ajal.|
|Pilveteenuse hetktõmmiseid töötlemine määr| 15 minutit/TB | <ul><li>Minimaalne aeg teha pilveteenuses hetktõmmise valmis üles TB eraldatud helitugevuse andmete varundamise kohta. </li><li> Aega kokku cloud hetktõmmise arvutatakse hetktõmmise üles ajal, mis mõjutavad Interneti-läbilaskevõime Cloud seekord lisamisega.
| Suurim klient lugemis-ja kirjutamisõigusega läbilaskevõime (kui kella SSD taseme) * | 920/720 MB/s ühtse 10 New York võrgu liidese | Kuni 2 x MPIO ja kahe võrgu liidesed. |
| Suurim klient lugemis-ja kirjutamisõigusega läbilaskevõime (kui kella HDD taseme) * | 120/250 MB/s |
| Suurim lubatud kliendi lugemis-ja kirjutamisõigusega läbilaskevõime (kui kätte: pilveteenuste taseme)* Update 3 ja uuemad versioonid** | 40/60 MB/s mitmetasandilise mahud<br><br>60/80 MB/s mitmetasandilise mahud arhiivimine helitugevuse loomise ajal valitud on suvand | Lugege läbilaskevõime sõltub klientide loomisel ja säilitades piisavalt I/O järjekorda sügavus. <br><br>Kiirus saavutada sõltub aluseks oleva salvestusruumi konto, mida kasutatakse kiirust. | 

& #42; Maksimaalse läbilaskevõime I/O tüüp on 100 protsenti lugemine ja kirjutamine 100 protsenti stsenaariumid mõõta. Tegelik läbilaskevõime võib olla väiksem ja sõltub I/O segada ja võrgu tingimused.

& #42; & #42; Jõudluse arvude enne Update 3 võib olla madalam.

## <a name="next-steps"></a>Järgmised sammud

Vaadake läbi [StorSimple süsteeminõuded](storsimple-system-requirements.md). 
