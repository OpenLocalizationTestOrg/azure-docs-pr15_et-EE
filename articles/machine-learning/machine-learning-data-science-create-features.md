<properties
    pageTitle="Funktsioon matemaatika Cortana analüüsi käigus | Microsoft Azure'i" 
    description="Selgitatakse kasutatakse funktsiooni matemaatika erifunktsioonid ja näited andmete suurendamise protsess masina õppe roll."
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


# <a name="feature-engineering-in-the-cortana-analytics-process"></a>Funktsioon matemaatika Cortana analüüsi käigus 

Selles teemas selgitatakse, kasutatakse funktsiooni matemaatika ja tuuakse näiteid selle rolli masina õppe andmete suurendamise protsess. Näited, mida kasutatakse selle protsessi kujutamiseks on pärit Azure seadme õ Studio. 

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

See **menüü** erinevaid funktsioone andmete loomist kirjeldavate teemade lingid. See toiming on juhises [Meeskonnatöö andmete teadus protsess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Funktsioon engineering katsete suurendamiseks prognoosivõime õppe loomisega funktsioonid lähteandmed, mis aitavad õ protsessi hõlbustamiseks algoritmide kohta. Matemaatika erifunktsioonid ja funktsioonide valik on üks osa TDSP, mis on kirjeldatud selle [mis on andmete meeskonna teadus protsessi?](data-science-process-overview.md) Funktsioon matemaatika- ja valiku on osa, mis on TDSP **töötada funktsioonid** etapil. 

* **funktsioon matemaatika**: selle protsessi avaldab lisafunktsioone oluline koostada olemasolevate töötlemata funktsioonide andmed ja suurendamiseks prognoosivõime õ algoritmi.

* **funktsioonide valik**: selle protsessi valib võtme alamhulk algse andmete funktsioone, et vähendada dimensionaalsus koolitus probleemi.

Tavaliselt **funktsiooni matemaatika** rakendatakse esmalt luua uusi funktsioone ja seejärel **funktsioonide valik** etapis toimub kõrvaldamiseks oluline, liigset või tugevalt seotud funktsioonid.

Kasutatud seadme õ koolitus andmeid saab sageli parandada funktsioonid ekstraktimine kogutud töötlemata andmete. Näide valmistatud funktsioon liigitamiseks käsitsi kirjutatud märkide pilte õppimise kontekstis on veidi tihedusfunktsiooni kaardi valmistatud töötlemata bitine jaotuse andmete loomine. See kaart aitab leida märke servad efektiivsemaks lihtsalt kasutades töötlemata jaotuse otse.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="creating-features-from-your-data---feature-engineering"></a>Funktsioonide loomise oma andmetest - funktsioonide matemaatika erifunktsioonid

Koolitus andmed koosneb maatriksi näited (kirjete või ridade talletatud vaatluste), mis koosneb mis on seatud funktsioonid (muutujate või väljad, mis on talletatud veerud). Funktsioonid, mis on määratud katse kujundus oodatakse vabadusastmeid mustrite andmeid. Ehkki paljud toorandmetega väljad saab otse lisada kasutatud mudeli koolitada valitud funktsioonikomplekti, on sageli nii, et lisafunktsioone (valmistatud) on vaja luua mõne täiustatud koolitus andmekomplekti toorandmetega funktsioonide ehitatud.

Millised funktsioonid luuakse täiustamiseks andmekomplekti, kui mudeli koolitus? Valmistatud funktsioonid, mis koolituse täiustamiseks teavet, mis paremini mustrite andmete eristab. Loodame esitada täiendavat teavet, mis pole selgelt jäädvustatud või lihtsalt nähtava algse või olemasoleva funktsiooni määramine uusi funktsioone. Kuid see protsess on midagi art. Heli- ja otsuseid sageli vaja mõned domeeni teadmisi.

Azure'i masina õppimisega käivitamisel on lihtsam aru saada selle protsessi konkreetselt proovide Studio abil. Kaks näidet on siin esitatud:

* Regressioonisirge näide [ennustamine jalgrattalaenutust arvu](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) kuuluva katse, kui sihtkoht väärtused on teada
* Teksti kaevandamine liigitamine näide [Räsi funktsiooni](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) abil

### <a name="example-1-adding-temporal-features-for-regression-model"></a>Näide 1: Regressiooni mudel ajaline funktsioonide lisamine ###

Kasutame katse "nõudmisel prognoosimine, jalgrattad" Azure seadme õ Studios demonstreerib insener regressiooni tööülesande funktsioone. See katse eesmärk on prognoosida nõudmisel rataste, mis on arvu jalgrattalaenutust kindla kuu/päev/tunni jooksul. Andmekomplekti "rattarenti UCI andmekomplekti" kasutatakse Sisestuskeel toorandmetega. Tuvastatavate põhineb tegelikke andmeid säilitab bike rent võrgu Washingtoni näiteks Põhiliselt USA muutmine Bikeshare ettevõttest. Andmekomplekti tähistab jalgrattalaenutust arvu teatud tunni aasta 2011 ja aasta 2012 päeva jooksul ja see sisaldab 17379 ridade ja veergude 17. Töötlemata funktsioonide kogum sisaldab ilm tingimused (temperatuur/niiskus/tuulekiirus) ja päeva (Püha/nädalapäev) tüüp. Väli prognoosida on "cnt", arv, mis tähistab soovitud jalgrattalaenutust kindla tunni jooksul ja mis ulatub 1-977 vahemikke.

Eesmärgiga ehitamine efektiivse funktsioonid koolitus andmed, neli regressiooni mudelid on mõeldud sama algoritmi abil, kuid nelja eri koolitus andmekomplektide. Nelja andmekomplektide tähistada sama Sisestuskeel lähteandmed, kuid üha funktsioonid seada. Need funktsioonid on rühmitatud nelja kategooriasse.

1. A = ilm + Püha weekday + nädalavahetuse prognoositud päeva funktsioonid
2. B = jalgrattad, mis olid rentida iga eelmise 12 tundi arv
3. C = jalgrattad, mis olid rentida iga eelmise 12 päeva sama tund arv
4. D = arv jalgrattad, mis olid rentida iga viimase 12 nädala samale tunni ja samale päevale

Lisaks funktsioon komplekti A, mis on juba olemas algse lähteandmed, luuakse on kolm komplekti funktsioonide tehnika protsessi funktsiooni kaudu. Funktsioon komplekti B teeb väga viimaste nõudmisel on rataste. Funktsioon määrata C teeb rataste nõudmisel kindla tunnis. Funktsioon määrata D teeb nõudmisel rataste kindla tunni ja kindla nädalapäeva. Nelja koolitus andmekomplektide iga kaasab funktsioonide kogum A, A + B, A + B + C ja A + B + C + D vastavalt.

Azure'i masina õ katse, on neid neli koolitus andmekomplektide moodustada neli harude kaudu eelnevalt töödeldud Sisestuskeel andmekomplekti kaudu. Enamik haru, iga haru need sisaldab vasakul peale [käivitamine R skripti](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) moodul, mis, kus kogumi saadud funktsioonid (funktsioon määratud B, C ja D) vastavalt ehitatud ning lisatakse imporditud andmekomplekti. Järgmisel joonisel näitab R skripti saab luua funktsioonikomplekt B teise vasakul kontoris.

![funktsioonide loomine](./media/machine-learning-data-science-create-features/addFeature-Rscripts.png)

Järgmises tabelis on kokku võetud tulemusi neli mudelite võrdlus. Parima tulemuse kuvatakse funktsioonid A + B + C. Teate, et vigade väheneb, kui täiendavad funktsioonikomplekt kaasatud koolitus andmed. See kinnitab meie eeldatakse, et funktsioonikomplekt B, C täiendava asjakohase teabe regressiooni ülesande jaoks. Kuid lisamise funktsiooni D tundu vigade täiendavad vähendamine.

![tulem võrdlus](./media/machine-learning-data-science-create-features/result1.png)

### <a name="example2"></a>Näide 2: Tekstimahukuse funktsioonide loomine  

Funktsioon matemaatika rakendatakse ulatuslikult tekstimahukuse, nt dokumendi liigitamine ja meeleolu analüüsi seotud ülesanded. Näiteks kui soovime dokumentide liigitada mitmesse kategooriasse, tüüpilised eeldusel, et on word/laused ühe dokumendi kategooria tõenäoliselt mõne muu dokumendi kategooria ilmneda. Mõne muu sõna, on võimalik vabadusastmeid erinevate dokumenditüüpide kategooriad sagedus sõna/laused jaotuse. Teksti kaevandamine rakendustes, kuna üksikute osade teksti sisu tavaliselt kasutada sisendandmete, funktsiooni tehnika protsess on vaja luua funktsioone, mis on seotud sõna/fraas sagedus.

Selles ülesandes saavutamiseks rakendatakse mida kutsutakse **funktsiooni räsi** tõhus muuta suvalise teksti funktsioonid indeksite. Selle asemel, et seostada iga funktsioon (sõna/laused) kindla registrisse, rakendades räsi funktsiooni funktsioonidele ja nende räsi väärtused otse indeksite kasutamine selle meetodi funktsioonid.

Azure'i masina õ, on [Funktsioon räsi](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) moodul, mis loob nende sõna/fraas funktsioone mugavalt. Järgmisel joonisel on kujutatud selle mooduli abil. Sisestuskeel andmekomplekti on kaks veergu: aadressiraamatu reiting vahemikus 1-5, ja tegeliku läbivaatus sisu. See [Funktsioon räsi](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) mooduli eesmärk on alla laadida hulga uued funktsioonid esinemissageduse vastavate sõnad, mis näitavad / lause(te) jooksul kindla raamat ülevaade. Selle mooduli kasutamiseks on vaja tehke järgmist:

* Esmalt valige veerg, mis sisaldab teksti sisestamine ("Col2" selles näites).
* Teiseks väärtuseks "Hashing bitsize" 8, mis tähendab, et 2 ^ 8 = 256 luuakse funktsioone. Wordi/etapp kogu tekst kuvatakse räsitud 256 jaotistele. Parameetri "Hashing bitsize" vahemikus 1 kuni 31. Sõnu / lause(te) tõenäoliselt väiksem kui määrata, et see on suurem arv sama registrisse räsitud.
* Kolmas, seadke parameetri "G-N" 2. See saab teksti sisestamine esinemiskord sagedus unigrams (funktsioon iga sõna) ja bigrams (funktsioon iga paari kõrvuti sõnu). Parameetri "G-N" vahemikud 0-10, mis näitab järjestikku sõna kaasatakse funktsiooni maksimaalne arv.  

![Mooduli "Funktsioonide Hashing"](./media/machine-learning-data-science-create-features/feature-Hashing1.png)

Järgneval joonisel on kujutatud, mida nende funktsioonide uus ilme nagu.

!["Funktsioonide Hashing" näide](./media/machine-learning-data-science-create-features/feature-Hashing2.png)


## <a name="conclusion"></a>Kokkuvõte

Valmistatud ja valitud funktsioonide tõhustada koolitus protsessi, mis avaldab eraldamiseks olulisi andmeid sisalduvat teavet. Need parandavad power nende mudelite sisendandmete täpselt liigitada ja prognoosida tulemuste huvi veel jõuliselt. Funktsioon matemaatika- ja valiku saab ühendada ka Lisa veel arvesatud tractable teha. Seda tehes täiustamine ja seejärel vähendada mitmeid funktsioone, mis on vaja kalibreerida või koolitada mudel. Matemaatiliselt räägi, valitud mudeli koolitada funktsioonid on võimalikult väike hulk sõltumatu muutuja, mis selgitavad mustrite andmed ja seejärel edukalt prognoosida tulemusi.

Pange tähele, et mitte alati tingimata funktsioonide matemaatika või funktsioonide valik sooritamiseks. Kas see on vaja või ei sõltub meil või koguda andmeid, saame valige algoritmi ja katse eesmärk.
 
