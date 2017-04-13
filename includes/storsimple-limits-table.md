<!--author=alkohli last changed: 12/15/15-->

| Piiratud identifikaator | Limiit | Kommentaarid |
|----------------- | ------|--------- |
| Suurim lubatud arv salvestusruumi konto identimisteave | 64 | |
| Helitugevuse ümbriste arvu ülempiir | 64 | |
| Suurim lubatud arv mahud | 255 | |
| Suurim lubatud arv ajakava läbilaskevõimet malli kohta | 168 | Ajakava iga tund iga päev nädala (24 * 7). |
| Astmeline helitugevuse füüsilise seadmetes maksimaalne maht | 64 TB 8100 ja 8600 | 8100 ja 8600 on füüsilise seadmed. |
| Astmeline helitugevuse Azure virtuaalse seadmetes maksimaalne maht | 30 TB 8010 jaoks <br></br> 64 TB 8020 jaoks | 8010 ja 8020 on virtuaalse seadmete Azure Standard ja Premium vastavalt kasutada. |
| Kohalik kinnitatud helitugevuse füüsilise seadmetes maksimaalne maht | 9 TB 8100 jaoks <br></br> 24 TB 8600 jaoks | 8100 ja 8600 on füüsilise seadmed. |
| Suurim lubatud arv iSCSI ühendused | 512 | |
| Algataja iSCSI Ühenduste maksimumarv | 512 | |
| Juurdepääsu juhtimine kirjete seadme kohta | 64 | |
| Maksimumarv mahud varukoopia poliitika kohta. | 24 | |
| Suurim arv varukoopiate säilitatakse varukoopia poliitika kohta. | 64 | |
| Suurim arv ajakavasid varukoopia poliitika kohta. | 10 | |
| Tüüp, mida saab säilitada ruumala hetktõmmiste arvu ülempiir | 256 | See hõlmab kohaliku hetktõmmiseid ja Pilveteenusest hetktõmmiseid luua. |
| Suurim lubatud arv pilte, mis võib olla mis tahes seadme | 10 000 | |
| Maksimaalne maht, mida saab töödelda paralleelselt varundada, taastada või klooni arv | 16 |<ul><li>Kui loendis on rohkem kui 16 maht, töödeldakse kui töötlemine slots saadaval järjestikku.</li><li>Enne selle toimingu lõpetamist, ei saa tekkida uue varukoopiaid on kloonitud või taastatud astmeline helitugevust. Kuid kohaliku maht, varukoopiate on lubatud pärast maht on võrguühendus.</li></ul>|
| Taastamine ja klooni taastamine aega astmeline mahud | < kaks minutit | <ul><li>Helitugevus on kättesaadav olenemata helitugevuse suuruse taastamine või klooni toimingute 2 minuti jooksul.</li><li>Helitugevuse algselt võib aeglasem kui tavaline, nagu enamik andmed ja metaandmed asub endiselt pilveteenuses. Nimega andmete liikumine pilvest StorSimple seadme jõudlust suurendada.</li><li>Kogu aeg alla laadida metaandmete sõltub eraldatud maht. Metaandmete automaatne välisandmevahemik seadme taustal moodustab 5 minutit TB eraldatud helitugevuse andmete kohta. See määr võib mõjutada Interneti-läbilaskevõime pilveteenusesse.</li><li>Taastada või klooni toiming on lõpule jõudnud, kui kõik metaandmete on seadmes.</li><li>Varukoopia ei saa toiminguid kuni taastamine või klooni toiming on täielikult lõpule viidud.|
| Kohalik kinnitatud mahud taastamine aega taastamine | < kaks minutit | <ul><li>Helitugevus on kättesaadav taastetoimingu sõltumata mahu suurus 2 minuti jooksul.</li><li>Helitugevuse algselt võib aeglasemalt tavaline, nagu enamik andmed ja metaandmed asub endiselt pilveteenuses. Nimega andmete liikumine pilvest StorSimple seadme jõudlust suurendada.</li><li>Kogu aeg alla laadida metaandmete sõltub eraldatud maht. Metaandmete automaatne välisandmevahemik seadme taustal moodustab 5 minutit TB eraldatud helitugevuse andmete kohta. See määr võib mõjutada Interneti-läbilaskevõime pilveteenusesse.</li><li>Erinevalt astmeline maht, kohalikult kinnitatud köite korral helitugevuse andmed laaditakse kohalikult seadmes. Taastetoimingu on lõpule jõudnud, kui kõik helitugevuse andmed on toodud seadmele.</li><li>Taasta toimingute võib olla pikk ja kogu aeg lõpuleviimiseks taastamine sõltub mahu ettevalmistatud kohaliku maht, Interneti-läbilaskevõime ja seadme olemasolevad andmed. Varukoopia toiminguid kohalikult kinnitatud maht on lubatud taastetoimingu ajal.|
| Õhuke taastamine-saadavus | Viimase Tõrkesiirde | |
| Suurim klient lugemis-ja kirjutamisõigusega läbilaskevõime (kui kella SSD taseme) * | 920/720 MB/s ühe 10GbE võrgu kasutajaliidese | Kuni 2 x MPIO ja kahe võrgu liidesed. |
| Suurim klient lugemis-ja kirjutamisõigusega läbilaskevõime (kui kella HDD taseme) * | 120/250 MB/s |
| Suurim lubatud kliendi lugemis-ja kirjutamisõigusega läbilaskevõime (kui kätte: pilveteenuste taseme) * | 11/41 MB/s | Lugege läbilaskevõime sõltub klientide loomisel ja säilitades piisavalt I/O järjekorda sügavus. |

& #42; Maksimaalse läbilaskevõime I/O tüüp on 100 protsenti lugemine ja kirjutamine 100 protsenti stsenaariumide mõõta. Tegelik läbilaskevõime võib olla väiksem ja sõltub I/O segada ja võrgu tingimused.
