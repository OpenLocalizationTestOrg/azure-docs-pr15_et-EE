<properties
    pageTitle="Juurutamise veebiteenuse mitme piirkonna | Microsoft Azure'i"
    description="Juhised (Kopeeri) uus veebiteenus muude piirkondade juurutamiseks."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="how-to-deploy-a-web-service-to-multiple-regions"></a>Juurutamise veebiteenuse mitme piirkonna

Uue Azure veebiteenused võimaldavad hõlpsalt juurutamine veebiteenuse mitme piirkonna vajamata mitu tellimust või tööruumid. 

Hinnad on teatud, seega peate määratlema arvelduse kavandamine iga piirkonna kohta, kus juurutamist veebiteenuse piirkond.

## <a name="to-create-a-plan-in-another-region"></a>Mõne muu piirkonna plaani loomiseks

1. Logige sisse teenusekomplekti [Microsoft Azure'i masina Õppekeskuse veebiteenused](https://services.azureml.net/).
2. Klõpsake menüü suvandit **lepingud** .
3. Lepingute view lehe kohale, klõpsake nuppu **Uus**.
4. Valige rippmenüüst **tellimuse** tellimus, hakkavad asuma uue lepingu raames.
5. Valige rippmenüüst **piirkond** uue lepingu raames piirkonnas. Piirkonna jaoks plaanimine suvandid kuvatakse lehe jaotises **Suvandid kavandamine** .
6. Valige rippmenüüst **Ressursirühm** ressursirühma lepingu raames. Ressursi rühmade kohta lisateabe saamiseks lugege artiklit [portaali kaudu hallata Azure ressursse](../azure-portal/resource-group-portal.md).
7. **Lepingu nimi** tippige selle lepingu nimi.
8. Klõpsake jaotises **Lepingu suvandid**nuppu arvelduse tasemele uue lepingu raames.
9. Klõpsake nuppu **Loo**.


## <a name="deploying-the-web-service-to-another-region"></a>Mõne muu alale veebiteenuse juurutamine

1. Klõpsake suvandit **Veebiteenuste** menüü.
2. Valige veebiteenuse võtate kasutusele uue alale.
3. Klõpsake nuppu **Kopeeri**.
4. **Web teenuse nimi**, tippige uus nimi veebiteenuse.
5. **Web teenuse kirjeldus**, tippige veebiteenuse kirjeldus.
6. Valige rippmenüüst **tellimuse** uue veebiteenuse hakkavad asuma tellimus.
7. Valige rippmenüüst **Ressursirühm** veebiteenuse ressursirühma. Ressursi rühmade kohta lisateabe saamiseks lugege artiklit [portaali kaudu hallata Azure ressursse](../azure-portal/resource-group-portal.md).
8. Valige rippmenüüst **piirkond** , kus juurutada veebiteenuse piirkond.
9. Valige rippmenüüst **salvestusruumi konto** salvestusruumi konto, kus soovite talletada veebiteenuse.
10. Valige rippmenüüst **Hind lepingu** 8 juhises valitud piirkonna leping.
11. Klõpsake nuppu **Kopeeri**.

