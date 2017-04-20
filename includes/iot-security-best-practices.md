# <a name="internet-of-things-security-best-practices"></a>Asjade Interneti parimad

Tagamiseks on asjade Interneti (IoT) infrastruktuuri vaja range turvalisuse põhjalikud strateegiat. Selle strateegia jaoks on vaja kindlustada pilves, kaitsta andmete terviklikkuse liikudes üle avaliku Interneti ning kindlalt ette valmistada seadmeid. Iga kiht ehitab suurem üldise infrastruktuuri turvalisuse tagamisel.

## <a name="secure-an-iot-infrastructure"></a>Tagada asjade Interneti infrastruktuuri

Turvalisuse põhjalikud strateegia saab töötatakse välja ja viiakse ellu aktiivselt tegelenud tootmise, arendamise ja IoT seadmete ja infrastruktuuri osalejate. Pärast on need mängijad üksikasjalikult.  

- **Asjade Interneti riistvara tootja/integraator**: tavaliselt on need sinna, integreerijad kokkupanek riistvara eri tootjatelt või IoT juurutamine valmistatud või muude tarnijate poolt integreeritud riistvara osutajate IoT riistvara tootjad.
- **Asjade Interneti lahendus arendaja**: IoT lahendust arengu tavaliselt teevad seda lahendus arendaja. Arendaja võib osa hotelli meeskond või selle tegevuse spetsialiseerunud süsteemiintegraator (SI). IoT lahendust arendaja saab arendada IoT lahendust nullist erinevate komponentide, integreerida erinevate off-the-shelf või avatud lähtekoodiga komponendid või eelkonfigureeritud leidma väiksemaid kohandamisel.
- **IoT lahendust juurutaja**: pärast an IoT lahendust on arenenud, tuleb kasutusele võtta välja. Tegemist riistvara, seadmete sidumist ning lahendusi riistvaraseadmed või pilve kasutuselevõtu.
- **IoT lahendust operaator**: pärast IoT lahendust juurutatakse, see nõuab pikaajalise tegevuse, järelevalve, uuendamine ja hooldus. Seda saab teha hotelli meeskond, mis hõlmab teavet tehnoloogia spetsialistid, riistvara toimingud ja hoolduse meeskondade ja domeeni spetsialistid, kes jälgida korrektset käitumist üldiste asjade Interneti infrastruktuuri.

Järgnevates jaotistes pakkuda parimate tavade igaüks neist aidata arendada, juurutada ja kasutada turvalise asjade Interneti infrastruktuuri.

## <a name="iot-hardware-manufacturerintegrator"></a>Asjade Interneti riistvara tootja/integraator

Järgnevalt IoT riistvara tootjad ja riistvara integreerijad parimad tavad.

- **Reguleerimisala riistvara miinimumnõuded**: riistvara disain peaks sisaldama on minimaalne funktsioonid, toimimiseks riistvara, ja ei midagi enamat. Näiteks peab sisaldama USB-porti, ainult siis, kui seadme toimimiseks vajalikud. Seadme puhul tuleks vältida soovimatu ründevektoreid avamine neid lisafunktsioone.
- **Veenduge, et riistvara rikkumiskindlad**: luua mehhanismid, mis tuvastab füüsilise rikkumise eest, näiteks seadme kaane avamine või seadme osa. Need tamper signaale võib üles laadida pilves, mis võiks juhivad need ettevõtjad andmevoos osa.
- **Ehitada ümber turvaline riistvara**: kui OMAHIND võimaldab ehitada turvaelemente nagu turvaline ja krüpteeritud või boot funktsiooni vastavalt kohta usaldusväärse platvormi mooduli (TPM). Need funktsioonid muudavad seadmete turvalisuse suurendamiseks ja üldise asjade Interneti infrastruktuuri kaitsmiseks.
- **Turvaline uuendused**: kestel ning seadme püsivara uuendamine on paratamatu. Turvalist teed seadmete uuendamine ja krüptograafilise tagamise firmware versioonid hoone võimaldab seade on turvaline ja pärast uuendamine.

## <a name="iot-solution-developer"></a>Asjade Interneti lahendus arendaja

Asjade Interneti lahenduste arendajatele parimad tavad on järgmised:

- **Järgida turvalise tarkvara väljatöötamise metoodika**: turvalise tarkvara arendamine nõuab maa-up mõelda turvalisuse, algusest lõpuni, et selle rakendamise, katsetamise ja juurutamise projekti. Platvormid, keeled ja vahendite valikuid kõik mõjutavad nimetatud metoodikaga kooskõlas. Microsoft Security arendustsükli näeb hoone turvalise tarkvara samm-sammult lähenemine.
- **Vali avatud lähtekoodiga tarkvara hoolikalt**: avatud lähtekoodiga tarkvara annab võimaluse kiiresti arendada lahendusi. Avatud lähtekoodiga tarkvara valides peaksite arvestama ühenduse iga avatud lähtekoodiga komponendi aktiivsus. Aktiivne tagab tarkvara toetab ja küsimused on avastanud ja tegeleda. Teise segased ja passiivne avatud lähtekoodiga tarkvara ei pruugi olla toetatud ja küsimusi ilmselt ei pruugi avastada.
- **Integreerida hoolikalt**: paljud tarkvara turvalisuse vigu esineb raamatukogud ja API-d. Funktsioone, mis ei pruugi olla vaja praeguse juurutamise olla kättesaadavad API kiht. Üldise turvalisuse tagamiseks veenduge, et kontrollida kõiki liideseid komponentide integreeritud turvalisuse vigu.      

## <a name="iot-solution-deployer"></a>IoT lahendust juurutaja

Asjade Interneti lahenduse esimene põhitõed on järgmised:

- **Kasutada riistvara kindlalt**: IoT kasutuselevõttu võivad vajada riistvara ebaturvalist kohtades, nagu näiteks avaliku ruumi või järelevalveta locales kasutusele võtta. Sellistel puhkudel veenduge, et riistvara juurutamine on kaitstud palju. Kui USB ja muud sadamad on saadaval riistvara, veenduge, et nad on turvaliselt kaetud. Paljud ründevektoreid kasutada järgmisi kanne punktidena.
- **Turvalisuse autentimise võtmed**: juurutamisel, iga seade nõuab seadme ID-d ja sarnased loodud pilve teenuse autentimise võtmed. Isegi pärast kasutamist hoida need võtmed füüsiliselt ohutu. Ohustatud võti saab maskeeruda olemasoleva seadme pahatahtliku seade.

## <a name="iot-solution-operator"></a>IoT lahendust operaator

IoT lahendust ettevõtjate parimad tavad on järgmised:

- **Süsteemi ajakohasena hoidmine**: tagada uusima versiooniga täiendatakse seadme operatsioonisüsteemi ja seadme draiverid. Kui lülitate automaatsed värskendused Windows 10 (IoT või teiste SKUs) Microsoft hoiab kursis, anda turvalise operatsioonisüsteemi IoT seadmete. Hoides teiste operatsioonisüsteemide (nt Linux) ajakohastatud aitab tagada samuti kaitstakse pahatahtlike rünnakute.
- **Kaitse kuritahtliku tegevuse vastu**: kui operatsioonisüsteemi installida viirusetõrje- ja ründevaratõrjeprogrammi viimaseid iga seadme operatsioonisüsteemi. See aitab leevendada Viimane väliste ohtude. Saate vastavaid operatsioonisüsteemides ohtude vastu kaitsta.
- **Auditi sageli**: auditeerimise asjade Interneti infrastruktuuri turvalisusega seotud küsimustes on oluline turvaintsidentidele reageerimise. Enamik operatsioonisüsteemide sisseehitatud sündmuste logimine, et vaadatakse sageli veendumaks, et ükski turvalisust on rikutud. Auditi teavet võib saata eraldi telemeetria Stream pilveteenusesse kus võib analüüsida.
- **Füüsiliselt kaitsta asjade Interneti infrastruktuuri**: halvim rünnakutest vastu asjade Interneti infrastruktuuri välja kasutades füüsilist juurdepääsu seadmetele. Üks tähtis ohutusalane tegevus on kaitseks pahatahtliku kasutamise USB-porti ning muud füüsilise. Üks võti paljastamiseks rikkumistest, mis võis toimuda logib ligipääsu, nagu USB port kasutamine. Uuesti, Windows 10 (asjade Interneti ja teiste SKUs) lubab need üksikasjalikku logimist.
- **Kaitse pilve mandaat**: pilve autentimiseks mandaadi konfigureerimine ja tegutsevad IoT juurutamine on võib-olla kõige mugavam ja ohustada IoT süsteemi. Kaitsta mandaat parool muutuvad sageli ja mitte kasutama seda mandaati avaliku masinad.

Võimalusi erinevate IoT seadmete erinevad. Mõned seadmed võivad olla arvutid, milles töötavad ühise Töölaua operatsioonisüsteemid ja mõned seadmed võib töötada väga kerge operatsioonisüsteemi. Parimaid turvalisuse tavasid kirjeldatud varem võidakse rakendada nende seadmete erineval määral. Kui, järgida täiendavaid turvalisuse ja juurutamise parimad tavad nende seadmete tootjatelt.

Mõned pärand ja piiratud seadmed võib ei on mõeldud spetsiaalselt IoT juurutamine. Need seadmed võivad puududa võime krüpteerida andmeid, luua ühenduse Internetiga või anda kogenud auditeerimine. Sellisel juhul saab kaasaegne ja turvaline värav koguda andmeid vanemate seadmetega ja nõutav nende seadmete Interneti turvalisuse. Väli väravaid võib pakkuda turvalist autentimist, läbirääkimiste krüptitud istungid, saamist käsud pilve ja paljud muud turvafunktsioonid.
