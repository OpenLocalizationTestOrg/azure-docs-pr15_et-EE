<properties
    pageTitle="Kuidas arvuti õ mudeli edenedes katse, on operationalized veebiteenusele | Microsoft Azure'i"
    description="Kuidas oma Azure seadme õ mudeli arendamise kaudu arendus katsetamiseks operationalized Web teenusesse mehaanika ülevaade."
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


# <a name="how-a-machine-learning-model-progresses-from-an-experiment-to-an-operationalized-web-service"></a>Kuidas arvuti õ mudeli edenedes katse, on operationalized veebiteenusele

Azure'i masina õ Studio pakub interaktiivne lõuend, mis võimaldab töötada, käivitamine, testida, ja kordamise on ***katsetamiseks*** sõnastikupõhise analüüsi mudeli. On saadaval moodulid, mida saate mitmesuguste.

* Sisendandmete sisse oma katse
* Töödelda andmeid
* Koolitada mudeli abil arvuti õ algoritmide kohta
* Keskmine mudel
* Tulemuste hindamine
* Väljundi lõplik väärtused

Kui olete oma katse, saate juurutamist ***klassikaline Azure seadme õ veebiteenuse*** või ***uue Azure seadme õ veebiteenuse*** nii, et kasutajad saavad uute andmete saatmine ja vastuvõtmine tagasi tulemused.

Selles artiklis anname ülevaade sellest, kuidas oma arvuti õ mudeli arendamise kaudu arendus katsetamiseks operationalized Web teenusesse mehaanika.

>[AZURE.NOTE] Muud võimalused arendada ja kasutada seadme õppe mudelid on olemas, aga see artikkel keskendub arvuti Õppekeskuse Studio kasutamise. Kuidas luua klassikaline sõnastikupõhise veebiteenuse R arutelu, leiate ajaveebipostitusest [koostamine ja juurutamine sõnastikupõhise Web Apps abil RStudio ja Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx).

Ajal Azure seadme õ Studio eesmärk on aidata teil arendamise ja juurutamine *sõnastikupõhise mudeli*, on võimalik arendada katse, mis ei sisalda sõnastikupõhise analüüsi mudeli Studio abil. Näiteks võib katse lihtsalt sisestada andmeid, töödelda seda ja seejärel väljund tulemused. Sõnastikupõhise analüüsi katse, nagu see-sõnastikupõhise katse nimega veebiteenuse juurutamist, kuid see on lihtsam protsessi kuna pole koolitus või seadme õ mudeli hinded. Ei ole tüüpiline Studio kasutamine sellisel viisil, lisame selle arutelu, et me ei saa anda täielikku selgitus Studio tööpõhimõte.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Arendamise ja sõnastikupõhise veebiteenuse juurutamine

Siin on etapid järgneva tüüpilised lahenduse arendamise ja juurutamine masina õ Studio abil.

![Juurutamise meilivoo](media\machine-learning-model-progression-experiment-to-web-service\model-stages-from-experiment-to-web-service.png)

*Joonis 1 - mudeli tüüpilised sõnastikupõhise analüüsi etapid*

### <a name="the-training-experiment"></a>Katse koolitus

***Koolitus katsetamiseks*** on arendada oma arvuti õ Studios veebiteenuse algse etapp. Koolitus katse eesmärk teile arendamiseks, testige itereerima ja lõpuks koolitada Õppekeskuse mudeli masina koht. Te saate isegi koolitada mitme mudelite korraga saate otsida parim lahendus, kuid kui olete lõpetanud katsetamine teil tuleb valige koolitada ühe modelleerimise ja ülejäänud katse kõrvaldada. Näide arendamise sõnastikupõhise analüüsi katsetamiseks leiate teemast [töötada krediitkaardiga riskianalüüs Azure seadme õppe ennustav lahendus](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="the-predictive-experiment"></a>Sõnastikupõhise katse

Kui olete oma koolitus katse koolitatud mudeli, klõpsake **Seadmine veebiteenuse** ja valige **Sõnastikupõhise veebiteenuse** masina õ Studios teisendamine oma koolitus katse, on ***sõnastikupõhine katse***algatada. Sõnastikupõhise katse eesmärk Keskmine uued andmed, lõpuks muutub operationalized Azure Web teenust eesmärgiga koolitatud mudeli abil.

Teisendamine toimub saate teha järgmist:

-   Teisendamine moodulid kasutatakse koolitus sisse ühe mooduli määramine ja salvestage see koolitatud mudel

-   Kõrvaldada pole seotud hinded mis tahes kõrvalised moodulid

-   Lisage sisend- ja lõpuks veebiteenuse kasutavate pordid

Võib olla veel muudatusi soovite teha oma sõnastikupõhise katse nimega veebiteenuse juurutamine ettevalmistamine. Kui soovite veebiteenuse väljund alamhulka tulemused, saate lisada filtreerimise mooduli enne väljund porti.

Klõpsake selle protsessi tulemus koolitus katse on hüljata. Kui protsess on lõpule jõudnud, on kaks vahekaarti Studios: üks koolitus katse ja teine sõnastikupõhise katse puhul. Muudatusi saate teha katse koolitus, enne oma veebiteenuse juurutada ja taastada sõnastikupõhise katse nii. Või koolitus katse katsetused teise rea alustamiseks koopia salvestada.

>[AZURE.NOTE] **Sõnastikupõhise veebiteenuse** klõpsamisel automaatne protsess teisendada oma koolitus katse sõnastikupõhise katse käivitate ja see töötab enamikel juhtudel korralikult. Kui teie koolitus katse on keeruline (näiteks teil on mitu teed koolitus, mida te kokkuliitmiseks), võib-olla eelistate teha käsitsi ümberarvutamine. Lisateabe saamiseks vt [seadme õ koolitus katse sõnastikupõhise katse teisendada](machine-learning-convert-training-experiment-to-scoring-experiment.md).

### <a name="the-web-service"></a>Veebiteenuse

Kui olete rahul oma sõnastikupõhise katse on valmis, saate juurutada teenust ükskõik kumma klassikaline veebiteenuse nimega või uue veebiteenuse vastavalt Azure'i ressursihaldur. Kiireks mudelisse, kasutades selle *klassikalise masina õ veebiteenuse*, klõpsake **Juurutada veebiteenuse** ja valige käsk **Juurutamine veebiteenuse [Classic]**. Juurutada *uue arvuti õ veebiteenuse*, klõpsake **Juurutada veebiteenuse** ja valige **Juurutamine veebiteenuse [uus]**. Kasutajad saavad abil veebiteenuse REST API mudelist andmeid saata ja vastu võtta tagasi tulemused. Lisateavet leiate teemast [teenuse Azure seadme õ Web, mis on kasutusele võetud Õppekeskuse katse seadmes peab olema](machine-learning-consume-web-services.md).

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>Tüüpilised juhul:-sõnastikupõhise veebiteenuse loomine

Kui oma katse ei koolitada sõnastikupõhise analüüsi mudeli, siis pole vaja luua nii koolitus katse ja hinded katse – on ainult üks katse ja saate juurutamist nimega veebiteenusest. Seadme õ Studio tuvastab, kas teie katse sisaldab prognoositava mudeli analüüsib moodulid, mida olete kasutanud.

Kui olete sisse oma katse näidata ja on rahul:

1.  Klõpsake nuppu **Määra veebiteenuse** ja valige **Ümberõpet veebiteenuse** - sisend- ja sõlmed lisatakse automaatselt

2.  Klõpsake nuppu **Käivita**

3. Klõpsake **Juurutada veebiteenuse** ja valige **Juurutamine veebiteenuse [Classic]** või **Juurutada veebiteenuse [uus]** sõltuvalt sellest, kuhu soovite juurutada keskkonna.

Nüüd juurutatakse oma veebiteenuse ja pääsete juurde ja saate seda hallata nii nagu sõnastikupõhise veebiteenus.

## <a name="updating-your-web-service"></a>Oma veebiteenuse värskendamine

Nüüd, kui juurutamist oma katse, on veebipõhine teenus, mida teha, kui peate värskendama seda?

See sõltub sellest, mida soovite värskendada:

**Soovite muuta sisestada või väljastada või soovite muuta, kuidas veebiteenuse manipuleerib andmed**

Kui muudetav Domeen mudelit, kuid ainult muutuvad, kuidas käsitleb veebiteenuse andmeid, saate redigeerida sõnastikupõhise katse ja siis nuppu **Veebiteenus juurutada** ja **Juurutamine veebiteenuse [Classic]** või **Juurutada veebiteenuse [uus]** uuesti valida. Veebiteenuse peatatakse, värskendatud sõnastikupõhise katse on juurutatud ja veebiteenuse taaskäivitamist.

Siin on näide: Oletame, et oma sõnastikupõhise katse annab vastuseks prognoositud tulemiga sisendandmete kogu rida. Võib juhtuda, et soovite veebiteenuse lihtsalt tagasi tulemus. Nii saate lisada **Project veergude** mooduli sõnastikupõhise katse, väljund porti tulemi peale veergude välistamine enne. Kui klõpsate **Veebiteenuse juurutada** ja valige **Juurutamine veebiteenuse [Classic]** või **Juurutada veebiteenuse [uus]** uuesti, värskendatakse selle veebiteenuse.

**Soovite ümber mudeli uute andmetega**

Kui soovite säilitada oma seadme Õppekeskuse mudeli, kuid soovite selle ümber uute andmetega, on teil kaks võimalust.

1.  **Mudeli veebiteenuse töötamise ümber** – kui soovite ümber mudelisse ajal sõnastikupõhise veebi teenus töötab, saate seda teha, muutes paari muudatusi teha on ***ümberõpe katse***katse koolitus, siis saate juurutamist ** *ümberõpe* veebiteenuse**. Juhised selle tegemiseks leiate teemast [ümber masina õ mudelid programmiliselt](machine-learning-retrain-models-programmatically.md).

2.  **Minge tagasi algse koolitus katse ja kasutada paljudes erinevates andmete mudelisse arendada** - oma sõnastikupõhise katse on seotud veebiteenuse, kuid koolitus katse on lingitud otse sel viisil. Algne koolitus katse muutmisel ja klõpsake nuppu **Määra veebiteenuse**, see loob *uue* sõnastikupõhise katsetada, mille juurutamisel, loob *uue* veebiteenuse. Lihtsalt ei värskendata algse veebiteenus.

    Kui teil on vaja muuta koolitus katse, avage see ja klõpsake nuppu **Salvesta nimega** koopia. See jäta puutumata algse koolitus katse sõnastikupõhise katse, ja veebiteenus. Nüüd saate luua uue veebiteenuse ja teie muudatused. Kui olete juurutatud, siis saate otsustada, kas eelmise veebiteenuse peatamiseks või jätke see töötab koos uue uue veebiteenuse.

**Soovite koolitada erinev mudel**

Kui soovite muuta oma algse sõnastikupõhise katse, nt teise arvutisse õ algoritmi, valides proovimise erinevate koolituse meetodi jne, siis peate järgige eespool kirjeldatud ümberõppe mudelisse teist protseduuri: koolitus katse avatud, klõpsake nuppu **Salvesta nimega** koopia, ja seejärel alustada alla uue tee arendamise mudelisse , ennustava katse loomine ja juurutamine veebiteenuse. See loob uue Web teenuse mitteseotud algsest – saate otsustada, millest üks või mõlemad hoida töötab.

## <a name="next-steps"></a>Järgmised sammud

Arendamise protsessi katse kohta lisateabe saamiseks lugege järgmisi artikleid:

-   katse - [seadme õ koolitus katse sõnastikupõhise katse teisendada](machine-learning-convert-training-experiment-to-scoring-experiment.md) teisendamine

-   veebiteenuse - [Deploy teenuse Azure seadme õ web](machine-learning-publish-a-machine-learning-web-service.md) juurutamine

-   ümberõpe mudel - [ümber masina õ mudelid programmiliselt](machine-learning-retrain-models-programmatically.md)

Näiteid kogu protsessi, leiate artiklist:

-   [Seadme õ õpetus: luua oma esimese katse Azure seadme Õppekeskuse Studio](machine-learning-create-experiment.md)

-   [Juhend: Arendamise krediitkaardiga riskianalüüs Azure seadme õppe ennustav lahendus](machine-learning-walkthrough-develop-predictive-solution.md)