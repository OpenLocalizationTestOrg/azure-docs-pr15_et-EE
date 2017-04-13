<properties
    pageTitle="Samm 3: Loo uus arvuti õ katse | Microsoft Azure'i"
    description="Töötada lühiülevaade sõnastikupõhise lahenduse samm 3: luua uus koolitus katse Azure seadme õ Studio."
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
    ms.date="10/05/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Kiirtutvustus samm 3: Loo uus Azure seadme õ katse

See on kolmas toiming kiirtutvustus, [arendada Azure'i masina õ ennustav lahendust](machine-learning-walkthrough-develop-predictive-solution.md) .


1.  [Seadme õ tööruumi loomine](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Laadige üles olemasolevad andmed](machine-learning-walkthrough-2-upload-data.md)
3.  **Looge uus katse**
4.  [Koolitada ja hindate mudelid](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Veebiteenuse juurutamine](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Accessi veebiteenuse](machine-learning-walkthrough-6-access-web-service.md)

----------

Järgmiseks ülevaate on luua katse masina õ Studios, mis kasutab andmekomplekti me üles laadida.  

1.  Studios, valige **+ Uus** akna allosas.
2.  Valige **katse**ja seejärel valige "Tühja katse". Valige lõuend ülaosas katse vaikenime ja pange selle nimeks tähenduslik

    > [AZURE.TIP] See on hea tava täitmiseks **Kokkuvõte** ja **Kirjeldus** paanil **Atribuudid** katse. Neid atribuute teile võimalust dokumendi katse nii, et keegi, kes vaatavad seda hiljem mõistate oma eesmärgid ja meetodid.

3.  Mooduli värvipaleti katse vasakul lõuend, laiendage **Andmekomplektide salvestatud**.
4.  Otsige loodud jaotises **Minu andmekomplektide** andmekomplekti ja lohistage see lõuend. Andmekomplekti Leia **otsinguvälja kohal olevat värvipaleti** nime sisestamine.  

##<a name="prepare-the-data"></a>Andmete ettevalmistamine
Saate vaadata, klõpsates väljundi pordi andmekogumi (väike ringi allosas) ja valides **Visualiseeri**esimest 100 read andmete ja kogu andmekomplekti statistika teavet.  

Kuna andmefaili kaasas veerupäised, Studio on andnud üldise pealkirjad *(Col1, Col2 jne)*. Hea pealkirjad pole oluline, et luua mudeli, kuid need oleks lihtsam töötada katse andmed. Samuti kui me lõpuks avaldada selle mudeli veebiteenuse, pealkirjad aitab tuvastada teenuse kasutajale veerud.  

[Metaandmete redigeerimine] veerupäised lisame[ edit-metadata] mooduli.
Saate kasutada [Metaandmete redigeerimine] [ edit-metadata] mooduli muutmiseks andmekomplekt seotud metaandmed. Sel juhul jätab rohkem sõbralik veerupäiste nimesid. 

[Metaandmete redigeerimine]kasutamise[edit-metadata], saate esmalt määrama veergude muuta (käesoleval juhul neid kõiki.) Järgmiseks teie määratud toimingu kohta nendes veergudes (antud juhul muutmine veerusilte või päiseid.)

1.  Mooduli värvipaleti, tippige **otsinguväljale** "metaandmed". [Metaandmete redigeerimine] näete[ edit-metadata] mooduli loendis.
2.  Klõpsake ja lohistage [Metaandmete redigeerimine] [ edit-metadata] mooduli soovitud lõuendile ja kukutage see all andmekomplekti lisasime varasemas versioonis.
3.  Ühenduse [Redigeerimine metaandmete]andmekomplekti[edit-metadata]: klõpsake väljundi pordi andmekogumi (väike ringi andmekomplekti allosas.) Seejärel lohistage Sisestuskeel pordi metaandmete [Redigeerimine] [ edit-metadata] (väike ringi mooduli ülaosas), seejärel vabastage hiirenupp. Andmekomplekti ja mooduli jäävad ühendatud isegi juhul, kui te liikuda kas lõuend.

    Katse peaks nüüd välja nägema umbes selline:  

    ![Redigeeri metaandmete lisamine][2]
    
    Punane hüüumärk näitab, et me pole selle mooduli atribuutide seadmine veel. Teeme selle edasi.
    
    > [AZURE.TIP] Kommentaari lisamiseks mooduli topeltklõpsates mooduli ja teksti sisestamine. See aitab teil kiiresti vaadata, mida mooduli teeb oma katse. Selles näites topeltklõpsake [Metaandmete redigeerimine] [ edit-metadata] mooduli ja tippige kommentaari "lisa veerupäised". Klõpsake mis tahes muud lõuendil sulgemiseks klõpsake tekstivälja. Kommentaari kuvamiseks klõpsake allanoolt moodul.

4.  Valige [Redigeeri metaandmete][edit-metadata], lõuend paremal paanil **Atribuudid** , klõpsake **veeru Vaateselektori käivitada**.
5.  **Valige veerud** dialoogiboksi, valige kõik read **Saadaolevad veerud** ja klõpsake nuppu > teisaldamiseks **Valitud veerud**.
Dialoogiboks peaks välja nägema selline: ![koos kõigi veergude valitud veeru valija][4]
7.  Klõpsake nuppu **OK** märke.
8.  Tagasi paanil **Atribuudid** , otsige **Uus veerunimede** parameeter. Sisestage sellele väljale veerud 21 andmekomplekti, komadega eraldatult ja veerujärjestuse nimede loendi. Võite saada veergude nimed andmekomplekti dokumentatsiooni kohta UCI veebisaidi või mugavuse saate kopeerida ja kleepida järgmises loendis.  

          Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  

    Paanil atribuudid näeb välja umbes järgmine:

    ![Redigeeri metaandmete atribuutide määramine][1]

> [AZURE.TIP] Kui soovite kontrollida veerupäised, käivitage katse (klõpsake **käivitada** katse lõuend allpool). Kui see on töö lõpetanud (roheline märge kuvatakse [Metaandmete redigeerimine][edit-metadata]), klõpsake väljundi pordi [Metaandmete redigeerimine] [ edit-metadata] mooduli ja valige **Visualiseeri**. Mis tahes mooduli väljund saate vaadata samal viisil andmete kaudu katse edenemise kuvamiseks.

##<a name="create-training-and-test-datasets"></a>Koolitus loomine ja andmekomplektide testimine

Luua eraldi kogumid, mis kasutame koolitus ja testimine meie mudel on katse järgmise juhisega.

Selleks kasutame [Tükeldatud andmetega] [ split] mooduli.  

1.  [Tükeldatud andmete] otsimine[ split] mooduli, lohistage lõuendi ja ühendage see viimane [Metaandmete redigeerimine] [ edit-metadata] mooduli.
2.  Vaikimisi on tükeldatud suhe 0,5 ning **randomiseeritud tükeldamine** parameeter on seatud. See tähendab, et juhusliku pooles andmed on üks port [Tükeldatud andmetega] ühenduse kaudu[ split] moodul ja pool teise kaudu. Saate reguleerida need, samuti **juhuslik seemne** parameeter muutmine tükeldatud koolitus ja testimine andmete vahel. Selle näite puhul me jätta neid-on.
    
    > [AZURE.TIP] Atribuut **murdosa väljundi andmekomplekti esimese ridu** määratleb, kui palju andmeid on vasakul väljundi pordi-ühenduse kaudu. Näiteks kui seate 0,7-suhte, siis 70% andmed on väljundi vasakul pordi kaudu ja 30% õige pordi kaudu.  
    
3. Topeltklõpsake [Tükeldatud andmetega] [ split] mooduli ja sisestage kommentaari, "koolitus/andmete tükeldamine 50%". 

Me ei saa kasutada [Tükeldatud andmetega] väljundid[ split] mooduli aga sarnaselt, kuid vaatame kasutada vasakul väljundi koolitus andmed ja õige väljund nii testimise andmete valimine.  

Veebisaidil UCI nimetatud misclassifying kõrge krediidi risk madal maksumus on viis korda suurem kui kulu misclassifying madal krediidi risk nii kõrge. Konto nii, et loome uue andmekomplekti, mis kajastab selle maksumus funktsiooni. Uue andmekomplekti on iga suure näide kopeeritud viis korda, samal ajal iga madal risk näide kopeeritud.   

Me kopeerimist R-koodi abil:  

1.  Otsimine ja lohistage [Käivitada R skripti] [ execute-r-script] mooduli kopeerida katse lõuend ja vasakul väljundi pordi [Tükeldatud andmetega] ühenduse[ split] esimese Sisestuskeel portide ("Dataset1"), [R skripti käivitada] [ execute-r-script] mooduli.
2. Topeltklõpsake [Käivitada R skripti] [ execute-r-script] mooduli ja sisestage kommentaari, "kulu määramine korrigeerimine".
2.  Paanil **Atribuudid** kustutamine **R skripti** parameetri vaiketekst ja sisestage selle skripti:

          dataset1 <- maml.mapInputPort(1)
          data.set<-dataset1[dataset1[,21]==1,]
          pos<-dataset1[dataset1[,21]==2,]
          for (i in 1:5) data.set<-rbind(data.set,pos)
          maml.mapOutputPort("data.set")


Meil on vaja teha sama kopeerimise toimingut iga väljundi jaoks [Tükeldatud andmetega] [ split] mooduli nii, et koolitus- ja andmed on sama maksumus kohandus.

1.  Paremklõpsake [Käivitada R skripti] [ execute-r-script] mooduli ja valige **Kopeeri**.
2.  Paremklõpsake katse lõuendit ja klõpsake siis käsku **Kleebi**.
3.  Ühenduse esimese Sisestuskeel pordi selle [Käivitada R skripti] [ execute-r-script] mooduli õige väljund port [Tükeldatud andmetega] [ split] mooduli. 
4.  Lõuend allservas nuppu **Käivita**. 

> [AZURE.TIP] R skripti käivitada mooduli koopia sisaldab moodulit algse nimega sama skripti. Kui kopeerite ja kleebite mooduli lõuendile, Kopeeri säilitab kõik atribuudid, mis originaali.  

Meie katse nüüd näeb välja umbes selline:

![Tükeldatud mooduli ja R skriptide lisamine][3]

R skriptide kasutamine oma katsete kohta leiate lisateavet teemast [pikenda oma eksperimenteerida R](machine-learning-extend-your-experiment-with-r.md).

**Järgmine: [rongi ja hindate mudelid](machine-learning-walkthrough-4-train-and-evaluate-models.md)**


[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/create1.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/create2.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/create3.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/columnselector.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
