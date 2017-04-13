<properties
    pageTitle="Õppekeskuse mudeli masina ümber | Microsoft Azure'i"
    description="Saate teada, kuidas ümber mudeli ja värskendada veebiteenuse Azure seadme õ äsja koolitatud mudeli kasutamine."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-machine-learning-model"></a>Õppekeskuse mudeli masina ümber

Osana tulemustabeli kasutuselevõtmise üle ka arvuti õ andmemudelites Azure seadme Õppekeskuse protsess on mudelisse väljaõppe ja salvestada. Seejärel saate selle luua Predikatiivses veebiteenusest. Veebiteenuse saate tarbitud seejärel veebisaite, armatuurlaudu ja mobiilirakendused. 

Seadme õ abil loote mudelite pole tavaliselt staatiline. Uute andmete vabanemisest teavitamise või kui tarbija API on oma andmete mudelisse peab koolitada ümber. 

Ümberõpe võib ilmneda sagedamini. Programmiline ümberõpet API funktsiooni abil saate programmiliselt ümber mudeli ümberõpet API-de kasutamine ja värskendada veebiteenuse äsja koolitatud mudeliga. 

Selles dokumendis kirjeldatakse ümberõpe ja näidatakse, kuidas kasutada ümberõpet API-d.

## <a name="why-retrain-defining-the-problem"></a>Miks ümber: probleemi määratlemine  

Õppekeskuse koolitus protsessi masina osana õpetatakse mudeli andmete kasutamine. Seadme õ abil loote mudelite pole tavaliselt staatiline. Uute andmete vabanemisest teavitamise või kui tarbija API on oma andmete mudelisse peab koolitada ümber.

Need stsenaariumid, pakub programmiline API mugavam luba teil või tarbija oma API-de loomiseks klient, mida saate, ühekordse või tavalise alusel, ümber abil oma andmeid mudel. Need ümberõpet tulemusi hinnata ja värskendada teenuse Veebiteenuste äsja koolitatud mudeli kasutamine.

>[AZURE.NOTE] Kui teil on mõne olemasoleva koolitus katse ja uue veebiteenuse, soovite kontrollida ümber olemasoleva sõnastikupõhise Web teenuse asemel jälgimise kiirtutvustus, järgmises jaotises kirjeldatud.

## <a name="end-to-end-workflow"></a>Töövoo lõpuni 

Protsess hõlmab järgmisi komponente: A koolitus katse ja sõnastikupõhise katse avaldatud veebiteenusest. Ümberõpet koolitatud mudeli lubamiseks peab olema avaldatud koolitus katse nimega koolitatud mudeli väljundit veebiteenusest. See võimaldab API juurdepääsu mudeli ümberõppeks. 

Järgmised juhised kehtivad nii uus ja klassikaline veebiteenuseid:

Algne sõnastikupõhise veebiteenuse loomiseks tehke järgmist.

* Koolitus katse loomine
* Sõnastikupõhise web katse loomine
* Sõnastikupõhise veebiteenuse juurutamine

Veebiteenuse ümber:

* Koolitus katse lubamiseks ümberõppe värskendamine
* Ümberõpe veebiteenuse juurutamine
* Paketi täitmise teenuse kood koolitada mudeli kasutamine

Samm-sammult juhendi eelmist toimingut, leiate teemast [ümber masina õ mudelid programmiliselt](machine-learning-retrain-models-programmatically.md).

Kui olete juurutanud klassikaline veebiteenuse:

* Saate luua uue lõpp-punkti sõnastikupõhise teenuses
* PAIK URL-i ja koodiga.
* Osutage uue lõpp-punkti retrained mudeli paik URL-i abil 

Samm-sammult juhendi eelmist toimingut, leiate teemast [klassikaline veebiteenuse ümber](machine-learning-retrain-a-classic-web-service.md).

Kui tekib probleeme ümberõpet klassikaline veebiteenuse, lugege teemat [tõrkeotsing teenuse Azure seadme õ klassikaline Web ümberõpet](machine-learning-troubleshooting-retraining-models.md).

Kui olete juurutanud uue veebiteenuse:

* Ressursihaldur Azure'i kontosse sisselogimine
* Saada Web teenuste määratlemine
* Eksportida JSON Web teenuste määratlemine
* Värskendada viide on `ilearner` klõpsake soovitud JSON bloobimälu
* Funktsiooni JSON importimine Web teenuste määratlemine
* Värskendage veebiteenuse uus Web teenuse määratlemine

Samm-sammult juhendi eelmist toimingut, leiate teemast [arvuti õ halduse PowerShelli cmdlet-käskude abil uus veebiteenuse ümber](machine-learning-retrain-new-web-service-using-powershell.md).

Klassikaline veebiteenuse ümberõpe häälestamise hõlmab järgmist:

![Ümberõpe protsessi ülevaade][1]

Uue veebiteenuse ümberõpe häälestamise hõlmab järgmist:

![Ümberõpe protsessi ülevaade][7]

## <a name="other-resources"></a>Muud ressursid

- [Azure'i andmed Factory ümberõpe ja värskendamine Azure seadme õ mudelite](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
- [Luua mitme seadme õppe mudelid ja web teenuse lõpp-punktid ühe katse PowerShelli abil](machine-learning-create-models-and-endpoints-with-powershell.md)
- Selle [Koostamisel ümberõpet mudelite abil API](https://www.youtube.com/watch?v=wwjglA8xllg) video näitab, kuidas koolitada masina õppe mudelid loodud Azure seadme õ ümberõpet API-d ja PowerShelli abil.

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

