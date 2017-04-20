<properties
    pageTitle="Lihtne eksperiment masin õppe Studio | Microsoft Azure"
    description="See masin õppe juhendaja te loeksite lihtne andmete teadus katse. Me ennustada, regressiooni algoritmi auto hind."
    keywords="eksperiment, lineaarse regressiooni, masin õppe algoritme, masin õppe juhendaja ja ennustava modelleerimismeetodite, andmete teadus katse"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="garye"/>

# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Masin õppe juhendaja: luua oma esimene andmete teadus katse Azure masin õppe Studio

See masin õppe juhendaja te loeksite lihtne andmete teadus katse. Loome lineaarse regressiooni mudel, mis ennustab vastavalt erinevaid muutujaid nt sõiduauto hinna teha ja tehnilised kirjeldused. Selleks kasutame Azure masin õppe Studio ja ennustava lihtne kinnitada, analytics eksperimenteerida.

*Ennustav* on omamoodi andmed teadus, mis kasutab praeguse, et prognoosida tulevasi tulemusi. Ennustav väga lihtne näide, Vaata andmeid teaduse algajatele video 4: [ennustada vastust lihtne mudel](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) (Kestus: 7:42).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Kuidas masin õppe Studio aitab?

Masin õppe Studio on lihtne seadistada kasutades drag-and-drop moodulid programmeeritud sõnastikupõhise modelleerimismeetodite eksperiment. Eksperiment ja ennustada vastust, saad kohapeal õppe Studio abil *luua mudel*, *rongi mudel*ja *Keskmine ja test mudel*.

Sisestage masin õppe Studio: [https://studio.azureml.net](https://studio.azureml.net). Kui olete masina õppe Studio enne sisse logitud, klõpsake linki **Logi sisse siin**. Muul juhul klõpsake nuppu **Sign Up** ja valida tasuta ja tasulisi võimalusi.

Üldine lisateave masin õppe Studio, vt [mis on masin õppe Studio?](machine-learning-what-is-ml-studio.md)

## <a name="five-steps-to-create-an-experiment"></a>Viis sammu luua eksperiment

Selle kohapeal õppe juhendaja, teil jälgida ehitada masin õppe Studio loomiseks, õpetada ja Keskmine mudelis eksperiment viis põhisammu:

- Luua mudel
    - [1. samm: Saada andmed]
    - [2. samm: Preprocess andmed]
    - [3. samm: Määratlemine funktsioonid]
- Rongi mudel
    - [4. samm: Vali ja rakendada õppe algoritm]
- Keskmine hinne ja testida loodud mudelit
    - [5. samm: Ennustada uute autode hinnad]

[1. samm: Saada andmed]: #step-1-get-data
[2. samm: Preprocess andmed]: #step-2-preprocess-data
[3. samm: Määratlemine funktsioonid]: #step-3-define-features
[4. samm: Vali ja rakendada õppe algoritm]: #step-4-choose-and-apply-a-learning-algorithm
[5. samm: Ennustada uute autode hinnad]: #step-5-predict-new-automobile-prices


## <a name="step-1-get-data"></a>1. samm: Saada andmed

On mitmeid proovi andmekogumite valida kohapeal õppe Studio kaasas, ja saate importida andmeid mitmest allikast. Selles näites me kasutame valimisse kaasatud andmekomplekti, **auto andmeid (Raw)**.
Tuvastatavate sisaldab kannete arvu üksikute autod, sh teave Mark, mudel, tehnilised kirjeldused ja hind.

1. Uus eksperiment käivitamiseks klõpsake nuppu **+ Uus** masin õppe Studio akna allosas, valige **EKSPERIMENTEERIDA**ja valige **Tühi eksperimenteerida**. Valige eksperiment vaikenime lõuend ülaosas ja Nimeta see midagi olulist, näiteks **auto hinna ennustus**.

2. Eksperimendi vasakul lõuend on palett andmekogumite ja moodulid. See palett leida andmekogumi ülaosas otsinguväljale tüüp **auto** nimi **auto andmeid (Raw)**.

    ![Palett Otsi][screen1a]

3. Lohistage andmekomplekti eksperiment lõuend.

    ![Andmekogumi][screen1]

Näha, kuidas need andmed välja näeb, klõpsake väljundport auto andmekogumi all ja valige **Visualiseeri**.

![Moodul väljundport][screen1c]

Andmekogumi muutujad esitatakse ja iga astme sõiduauto ilmub järjest. Paremäärmuslikud veerg (veerg 26 ja pealkirjaga "hind") on eesmärgi muutuja, me ei kavatse proovida ennustada.

![Andmekogumi visualiseerimine][screen1b]

Sulgege aken visualiseerimine, klõpsates "**x**" ülemises paremas nurgas.

## <a name="step-2-preprocess-data"></a>2. samm: Preprocess andmed

Andmekogumi nõuab tavaliselt mõned eeltöötlus enne võib analüüsida. Te olete märganud puuduvad väärtused erinevate ridade veergude kohal. Need puuduvad väärtused tuleb puhastada nii, et mudeli analüüsida andmeid õigesti. Meie puhul, siis me eemaldame kõik read, mis on puuduvad väärtused. Ka, veeru **normaliseeritud kahju** on suur osa puuduvad väärtused, nii saad VÄLISTAME selle veeru mudel kokku.

> [AZURE.TIP] Puhastamine: andmed puuduvad väärtused on nõutav kasutades enamik moodulitest.

Esmalt eemaldatakse **normaliseeritud kahjumi** veerg ja seejärel eemaldatakse rida, milles andmed puuduvad.

1. **Valige veerud,** tippige otsingukasti ülaosas moodul palett leidmiseks [Vali veerud andmekogumis] [ select-columns] moodul ja lohistage see eksperiment lõuend ja ühendage see väljund porti **auto andmeid (Raw)** andmekogumi. See moodul võimaldab valida mis veerge, tahame kaasata või välja jätta mudel.

2. Valige [Valige veerud andmekogumis] [ select-columns] moodul ja **käivitada veeruselektor** Klõpsake paanil **Atribuudid** .

    - Klõpsake vasakul, **eeskirjade**
    - Klõpsake jaotises **Alustada koos**, **kõik veerud**. See suunab [Andmekogumis Vali veerud] [ select-columns] läbi kõik veerud (välja arvatud me parasjagu välja jätta).
    - Tilk-Downs, valige **välja jätta** ja **veergude nimed**ja seejärel klõpsake tekstiväljal kohta. Kuvatakse loend veergude. Valige **normaliseeritud kahju**ning see lisatakse viia.
    - Veeru valija sulgemiseks nuppu märge (OK).

    ![Vali veerud][screen3]

    **Valige** veergude andmekogumis omadustega pane näitab, et see läbib kõik veerud, välja arvatud **normaliseeritud kahjumi**andmekomplekti.

    ![Valige veergude andmekogumi majutusasutust][screen4]

    > [AZURE.TIP] Kommentaari lisamiseks moodul topeltklõpsake moodul ja teksti sisestamine. See aitab teil kiiresti vaadata, mida moodul teeb oma katses. Sel juhul topeltklõpsake [Andmekogumis Vali veerud] [ select-columns] moodul ja Kirjuta kommentaar "välistada normaliseeritud kahju."

3. Lohistage [Puhta puuduvad andmed] [ clean-missing-data] eksperiment moodul lõuend ja loomiseks [Valige veerud andmekogumis] [ select-columns] moodul. Valige paanil **Atribuudid** **eemaldada terve rea** all **puhastus režiimi** andmete puhastamist eemaldage read, mis on puuduvad väärtused. Topeltklõpsake moodul ja tippige kommentaari "Eemalda puuduvate väärtuste rea."

    ![Puhas puuduvate andmete atribuudid][screen4a]

4. Katse käivitada klõpsates **käivitada** katse lõuend.

Katse lõppedes kõik moodulid on roheline märge näitamaks, et nad edukalt lõppenud. Märgata ka **töötab lõpetatud** olek üleval paremas nurgas.

![Esimese katse käivitada][screen5]

Kõik me oleme teinud katse seda punkti on andmete puhastamist. Puhastatud andmekomplekti kuvamiseks, klõpsake vasakul väljundport [Puhta puuduvad andmed] [ clean-missing-data] moodul ("Cleaned andmekogumi") ja valige **Visualiseeri**. Märkate, et **normaliseeritud kadude** veerus ei mainita ja puuduvaid väärtusi.

Nüüd, kui saadud andmed on, olete valmis täpsustada, mida me ei kavatse kasutada sõnastikupõhist funktsioonid.

## <a name="step-3-define-features"></a>3. samm: Määratlemine funktsioonid

*Kohapeal õppida, on midagi olete huvitatud mõõdetavad omaduste.* Meie andmekogumis iga rida tähistab üks auto ja iga veeru iseloomustab et auto.

Leida hea funktsioone prognoositava mudeli loomiseks vajab katsetamist ja teadmisi probleem lahendatav. Mõned funktsioonid on parem ennustavad eesmärgi kui teised. Ka, mõned funktsioonid on tugev seos muude funktsioonidega (nt linna-mpg versus highway-mpg), nii et nad ei lisa palju uut teavet esitatud näidisele ja neid saab eemaldada.

Rajame mudel, mis kasutab funktsioone osa meie andmekogumis. Saate tagasi tulla ning valida erinevate katse uuesti käivitada ja näha kas te saate paremaid tulemusi. Aga alustada, let's proovida järgmisi funktsioone:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Lohistage teise [Valige veerud andmekogumis] [ select-columns] eksperiment moodul lõuend ja ühendage see vasakul väljundport [Puhta puuduvad andmed] [ clean-missing-data] moodul. Topeltklõpsake moodul ja tippige "Valige funktsioonid ennustus."

2. Klõpsake paanil **Atribuudid** **käivitada veeruselektor** .

3. Klõpsake nuppu **reeglid**.

4. Alusel **Alustada koos** **veerge,**klõpsake ja valige filter reas **kaasa** ja **veeru nimed** . Sisestage meie loetelu veergude nimed. See suunab läbi ainult veerud, mida me määrata moodul.

    > [AZURE.TIP] Käivitate katse, Tagame meie andmed veerukirjeldustele kiiruselt andmekomplekti kaudu [Puhta puuduvad andmed] [ clean-missing-data] moodul. See tähendab, et loote teistes moodulites on ka teave esitatud andmete.

5. Märge (OK) nuppu.

![Vali veerud][screen6]

See tekitab mida kasutatakse õppe algoritmi järgmise sammu. Hiljem, tagasi ja proovige uuesti koos erinevaid funktsioone.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>4. samm: Vali ja rakendada õppe algoritm

Nüüd, kui andmed on valmis, prognoositava mudeli ehitus koosneb eksamineerimise. Me kasutame meie andmed rongi mudel ja testige mudel näha, kui lähedal on võimalik ette näha eest. Nüüd, ära muretse miks meil on vaja koolitada ja testige mudel.

*Klassifikatsioon* ja *regressiooni* on kahte tüüpi järelevalve all kohapeal õppe meetodeid. Klassifikatsioon ennustab vastust: kindlaks määratud kategooriaid, nagu värvi (punane, sinine või roheline). Regressiooni abil ennustada mitmeid.

Sest me tahame ennustada hind, mille number, kasutame regressiooni mudel. Näiteks, me koolitame lihtsa *lineaarse regressiooni* mudel ja järgmise sammuna me test see.

1. Kasutame meie koolituse ja testimise jagades eraldi koolituse ja testimise komplekti. Valige ja lohistage [Split andmete] [ split] moodul eksperiment lõuend ja ühendage see väljund viimase [Andmekogumis Vali veerud] [ select-columns] moodul. Määratud **osa ridu esimene väljund andmekogumis** 0,75. Nii, vaatame rongi Mudel 75 protsenti andmete abil, ja tagasi hoida 25 protsenti testimiseks.

    > [AZURE.TIP] **Random seemne** parameeter muutes saab toota eri juhuvaliku eksamineerimise eest. See parameeter kontrollib pseudo numbrigeneraator külv.

2. Katse käivitada. See võimaldab [Valida veergude andmekogumis] [ select-columns] ja [Split andmete] [ split] edasi veerukirjeldustele me lisame järgmine moodulitega moodulite.  

3. Õppe algoritmi valimiseks Laiendage vasakul lõuend **Masin õppe** moodul palett kategooria ja seejärel laiendage **Lähtestada mudel**. See kuvab mitu liiki moodulid, mida saab käivitada masin õppe algoritme.

    Selle katse puhul valige [Lineaarse regressiooni] [ linear-regression] moodul (leiate ka moodul "lineaarse regressiooni" palett otsinguväljale tippides) **regressiooni** kategooria ja lohistage see eksperiment lõuend.

4. Leida ja lohistage [Rongi mudel] [ train-model] moodul eksperiment lõuend. Ühendage vasak sisendpordiga väljundi [Lineaarse regressiooni] [ linear-regression] moodul. Ühendage õige sisendpordiga koolitus andmete väljund (vasakul port) [Split andmete] [ split] moodul.

5. Valige [Rongi mudel] [ train-model] moodul, klõpsake **veeru valija käivitada** paanil **Atribuudid** ja valige veeru **Hind** . See on väärtus, et meie mudel saab ennustada.

    !["Hind" veeru valimine][screen7]

6. Katse käivitada.

Tulemuseks on koolitatud regressiooni mudel, mida saab koguda uued proovid teha prognoose.

![Kohalda masin õppe algoritm][screen8]

## <a name="step-5-predict-new-automobile-prices"></a>5. samm: Ennustada uute autode hinnad

Nüüd, kui meil on koolitatud Mudel 75% meie andmeid kasutades, saaksime seda teiste 25 protsenti andmete lugege hoolikalt koguda meie mudel toimib.

1. Leida ja lohistage [Keskmine mudel] [ score-model] eksperiment moodul lõuend ja vasakul sisendpordiga ühendada väljund [Rongi mudel] [ train-model] moodul. Õige sisendpordiga ühendada [Split andmed] (just sadama) katse väljundandmed[ split] moodul.  

    ![Keskmine mudel moodul][screen8a]

2. Eksperiment ja Vaata toodangu [Keskmine mudel] [ score-model] moodul, klõpsake väljund porti ja seejärel valige **Visualiseeri**. Väljund kuvatakse hinnaga ennustatud väärtused ja katseandmed teadaolevad väärtused.  

3. Lisaks tulemuste kvaliteedi testimiseks valige ja lohistage [Hinnata mudel] [ evaluate-model] eksperiment moodul lõuend ja ühenduse toodangu [Keskmine mudel] vasakul sisendpordiga[ score-model] moodul. (On olemas kaks sisendpordi, sest [Hinnata mudel] [ evaluate-model] moodul saab võrrelda kahe mudeli.)

4. Katse käivitada.

Vaatamiseks väljund [Hinnata mudel] [ evaluate-model] moodul, klõpsake väljund porti ja seejärel valige **Visualiseeri**. Meie mudeli näidatakse järgmise statistika:

- **Keskmine absoluutne viga** (MAE): keskmine absoluutne viga ( *error* on ennustatud väärtuse ja tegeliku väärtuse vahe).
- **Root Keskmine vea ruutkeskmine** (RMSE): ruudus vead prognoose teha katse andmekogumi Keskmine ruutjuure.
- **Absoluutne suhteline viga**: keskmine absoluutne vigade suhtes absoluutne erinevus tegeliku väärtuse ja kõigi tegelike väärtuste keskmine.
- **Suhtelise vea ruutkeskmine**: ruudus vead võrreldes ruudus erinevus tegeliku väärtuse ja keskmise kõigi tegelike väärtuste keskmine.
- **Koefitsiendi määramine**: tuntud ka kui **R ruudus väärtus**, see on statistiline mõõdik, mis näitab, kuidas mudel sobib andmed.

Iga viga on parem statistika, väiksem. Väiksem väärtus näitab, et prognoose mängu tihedamalt tegelike väärtuste. **Koefitsiendi määramine**, mida lähemale selle väärtus on üks (1,0), seda paremini ennustada.

![Hindamise tulemused][screen9]

Lõpliku katse peaks välja nägema selline:

![Masin õppe juhendaja: täielik lineaarse regressiooni eksperiment, mis kasutab sõnastikupõhise modelleerimismeetodite.][screen10]

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui sa oled lõpetanud esimese masin õppe juhendaja ja on oma katse luua, saate itereerima proovida parandada mudelit. Näiteks saate muuta oma ennustus kasutatud funktsioonid. Või [Lineaarse regressiooni] atribuutide muutmiseks[ linear-regression] algoritmi või algoritmi üldse proovida. Isegi lisada mitu masin õppe algoritme oma katse ajal ja kaks [Hinnata mudeli] abil võrrelda[ evaluate-model] moodul.

> [AZURE.TIP] Nupuga **SAVE AS** alla eksperiment lõuend iga iteratsiooni oma katse kopeerida. Näete kõiki oma katse iteratsioonide klõpsates **Käivitada ajaloo vaatamine** lõuend. Vt [Halda katsetada iteratsioonide Azure masin õppe Studio] [ runhistory] lähemalt.

[runhistory]: machine-learning-manage-experiment-iterations.md

Kui olete oma mudeli, saab see kasutada veebiteenistuse kasutatava auto eest ennustada uute andmetega. Vt [Deploy Azure'i masinõppe veebiteenuse] [ publish] lähemalt.

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Sõnastikupõhise modelleerimise tehnikaid, et luua laiem ja üksikasjalikku ülevaadet, koolitus, hinded ja juurutamise mudeli, vt [arendada Azure'i masinõppe abil ennustava lahendus][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen1c]: ./media/machine-learning-create-experiment/screen1c.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
