<properties
    pageTitle="Juurutada masinõpet veebiteenusele | Microsoft Azure"
    description="Kuidas teisendada koolitus eksperiment sõnastikupõhine eksperiment, valmistuda juurutamine siis Azure'i masinõppe web teenust kasutada."
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

# <a name="deploy-an-azure-machine-learning-web-service"></a>Azure'i masinõppe veebiteenuse juurutamine

Azure'i masinõpet saate ehitada, katsetada ja juurutada sõnastikupõhise analüütiline lahendusi.

Kõrgetasemeline-of-vaatepunktist, seda tehakse kolm sammu:

- **[Loo koolitus eksperiment]** - Azure masin õppe Studio on koostöö visuaalne arendamise keskkond, mis kasutatakse rongi ja test kasutades treeningu andmed sisestatud ennustav mudel.
- **[Muuta see sõnastikupõhine eksperiment]** - kord mudelis on koolitatud koos olemasolevate andmete ja olete valmis kasutama uusi andmeid koguda, valmistada ja täiustada oma katse ennustused.
- **Kasutuselevõtmiseks on web Service** - saab kasutada sõnastikupõhist eksperiment on [Uus] või [klassikaline] Azure web Service. Kasutajate andmete näidis- ning saab oma modelleerimisel.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Luua koolituse eksperiment

Ennustav mudel koolitada Azure masin õppe Studio abil luua koolituse eksperiment kui kaasate erinevate moodulite laadida treeningu andmed, koostamist vastavalt vajadusele, masin õppe algoritme ja tulemuste hindamiseks. Saate kinnitada katse ja proovige teise arvutisse õppe algoritme, et võrrelda ja hinnata.

Protsessi loomise ja haldamise koolitus katseid käsitletakse põhjalikumalt. Lisateabe saamiseks lugege järgmistest artiklitest:

- [Luua lihtne eksperiment Azure masin õppe Studio](machine-learning-create-experiment.md)
- [Sõnastikupõhise lahenduse koos Azure'i masinõppe](machine-learning-walkthrough-develop-predictive-solution.md)
- [Treeningu andmed importida Azure masin õppe Studio](machine-learning-data-science-import-data.md)
- [Hallata eksperiment iteratsioonide Azure masin õppe Studio](machine-learning-manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Koolituse katse teisendada sõnastikupõhine eksperiment

Kui olete koolitatud mudelist, olete valmis muuta oma koolituse katse koguda uusi andmeid sõnastikupõhine eksperiment.

Teisendades sõnastikupõhine eksperiment sa saada koolitatud mudelis punktisüsteem veebiteenistuse tegemas. Teenuse kasutajad saavad saata andmeid näidis- ja mudelist saadab tagasi ennustus tulemused. Kui teisendate sõnastikupõhise eksperiment, pidage meeles kasutusjuhud mudelis kasutada.

Oma koolituse katse teisendada sõnastikupõhine eksperiment nuppu **Käivita** allosas eksperiment lõuend ja **Web Service**siis valige **Sõnastikupõhise veebiteenus**.

![Teisendada hinded eksperiment](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

Selle teisenduse kohta lisateabe saamiseks vt [teisendada masinõpet koolitus eksperiment sõnastikupõhine eksperiment](machine-learning-convert-training-experiment-to-scoring-experiment.md).

Järgnevalt kirjeldatakse juurutamine kui uus veebiteenus sõnastikupõhine eksperiment. Saate juurutada eksperiment kui klassikaline veebiteenus.

## <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>Juurutada sõnastikupõhise eksperiment kui uus veebiteenus

Nüüd sõnastikupõhise katse on ette valmistatud, saab juurutada see Azure web teenust. Veebiteenuse abil kasutajad saavad saata andmed näidis- ja mudel naaseb oma prognoose.

Juurutamiseks sõnastikupõhine eksperiment nuppu **Käivita** allosas eksperiment lõuend. Kui katse on töö lõpetanud, klõpsake **Juurutada veebiteenus** ja valige **juurutamise veebiteenus [Uus]**.  Juurutamise leht masin õppe Web teenuste portaal avaneb.

### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Masin õppe Web Service portal Deploy eksperiment lehekülg

Katse juurutada, Sisestage veebiteenuse nimi.
Valige hinnakujunduse kava. Kui teil on olemasoleva hinnakujunduse kava, saate selle valida, muidu peate looma teenuse hind aasta.

1.  **Hinnad** rippmenüüst, valige olemasoleva energiarežiimi või valige suvand **Valige uus plaan** .
2.  **Plaani nimi**tippige nimi, mis tuvastab teie arvel kava.
3.  Valige üks **Kuu plaani käik**. Kava kolmeks kategooriaks vaikimisi oma vaikimisi piirkonnas ja web service kavad juurutatakse sellesse piirkonda.

Klõpsake **Deploy** ja oma web avab **Quickstart** lehekülje.

Teenuse Quickstart veebileht annab teile juurdepääsu ja juhiseid kõige levinum ülesandeid, siis täidab pärast veebiteenusele. Siit pääsete hõlpsasti testleht ja tarbida lehekülg.

<!-- ![Deploy the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)-->

### <a name="test-your-web-service"></a>Testi oma veebiteenuse

Uue veebiteenuse testimiseks klõpsake all Tavaülesannete **Test veebiteenus** . Lehel Test saate testida oma veebiteenuse nagu on päringu vastuse-teenus (PVR) või teenuse tingimustele (BES).

RRS katse lehel kuvatakse sisendite, väljundite ja globaalse parameetrid on määratletud katse. Veebiteenuse testimiseks saate käsitsi sisestada väärtused sisendite puhul või esitada komaeraldusega väärtuste (CSV) vormindatud fail, mis sisaldab testi väärtused.

Kasutades RRS, lisanud vaaterežiim testimiseks sisestage väärtused sisendeid ja klõpsake nuppu **Test taotluse vastus**. Ennustus tulemused kuvatakse väljund veeru vasakule.

![Veebiteenuse juurutamine](./media/machine-learning-publish-a-machine-learning-web-service/figure-5-test-request-response.png)

Oma BES testimiseks klõpsake nuppu **Pakktöötlus**. Partii katse lehel nuppu Sirvi all sisendit ja valige CSV faili, mis sisaldab vajaliku valimi väärtuste. Teil CSV-faili, kui olete loonud masin õppe Studio kasutades sõnastikupõhine eksperiment, saate alla laadida esitatud andmete sõnastikupõhine eksperiment ja seda kasutada.

Esitatud andmete allalaadimiseks avage masin õppe Studio. Ava oma sõnastikupõhise katsetada ja paremklõps oma katse sisendiks. Menüüst, valige **andmekogumi** ja valige **Laadi alla**.

![Veebiteenuse juurutamine](./media/machine-learning-publish-a-machine-learning-web-service/figure-7-mls-download.png)

Klõpsake nuppu **Test**. Pakktöötluse töö olek kuvatakse paremal all **Katse pakett-töid**.

![Veebiteenuse juurutamine](./media/machine-learning-publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)-->

**Konfiguratsiooni** lehele saate muuta kirjeldus, pealkiri, muudetud ladustamise konto võti ja võimaldada valimi andmeid oma veebiteenuse.

![Konfigureerida web](./media/machine-learning-publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Kui veebiteenus juurutamist saate:

- **Juurdepääs** veebi teenus API kaudu.
- **Halda** Azure'i masinõppe teenuste portaal või klassikaline Azure portaali kaudu.
- **Update** seda kui oma mudeli muutused.

### <a name="access-the-web-service"></a>Veebiteenuse juurdepääs

Kui sul kasutada oma veebiteenuse masin õppe Studio, saab teenuse andmeid saata ja saada vastuseid programmiliselt.

**Tarbida** lehelt leiate kogu teabe oma veebiteenuse juurdepääsuks. Näiteks API võti antakse volitatud juurdepääsu teenus.

Masinõpet veebi teenuse kohta saate lisateavet teemast [Azure'i masinõppe juurutatud veebiteenuse tarbida](machine-learning-consume-web-services.md).

### <a name="manage-your-new-web-service"></a>Hallata oma uus veebiteenus

Saate hallata klassikaline teenused kohapeal õppe veebiteenuste veebiportaali. [Portaali Avaleht](https://services.azureml-test.net/), klõpsake **Veebiteenuste**. Teenuste leheküljel, saate kustutada või kopeerida teenuse. Konkreetse teenuse jälgimiseks teenust ja seejärel käsku **Juhtpaneel**. Pakett-tööd, mis on seotud veebiteenuse jälgimiseks klõpsake **Partii taotleda Logi**.

## <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>Juurutada sõnastikupõhine eksperiment on klassikaline web Service

Nüüd, kui ennustav katse on piisavalt ette valmistatud, saab juurutamist Azure web teenust. Veebiteenuse abil kasutajad saavad saata andmed näidis- ja mudel naaseb oma prognoose.

Juurutamiseks sõnastikupõhine eksperiment nuppu **Käivita** allosas eksperiment lõuend ja klõpsake **Juurutada veebiteenus**. Veebiteenuse seadistatakse ja paigutatakse web service armatuurlaud.

![Veebiteenuse juurutamine](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

Veebiteenuse saate testida kohapeal õppe veebiteenuste portaali või kohapeal õppe Studio.

Veebiteenuse taotluse vastus testimiseks nuppu **Test** web service armatuurlaual. Küsida andmeid sisestada teenuse hüppab dialoogi. Need on oodata punktisüsteem katsetega veerud. Sisestage andmed ja klõpsake nuppu **OK**. Veebiteenuse abil saadud tulemuste kuvatakse armatuurlaua all.

Te saate linki **katsetada** eelvaade testida oma teenust Azure masin õppe veebiteenuste portaali nagu varem uus veebijaotis teenus.

Partii täitmise teenus testimiseks klõpsake **testi** eelvaade link. Partii katse lehel nuppu Sirvi all sisendit ja valige CSV faili, mis sisaldab vajaliku valimi väärtuste. Teil CSV-faili, kui olete loonud masin õppe Studio kasutades sõnastikupõhine eksperiment, saate alla laadida esitatud andmete sõnastikupõhine eksperiment ja seda kasutada.

![Veebiteenuse test](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

**Konfiguratsiooni** lehele, saate muuta teenuse nime, see lühikirjeldus. Nimi ja kirjeldus kuvatakse [klassikaline Azure'i portaal](http://manage.windowsazure.com/) kus hallata oma veebiteenuseid.

Sisestate kirjelduse andmed, andmete ja web teenuse parameetrid sisestada stringi iga veeru all **Sisendi SKEEMI**, **Väljund SKEEMI**ja **Veebiteenuse parameeter**. Need kirjeldused on kasutatud proovi kood materjalidest veebiteenuse.

Saate lubada logimise diagnoosimiseks rikete, et näete, kui teie veebiteenus on kättesaadav. Vaadake lisateavet jaotisest [logimise masinõpet veebiteenuste jaoks](machine-learning-web-services-logging.md).

![Konfigureerida web](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)

Samuti saate konfigureerida Lõpptulemuste veebiteenuse sarnane varem esitatud uus veebijaotis teenust Azure masin õppe veebiteenuste portaalis. Valikud on erinevad, saate lisada või muuta teenuse kirjeldus, Luba logimine ja luba Näidisandmete testimiseks.

### <a name="access-the-web-service"></a>Veebiteenuse juurdepääs

Kui sul kasutada oma veebiteenuse masin õppe Studio, saab teenuse andmeid saata ja saada vastuseid programmiliselt.

Armatuurlaua leiate kogu teabe, mis on oma veebiteenuse juurdepääsuks. Näiteks API võti antakse volitatud juurdepääsu teenuse ja API abilehed on mis aitavad teil alustada kirjalikult oma kood.

Masinõpet veebi teenuse kohta saate lisateavet teemast [Azure'i masinõppe juurutatud veebiteenuse tarbida](machine-learning-consume-web-services.md).

### <a name="manage-the-web-service"></a>Hallata veebiteenus

On mitmeid tegevusi saate jälgida veebiteenusele. Saate värskendada ja kustutada. Võite sisestada ka Täiendavad tulemusnäitajad Classic veebiteenusega loodud juurutamist vaikelõpp lisaks.

Vaadake lisateavet jaotisest [Halda Azure'i masinõppe tööruumi](machine-learning-manage-workspace.md) ja [Halda Azure masin õppe veebiteenuste portaali veebiteenusele](machine-learning-manage-new-webservice.md).

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>Update veebiteenus

Saate muuta oma veebiteenuse nagu mudeli uuendamine lisakoolitust andmetega ja juurutada see uuesti, ülekirjutamist web teenust.

Veebiteenuse värskendamiseks avage algne sõnastikupõhise katse juurutada veebiteenuse ning muuta redigeeritava eksemplari, klõpsates nuppu **SAVE AS**. Tehke soovitud muudatused ja klõpsake **Juurutada veebiteenus**.

Sest see katse enne juurutamist küsitakse, kas soovite üle kirjutada (klassikaline veebiteenus) või värskendada olemasolevat teenust (uus veebiteenus). Klõpsake **Jah** või **Update** peatub olemasoleva veebiteenus ja juurutab uue sõnastikupõhise eksperiment juurutatakse selle asemel.

> [AZURE.NOTE] Kui algne veebiteenuses tehtud konfiguratsiooni muudatusi, näiteks uus kuvatav nimi või kirjeldus, peate uuesti sisestama need väärtused.

Üks võimalus oma veebiteenuse uuendamiseks on koolitada mudel programmiliselt. Lisateabe saamiseks vt [ümber masinõpet mudelid programmiliselt](machine-learning-retrain-models-programmatically.md).


<!-- internal links -->
[Luua koolituse eksperiment]: #create-a-training-experiment
[Muuta see ennustav eksperiment]: #convert-the-training-experiment-to-a-predictive-experiment
[Uus]: #deploy-the-predictive-experiment-as-a-new-Web-service
[klassikaline]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
