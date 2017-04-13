<properties
    pageTitle="Ühenduse loomine veebiteenuse masina õ | Microsoft Azure'i"
    description="C# või Python, luua ühenduse teenuse Azure seadme õ Web on autoriseerimine võtme abil."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016" 
    ms.author="garye" />


# <a name="connect-to-an-azure-machine-learning-web-service"></a>Azure'i masina õ veebiteenuse ühendamine

Azure'i masina õ Arendaja kogemus on teenuse Veebiteenuste teha Tra sisendandmete reaalajas või kogumitena. Prognoose luua ja juurutada Azure seadme õ Web teenuse Azure seadme õ Studio abil.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Lisateavet selle kohta, kuidas luua ja juurutada masina õ veebiteenuse abil arvuti õ Studio:

- Juhendi loomise katse masina õ Studios, leiate [oma esimese katse loomine](machine-learning-create-experiment.md).
- Veebiteenuse juurutamise kohta täpsema teabe saamiseks vt [Deploy masina õ veebiteenusest](machine-learning-publish-a-machine-learning-web-service.md).
- Lisateabe saamiseks arvuti õ üldiselt, külastage [Seadme dokumentatsiooni õppekeskus](https://azure.microsoft.com/documentation/services/machine-learning/).

## <a name="azure-machine-learning-web-service"></a>Azure'i masina õ veebiteenuse ##

Teenusega Azure seadme õ Web rakenduse suhtleb arvuti õ töövoo hinded mudeli reaalajas. Seadme õ veebiteenuse kutse tagastab ennustamine tulemuste rakenduse. Veendumaks, et arvuti õ veebiteenuse helistada, te kaotate API võti, mis luuakse siis, kui juurutate ennustamiseks. Seadme õ veebiteenuse põhineb ülejäänud, populaarsed arhitektuur valik veebi programmeerimine projektide jaoks.

Azure'i masina õ on kahte tüüpi teenuseid.

- Päringu vastuse teenuse (RRS) – madal latentsus, väga paindlik teenus, mis sisaldab kasutajaliidese kodakondsuseta mudelid luuakse ja juurutatud masina õ Studio.
- Paketi täitmise teenuse (BES) on asünkroonne teenuse et hinded on andmed kirjete paketi.

Seadme õ veebiteenuste kohta leiate lisateavet teemast [Deploy masina õ veebiteenus](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Saada on Azure seadme õ autoriseerimine võti ##

Kui juurutate oma katse, luuakse veebiteenuse API võtmed. Saate tuua mitu asukohtadest võtmed.

## <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>Microsoft Azure'i masina õ veebiteenuste portaalis

[Microsoft Azure'i masina õ veebiteenuste](https://services.azureml.net) portaali sisse logida.

Tuua uue arvuti õ veebiteenuse API võti:

1. Azure'i masina õ veebiteenuste portaalis, klõpsake **Veebiteenuste** top menüü.
2. Klõpsake nuppu veebiteenus, mille soovite alla laadida võti.
3. Klõpsake menüü top **tarbida**.
4. Kopeerige ja salvestage **Primaarvõti**.


Toomiseks klassikaline masina õ veebiteenuse API võti:

1. Klõpsake Azure seadme õ veebiteenuste portaalis **Klassikaline veebiteenuste** top menüü.
2. Klõpsake nuppu veebiteenus, millega töötate.
3. Klõpsake nuppu lõpp-punkti, mille soovite alla laadida võti.
3. Klõpsake menüü top **tarbida**.
4. Kopeerige ja salvestage **Primaarvõti**.

## <a name="classic-web-service"></a>Klassikaline veebiteenuse ##

 Klassikaline veebiteenuse klahvi saate alla laadida ka arvuti õ Studio või Azure portaali kaudu.

### <a name="machine-learning-studio"></a>Seadme õ Studio ###

1. Seadme õ Studios, klõpsake vasakul nuppu **VEEBITEENUSED** .
2. Klõpsake nuppu veebiteenus. Vahekaart **ARMATUURLAUD** on **API võti** .

### <a name="azure-portal"></a>Azure'i portaal ###

1. Klõpsake vasakul nuppu **Arvuti õ** .
2. Klõpsake tööruumi, kus asub teie veebiteenus.
3. Klõpsake **VEEBITEENUSED**.
4. Klõpsake nuppu veebiteenus.
5. Klõpsake nuppu lõpp. "API võti" on alla alumises parempoolses nurgas.

## <a id="connect"></a>Ühenduse loomine veebiteenuse masina õ

Saate luua ühenduse arvuti õ veebiteenuse abil programmeerimiskeel, mis toetab HTTP-päringu ja vastuse. Saate vaadata näited C#, Python ja R veebilehelt masina õ teenuse spikker.

**Seadme õ API spikker** Seadme õ API abi luuakse veebiteenuse juurutamisel. Leiate [Azure'i masina õ kiirtutvustus - veebiteenuse juurutamine](machine-learning-walkthrough-5-publish-web-service.md).
Seadme õ API spikker sisaldab üksikasjalikku teavet ennustamiseks veebiteenus.

2. Klõpsake nuppu veebiteenus, millega töötate.
3. Klõpsake nuppu lõpp-punkti, mille jaoks soovite vaadata API spikri lehel.
3. Klõpsake menüü top **tarbida**.
3. Klõpsake **API aitab lehe** jaotises päringu vastuse või paketi täitmise lõpp-punktid.

**Vaate arvuti õ API uue veebiteenuse spikker**

Azure'i kohapeal Õppekeskuse Web Services portaal:

1. Klõpsake menüü top **VEEBITEENUSED** .
2. Klõpsake nuppu veebiteenus, mille soovite alla laadida võti.

Klõpsake soovitud URI-d saamiseks taotluse-Reposonse ja paketi täitmise teenused ja proovi kood C#, R ja Python **tarbida** .

Klõpsake nuppu Saada ärplema vastavalt dokumentatsioonist API-de esitatud URI-d nimega **Ärplema API** .

### <a name="c-sample"></a>C# näidis ###

Ühenduse loomiseks arvuti õ veebiteenuse, kasutada mõne **HttpClient** läbides ScoreData. ScoreData sisaldab FeatureVector, n mitmedimensioonilised arvväärtuste funktsioonid vektorkuju, mis tähistab soovitud ScoreData. Saate autentida masina õ teenusega on API võti.

Ühenduse loomiseks arvuti õ veebiteenuse, peab olema installitud **Microsoft.AspNet.WebApi.Client** Nugeti pakett.

**Installige Microsoft.AspNet.WebApi.Client Nugeti Visual Studio**

1. Allalaadimine: UCI andmekomplekti avaldamine: täiskasvanud 2 klassi andmekomplekti veebiteenus.
2. Klõpsake **menüü Tööriistad** > **Nugeti Package Manager** > **Package Manager konsooli**.
2. Klõpsake nuppu **Installi pakett Microsoft.AspNet.WebApi.Client**.

**Proovi kood käivitamiseks**

1. Avaldamine "näide 1: andmekomplekti alla laadida UCI: täiskasvanu 2 klassi andmekomplekti" katse, seadme õ valimi kollektsiooni.
2. Määrake apiKey võti veebiteenusest. Vt **on Azure seadme õ autoriseerimine võtme** ülaltoodud.
3. Määrake serviceUri taotlemine URI-ga.


### <a name="python-sample"></a>Python näidis ###

Seadme õ veebiteenuse ühendamiseks kasutage läbides ScoreData **urllib2** teek. ScoreData sisaldab FeatureVector, n mitmedimensioonilised arvväärtuste funktsioonid vektorkuju, mis tähistab soovitud ScoreData. Saate autentida masina õ teenusega on API võti.


**Proovi kood käivitamiseks**

1. Juurutamine "näide 1: andmekomplekti alla laadida UCI: täiskasvanu 2 klassi andmekomplekti" katse, seadme õ valimi kollektsiooni.
2. Määrake apiKey võti veebiteenusest. Jaotisest **on Azure seadme õ autoriseerimine võtme** lähedal alguses käesoleva artikli.
3. Määrake serviceUri taotlemine URI-ga.
