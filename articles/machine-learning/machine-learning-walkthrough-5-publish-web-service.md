<properties
    pageTitle="5. etapp: Juurutamine masin õppe veebiteenuse | Microsoft Azure"
    description="Arendada sõnastikupõhise lahendus läbikäiguks samm 5: kasutada masin õppe Studio on Web Service sõnastikupõhine eksperiment."
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


# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-web-service"></a>Juhend samm 5: Juurutada Azure masin õppe Web service

See on viies aste läbikäiguks, [arendada Azure'i masinõppe ennustav lahusena](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Masinõpet tööruumi loomine](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Laadida olemasolevaid andmeid](machine-learning-walkthrough-2-upload-data.md)
3.  [Loo uus eksperiment](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Rongi ja hinnata mudelid](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  **Veebiteenuse juurutamine**
6.  [Juurdepääs Web service](machine-learning-walkthrough-6-access-web-service.md)

----------

Et teistele võimalus kasutada sõnastikupõhist oleme välja töötanud ja see teid läbi, me kasutada see on Web Service Azure'i.

Selle hetkeni on meil olnud katsetavad koolitus meie mudel. Kuid juurutatud teenuse hakkab enam koolitus - see tekitab prognoose meie mudel põhineb kasutaja sisestatud hinded. Nii, et me ei kavatse teha mõningaid ettevalmistusi teisendada ***koolitus*** eksperiment, et see eksperiment on ***sõnastikupõhine*** eksperimenteerida. 

See on kaheastmeline:  

1. Teisendada selle *koolituse katsetada* oleme loonud *sõnastikupõhine eksperiment*
2. Juurutada sõnastikupõhise eksperiment on Web Service

Aga kõigepealt, on vaja see eksperiment natuke kärpima. Meil on praegu kahe erineva mudeli katse, kuid tahame ainult üks mudel, kui me kasutada see on veebipõhine teenus.  

Oletame, et oleme otsustanud et võimendatud puu mudel oli kasutada parem mudel. Nii et esimene asi, mida teha on eemaldada [Kaks klassi toetus vektori masin] [ two-class-support-vector-machine] moodul ja moodulid, mida kasutati see koolitus. Võite teha koopia eksperiment esmalt nuppu **Salvesta nimega** eksperiment lõuend allosas.

Meil on vaja kustutada järgmistest moodulitest:  

- [Kaks klassi toetus vektori masin][two-class-support-vector-machine]
- [Rongi mudel] [ train-model] ja [Keskmine mudel] [ score-model] mooduleid, mis olid seotud
- [Normaliseerida andmed] [ normalize-data] (mõlemad)
- [Hinnata mudel][evaluate-model]

Valige moodul ja klahvi Delete või moodul paremklõpsake ja valige **Kustuta**.

Nüüd olete valmis kasutama seda mudelit kasutades [Kaks klassi võimendatud Otsustepuu][two-class-boosted-decision-tree].

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Koolituse katse teisendada sõnastikupõhine eksperiment

Teisendamine sõnastikupõhise eksperiment hõlmab kolme etappi:

1. Salvesta mudel on koolitatud ja seejärel Asendage meie koolitusmoodulid
2. Kärbi katse kõrvaldada moodulid, mis on ainult vajalikud koolitus
3. Kui veebiteenus aktsepteerib sisend ja kui see on loonud väljund

Õnneks kõik kolm astet suunamist klõpsates **määrata Web service** eksperiment lõuend ( **ennustav Web service** suvandi valimine) allosas.

Kui klõpsate käsku **Määra Web service**, mitu asja:

- Koolitatud mudel salvestatakse ühe **Koolitatud mudel** mooduli moodul palett vasakul eksperiment lõuend (leiad selle alusel **Koolitatud mudelid**).
- Moodulid, mida kasutati koolitus eemaldatakse. Aasta juulis.
  - [Kaks klassi võimendatud Otsustepuu][two-class-boosted-decision-tree]
  - [Rongi mudel][train-model]
  - [Jagada andmed][split]
  - teine [R skripti käivitada] [ execute-r-script] moodul, mida kasutati katse
- Salvestatud koolitatud mudel lisata eksperiment.
- Lisatakse **veebi teenuse sisend** ja **väljund veebi teenus** moodulid.

> [AZURE.NOTE] Eksperiment on salvestatud kahes osas jaotises vahekaardid, eksperiment lõuend ülaosas lisatud: esialgsel katsel kasutatud koolitus on **koolitus katsetada**ja vastloodud sõnastikupõhine eksperiment on all **seoses eksperimenteerida**.

Me peame selle konkreetse katse oneadditional juhist.
Meil on kaks [R skripti käivitada] [ execute-r-script] moodulid ja -eksami andmete kaalumise funktsioon ette. Me ei vaja seda teha lõplik mudel.

Masin õppe Studio eemaldada ühe [R skripti käivitada] [ execute-r-script] moodul [Split] eemaldamisel[ split] moodul. Nüüd saab nüüd eemaldada teine ja ühendage [Metaandmete Editor] [ metadata-editor] saadetavad [Keskmine mudel][score-model].    

Meie katse peaks nüüd välja nägema selline:  

![Hinded koolitatud mudel][4]  

> [AZURE.NOTE] Siis võib küsida, miks me jätsime UCI Saksa krediitkaardi andmed andmekomplekti sõnastikupõhine eksperiment. Teenust saab kasutada kasutaja andmed, mitte originaal andmekomplekti, nii miks jäta originaal andmekomplekti mudel?
>
>On tõsi, et teenus ei pea krediitkaardi algandmete. Kuid see ei vaja skeemi andmeid, mille hulka kuuluvad näiteks veergude arvu on ja milliseid veerge on numbriline. Selle skeemi teavet on vaja tõlgendada andmeid. Jätame need komponendid, mis on ühendatud nii, et hinded moodul on andmekogumi skeem kui teenus töötab. Andmeid ei kasutata, vaid skeemi.  

Käivitada katse viimast korda (kliki **käivitada**.) Kui soovite kontrollida, kas mudel ikka toimib, klõpsake toodangu [Keskmine mudel] [ score-model] moodul ja valige **Kuva tulemid**. Näete, et algsed andmed kuvatakse koos krediidi riski väärtus ("viskas sildid") ja hinded tõenäosuse arvutamiseks ("viskas tõenäosuste".) 

## <a name="deploy-the-web-service"></a>Veebiteenuse juurutamine

Saab kasutada katse kas klassikaline veebiteenusele või vastavalt Azure ressursihaldur uus veebiteenus.

### <a name="deploy-as-a-classic-web-service"></a>Kasutada on klassikaline Web Service ###

Klassikaline veebiteenus, mis on saadud meie eksperiment juurutamiseks vajuta **Juurutamise veebiteenus** , lõuend ja valige **Juurutamise veebiteenus [Classic]**. Masin õppe Studio juurutab eksperiment on veebipõhine teenus ja viib teid veebiteenusele armatuurlaualt. Siit saab tagasi katse (**Vaata hetktõmmise** või **Vaata Viimane**) ja lihtne katse veebiteenuse (vt **katse veebiteenuse** allpool). Seal on teavet siin luua rakendusi, mis võivad pääseda Web (sellest pikemalt järgmises etapis see teid läbi).

![Web service armatuurlaud][6]

Saate konfigureerida teenuse **konfiguratsiooni** vahekaardil klõpsates. Siin saate muuta teenuse nimi (see on andnud eksperiment nimi vaikimisi), see lühikirjeldus. Sisend ja väljund veergude võite anda rohkem sõbralik silte.  

![Konfigureerida Web service][5]  

### <a name="deploy-as-a-new-web-service"></a>Juurutada uus Web teenusena

Juurutada uus veebiteenus, mis on saadud meie eksperiment, vajuta **Juurutamise veebiteenus** lõuend ja **Juurutada Web Service [uus]**. Masin õppe Studio üle Azure masin õppe teenuste juurutamine katse veebilehele.

Sisestage veebiteenuse nimi ja valige hinnakujunduse kava. Kui teil on olemasoleva hinnakujunduse kava, saate selle valida, muidu peate looma teenuse hind aasta. 

1.  **Hinnad** ripploendis valige olemasoleva energiarežiimi või **Valige uue** võimaluse.
2.  **Plaani nimi**tippige nimi, mis tuvastab teie arvel kava.
3.  Valige üks **Kuu plaani käik**. Kava kolmeks kategooriaks vaikimisi konna default Web service kavad juurutatakse sellesse piirkonda.

Klõpsake **Deploy** ja oma Web avab **Quickstart** lehekülje.

Teenuse saab konfigureerida, kui klõpsate nuppu **Konfigureeri** menüüvalik. Siin saate muuta teenuse nimi (see on andnud eksperiment nimi vaikimisi), see lühikirjeldus. 

Valige veebi teenus testimiseks klõpsake menüüvalik **Test** (vt **Test veebiteenuse** allpool). Luua rakendusi, mis võivad pääseda Web saamiseks klõpsake **tarbida** menüüvalik (sellest pikemalt järgmises etapis see teid läbi).

> [AZURE.TIP] Pärast juurutamist see veebiteenus värskendamiseks. Näiteks kui soovite muuta oma mudeli, muuta koolituse eksperiment, näpistama mudeli parameetrid ja klõpsake **Juurutada veebiteenus**. Valige **Juurutamise veebiteenus [Classic]** või **Juurutamise veebiteenus [uus]**. Katse uuesti juurutamisel asendab veebiteenuse, kasutades nüüd uuendatud mudel.  

## <a name="test-the-web-service"></a>Test Web service

### <a name="test-a-classic-web-service"></a>Test klassikaline veebiteenusele

Masin õppe Studio või Azure masin õppe veebiteenuste portaali teenust testida. Azure masin õppe veebiteenuste portaali testid on eelis võimaldab teil võimaldada 

**Masin õppe Studio test**

**Armatuurlaualehe,** nuppu **testi** alusel **Vaikimisi lõpp-punkti**. Dialoog pop up ja küsida teenuse andmeid sisestada. Need on sama veerud, mis ilmus algne Saksa krediidi risk andmekomplekti.  

Sisestage andmed ja klõpsake nuppu **OK**. 

**Azure masin õppe veebiteenuste portaali proovile**

**Armatuurlaualehe,** klõpsake jaotises **Default tulemusnäitaja**link **testi** eelvaade. Veebi teenuse lõpp-punkti portaalis Azure masin õppe veebiteenuste testlehte avab ja küsib andmeid sisestada teenuse. Need on sama veerud, mis ilmus algne Saksa krediidi risk andmekomplekti.

Klõpsake nuppu **Test taotluse vastus**. 

Veebiteenuses, andmed sisestab **veebi teenuse sisend** moodul, [Metaandmete Editor] kaudu[ metadata-editor] moodul, ning [Võlgnevuste mudelile] [ score-model] moodul, kus see hinne. Tulemused on siis toodangut veebiteenuse kaudu **Web toodangu**põhjal.

**Test uus veebiteenus**

Azure masin õppe teenuste veebiportaali, klõpsake lehe ülaosas **Test** . **Test** avaneb ja sisestatud andmeid teenuse. Kuvatud väljad vastavad veerud, mis ilmus algne Saksa krediidi risk andmekomplekti. 

Sisestage andmed ja klõpsake nuppu **Test taotluse vastus**.

Testi tulemused kuvatakse väljund veeru lehe parempoolses servas. 

Kui uuritakse portaalis Azure masin õppe veebiteenused, saate lubada valimi andmeid, mille abil saab testida päringu vastuse teenus. Kui lõite veebiteenuse masin õppe Studio, näidisandmete võetakse andmed oma kasutatud mudelis koolitada.

> [AZURE.TIP] Nii on meil konfigureeritud, sõnastikupõhine eksperiment kogu tuleneb [Keskmine mudel] [ score-model] moodul saadetakse tagasi. See hõlmab kõiki sisendandmete pluss krediidi riski väärtus ja hinded tõenäosus. Kui sa tahtsid tagasi midagi muud - näiteks ainult krediidi riski väärtus - siis võiks lisada [Projekti veerud] [ project-columns] moodul vahel [Keskmine] [ score-model] ja **Web toodangu** veergude veebiteenuse tagasi ei soovi kaotada. 

## <a name="manage-the-web-service"></a>Hallata Web service

**Hallata klassikaline veebiteenusele**

Kui klassikaline veebiteenuse juurutamist saate hallata see [klassikaline Azure'i portaal](https://manage.windowsazure.com).

1. [Klassikaline Azure portaali](https://manage.windowsazure.com)sisse logida.
2. Klõpsake paanil Microsoft Azure teenused **Kohapeal ÕPET**.
3. Klõpsake oma tööruumi.
4. Klõpsake vahekaardil **Web teenused** .
5. Klõpsake lõime veebiteenus.
6. Klõpsake nuppu "default" lõpp-punkti.

Siit saate teha näiteks jälgida, kuidas veebiteenuse teeb ja teha tulemuslikkuse lisasid mitu kõnede teenuse muutes saavad hakkama.
Võite isegi kirjutada oma veebiteenuse Azure Marketplace.

Üksikasjalikumat, Vaata:

- [Lõpp-punktide loomine](machine-learning-create-endpoint.md)
- [Web service tagi](machine-learning-scaling-webservice.md)
- [Azure masin õppe veebiteenuse avaldada Azure Marketplace](machine-learning-publish-web-service-to-azure-marketplace.md)

**Hallata Azure masin õppe veebiteenuste portaali veebiteenusele**

Kui juurutamist oma veebiteenuse, klassikaline või uus, saate seda hallata [Azure masin õppe teenuste portaal](https://servics.azureml.net).

Jälgida oma Web teenust:

1. [Azure masin õppe Web teenuste portaali](https://servics.azureml.net)sisse logida.
2. Klõpsake **veebiteenuste**.
3. Klõpsake oma veebiteenuse.
4. Klõpsake **armatuurlaual**.

----------

**Järgmine: [juurdepääs Web service](machine-learning-walkthrough-6-access-web-service.md)**

[1]: ./media/machine-learning-walkthrough-5-publish-web-service/publish1.png
[2]: ./media/machine-learning-walkthrough-5-publish-web-service/publish2.png
[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/