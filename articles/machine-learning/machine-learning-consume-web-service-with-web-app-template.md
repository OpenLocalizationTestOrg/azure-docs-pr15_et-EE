<properties
    pageTitle="Seadme õ veebiteenuse malliga web appi kasutamine | Microsoft Azure'i"
    description="Azure'i turuplatsi web app malli abil tarbimine sõnastikupõhise veebiteenuse Azure seadme õppe."
    keywords="veebiteenuse, tulemustabeli kasutuselevõtmise üle ka, REST API-ga masina õpetused"
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
    ms.date="10/10/2016"
    ms.author="garye;raymondl"/>

# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Azure'i masina õ veebiteenuse, web appi malli kasutamine

>[AZURE.NOTE] Selles teemas kirjeldatakse meetodite kehtivad klassikaline veebiteenuse abil. 

Üks kord olete sõnastikupõhise mudelisse väljatöötatud ja juurutatud Azure veebiteenuse, seadme õ Studio abil või tööriistadega nagu R või Python, pääsete operationalized mudeli REST API abil.

On mitmeid võimalusi, kuidas kasutada REST API-ga ja juurdepääs veebiteenuse. Näiteks saate kirjutada rakenduse C#, R või Python abil loodud teile (saadaval API spikri lehel web teenuse armatuurlaua masina õ Studios) veebiteenuse juurutamisel proovi kood. Või kasutage valimi Microsoft Exceli töövihik loodi teile (saadaval ka web teenuse armatuurlaua Studios).

Kuid kiireim ja lihtsaim viis oma veebiteenuse juurdepääsuks on saadaval [Azure Web Appi turuplatsi](https://azure.microsoft.com/marketplace/web-applications/all/)Web Appi mallide abil.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>Õppekeskuse Web Appi mallide Azure seade

Rakenduse veebimallide saadaval Azure'i turuplatsil saate koostada kohandatud veebirakenduse, teab oma veebiteenuse sisendandmete ja oodatud tulemusi. Teha pole vaja anda oma veebiteenuse ja andmete web app access ja malli ei ülejäänud.

Kahe Mallid on saadaval.

- [Azure'i ML päringu vastuse teenuse Web Appi Mall](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
- [Azure'i ML paketi täitmise teenuse Web App Mall](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Iga mall loob valimi ASP.net-i rakendused oma veebiteenuse API URI ja võti kasutamise ja kasutab seda Azure veebilehena. Päringu vastuse teenuse (RRS) mall loob web appi, mis võimaldab teil ühe andmerea saata veebiteenuse ühe tulemuse. Paketi täitmise teenuse (BES) mall loob web appi, mis võimaldab saata palju andmeread mitme tulemuse saamiseks.

Pole kodeerimine on vaja need mallide kasutamise kohta. Te lihtsalt sisesta API URI ja võti ja mall loob teie taotlus.

## <a name="how-to-use-the-request-response-service-rrs-template"></a>Kuidas kasutada malli päringu vastuse teenuse (RRS)

Kui teie veebiteenuse juurutamist järgite malli RRS web app, nagu on näidatud järgmisel joonisel alltoodud juhiseid.

![Protsessi RRS web malli kasutamiseks][image1]

1. Seadme õ Studios, avage **Veebiteenuste** menüü ja seejärel veebiteenuse, millele soovite juurde pääseda. Kopeerige võti loendis **API võti** ja salvestage see.

    ![API võti][image3]

2. Avage leht **Taotluse/vastuse** API spikker. **Koosolekukutse**, klõpsake jaotises spikker lehe ülaosas kopeerige **Taotlemine URI** väärtus ja salvestage see. See väärtus näeb välja umbes järgmine:

        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true

    ![Taotluse URI][image4]

3. Minge [Azure portaali](https://portal.azure.com) **sisselogimine**, klõpsake nuppu **Uus**, otsige ja valige **Azure ML päringu vastuse teenuse Web App**ja seejärel klõpsake nuppu **Loo**. 

    - Andke oma veebirakenduse kordumatu nimi. Veebirakenduse URL on selle nime, millele järgneb `.azurewebsites.net.` näiteks`http://carprediction.azurewebsites.net.`

    - Valige Azure tellimuse ja teenuseid, mis töötab teie veebiteenus.

    - Klõpsake nuppu **Loo**.

    ![Veebirakenduse loomine][image5]

4. Kui Azure on lõpule jõudnud, juurutamine veebirakenduse, klõpsake Azure web appi sätete lehe **URL-i** või sisestage URL-i veebibrauseris. Näiteks`http://carprediction.azurewebsites.net.`

5. Veebirakenduse esmakordsel käivitamisel ta küsib **API postituse URL-i** ja **API võti**.
Sisestage väärtused, mis on salvestatud mõnes varasemas versioonis.
    - **Taotleda URI** API aitab lehe **API postituse** URL-i
    - **API võti** web teenuse armatuurlaual **API võti**.

    Klõpsake **esitada**.

    ![Sisestage postituse URI ja API võti][image6]

6. Veebirakenduse kuvab selle **Web Appi konfigureerimine** lehest, kus on praeguse veebiteenuse sätted. Siin saate muudatusi teha veebirakenduse kasutatavad sätted.

    > [AZURE.NOTE] Siinsed sätted muutmine ainult muudatusi need selles veebirakenduse. See ei muuda oma veebiteenuse vaikesätted. Kui muudate siia **Kirjeldus** see ei muuda kuvatud arvuti õ Studios armatuurlaual web teenuse kirjeldus.

    Kui olete lõpetanud, klõpsake nuppu **Salvesta muudatused**ja seejärel klõpsake nuppu **Mine avalehele**.

7. Saate sisestada avalehel väärtused oma veebiteenuse saatmiseks klõpsake nuppu **Edasta**ja tagastatakse tulemuseks.

Kui soovite **konfiguratsiooni** lehele naasmiseks, minge selle `setting.aspx` lehe veebirakenduse. Näide: `http://carprediction.azurewebsites.net/setting.aspx.` teil palutakse sisestada API võti uuesti – peate mis lehe juurdepääsu ja nende sätteid värskendada.

Saate peatada, taaskäivitage või Azure'i portaalis nagu web appi veebirakenduse kustutamine. Kui see töötab saate liikuge home veebiaadress ja sisestage uued väärtused.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>Kuidas kasutada malli paketi täitmise teenuse (BES)

BES web appi malli saate kasutada samamoodi nagu RRS Mall, välja arvatud, et veebirakenduse, mis on loodud võimaldab teil mitme rea andmete edastamiseks ja vastu võtta mitu tulemit.

Paketi täitmise veebiteenusest tulemused talletatakse Azure storage ümbrises; sisendväärtuste võivad pärineda Azure salvestusruumi või kohalik fail.
Nii, peate Azure storage nõus pikalt veebirakenduse tulemite ja peate teie sisendandmete ettevalmistamine.

![Protsessi BES web malli kasutamiseks][image2]

1. Kasutage sama toimingut peale RRS malli, nagu BES veebirakenduse loomiseks:
    - Saada **Koosolekukutse URI** **Paketi täitmise** API aitab lehel veebiteenuse.
    - Avage [Azure'i ML paketi täitmise teenuse Web Appi malli](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) avamiseks Azure turuplatsilt BES Mall ja klõpsake **Veebirakenduse loomine**.

2. Määramaks, kus soovite tulemeid, mis on talletatud, sisestage teave sihtkoha container avalehel web app. Kus saan web appi sisendväärtuste kohaliku faili või mõne Azure storage container määrata.
Klõpsake **esitada**.

    ![Salvestamise teave][image7]

Veebirakenduse kuvatakse töö oleku leht.
Kui töö on valmis antakse teile asukoha Azure'i bloobimälu tulemusi. Teil on ka võimalus kohaliku faili allalaadimiseks tulemused.

## <a name="for-more-information"></a>Lisateabe saamiseks

Lisateavet...

- luua seadme Õppekeskuse Studio masina õ katse, vt [luua oma esimese katse Azure seadme Õppekeskuse Studios](machine-learning-create-experiment.md)

- teie arvuti õ juurutamise katsetamiseks nimega veebiteenuse leiate [Deploy Azure seadme Õppekeskuse veebiteenuse](machine-learning-publish-a-machine-learning-web-service.md)

- muud võimalused oma veebiteenuse teada, [Kuidas Azure'i masina õ web teenuse kasutamine](machine-learning-consume-web-services.md)


[image1]: media\machine-learning-consume-web-service-with-web-app-template\rrs-web-template-flow.png
[image2]: media\machine-learning-consume-web-service-with-web-app-template\bes-web-template-flow.png
[image3]: media\machine-learning-consume-web-service-with-web-app-template\api-key.png
[image4]: media\machine-learning-consume-web-service-with-web-app-template\post-uri.png
[image5]: media\machine-learning-consume-web-service-with-web-app-template\create-web-app.png
[image6]: media\machine-learning-consume-web-service-with-web-app-template\web-service-info.png
[image7]: media\machine-learning-consume-web-service-with-web-app-template\storage.png
