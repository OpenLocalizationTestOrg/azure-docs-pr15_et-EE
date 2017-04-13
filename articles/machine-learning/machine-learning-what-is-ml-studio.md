<properties 
    pageTitle="Mis on Azure seadme õ Studio? | Microsoft Azure'i"
    description="Azure'i ML Studio-hiirega vahend kiiresti koostamise mudelite valmis kasutada teegist moodulid ja algoritmide kohta ülevaate."
    keywords="Azure'i masina Õppekeskuse, azure ml ml studio"
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
    ms.topic="get-started-article"
    ms.date="09/09/2016"
    ms.author="garye"/>

# <a name="what-is-azure-machine-learning-studio"></a>Mis on Azure seadme õ Studio?

Microsoft Azure'i masina õ Studio on koostööpõhise, hiirega tööriista abil saate koostada, testige ja juurutada ennustav lahendusi oma andmete põhjal. Seadme õ Studio avaldab mudelite nimega veebiteenuseid, mis saab tarbitud hõlpsalt kohandatud rakendused või Ärianalüüsi tööriistade kohta Excelis.

Seadme õ Studio on, kui andmed teadus, ennustav, cloud materjale ja andmete kohtuda.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>Seadme õ Studio interaktiivsed tööruumi

Arendada sõnastikupõhise analüüsi mudeli, saate tavaliselt ühe või mitme allikatest pärit andmete kasutamine, muuta erinevate andmete käsitlemise ja statistikafunktsioonid andmeid analüüsida ja luua tulemuste komplekti. Välja umbes järgmine mudel on iteratiivne protsess. Kui muudate erinevaid funktsioone ja nende parameetrid, tulemused lähenevad seni, kuni olete rahul, et teil on koolitatud ja tõhus mudel.

**Azure seadme õ Studios** annab teile interaktiivne, visuaalne tööruumi hõlpsalt luua, testige ja saades sõnastikupõhise analüüsi mudeli. Saate-hiirega ***andmekomplektide*** ja analüüsi ***moodulid*** interaktiivsed lõuendile, mis ühendab need koos moodustavad on ***katsetada***, mille käivitate arvuti õ Studios. Saades mudeli kujunduse redigeerimise katse, kui soovitud koopia salvestamine ja käivitage see uuesti. Kui olete valmis, teisendada oma ***koolitus katsetamiseks*** on ***sõnastikupõhine katse***ja avaldage see ***veebiteenuse*** nii, et teised pääseb mudelisse.

>[AZURE.TIP] Alla laadida ning printida skeem, mis annab ülevaate arvuti õ Studio võimaluste, leiate [Azure'i masina õ Studio võimaluste ülevaade diagramm](machine-learning-studio-overview-diagram.md).

On programmeerimine ei ole nõutav, andmekomplektide ja moodulid ehitada mudelisse sõnastikupõhise analüüsi ainult visuaalselt ühendamine.

![Azure'i ML Studio skeemi: loomine katsete, paljudest allikatest andmeid lugeda, kirjutage viskas andmete, mudelid.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Seadme õ Studio kasutamise alustamine

Kui sisestate esmalt [Arvutisse õ Studio](https://studio.azureml.net) näete lehe **Avaleht** . Siit saate vaadata dokumente, videoid, veebiseminarid ja muud väärtuslik ressursid otsimine.

Selle ülaosas on kolm vahekaarti: **Avaleht** (kui hakkate), **Studio**ja **Galerii**.

### <a name="studio"></a>Studio

Klõpsake vahekaarti **Studio** ja teil palutakse logida sisse oma Microsofti kontoga või oma töökoha või kooli kontoga. Kui olete sisse loginud, näete järgmisi vahekaarte vasakule:

- **Projektide** - katsete, andmekomplektide, märkmike ja muud ressursid, mis tähistab ühe projekti
- **Katsete** - katsete, mis on loodud, käivitamine, ja mustandina
- **VEEBITEENUSED** - kaudu oma katsete juurutatud veebiteenused
- **Märkmike** - Jupyter märkmikke, mis on loodud
- **ANDMEKOMPLEKTIDE** - andmekomplektide üheks Studio üleslaaditud
- **Koolitatud mudelid** - mudelite on koolitada katsete ja seejärel salvestatud Studio
- **Sätted** – sätted, mille abil saate konfigureerida oma konto ja ressursside kogum.

### <a name="gallery"></a>Galerii

Klõpsake vahekaarti **Galerii** ja lähete Cortana ärianalüüsi galeriisse. Galerii on koht, kus saate andmeid teadlaste ja arendajate kogukond jagada komponendid komplekti Cortana ärianalüüsi abil loodud lahendusi.

Galerii kohta leiate lisateavet teemast [ühiskasutus ja leida lahenduse galeriis Cortana ärianalüüsi](machine-learning-gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Katse komponendid

Katse koosneb andmekomplektide analytical moodulid, mis koos ühendada koostada sõnastikupõhise analüüsi mudel andmetega varustavate. Täpsemalt on lubatud katse need omadused.

- Katse on vähemalt üks andmekomplekti ja ühe mooduli
- Andmekomplektide võib olla ühendatud ainult moodulid
- Moodulid võivad olla ühendatud andmekomplektide või muud moodulid
- Kõik Sisestuskeel Porte moodulid peab olema mõned ühenduse andmevoo
- Kõik nõutavad peab olema seatud iga mooduli parameetrid

Saate luua katse algusest peale ise luua, või saate kasutada olemasolevaid valimi katse mallina. Lisateabe saamiseks vt [kasutamine valimi katsed luua uue katsete](machine-learning-sample-experiments.md).

Näiteks luua lihtsa katse, leiate teemast [loomine lihtsa katse Azure seadme õ Studios](machine-learning-create-experiment.md).

Täielikumat ülevaadet ennustav lahenduse loomine, vt [arendada Azure'i masina õ sõnastikupõhise lahenduse](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Andmekomplektide

Andmekomplekt on andmeid, mis on üles laaditud masina õ Studio nii, et seda saab kasutada modelleerimine käigus. Arvu valimi andmekomplektide on kaasas seadme õ Studio saate katsetada, ja saate üles laadida rohkem andmekomplektide, kui neid vajate. Siin on mõned näited kaasatud andmekomplektide:

- **MPG andmete jaoks eri sõiduautode** - miili gallon (MPG) väärtuste automobiles alaga balloonide, Hobujõud jms kohta.
- **Rindade vähk andmete** - rindade vähi diagnoosimise andmed.
- **Mets tule andmed** – mets fire suurused kirdepiirkonna Portugali.

Kui koostate katse saate valida loendist andmekogumite saadaval vasakul lõuend.

Valimi andmekomplektide kaasatud masina õ Studio loendi leiate teemast [Azure seadme õ Studios andmekogumite kasutada](machine-learning-use-sample-datasets.md).

### <a name="modules"></a>Moodulid

Mooduli on algoritmi, mida saate teha oma andmete põhjal. Seadme õ Studio on arv vahemikus sissepääsu andmefunktsioonid koolitus, saades ja valideerimise protsessid moodulid. Siin on mõned näited ka mooduleid:

- [Teisendamine Hädaabiteabe] [ convert-to-arff] – teisendab .NET seeriasertide andmekomplekti atribuut-suhte faili vorming (Hädaabiteabe).
- [Arvuta põhikooli statistika] [ elementary-statistics] -arvutab põhikooli statistika, nt keskväärtuse standardhälbe, jne.
- [Lineaarse regressioonisirge] [ linear-regression] -online astmiku päritolu vastavalt lineaarse regressioonisirge mudelit, mis loob.
- [Keskmine mudel] [ score-model] -hinded koolitatud liigitamine või regressiooni mudel.

Kui koostate katse saate valida loendist moodulid saadaval vasakul lõuend.  

Mooduli võib-olla kogumi parameetrid, mille abil saate konfigureerida mooduli sisemine algoritmide kohta. Kui valite mooduli lõuendile, kuvatakse mooduli parameetrite lõuend paremal paanil **Atribuudid** . Saate muuta selle paani häälestada oma mudeli parameetrid.

Mõned abi navigeerimise seadme õ algoritmide saadaval suurt teeki, vaadake [teemat Valige algoritmide Microsoft Azure'i masina tundmaõppimiseks](machine-learning-algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Ennustav veebiteenuse juurutamine

Kui ennustav mudelisse on valmis, saate selle nimega veebiteenuse otse masina õ Studio juurutada. Selle protsessi kohta leiate lisateavet teemast [Deploy teenuse Azure seadme õ web](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
