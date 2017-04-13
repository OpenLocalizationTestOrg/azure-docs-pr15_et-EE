<properties
   pageTitle="Mitme-Geo aitab dokumentatsiooni | Microsoft Azure'i"
   description="Saate teada, kuidas tööruumi loomiseks ja avaldamiseks veebiteenuse Azure piirkonnas erinevate kaudu soovitud Lõuna keskse Ameerika Ühendriigid (SCUS) Azure piirkond."
   services="machine-learning"
   documentationCenter=""
   authors="tedway"
   manager="jhubbard"
   editor="rmca14"
   tags=""/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="tedway; neerajkh"/>

# <a name="multi-geo-help-documentation"></a>Mitme-Geo aitab dokumentatsioon

Selles artiklis kirjeldatakse, kuidas saate tööruumi loomine ja avaldamine veebiteenuse Azure mujal.

## <a name="create-a-workspace"></a>Tööruumi loomine

1. Azure'i klassikaline portaali sisse logima.

2.  Valige **+ Uus** > **DATA SERVICES** > **masina õ** > **kiire loomine**.  Valige jaotises **asukoha** teise piirkond, nt **Kagu-Aasia**.
![Mitme-Geo Spikker pilt 1][1]
3. Valige tööruumi ja seejärel klõpsake nuppu **Logi sisse ML Studio**.
![Mitme-Geo Spikker pilt 2][2]

4. Nüüd saate tööruumi teise ala, mida saate kasutada nii nagu mis tahes muud tööruumi. Tööruumide vaheldumisi aktiveerida, vaadake Kuva paremas ülanurgas. Klõpsake ripploendit, valige piirkonnale ja seejärel valige tööruum. Kõik on kohalik tööruumi alale; näiteks kõik teie loodud tööruumis veebiteenuste olema sama piirkonna tööruumi asub.
![Mitme-Geo Spikker pilt 3][3]

## <a name="open-an-experiment-from-gallery"></a>Avage katse galeriist

Kui avate katse galeriist, võite valida ka soovite kopeerida, et katse regioonis.

![Mitme-Geo Spikker pilt 4][4a]

## <a name="web-service-management"></a>Web Teenusehaldus

Programmiliselt haldamine veebiteenuste, näiteks ümberõppe, kasutage piirkonnakohase aadress.
- https://asiasoutheast.Management.azureml.net
- https://europewest.Management.azureml.net

### <a name="things-to-note"></a>Asjad, mida

1.  Saate kopeerida ainult katsete vahel tööruumid, mis kuuluvad piirkonna sellisel viisil. Kui teil on vaja kopeerida katse üle erinevate piirkondade tööruumid, saate kasutada [PowerShell](http://aka.ms/amlps) abil [*Kopeeri-AmlExperiment*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) tegemise mis. Mõne muu lahendus on avaldada katse galeriisse kontaktiloendist režiimis, avage see tööruumis muud piirkonnast.
2.  Piirkonna Vaateselektori kuvatakse ainult ühe piirkonna tööruumide korraga. Tulevikus, siis saab tööruumid, mida teil on juurdepääs kõigis piirkondades samal ajal täieliku loendi kuvamiseks.  
3.  Tasuta tööruumi või Külastajate juurdepääsu (anonüümne) tööruumi loonud ja Lõuna keskse U.S. majutatud Tulevikus, on võimalik tasuta/Külastajate juurdepääsu tööruumide ala, mille valimisel.  
4.  Ka valmimiseni veebiteenuste juurutatud tööruumist Kagu-Aasia, Kagu-Aasia. Tulevikus, on võimalik on paindlikkust loomise katsete ühe piirkonna ja juurutamine loodud web teenuse lõpp-punktid eri regioonide sisse.  

## <a name="more-information"></a>Lisateave

Küsimuse esitamine [Azure seadme õppe Foorum](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
