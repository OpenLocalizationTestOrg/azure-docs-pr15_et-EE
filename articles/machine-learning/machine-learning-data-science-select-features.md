<properties
    pageTitle="Funktsioonide valik andmete meeskonna teadus protsessi | Microsoft Azure'i" 
    description="Selgitab eesmärk funktsioonide valik ja tuuakse näiteid nende andmete suurendamise protsess masina õppe rolli."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Funktsioonide valik meeskonnatöö andmete teadus protsess (TDSP)

Selles artiklis selgitatakse kasutatakse funktsioonide valik ja näited andmete suurendamise protsess masina õppe roll. Need näited on koostatud Azure seadme õ Studio. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Selles teemas selgitatakse otstarbe funktsioonide valik ja antakse ülevaade mõne selle rolli masina õppe andmete suurendamise protsess. Need näited on koostatud Azure seadme õ Studio. 

Matemaatika erifunktsioonid ja funktsioonide valik on üks osa TDSP, mis on kirjeldatud selle [mis on andmete meeskonna teadus protsessi?](data-science-process-overview.md). Funktsioon matemaatika- ja valiku on osa, mis on TDSP **töötada funktsioonid** etapil.

* **funktsioon matemaatika**: selle protsessi avaldab lisafunktsioone oluline koostada olemasolevate töötlemata funktsioonide andmed ja prognoosivõime õ algoritmile suurendamiseks.

* **funktsioonide valik**: selle protsessi valib võtme alamhulk algse andmete funktsioone, et vähendada dimensionaalsus koolitus probleemi.

Tavalisel viisil **funktsiooni matemaatika** rakendatakse esmalt luua uusi funktsioone ja seejärel **funktsioonide valik** etapis toimub kõrvaldamiseks oluline, liigset või tugevalt seotud funktsioonid.


## <a name="filtering-features-from-your-data---feature-selection"></a>Filtreerimisvõimalusi oma andmetest - funktsioonide valik 

Funktsioonide valik on protsess, mis rakendatakse tihti rajamise koolitus andmekomplektide sõnastikupõhise modelleerimine tööülesannete nt liigitamine või regressiooni ülesannete jaoks. Eesmärk on funktsioonide alamhulk valida algse andmekomplekti, mis selle mõõdud vähendada tähistada suurt hulka andmeid dispersioon vähima funktsioonide abil. Selle alamhulga funktsioonid on seejärel koolitada mudeli lisada ainult funktsioone. Funktsioonide valik eesmärk on kaks peamist.

* Esmalt funktsioonide valik sageli kõrvaldamise oluline, liigsed liigitamine täpsuse suurendatakse või väga sarnased omadused.
* Teiseks vähendab see mitmeid funktsioone, mis muudab mudeli koolitus protsessi tõhusamaks. See on eriti oluline on koolitada, nt tugiteenuste vektorkuju masinad kallis õppurite jaoks.

Kuigi funktsioonide valik taotlemine mudeli koolitada kasutatakse andmekomplekti funktsioonide arvu vähendamiseks, mitte tavaliselt viidatakse terminiga "dimensionaalsus vähendamine". Funktsioon valiku meetodite ekstraktida algse funktsioonide andmete alamhulga need muutmata.  Dimensionaalsus vähendamise meetodite tööle valmistatud funktsioonid, mida saate muuta algse funktsioonid ja seega neid muuta. Dimensionaalsus vähendamise meetodite näited põhisumma Component analüüsi, kanoonilise korrelatsiooni analüüsi ja ainsuses väärtus lagunemise.

Hulgas üks ulatuslikult rakendatud kategooria funktsioon valiku meetoditest järelevalve kontekstis nimetatakse "filtri funktsioon selection". Järgi hindamisel korrelatsiooni funktsioonide ja target atribuut, nende meetodite rakendada mõõtmiseks funktsioonide lisamine hinde määramiseks. Funktsioonide järjestatud siis Keskmine, mis võib aidata piirmäärast säilitamise või kõrvaldamise teatud funktsioonide jaoks kasutada. Statistilised nende meetodite kasutatud mõõtudele näiteks isiku korrelatsiooni, vastastikune ja tš ruutude test.

Azure'i masina õ Studios, on mõeldud funktsioonide valik moodulid. Nagu on näidatud järgmisel joonisel, nende moodulid kaasata [filtrit vastavalt funktsioonide valik] [ filter-based-feature-selection] ja [Fisher lineaarse Discriminant analüüsi][fisher-linear-discriminant-analysis].

![Funktsioon valiku näide](./media/machine-learning-data-science-select-features/feature-Selection.png)


Võtke arvesse, näiteks [Filter-põhiste funktsioonide valik] Kasuta[ filter-based-feature-selection] mooduli. Selleks, et mugavuse huvides jätkame kasutada eespool kirjeldatud tekst kaevandamise näide. Oletagem, et luua regressiooni mudel pärast kogumi 256 funktsioonid on loodud kaudu [Funktsiooni räsi] soovime[ feature-hashing] mooduli ning et vastuse muutuja on "Col1" ja tähistab raamatu läbivaatamine hinnangute vahemikus 1-5. "Funktsioonide hinded meetod" seadmisega olema "Pearsoni korrelatsioonikordaja", "Target veerg" "Col1" ja "Arv soovitud funktsioonid" 50. Seejärel mooduli [filtri-põhiste funktsioonide valik] [ filter-based-feature-selection] toodaks andmekomplekt, mis sisaldab 50 funktsioonid koos target atribuudi "Col1". Järgmisel joonisel on see katse ja me lihtsalt kirjeldatud parameetrid.

![Funktsioon valiku näide](./media/machine-learning-data-science-select-features/feature-Selection1.png)

Järgmisel joonisel on tulemuseks andmekomplektide. Iga funktsiooni hinne vastavalt Pearsoni korrelatsiooni ise ja target atribuudi "Col1" kohta. Hoitakse funktsioonid ülemise hinded.

![Funktsioon valiku näide](./media/machine-learning-data-science-select-features/feature-Selection2.png)

Vastavale hinded valitud funktsioonid on näidatud järgmisel joonisel.

![Funktsioon valiku näide](./media/machine-learning-data-science-select-features/feature-Selection3.png)

Rakendades selle [filtri-põhiste funktsioonide valik] [ filter-based-feature-selection] mooduli 50 256 funktsioonid on valitud, kuna need on kõige seotud funktsioonid target muutujaga "Col1", mis põhineb hinded meetod "Pearsoni korrelatsioonikordaja".

## <a name="conclusion"></a>Kokkuvõte
Funktsioon matemaatika- ja funktsioonide valik on kaks tihti insenerirajatis ja valitud funktsioonide tõhustada koolitus protsessi, mis avaldab eraldamiseks olulisi andmeid sisalduvat teavet. Need parandavad power nende mudelite sisendandmete täpselt liigitada ja prognoosida tulemuste huvi veel jõuliselt. Funktsioon matemaatika- ja valiku saab ühendada ka Lisa veel arvesatud tractable teha. Seda tehes täiustamine ja seejärel vähendada mitmeid funktsioone, mis on vaja kalibreerida või koolitada mudel. Matemaatiliselt räägi, valitud mudeli koolitada funktsioonid on võimalikult väike hulk sõltumatu muutuja, mis selgitavad mustrite andmed ja seejärel edukalt prognoosida tulemusi.

Pange tähele, et mitte alati tingimata funktsioonide matemaatika või funktsioonide valik sooritamiseks. Kas see on vaja või ei sõltub meil või koguda andmeid, saame valige algoritmi ja katse eesmärk.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
 
