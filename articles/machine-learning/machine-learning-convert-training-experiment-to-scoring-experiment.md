<properties
    pageTitle="Seadme õ koolitus katse teisendamine sõnastikupõhise katse | Microsoft Azure'i"
    description="Kuidas muuta arvuti õ koolitus katse, kasutatakse koolitus mudelisse ennustav sõnastikupõhise katse, mida saab kasutada nimega veebiteenuse abil."
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
    ms.date="08/19/2016"
    ms.author="garye"/>

# <a name="convert-a-machine-learning-training-experiment-to-a-predictive-experiment"></a>Seadme õ koolitus katse teisendamine sõnastikupõhise katse

Azure'i masina õ võimaldab teil koostada, testige ja juurutada ennustav lahendusi.

Kui olete loonud ja klõpsake soovitud *koolitus katsetamiseks* mudelisse ennustav koolitada näidata ja olete valmis seda kasutada uute andmete Keskmine, peate ette valmistada ja sujuvamaks muutmine oma katse jaoks hinded. Seejärel seda *sõnastikupõhise katse* Azure web teenust juurutada, nii, et kasutajad saavad mudelist andmeid saata ja vastu võtta teie mudeli prognoose.

Teisendades sõnastikupõhise katse valmistute koolitatud mudelisse nimega veebiteenuse kasutusele võtta. Kasutajate veebiteenuse saadab sisendandmete mudelisse ja mudeli saadab tagasi ennustamine tulemused. Nii nagu teisendamist sõnastikupõhise katse on soovite pidage meeles, kui eeldate oma mudeli kasutada.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Koolitus katse teisendamine sõnastikupõhise katse hõlmab kolm toimingut:

1.  Salvestada seadme õ mudelit, mis te olete, ja seejärel nuppu Asenda Õppekeskuse algoritmi ja [Rongi mudeli] masina[ train-model] moodulid oma salvestatud koolitatud mudel.
2.  Kärpida, et katse ainult moodulid, mis on vajalikud hinded. Koolitus katse sisaldab mitmeid moodulid, mis on vajalikud koolitus, kuid ei ole vaja, kui mudeli on koolitatud ja kasutusvalmis ja hinded.
3.  Määratlemine, kus kuvatakse teie katse nõustuge andmeid web teenuse kasutaja ja milliseid andmeid ei tagastata.

## <a name="set-up-web-service-button"></a>Määrake veebiteenuse nupp

Oma katse (katse lõuendi allosas nuppu**Käivita** ) käivitate nupp **Web Service** ( **Sõnastikupõhine veebiteenuse** suvandi valimine) teha teile kolme juhiseid oma koolitus katse teisendamine sõnastikupõhise katse:

1.  See salvestab koolitatud mudelisse mooduli mooduli värvipaleti (katse lõuendi vasakul), et jaotises **Koolitatud mudelite** asendab Õppekeskuse algoritmi ja [Rongi mudeli] masina[ train-model] moodulid salvestatud koolitatud mudeliga.
2.  See eemaldab moodulid, mis on ei vaja. Selles näites see rühm sisaldab [Tükeldatud andmetega][split], 2 [Keskmine mudeli][score-model], ja [Hindab mudeli] [ evaluate-model] moodulid.
3.  See loob Web teenuse sisend ja väljund moodulid ja lisab need vaikeasukohta, mis teie katse.

Näiteks järgmine katse rongid kaks klassi võimendatud otsust puu mudeli rahvaloendus Näidisandmete abil:

![Koolitus katse][figure1]

See katse moodulid tehke põhiliselt nelja erinevaid funktsioone.

![Mooduli funktsioonid][figure2]

See koolitus katse teisendamisel sõnastikupõhise katse osa need moodulid ei ole enam vaja või nad on erinevate eesmärk.

- **Andmete** - andmete näidis tuvastatavate ei kasutata ajal hinded – kasutaja veebiteenuse esitab reageerima andmed. Siiski metaandmete kaudu tuvastatavate, nt andmetüübid kasutavad koolitatud mudel. Nii, et peate võtma andmekomplekti sõnastikupõhise katse nii, et see pakkuda seda metaandmete.

- **Prep** - andmed, mida saab esitada hinded, sõltuvalt nende moodulid võib või ei pruugi olla vajalik sissetulevate andmete töötlemiseks.

    Näiteks selles näites valimi andmekomplekti võib olla puuduvate väärtuste ja see sisaldab veerud, mida on vaja koolitada mudel. Nii [Clean puuduvad andmed] [ clean-missing-data] mooduli lisati lahendada puuduvate väärtuste ja [Veergude valimine andmekomplekti] [ select-columns] mooduli on kaasatud nende eest veergude välistamine andmete voogu. Kui teate, et andmeid, mis esitatakse jaoks hinded veebiteenuse kaudu ei ole puuduvate väärtuste, siis saate eemaldada [Clean puuduvad andmed] [ clean-missing-data] mooduli. Siiski, kuna [Andmehulga veergude valimine] [ select-columns] mooduli aitab määratleda tabamused funktsioonidega, et mooduli peab jääma.

- **Rongi** - kui mudel on edukalt koolitada, saate salvestada ühe koolitatud mudeli mooduli. Saate Asendage need üksikud moodulid salvestatud koolitatud mudel.

- **Keskmine** - tükeldatud mooduli selles näites kasutatakse andmevoos kogumi testi ja koolitus andmeid jagada. Sõnastikupõhise katse pole vaja ja saab eemaldada. Samamoodi 2 [Keskmine mudeli] [ score-model] mooduli ja [Hinnata mudeli] [ evaluate-model] mooduli kasutatakse võrrelda andmetest testi tulemused, nii et need moodulid on ka ennustava katse ei vaja. Ülejäänud [Keskmine mudeli] [ score-model] mooduli, aga vaja tagastada tulemuseks Keskmine veebiteenuse kaudu.

Käesolevas näites pärast nupu **Määrata veebiteenuse**näeb selline:

![Teisendatud sõnastikupõhise katse][figure3]

See võib olla piisavalt ette valmistada oma katse nimega veebiteenuse kasutusele võtta. Aga soovite teha mõne kindla oma katse lisatööd.

### <a name="adjust-input-and-output-modules"></a>Reguleerida sisend- ja moodulid

Oma koolitus katse, kasutatakse koolitus andmed ja seejärel ei saada andmed vormi, mida seadme õ algoritmi vaja mõned töötlemine. Kui eeldate, et saada veebiteenuse kaudu andmeid ei pea see töötlemine, saate teisaldada **Web teenuse Sisestuskeel mooduli** erineva sõlme oma katse.

Näiteks vaikimisi **Määratud veebiteenuse** paneb **Web teenuse sisendi** mooduli ülaosas oma andmevoog, nagu on näidatud ülaltoodud joonisel. Juhul, kui sisendandmete ei pea see töötlemine, siis saate käsitsi asetada **Web teenuse sisestatud** andmete töötlemise moodulid viimase:

![Web teenuse sisendi teisaldamine][figure4]

Sisendandmete veebiteenuse kaudu läheb nüüd otse Keskmine mudeli mooduli ilma mis tahes eeltöötlus.

Samuti vaikimisi **Seadmine veebiteenuse** paneb Web teenuse väljundi mooduli oma andmevoo allosas. Selles näites naaseb veebiteenuse kasutaja [Keskmine mudeli] väljund[ score-model] moodul, mis sisaldab täieliku sisendandmete vektori pluss loendamine tulemused.

Juhul, kui eelistate tagastamiseks midagi muud – nt, ainult hinded tulemused ja pole kogu vektorkuju sisendandmete -, siis saate lisada [Andmekomplekti veergude valimine] [ select-columns] mooduli välistamine kõik veerud peale hinded tulemused. Seejärel teisaldate **Web teenuse väljundi** mooduli [Andmekomplekti veergude valimine] väljundi[ select-columns] mooduli:

![Web teenuse väljundi teisaldamine][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Lisage või eemaldage täiendavate andmete töötlemise moodulid

Kui leidub veel moodulid oma katse, mida te ei tea, ei ole tarvis ajal hinded, saate need eemaldada. Näiteks, kuna oleme liikunud **Web teenuse sisendi** mooduli punkti pärast andmete töötlemise moodulid, saame eemaldada [Clean puuduvad andmed] [ clean-missing-data] mooduli sõnastikupõhise katse.

Meie sõnastikupõhise katse nüüd näeb välja umbes järgmine:

![Täiendavad mooduli eemaldamine][figure6]

### <a name="add-optional-web-service-parameters"></a>Valikuline Web teenuse parameetrite lisamine

Mõnel juhul soovite lubada kasutajal oma veebiteenuse muutmine moodulid toimimist, kui teenus on kättesaadav. *Web teenuse parameetrite* abil, saate seda teha.

[Andmete importimine] häälestamine on näide[ import-data] mooduli nii, et kasutaja juurutatud veebiteenuse saate määrata muud andmeallikat, kui veebiteenuse on juurdepääs. Või konfigureerida [Andmete eksportimine] [ export-data] mooduli nii, et eri sihtkoha saab määrata.

Saate veebis teenuse parameetrite määratlemine ja ühe või mitme mooduli parameetrite seostada, ja saate määrata, kas need on nõutav või valikuline. Veebiteenuse kasutaja saab seejärel andke väärtused järgmiste parameetrite kui teenus on kättesaadav ja mooduli toimingud on muuta.

Web teenuse parameetrite kohta lisateabe saamiseks vt [Abil Azure seadme õ Web teenuse parameetrid ] [ webserviceparameters].

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>Sõnastikupõhise katse nimega veebiteenuse juurutamine

Nüüd, kui sõnastikupõhise katse piisavalt valmis, saate selle juurutada Azure web teenust. Veebiteenuse abil kasutajad saavad saata andmete mudelisse ja mudeli tagastab selle prognoose.

Protsessi lõpetamine juurutamise kohta leiate lisateavet teemast [Deploy teenuse Azure seadme õ web][deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
