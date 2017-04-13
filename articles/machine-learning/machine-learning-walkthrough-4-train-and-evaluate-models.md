<properties
    pageTitle="Samm 4: Koolitada ja hindate analüütiline prognoosmudelite | Microsoft Azure'i"
    description="Töötada lühiülevaade sõnastikupõhise lahenduse samm 4: rongis, Keskmine ja hinnata mitu mudelite Azure seadme õ Studios."
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
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a>Kiirtutvustus samm 4: Koolitada ja analüütiline prognoosmudelite hindamine

See teema hõlmab kiirtutvustus, [töötada ennustav lahenduse Azure seadme õppe](machine-learning-walkthrough-develop-predictive-solution.md) neljas toiming


1.  [Seadme õ tööruumi loomine](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Laadige üles olemasolevad andmed](machine-learning-walkthrough-2-upload-data.md)
3.  [Looge uus katse](machine-learning-walkthrough-3-create-new-experiment.md)
4.  **Koolitada ja hindate mudelid**
5.  [Veebiteenuse juurutamine](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Accessi veebiteenuse](machine-learning-walkthrough-6-access-web-service.md)

----------

Üks luua seadme õ mudeleid Azure seadme Õppekeskuse Studio kasutamise eeliseid on võimalus proovida rohkem kui ühte tüüpi mudeli katse ajal ja võrrelda tulemusi. Seda tüüpi katsetamine aitab leida parim lahendus teie probleemi.

Katse, mida me ei arendamise ülevaate, vaatame kahte erinevat tüüpi mudelite loomine ja võrrelge hinded tulemuste otsustada, millist algoritmi soovime kasutada meie lõpliku katse.  

On meil võib valida erinevaid mudeleid. Saadaval mudelite vaatamiseks mooduli värvipaleti laiendage **Arvuti õ** , ja seejärel **Mudeli lähtestamine** ja sõlmed selle all. Selleks et see katse, me valige tugi vektorkuju masina (SVM) ja kaks klassi võimendada otsust puude moodulid.    

> [AZURE.TIP] Abi otsustamisel, milline arvuti õ algoritmi parim sobib kindla probleemi lahendamiseks, mida proovite lahendada, vaadake, [Kuidas valida Microsoft Azure'i masina õ algoritmid](machine-learning-algorithm-choice.md).

##<a name="train-the-models"></a>Koolitada mudelid
Kõigepealt loome häälestamine võimendatud otsust puu mudeli:  

1.  [Kaks klassi võimendada otsustuspuu] otsimine[ two-class-boosted-decision-tree] mooduli värvipaleti mooduli ja lohistage see lõuend.
2.  Leida [Rongi mudeli] [ train-model] mooduli, lohistage lõuendi ja ühendage väljundi võimendatud otsust puu mooduli vasakul Sisestuskeel pordi ("treenimata mudel") [Rongi mudeli] [ train-model] mooduli.
    
    [Kaks klassi võimendada otsustuspuu] [ two-class-boosted-decision-tree] mooduli lähtestab üldise mudeli ja [Koolitada mudeli] [ train-model] kasutab koolitus andmete mudelisse koolitada. 
     
3.  Ühenduse vasakul [Käivitada R skripti] vasakul väljundi "(tulem andmekomplekti)[ execute-r-script] mooduli õige sisestusmeetodi port ("andmekomplekti") [Rongi mudeli] [ train-model] mooduli.

    > [AZURE.TIP] Me ei pea kaks sisendeid ja üks väljundeid [Käivitada R skripti] [ execute-r-script] moodul see katse nii, et saaksime jätke need manustamata. 

4.  Valige [Rongi mudeli] [ train-model] mooduli. Paanil **Atribuudid** nuppu **Käivita veeru Vaateselektori**, valige **Kõik** rippmenüüst ette klõpsake loendis **Saadaolevad veerud** ja sisestage "Krediidi risk" tekstivälja. Valige rippmenüüst jaotises **Valitud veerud** **Kõik tüübid** . Valige "Krediidi risk" ja klõpsake esiletõstetud noolenuppu teisaldamiseks **Valitud veerud**. 
5.  Klõpsake nuppu **Salvesta**.


See osa katse nüüd näeb välja umbes selline:  

![Mudeli koolitus][1]

Järgmiseks me loodud SVM mudel.  

Peate esmalt veidi selgituse SVM kohta. Võimendatud otsust puude töötada ka funktsioone, mis tahes tüüpi. Kuna SVM mooduli loob lineaarse klassifitseerijale, loodava mudeli on siiski parim test viga kui arvuline kõik funktsioonid on sama skaala. Kõik arvuline funktsioonid teisendamiseks sama skaala kasutame "Tanh" transformatsioon ( [Andmete normaliseerimine] [ normalize-data] mooduli.) See muudab meie numbrid [0,1] vahemikus. Stringi funktsioonid on ümber SVM mooduli kategooriline funktsioone ja seejärel kahendarvu 0 ja 1 funktsioone, Seetõttu me ei pea käsitsi muuta stringi funktsioonid. Ka me ei soovi muuta krediidi Risk veerg (veerg 21) – on arvuline, kuid me ei koolitus mudeli prognoosida, nii, et läheb vaja eraldi lahkuda väärtus.  

SVM mudeli häälestada, tehke järgmist.

1.  [Kahe-klassi tugi vektorkuju masina] otsimine[ two-class-support-vector-machine] mooduli värvipaleti mooduli ja lohistage see lõuend.
2.  Paremklõpsake [Rongi mudeli] [ train-model] mooduli, valige **Kopeeri**ja seejärel paremklõpsake lõuend ja klõpsake siis käsku **Kleebi**. [Rongi mudeli] koopia[ train-model] mooduli on algse nimega sama veeru valiku.
3.  Väljundi SVM mooduli ühenduse vasakul Sisestuskeel pordi ("treenimata mudel") teise [Rongi mudeli] [ train-model] mooduli.
4.  [Andmete normaliseerimine] otsimine[ normalize-data] mooduli ja lohistage see lõuend.
5.  Selle mooduli sisendi ühenduse vasakul väljundi vasakul [Käivitada R skripti] [ execute-r-script] mooduli (Pange tähele väljundi pordi mooduli ühendatakse rohkem kui üks muu moodul).
6.  [Andmete normaliseerimine] vasakul väljund porti ("ümber andmekomplekti") ühenduse[ normalize-data] mooduli õige sisestusmeetodi port ("andmekomplekti") teise [Rongi mudeli] [ train-model] mooduli.
7.  [Andmete normaliseerimine] paanil **Atribuudid** [ normalize-data] mooduli, valige **Tanh** parameetri **transformatsiooni meetodit** .
8.  Klõpsake **veeru Vaateselektori käivitada**, valige "veerge," jaoks **Alga väärtusega**, **kaasa** klõpsake esimest ripploendit, teine rippmenüüst **veeru tüübi** valimine, ja valige **arvulise** kolmanda rippmenüüst. Seda määrab kõik arvulised veerud (ja ainult arv) ümber.
9.  Klõpsake selle rea paremal olevat plussmärki (+) – See loob rippmenüüst rida. Valige **välistamine** esimest ripploendit, valige **veerunimede** teise rippmenüü, ja klõpsake nuppu tekstiväli ja valige "Krediidi risk" veergude loendist. Seda määrab krediitkaardiga Risk veeru ignoreerida (läheb vaja selleks, sest see veerg on arvuline ja nii muul viisil ümber).
10. Klõpsake nuppu **OK**.  


[Andmete normaliseerimine] [ normalize-data] mooduli on nüüd seatud Tanh teisendus sooritada kõik arvulised veerud, välja arvatud krediidi Risk veerg.  

See osa meie katse peaks nüüd välja nägema umbes järgmine:  

![Teine mudel koolitus][2]  

##<a name="score-and-evaluate-the-models"></a>Keskmine ja hindate mudelid

Testimise andmeid, mis on eraldatud [Tükeldatud andmetega] [ split] mooduli Keskmine meie koolitatud mudelid. Saame siis võrrelda tulemusi kaks mudelit, mis loodud paremate otsingutulemite kuvamiseks.  

1.  Leida [Keskmine mudeli] [ score-model] mooduli ja lohistage see lõuend.
2.  Ühenduse [Rongi mudeli] [ train-model] moodul, mis on ühendatud [Kaks klassi võimendada otsustuspuu] [ two-class-boosted-decision-tree] mooduli vasakule sisestusmeetodi pordi [Keskmine mudeli] [ score-model] mooduli.
3.  Ühenduse õige Sisestuskeel pordi [Keskmine mudeli] [ score-model] mooduli vasakul väljundi õige [Käivitada R skripti] [ execute-r-script] mooduli.

    [Keskmine mudeli] [ score-model] mooduli saate nüüd teha krediitkaardi teabe testimise andmed, käivitage see läbi mudeli ja mudeli genereeritud testide andmeid veeruga tegelik krediitkaardiga risk prognoose võrdlus.

4.  Kopeerimine ja kleepimine [Keskmine mudeli] [ score-model] mooduli teine koopia loomiseks või lohistage uue mooduli lõuend.
5.  Vasakul Sisestuskeel pordi selle mooduli ühenduse SVM mudeli (st väljundi pordi [Rongi mudeli] ühenduse[ train-model] moodul, mis on ühendatud [Kahe-klassi tugi vektorkuju masina] [ two-class-support-vector-machine] mooduli).
6.  Mudeli SVM meil tehke sama teisendus testi andmetele, nagu koolitus andmetega. Nii kopeerimine ja kleepimine [Andmete normaliseerimine] [ normalize-data] mooduli teine koopia loomiseks ja ühenduse õige [Käivitada R skripti] vasakul väljund[ execute-r-script] mooduli.
7.  Ühenduse õige Sisestuskeel pordi [Keskmine mudeli] [ score-model] vasakul väljund [Andmete normaliseerimine] mooduli[ normalize-data] mooduli.  

Kahe hinded tulemuste hindamiseks kasutame, mis [Annavad mudeli] [ evaluate-model] mooduli.  

1.  Otsimine [Hinnata mudeli] [ evaluate-model] mooduli ja lohistage see lõuend.
2.  Vasakul Sisestuskeel pordi ühenduse väljundi pordi [Keskmine mudeli] [ score-model] mooduli seostatud võimendatud otsust puu mudel.
3.  Õige Sisestuskeel pordi ühenduse loomine muu [Keskmine mudeli] [ score-model] mooduli.  

Katse käivitamiseks nuppu **Käivita** all lõuend. Võib kuluda mõni minut. Iga moodul, mis näitab, et see töötab, ja seejärel roheline märge mooduli lõpetades valmistamata näidik. Kui kõik moodulid on märgitud, on töö lõpetanud katse.

Katse peaks nüüd välja nägema umbes selline:  

![Hindamisel mõlema][3]

Tulemuste kontrollimiseks klõpsake väljund porti [Hinnata mudeli] [ evaluate-model] mooduli ja valige **Visualiseeri**.  

[Mudeli hinnata] [ evaluate-model] mooduli toodab paari Kõverjoonte ja mõõdikud, mis võimaldavad teil võrrelda kahe poolitusjoonega mudeli tulemusi. Saate vaadata tulemusi vastuvõtja tehtemärk omadus (ROC) kõverad, arvutustäpsuse/tagasivõtmise kõverjoonte või lifti kõverad. Kuvatakse täiendavate andmete sisaldab segadust maatriks, kumulatiivse väärtused kõvera (AUC) ja muude mõõdikute all. Saate muuta piirmäärast, teisaldades liugurit vasakule või paremale ja vaadake, kuidas see mõjutab mõõdikute kogum.  

Graafik paremal nuppu **Scored andmekomplekti** või **Scored andmekomplekti võrdlemiseks** seotud kõvera esiletõstmiseks ja seotud mõõdikud allpool kuvamiseks. Kõverad legendis "Viskas andmekomplekti" vastab [Hinnata mudeli] vasakul Sisestuskeel pordiga[ evaluate-model] mooduli - meie puhul, on see võimendatud otsust puu mudel. "Viskas andmekomplekti võrdlemiseks" vastab õige Sisestuskeel pordi - SVM mudeli meie puhul. Kui klõpsate neid silte, seda mudelit kõver on esile tõstetud ja vastavate mõõdikute kuvamiseks, nagu on näidatud järgmisel joonisel.  

![ROC kõverjoonte mudelid][4]

Uurides neid väärtusi, saate otsustada, milline mudel on kõige lähemal annab teile tulemused, mida otsite. Saate minge tagasi ja saades oma katse, muutes erinevate mudelite väärtused. 

> [AZURE.TIP] Käivitage ajalugu hoitakse iga kord, kui käivitate katse selle iteratsiooni kirje. Saate vaadata nende iteratsiooni ja taastada, klõpsates all lõuend **Käivitada ajaloo kuvamine** . Võite klõpsata ka **Eelnevalt käivitada** paanil **Atribuudid** naasmiseks eelmise iteratsiooni ühe teil on avatud.
> 
Saate mis tahes iteratsiooni oma katse koopia, klõpsates nuppu **SAVE AS** all lõuend. Arvestuse pidamiseks, mida olete proovinud oma katse iteratsiooni kasutada atribuudid on katse **Kokkuvõte** ja **Kirjeldus** .

>  Leiate lisateavet teemast [haldamine katsetamiseks iteratsiooni Azure seadme õ Studios](machine-learning-manage-experiment-iterations.md).  


----------

**Järgmine: [Deploy veebiteenuse](machine-learning-walkthrough-5-publish-web-service.md)**

[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train1.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train2.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train3.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train4.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
