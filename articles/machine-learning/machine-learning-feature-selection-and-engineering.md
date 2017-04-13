<properties
    pageTitle="Matemaatika erifunktsioonid ja Azure seadme õ valikut | Microsoft Azure'i"
    description="Selgitab funktsioonide valik ja funktsioon matemaatika ja tuuakse näiteid nende andmete-parandamiseks protsessi arvuti õ rolli."
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
    ms.date="09/12/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Matemaatika erifunktsioonid ja Azure seadme õ valikut

See teema selgitab funktsiooni matemaatika-ja funktsioonide valik andmete-parandamiseks protsessi masina õppe. See näitab, mida need protsessid hõlmab näiteid Azure seadme õ Studio abil.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Kasutatud seadme õ koolitus andmeid saab sageli parandada valiku või funktsioonide ekstraktimine kogutud töötlemata andmete. Näide valmistatud funktsioon liigitamiseks käsitsi kirjutatud märkide pilte õppimise kontekstis on valmistatud töötlemata bitine jaotuse andmed bit-tihedus kaart. See kaart aitab leida märke servad efektiivsemaks töötlemata jaotuse.

Valmistatud ja valitud funktsioonide tõhustada koolitus protsessi, mis avaldab eraldamiseks olulisi andmeid sisalduvat teavet. Need parandavad power nende mudelite sisendandmete täpselt liigitada ja prognoosida tulemuste huvi veel jõuliselt. Funktsioon matemaatika- ja valiku saab ühendada ka Lisa veel arvesatud tractable teha. Seda tehes täiustamine ja seejärel vähendada mitmeid funktsioone, mis on vaja kalibreerida või koolitada mudel. Matemaatiliselt räägi, valitud mudeli koolitada funktsioonid on võimalikult väike hulk sõltumatu muutuja, mis selgitavad mustrite andmed ja seejärel edukalt prognoosida tulemusi.

Matemaatika erifunktsioonid ja funktsioonide valik on üks osa suurema protsessi, mis koosneb tavaliselt neli toimingut:

* Andmete kogumine
* Andmete parandamiseks
* Andmemudeli loomine
* Töötlemise

Matemaatika erifunktsioonid ja valiku moodustavad masina õ etapil andmete parandamiseks. Selle protsessi aspektide eristatakse meie eesmärkide puhul:

* **Enne töötlemise**: selle protsessi proovib veenduge, et kogutud andmete on selge ja süsteemsuse. See sisaldab mitut andmekogumi, käsitsemise puuduvad, töötlemine vastuolus andmete integreerimisse ja andmetüüpide teisendamise.
* **Matemaatika erifunktsioonid funktsioonide**: selle protsessi avaldab lisafunktsioone oluline koostada olemasolevate töötlemata funktsioonide andmed ja prognoosivõime õ algoritmile suurendada.
* **Funktsioonide valik**: selle protsessi valib võtme alamhulk algse andmete funktsioonid dimensionaalsus koolitus probleemi vähendada.

Selles teemas käsitletakse ainult funktsiooni matemaatika ja funktsioonide valik aspektide andmete suurendamise protsess. Andmete eelnevalt töötlemiseks samm kohta leiate lisateavet teemast [eeltöötlus andmete Azure seadme õ Studios](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).


## <a name="creating-features-from-your-data--feature-engineering"></a>Funktsioonide loomise oma andmetest--funktsioonide matemaatika erifunktsioonid

Koolitus andmed koosneb maatriksi näited (kirjete või ridade talletatud vaatluste), mis koosneb mis on seatud funktsioonid (muutujate või väljad, mis on talletatud veerud). Funktsioonid, mis on määratud katse kujunduses oodatakse vabadusastmeid mustrite andmeid. Ehkki paljud toorandmetega väljad saab otse lisada kasutatud mudeli koolitada valitud funktsioonikomplekti, valmistatud lisafunktsioone sageli vaja ehitatud funktsioonide toorandmetega loomiseks on täiustatud koolitus andmehulgas.

Millised funktsioonid luuakse täiustamiseks andmehulga, kui mudeli koolitus? Valmistatud funktsioonid, mis koolituse täiustamiseks teavet, mis paremini mustrite andmete eristab. Eeldate lisateavet, mida ei jäädvustata selgelt uusi funktsioone või lihtsalt nähtava algse või olemasoleva funktsioonikomplekti, kuid see protsess on midagi art. Heli- ja otsuseid sageli vaja mõned domeeni teadmisi.

Azure'i masina õppimisega käivitamisel on lihtsam aru saada selle protsessi konkreetselt masina õ Studios proovide abil. Kaks näidet on siin esitatud:

* Regressioonisirge näide ([arv jalgrattalaenutust ennustamiseks](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)) kuuluva katse, kui sihtkoht väärtused on teada
* Teksti hankimine liigitamine näide [Räsi funktsiooni] abil[feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>Näide 1: Lisamise regressiooni mudeli ajaline funktsioonid ###

Demonstreerib insener funktsioonid regressiooni tööülesande kasutame katse "nõudmisel prognoosimine, jalgrattad" PivotTable Azure seadme õ Studio. See katse eesmärk on prognoosida nõudmisel rataste, mis on arvu jalgrattalaenutust määratud kuu, päev või tund. Kasutatakse **Bike rent UCI andmehulgas** andmehulga Sisestuskeel toorandmetega.

Selle andmehulga põhineb tegelikke andmeid säilitab bike rent võrgu Washingtoni näiteks Põhiliselt USA muutmine Bikeshare ettevõttest. Andmehulga tähistab jalgrattalaenutust arvu teatud tunni mõne päeva jooksul 2011 2012 – ja see sisaldab 17379 ridade ja veergude 17. Töötlemata funktsioonide kogum sisaldab ilm tingimused (temperatuur, õhuniiskus ja tuulekiirus) ja tüüp (pühade või weekday) päeva. Väli prognoosida on **cnt**, arv, mis tähistab soovitud jalgrattalaenutust kindla tunni jooksul ja mis ulatub 1 977.

Ehitada koolitus andmete tõhus funktsioone, neli regressiooni mudelid on loodud, kasutades sama algoritmi, kuid neli paljudes erinevates andmekogumi. Nelja andmekogumi tähistada sama Sisestuskeel lähteandmed, kuid üha funktsioonid seada. Need funktsioonid on rühmitatud nelja kategooriaid.

1. A = ilm + Püha weekday + nädalavahetuse prognoositud päeva funktsioonid
2. B = jalgrattad, mis olid rentida iga eelmise 12 tundi arv
3. C = jalgrattad, mis olid rentida iga eelmise 12 päeva sama tund arv
4. D = arv jalgrattad, mis olid rentida iga viimase 12 nädala samale tunni ja samale päevale

Lisaks funktsioon komplekti A, mis on juba olemas algse lähteandmed, luuakse on kolm komplekti funktsioonide tehnika protsessi funktsiooni kaudu. Funktsioon komplekti B teeb jalgrattad tehtud järele. Funktsioon määrata C teeb rataste nõudmisel kindla tunnis. Funktsioon määrata D teeb nõudmisel rataste kindla tunni ja kindla nädalapäeva. Iga nelja koolitus andmekogumi kaasab funktsioon määrab A, A + B, A + B + C ja A + B + C + D vastavalt.

Azure'i masina õ katse, on neid neli koolitus andmekogumi moodustada neli eelmääratletud töödeldud Sisestuskeel andmetel põhinevad harude kaudu. Vasakpoolse haru, välja arvatud iga nende harude sisaldab mõnda [Käivitada R skripti] [ execute-r-script] mooduli, kus kogumi saadud funktsioonid (funktsioon määrab B, C ja D) vastavalt ehitatud ja lisatud imporditud andmete määramine. Järgmisel joonisel näitab R skripti saab luua funktsioonikomplekt B teise vasakul kontoris.

![Funktsioon hulga loomine](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

Järgmises tabelis on neli mudelit jõudluse tulemuste võrdlus. Parima tulemuse kuvatakse funktsioonid A + B + C. Pange tähele, et vigade väheneb, kui täiendavad funktsioon määrab kaasatud koolitus andmed. See kinnitab meie eeldatakse, et funktsioon määrab B ja C täiendava asjakohase teabe regressiooni tööülesande. D funktsioonikomplekt lisamine ei tundu vigade täiendavad vähendamine.

![Võrrelda tulemusi](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>Näide 2: Tekstimahukuse funktsioonide loomine  

Funktsioon matemaatika rakendatakse ulatuslikult tekstimahukuse, nt dokumendi liigitamine ja meeleolu analüüsi seotud ülesanded. Näiteks kui soovite liigitada dokumentide mitmesse kategooriasse, tüüpilised eeldusel, et on sõnu või fraase, mis sisaldab ühe dokumendi kategooria tõenäoliselt mõne muu dokumendi kategooria ilmneda. Teisisõnu, sõna või fraasi jaotuse sagedus on võimalik vabadusastmeid erinevate dokumenditüüpide kategooriad. Teksti kaevandamine rakendustes funktsiooni tehnika protsess on vaja luua funktsioone, mis on seotud sõna või fraas, sagedus, kuna üksikute osade teksti sisu tavaliselt kasutada sisendandmete.

Selles ülesandes saavutamiseks rakendatakse mida kutsutakse *funktsiooni räsi* tõhus muuta suvalise teksti funktsioonid indeksite. Selle asemel, et teatud registrisse, selle meetodi funktsioone, rakendades räsi funktsiooni funktsioonidele ja kasutades nende väärtuste räsi indeksite otse seostada iga funktsioon (sõnu või fraase).

Azure'i masina õ, on [Funktsioon räsi] [ feature-hashing] moodul, mis loob nende funktsioonide sõna või fraas. Järgmisel joonisel on kujutatud selle mooduli abil. Sisestuskeel andmehulga on kaks veergu: aadressiraamatu reiting vahemikus 1 kuni 5 ja tegeliku vaadata sisu. See [Funktsioon räsi] eesmärk[ feature-hashing] moodul on tuua uusi funktsioone, mis Kuva esinemiskord sagedus vastavate sõnu või fraase kindla raamatu arvustus sees. Selle mooduli kasutamiseks peate tehke järgmist:

1. Valige veerg, mis sisaldab teksti sisestamine (selles näites**Col2** ).
2. Määrake *Hashing bitsize* 8, mis tähendab, et 2 ^ 8 = 256 funktsioonid on loodud. Sõna või fraasi on seejärel räsitud 256 indeksite. Parameetri *Hashing bitsize* vahemikus 1 kuni 31. Kui parameeter on seatud arvu, sõnu või fraase, on vähem tõenäoline, et sama registrisse räsitud.
3. Seadke parameetri *N-g* 2. See toob esinemiskord sagedus unigrams (funktsioon iga sõna) ja bigrams (funktsioon iga paari kõrvuti sõnu) teksti sisestamine. Parameetri *N-g* vahemikud 0-10, mis näitab järjestikku sõna kaasatakse funktsiooni maksimaalne arv.  

![Funktsioon loob rakendus mooduli](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

Järgmisel joonisel on need uued funktsioonid välja näevad.

![Funktsioon loob rakendus näide](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Filtreerimisfunktsioonid oma andmetest--funktsioonide valik  ##

*Funktsioonide valik* on protsess, mis rakendatakse sageli ehituse koolitus andmekogumi sõnastikupõhise modelleerimine tööülesannete nt liigitamine või regressiooni ülesannete jaoks. Eesmärk on funktsioonide alamhulk valida algse andmehulga, mis vähendab selle mõõdud tähistada suurt hulka andmeid dispersioon vähima funktsioonide abil. Selle alamhulga funktsioone sisaldab ainult funktsioone kaasatavad koolitada mudel. Funktsioonide valik eesmärk on kaks peamist:

* Funktsioonide valik sageli kõrvaldamise oluline, liigsed liigitamine täpsuse suurendatakse või väga sarnased omadused.
* Funktsioonide valik väheneb mitmeid funktsioone, mis muudab mudeli koolitus protsessi tõhusamaks. See on eriti oluline on koolitada, nt tugiteenuste vektorkuju masinad kallis õppurite jaoks.

Kuigi funktsioonide valik soovib funktsioone kasutada mudeli koolitada andmehulga arvu vähendamiseks, mitte tavaliselt viidatakse terminiga *dimensionaalsus vähendamine.* Funktsioon valiku meetodite ekstraktida algse funktsioonide andmete alamhulga need muutmata.  Dimensionaalsus vähendamise meetodite tööle valmistatud funktsioonid, mida saate muuta algse funktsioonid ja seega neid muuta. Dimensionaalsus vähendamise meetodite näited peamine komponent analüüsi, kanoonilise korrelatsiooni analüüsi ja ainsuses väärtus lagunemise.

Funktsioon valiku meetodite järelevalve kontekstis ühe ulatuslikult rakendatud kategooria on filter-põhiste funktsioonide valik. Järgi hindamisel korrelatsiooni funktsioonide ja target atribuut, nende meetodite rakendada mõõtmiseks funktsioonide lisamine hinde määramiseks. Funktsioonide järjestatud siis Keskmine, mille abil saate hoida või kõrvaldamise teatud funktsiooni piirmäära seadmine. Statistilised nende meetodite kasutatud mõõtudele näiteks Pearsoni korrelatsioonikordaja, vastastikune ja χ2-test.

Azure'i masina õ Studio pakub moodulid funktsioonide valik. Nagu on näidatud järgmisel joonisel, nende moodulid kaasata [filtrit vastavalt funktsioonide valik] [ filter-based-feature-selection] ja [Fisher lineaarse Discriminant analüüsi][fisher-linear-discriminant-analysis].

![Funktsioon valiku näide](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)


Näiteks kasutada [filtri-põhiste funktsioonide valik] [ filter-based-feature-selection] mooduli teksti kaevandamine näiteks eelnevalt kirjeldatud. Oletame, et soovite luua regressiooni mudel pärast kogumi 256 funktsioonid on loodud kaudu [Funktsiooni räsi] [ feature-hashing] mooduli ja et vastuse muutuja **Col1** ja tähistab raamatu läbivaatamine reiting vahemikus 1-5. Seadke **funktsioon hinded meetod** **Pearsoni korrelatsioonikordaja**, **Col1** **Target veerg** ja **50** **arv soovitud funktsioonid** . Mooduli [filtri-põhiste funktsioonide valik] [ filter-based-feature-selection] seejärel annab tulemiks sisaldavad 50 funktsioonid koos target atribuudi **Col1**andmehulgas. Järgmisel joonisel on see katse ja parameetrid.

![Funktsioon valiku näide](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

Järgneval joonisel on kujutatud tulemiks oleva andmehulga. Iga funktsiooni hinne vastavalt Pearsoni korrelatsiooni ise ja target atribuudi **Col1**kohta. Hoitakse funktsioonid ülemise hinded.

![Funktsioon filter-põhise valiku andmekogumi](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

Järgmisel joonisel on valitud funktsioonide vastavale hinded.

![Valitud funktsiooni hinded](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Rakendades selle [filtri-põhiste funktsioonide valik] [ filter-based-feature-selection] mooduli 50 256 funktsioonid on valitud, kuna need on kõige rohkem funktsioone, mis on seotud sihtkohas muutuv **Col1** põhineb hinded meetod **Pearsoni korrelatsioonikordaja**.

## <a name="conclusion"></a>Kokkuvõte
Funktsioon matemaatika- ja funktsioonide valik on tavaliselt läbi koolitus andmete ettevalmistamine, kui seadme õ mudeli loomine kaks toimingut. Tavaliselt funktsiooni matemaatika rakendatakse esmalt luua uusi funktsioone ja seejärel funktsioon valiku etapis toimub kõrvaldamiseks oluline, liigset või tugevalt seotud funktsioonid.

See ei ole alati tingimata funktsioonide matemaatika või funktsioonide valik sooritamiseks. Kas on vajalik sõltub teie või koguda andmeid, saate valida algoritmi ja katse eesmärk.


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
