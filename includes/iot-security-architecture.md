# <a name="internet-of-things-security-architecture"></a>Asjade Interneti turvalisuse arhitektuur

Süsteemi projekteerimisel tuleb mõista selle süsteemi võimalike ohtudega ja lisada asjakohane kaitsemehhanisme, kui süsteem on kavandatud ja architected. See on eriti oluline kujundada algusest toote turvalisust silmas pidades, sest kui ründaja võib ohustada süsteem aitab kindlasti asjakohane kergendamise mõistmist on olemas algusest. 

## <a name="security-starts-with-a-threat-model"></a>Turvalisus algab oht mudel
 
Microsoft on pikka aega kasutatud oht mudelid oma toodetele ja teinud ettevõtte ohtu modelleerimise protsessi avalikult kättesaadavad. Firma kogemus näitab, et modelleerimine on vahetu ülevaate millised ohud on kõige ootamatu hüvede kohta. Näiteks, samuti luuakse avenue avatud arutelu teistega väljaspool arendusmeeskond, mis võib kaasa tuua uusi ideid ja toote parandamist.
  
Ohtu modelleerimise eesmärk on mõista, kuidas ründaja võib kahjustada süsteemi ning veenduge, et kehtestatud on asjakohane kergendamise. Ohtu modelleerimine jõud disain meeskond kaaluda kergendamise süsteemi ootuspäraselt, mitte pärast süsteemi juurutamist. See on äärmiselt oluline, sest turvalisuse kaitsemehhanisme seadmed välja hulgaliselt moderniseerimine on võimatu, palju vigu ja lahkuvad külastajad ohus.

Palju arengu võistkonda ei hõivamiseks funktsionaalsete nõuete süsteem, et hotelli suurepäraselt. Siiski mitte ilmne viiside, et keegi võib kuritarvitada süsteem on keerukam. Ohtu modelleerimine aitab mõista, mida ründaja võib teha arendusmeeskonnad ja miks. Ohtu modelleerimine on struktureeritud protsess, mis loob arutelu selle disaini otsuseid süsteemi ning muudab tehtud teel tagatise mõju projekti. Kuigi oht mudel on lihtsalt dokument, dokumentide esindab ka ideaalne võimalus tagada järjepidevus teadmiste omandiõiguse lugu õppinud ja abi uus meeskond pardal kiiresti. Lõpuks käsitlevate ohtu modelleerimine on võimaldada teil kaaluda teisi aspekte, nt mida soovite pakkuda oma klientidele julgeolekukohustusi. Ohtu modelleerimine koos kohustuse teavitada ja sõita oma asjade Interneti (IoT) lahenduse testimine.
 

### <a name="when-to-threat-model"></a>Kui oht mudel

[Ohtu modelleerimine](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) pakub suurimat väärtus, kui see on ühendatud projekteerimise käigus. Kujundamisel, on suurim võimalus muuta ohtude kõrvaldamiseks. Kõrvaldada ohud disain on soovitud tulemus. See on palju lihtsam kui lisades kergendamise, katsetamisele ja nad hoida ning lisaks nende kõrvaldamine ei ole alati võimalik. Muutub see raskesti kõrvaldada ohud nagu toote saab küpsemaks ja omakorda lõpuks nõuab rohkem tööd ja palju raskem kompromissidega kui ohtu varakult arengu modelleerimine.

### <a name="what-to-threat-model"></a>Mida ohu mudel

Tuleks teema mudel kogu lahendus ja keskenduma ka järgmistes valdkondades:

- Turvalisus ja privaatsus funktsioonid
- Kelle vead on asjakohased turvalisuse funktsioonid
- Funktsioonid, mis puudutavad usaldus piir 

### <a name="who-threat-models"></a>Kes oht mudelid

Ohtu modelleerimine on nagu iga teine protsess.  On mõistlik ravida nagu mõne muu komponendi lahuse ohtu näidis ja kinnitada seda. Palju arengu võistkonda ei hõivamiseks funktsionaalsete nõuete süsteem, et hotelli suurepäraselt. Siiski mitte ilmne viiside, et keegi võib kuritarvitada süsteem on keerukam. Ohtu modelleerimine aitab mõista, mida ründaja võib teha arendusmeeskonnad ja miks.

### <a name="how-to-threat-model"></a>Kuidas ohtu mudel

Protsessi modelleerimine ohtu koosneb neljas etapis; sammud on:

- Mudeli taotlus
- Loetleda ohud
- Leevendada ohud
- Kinnitada kergendamise

#### <a name="the-process-steps"></a>Protsessi etappide

Kolm eeskirju thumb kui ohtu Saladuslik meeles pidama:

1. Loo diagramm viide arhitektuuri kokku. 
2. Alustada laiuti. Ülevaade ja mõista süsteemi tervikuna enne sügavale sukeldumine.  Sellega tagate, et te deep-dive õiges kohas.
3. Juhtida protsessi, ei lase autot sa protsessi. Kui leiate probleemi modelleerimise faasis ja soovite seda kasutama hakata, minna seda!  Ei tunne sa pead järgige neid samme lömitab finants.  

#### <a name="threats"></a>Ohud

Neli põhielemente ohtu mudel on:

- Protsessid (veebiteenused, Win32 services * nix deemonid jne. Pange tähele, et mõned keeruliste üksuste (nt väli väravaid ja andurid) saab võetud protsess, kui tehnilised süvitsiminek nendes valdkondades ei ole võimalik.
- Andmeid talletab (kusagil andmete salvestamise nagu konfiguratsiooni faili või andmebaasi)
- Andmevoo (andmete asub teiste elementide taotluses)
- Väliste üksustega (midagi, mis suhtleb süsteem, kuid ei ole taotluse kontrolli all, näited hõlmavad kasutajad ja satelliit-kanalid)

Kõik arhitektuuri diagrammis kuuluvad ohtudele; kasutame Mnemoonika RÜTM. Loe [Ohtu modelleerimine Jällegi JOOKSUSAMMU](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) RÜTM elementide kohta rohkem teada.

Erinevate taotluse diagramm kuuluvad teatud sammu ohud:

- Kohaldatakse RÜTM
- Andmete edastamise suhtes kohaldatakse kolm korda Ööpäevas
- Andmete kuuluvad kolm korda Ööpäevas, ja mõnikord R, kui andmete on logifailid.
- Välistele isikutele kuuluvad SRD

## <a name="security-in-iot"></a>Asjade Interneti turvalisus

Sihtotstarbeline seadmetel on palju võimalikke koostoimeid pindala ja suhtluse mustreid, mis kõik tuleb pidada luuakse raamistik digitaalse juurdepääsu nende seadmete. Termin "digitaalne juurdepääsetavus" kasutatakse siin eristada tegevusest, mis otse seadme koostoime kaudu juurdepääsu turvalisuse osutamise füüsilise juurdepääsu kontrollimise abil. Näiteks hakanud seade ruumi lukustatav uks. Kuigi ei saa välistada füüsilise juurdepääsu, kasutades tarkvara ja riistvara, mõõte saab võtta mis takistavad sissepääsu toovad süsteemi häired. 

Nagu me uurida suhtluse mustreid, me vaatame "seadme kontroll" ja "seadme andmed" sama tasandi tähelepanu. Mis tahes teave seadmega poolte poolt ning nende eesmärk on muuta või mõjutada tema käitumist tema või tema keskkonna seisundi kohta võib liigitada "Juhtseadme". Seade kiirgab teisele poolele oma ja oludega täheldatud riigi kohta teavet võib liigitada "Seadme andmed".
   
Et optimeerida parimad, soovitatakse, et tüüpiline asjade arhitektuur jaguneb mitme komponendi/tsooni osana modelleerimine kasutamise ohtu. Nendes piirkondades on täielikult kirjeldatud käesolevas lõigus toodud ja sisaldama:

-   Seadme,
-   Gateway väli
-   Pilve väravaid, ja
-   Teenused.

Tsoonid on lai, nii et segmendi lahendus; Iga tsoon on sageli oma andmeid ja autentimise ja luba nõuded. Piirkondades saab kasutada eraldi kahju ja piirata mõju madal usaldus tsoonide suurem usaldus tsoone.

Iga tsooni eraldatud usaldada piiri, mis on märgatud joonist punane punktiirjoon. Ta esindab ülemineku andmeid ühest kohast teise. Selle ülemineku ajal andmeid ja teavet võidakse kelmuse, rikkumine, lepingu rikkumine, teabe avalikustamist, teenusetõkke ja reljeefi privileeg (RÜTM).

![IoT turvalisuse tsoonid](media/iot-security-architecture/iot-security-architecture-fig1.png) 

Jooksul iga piire kujutatud komponendid läbivad ka RÜTM, mis võimaldab täielikult 360 ohtu modelleerimine vaade lahust. Allpool täpsustada iga komponendid ja julgeoleku probleeme ja lahendusi, mis tuleb kasutusele võtta.

Lõigud, mis järgneb arutab standardseid komponente, mis on tavaliselt leitud nendes tsoonides.

### <a name="the-device-zone"></a>Seadme tsooni

Seadme keskkond on vahetu füüsilise ruumi seadme ümber kui füüsilise juurdepääsu ja/või "kohalikus võrgus" peer-to-peer digitaalne seade on teostatav. "LAN" on eeldatavalt võrku, mis on eraldi ja isoleeritud – aga potentsiaalselt sillatud et – avaliku Interneti ja hõlmab lähiala raadioside tehnoloogia, mis võimaldab peer-to-peer edastamise seadmed. See ei *ole* hulka võrgu virtualiseerimine tehnoloogia, kohaliku võrgu illusiooni ja ka ei sisalda avaliku sektori operaatorile võrkudes, mis nõuavad iga kahe seadme suhelda üle avaliku võrgu ruumi oleksid need sisestada peer-to-peer side suhe.

### <a name="the-field-gateway-zone"></a>Gateway väli tsooni

Väli gateway on seade/aparaat või mõned üldotstarbelised server arvuti tarkvara, mis toimib nagu teatises võimaldaja ja võimalusel ka seadme süsteemile ja seadme andmete töötlemise keskus. Väli gateway tsoon hõlmab välja värav ise ja kõik seadmed, mis on kinnitatud selle külge. Nagu nimigi ütleb, välja sinna tegutsema väljaspool spetsiaalne töötlemise vahendid on tavaliselt seotud asukohta, on potentsiaalselt füüsilise sissetungi ja on piiratud töö koondamise. Kõik rääkida, et välja värav on tavaliselt asi üks touch ja teades, mis oma funktsiooni on sabotaaž. 

Gateway väli erineb lihtsalt liikluse ruuter, et tal on aktiivselt juurdepääsu haldamine ning teabe liikumist, taotlus on adresseeritud üksus ja võrguühenduste või terminali seansi. NAT-seadme või tulemüür, seevastu ei kuulu väravatena välja kuna nad ei ole otseselt ühenduse või seansi terminalid, kuid pigem marsruudi (või ploki) ühendused või nende kaudu tehtud istungid. Gateway väli on kaks erinevat pindala. Üks nägu seadmed, mis on kinnitatud selle külge ja moodustab selle tsooni sees ja teine nägu väliste osapoolte ja on vööndi servale.   

### <a name="the-cloud-gateway-zone"></a>Pilve gateway tsooni

Pilve gateway on süsteem, mis võimaldab serveri suhtlemise: ja vahendite või välja sinna mitu eri kohtades üle avaliku võrgu ruumi tavaliselt pilvepõhise kontroll ja andmete analüüsi süsteemi, nende süsteemide Föderatsiooni suunas. Mõnikord pilve värav võib kohe soodustama sihtotstarbeline seadmetele automaatidest nagu telefonid või tabletid. Siin kontekstis tähendab "pilve" viitavad spetsiaalne andmetöötlussüsteemi, mis on seotud samal territooriumil ühendatud seadmed või välja väravaid. Ka pilve tsoonis operatiivsete meetmete takistavad suunatud sissepääsu ja see ei ole tingimata "avalik pilv" infrastruktuuri.  

Pilve värav võib potentsiaalselt tuleb kaardistada võrgu virtualiseerimine ülekate soojustada pilve gateway ja kõigis ühendatud seadmed ja välja sinna võrgu liiklust. Pilve gateway, ise on ei ole seadme juhtimissüsteemi ega töötlemise või hoidla seadme andmed; neid süsteemiga pilve gateway. Pilve gateway tsoon hõlmab kõik välja väravaid ja seadmed, mis on otseselt või kaudselt seotud ise pilve gateway. Vööndi servale on eraldi pindala, kus isikutele välise suhtluse kaudu.

### <a name="the-services-zone"></a>Teenuste tsoon

"Teenus" on määratud seoses tarkvara komponendi või moodul, mis on liidestunud seadmed välja - või pilve lüüsi kaudu andmete kogumine ja analüüs, samuti juhtimis- ja kontrollisüsteemi.  Teenused on vahendajad. Nad tegutsevad väravaid ja peavad järgima samasuse alusel, säilitada ja analüüsida andmeid, iseseisvalt anda käsud seadmetega andmete teadmisi või sõiduplaanid ja paljastada teavet lihtsamaid ja volitatud lõppkasutajatele.

### <a name="information-devices-vs-special-purpose-devices"></a>Infoseadmete vs sihtotstarbeline seadmed

Arvutid, telefonid ja tabletid on peamiselt interaktiivseid seadmeid. Läheduses maksimeerides aku eluiga selgesõnaliselt optimeeritud telefonid ja tabletid. Nad soovitavalt välja lülitada osaliselt mitte kohe kasutamisel isik, või kui ei paku teenuseid, nagu muusika mängimine või suunavad nende omanik kindlasse asukohta. Süsteemide küljest need teavet tehnoloogia seadmed tegutsevad peamiselt volikirju inimesed. Nad on "inimesed ajamid" selliste meetmete "inimesed andurid" kogumise sisend. 

Sihtotstarbeline seadmed, lihtne temperatuuri andurid keeruline tehase tootmisliinide tuhandeid komponendid sees, on erinevad. Need seadmed on palju hõlmavaid eesmärk ja ka siis, kui nad pakuvad mõned kasutajaliidese, nad on suuresti hõlmavaid liidestamine või integreerida vara füüsilises maailmas. Nad mõõta keskkonnatingimustest, keerake ventiilid, juhtida servos, heli alarmid, valgusteid, ja aru ei ülesandeid. Need aitavad teha tööd mille teavet seade on liiga üldine, liiga kallis, liiga suur või liiga haprad. Konkreetne eesmärk dikteerib oma tehnilise projekti kohe kui ka saadaval rahaline eelarve ning operatsiooni kavandatud eluea. Nimetatud kaks peamiste tegurite koosmõju piirab saadaval tegevuse energia eelarve, füüsilise jalajälg ja seega saadaolevat, Arvuta ja turbevõimalused.  

Kui midagi "halba" automatiseeritud või remote kontrollitavad seadmed, näiteks füüsikalisi defekte või kontrolli vead tahtliku volitamata sissetungi ja manipuleerimine. Toodangu partiid võib hävitada, hoonete võib rüüstati ja põles ning inimesed olla vigastatud või isegi surra. See on muidugi kahju kui keegi maxing on varastatud krediitkaardi limiidi läbi kogu teine klass. Julgeoleku bar seadmed, mis teevad asjad liikuma, ja ka andurite andmeid, mis lõpuks tulemuseks käsud, mis põhjustavad asjad liikuma, ei tohi ületada pood või Panga stsenaarium. 


### <a name="device-control-and-device-data-interactions"></a>Juhtseadme ja seadme andmete vastastikuse mõju

Sihtotstarbeline seadmetel on palju võimalikke koostoimeid pindala ja suhtluse mustreid, mis kõik tuleb pidada luuakse raamistik digitaalse juurdepääsu nende seadmete. Termin "digitaalne juurdepääsetavus" kasutatakse siin eristada tegevusest, mis otse seadme koostoime kaudu juurdepääsu turvalisuse osutamise füüsilise juurdepääsu kontrollimise abil. Näiteks hakanud seade ruumi lukustatav uks. Kuigi ei saa välistada füüsilise juurdepääsu, kasutades tarkvara ja riistvara, mõõte saab võtta mis takistavad sissepääsu toovad süsteemi häired. 
 
Nagu me uurida suhtluse mustreid, me vaatame "seadme kontroll" ja "seadme andmed" sama tasandi tähelepanu samas ohtu modelleerimine. Mis tahes teave seadmega poolte poolt ning nende eesmärk on muuta või mõjutada tema käitumist tema või tema keskkonna seisundi kohta võib liigitada "Juhtseadme". Seade kiirgab teisele poolele oma ja oludega täheldatud riigi kohta teavet võib liigitada "Seadme andmed". 

## <a name="threat-modeling-the-azure-iot-reference-architecture"></a>Ohtu modelleerimine Azure IoT viide arhitektuur

Microsoft kasutab eespool kirjeldatud oht kasutatakse Azure IoT raames. Allpool jaotises Seetõttu kasutame Azure IoT viide arhitektuuri konkreetne näide kuidas mõelda modelleerimine IoT ohtu ja kuidas ohtu. Meie puhul me kindlaks neli peamist tegevusala:

-   Seadmed ja andmeallikad,
-   Andmed transpordi,
-   Seadme ja sündmuse töötlemisega ja
-   Esitlus

![Ohtu Azure'i IoT modelleerimine](media/iot-security-architecture/iot-security-architecture-fig2.png) 

Joonist annab lihtsustatud ülevaate Microsofti IoT arhitektuur andmete vooldiagrammi mudelit, mida kasutab Microsoft ohtu modelleerimine tööriist:

![Ohtu modelleerimine Azure IoT MS ohtu modelleerimine tööriista abil](media/iot-security-architecture/iot-security-architecture-fig3.png)

On oluline märkida arhitektuur eraldab seadme ja gateway võimeid. See võimaldab kasutajal kasutada gateway seadmed, mis on turvalisemad: nad oskavad suhelda pilve lüüsi abil turvalise protokollid, mis nõuab tavaliselt rohkem töötlemise et emakeelena seade - termostaat - nagu annaks ise. Tsoonis Azure'i teenuste eeldame pilve Gateway esindab Azure IoT Rummu teenust.

### <a name="device-and-data-sourcesdata-transport"></a>Seadme ja andmete allikad/andmete transportimise

Käesoleva uurib läbi objektiivi ohtu modelleerimine eespool arhitektuur ja antakse ülevaade sellest, kuidas me käsitleme mõningaid omane. Keskendume ohtu mudeli põhielemente:

- Protsessid (need meie kontrolli all ja väliste üksuste)
- Side (ehk andmete edastamise)
- Hoiuruum (ehk andmete)

#### <a name="processes"></a>Protsesside

Igas kategoorias, mis on esitatud Azure IoT arhitektuur, püüame leevendada erinevad ohud on olemas andmeid eri etappides: protsessi-, side- ja ladustamine. Allpool anname ülevaate enim levinud "protsessi" kategooria, järgneb ülevaade sellest, kuidas need võiksid kõige paremini leevendada: 

**Tüssamine (S)**: ründaja võib väljavõte krüptograafilise võtme materjali seadme, tarkvara või riistvara tasandil ja hiljem juurdepääsu süsteemi alusel isiku füüsiliste või virtuaalsete seadmesse seadme võtit on võetud. Heaks näiteks on kaugjuhtimispuldid, mis omakorda Formaate ja mis on populaarne Kujeilija.

**Eitamine teenus (D)**: seade võib laskekõlbmatuks toimimise või raadiosageduste või lõikamine juhtmed häirimise kaudu suhtlemine. Näiteks Valve kaamera, mis oli selle tahtlikult koputasin välja võimu või võrgu ühendus esitab aruande andmed, üldse.

**Rikkumine (T)**: ründaja võib osaliselt või täielikult asendada seadmes töötava tarkvara võimaldab potentsiaalselt asendatud tarkvara kasutada seadet tõelise identiteedi kui võtit või holding peamised materjalid krüptograafilised vahendid olid kättesaadavad ebaseadusliku programm. Näiteks ründaja võib võimendada kaevandatud olulist materjali pealt kuulata ja suruda side teel andmed ja asendada see valeandmeid, mis on kinnitatud varastatud oluline materjal.

**Teabe avalikustamist, (I)**: kui seade on manipuleeritud tarkvara, manipuleeritud tarkvara võib potentsiaalselt põhjustada sööbiva andmete volitamata isikutele. Näiteks ründaja võib võimendada kaevandatud olulist materjali ise süstida side tee seade ja vastutava töötleja või välja gateway või pilve värav sifooni välja teabe vahel.

**Elevatsiooniga privileeg (E)**: seade, mis teeb kindlat ei saa sundida midagi muud teha. Näiteks võib avada täiesti meelitatakse ventiil, mis on programmeeritud poolenisti.

| **Osa** | **Ohtu** | **Leevendamise**                                                                                                                                                | **Riski**                                                                                                                                                                                                    | **Rakendamine**                                                                                                                                                                                                                                                                                                                                     |
|---------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Seadme        | S          | Seadme identiteedi määramine ja autentimisel seadme                                                                                                | Asendades muu seade või seadme osa. Kuidas me teame, me räägime õige seade?                                                                                           | Autentimisel seadme, transpordikihi turbe (TLS) või IPSec. Infrastruktuuri toetama kasutades eeljagatud võti (PSK), neis seadmetes, mida ei oska täis asümmeetriline krüptograafia. [OAuthi](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt) võimendav Azure AD                             |
|               | TRID       | Siduda võltsimiskindlat mehhanismid seade näiteks võimatu ekstrakti Seadme klahvid ja muud krüptograafilise materjali väga raske teha. | Oht on, kui keegi on avatud seade (füüsiline sekkumine). Kuidas me oleme kindel, et seade on rikkumata.                                                                                 | Kava on usaldusväärse platvormi mooduli (TPM) võimalus, mille abil, salvestades võtmed erilist-kiibil lülitused, mille võtmed on loetamatu, kuid ainult kasutada krüptograafilisi operatsioone, mis klahvi kuid kunagi avalikustada võti. Seadme mälu krüptimine. Juhtkonna seadme. Allkirja tähis. |
|               | E          | Võttes seade pääsuõigused. Ühekordse üldloa süsteemi.                                                                                                    | Kui seade võimaldab hotellile teostatavad tegevused põhinevad käsud kolmandatele isikutele või isegi kahjustada andurit, lubab see rünnak toiminguid ei ole kättesaadav. | Võttes ühekordse üldloa süsteemi seade                                                                                                                                                                                                                                                                                                             |
| Gateway väli | S          | Manifestidest välja värav pilve Gateway (cert vastavalt, PSK, nõude,..)                                                                           | Kui keegi võltsida välja Gateway, siis see saab esitada enda igas seadmes.                                                                                                                               | TLS RSA/PSK, IPSe, [RFC 4279](https://tools.ietf.org/html/rfc4279). Ühesugused ladustamise ja tõendamiseks põhiprobleemid seadmete üldiselt – parimal juhul on kasutada TPM-i. 6LowPAN laiendamine IPsec toetada Wireless andur võrgustike (WSN).                                                                                                              |
|               | TRID       | Kaitsta välja Gateway vastu rikkumine (TPM?)                                                                                                            | Tüssamine rünnakuid, et trikk see räägib välja gateway pilve gateway mõtlemine võib põhjustada teabe avalikustamist ja andmete rikkumise                                                             | Mälu krüptimine, TPM's, autentimine.                                                                                                                                                                                                                                                                                                              |
|               | E          | Juurdepääsu kontroll mehhanism välja Gateway                                                                                                                    |                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                        |

Siin on mõned näited ohud selles kategoorias:

Tüssamine: Ründaja võib väljavõte krüptograafilise võtme materjali seade, kas tarkvara või riistvara tasandil, ning seejärel juurdepääsu süsteemi alusel isiku füüsiliste või virtuaalsete seadmesse seadme võtit on võetud.

**Teenusetõkke**: seade võib laskekõlbmatuks toimimise või raadiosageduste või lõikamine juhtmed häirimise kaudu suhtlemine. Näiteks Valve kaamera, mis oli selle tahtlikult koputasin välja võimu või võrgu ühendus esitab aruande andmed, üldse.

**Rikkumine**: ründaja võib osaliselt või täielikult asendada seadmes töötava tarkvara võimaldab potentsiaalselt asendatud tarkvara kasutada seadet tõelise identiteedi kui võtit või holding peamised materjalid krüptograafilised vahendid olid kättesaadavad ebaseadusliku programm.

**Rikkumine**: Valve kaamera, mis näitab tühja koridori nähtava spektriosa pilt võiks olla suunatud selliste koridoris foto. Suitsu või tule andur võiks aru kedagi, kellel selle alusel tulemasin. Mõlemal juhul seade võib olla tehniliselt täielikult usaldusväärne süsteem, kuid esitab manipuleeritud teavet.

**Rikkumine**: ründaja võib võimendada kaevandatud olulist materjali pealt kuulata ja suruda side teel andmed ja asendada see valeandmeid, mis on kinnitatud varastatud oluline materjal.

**Rikkumine**: ründaja võib osaliselt või täielikult asendada seadmes töötava tarkvara võimaldab potentsiaalselt asendatud tarkvara kasutada seadet tõelise identiteedi kui võtit või holding peamised materjalid krüptograafilised vahendid olid kättesaadavad ebaseadusliku programm.
   
**Teabe avalikustamist**: kui seade on manipuleeritud tarkvara, manipuleeritud tarkvara võib potentsiaalselt põhjustada sööbiva andmete volitamata isikutele.

**Teabe avalikustamist**: ründaja võib võimendada kaevandatud olulist materjali ise süstida side tee seadme ja vastutava töötleja või välja gateway või pilve värav sifooni välja teabe vahel.

**Teenusetõkke**: seade olla välja lülitatud või muutunud režiim, kus side pole võimalik (mis on tahtlik palju tööstuslik masinaid).

**Rikkumine**: seadme saab ümber konfigureerima kontrollisüsteemi (väljaspool tuntud kalibreerimisparameetrid) teadmata olekus ja seega esitab andmed, mis võib olla valesti tõlgendatud

**Privileeg tõusu**: seade, mis teeb kindlat ei saa sundida midagi muud teha. Näiteks võib avada täiesti meelitatakse ventiil, mis on programmeeritud poolenisti.

**Teenusetõkke**: seadme saan olekusse, kui teatises ei ole võimalik.

**Rikkumine**: seadme saab ümber konfigureerima kontrollisüsteemi (väljaspool tuntud kalibreerimisparameetrid) teadmata olekus ja seega esitab andmed, mis võib olla valesti tõlgendatud.
 
**Kelmuse/rikkumine/lepingu rikkumine**: kui ei ole tagatud (mis on harva koos tarbija puldiga) ründaja saab manipuleerida seadme oleku anonüümselt. Heaks näiteks on kaugjuhtimispuldid, mis omakorda Formaate ja mis on populaarne Kujeilija.

#### <a name="communication"></a>Teatis

Vaatamisväärsuse side tee seadmed, seadmete ja välja väravaid ja gateway seade ja pilve vahel ohud. Tabelis on avatud sokleid seadme/VPN suuniseid:

| **Osa**               | **Ohtu** | **Leevendamise**                                      | **Riski**                                                                                                      | **Rakendamine**                                                                                                                                                                                                                                                                                                                                                               |
|-----------------------------|------------|-----------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Seadme IoT keskus              | KOLM KORDA ÖÖPÄEVAS        | (D) TLS (PSK/RSA), krüptida liiklust             | Pealtkuulamise või häiriv seade ja värav suhtlemine                             | Protokolli tasemel turvalisus. Kohandatud protokolle, peame mõtlema, kuidas neid kaitsta. Enamasti edastamine toimub seadme IoT keskus (seade algatab ühenduse).                                                                                                                                                                 |
| Seadme seade               | KOLM KORDA ÖÖPÄEVAS        | (D) TLS (PSK/RSA), krüptida liiklust.            | Seadmete vahelise andmete lugemine. Rikkumise andmeid. Ülekoormus seadme uued ühendused | Turvalisuse taseme protokolli (AMQP/MQTT/HTTP/CoAP. Kohandatud protokolle, peame mõtlema, kuidas neid kaitsta. DoS ohu vähendamiseks on peer seadmete pilve või välja lüüsi kaudu ja lasta tegutseda ainult klientidena elektrivõrgu poole. Selle silmitsemine võib põhjustada otsest seost eakaaslastega pärast võttes raporteerimine värav |
| Väline üksus seadme      | KOLM KORDA ÖÖPÄEVAS        | Tugev sidumine teleriga välise üksuse | Pealtkuulamise seade ühendus. Teatises häiriv seade                     | Turvaliselt sidumine NFC/Bluetooth LE seadme väline üksus. Kontrolliv operatiivne paneel seade (füüsiline)                                                                                                                                                                                                                                                  |
| Väli Gateway pilve Gateway | KOLM KORDA ÖÖPÄEVAS        | TLS (PSK/RSA), krüptida liiklust.               | Pealtkuulamise või häiriv seade ja värav suhtlemine                             | Turvalisuse taseme protokolli (AMQP/MQTT/HTTP/CoAP). Kohandatud protokolle, peame mõtlema, kuidas neid kaitsta.                                                                                                                                                                                                                                                       |
| Seadme pilve Gateway        | KOLM KORDA ÖÖPÄEVAS        | TLS (PSK/RSA), krüptida liiklust.               | Pealtkuulamise või häiriv seade ja värav suhtlemine                             | Turvalisuse taseme protokolli (AMQP/MQTT/HTTP/CoAP). Kohandatud protokolle, peame mõtlema, kuidas neid kaitsta.                                                                                                                                                                                                                                                       |

Siin on mõned näited ohud selles kategoorias:

**Teenusetõkke**: piiratud seadmed üldiselt DoS ohus kui nad aktiivselt kuulata sissetulevaid ühendusi või pealesunnitud datagrams võrgus, sest ründaja saab avatud ühenduste arv on samal ajal ja ei neid teenuse või teenus neid väga aeglaselt või seade võib toimuda pealesunnitud liiklust. Mõlemal juhul seade saab tõhusalt tuleb kasutuskõlbmatuks muudetud võrgus.

**Kelmuse, teabe avalikustamist**: piiratud seadmete ja eriotstarbelise seadmed on sageli ühe-eest-kõik turvalisuse vahendid nagu parooli või PIN-koodi, või nad tugineda täielikult usaldav võrk, st nad tagama juurdepääsu teabele, kui seade on samas võrgus, ja selle võrgu sageli vaid kaitsevad jaotatud võti. See tähendab, et kui ühise saladuse seadme või võrguga on võimalik kontrollida seadme või jälgida andmeid seadmest eraldunud.  

**Kelmuse**: ründaja võib peatada või osaliselt alistada levisaate ja paroodia koostaja (mees keset)

**Rikkumine**: ründaja võib peatada või osaliselt alistada levisaate ja saada valeandmeid 

**Teabe avalikustamist:** ründaja võib pealt kuulata eetrisse ja saada teavet ilma loata **teenusetõkke:** ründaja võib muutmata teabe levitamise jam eetrisse signaal ja

#### <a name="storage"></a>Ladustamine

Iga seadme ja välja gateway on mingisugune hoiuruum (ajutine Queuing andmeid, os image hoiuruum).

| **Osa**                            | **Ohtu** | **Leevendamise**                       | **Riski**                                                                                                                                                                                                                                                                                                                | **Rakendamine**                                                                                                                                                     |
|------------------------------------------|------------|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Seadme talletusruumi                           | TRID       | Ladustamise krüptimine, allkirjastamine palgid | Andmete ladustamine (PII andmed), koguda andmeid omavoliline lugemine. Rikkumine sabas või vahemällu käsk kontrollide tulemused. Omavoliline konfiguratsiooni või püsivara värskendus pakenditele vahemällu või järjekorda kohalikul tasandil võib viia OS ja/või süsteemi komponentide väärkasutus                                         | Krüptimine, sõnumi autentimise koodi (MAC) või digitaalallkiri. Kus võimalik, tugev juurdepääsu kontroll ressursside kättesaadavuse kontrollida nimekirjad (ACL) või muude. |
| Seadme OS image                          | TRID       |                                      | Omavoliline OS / OS osade asendamine                                                                                                                                                                                                                                                                         | Kirjutuskaitstud OS partitsiooni, allkirjastatud OS image, krüptimine                                                                                                                    |
| Gateway väli hoiuruum (järjekordi andmed) | TRID       | Ladustamise krüptimine, allkirjastamine palgid | Andmete ladustamine (PII andmed), koguda andmeid, omavoliline lugemine rikkumine sabas või vahemällu käsk kontrollide tulemused. Konfiguratsiooni või püsivara värskendus pakenditele (mõeldud seadmed või välja gateway) omavoliline vahemällu või järjekorda kohalikul tasandil võib viia OS ja/või süsteemi komponentide väärkasutus | BitLockeri                                                                                                                                                              |
| Väli Gateway OS pilt                   | TRID       |                                      | Omavoliline OS / OS osade asendamine                                                                                                                                                                                                                                                                          | Kirjutuskaitstud OS partitsiooni, allkirjastatud OS image, krüptimine                                                                                                                    |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Seadme ja sündmuse töötlemisega/cloud gateway tsooni

Pilve lüüs on süsteem, mis võimaldab serveri suhtlemise: ja vahendite või välja sinna mitu eri kohtades üle avaliku võrgu ruumi tavaliselt pilvepõhise kontroll ja andmete analüüsi süsteemi, nende süsteemide Föderatsiooni suunas. Mõnikord pilve värav võib kohe soodustama sihtotstarbeline seadmetele automaatidest nagu telefonid või tabletid. Siin kontekstis "pilve" mõeldakse viidata spetsiaalne andmetöötlussüsteemi, mis on seotud samal territooriumil ühendatud seadmed või välja väravaid ja kus operatiivsete meetmete suunatud füüsilise juurdepääsu, kuid ei pruugi "avalik pilv" infrastruktuuri.  Pilve värav võib potentsiaalselt tuleb kaardistada võrgu virtualiseerimine ülekate soojustada pilve gateway ja kõigis ühendatud seadmed ja välja sinna võrgu liiklust. Pilve gateway, ise on ei ole seadme juhtimissüsteemi ega töötlemise või hoidla seadme andmed; neid süsteemiga pilve gateway. Pilve gateway tsoon hõlmab kõik välja väravaid ja seadmed, mis on otseselt või kaudselt seotud ise pilve gateway.

Pilve gateway on peamiselt kohandatud ehitatud tarkvara jookseb kui teenus avatud lõpp mis välja gateway ja seadmed ühendada. Sellisena tuleb konstrueerida turvalisust silmas pidades. Palun järgige [SDL](http://www.microsoft.com/sdl) protsess projekteerimisel ja ehitamisel seda teenust. 

#### <a name="services-zone"></a>Teenuste tsoon

Juhtimissüsteemi (või vastutav töötleja) on tarkvaralahendus, mis ühendab seadme välja gateway, või pilve värav et kontrollida ühe või mitme seadme ja/või koguda ja/või salvestada ja/või seadme andmete esitusviisi või hilisema kontrolli eesmärgil. Süsteemid on ainus üksuste arutelule, mis võib kohe hõlbustavad suhelda inimestega reguleerimisalasse. Erandiks on vahe füüsilise kontrolli pindade seadmed, nagu lüliti, mis võimaldab isikul seade välja lülitada või muude atribuutide muutmine ja millel on funktsionaalne ekvivalent, mida saab kasutada digitaalselt. 

Vahe füüsilise kontrolli pinnad on need, kus mingit reguleerivad loogika piirab füüsilise pinnal funktsiooni nii, et samaväärne funktsioon võib algatada eemalt või remote sisend sisendi konflikte saab vältida-sellised vahendatud kontrolli pinnad on kontseptuaalselt seotud paikse juhtimissüsteemi, mis intensiivistab sama aluseks oleva funktsiooni muude kaugjuhtimise süsteem, mida seade võib siduda samal ajal. Suurimad ohud pilvandmetöötluse lugemiseks [Cloud Security Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) lehele.


## <a name="additional-resources"></a>Täiendavad ressursid

Vaadake lisateavet järgmistes artiklites:

- [SDL ohtu modelleerimine tööriist](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
- [Microsoft Azure IoT viide arhitektuur](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)
 
