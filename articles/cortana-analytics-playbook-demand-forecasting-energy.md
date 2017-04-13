<properties
    pageTitle="Cortana ärianalüüsi lahenduse malli Playbook kujutatud nõudmisel prognoosimiseks | Microsoft Azure'i"
    description="Lahendus malli Microsoft Cortana ärianalüüsi, mis aitab vajadusel kujutatud kasuliku ettevõtte prognoosimiseks."
    services="cortana-analytics"
    documentationCenter=""
    authors="ilanr9"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/24/2016"
    ms.author="ilanr9;yijichen;garye"/>

# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>Cortana ärianalüüsi lahenduse malli Playbook kujutatud nõudmisel prognoosimiseks  

## <a name="executive-summary"></a>Ametlik Kokkuvõte  

Viimase aasta Internet, asjade, alternatiivsete ja suur andmed on ühendatud luua suur võimalused kasuliku ja vaeva Domeen. Samal ajal kasuliku ja kogu sektori näinud tarbimine lamedamad tarbijate nõuavad hõlbus kontrollida oma kujutatud kasutamist. Seega kasuliku ja nutikas ruudustiku ettevõtete on väga vaja uuendada ja uuendada ise. Lisaks palju power ja kasuliku analüüsitabelite muutuvad aegunud ja kulukate säilitamiseks ja haldamiseks. Viimase aasta jooksul töötanud meeskond kokkulepete domeenis kujutatud arv. Nende kokkulepete ajal on esinenud paljudel juhtudel, kus Utiliidid või tarkvaratoode (sõltumatu tarkvara hankijad) on antud uurime jaoks tulevaste kujutatud nõudmisel prognoosi koostamiseks. Need prognoosi oma ettevõtte praegustele ja tulevastele oluline roll ja saanud Foundationi kasutamine eri juhtudel. Nendeks on lühikesed ja pikaajalise power laadi prognoosi, börsipäev, koormusetasandus, ruudustiku optimeerimine jne. Big data ja Täpsemad a Analytics (a) meetodite nt arvuti õ (ML) on oluliste võimaluste täpne ja usaldusväärne prognooside koostamiseks.  

See playbook Panime koos business ja analytical juhiseid eduka arendamise ja juurutamise kujutatud nõudmisel prognoosi koostamise lahendus. Need ettepanek juhised aitavad Utiliidid, andmeteadlaste ja andmete insenerid loomisel täielikult operationalized, pilvepõhist, vajadusel prognoosi koostamiseks lahendusi. Ettevõtetes, kes on hakanud just oma big data ja täiustatud analüüsi reisi, selline lahendus võib tähistada oma pikaajalise nutikas ruudustiku strateegia Algseemneta.

>[AZURE.TIP] Skeem, mis annab ülevaate arhitektuuri selle malli allalaadimiseks lugege [Cortana ärianalüüsi lahenduse malli arhitektuur kujutatud nõudmisel prognoosimiseks](cortana-analytics-architecture-demand-forecasting-energy.md).  

## <a name="overview"></a>Ülevaade  

Selle dokumendi hõlmab business, andmed ja Cortana ärianalüüsi kasutada, ja klõpsake kindla Azure seadme õ (koostamisel) rakendamiseks ja kujutatud prognoosi koostamiseks lahenduste kasutuselevõtu aspektidega. Dokumendi koosneb kolm põhiosa:  

1. Business mõistmine  
2. Andmete mõistmine  
3. Tehniline rakendamine

**Business mõistmine** osas kirjeldatakse business proportsioonid, tuleb arvesse võtta enne investeeringu tegemist ja mõistmiseks. Seda selgitatakse, kuidas saada business probleemi käepärast, et tagada ennustav ja seadme õpetused tõepoolest tõhus ja rakendatav. Dokumendi täpsemaks antakse ülevaade seadme õppimine ja kuidas seda kasutatakse kujutatud prognoosi koostamiseks probleemide lahendamiseks. See kirjeldab eeltingimused ja kvalifikatsioon kriteeriumide kasutamine asja. Mõned valimi kasutada juhtumite ja ettevõtte puhul on saadaval ka stsenaariumid.

Andmed on peamine komponent iga masina Õppekeskuse lahendus. **Andmete mõistmine** selle dokumendi osa hõlmab mõned andmed olulisemaid aspekte. See kirjeldab selliseid andmeid, mida on vaja kujutatud prognoosi koostamiseks, nõuded ja millised andmeallikad on tavaliselt olemas. Selgitame töötlemata andmete kasutamise ettevalmistamiseks andmete funktsioonid, mis tegelikult modelleerimine osa.

Dokumendi kolmas osa hõlmab lahenduse **Tehniline rakendamine** osa. Funktsioon matemaatika erifunktsioonid ja modelleerimine andmete teadus protsessi keskmes on seega arutatakse üksikasjalikumalt. See hõlmab mõiste veebiteenuseid, mis on pilv ennustav lahenduste kasutuselevõtu oluline vahend. Me liigendada ka tüüpilised arhitektuur operationalized lahenduse lõpuni.

Lisaks sisaldab dokumendi viide materjali täpsemaks mõistmiseks ja domeeni kasutavad.

See on oluline märkida, et me ei kavatse katta selles dokumendis põhjalikult teadus protsessi, matemaatilised ja tehniliste aspektide. Need andmed leiate [Azure'i ML dokumentatsiooni](http://azure.microsoft.com/services/machine-learning/) ja [ajaveebide](http://blogs.microsoft.com/blog/tag/azure-machine-learning/).

### <a name="target-audience"></a>Sihtrühma   
Sihtrühma selle dokumendi jaoks on äri- ja tehnilised töötajad, kes soovivad teadmisi ja mõistmise masina õ vastavalt lahenduste ja kuidas neid kasutatakse spetsiaalselt domeenis kujutatud prognoosi koostamiseks.

Andmeteadlaste saavad ka selle dokumendi paremaks mõistmiseks kõrge protsessi, mis on kujutatud prognoosi koostamiseks lahenduste kasutuselevõtu lugemise. Selles kontekstis seda saab kasutada ka luua hea võrdlusalus ja alguspunkti lisateavet ja üksikasjalikud materjali Täpsem.

### <a name="industry-trends"></a>Valdkonna trendide  
Möödunud aastate asjade, alternatiivsete ja suur andmed on ühendatud luua suur võimalusi kasuliku ja vaeva ruumi. Samal ajal kasuliku ja kogu sektorid näinud tarbimine lamedamad tarbijate nõuavad hõlbus kontrollida oma kujutatud kasutamist.

Mitme kasuliku ja nutikas kujutatud ettevõtete on antud teedrajav [nutikas ruudustiku](https://en.wikipedia.org/wiki/Smart_grid) kasutades arv juhtudel, mis muudavad kasutus loodud ruudustiku andmete kasutamine. Kasutage paljudel juhtudel toetuvad omadused ning elektritootmise: ei saa kogunenud ega salvestatud varude kõrvale. Jah, mis toodab tuleb kasutada. Utiliidid, mida soovite tõhusamaks vaja prognoosimiseks energiatarbimist lihtsalt kuna, et anda neile suurem võimalus **Saldo pakkumise ja nõudmise**, takistades kujutatud kadu, **vähendada kasvuhoonegaaside**ja juhtelemendi maksumus.

Kui kõik rääkimise kulude, on teine tähtis, mis on hind. Uue võimet võlad power vahel Utiliidid toonud vajavaid prognoosimiseks **tulevaste nõudmisel ja tulevaste elektri hind**. See aitab määratleda tootmiskoguste ettevõtetes.

Kui sõna "nutikas" kasutamiseks me tegelikult viidata saate siit saate teada, ja siis tehke prognoose koordinaatvõrgu. Saate näha, hooajaliste muutuste samuti **ette ajutine ülekoormuse olukordades ja seda automaatselt reguleerida**. Kaugühenduse teel reguleerimise tarbimine (abiga nende kasutamisel), võib käsitleda lokaliseeritud ülekoormuse olukordades. **Esmalt prognoosida, ja seejärel**, ruudustiku teeb ise nutikamalt aja jooksul.

Selle dokumendi me keskenduda kasutamise juhtudel, mis hõlmavad prognoosi koostamiseks tulevikus, teatud pere ülejäänud lühiajaline ja pikaajaline kujutatud nõudmisel. Me nendes paar kuud töötanud ja saanud mõned teadmised ja oskused, mis võimaldavad valdkonna hinde tulemusi. Kasutage muudel juhtudel käsitletakse ka dokumendi lähitulevikus.

## <a name="business-understanding"></a>Business mõistmine

### <a name="business-goals"></a>Ettevõtte eesmärkide
**Kujutatud Demo** eesmärk on tüüpilised ennustav ja seadme Õppekeskuse väga lühikese aja jooksul juurutatud lahendus. Täpsemalt meie praeguse fookus on kujutatud nõudmisel prognoosi koostamise lahenduste lubamine, et business väärtusega kiiresti aru ja kasutada. Teave selle playbook aitavad järgmised eesmärkide saavutamise kliendi.
-   Lühike kellaaeg väärtuseks masina õ põhise lahenduse
-   Võimalus laiendada pilootprojekti kasutusmall kasutamine muudel juhtudel või vastavalt nende vajadust laiem
-   Kiiresti teadmisi Cortana ärianalüüsi komplekti toode

Neid eesmärke silmas pidades, see playbook eesmärk business ja tehnika, mis aitab neid eesmärkide saavutamisel.

### <a name="power-load-and-demand-forecasting"></a>Power laadimine ja vajadusel prognoosi koostamiseks
Sektorile kujutatud võib olla mitmel viisil, mis nõudmisel prognoosi koostamiseks võib aidata business kriitiliste probleemide lahendamine. Tegelikult nõudmisel prognoosi koostamiseks pidada paljudel juhtudel core kasutamine valdkonnas alus. Üldiselt arvesse võtta kujutatud vajadusel prognooside kahte tüüpi: lühiajaline ja pikaajaline. Iga võib olla eri eesmärk ja kasutada eri lähenemine. Kahe põhilise vahe on prognoosimise Horisont, s.t vahemiku aja me oleks prognoosimiseks tulevikku.

#### <a name="short-term-load-forecasting"></a>Lühike Termini laadi prognoosi koostamiseks
Kujutatud nõudmisel raames lühike Termini laadimine prognoosi koostamiseks (STLF) on määratletud liidetud laadi, mis on prognoositud lähitulevikus erinevate osade ruudustiku (või kogu koordinaatvõrgu). Sellega määratletakse lühiajaliselt olema aega kokku vahemikus 1 tund 24 tundi. Mõnel juhul on võimalik ka Horisont 48 tundi. Seetõttu on väga levinud kasutamise juhul ruudustiku STLF. Siin on mõned näited STLF juhitud kasutamise juhtudel:
-   Pakkumise ja nõudmise tasakaalustamiseks
-   Power kauplemise tugi
-   Market tegemine (säte power hind)
-   Koordinaatvõrgu tegevuse optimeerimine
-   [Nõudmisel vastus](https://en.wikipedia.org/wiki/Demand_response)
-   Maksimumi nõudmisel prognoosi koostamiseks
-   Nõudmisel juhtimise
-   Laadi tasakaalustamiseks ja ülekoormuse vältimine
-   Pikaajalist laadi prognoosi koostamiseks
-   Viga ja normaalne tuvastamine
-   Tippväärtus piiramine/ühtlustamise. 

STLF mudeli põhinevad enamasti läheduses varem (viimase päeva või nädala) andmed ja kasutage prognoositud temperatuuri on oluline ennustaja. Prognoosi koostamise tund temperatuuri saamise ja kuni 24 tundi on muutumas väiksem probleemiks nüüd päeva. Need mudelid on vähem tundlik hooajaline andmemustreid ja -pikaajaline tarbimine trende.

SLTF lahenduste tõenäoliselt luua suure hulga ennustamine kõned (hooldustaotlused) Kuna need on on ilmnenud tunnis ja mõnel juhul suurema sagedusega. See on väga levinud siirdamiseks, kus iga üksiku alajaama või muunduri on esitatud arvuna autonoomse mudeli ja ennustamine maht eelnevast on veelgi kuvamiseks.

#### <a name="long-term-load-forecasting"></a>Pikaajalist laadi prognoosi koostamiseks
Eesmärk on pikk Termini laadi prognoosi koostamiseks (LTLF) on prognoosi koostamine power vajadusel koos tähtaeg, mis on vahemikus 1 nädal mitme kuu (ja mõnel juhul aastate arv). Enamasti rakendatakse see hulga Horisont kavandamise ja investeeringu kasutamine juhtudel.

Pikaajaline stsenaariumi puhul, on oluline on kõrge kvaliteediga andmeid, mis hõlmab ajavahemiku mitu aastat (3 aastat). Need mudelid tavaliselt ekstrakti andmed hooajalisus mustrite ja kasutada näiteks välise predicators ilm ja kliima mustrite nimega.

See on oluline aitavad täpsustada, mida enam prognoosimise Horisont on, vähem täpne prognoosi võib olla. Seetõttu on oluline, andes koos tegelik prognoosi, mis võimaldab inimestel arvestada võimalike variatsioonide nende kavandamise protsessi mõned usaldusvahemik.

Kuna enamasti plaanib LTLF tarbimine stsenaarium, võib oodata palju alumise ennustamine mahud (võrreldes STLF). Me tavaliselt näeksid neid prognoose visualiseeringu tööriistad, nt Excelis või PowerBI lisada ja kasutada kasutaja käsitsi.

### <a name="short-term-vs-long-term-prediction"></a>Lühike termin ja vana Termini ennustamine
Järgmises tabelis võrreldakse STLF ja LTLF kõige olulisem atribuutide osas:

|Atribuut|Lühiajaline laadi prognoos|Laadi prognoosi pikaajalist|
|---|---|---|
|Funktsioon FORECAST Horisont|1 tund 48 tundi|1 kuni 6 kuud või rohkem|
|Andmete granulaarsus|Kord tunnis|Tunni või iga päev|
|Tüüpilised kasutamise juhtudel|<ul><li>Nõudmise/pakkumise tasakaalustamiseks</li><li>Valige tunni prognoosi koostamiseks</li><li>Nõudmisel vastus</li></ul>|<ul><li>Pikaajaliselt kavandamise</li><li>Koordinaatvõrgu varad kavandamine</li><li>Ressursi plaanimine</li></ul>|
|Tüüpilised aitavad prognoosida|<ul><li>Päeva ja nädala</li><li>Päeva tunni</li><li>Kord tunnis temperatuur</li></ul>|<ul><li>Aasta kuu</li><li>Kuu</li><li>Pikaajaline temperatuur ja kliima</li></ul>|
|Ajalooliste andmevahemiku|2-3 aastat väärtuses andmed|Viis kuni 10 aastat väärtuses andmed|
|Tüüpiline täpsus|MAPE * 5% või alla|MAPE * 25% või alla|
|Prognoosi koostamise sagedus|Iga tund või kord 24 tunni jooksul|Kui kvartali, kuus või aastas toodeti|
\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – keskväärtus Keskmine protsendimäärana tõrge

Nagu näha selle tabeli, on väga oluline eristada lühike ja prognoosi koostamiseks stsenaariumid, nagu need tähistavad erinevate ärivajaduste ja võib-olla erinevad juurutus- ja tarbimine mustrite pikaajaliselt.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Näide kasutamine juhtum 1: eSmart Systems-ülekoormuse optimeerimine
[Nutikas ruudustiku](https://en.wikipedia.org/wiki/Smart_grid) olulist on dünaamiliselt ja pidevalt optimeerimine ja muutmisega tarbimine mustrite jaoks kohandada. Energiatarbimist võivad mõjutada lühiajaline muudatused, mis on peamiselt põhjustatud temperatuur (*nt*rohkem power kasutatakse air tingimus või Küte). Samal ajal energiatarbimist mõjutab ka pikaajalisi trende. Need võivad sisaldada hooajalisus efektid, riigipühadel, pikaajaline tarbimine ja isegi economic tegureid, nt tarbija index, oil hind ja SKP.

Sellisel juhul kasutamine [eSmart](http://www.esmartsystems.com/) kalendriüksusi pilvepõhist lahenduse, mis võimaldab prognoosimine ülekoormuse olukord ruudustiku klõpsake mis tahes antud alajaama kalduvust juurutamine. Eelkõige eSmart kalendriüksusi alajaamade tõenäoliselt ülekoormuse järgmise tunni jooksul nii, et kohe toimingu võib võtta, et vältida või lahendada olukord tuvastamiseks.

Täpselt ja kiiresti läbimiseks ennustamine nõuab kolme prognoosmudelite rakendamiseks tehke järgmist.
-   Pikk Termini mudelit, mis võimaldab prognoosi koostamiseks energiatarbimist iga alajaama järgmise mõne nädala või kuu jooksul.
-   Lühiajaline mudelit, mis võimaldab ennustamine ülekoormuse olukorra antud alajaama järgmise tunni jooksul
-   Temperatuur mudelit, mis pakub prognoosi koostamiseks tulevaste temperatuuri üle mitme stsenaariumid

Pikaajaline mudeli eesmärk on alajaamad nende kalduvus ülekoormuse (anda oma power läbilaskevõimet) ajal järgmise nädala või kuu alusel järjestada. See võimaldab alajaamade, et lühiajaline ennustamine sisend lühikese loendi loomine. Temperatuur on pikaajaline mudeli on oluline ennustaja, on vaja pidevalt mitme stsenaariumi temperatuur prognooside ja kanali need sisendina pikaajalise mudeli sisse. Lühiajaline prognoosi kutsumisel seejärel prognoosida, millised alajaama tõenäoliselt ülekoormuse üle järgmise tunni.

Lühiajaline ja pikaajaline mudelid on juurutatud eraldi iga alajaama kohta. Seega need mudelid praktiline täitmine nõuab olulisel korraldamise. Saada lühiajaliselt suurema ennustamine täpsuse, täpsema mudeli eesmärk on jätkuvalt iga päev tund. Kõik need mudelid täidetakse iga tunni ja täitmise piisavalt aega vastata ja võtta ennetavad toimingud vajadusel mõne minuti jooksul lõpule. Selle saidikogumi mudelite ajakohasuse perioodiliste ümberõppe abil värskeimad andmed.

Sel juhul kasutamise kohta lisateabe saamiseks leiate [siit](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Kasutage juhul kvalifikatsioon kriteeriumid – eeltingimused
Peamised Cortana ärianalüüsi on selles võimas, et juurutada ja mastaapimiseks masina õ kesksele lahendusi. See on mõeldud tugiteenuste tuhandeliste prognoose, mis on üheaegselt täita. See automaatselt mastaapida, muutuv tarbimine muster täita. Lahenduse fookus on seega täpsuse ja arvutuslikke jõudlus. Näiteks kasuliku ettevõte on huvitatud toodavad täpne kujutatud nõudmisel tund ja iga päev tund. Teisalt, oleme vähem huvitatud, miks prognoositakse järele, kui see on küsimusele vastamiseks (mudel ise hoolitseb mis).

Seetõttu on oluline, et kõik juhtumid ja äriprobleemidest tõhusalt lahendada abil arvuti õ mõista.

Cortana ärianalüüsi ja arvuti koolitus võib olla väga tõhus antud business probleemi lahendada, kui järgmised tingimused on täidetud:
-   Probleemi lahendamiseks käsitsi business on **sõnastikupõhine** laadi. Sõnastikupõhise kasutamine puhul näiteks on kasuliku ettevõtte, mida soovite prognoosida power koormus antud alajaama järgmise tunni jooksul. Teisalt, analüüsida ja järjestamine ajalooliste nõudmisel juhtide oleks **kirjeldav** laad ja seetõttu vähem korral.
-   Pole selge **tee toimingu** võtta, kui selle tekstiennustus pole saadaval. Näiteks saate prognoosimine ülekoormuse alajaama sisse järgmise tunni jooksul käivitamine aktiivse tegevuse, laadi, mis on seotud selle ühendatud ja seega vältida potentsiaalselt ülekoormuse.
-   Kasutamine juhul tähistab **tüüpilised tüüpi probleem** selline et kui lahendatud, siis saate valmistada lahendada muid sarnaseid kasutamine juhtudel.
-   Kliendi saate seada **ja kvantitatiivsete eesmärkide saavutamise** näidata eduka lahenduse juurutamine. Näiteks oleks hea kvantitatiivne eesmärk kujutatud nõudmisel Forecast täpsuse lävi (*nt*kuni 5% tõrge on lubatud) või kui prognoosimine alajaama ülekoormuse seejärel arvutustäpsuse (true positiivsed rate) ja tagasivõtmise (true positiivsed ulatus) peaks olema antud kõrgemal. Need eesmärgid tuleks saadud kliendi ettevõtte eesmärke.
-   Pole selge **kataloogiintegreerimise stsenaariumi** ettevõtte töövoos. Näiteks saate alajaama laadi prognoosi integreeritud ruudustiku juhtimiskeskus ülekoormuse vältimise tegevuste lubamiseks.
-   Kliendil on **andmete piisavalt kvaliteediga** toetamiseks kasutamine täheregistrit (vt rohkem jaotises järgmise **Andmete kvaliteedi**, see playbook) kasutamiseks valmis.
-   Kliendi hõlmab pilve kesksele andmete arhitektuuri või **seadme pilvepõhist Õppekeskuse**, sh Azure ML ja muud Cortana ärianalüüsi komplekti komponendid.
-   Klient soovib luua **mõne lõpuni andmevoo** selle vahendid kohaletoimetamise andmete pilve pidevalt, ja valmis **käivitama** lahendus.
-   Kliendi on valmis **suunata ressursid** , kes on aktiivselt algse katseprojekti rakendamisel nii, et teadmised ja omandiõiguse lahendus saab üle kanda edukaks klient.
-   Kliendi ressursside peaks olema **kvalifitseeritud andmete professionaalse**, parim andmete teadlane.

Kutse ülaltoodud kriteeriumide alusel kasutamine juhul võivad oluliselt parandada edukust puhul kasutada ja luua hea Sillanpää edaspidiseks kasutamiseks juhtudel rakendamiseks.

### <a name="cloud-based-solutions"></a>Pilvepõhine lahendused
Cortana ärianalüüsi komplekti Azure on integreeritud keskkond, mis asub pilveteenuses. Täiustatud analüüsi lahenduse cloud keskkonnas juurutamise hoiab olulisi eelised ettevõtetele ja samal ajal võib keskmise suur ettevõtted, mis siiski kasutada kohapealne seda muuta lahendusi. Sektorile kujutatud on selge suund järk-järgult migreerimise toimingute pilve. See trend läheb käima koos smart grid arendamine nagu eespool **Valdkonna trende**. Nagu see playbook keskendub pilvepõhist lahenduse kujutatud domeen, on oluline selgitada eeliste ja muud kaalutlused pilvepõhist lahenduse juurutamine.

Võib-olla pilvepõhist lahenduse suurim eelis on hind. Lahendusena kasutab cloud juurutatud komponendid, ei ole algusest peale kulude või sellega seotud OMAHIND (müüdud kauba kulude) komponent kulude. Mida tähendab, et ei ole vaja investeerida riistvara, tarkvara ja IT hooldus ja seetõttu on oluliselt vähendada business risk.

Teine oluline eelis on pensionitingimustega maksumus struktuuri pilvepõhise lahendusi. Pilvepõhine serverid IT-või saate juurutada ja mastaabitud vajadusel lihtsalt alusel. See tähistab pilvepõhist lahenduse maksumus tõhusust eeliseid.

Lõpuks on ei ole vaja investeerida IT hooldustööd või tulevaste infrastruktuuri nagu kõik see on osa pilvepõhist pakkumine. Selles osas Cortana ärianalüüsi Suite sisaldab kõige paremini klassi teenuste ja selle tegevuskava hoiab areneb. Uusi funktsioone, komponendid ja võimalusi pidevalt juurutatud ja muutuda.

Mõni ettevõte, mis on hakanud just selle ülemineku pilve me on väga soovitab kasutada järk-järgult lähenemisviisi rakendades pilv migreerimise Teevaade: kaart. Me loomise turvalisuses kindel, et see playbook kirjeldatud juhtudel kasutamine tähistada Utiliidid ja ettevõtetele kujutatud domeen, suurepärane võimalus juhtimine ennustav lahenduste pilveteenuses.

#### <a name="business-case-justification-considerations"></a>Ettevõtte juhul rööpjoondus kaalutlused
Paljudel juhtudel võib kliendi huvitatud tegemise business rööpjoondus pilvepõhist lahenduse ja seadme õ on olulised komponendid antud kasutamine juhtumi. Erinevalt lahenduse kohapealne puhul pilvepõhist lahenduse esmast maksumus komponent on minimaalne ja enamik maksumus elemendid on seostatud tegelik kasutus. Kui tegemist on kujutatud prognoosi koostamiseks Cortana ärianalüüsi komplekti lahenduse juurutamine, mitmes teenuses võib olla integreeritud ühe levinud kulude struktuuri. Näiteks andmebaasid (*nt*SQL Azure'i) saab kasutada töötlemata andmete talletamiseks ja seejärel tegelik prognooside Azure'i ML kasutatakse majutada prognoosimise teenuseid. Selles näites hõlmata kulude struktuuri salvestus- ja ülekande komponendid.

Teisalt, üks peaks olema hea mõistmine business väärtus opsüsteem on kujutatud nõudmisel prognoosi koostamiseks (pika või lühikese Termini). Tegelikult on oluline ettevõtete väärtust iga prognoosi koostamise toimingu. Näiteks täpselt prognoosi koostamiseks power laadi järgmise 24 tunni jooksul saate takistada ületootmine või aitab vältida ülekoormuse võrku ja seda saab arvudes osas rahandus säästude iga päev.

Lihtsa valemi arvutamiseks rahandus nõudmise prognoosimiseks lahendus oleks: ![lihtsa valemi arvutamiseks rahandus nõudmise prognoosi koostamise lahendus](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Kuna Cortana ärianalüüsi pakub pensionitingimustega hinnakirjad mudeli, ei ole vaja võtmiseks fikseeritud omahind komponent, mida soovite seda valemit. Seda valemit saab arvutada iga päev, iga kuu või aasta alusel.

Praeguse Cortana ärianalüüsi komplekti ja Azure ML paketid leiate [siit](http://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Lahendusprotsessi arengu
Prognoosi koostamiseks lahenduse tähendab tavaliselt 4 etapist, mis kõik teeme on kujutatud nõudmisel elutsüklit kasutada pilvepõhist tehnoloogiad ja teenuste Cortana ärianalüüsi komplekti.

See on kujutatud järgmisel joonisel:

![Tsükli Smart Grid](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

Lõige kirjeldatakse selles 4 järgmist toimingut:

1.  **Andmete kogumine** – mis tahes täiustatud analüüsi põhise lahenduse tugineb andmete (vt teemat **Andmete mõistmine**). Eelkõige sellest, kui tegemist on ennustav ja prognoosi koostamiseks, me toetuvad poolelioleva, dünaamiline andmete liikumist. Kujutatud nõudmisel prognoosi koostamiseks, andmed võib olla pärit otse kasutamisel või kohapeal andmebaasis juba kokku liita. Me toetuvad ka muud andmed nt ilm ja temperatuur välistest allikatest. See pidev vool andmete orkestreerinud need ajastatud ja talletatud. [Azure'i andmed Factory](http://azure.microsoft.com/services/data-factory/) (ADF) on meie peamised tööhobune täites selle ülesande jaoks.
2.  **Modelleerimine** – jaoks täpne ja usaldusväärne prognoosib, tuleb töötada (rong) ja säilitada, et kasutada varasemaid andmeid ja ekstraktib põnevamaks ja sõnastikupõhise mustrite andmeid suur mudel. Ala seadme õ (ML) on kasvanud kiiresti abil täpsemate algoritmide rutiinselt välja töötatud. Azure'i ML Studio pakub hea kasutusvõimalused, mis aitab kasutada kõige täpsemalt ML algoritmide jooksul lõpule viidud töövoog. See töövoog on kujutatud on intuitiivne vooskeemi ja sisaldab andmete ettevalmistamise, funktsioon eraldamine, modelleerimine ja mudeli hindamise. Kasutaja saab eraldada sadu erinevate mudelite, mis sisalduvad selles keskkonnas. Selle etapi lõpus on andmete teadlane töötamise mudelit, mis on täielikult hinnatud ja juurutamiseks valmis.

    Järgmisel joonisel on tüüpilised töövoo näide:

    ![Töövoo modelleerimine](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)

3.  **Juurutamise** – koos töötamine mudeli käsitsi, on järgmiseks võimaluseks juurutamise. Siin on mudeli ümber veebiteenus, mis seab rahulik API-ga, mida saate kasutada samaaegselt erinevate tarbimine klientide Interneti kaudu. Azure'i ML pakub lihtsat viisi juurutamine mudeli otse Azure'i ML Studio nupp ühe klõpsuga. Kogu juurutamise protsessi juhtub kaitstud. See lahendus saab automaatselt mastaapida nõutav tarbimine täita.

4.  **Tarbimine** – selles etapis tegelikult teeme koostada prognoose prognoosimise mudeli kasutamine. Tarbimine saate juhtida kasutaja rakendus (*nt*, armatuurlaud) või otse kaudu näiteks operatsioonisüsteemist nõudmise/pakkumise tasakaalustamiseks süsteemi või koordinaatvõrgu optimeerimine lahendus. Kasutage mitut juhtumit saate juhtida ühe mudel.

## <a name="data-understanding"></a>Andmete mõistmine
Pärast kataks business kaalutlused (vt **Business mõistmine**) on kujutatud nõudmisel prognoosimise lahenduse, meil on nüüd valmis arutada andmete osa. Mis tahes ennustav lahenduse tugineb usaldusväärsed andmed. Kujutatud nõudmisel prognoosi koostamiseks, me toetuvad ajalooliste andmed koos granulaarsus erinevaid tasemeid. Neid kasutatakse ajaloolisi andmeid. See läbivad, kus andmed teadlane tuvastab aitavad prognoosida (nimetatakse ka funktsioone), mis võib kasutusele võtta mudelit, mis loob lõpuks nõutav prognooside hoolikalt analüüsida.

Ülejäänud selles jaotises kirjeldatakse eri juhiseid ja kaalutluste kohta andmeid ja kuidas tuua kasutusvalmis.

### <a name="the-model-development-cycle"></a>Mudeli arengutsükli
Tootva hea prognoosi koostamiseks mudelite jaoks on vaja mõned ettevaatlik ettevalmistamine ja kavandamine. Kõrvaldamine modelleerimine protsessi üheks mitme juhiseid ja keskendudes sammhaaval korraga võib oluliselt parandada kogu protsessi tulemusi.

Järgmine diagramm näitab, kuidas modelleerimine protsess võib jaotada mitme juhiseid.

![Mudeli arengutsükli](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Nagu näha tsükkel koosneb kuus näpunäidet:
-   Probleemi sõnastus
-   Andmete manustamisest ja andmete uurimine
-   Andmete ettevalmistamine ja funktsioon matemaatika erifunktsioonid
-   Modelleerimine
-   Mudeli hindamine
-   Arengu

Ülejäänud selles jaotises kirjeldatakse üksikute juhiseid ja üksused, millega tuleks arvestada iga toimingu juures.

### <a name="problem-formulation"></a>Probleemi sõnastus
Saame uurida, tuleks enne rakendamist ennustav lahendus kõige tähtsamate sammuna probleemi kujundamisel. Siin me muudaks business probleemi ja lagunevad selle kindlatele elementidele, mille abil andmeid ning modelleerimine meetodite abil lahendada. See on hea tava koostada probleemi soovime vastata küsimustele kogumina. Siin on mõned võimalikud küsimusi, millele võidakse rakendada piires kujutatud nõudmisel prognoosi koostamiseks.
-   Mis on oodatud koormus kuvatakse üksikute alajaama järgmise tund või päev?
-   Mis kellaajal minu ruudustiku kogemus maksimaalsest?
-   Kuidas tõenäoliselt minu ruudustiku säilitada oodatud tippväärtus laadi?
-   Kui palju power peaks seda iga päeva jooksul luua?

Koostada neile küsimustele võimaldab meil keskenduda saada õigeid andmeid ja rakendamise lahenduse, mis on täielikult joondatud business probleemi käes. Lisaks me siis määratud mõned olulisemad mõõdikud võimaldavad meil hindab mudel. Näiteks kui täpne prognoosi tuleks ja mis on lahtrivahemik, vea, mille ikkagi nõustub ettevõtte?

### <a name="data-sources"></a>Andmeallikad
Tänapäevane nutikas ruudustiku abil kogutud andmete erinevate osade ja komponentide ruudustiku. Need andmed esindavad eri omadusi toiming ja kasutamine power võrguoperaatorile. Piires on kujutatud nõudmisel prognoos, piiravad me andmeallikad, mis kajastab tegelik nõudmisel arutelu. Üks oluline allikas energiatarbimist on kasutamisel. Kogu maailmas Utiliidid kiiresti rakendades kasutamisel nende tarbijate jaoks. Kasutamisel kirje tegelik energiatarbimist ja relee pidevalt uuesti kasuliku ettevõtte andmed. Andmete kogumise ja saadetud tagasi kindla intervalliga, iga 5 minuti vahemikus 1 tund. Täpsemate kasutamisel saate kontrollida ja tegelik tarbimine on koduste jooksul tasakaalustamiseks programmeeritud kaugühenduse teel. Nutikas arvesti andmed on üsna usaldusväärne ja sisaldab ajatempel. Mis on olulised komponent nõudmisel prognoosi koostamise jaoks muudab. Arvesti andmeid saab liita (ületa) soovitud võrgutopoloogia eri tasemel: *muunduri, alajaama, piirkond jne*. Me saate valige seda mudeli prognooside koostamiseks vajalik koondamine tase. Näiteks kui kasuliku ettevõtte prognoosimiseks tulevaste koormus iga selle ruudustiku alajaamade seejärel kõik meetrit andmeid saab iga üksiku alajaama kokku liita ja kasutada sisendina prognoosimise mudeli. Me viidata kasutamisel sisemise andmeallikana.

Usaldusväärne kujutatud nõudmisel prognoosi sõltuvad ka muude väliste andmeallikatega. Üks oluline tegur, mis mõjutab energiatarbimist on ilm või täpsemalt temperatuur. Varasemad andmed kuvatakse tugev korrelatsiooni väljaspool temperatuur ja energiatarbimist. Kuum suvi päeva jooksul tarbijad veenduge, et kasutada nende kliimaseadmete ja lülitage talv küttesüsteemide ajal. Seetõttu on usaldusväärsest allikast ajalooliste temperatuuride asukohas ruudustiku võti. Lisaks me ka toetuvad täpne prognoosi temperatuuri kui ennustaja energiatarbimist.

Muude väliste andmeallikatega aitab ka hoone kujutatud nõudmisel prognoosi koostamise mudelid. Nendeks pikaajaline kliimamuutused, ökonoomne registrid (*nt*SKP) ja teised. Selles dokumendis me ei sisalda neid muudest andmeallikatest.

### <a name="data-structure"></a>Andmestruktuur
Pärast nõutud andmeallikate tuvastamine, soovime tagamaks, et lähteandmed, mis on kogutud õigete andmete funktsioonid. Usaldusväärne nõudmisel prognoosi koostamise mudeli koostamiseks, oleks vaja läbi tagamaks, et kogutud andmete andmete elemente, mis aitab prognoosida tulevaste nõudmisel. Siin on mõned põhilised nõuded on toorandmetega andmestruktuur (skeem).

Töötlemata andmetega koosneb ridu ja veerge. Iga mõõde on esitatud andmed ühe reana. Iga rea andmed sisaldab mitme veeru (nimetatakse ka funktsioone või väljad).

1.  **Ajatempliga** – ajatempli välja tähistab kulunud aja mõõtmine salvestati kui. See peaks täitma mõnda levinud kuupäeva/kellaaja vormingud. Nii kuupäeva ja kellaaja osa tuleks lisada. Enamikul juhtudel ei ole vaja registreerida kuni teise taseme granulaarsus korda. See on oluline, kus andmed on salvestatud ajavööndi määramine.
2.  **Arvesti ID** - väli tuvastab arvesti või mõõtühikute seade. See on kategooriline muutuja ja võib olla numbrit ja märkide kombinatsioon.
3.  **Tarbimine väärtuse** – see on tegelik tarbimine veebisaidil kuupäev/kellaaeg. Tarbimine saab mõõta kWh (kilovatt) või mõne muu eelistatud üksused. See on oluline Pange tähele, et mõõtühik peab jääma ühtsete üle kõik mõõdud andmeid. Mõnel juhul tarbimine saab esitada üle 3 power etapist. Sel juhul meil vaja koguda sõltumatu tarbimine etapid.
4.  **Temperatuur** – temperatuur on tavaliselt kogutud sõltumatu. Siiski peaks olema ühilduvad tarbimine andmetega. See peaks sisaldama eespool kirjeldatud ajatempli, mis võimaldab tegelik kulu andmetega sünkroonida. Temperatuuri väärtust saab määrata Celsiuse või Fahrenheiti, kuid peaks jääma ühtsete üle kõik mõõdud.
5.  **Asukoht –** Välja asukoht on tavaliselt seotud koht, kus kogutud andmete temperatuur. Ta saab esindatud arvuna sihtnumber või laiuskraad/pikkuskraad (Läti/pikk) vormingus.

Järgmises tabelis on hea tarbimine ja temperatuur andmevorming näited:

|**Kuupäev**|**Aeg**|**Arvesti ID**|**Etapp 1**|**Etapp 2**|**Etapp 3**|
|--------|--------|------------|-----------|-----------|-----------|
|7/1/2015|10:00:00|ABC1234     |7.0        |2.1        |5.3        |
|7/1/2015|10:00:01|ABC1234     |7.1        |2.2        |4.3        |
|7/1/2015|10:00:02|ABC1234     |6.0        |2.1        |4.0        |

|**Kuupäev**|**Aeg**|**Asukoht**|**Temperatuur**|
|--------|--------|-------------|---------------|
|7/1/2015|10:00:00|11242        |24,4           |
|7/1/2015|10:00:01|11242        |24,4           |
|7/1/2015|10:00:02|11242        |24,5           |

Nagu eespool näha, sisaldab selle näite 3 erinevate väärtuste tarbimine seotud 3 power etapist. Samuti võtke arvesse, kuupäeva ja kellaaja väljad on eraldatud, aga ka kombineerimiseks üheks veeruks. Sel juhul asukoht veerg on esindatud 5-kohaline sihtnumber vorming ja vormingus Celsiuse temperatuuriskaala.

### <a name="data-format"></a>Andmete vormindamine
Cortana ärianalüüsi komplekti toetab kõige levinum andmevormingute, näiteks CSV, JSON, TSV *jne*. On oluline, et andmete vorming jääb ühtsete kogu projekti elutsükli.

### <a name="data-ingestion"></a>Andmete manustamisest
Kuna kujutatud nõudmisel prognoosi prognoositakse sageli ja pidevalt, me veenduge, et töötlemata andmete pääseb vabalt ühtlase ja usaldusväärsed andmed lisamise protsessi abil. Lisamise protsessi, tuleb tagada selle toorandmetega on vajalik aeg prognoos jaoks saadaval. Mida tähendab, et andmete manustamisest sagedus olema suurem kui prognoosimise.

Näide: kui meie nõudmisel prognoosimise lahenduse tekitaks uue prognoosi juures 8.00 iga päev, siis tuleb tagada, et kõik andmed, mida on kogutud viimase 24 tunni jooksul on täielikult märkimisväärselt, kuni selle punkti ning isegi on sisaldavad viimase tunni andmete.

Saavutamiseks, et Cortana ärianalüüsi Suite pakub toetavad usaldusväärsed andmed lisamise protsessi erinevatel viisidel. See arutatakse **juurutamise** jaotises selle dokumendi.

### <a name="data-quality"></a>Andmete kvaliteedi
Lähteandmed allika läbimiseks usaldusväärset ja täpset nõudmisel prognoosi koostamiseks vajalik peab vastama mõne lihtsa andmete kvaliteedi kriteeriumid. Kuigi täpsemalt statistiliste meetodite abil saab hüvitamine mõned võimalikud andmete kvaliteedi probleemi peame veel tagada, et saaksime ületavad mõned andmed kvaliteedi lävi kui söömisega uute andmete. Siin on mõned kaalutlused toorandmetega kvaliteedi.
-   **Puuduvad väärtuse** – see viitab olukord kindla ei kogutud. Selle lihtsa nõude siin on, et puuduvad väärtuse määr ei tohi olla suurem kui 10% mis tahes antud aja jooksul. Juhul ühe väärtuse pole see peaks tähistatud eelmääratletud väärtuse kasutamine (nt: "9999") ja mitte 0, mis võiks olla lubatud mõõde.
-   **Mõõtühikute täpsuse** – tegelik väärtus tarbimis- ega temperatuur tuleks täpselt registreerida. Ebatäpne mõõtmed toodavad ebatäpsete prognooside. Tavaliselt mõõtühikute viga peaks olema väiksem kui 1% väärtus true.
-   **Aja mõõtmine** – see on nõutav, et kogutud andmete tegelik ajatempli ei erine rohkem kui 10 sekundi true aeg tegelik mõõtühikute suhtes.
-   **Sünkroonimise** – mitmest andmeallikast kasutamisel (*nt*, tarbimine ja temperatuur) peame tagama, et seal on pole aja sünkroonimise probleemid nende vahel. See tähendab, et ei tohi kogutud ajatempli kahe sõltumatu andmeallikatest aja vahe rohkem kui 10 sekundit.
-   **Latentsus** - nagu eespool **Andmete manustamisest**, me sõltuvad usaldusväärsed andmed paanivoog ja manustamisest protsess. Kontrolli peame tagama, et me kontrollida andmete latentsus. See on määratud kellaaja vahe tegelik mõõtühikute võeti vastu ja aja, mis see on laaditud Cortana ärianalüüsi komplekti salvestusruumi ja kasutamiseks valmis. Lühiajaline koormus prognoosi koostamiseks kokku latentsus ei tohi olla suurem kui 30 minutit. Pikaajaline koormus prognoosi koostamiseks kokku latentsus ei tohi olla suurem kui 1 päeva.

### <a name="data-preparation-and-feature-engineering"></a>Andmete ettevalmistamine ja funktsioon matemaatika erifunktsioonid
Kui soovitud toorandmetega on sissevõetava (vt **Andmete manustamisest**) ja turvaliselt salvestatud, on töötlemiseks valmis. Andmete ettevalmistamise faasis põhimõtteliselt võttes töötlemata andmete ja teisendamine (muutes ümberkujundamine) selle etapi modelleerimine vormile. Mis võib sisaldada nagu abil toorandmetega veerg on tegelik mõõdetud väärtusega, standardne väärtused, keerukamaid nagu [aja maha](https://en.wikipedia.org/wiki/Lag_operator)ja teistega koos. Äsja loodud andmete veerud on edaspidi andmete funktsioonid ja need loomisel nimetatakse funktsiooni matemaatika erifunktsioonid. Selle protsessi lõpus meil uue andmehulga, mis on tuletatud töötlemata andmete ja kasutada modelleerimine. Lisaks andmete ettevalmistamise faasis peab Olge ettevaatlik puuduvate väärtuste (vt teemat **Andmete kvaliteedi**) ja neid kompenseeri. Mõnel juhul meil ka vaja tagamaks, et kõik väärtused on esindatud sama skaala andmete normaliseerimine.

Selles jaotises on loetletud mõned levinud andmete funktsioonid, mis sisalduvad on kujutatud nõudmisel prognoos mudelid.

**Aja juhitud funktsioonid:** Need funktsioonid on tuletatud kuupäeva/ajatempli andmed. Need on ekstraktimist ja ümber kategooriline funktsioone, näiteks:
-   Aja päeva – see on tunni päeva, mis võtab väärtusi vahemikus 0 kuni 23
-   Päeva nädala – see tähistab soovitud nädalapäeva ja võtab väärtused 1 (pühapäev) kuni 7 (laupäev)
-   Päeva kuu – see tähistab tegelik kuupäev ja võib võtta väärtused 1 – 31
-   Kuu aasta – see tähistab kuu ja võtab väärtused 1 (jaanuar) kuni 12 (detsember)
-   Nädalavahetuse – see on kahendväärtus funktsioon, mis võtab väärtused 0 nädalapäevade või 1 nädalavahetuse jaoks
-   Pühade – see on kahendväärtus funktsioon, mis võtab väärtused 0 tavaline päev või 1 puhkus
-   Fourier tingimused – Fourier tingimused on kaalu, mis on tuletatud ajatempli ja kasutatakse jäädvustada hooajalisus (tsüklit) andmed. Kuna me võib olla mitu seasons meie andmete vajame mitme Fourier tingimustel. Näiteks võib vajadusel väärtused on aasta-, nädala- ja iga päev seasons/tsüklit, mille tulemuseks 3 Fourier terminid.

**Sõltumatu mõõtühikute funktsioonid:** Sõltumatu funktsioonid kaasata kõik soovime kasutada prognoosida meie mudeli andmete elemente. Siin me välistada meil vaja prognoosida sõltuvad funktsiooni.
-   Lag funktsiooni – need on nihutatud kellaaja väärtuste tegelik nõudmisel. Näiteks lag 1 funktsioonid korraldab vajadusel väärtus eelmise tunni (eeldades, et ühe tunni andmete) praeguse ajatempli suhtes. Samuti saame lisada lag 2, 3, viivitusega *jne*. Tegelik kombinatsiooni lag funktsioonid, mida kasutatakse määratakse etapis modelleerimine mudeli tulemuste hindamine.
-   Pikaajaliselt trendid – see funktsioon tähistab lineaarse kasvu nõudmisel aastat.

**Sõltuv funktsioon:** Sõltuvad funktsioon on veeru andmete, mille soovime prognoosida meie mudel. [Kuuluva masina Õppekeskuse](https://en.wikipedia.org/wiki/Supervised_learning), tuleb esmalt koolitada mudeli sõltuvad funktsioonide abil (mis on nimetatud ka siltidena). See võimaldab mudeli mustrite andmed sõltuvad funktsiooniga seotud teavet. Prognoosi koostamise energianõudluse soovime prognoosida tegelik nõudmisel tavaliselt ja seetõttu meil tuleks kasutada seda sõltuv funktsioon.

**Puuduvaid väärtusi töötlemise:** Andmete ettevalmistamine etapis oleks vaja läbi parim strateegia puuduvate väärtuste käsitlemise määramiseks. Enamasti seda erinevate statistiliste [meetodite andmete arvestamine](https://en.wikipedia.org/wiki/Imputation_(statistics))abil. Puhul kujutatud nõudmisel prognoosi koostamiseks, saame omistamiseks tavaliselt puuduvate väärtuste abil jooksva keskmise: eelmise saadaval andmepunktid.

**Andmete normaliseerimine:** Andmete normaliseerimine on muud tüüpi transformatsioon, mida kasutatakse toomiseks kõik arvulised andmed, nt nõudmisel prognoos ühel. See tavaliselt aitab parandada mudeli täpsuse ja täpsusega seotud funktsioone. Me oleks tavaliselt seda, jagades tegelik väärtus vahemiku andmed.
See skaala esialgse väärtuse väiksema vahemikku, tavaliselt -1 ja 1 vahel.

## <a name="modeling"></a>Modelleerimine
Modelleerimine etapp on, kus toimub mudeli andmete teisendamine. Selle protsessi seal keskmes on täpsemalt algoritmide kohta, mida ajalooliste andmete (koolitus andmed), eraldada mustrite ja koostada mudel. Seda mudelit saab hiljem prognoosida, uued andmed, mis pole kasutatud mudeli koostamiseks.

Kui meil töötamise usaldusväärne mudel saame siis kasutada seda Keskmine uusi andmeid, mis on struktureeritud lisada nõutavad funktsioonid (X). Hinded protsessi muudavad kasutada nõutud mudeli (objekti koolitus etapp) ja prognoosida target muutuja, mis on tähistatud Ŷ.

### <a name="demand-forecasting-modeling-techniques"></a>Nõudmisel prognoosi koostamiseks modelleerimine meetodite abil
Prognoosi koostamiseks teeme vajadusel puhul kasutada varasemaid andmeid, mis on määratud aeg. Me üldiselt viidata andmed, mis sisaldab [aega](https://en.wikipedia.org/wiki/Time_series)sarjana ajadimensiooni. Aja sarja modelleerimine eesmärk on leida aega seotud trende, hooajalisus, automaatne-korrelatsioonikordaja (korrelatsioonikordaja aja jooksul), ja need mudeli kujundada.

Viimasel täpsemalt on arendatud ajasarja prognoosimise mahutamiseks ja parandada prognooside täpsust. Käsitleme lühidalt mõned neist siin.

> [AZURE.NOTE] Selles jaotises ei peaks masina Õppekeskuse ja prognoosi koostamiseks ülevaade, vaid kui lühike ülevaade modelleerimine meetodite abil, mis on tavaliselt kasutatakse vajadusel prognoosi koostamiseks kasutada. Lisateavet ja koolitusmaterjalid kohta ajasarja prognoosimise, soovitame tungivalt Web App raamatust [prognoosimise: põhimõtted ja praktika](https://www.otexts.org/book/fpp).

#### <a name="ma-moving-averagehttpswwwotextsorgfpp62"></a>[**MA (libisev keskmine)**](https://www.otexts.org/fpp/6/2)
Jooksva keskmise on üks esimese analytical meetodite abil, mis on kasutatud ajasarja prognoosimise ja see on veel üks kõige levinumaid võtteid alates tänasest. See on ka alus Täpsemad meetodid prognoosi koostamiseks. Ja libiseva keskmise on meil prognoosimise järgmise andmepunkti, keskmistamine üle K viimase punktid, kus K tähistab jooksva keskmise järjestuse.

Jooksva keskmise meetod on Eksponentsilumise prognoosi mõju ja seetõttu võib käsitleda ka suur Volatiilsus andmeid.

#### <a name="ets-exponential-smoothinghttpswwwotextsorgfpp75"></a>[**ETS (Eksponentsilumine)**](https://www.otexts.org/fpp/7/5)
Eksponentsilumise (ETS) on erinevad meetodid, mida kasutada kaalutud keskmist tehtud andmepunktide et prognoosida järgmise andmepunkti pere. Mõte on uuemasse väärtuste määramiseks suurema kaalu ja järk-järgult vähendamiseks seda kaalu vanemate mõõdetud väärtused. On mitmeid erinevaid viise selle pere, mõned neist kaasata käsitlemine hooajaliste andmete [Holt-talv hooajaline](https://www.otexts.org/fpp/7/5)meetod.

Mõned nende meetodite tegur hooajaliste andmete.

#### <a name="arima-auto-regression-integrated-moving-averagehttpswwwotextsorgfpp8"></a>[**ARIMA (automaatne regressiooni integreeritud jooksev keskmine)**](https://www.otexts.org/fpp/8)
Automaatne regressiooni integreeritud jooksev keskmine (ARIMA) on teine pere võimalused, mis on levinud aja sarja prognoosi koostamiseks. See praktiliselt ühendab automaatne-regressiooni meetodite jooksev keskmine. Automaatne – regressiooni meetodid kasutada regressiooni mudelite alusel eelmise aja sarja väärtused, et arvutada järgmise kuupäevapunkti. ARIMA meetodite rakendada ka differencing meetodid, mis sisaldavad andmepunktide vahe arvutamine ja nende asemel algse mõõdetud väärtuse kasutamine. Lõpuks ARIMA ka kasutab jooksva keskmise meetodite abil, mis on eespool. Kõik need meetodid mitmel viisil kombinatsioon on, mis sisuosast pere ARIMA võimalused.

ETS ja ARIMA on levinud täna kujutatud nõudmisel prognoosi koostamiseks ja palju muud prognoosimise probleeme. Paljudel juhtudel need on ühendatud koos, et väga täpne tulemusi.

**Üldine mitme regressiooni** Regressioonisirge mudelite võib olla kõige olulisem modelleerimine lähenemine masina õppimine ja statistika domeeni. Aja sarja kontekstis kasutame regressiooni prognoosida tulevaste väärtuste (*nt*nõudmise). Regressiooni võtame kombinatsiooni lineaarne soovitud aitavad prognoosida ja lugege need aitavad prognoosida kaalu (nimetatakse ka kordajad) koolituse käigus. Eesmärk on, andes regressioonisirge, mis prognoosib meie prognoositud väärtus. Regressioonisirge meetodid sobivad kui target muutuja on arvuline ja seetõttu ka sobib ajasarja prognoosimise. On mitmeid regressiooni meetodid, sh väga lihtne regressiooni mudelid nagu [Lineaarse regressioonisirge](https://en.wikipedia.org/wiki/Linear_regression) ja täpsemate neist, nt otsust puude, [Juhuslik metsade](https://en.wikipedia.org/wiki/Random_forest), [Närvivõrgud](https://en.wikipedia.org/wiki/Artificial_neural_network)ja võimendada otsust puude.

Ehitamine kujutatud nõudmisel prognoosi koostamiseks regressiooni probleem annab meile palju paindlikkust valida meie andmete funktsioonid, mida saab kombineerida tegelik vajadusel aega sarja andmete ja väliste tegureid nagu temperatuur. Funktsiooni tehnika (vt **andmete ettevalmistamine ja funktsioon Engineering**) osa sellest playbook arutatakse valitud funktsioonide kohta lisateabe saamiseks.

Meie kogemus rakendamise ja juurutamise kujutatud nõudmisel prognooside katseprojekti, on leitud, et täpsemalt regressiooni mudelid, mis on saadaval Azure ML pigem parimaid tulemusi ja teeme neid kasutada.

## <a name="model-evaluation"></a>Mudeli hindamine
Mudeli hindamise on **Mudel arengutsükli**kriitiline roll. Selles etapis vaatame mudeli ja töövõimet reaal elavamaks andmete valideerimine. Modelleerimine toimingu käigus kasutame koolitus mudeli olemasolevate andmete osa. Hindamise etapis võtame ülejäänud andmed testimiseks mudel. Praktiliselt tähendab see, et meil on söötmine mudel uusi andmeid, mis on ümber ja see sisaldab sarnaseid funktsioone nagu koolitus andmekomplekti. Siiski valideerimise käigus kasutame mudel prognoosida target muutuja asemel esitada saadaval target muutujana. Me sageli viidata selle protsessi mudeli hinded. Me siis kasutage true target väärtusi ja võrrelda prognoositud need. Siin eesmärk on mõõta ja ennustamine viga, s.t vahe prognoose ja väärtus true. Tõrge mõõtühikute iseloomustab on klahvi, kuna soovime mudeli viimistlemine ja kontrollida, kas viga tegelikult väheneb. Peenhäälestus mudelit saab teha muutes õ protsessi mudeli parameetrid, või lisamise või eemaldamise andmete funktsioonid (nimetatakse [parameetrite korrastamine](https://channel9.msdn.com/Blogs/Windows-Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Praktiliselt see tähendab, et vajame võib vahel modelleerimine, funktsioon matemaatika erifunktsioonid itereerima ja modelleerimise hindamise faasis mitu korda, kuni me ei suuda viga vähendada nõutud tasemele.

On oluline, et ennustamine tõrke kunagi olla null Rõhutuse pole kunagi mudelit, mida saab suurepäraselt prognoosida iga tulemusi. Kuid on tõrge, mis on aktsepteeritav, ettevõtte ulatust. Valideerimise käigus soovime veenduge, et tasemel on meie mudeli ennustamine tõrge või suurem kui business lubatud tase. Oluline määrata lubatud hälve ajal **Probleemi koostamise** etapis tsükli alguses.

### <a name="typical-evaluation-techniques"></a>Tüüpilised hindamise meetodite abil
On mitu võimalust, mis ennustamine saate tõrke mõõta ja mõõta. Selles jaotises me keskendub arutelu hindamise võtteid, aeg sari ja esitatud kujutatud nõudmisel prognoosi koostamise kohta.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE tähistab tähendab absoluutne protsent tõrge. Koos MAPE meil on arvutuste vahe iga prognoositud punkti ja tegelik väärtus punktis. Me siis arvuliselt väljendatud viga ühe punkti arvutades erinevuse suhtes tegeliku väärtuse osakaal. Viimase toimingu juures me keskmiselt need väärtused. Kasutatakse MAPE matemaatiline valem on järgmine:

![MAPE valemi](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*koht, kus<sub>t</sub> on tegelik väärtus, F<sub>t</sub> on prognoositud väärtus ja n on prognoositud Horisont.*

## <a name="deployment"></a>Juurutamine
Kui meil jalad alla modelleerimine etapp ja kinnitatud mudeli jõudluse oleme valmis juurutamise etappi. Siinkohal juurutamise tähendab, et lubada kliendi mudeli kasutamine käivitades tegelik prognoose seda suures ulatuses. Juurutamise on võti Azure'i ml meie peamine eesmärk on pidevalt autonoomsest ennustatud väärtuste asemel just saada ülevaade andmete põhjal. Juurutamise etapp on osa, kus oleme lubamine mudeli, mida saab tarbitud suuremahuliste veebisaidil.

Meie eesmärk on kujutatud nõudmisel prognoosi raames autonoomsest pidev ja perioodiliste prognooside, tagades samas, et uute andmete on saadaval mudeli ja et prognoositud andmed saadetakse nõudvate kliendile.

### <a name="web-services-deployment"></a>Web teenuste juurutamine
Peamised käivituva koosteüksuse Azure'i ml on veebiteenuse. See on kõige tõhusam viis lubamiseks tarbimine prognoositava mudeli pilveteenuses. Veebiteenuse kapseloi mudeli ja murtakse see [RESTful](http://www.restapitutorial.com/) API (rakenduse programmeerimise liides). API saab kasutada mis tahes kliendi kood, nagu on näidatud alloleval joonisel osana.

![Meie teenuste juurutamine ja tarbimine](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Nagu näha, veebiteenuse juurutatakse pilveteenuses Cortana ärianalüüsi komplekti ja saab kasutada üle selle exposed REST API lõpp-punkti. Erinevat tüüpi klientide eri domeenides saate korraga Kasuta teenust Web API kaudu. Veebiteenuse saab ka skaala toetavad tuhandete samaaegseid kõned.

### <a name="a-typical-solution-architecture"></a>Tüüpilised lahenduse arhitektuur
Prognoosi koostamiseks lahendus on kujutatud nõudmisel juurutamisel oleme huvitatud lõpuni lahenduse, mis ületab ennustamine veebiteenuse ja lihtsustab kogu andmete juurutamine. Me autonoomsest uue prognoosi ajal me oleks vaja veenduge, et mudeli juhitakse ajakohane andmete funktsioone. See tähendab, et äsja kogutud lähteandmed, et on pidevalt märkimisväärselt, töödeldav ja nõutav funktsioon pannud mudeli oli ehitatud ümber. Samal ajal soovime tarbimine kliendid lõppu prognoositud andmete kättesaadavaks. Alloleval joonisel on kujutatud on näide andmete meilivoo cycle (või andmete müügivõimaluste):

![Funktsioon Forecast kujutatud nõudmisel lõpuni andmevoo](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Neid juhiseid, mis leiavad aset seotud nõudmisel prognoosi koostamise käigus:
1.  Miljoneid juurutatud andmete meetrit loovad pidevalt power andmete reaalajas.
2.  Andmed on kogutud ja pilveteenuse hoidla (*nt*Azure'i bloobimälu) faile üles laadida.
3.  Enne töötlemist, lisatakse selle toorandmetega alajaama või piirkondliku ettevõtte määratletud.
4.  Funktsioon töötlemine (vt teemat **andmete ettevalmistamine ja funktsioon Processing**) siis toimub ja toodab jaoks on vaja andmeid mudel koolitus või hinded-funktsiooni määramine andmed salvestatakse andmebaasi (*nt*SQL Azure'i).
5.  Uuesti koolitus teenuse kutsumisel koolitada uuesti prognoosimise mudeli – mudeli värskendatud versiooni püsis nii, et seda saab kasutada hinded veebiteenus.
6.  Hinded veebiteenuse järgida ajakava, mis sobib nõutav prognoosi koostamise sagedus.
7.  Prognoositud andmed on salvestatud andmebaasis, millele pääseb end tarbimine kliendi poolt.
8.  Tarbimine kliendi toob prognooside, rakendatakse see uuesti sisse ruudustiku ja tarbib seda vastavalt puhul nõutav kasutamine.

See on oluline märkida, et see kogu tsükkel on täielikult automatiseeritud ja käivitatakse. Selle kogu korraldamise selle andmete tsükli saab teha tööriistadega nagu [Azure andmete Factory](http://azure.microsoft.com/services/data-factory/).

### <a name="end-to-end-deployment-architecture"></a>Lõpuni juurutamise arhitektuur
Praktiliselt juurutamiseks kujutatud nõudmisel prognoosi koostamise lahenduse Cortana ärianalüüsi kohta, läheb vaja tagamiseks, et õigesti konfigureeritud ja on nõutavad komponendid.

Järgmine diagramm näitab tüüpiline Cortana ärianalüüsi vastavalt arhitektuur, mis rakendab ja orchestrates andmete meilivoo tsükli, mis on eespool kirjeldatud.

![Lõpuni juurutamise arhitektuur](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Lisateavet iga komponendid ja kogu arhitektuur vaadake kujutatud lahenduse mall.
