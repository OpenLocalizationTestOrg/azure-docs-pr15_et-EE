<properties
   pageTitle="Vahemällu salvestamise juhised | Microsoft Azure'i"
   description="Juhised vahemällu parandamiseks jõudlus ja skaleeritavus."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/14/2016"
   ms.author="masashin"/>


# <a name="caching-guidance"></a>Vahemällu salvestamise juhised

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

Vahemällu on levinud meetod, mis eesmärk on jõudlus ja skaleeritavus süsteemi parandada. See ajutiseks kopeerides sageli külastatud andmete kiire salvestusruumi, mis asub lähedal rakendus. Kui selle kiire andmesalv asub lähemal kui algallikas rakendusse, siis vahemällu võib oluliselt parandada klientrakendustes vastamise aeg, serveeritakse andmeid kiiremini.

Vahemällu on kõige tõhusam kliendi eksemplari loeb korduvalt samu andmeid, eriti siis, kui kõik järgmised tingimused rakendamine algse andmesalve:
- Jääb suhteliselt staatilise.
- See on aeglane, võrrelduna vahemälu kiirust.
- See kehtib väide kõrge.
- On eemal, kui võrgu latentsus, võivad põhjustada access aeglane.

## <a name="caching-in-distributed-applications"></a>Vahemällu hajutatud rakendusi

Hajutatud rakendusi rakendada tavaliselt ühe või mõlemad järgmised strateegiad, kui andmete vahemällu:

- Privaatne vahemälu, kus andmeid hoitakse kohapeal arvutis, kus töötab eksemplari rakenduse või teenuse abil.
- Ühiskasutuses vahemälu, serveeritakse levinud allikana, mida saab kasutada mitut protsesside ja/või masinad abil.

Mõlemal juhul vahemällu saab teha kliendipoolne ja/või serveripoolne. Kliendipoolne vahemälu tehakse protsess, mis pakub kasutajaliidese süsteemi, nt veebibrauseri või töölauarakendus.
Serveripoolne vahemällu tehakse protsessi, mis pakub business teenuste kaugühenduse teel töötavad.

### <a name="private-caching"></a>Privaatne vahemällu talletamine

Kõige olulisemaid vahemälu on-mälu poest. See on ühe protsessi aadressiruumi olevad ja korrad koodi, mis töötab selle protsessi. Seda tüüpi vahemälu on väga kiiresti juurde pääseda. Samuti saate ette äärmiselt tõhusaid talletamise mõõdukas staatilise andmemahtude, kuna tavaliselt piirab suurus, vahemälu maht mälu, mis on saadaval majutusteenuse protsessi arvutis.

Kui vajate rohkem teavet, kui on võimalik füüsilise mälu vahemälu, saate kirjutada kohaliku failisüsteemi vahemällu talletatud andmetega. See aeglasem kui andmeid, mis toimub-mälu juurde, kuid peaksid olema ikka kiirem ja võrreldes andmete toomine kaudu.

Kui teil on mitu eksemplari rakendus kasutab seda mudelit, mis töötab üheaegselt, iga rakenduse eksemplari on oma sõltumatu vahemälu hoides oma andmetest koopia.

Võrrelda vahemälu hetktõmmise algsed andmed mingil hetkel minevikus. Kui andmed ei ole staatiline, on tõenäoline erinevate teenuserakenduse eksemplaride hoida oma vahemälu erinevate versioonide andmed. Seetõttu sama päringu poolt juhul võib tagastada teistsuguseid tulemusi, nagu on näidatud joonisel 1.

![Mõni vahemälu eri juhtudel rakenduse kasutamine](media/best-practices-caching/Figure1.png)

_Joonis 1: Abil soovitud vahemälu eri juhtudel rakenduse_

### <a name="shared-caching"></a>Ühiskasutusse antud, vahemällu talletamine

Ühiskasutuses vahemälu abil saate probleemid, et andmed võivad erineda iga vahemälu, mis võivad ilmneda-mälu vahemälu leevendada. Ühiskasutusega vahemällu tagab, et muu rakenduse eksemplari vahemällu salvestatud andmeid sama kuvada. See selle asukoha vahemälu eraldi asukohta, tavaliselt majutatud osana eraldi teenus, nagu on näidatud joonisel 2.

![Ühiskasutuses vahemälu kasutamine](media/best-practices-caching/Figure2.png)

_Joonis 2: Kaudu ühiskasutusse antud vahemälu_

Ühiskasutusega vahemällu lähenemisviisi olulist kasu on skaleeritavus, pakub. Paljude ühiskasutuses vahemälu teenuste rakendamise abil kobar serverite ja kasutada tarkvara, mis jagab andmed üle klaster on läbipaistev. Rakenduse eksemplari lihtsalt saadab taotluse vahemälu teenuse.
Aluseks infrastruktuuri on vahemällu talletatud andmetega klaster asukoha määramiseks. Vahemälu saate hõlpsasti skaala, lisades rohkem servereid.

On kaks peamist miinust ühiskasutusega vahemällu lähenemisviisi.
- Vahemälu on aeglasem juurde pääseda, kuna seda pole enam hoitakse kohapeal iga rakenduse eksemplari.
- Nõude eraldi vahemälu teenuse võimalik lahendus keerukuse lisada.

## <a name="considerations-for-using-caching"></a>Teave kasutamise vahemällu talletamine

Järgmistes jaotistes kirjeldatakse üksikasjalikumalt kaalutlused kujundamise ja kasutamise vahemälu.

### <a name="decide-when-to-cache-data"></a>Otsustada, millal andmete vahemälu

Vahemällu võib oluliselt parandada kättesaadavus jõudlus ja skaleeritavus. Teil on rohkem andmeid ja suurem kasutajate arv, kes vajavad juurdepääsu andmed, seda suurem kasu saada vahemällu. On põhjus vahemällu vähendab latentsus ja argument, mis on seotud käsitleda suuri samaaegseid taotlusi algse andmesalve.

Näiteks andmebaasi võivad toetada teatud samaaegseid ühendusi. Andmete allalaadimine ühiskasutuses vahemälu siiski aluseks oleva andmebaasi, mitte võimaldab klientrakendusega juurde pääseda isegi juhul, kui saadaval ühenduste arv on praegu lõpe andmed. Lisaks, kui andmebaas pole saadaval, võib olla klientrakendustes jätkata, kasutades andmeid hoitakse vahemälu.

Soovitatav on sageli lugeda, kuid harva (nt suurema osa toimingud kui kirjutada andmeid) muuta andmete vahemällu. Siiski ei ole soovitatav kasutada vahemälu olulise teabe autoriteetsete pood. Selle asemel, veenduge, et alati salvestatakse kõik muudatused, mida ei saa rakenduse endale lubada püsivate andmesalve. See tähendab, et kui vahemälu pole saadaval, siis rakenduse võib siiski jätkata, kasutades andmesalve ja ei lähe kaotsi olulist teavet.

### <a name="determine-how-to-cache-data-effectively"></a>Andmete tõhus vahemälu määratlemine

Võti vahemälu tõhus kasutamine seisneb määratlemine kõige paremini andmete vahemälu ja vahemällu see sobival ajal. Andmeid saab lisada rakenduse tuuakse nõudmisel esmakordsel vahemällu. See tähendab, et rakendus peab ainult üks kord andmeid tuua andmesalve ja selle hilisema Accessi saab täidetud vahemälu.

Teise võimalusena vahemälu võib olla osaliselt või täielikult asustada ette, tavaliselt rakenduse käivitamisel (tuntud külv lähenemisviisi). Siiski võib olla soovitatav rakendada külv suurte vahemälu, kuna seda moodust saab määrata algse andmesalve ootamatu, suur koormus kui rakendus käivitub, kus töötab.

Sageli mustreid analüüsi aitab teil otsustada, kas täielikult või osaliselt eeltäita vahemälu ja valida andmete vahemällu. Näiteks võib olla kasulik seeme staatilise kasutaja profiili andmete vahemälu klientidele, kes kasutavad regulaarselt rakenduse (ehk iga päev), kuid mitte klientidele, kes kasutavad rakenduse ainult üks kord nädalas.

Vahemällu tavaliselt töötab ka andmeid, mis on püsiv või mis muutub harva. Näiteks viide teavet, nt toote ja hinnateavet pood rakendusest või ühiskasutusega staatilise ressursse, mis on kulukas ehitada. Mõnda või kõiki neid andmeid saab laadida rakenduse käivitamisel minimeerimiseks nõudmisel ressursid ja jõudluse parandamiseks vahemälu. See võib olla sobiv tausta protsessi, mis värskendab perioodiliselt viide on andmete vahemälu tagamaks, et see on ajakohane või mis värskendab vahemälu kui lähteandmete muudatused.

Vahemällu on vähem kasulik dünaamilise, kuigi on mõned erandid see (vt jaotist vahemälu väga dünaamiline andmete lisateabe saamiseks selle artikli). Kui algsed andmed muutub regulaarselt, vahemälus talletatud teave muutub aegunud väga kiiresti või üldkulu vahemälu sünkroonimine algse andmesalve vähendab tõhusust vahemällu.

Pange tähele, et vahemälu ei saa üksuse täieliku andmete kaasamiseks. Näiteks kui andmete üksuse tähistab mitme väärtusega objekti, näiteks panga kliendi nimi, meiliaadress ja konto saldo, osa elemente võib muutu (nt nimi ja aadress), teised (nt konto saldo) aga dünaamilisemaks. Sel juhul võib olla kasulik staatilise osade andmete vahemälu ja tuua (või arvutamine) ainult ülejäänud teave, kui see on nõutav.

Soovitame teostada jõudluse testimine ja kasutamine analüüsi kindlaks määrata, kas vahemälu või mõlemaid, eelse populatsiooni või nõudmisel laadimist. Otsus põhinema volatiilsus ja kasutus mustri andmed. Vahemälu kasutamise ja jõudluse analüüs on eriti oluline rakendustes, mis ilmneda raske laadimise ja peab olema väga paindlik. Näiteks väga paindlik stsenaariumide võib mõttekas seeme vahemälu vähendamiseks andmesalve koormus tippväärtus ajal.

Vahemällu saate kasutada ka korduvate arvutuste, kui rakendus töötab vältimiseks. Kui toimingu muudab andmete või sooritab keerukaid arvutusi, seda saab salvestada selle toimingu tulemuste vahemälu. Kui sama arvutuse nõutakse hiljem, rakenduse saab lihtsalt toomiseks vahemälu.

Rakenduse saate muuta andmeid, mis toimub vahemälu. Kuid soovitame mõelda vahemälu nimega siirdamiseks andmesalve, mis võivad kaduda igal ajal. Ärge hoidke väärtuslik andmete vahemälu ainult; Veenduge, et haldate ka algse andmesalve teave. See tähendab, et kui vahemälu pole saadaval, saate märgiks andmete kaotsimineku.

### <a name="cache-highly-dynamic-data"></a>Väga dünaamiline andmete vahemälu

Saate talletada teavet kiiresti muuta püsivate andmete poes, saate seda esitada mõne pea kohal süsteemi. Näiteks kaaluge pidevalt aruannete oleku või mõne muu mõõtühikute seadet. Kui rakendus ei soovi vahemälu andmete põhjal, et vahemälus talletatud teabe peaaegu alati on aegunud, siis samadel võiks true, kui säilitamine ja allalaadimine seda teavet andmete poest. Aja jooksul kulub salvestamine ja selle andmete toomiseks, võib see on muutunud.

Sellisel juhul nagu see, kaaluge dünaamiline teabe talletamine otse vahemälu asemel püsivate andmesalve eelised. Kui andmed ei ole kriitiline ja ei nõua auditeerimine, siis pole oluline, kas aeg-ajalt muutmine läheb kaotsi.

### <a name="manage-data-expiration-in-a-cache"></a>Andmete aegumise vahemälus haldamine

Enamikul juhtudel toimub vahemälu andmed on andmeid, mis toimub algse andmesalve koopia. Algse andmesalve andmeid võib muutuda, kui see on vahemälus talletatud, mis põhjustavad vahemällu talletatud andmed muudetakse aegunud. Paljud vahemällu süsteemid võimaldavad teil konfigureerida vahemälu andmete aegumise ja vähendamiseks periood, mille andmeid võib olla aegunud.

Vahemällu talletatud andmetega aegumisel selle eemaldatakse vahemälu ja rakendus peab algse andmesalve (selle saab panna äsja tõmmatud teabe tagasi vahemälu) andmete toomiseks. Saate seada vaikimisi aegumise poliitika vahemälu konfigureerimisel. Mitme vahemälu teenused, saate samuti näha aegumise perioodi kirjutatava, kui salvestate need programmiliselt vahemälu.
Mõned vahemälu võimaldavad teil määrata kehtivuse aegumise absoluutväärtus või libistades väärtus, mis põhjustab üksuse eemaldatakse vahemälu, kui ei ole kättesaadav määratud aja jooksul. See säte alistab kõik vahemälu kogu aegumise poliitika, kuid ainult määratud objektide jaoks.

> [AZURE.NOTE] Kaaluge võimalust aegumise perioodi vahemälu ja objekte, mis sisaldab hoolikalt. Kui teete liiga lühike, objekte ei aeguks liiga kiiresti ja siis vähendada vahemälu kasutamise eelised. Kui teete perioodi liiga pikk, saate riskige muutumas kehtetud andmed.

Samuti on võimalik, et vahemälu võib täitke üles, kui andmed on lubatud jäävad elavate pikka aega. Sel juhul taotlused uute üksuste lisamiseks vahemälu võib põhjustada mõne üksuse tuntud väljatõstmine protsessi sunniviisiliselt eemaldada. Vahemälu teenuste tavaliselt tõstma vähemalt viimati kasutatud (LRU) alusel, kuid saate selle poliitika alistada ja üksuste takistada on eemaldada. Juhul, kui seda moodust vastu võtta teie riskige ületades mälu, mis on saadaval vahemälu. Rakendus, mis avaldab üksuse lisamiseks vahemälu ei õnnestu erandi.

Mõned vahemällu rakendusi võib esitada täiendavad väljatõstmine poliitika. On mitut tüüpi väljatõstmine poliitika. Järgmised:
- Enamik viimati kasutatud poliitika (jaotises eeldusel, et andmed ei ole vaja uuesti).
- Esimene-sisse-esimese välja poliitika (vanimast andmed on esmalt eemaldada).
- Konkreetsete eemaldamine poliitika käivitatud juhul (nt andmed, mida on muudetud).

### <a name="invalidate-data-in-a-client-side-cache"></a>Kliendipoolne vahemälu andmete seadmele

Andmed, mis toimub kliendipoolne vahemälu üldiselt peetakse väljaspool teenus, mis pakub andmete kliendi juures. Teenus ei saa otse lisada või eemaldada kliendipoolne vahemälu teave kliendi jõustada.

See tähendab, et on võimalik klient, mis kasutab halvasti konfigureeritud vahemälu edasi kasutada aegunud teavet. Näiteks kui aegumise poliitika vahemälu pole õigesti juurutatud, klient võib kasutada aegunud teavet, mis on vahemälus talletatud kohalik, kui algse andmeallika teave on muutunud.

Kui olete hoone veebirakendus, mis pakub andmete HTTP-ühenduse kaudu, saate jõustada teabe toomiseks peidetult web kliendi (nt brauseris või veebiteenuse puhverserveri). Saate seda teha, kui ressursi värskendatakse selle ressursi URI muutus. Veebikliendid tavaliselt kasutada ressursi URI kliendipoolne vahemälu klahvi, nii, et kui URI muutub, web klient ignoreerib mõni varem vahemällu talletatud versioonide ressursi ja uus versioon toob selle asemel.

## <a name="managing-concurrency-in-a-cache"></a>Vahemälus kokkulangevus haldamine

Vahemälu eesmärk on sageli ühiselt mitu rakenduse eksemplari. Iga rakenduse eksemplari võite lugeda ja muuta andmete vahemälu. Seega sama kokkulangevus probleeme tekkida mis tahes ühiskasutusega andmesalve rakendada ka vahemälu. Olukorras, kus nõuab andmeid, mis toimub vahemälu muuta, peate võib tagada tehtud ühe eksemplari rakenduse kirjutate üle teise eksemplari tehtud muudatused.

Sõltuvalt andmete laad ja kokkupõrkeid tõenäosuse, saate vastu ühte kokkulangevus kaks lähenemisviisi:

- __Loodetav.__ Vahetult enne andmete uuendamine rakendus kontrollib, kas vahemälu andmed on muudetud, kuna see toodud. Kui andmed on endiselt sama, saate selle muuta. Muul juhul taotlus on otsustada, kas soovite seda uuendada. (Äriloogika, mis juhib seda otsust saab rakenduse kohased.) Seda moodust sobib olukordades, kus värskendused on harv või kus kokkupõrkeid on tõenäoline.
- __Kardetav.__ Kui toob andmed, rakenduse lukustab vahemälu muus eksemplaris takistada selle muutmine. Selle protsessi tagab kokkupõrkeid ei saa tekkida, et nad blokeerib ka muud eksemplarid, mis on vaja protsessi samad andmed. Kardetav kokkulangevus võib mõjutada skaleeritavus lahenduse ja on soovitatav ainult lühiajaline toimingud. Seda moodust võib olla asjakohane olukordades, kus on kokkupõrgete tõenäosus, eriti siis, kui rakenduse värskendused mitu üksust vahemälu ning tagama, et need muudatused rakendatakse süsteemne.

### <a name="implement-high-availability-and-scalability-and-improve-performance"></a>Rakendada kõrge-saadavus ja skaleeritavus ja jõudluse parandamiseks

Vältige vahemälu nimega esmane hoidla andmeid; See on algse andmesalve, millest vahemälu on täidetud. Algne andmesalve vastutab püsimine andmete eest.

Olla ettevaatlik, et lisada oma lahenduste kriitilised sõltuvuse ühiskasutuses vahemälu teenuse kättesaadavus. Rakendus peaks olema jätkata toimimise kui ühiskasutuses vahemälu teenuse pole saadaval. Rakendus peaks hangub või ooteajal vahemälu teenuse jätkamiseks nurjuda.

Seetõttu rakendus peab olema valmis vahemälu teenuse olemasolu tuvastamine ja langeda tagasi algse andmesalve kui vahemälu ei pääse juurde. [Lüliti mustri](http://msdn.microsoft.com/library/dn589784.aspx) on kasulik töötlemise seda stsenaariumi. Vahemälu teenuse taastada, ja kui see on saadaval, vahemälu saab uuesti asustada, kui andmed on kirjutuskaitstud vormi algse andmesalve, nt [vahemälu-kõrvale mustri](http://msdn.microsoft.com/library/dn589799.aspx)strateegia pärast.

Siiski võib olla skaleeritavus mõju süsteemi kui rakenduse langeb tagasi algse andmesalve vahemälu pole ajutiselt saadaval.
Andmesalve taastatakse algne andmesalve võib Hukkua taotlusi, tulemuseks ajalõpud ja ühendused nurjus.

Kaaluge privaatne, kohaliku vahemälu rakendamisel iga eksemplari taotluse koos ühiskasutuses vahemälu, et kõik teenuserakenduse eksemplaride juurdepääs. Kui rakendus toob üksuse, seda vaadata esmalt oma kohaliku vahemälu ja seejärel klõpsake ühiskasutuse vahemälu ja lõpuks algsed andmed salvestada. Kohaliku vahemälu saate täidetud abil andmete ühiskasutuses vahemälu või andmebaasi kui ühiskasutuses vahemälu pole saadaval.

Seda moodust vajab ettevaatlik konfiguratsiooni vältida kohaliku vahemälu liigset liiga aegunud ühiskasutuses vahemälu suhtes. Siiski kohaliku vahemälu toimib puhvrit kui ühiskasutuses vahemälu on kättesaamatu. Joonis 3 näitab selle struktuuri.

![Privaatne, kohaliku vahemälu abil ühiskasutuses vahemälu](media/best-practices-caching/Caching3.png)
_joonis 3: privaatne, kohaliku vahemälu abil ühiskasutuses vahemälu_

Suure vahemälu suhteliselt vanade andmete reguleerivad toetamiseks pakuvad mõned vahemälu teenused kõrge-saadavus suvand, mis rakendab automaatselt Tõrkesiirde kui vahemälu kättesaamatu. Seda moodust tähendab tavaliselt imitatsiooniga vahemällu talletatud andmetega, mis on talletatud teisene vahemälu serveris serveris esmane vahemälu ja üle teise serverisse esmane server nurjumisel või ühenduvuse läheb kaotsi.

Latentsus, mis on seotud mitu sihtkohta kirjutamise vähendamiseks kopeerimine teise serverisse võivad ilmneda asünkroonselt kui andmed on kirjutatud vahemälu esmane serveris. Seda moodust viib võimalus, et mõned vahemälus talletatud teave kaotsi tõrke korral, kuid andmed osa peab olema väike võrreldes vahemälu üldmahu.

Kui ühiskasutuses vahemälu on väga mahukas, võib olla kasulik partition vahemällu talletatud andmetega üle sõlmed vähendada väide võimalusi ja parandada skaleeritavus. Mitme ühiskasutuses vahemälu tugiteenuste võimalus dünaamiliselt lisada (ja eemaldada) sõlmed ja taastub andmed üle sektsioonid. Seda moodust võib hõlmata rühmitamise, kus saidikogumi sõlmed on esitatud klientrakendustele sujuvalt, ühe vahemälu. Sees, aga andmed on hajutatud eelmääratletud jaotuse strateegia, mis nõuded laadi ühtlaselt sõlme vahel. [Andmete eraldamine juhise](http://msdn.microsoft.com/library/dn589795.aspx) Microsofti veebisaidil annab Lisateavet võimalike eraldamine strateegiad.

Rühmitamise saate suurendada vahemälu olemasolu. Sõlme nurjumisel vahemälu jääk on endiselt kättesaadav.
Rühmitamise kasutatakse sageli koos kopeerimise ja Tõrkesiirde. Iga sõlme saab korrata ja koopia saate kiiresti esitada online sõlme nurjumisel.

Paljud lugemine ja kirjutamine toimingud on kaasa ühe andmeväärtusi või objektid. Siiski aeg-ajalt oleks vaja salvestada või tuua suuri andmemahtusid kiiresti.
Näiteks külv vahemälu võivad hõlmata kirjutamine sadu või üksuste tuhandeliste vahemällu. Rakenduse tekkida vajadus tuua suure hulga seotud üksuste vahemälu sama taotluse osana.

Mitme suuremahuliste vahemälu paku nende toimingud. See võimaldab klientrakendusega üles suurel hulgal üksuste importimine ühe päringu paketi ja vähendab pea kohal, läbimiseks small taotlusi suure hulga seotud.

## <a name="caching-and-eventual-consistency"></a>Vahemälu ja lõpuks järjepidevus

Vahemälu-kõrvale muster töötamiseks, rakendus, mis kuvab vahemälu eksemplari peab olema juurdepääs tehtud ja terviklik versiooni andmed. Süsteemis, mis rakendab lõpliku järjepidevuse (nt kopeeritud andmed salvestada) see ei pruugi olla täheregistrit.

Ühe eksemplari rakendus võib muuta andmete üksuse ning seadmele selle üksuse vahemällu talletatud versiooni. Teise eksemplari rakendus võib proovivad lugeda selle üksuse vahemälu, mis põhjustab nii, et see loeb andmeid andmesalve ja lisab selle vahemälu vahemälu vahele. Kui andmesalve pole muude koopiad täielikult sünkroonitud, rakenduse võib lugeda ja asustada vahemälu ja vana väärtuse.

Andmete järjepidevuse töötlemise kohta leiate lisateavet teemast [andmete järjepidevuse primer](http://msdn.microsoft.com/library/dn589800.aspx) lehe Microsofti veebisaidil.

### <a name="protect-cached-data"></a>Kaitse vahemällu talletatud andmetega

Sõltumata vahemälu teenuse kasutate, võtke arvesse, kuidas kaitsta andmeid hoitakse vahemälu volitamata juurdepääsu. On kaks peamist probleemid.

- Vahemälu andmete privaatsust
- Kuna andmete privaatsust liigub sujuvalt vahemälu ja rakendus, mis kasutab vahemälu vahel

Andmete vahemälu kaitsmiseks võib vahemälu teenuse rakendada autentimise süsteem, mis nõuab, et rakenduste Määrake järgmine:
- Millised identiteedid pääsevad andmete vahemälu.
- Milliseid toiminguid (lugemine ja kirjutamine), mis neid identiteedid lubatud.

Vähendamiseks kohal, mis on seotud lugemine ja kirjutamine andmete pärast identiteedi andmist kirjutamine ja/või vahemälu, et identiteedi saate kasutada mis tahes andmete vahemälu lugemisõigus.

Kui soovite piirata juurdepääsu kohta vahemällu talletatud andmetega, saate teha ühte järgmistest.

- Vahemälu jagatud sektsioonid (abil muu vahemälu serverid) ja ainult juurdepääsu identiteetidega sektsioonid, et nad peaksid saama kasutada.
- Iga alamhulk andmete erinevate klahvide abil krüptida ja sisestage identiteetidega, mis peaks olema juurdepääs iga alamhulk krüptimise võtmed. Kliendi rakendus võib olla võimalik kõik vahemälus andmete toomiseks, kuid seda saab ainult andmeid, mis on toodud klahvid dekrüptida.

Andmed tuleb kaitsta ka nagu vahemälu ja sealt juhtimine. Selleks saate sõltuvad turbefunktsioonid võrgu taristule klientrakendustes ühenduse vahemälu kasutavate järgi. Kui vahemälu on rakendatud sama ettevõttes kohapeal serveri kasutamine majutatakse kliendi rakendused, siis võrk ise eraldamine ei nõuda täiendavaid samme. Kui vahemälu asub kaugühenduse teel ja nõuab TCP või HTTP ühendust avaliku võrgu (nt Internet), kaaluma SSL.

## <a name="considerations-for-implementing-caching-with-microsoft-azure"></a>Kaalutlused vahemällu Microsoft Azure rakendamiseks

Azure'i leiate Azure'i Redis vahemälu. See on avatud allika Redis vahemälu, mis töötab teenus on Azure andmekeskuses rakendamist. Kas rakendus on rakendatud pilveteenus veebisaidi või sees on Azure virtuaalse masina pakub vahemällu teenus, mida saab kasutada mis tahes Azure'i rakendusest. Vahemälu saab jagada on sobiv kiirklahv klientrakendustes.

Azure'i Redis vahemälu on suure jõudlusega vahemällu lahenduse, mis pakub kättesaadavus, skaleeritavus ja turvalisus. See töötab tavaliselt ühe või mitme sihtotstarbeline arvutites levinud teenus. See püüab säilitada nii palju teavet, nagu seda saab mälu, et tagada kiire juurdepääs. See arhitektuur eesmärk madal latentsus ja läbilaskevõime kõrge, tuleb teha aeglane/v-toimingud.

 Azure'i Redis vahemälu ühildub paljude erinevate API-d, mida kasutatakse klientrakendustes. Kui teil on juba kasutusel Azure'i Redis vahemälu töötab kohapealse olemasolevate rakenduste, Azure'i Redis vahemälu annab kiirülevaate migreerimise tee, vahemällu talletamine pilveteenuses.

> [AZURE.NOTE] Azure'i pakub hallatavate vahemälu teenust. See teenus on Azure teenuse struktuuri vahemälu engine alusel. See võimaldab teil luua jaotatud vahemälu, mida saab jagada lõdvalt ühendatud rakendused. Suure jõudlusega serverites töötab ka Azure andmekeskuse majutatakse vahemälu.
Kuid see suvand pole enam soovitatav ja ainult ette abi olemasolevaid rakendusi, mis on ehitatud seda kasutada. Kõigi uute arengu, kasutage Azure'i Redis vahemälu.
>
> Lisaks toetab Azure-rolli vahemällu. See funktsioon võimaldab teil luua vahemälu, mis on seotud pilveteenusesse.
Vahemälu hostib eksemplarid veebis või töötaja rolli ja pääseb juurde ainult rollid, mis töötavad sama pilve juurutamise üksuse osana. (Juurutamise üksus on rolli aknad, teatud alale pilveteenus juurutatud kogum.) Rühmitatud vahemälu ja kõik eksemplarid roll sama juurutamise üksuses, mis hostib vahemälu osaks sama vahemälu kobar. Kuid see suvand pole enam soovitatav ja ainult ette abi olemasolevaid rakendusi, mis on ehitatud seda kasutada. Kõigi uute arengu, kasutage Azure'i Redis vahemälu.
>
> Azure'i hallatavate vahemälu teenuse nii Azure-rolli vahemälu praegu slated aegumine 16 November 2016.
Soovitatav on migreerimiseks Azure'i Redis vahemällu selle aegumine ettevalmistamiseks. Lisateabe saamiseks külastage lehte   [mis on Azure Redis vahemälu pakkumise ja milliseid suurust tuleks kasutada?](redis-cache/cache-faq.md#what-redis-cache-offering-and-size-should-i-use) Microsofti veebisaidil.


### <a name="features-of-redis"></a>Redis funktsioonid

 Redis on rohkem kui lihtne vahemälu server. Jaotatud-mälu andmebaasi pakub olulisel käsud, mis toetab mitme levinud stsenaariumi. Need on kirjeldatud allpool selle dokumendi jaotist abil Redis vahemällu. Selles jaotises on kokku võetud olulisi funktsioone, mis pakub Redis.

### <a name="redis-as-an-in-memory-database"></a>Redis nimega-mälu andmebaasist loobumine

Redis toetab nii lugeda ja kirjutada. Redis, saate kaitstud kirjutab süsteemi tõrge kas perioodiliselt salvestatud faili kohaliku hetktõmmise või lisa ainult logifaili. See pole nii palju vahemälu (mis tuleks kaaluda ajutine andmete).

 Kõik kirjutab on asünkroonne ja blokeerida kliendid lugemise ja kirjutamise andmeid. Kui Redis käivitamise töötab, loeb failist hetktõmmise või Logi andmeid ja kasutab seda ehitada vahemälu. Lisateabe saamiseks vaadake [Redis püsimine](http://redis.io/topics/persistence) Redis veebisaidil.

> [AZURE.NOTE] Redis ei garanteeri katastroofiline tõrke korral salvestatakse kõik kirjutab, et paku võib ainult mõne sekundi andmete kaotsi minna. Pidage meeles, et vahemälu on mõeldud autoriteetsete andmeallikana toimivad ja vastutab rakenduste abil tagada kriitilised andmed on salvestatud mõnda sobivat andmesalve edukalt vahemälu. Lisateabe saamiseks lugege teemat [vahemälu-kõrvale muster](http://msdn.microsoft.com/library/dn589799.aspx).

#### <a name="redis-data-types"></a>Redis andmetüübid

Redis on võti-väärtus poe, kus väärtused võivad sisaldada või keerukas andmestruktuurid hashes, loendid, näiteks lihtsa failitüübid ja määrab. See toetab atomic toimingute kohta järgmist tüüpi andmeid. Klahvid saab püsivate või sildistatud, kellel on piiratud aja-to-live, misjärel võti ja selle vastavate väärtuste eemaldatakse automaatselt vahemälu. Redis võtmed ja väärtuste kohta leiate lisateavet lehelt [Sissejuhatus Redis andmetüüpide ja veevõtu](http://redis.io/topics/data-types-intro) Redis veebisaidil.

#### <a name="redis-replication-and-clustering"></a>Redis kopeerimise ja rühmitamise

Redis toetab juhtslaidi/alluva kopeerimise abil tagada kättesaadavus ja läbilaskevõime säilitada. Kirjutage Redis juhtslaidi sõlm toimingud ei kopeeritud ühe või mitme alluv sõlmed. Read toimingud serveeritakse juhteksemplari või mõni selle alluvate.

Korral võrgu sektsiooni, alluva saate edaspidi kasutada andmete ja seejärel läbipaistev sünkroonige uuesti koos juhteksemplari kui taastub ühendus. Lisateabe saamiseks külastage lehte [Dispersioonanalüüs](http://redis.io/topics/replication) Redis veebisaidil.

Redis pakub rühmitamise, mis võimaldab teil läbipaistev partition andmed üheks shards serverites ja levinud laadi. See funktsioon parandab skaleeritavus, kuna saab lisada uue Redis serverid ja suurendavad repartitioned suurus, vahemälu andmed.

Lisaks saab paljundada iga serveri klaster juhtslaidi/alluva kopeerimise abil. See tagab kättesaadavus kogu iga sõlme klaster. Rühmitamise ja sharding kohta lisateabe saamiseks külastage [Redis kobar kuueosalisest lehe](http://redis.io/topics/cluster-tutorial) Redis veebisaidi.

### <a name="redis-memory-use"></a>Redis mälu kasutamine

Redis vahemälu on piiratud suurus, mis sõltub hosti arvutis olemas ressursid. Kui konfigureerite Redis server, saate määrata mälu, saate seda kasutada suurima summa. Samuti saate konfigureerida klahvi Redis vahemälu, mis on aegumise aeg, mille järel automaatselt eemaldatakse see vahemälu. See funktsioon aitab vältida vahemälu täitmise ja vana või kehtetud andmed.

Kui mälu täitub, Redis saab automaatselt tõstma võtmed ja nende väärtuste poliitikast, järgides. Vaikimisi on LRU (vähemalt viimati kasutatud), kuid võite valida ka muu poliitika, nt evicting klahvid juhuslike või väljalülitamine väljatõstmine hoopis (sisse, juhul katsed lisada üksuste vahemällu nurjuda, kui see on täis). Lehe [Kaudu kui ka LRU vahemälu Redis](http://redis.io/topics/lru-cache) annab Lisateavet.

### <a name="redis-transactions-and-batches"></a>Redis tehingud ja töölehed

Redis võimaldab klientrakendusega esitada andmete lugemine ja kirjutamine vahemälu nimega atomic toimingu toimingute sarja. Tehingu kõik käsud on tagatud järjest käivitamiseks ja pole antud teiste samaaegseid klientide käsud on omavahel seotud nende vahel.

Aga need on tõene tehingud nagu relatsiooniandmebaasist peaksite nende toimingute. Tehingu töötlemise koosneb kahest etapist--esimene on kui käskude järjekorda ja teine on käsud käitamisel. Kliendi hinnata etapis käsk andmebaasitõrge käsud, mis moodustavad tehingu. Mingi tõrke ilmnemisel sel hetkel (nt süntaksi viga või vale parameetrite arv) siis Redis keeldub töötlemine kogu tehing ja selle hüljatakse.

Käivita faasis teostab Redis järjest iga ootel käsu. Käsu nurjumisel selles etapis Redis jätkub ootel järgmise käsu abil ja taastage mõju käsud, mis juba kasutanud. Tehingu selle lihtsustatud vormi aitab säilitada jõudlus ja vältida jõudlusprobleemide, mis põhjustavad argument.

Redis rakendada loodetav lukustamine, et aidata säilitada järjepidevuse vorm. Üksikasjalikku teavet tehingud ja lukustamine ja Redis, külastage [tehingud lehe](http://redis.io/topics/transactions) Redis veebisaidil.

Redis toetab samuti mittetoetavate partiide taotluste. Käskude saatmiseks Redis serveri kasutavate klientide Redis protokoll võimaldab kliendil toimingute sarja saata sama kutse osana. See aitab vähendada paketi killustatust võrgus. Kui paketi töödeldakse, sooritatakse iga käsu. Kui mõni järgmised käsud on vigane, nad saavad lükata (mis ei juhtu, kus tehingu), kuid ülejäänud käskude tehakse. Olemas on ka mingit garantiid paketi käsud töödeldakse tellimuse kohta.

### <a name="redis-security"></a>Redis turvalisus

Redis on suunatud puhtalt andmete kiire juurdepääsu ning on mõeldud töötama usaldusväärsete keskkonnas, millele pääsevad juurde ainult usaldusväärsete kliendid sees. Redis toetab piiratud turvalisus mudeli põhjal Paroolautentimine. (See on võimalik autentimise täielikult eemaldada, kuigi me ei soovita seda.)

Kõik autenditud klientidele ühiskasutusse anda globaalne sama parool ja juurdepääs sama ressursid. Kui teil on vaja põhjalikumat turvalisus, rakendate oma turvalisus kiht ette Redis server ja kõik klientide päringutele tuleks läbida see täiendavad kiht. Redis tuleks otse kokku puutuda ebausaldusväärsete või autentimata klienti.

Saate piirata juurdepääsu käsud keelata või neid ümbernimetamine (ja tagades ainult õigustega klientidele uute nimede).

Redis ei toeta otseselt andmete krüptimine igasuguse nii, et kõik kodeerimine tuleb teha klientrakendustes. Lisaks ei paku Redis igasuguse turvalisus. Kui vajate juhtimine üle võrgu andmete kaitsmiseks, soovitame rakendada mõne SSL-i puhverserveri.

Lisateabe saamiseks külastage lehte [Redis turvalisus](http://redis.io/topics/security) Redis veebisaidil.

> [AZURE.NOTE] Azure'i Redis vahemälu pakub oma turvalisus kiht, mille kaudu klientrakendustega. Aluseks oleva Redis serverid ei satuks avaliku võrgu.

### <a name="using-the-azure-redis-cache"></a>Azure'i Redis vahemälu kasutamine

Azure'i Redis vahemälu annab juurdepääsu Redis serverid serverites majutatud veebisaidil on Azure andmekeskuse; See toimib fassaadi, mis pakub juurdepääsu reguleerimine ja turvalisus. Azure haldusportaali abil saate ette valmistada vahemälu. Portaali pakub mitmesuguseid eelmääratletud konfiguratsioone, alates töötab sihtotstarbeline teenus, mis toetab (privaatsus) SSL-i suhtlus ja juhtslaidi/alluva kopeerimisest SLA 99,9%-saadavus, kuni 250 MB vahemälu korduseta dispersioonanalüüs (kättesaadavus garantiid) töötavate ühiskasutusega riistvara 53GB vahemälu.

Azure haldusportaali abil saate konfigureerida vahemälu väljatõstmine poliitika, ja vahemälu juurdepääsu reguleerimine, lisades kasutajate rollid, mis on esitatud; Omanikule, kaasautor, lugeja. Administraatorirollid määratleda toimingud, mida liikmed saavad täita. Näiteks omanik rolli liikmed on vahemälu (sh turvalisus) ja selle sisu üle täielik kontroll, liikmete kaasautori roll saate lugeda ja kirjutada teabe vahemälu ja lugeja rolli liikmed saate tuua ainult andmeid vahemälu.

Enamik haldustoiminguid teostamise kaudu Azure'i haldusportaal ja paljud haldus käsud saadaval standardversioonis Redis on pole saadaval, sh muuta konfiguratsiooni programmiliselt, seetõttu sulgumist Redis server, konfigureerida täiendavaid orjad või sunniviisiliselt Salvesta andmed kettale.

Azure'i haldusportaal sisaldab mugav graafiline, mis võimaldab teil jälgida vahemälu jõudlust. Näiteks saate vaadata tehtud ühenduste arv taotlusi läbi, loeb ja kirjutab, maht ja arv vahemälu tabab võrreldes vahemälu jätab. Kasutades seda teavet, saate määrata tõhusust vahemälu ja kui vajadusel muuta väljatõstmine poliitika või mõne muu konfiguratsiooni üle minna. Lisaks saate luua teatisi, mis saata meilisõnumeid, kui üks või mitu kriitilised mõõdikute väljaspool vahemikku on administraator. Näiteks kui vahemälu jätab arv ületab määratud väärtuse, viimase tunni, administraator võib teatisi, kui vahemälu võib olla liiga väike või andmete võib olla eemaldada liiga kiiresti.

Samuti saate jälgida CPU, mälu ja võrgu kasutamine vahemälu.

Lisateave ja näiteid, mis näitab, kuidas luua ja konfigureerida Azure Redis vahemälu, külastage lehte [ringi ümber Azure'i Redis vahemälu](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) Azure ajaveebist.

## <a name="caching-session-state-and-html-output"></a>Vahemällu seansi olek ja HTML väljund

Kui te koostamise ASP.net-i veebirakendusi, et käivitada, kasutades Azure web rollid, saate salvestada seansi olekuteabe ja HTML-i väljundi Azure'i Redis vahemälu. Seansi oleku pakkuja Azure'i Redis vahemälu võimaldab teil vahel eri ASP.net-i web rakenduse eksemplari seansi teabe jagamiseks ja on väga kasulik web serveripargi olukordades, kus kliendi serveri osaleja pole saadaval ja vahemällu seansi andmete-mälu ei saa.

Kasutades seansi oleku pakkuja Azure'i Redis vahemälu pakub mitmeid eeliseid, sh:

- Selle saate ühiskasutusse anda seansi olek seas suure hulga eksemplarid ASP.net-i veebirakenduses, pakkudes täiustatud skaleeritavus.
- Sama seansi oleku andmetele juurdepääsu kontrollitud, samaaegne toetab mitut lugejad ja ühe kirjutaja, ja
- Selle abil saate tihendamise salvestamine mälu ja võrgu jõudlust parandada.

Lisateabe saamiseks külastage Microsofti veebisaidil [Azure'i Redis vahemälu ASP.net-i seansi olek pakkuja](redis-cache/cache-aspnet-session-state-provider.md) lehel.

> [AZURE.NOTE] Kasutage seansi oleku pakkuja Azure'i Redis vahemälu ASP.net-i rakendusi, mis töötavad väljaspool Azure keskkonnas. Latentsus juurdepääs väljaspool Azure'i vahemälu saate eemaldada jõudluse kasu andmete vahemällu.

Samuti väljundi vahemälu pakkuja Azure'i Redis vahemälu võimaldab salvestada loodud ASP.net-i web rakenduse HTTP vastuseid. Azure'i Redis vahemälu väljundi vahemälu pakkuja abil saate parandada vastuse ajad rakendusi, mis muudavad keerukate HTML väljund; Teenuserakenduse eksemplaride loomisel sarnaseid vastuseid saate teha kasutamine ühiskasutusega väljundi fragmendid vahemälu asemel selle HTML-i väljund uuesti genereerimine.  Lisateabe saamiseks külastage Microsofti veebisaidil [ASP.net-i väljundi vahemälu pakkuja Azure'i Redis vahemälu](redis-cache/cache-aspnet-output-cache-provider.md) lehe.

### <a name="azure-redis-cache"></a>Azure'i Redis vahemälu

Azure'i Redis vahemälu võimaldab juurdepääsu Redis serverid, mis on majutatud veebisaidil on Azure andmekeskuse. See toimib fassaadi, mis pakub juurdepääsu reguleerimine ja turvalisus. Azure portaali abil saate ette valmistada vahemälu.

Portaali pakub mitmesuguseid eelmääratletud konfiguratsioone. Need ulatuvad 53 GB vahemälu töötab sihtotstarbeline teenus, mis toetab (privaatsus) SSL-i suhtlus ja juhtslaidi/alluva kopeerimisest SLA 99,9%-saadavus, 25 0 MB vahemälu korduseta dispersioonanalüüs (kättesaadavus garantiid) töötavate ühiskasutusega riistvara allapoole.

Azure portaali kasutamisel saate ka konfigureerida väljatõstmine poliitika vahemälu ja vahemälu juurdepääsu reguleerimine, lisades kasutajate rollid, mis on esitatud.  Need rollid, mis määratlevad toimingud, mida saab teha liikmed, kaasake omanik, kaasautor ja lugeja. Näiteks omanik rolli liikmed on vahemälu (sh turvalisus) ja selle sisu üle täielik kontroll, liikmete kaasautori roll saate lugeda ja kirjutada teabe vahemälu ja lugeja rolli liikmed saate tuua ainult andmeid vahemälu.

Enamik haldustoiminguid teostamise Azure portaali kaudu. Seetõttu paljud haldus käsud, mis on saadaval standardversioonis Redis ei ole saadaval, sh võimalus muuta konfiguratsiooni programmiliselt, Redis serveri sulgeda, konfigureerida täiendavaid alluva või sunniviisiliselt andmete kettale salvestamine.

Azure'i portaalist leiate mugav graafiline, mis võimaldab teil jälgida vahemälu jõudlust. Näiteks saate vaadata tehtud, läbi taotlusi, loeb ja kirjutab, maht ühenduste arv ja Vahemälutabamusi võrreldes vahemälu jätab arv. Selle teabe abil saate määrata tõhusust vahemälu ja vajadusel vahetada mõne muu konfiguratsiooni või muuta väljatõstmine poliitika.

Lisaks saate luua teatisi, mis saata meilisõnumeid, kui üks või mitu kriitilised mõõdikute väljaspool vahemikku on administraator. Näiteks võite soovida Teavita administraator kui vahemälu jätab arv ületab määratud väärtuse, viimase tunni, kuna see tähendab vahemälu võib olla liiga väike või andmete võib olla eemaldada liiga kiiresti.

Samuti saate jälgida CPU, mälu ja võrgu kasutamine vahemälu.

Lisateave ja näiteid, mis näitab, kuidas luua ja konfigureerida Azure Redis vahemälu, külastage lehte [ringi ümber Azure'i Redis vahemälu](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) Azure ajaveebist.

## <a name="caching-session-state-and-html-output"></a>Vahemällu seansi olek ja HTML väljund

Kui olete hoone ASP.net-i veebirakenduste, et käivitada, kasutades Azure web rollid, saate salvestada seansi maakond HTML-i väljundi Azure'i Redis vahemälu ja teave. Seansi oleku pakkuja Azure'i Redis vahemälu võimaldab teil vahel eri ASP.net-i web rakenduse eksemplari seansi teabe jagamiseks ja on väga kasulik web serveripargi olukordades, kus kliendi serveri osaleja pole saadaval ja vahemällu seansi andmete-mälu ei saa.

Kasutades seansi oleku pakkuja Azure'i Redis vahemälu pakub mitmeid eeliseid, sh:

- Suure hulga ASP.net-i veebirakenduste eksemplari ühiskasutusse andmise seansi olek.
- Täiustatud skaleeritavus esitada.
- Kontrollitud, samaaegne sama seansi oleku andmetele juurdepääsu toetamiseks mitme lugejad ja ühe kirjutaja.
- Tihendamise abil salvestada mälu ja võrgu jõudlust parandada.

Lisateabe saamiseks külastage Microsofti veebisaidil [ASP.net-i seansi oleku pakkuja Azure'i Redis vahemälu](redis-cache/cache-aspnet-session-state-provider.md) lehte.

> [AZURE.NOTE] Kasutage seansi oleku pakkuja Azure'i Redis vahemälu ASP.net-i rakendustega, mida käitatakse väljaspool Azure keskkonnas. Latentsus juurdepääs väljaspool Azure'i vahemälu saate eemaldada jõudluse kasu andmete vahemällu.

Samuti väljundi vahemälu pakkuja Azure'i Redis vahemälu võimaldab salvestada loodud ASP.net-i web rakenduse HTTP vastuseid. Azure'i Redis vahemälu väljundi vahemälu pakkuja abil saate parandada rakendusi, mis muudavad keerukate HTML-i väljundi vastuse ajad. Teenuserakenduse eksemplaride andvate sarnaseid vastuseid saate teha ühiskasutusse antud väljundi fragmendid vahemälu asemel selle HTML-i väljund uuesti genereerimine kasutamine. Lisateabe saamiseks külastage Microsofti veebisaidil [ASP.net-i väljundi vahemälu pakkuja Azure'i Redis vahemälu](redis-cache/cache-aspnet-output-cache-provider.md) lehte.

## <a name="building-a-custom-redis-cache"></a>Kohandatud Redis vahemälu ehitamine

Azure'i Redis vahemälu toimib fassaadi aluseks Redis serverid. Praegu toetab konfiguratsioone fikseeritud kogum, kuid ei paku Redis rühmitamiseks. Kui vajate täiustatud konfiguratsiooni, mis ei hõlma Azure'i Redis vahemälu (nt vahemälu on suurem kui 53 GB) saate koostada ja majutada oma Redis serverid Azure'i virtuaalmasinates abil.

See on potentsiaalselt keerukas protsess, kuna teil võib olla vaja luua mitme juhtslaidi ja alluv sõlmed toimida, kui soovite rakendada dispersioonanalüüs VMs. Lisaks, kui soovite luua klaster, peate mitme juhtslaidi ja alluv serverid. Minimaalne rühmitatud dispersioonanalüüs topoloogia, mis annab kõrge kättesaadavus ja skaleeritavus sisaldab vähemalt kuus VMs korraldatud kolme paari juhtslaidi/alluva servers (klaster peab sisaldama vähemalt kolme juhtslaidi sõlmed).

Juhtslaidi/alluva iga paari asuda lähedale latentsus minimeerimiseks. Iga paari kogumi saate töötab muus Azure andmekeskuste asub regioonid, kui soovite leida vahemällu talletatud andmetega lähedale rakendusi, mis on kõige tõenäolisemad seda kasutada?. Lehe [Töötab Redis CentOS Linux VM Azure](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) Microsofti veebisaidil tutvustatakse näide, mis näitab, kuidas luua ja konfigureerida Redis sõlm töötab ka Azure VM.

[AZURE.NOTE] Pange tähele, et kui otsustate oma Redis vahemälu sel viisil, olete jälgida, hallata ja turvaliseks teenus.

## <a name="partitioning-a-redis-cache"></a>Eraldamine Redis vahemälu

Vahemälu eraldamine hõlmab vahemälu tükeldamine mitmesse arvutisse. See struktuur annab mitmeid eeliseid ühe vahemälu serveri, sh kasutamine.

- Luua vahemälu, mis on palju suuremad kui saate ühe serveris talletatud.
- Andmete levitamise serverites, kättesaadavuse parandamiseks. Kui üks server ei või muutub kättesaamatuks, pääseb siiski andmed, mida tal pole saadaval, kuid andmed ülejäänud serverites. Jaoks vahemälu, see pole oluline, kuna vahemällu talletatud andmed on ajutine koopia andmeid hoitakse andmebaasi. Vahemällu talletatud andmetega serveris, mis muutub kättesaamatuks saab kopeerida hoopis mõnes muus serveris.
- Laadi levitada serverites, parandades jõudlus ja skaleeritavus.
- Kasutajatele, mille juurde pääsevad, vähendades latentsus sulgege geolokatsiooniline andmed.

Vahemälu, on kõige levinum eraldamine sharding. See strateegia iga sektsiooni (või Kildu) on Redis vahemälu omaette. Andmete suunatakse kindlasse sektsiooni sharding loogika, mida saate kasutada mitmesuguseid lähenemisel levitamine andmete abil. Lisateabe saamiseks rakendamise sharding pakub [Sharding mustri](http://msdn.microsoft.com/library/dn589797.aspx) .

Rakendada eraldamine Redis vahemälu, tehke ühte järgmistest võimalustest:

- _Serveripoolne päringu marsruutimine._ Selle tehnika kliendi rakendus saadab mis tahes Redis serverid, mis sisaldavad vahemälu (ilmselt kõige lähemal server). Metaandmete partition, et see on ja sisaldab ka teavet selle kohta, mis asuvad sektsioonid teiste serverite kirjeldav salvestab iga Redis server. Redis server uurib kliendi taotlus. Kui see võib lahendada kohalikult, täidab see toiming. Muul juhul edastab selle taotluse vastav serverisse sisse logida. See mudel rakendatakse Redis rühmitamise ja on üksikasjalikult kirjeldatud Redis veebisaidi lehel [Redis kobar õpetuse](http://redis.io/topics/cluster-tutorial) . Redis rühmitamise on läbipaistev klientrakenduste ja täiendavad Redis serverid saab lisada klaster (ja uuesti Liigendatud andmete) nõudmata kliendid ümber konfigureerida.

- _Kliendipoolne eraldamine._ See mudel sisaldab klientrakendust loogika (võimalik, et vormi teeki), mis marsruudib taotlusi sobivasse sisselogimisserverisse Redis. Seda moodust saab kasutada Azure Redis vahemälu. Mitme Azure'i Redis vahemälu (üks iga andmete partition) loomine ja rakendada kliendipoolse loogika, mis marsruudib taotlused õige vahemällu. Kui eraldamine kava muutub (kui täiendavad Azure'i Redis vahemälu on loodud), peate klientrakendustes ümber konfigureerida.

- _Puhverserveri abil eraldamine._ Selle kava, Redis kliendi rakenduste saada taotleb vahendaja puhverserveri teenusega, mis mõistab, kuidas on liigendatud andmete ja seejärel sobiv marsruudib taotluse server. Seda moodust saab kasutada ka Azure Redis vahemälu; puhverserveri teenuse saate rakendada mõne Azure pilveteenuses. Seda moodust nõuab on täiendavad tase keerukuse rakendada teenuse ja taotlusi võib olla pikem kui kliendipoolne eraldamine abil teha.

Lehe [Eraldatav: kuidas jagada andmete hulgas Redis mitmes eksemplaris](http://redis.io/topics/partitioning) klõpsake soovitud Redis veebisaidilt leiate veel teavet rakendamise abil Redis eraldamine.

### <a name="implement-redis-cache-client-applications"></a>Rakendada Redis vahemälu klientrakendused

Redis kirjutatud mitmeid programmeerimiskeele toetab klientrakendustes. Kui on uue rakenduste loomine, kasutades .NET Frameworki, on soovitatav kasutada StackExchange.Redis kliendi teek. See teek pakub .NET Framework objektimudel see kokkuvõtteid Redis serveriga, käskude saatmine ja vastuvõtmine vastuste üksikasjad. See on saadaval Visual Studio Nugeti paketina. Saate selle sama teegi ühenduse Azure'i Redis vahemälu või kohandatud Redis vahemälu majutatud VM.

Redis serveriga ühenduse loomiseks saate kasutada staatiline `Connect` meetod on `ConnectionMultiplexer` klassi. See meetod loob ühenduse on mõeldud kasutamiseks klientrakenduse eluea ja sama ühendust saab kasutada mitut samaaegne teemad. Ühendage ja katkestada iga kord, kui Redis toimingut sooritada, kuna see võib jõudlust halvendada.

Saate määrata parameetreid, nt aadress Redis host ja parool. Kui kasutate Azure Redis vahemälu, parool on kas põhi- või klahvi, mis luuakse Azure'i Redis vahemälu Azure haldusportaali abil.

Pärast Redis server on ühendatud, saate hankida Redis andmebaasi, mis toimib vahemälu pidet. Redis ühendus pakub soovitud `GetDatabase` meetodi abil tehke järgmist. Saate tuua üksuste vahemälu ja vahemälu andmete talletamiseks, kasutades funktsiooni `StringGet` ja `StringSet` viise. Nende meetodite oodata parameetrina klahvi ja tagastada üksus, kas vahemälu, mis sisaldab vastav väärtus (`StringGet`) või üksuse lisamine vahemälu see võti (`StringSet`).

Sõltuvalt asukohast Redis server, palju tegevust võib tekkida mõned latentsuse ajal taotluse edastatakse server ja vastuse tagastatakse kliendile. Teegi StackExchange pakub asünkroonne versioonid meetodid, mis see seab aitab jäävad reageeri klientrakendustes paljude. .NET Framework tugi [Toimingupõhise asünkroonne mustri](http://msdn.microsoft.com/library/hh873175.aspx) järgmised võimalused.

Kuvatakse järgmised koodilõigu nimega meetodi `RetrieveItem`. See näitab vahemälu-kõrvale mustri Redis ja StackExchange teegi rakendamist. Meetod võtab stringi võtmeväärtuse ja vastavale üksusele toomiseks Redis vahemälu helistades avaldab selle `StringGetAsync` meetod (asünkroonne versiooni `StringGet`).

Kui üksust ei leita, võetakse aluseks olevate andmete allikas kasutada funktsiooni `GetItemFromDataSourceAsync` meetod (mis on kohalik meetod ja StackExchange teegi osa). See lisatakse vahemälu abil soovitud `StringSetAsync` nii, et see saab tuua kiiremini järgmine kord, kui meetod.

```csharp
// Connect to the Azure Redis cache
ConfigurationOptions config = new ConfigurationOptions();
config.EndPoints.Add("<your DNS name>.redis.cache.windows.net");
config.Password = "<Redis cache key from management portal>";
ConnectionMultiplexer redisHostConnection = ConnectionMultiplexer.Connect(config);
IDatabase cache = redisHostConnection.GetDatabase();
...
private async Task<string> RetrieveItem(string itemKey)
{
    // Attempt to retrieve the item from the Redis cache
    string itemValue = await cache.StringGetAsync(itemKey);

    // If the value returned is null, the item was not found in the cache
    // So retrieve the item from the data source and add it to the cache
    if (itemValue == null)
    {
        itemValue = await GetItemFromDataSourceAsync(itemKey);
        await cache.StringSetAsync(itemKey, itemValue);
    }

    // Return the item
    return itemValue;
}
```

Funktsiooni `StringGet` ja `StringSet` meetodid ei ole piiratud toomine või talletamise stringi väärtuse. Nad saavad võtta mis tahes üksust, mis on seeriasertide massiivina baiti. Kui vajate .NET objekti salvestamiseks, saate selle nimega bait voo serialiseerida ja kasutada funktsiooni `StringSet` meetod seda kirjutada vahemällu.

Samuti saate lugeda objekti vahemälu, kasutades funktsiooni `StringGet` meetod ja deserializing selle .NET objektina. Järgmine kood kuvatakse kogumi laiend meetodid IDatabase kasutajaliidese (selle `GetDatabase` meetod Redis ühenduse tagastab mõne `IDatabase` objekti), ja mõned proovi kood, mida kasutab nende meetodite lugemine ja kirjutamine on `BlogPost` objekti vahemälu:

```csharp
public static class RedisCacheExtensions
{
    public static async Task<T> GetAsync<T>(this IDatabase cache, string key)
    {
        return Deserialize<T>(await cache.StringGetAsync(key));
    }

    public static async Task<object> GetAsync(this IDatabase cache, string key)
    {
        return Deserialize<object>(await cache.StringGetAsync(key));
    }

    public static async Task SetAsync(this IDatabase cache, string key, object value)
    {
        await cache.StringSetAsync(key, Serialize(value));
    }

    static byte[] Serialize(object o)
    {
        byte[] objectDataAsStream = null;

        if (o != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream())
            {
                binaryFormatter.Serialize(memoryStream, o);
                objectDataAsStream = memoryStream.ToArray();
            }
        }

        return objectDataAsStream;
    }

    static T Deserialize<T>(byte[] stream)
    {
        T result = default(T);

        if (stream != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream(stream))
            {
                result = (T)binaryFormatter.Deserialize(memoryStream);
            }
        }

        return result;
    }
}
```

Järgmine kood illustreerib meetodi nimega `RetrieveBlogPost` , mis kasutab viiside laiend lugemine ja kirjutamine on sarjadesse jaotatav `BlogPost` objekti vahemälu vahemälu-kõrvale piirangutega:

```csharp
// The BlogPost type
[Serializable]
private class BlogPost
{
    private HashSet<string> tags = new HashSet<string>();

    public BlogPost(int id, string title, int score, IEnumerable<string> tags)
    {
        this.Id = id;
        this.Title = title;
        this.Score = score;
        this.tags = new HashSet<string>(tags);
    }

    public int Id { get; set; }
    public string Title { get; set; }
    public int Score { get; set; }
    public ICollection<string> Tags { get { return this.tags; } }
}
...
private async Task<BlogPost> RetrieveBlogPost(string blogPostKey)
{
    BlogPost blogPost = await cache.GetAsync<BlogPost>(blogPostKey);
    if (blogPost == null)
    {
        blogPost = await GetBlogPostFromDataSourceAsync(blogPostKey);
        await cache.SetAsync(blogPostKey, blogPost);
    }

    return blogPost;
}
```

Redis toetab käsk torujuhtmete, kui mitu asünkroonne taotlused saadab kliendi rakendus. Redis saate multiplex sama ühenduse kaudu, mitte vastu ja vastates käske range järjestuses.

See aitab vähendada, muutes tõhusamalt kasutada võrgu latentsus. Järgmised koodilõigu kuvatakse näide, mis toob samaaegselt kahe kliendi üksikasju. Koodi esitab kahe taotlusi ja seejärel sooritab mõne muu töötlemine (ei kuvata) enne tulemusi saada. Funktsiooni `Wait` objekti vahemälu meetodit on sarnane .NET Framework `Task.Wait` meetod:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
var task1 = cache.StringGetAsync("customer:1");
var task2 = cache.StringGetAsync("customer:2");
...
var customer1 = cache.Wait(task1);
var customer2 = cache.Wait(task2);
```

Microsofti veebisaidi lehel [Azure'i Redis vahemälu dokumentatsiooni](https://azure.microsoft.com/documentation/services/cache/) abil saate rohkem teavet selle kohta, kuidas kirjutada klientrakendustes, mida saate kasutada Azure Redis vahemälu. Lisateave on saadaval veebisaidil StackExchange.Redis [põhialused lehe](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) .

Sama veebisaidi lehele [torujuhtmed ja multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) annab Lisateavet asünkroonsete toimingute ja torujuhtmete Redis ja StackExchange teegi kohta.  Järgmises jaotises abil Redis vahemällu, selles artiklis antakse ülevaade mõne rohkem täiustatud viise, mida saate rakendada andmetele, mis toimub Redis vahemälu.

## <a name="using-redis-caching"></a>Redis vahemällu

Lihtsaim kasutamine Redis vahemällu probleemid on võti ja väärtuse paarideks, kui väärtus on suvaline pikkus, mis võib olla mis tahes binaarandmeid uninterpreted string. (See on põhiosas massiivi baiti, mida võib käsitleda stringina). See stsenaarium on kujutatud käesoleva artikli jaotisest rakendada Redis vahemälu klientrakendustes.

Pange tähele, et klahvid ka sisaldada uninterpreted andmete abil saate teavet kahendarvu võti. Enam oluline on, aga rohkem ruumi kulub talletamiseks ja enam kulub otsing toiminguid. Kasutatavuse ja haldamise hõlbustamiseks, kujundada oma keyspace hoolikalt ja mõtestatud (kuid mitte Paljusõnaline) abil.

Näiteks tähistada võti kliendi ID 100, mitte lihtsalt "100" nagu "klientide: 100" liigendatud klahvide abil. Selle kava võimaldab teil hõlpsasti eristada talletada eri andmetüübiga väärtusi. Näiteks võib ka kasutada võti "tellimused: 100" tähistada Tellimuse ID 100 võti.

Ühemõõtmelise kahendarvu stringid, välja arvatud ka mahub paar Redis võti väärtus väärtus struktureerituma teavet, sh loendid, määrab (sorditud ja sortimata) ja hashes. Redis pakub mitmeid täielik käsk, mille saab töödelda järgmist tüüpi ja paljud need käsud on saadaval .NET Framework rakendustele nagu StackExchange kliendi teegi kaudu. Redis veebisaidi lehele [Sissejuhatus Redis andmetüüpide ja veevõtu](http://redis.io/topics/data-types-intro) ülevaade üksikasjalikumat ja käsud, mille abil saate neid muuta.

Selles jaotises toodud levinud kasutamine mõnikord nende andmetüübid ja käsud.

### <a name="perform-atomic-and-batch-operations"></a>Tehke atomic ja toimingud

Redis toetab stringiväärtust atomic get-ja set toimingute sarja. Neid toiminguid eemaldamine võimalik võistluse ohud, mis võivad tekkida kasutamisel eraldi `GET` ja `SET` käsud. Järgmised toimingud, mis on saadaval.

- `INCR`, `INCRBY`, `DECR`, ja `DECRBY`, mis toiminguid atomic lisandus ja decrement täisarvuni arvandmed väärtused. Teegi StackExchange pakub ülekoormatud versioonide soovitud `IDatabase.StringIncrementAsync` ja `IDatabase.StringDecrementAsync` võimalust neid toiminguid ja vahemälus talletatud tulemiks oleva väärtuse. Järgmised koodilõigu illustreerib, kuidas kasutada järgmisi võimalusi:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  await cache.StringSetAsync("data:counter", 99);
  ...
  long oldValue = await cache.StringIncrementAsync("data:counter");
  // Increment by 1 (the default)
  // oldValue should be 100

  long newValue = await cache.StringDecrementAsync("data:counter", 50);
  // Decrement by 50
  // newValue should be 50
  ```

- `GETSET`, mis toob väärtuse, mis on seotud klahvi ja muudab selle uue väärtuse. Teegi StackExchange teeb selle toimingu kaudu soovitud `IDatabase.StringGetSetAsync` meetod. Allpool koodilõigu kujutab näidet seda meetodit. Järgmine kood tagastab praeguse väärtuse, mis on seotud võti "andmed: kassaväärtuse" eelmises näites. Seejärel lähtestab see võti väärtus tagasi nullini kõik osana sama toimingut:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  string oldValue = await cache.StringGetSetAsync("data:counter", 0);
  ```

- `MGET`ja `MSET`, mis saab taastada või muuta ühe toiminguga stringi väärtuste kogumist. Funktsiooni `IDatabase.StringGetAsync` ja `IDatabase.StringSetAsync` meetodite ülekoormatud toetama seda funktsiooni, nagu on näidatud järgmises näites:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  // Create a list of key-value pairs
  var keysAndValues =
      new List<KeyValuePair<RedisKey, RedisValue>>()
      {
          new KeyValuePair<RedisKey, RedisValue>("data:key1", "value1"),
          new KeyValuePair<RedisKey, RedisValue>("data:key99", "value2"),
          new KeyValuePair<RedisKey, RedisValue>("data:key322", "value3")
      };

  // Store the list of key-value pairs in the cache
  cache.StringSet(keysAndValues.ToArray());
  ...
  // Find all values that match a list of keys
  RedisKey[] keys = { "data:key1", "data:key99", "data:key322"};
  RedisValue[] values = null;
  values = cache.StringGet(keys);
  // values should contain { "value1", "value2", "value3" }
  ```

Samuti saate ühendada mitu toimingud ühe Redis toimingu Redis tehingud ja töölehtede jaotises selles artiklis kirjeldatud. Teegi StackExchange toetab kaudu soovitud `ITransaction` kasutajaliidese.

Saate luua mõne `ITransaction` objekti abil soovitud `IDatabase.CreateTransaction` meetod. Kutsute tehingu käskude abil meetodid, mida on `ITransaction` objekti.

Funktsiooni `ITransaction` kasutajaliides annab juurdepääsu meetodite kogum, mis sarnaneb korrad on `IDatabase` kasutajaliides, välja arvatud, et kõik meetodid on asünkroonne. See tähendab, et need on ainult teha, kui selle `ITransaction.Execute` meetod on vaja järgida. Väärtus, mis tagastatakse funktsiooni `ITransaction.Execute` meetod näitab, kas tehing on loodud (tõene) või kui see ei õnnestunud (false).

Järgmised koodilõigu kuvatud näide selle kerimine ja taseme kaks hinnale sama tehingute osana.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
ITransaction transaction = cache.CreateTransaction();
var tx1 = transaction.StringIncrementAsync("data:counter1");
var tx2 = transaction.StringDecrementAsync("data:counter2");
bool result = transaction.Execute();
Console.WriteLine("Transaction {0}", result ? "succeeded" : "failed");
Console.WriteLine("Result of increment: {0}", tx1.Result);
Console.WriteLine("Result of decrement: {0}", tx2.Result);
```

Pidage meeles, et Redis tehingud on erinevalt tehingud relatsioonandmebaasidest. Funktsiooni `Execute` meetod lihtsalt järjekorrad kõik käsud, mis sisaldavad tehingu käivitada, ja kui neid on vigane siis tehingu on peatatud. Kui kõik käsud on edukalt järjekorras, iga käsk käivitatakse asünkroonselt.

Kui käsk nurjub, teised on jätkuvalt töötlemine. Kui teil on vaja veenduge, et käsk on lõpule viidud, peab toomiseks käsu tulemused **tulemi** atribuudi vastava tööülesande, nagu eeltoodud näites. Atribuudi **tulemi** lugemine blokeerib helistaja jutulõnga kuni tööülesanne on lõpule viidud.

Lisateabe saamiseks vaadake lehte [tehingute Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) StackExchange.Redis veebisaidil.

Kui paketi toimingute tegemiseks saate kasutada funktsiooni `IBatch` kasutajaliidese StackExchange teegi. See liides võimaldab juurdepääsu kogumi meetodite sarnanevad korrad on `IDatabase` kasutajaliides, välja arvatud, et kõik meetodid on asünkroonne.

Loote mõne `IBatch` objekti abil soovitud `IDatabase.CreateBatch` meetod ja seejärel Käivita pakett abil soovitud `IBatch.Execute` meetod, nagu on näidatud järgmises näites. Järgmine kood lihtsalt määrab stringiväärtus, kerimine ja taseme sama hinnale, mis eelmises näites kasutatakse ja kuvab tulemused.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
IBatch batch = cache.CreateBatch();
batch.StringSetAsync("data:key1", 11);
var t1 = batch.StringIncrementAsync("data:counter1");
var t2 = batch.StringDecrementAsync("data:counter2");
batch.Execute();
Console.WriteLine("{0}", t1.Result);
Console.WriteLine("{0}", t2.Result);
```

On oluline, et aru saada, et erinevalt tehing, kui käsk partii nurjub, kuna see on vigane, käsud võib siiski käivitada. Funktsiooni `IBatch.Execute` meetod ei tagasta viidet teavitab õnnestumisest või nurjumisest.

### <a name="perform-fire-and-forget-cache-operations"></a>Tehke fire ja unustage vahemälu toimingud

Redis toetab fire ja unustage toimingud käsk lipud abil. Sellisel juhul kliendi lihtsalt käivitab toimingu, kuid ei ole huvi tulem ja pole oodata käsk täidetakse. Järgmises näites kujutatakse DETECTED käsu nimega tule ja unustada toiming:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
await cache.StringSetAsync("data:key1", 99);
...
cache.StringIncrement("data:key1", flags: CommandFlags.FireAndForget);
```

### <a name="specify-automatically-expiring-keys"></a>Määrake automaatselt aeguvad võtmed

Kui salvestate üksuse Redis vahemälu, saate määrata, pärast mida üksuse eemaldatakse automaatselt Vahemälu ajalõpp. Saate ka päringu, kui palju aega klahvi on enne selle lõppemist, kasutades funktsiooni `TTL` käsk. See käsk on saadaval StackExchange rakendustele, kasutades funktsiooni `IDatabase.KeyTimeToLive` meetod.

Järgmised koodilõigu näitab, kuidas Määrake aegumise kellaaeg 20 minutit võti ja päringu järelejäänud elu võti:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration time of 20 seconds
await cache.StringSetAsync("data:key1", 99, TimeSpan.FromSeconds(20));
...
// Query how much time a key has left to live
// If the key has already expired, the KeyTimeToLive function returns a null
TimeSpan? expiry = cache.KeyTimeToLive("data:key1");
```

Samuti saate aegumise aeg kindla kuupäeva ja kellaaja aegumise käsu, mis pole saadaval, kui teegis StackExchange abil soovitud `KeyExpireAsync` meetod:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration date of midnight on 1st January 2015
await cache.StringSetAsync("data:key1", 99);
await cache.KeyExpireAsync("data:key1",
    new DateTime(2015, 1, 1, 0, 0, 0, DateTimeKind.Utc));
...
```

> _Näpunäide:_ Saate käsitsi eemaldada üksuse vahemälust, kasutades käsku DEL, mis on saadaval nimega StackExchange teegi kaudu soovitud `IDatabase.KeyDeleteAsync` meetod.

### <a name="use-tags-to-cross-correlate-cached-items"></a>Rist-korrelaatideta vahemällu talletatud üksuste siltide abil

Redis on mitu üksust, mida jagada ühe võtme kogum. SADD käsu abil saate luua kogum. SMEMBERS käsu abil saate tuua vastuseks kogumis olevate üksuste. Teegi StackExchange rakendab SADD käsku selle `IDatabase.SetAddAsync` meetod ja selle SMEMBERS käsk koos selle `IDatabase.SetMembersAsync` meetod.

Samuti saate ühendada olemasoleva komplekti luua uue komplekti SDIFF (määramine erinevus), räbu (seadmine ühisosa) ja SUNION (määramine Liit) käskude abil. Teegi StackExchange ühendab neid tegevusi selle `IDatabase.SetCombineAsync` meetod. Esimese parameetrina selle meetodi määrab määramine toimingu sooritamiseks.

Järgmised Koodilõigud kuvada, kuidas komplekti võib olla kasulik kiiresti säilitamine ja allalaadimine seostuvate üksuste kogumid. Järgmine kood kasutab funktsiooni `BlogPost` tüüp, mida on kirjeldatud jaotises rakendada Redis vahemälu klientrakendused käesoleva artikli.

A `BlogPost` objekt sisaldab neljal-ID, tiitel, järjestuse Keskmine ja siltide kogum. Esimese koodilõigu allpool kuvatakse C# loendi asustamiseks kasutatav Näidisandmete `BlogPost` objektid:

```csharp
List<string[]> tags = new List<string[]>()
{
    new string[] { "iot","csharp" },
    new string[] { "iot","azure","csharp" },
    new string[] { "csharp","git","big data" },
    new string[] { "iot","git","database" },
    new string[] { "database","git" },
    new string[] { "csharp","database" },
    new string[] { "iot" },
    new string[] { "iot","database","git" },
    new string[] { "azure","database","big data","git","csharp" },
    new string[] { "azure" }
};

List<BlogPost> posts = new List<BlogPost>();
int blogKey = 0;
int blogPostId = 0;
int numberOfPosts = 20;
Random random = new Random();
for (int i = 0; i < numberOfPosts; i++)
{
    blogPostId = blogKey++;
    posts.Add(new BlogPost(
        blogPostId,               // Blog post ID
        string.Format(CultureInfo.InvariantCulture, "Blog Post #{0}",
            blogPostId),          // Blog post title
        random.Next(100, 10000),  // Ranking score
        tags[i % tags.Count]));   // Tags--assigned from a collection
                                  // in the tags list
}
```

Silte saate salvestada iga `BlogPost` objekti kogumina Redis vahemälu ja seostada ID-d igas kogumis on `BlogPost`. See võimaldab rakenduse kõik sildid, mis kuuluvad tteatud ajaveebipostituse kiireks leidmiseks. Teistpidi otsimise lubamine ja kõigi ajaveebipostituste jagavad teatud sildi, saate luua veel mõni, mis hoiab postitused viitamine sildi ID võti:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Tags are easily represented as Redis Sets
foreach (BlogPost post in posts)
{
    string redisKey = string.Format(CultureInfo.InvariantCulture,
        "blog:posts:{0}:tags", post.Id);
    // Add tags to the blog post in Redis
    await cache.SetAddAsync(
        redisKey, post.Tags.Select(s => (RedisValue)s).ToArray());

    // Now do the inverse so we can figure how which blog posts have a given tag
    foreach (var tag in post.Tags)
    {
        await cache.SetAddAsync(string.Format(CultureInfo.InvariantCulture,
            "tag:{0}:blog:posts", tag), post.Id);
    }
}
```

Need struktuurid võimaldavad teil teha mitme levinud päringute väga tõhusalt. Näiteks saate otsida ja kuvada siltide ajaveebipostituse 1 umbes järgmine:

```csharp
// Show the tags for blog post #1
foreach (var value in await cache.SetMembersAsync("blog:posts:1:tags"))
{
    Console.WriteLine(value);
}
```

Leiate kõik sildid, mis on levinud ajaveebi postitamine 1 ja ajaveebi postituse 2 määramine ühisosa toiming, tehes järgmiselt:

```csharp
// Show the tags in common for blog posts #1 and #2
foreach (var value in await cache.SetCombineAsync(SetOperation.Intersect, new RedisKey[]
    { "blog:posts:1:tags", "blog:posts:2:tags" }))
{
    Console.WriteLine(value);
}
```

Ja leiate, et kõik teatud silti sisaldavad postitused.

```csharp
// Show the ids of the blog posts that have the tag "iot".
foreach (var value in await cache.SetMembersAsync("tag:iot:blog:posts"))
{
    Console.WriteLine(value);
}
```

### <a name="find-recently-accessed-items"></a>Üksuste otsimine viimati kasutatud

Levinud tööülesande paljude rakenduste nõutav on kõige viimati kasutatud üksuste otsimiseks. Näiteks võite ajaveebindus saidi kuvada teavet kõige viimati loetuks ajaveebipostituste.

Saate rakendada selle funktsiooni abil Redis loendi. Redis loend sisaldab mitut üksust, jagavad sama võti. Loendi toimib kahepoolne järjekorda. Vajutada üksused või loendi lõpus LPUSH (vasakul push) ja RPUSH (õige push) käskude abil. Saate tuua üksused või loendi lõpus LPOP ja RPOP käskude abil. Samuti saate naasta element LRANGE ja RRALDA käskude abil.

Allpool Koodilõigud näitavad, kuidas saate teha järgmisi toiminguid StackExchange teegi abil. Järgmine kood kasutab funktsiooni `BlogPost` eelmistes näidetes tüüp. Nagu ajaveebipostituse on kasutaja, lugege selle `IDatabase.ListLeftPushAsync` meetod sunnib peale loend, mis on seotud võti "blog:recent_posts" Redis vahemälu ajaveebipostituse pealkiri.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:recent_posts";
BlogPost blogPost = ...; // Reference to the blog post that has just been read
await cache.ListLeftPushAsync(
    redisKey, blogPost.Title); // Push the blog post onto the list
```

Nagu rohkem ajaveebipostituste on lugeda, on nende pealkirjade viinud peale samas loendis. Loendis on tellitud pealkirjad on lisatud jada. Kõige lugeda viimati ajaveebipostituste on vasakul loendi lõpus. (Kui sama ajaveebipostituse on rohkem kui üks kord lugeda, selle on mitu kirjet loendis.)

Saate kuvada kõige viimati loetuks postituste pealkirjad, kasutades selle `IDatabase.ListRange` meetod. See meetod suunab klahvi, mis sisaldab loendi, alguspunkt ja lõpeb punkti. Järgmine kood toob 10 ajaveebipostituste (kirjed vahemikus 0 – 9) loendi lõpus vasakpoolse pealkirjad:

```csharp
// Show latest ten posts
foreach (string postTitle in await cache.ListRangeAsync(redisKey, 0, 9))
{
    Console.WriteLine(postTitle);
}
```

Pange tähele, et selle `ListRangeAsync` meetod teegist loendist. Selleks saate kasutada funktsiooni `IDatabase.ListLeftPopAsync` ja `IDatabase.ListRightPopAsync` viise.

Loendi takistada kasvab lõpmatuseni, saate perioodiliselt praagitud trimmimise loendi üksuste. Allpool koodilõigu näidatakse, kuidas eemaldada kõik, kuid viie vasakpoolse üksuste loendist.

```csharp
await cache.ListTrimAsync(redisKey, 0, 5);
```

### <a name="implement-a-leader-board"></a>Tahvli juhataja rakendada.

Vaikimisi on otsustanud vastuseks kogumis olevate üksuste kindlas järjestuses. ZADD käsu abil saate luua tellitud komplekti (selle `IDatabase.SortedSetAdd` StackExchange teegis meetod). Üksused on järjestatud abil nimega Keskmine, mis on esitatud parameetrina käsk arvulise väärtuse.

Järgmised koodilõigu lisab pealkiri ajaveebipostituse järjestatud loendisse. Selles näites on iga ajaveebipostituse ka Keskmine välja, mis sisaldab ajaveebipostituse järjestuse.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:post_rankings";
BlogPost blogPost = ...; // Reference to a blog post that has just been rated
await cache.SortedSetAddAsync(redisKey, blogPost.Title, blogpost.Score);
```

Saate tuua ajaveebi postituse pealkirjad ja hinded tõusvas järjestuses Keskmine, kasutades funktsiooni `IDatabase.SortedSetRangeByRankWithScores` meetod:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(redisKey))
{
    Console.WriteLine(post);
}
```

> [AZURE.NOTE] StackExchange teek sisaldab ka selle `IDatabase.SortedSetRangeByRankAsync` meetod, mis tagastab andmed Keskmine järjestuses, kuid ei tagasta hinded.

Saate ka üksused laskuvas järjestuses hinded alla laadida ja esitada täiendavad parameetrid tagastatud üksuste arv piirata selle `IDatabase.SortedSetRangeByRankWithScoresAsync` meetod. Järgmises näites kuvatakse tiitlite ja hulgaliselt ülemine 10 järjestatud ajaveebipostituste:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(
                               redisKey, 0, 9, Order.Descending))
{
    Console.WriteLine(post);
}
```

Järgmises näites kasutatakse funktsiooni `IDatabase.SortedSetRangeByScoreWithScoresAsync` meetod, mille abil saate piirata üksused, mis tagastatakse need, mis kuuluvad antud Keskmine vahemik:

```csharp
// Blog posts with scores between 5000 and 100000
foreach (var post in await cache.SortedSetRangeByScoreWithScoresAsync(
                               redisKey, 5000, 100000))
{
    Console.WriteLine(post);
}
```

### <a name="message-by-using-channels"></a>Sõnumi kanalite abil

Peale abistas andmete vahemälu, pakub Redis server, sõnumside kaudu suure jõudlusega Publisheri/abonendi süsteem. Kanali saate tellida klientrakendustes ja muud rakendused või teenused saate avaldada sõnumite kanal. Tellimise rakenduste saadetakse sõnumid ja neid saab töödelda.

Redis pakub kliendi rakenduste abil tellida kanalite käsku Telli. Eeldab, et selle käsu nimi ühe või mitme kanalid, millel kuvatakse rakenduse meilisõnumeid vastu võtma. StackExchange teek, mis sisaldab soovitud `ISubscription` kasutajaliides, mis võimaldab tellida ja avaldada kanalite rakenduse .NET Framework.

Saate luua mõne `ISubscription` objekti abil soovitud `GetSubscriber` Redis serveriga ühenduse meetodit. Seejärel saate kuulata sõnumeid kanali abil soovitud `SubscribeAsync` selle objekti meetodit. Koodi järgmises näites kujutatakse nimega "sõnumid: blogPosts" kanali tellimine:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
await subscriber.SubscribeAsync("messages:blogPosts", (channel, message) =>
{
    Console.WriteLine("Title is: {0}", message);
});
```

Esimene parameeter on `Subscribe` meetod on kanali nimi. Sellise nimega järgib sama põhimõtted, kasutatavate kiirklahvide (Pääsuklahvide) vahemälu. Nimi võib sisaldada mis tahes binaarandmeid, kuigi see on soovitatav kasutada suhteliselt lühikesed ja mõtestatud stringide tagada head jõudlust ja hooldatavust.

Pange tähele, et kasutavad kanalite nimeruumi on eraldi, et kasutada klahvide abil. See tähendab, et teil võib olla kanalid ja tutvustatakse, mida on sama nimi, kuigi see võib muuta oma rakenduse koodi raskem säilitada.

Teine parameeter on soovitud toimingu volitatud esindaja. Selle volitatud esindaja käivitatakse asünkroonselt iga kord, kui uus sõnum kuvatakse kanal. Selles näites kuvab teate lihtsalt Console (sõnum sisaldab ajaveebipostituse pealkiri).

Kanali avaldamiseks rakenduse käsuga Redis avaldada. StackExchange teek on `IServer.PublishAsync` meetod selle toimingu sooritamiseks. Järgmise koodilõigu näitab, kuidas avaldada teade "sõnumid: blogPosts" kanali:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
BlogPost blogpost = ...;
subscriber.PublishAsync("messages:blogPosts", blogPost.Title);
```

On mitu tuleks mõista avalda/tellimise süsteemi kohta:

- Sama kanali saate tellida mitme tellijad ja kõik saavad nad sõnumid, mis on avaldatud selle kanali.
- Tellijad on ainult sõnumeid, mis on avaldatud, kui nad on tellinud. Kanalite on ei puhverdatud ja kui sõnum on avaldatud, Redis taristu sunnib sõnumit, et iga abonendi ja seejärel eemaldab selle.
- Vaikimisi saadetud meilisõnumid tellijad need saadetakse järjestuses. Suure hulga sõnumeid ja palju tellijad ja tootjad aktiivne süsteemis, tagatud järjestikku sõnumite kohaletoimetamisega aeglustada süsteemi. Kui iga sõnumi ei sõltu ja tellimus on ebaolulised, saate lubada samaaegseid töötlemine Redis süsteemi, mis aitab parandada tundlikkuse järgi. Võite saavutada see StackExchange klient, seades PreserveAsyncOrder kasutatavaid abonendi FALSE ühendus:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
redisHostConnection.PreserveAsyncOrder = false;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
```

## <a name="related-patterns-and-guidance"></a>Seotud mustrite ja juhised

Järgmise mustriga ka leiduda milline olukord teie jaoks oluline, kui rakendate rakenduste vahemällu.

- [Vahemälu-kõrvale muster](http://msdn.microsoft.com/library/dn589799.aspx): See muster kirjeldatakse, kuidas andmete nõudmisel laadimiseks poest andmete vahemälu. See muster aitab hallata andmeid, mis toimub vahemälu ja algse andmesalve andmeid.
- [Sharding mustri](http://msdn.microsoft.com/library/dn589797.aspx) leiate teavet rakendamise horisontaalne eraldamine skaleeritavus talletamine ja juurdepääsemisel suuri andmemahtusid täiustamiseks.

## <a name="more-information"></a>Lisateave

- Microsofti veebisaidi lehele [MemoryCache klassi](http://msdn.microsoft.com/library/system.runtime.caching.memorycache.aspx)
- [Azure'i Redis vahemälu dokumentatsiooni](https://azure.microsoft.com/documentation/services/cache/) lehel Microsofti veebisaidil
- Microsofti veebisaidi [Azure'i Redis vahemälu KKK](redis-cache/cache-faq.md) lehele
- Microsofti veebisaidi lehele, [konfigureerimine mudel](http://msdn.microsoft.com/library/windowsazure/hh914149.aspx)
- [Toimingupõhise asünkroonne mustri](http://msdn.microsoft.com/library/hh873175.aspx) lehe Microsofti veebisaidil
- StackExchange.Redis GitHub repo [torujuhtmed ja multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) lehele
- Redis veebisaidi lehele [Redis püsimine](http://redis.io/topics/persistence)
- [Lehe kopeerimine](http://redis.io/topics/replication) Redis veebisaidil
- Redis veebisaidi lehele [Redis kobar õpetus](http://redis.io/topics/cluster-tutorial)
- Funktsiooni [Eraldatav: kuidas jagada andmete hulgas Redis mitmes eksemplaris](http://redis.io/topics/partitioning) Redis veebisaidi lehele
- Redis veebisaidi lehele [Nimega mõne LRU vahemälu Redis abil](http://redis.io/topics/lru-cache)
- Redis veebisaidi lehele, [tehingud](http://redis.io/topics/transactions)
- Redis veebisaidi lehele [Redis turvalisus](http://redis.io/topics/security)
- [Ringi ümber Azure'i Redis vahemälu](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) lehele Azure ajaveeb
- [Töötab Redis CentOS Linux VM Azure](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) Microsofti veebisaidil lehele
- [ASP.net-i seansi oleku pakkuja Azure'i Redis vahemälu jaoks](redis-cache/cache-aspnet-session-state-provider.md) lehel Microsofti veebisaidil
- [ASP.net-i väljundi vahemälu pakkuja Azure'i Redis vahemälu](redis-cache/cache-aspnet-output-cache-provider.md) lehe Microsofti veebisaidil
- Redis veebisaidi lehele, [Redis andmetüüpide ja veevõtu tutvustus](http://redis.io/topics/data-types-intro)
- StackExchange.Redis veebisaidi lehele, [põhialused](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md)
- StackExchange.Redis repo [Redis tehingute](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) lehele
- Microsofti veebisaidil [andmete eraldamine juhend](http://msdn.microsoft.com/library/dn589795.aspx)
