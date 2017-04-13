<properties
   pageTitle="Loogika rakenduste näited ja stsenaariumid | Microsoft Azure'i"
   description="Levinud loogika rakenduste näiteid ja saate teada, kuidas rakendada tavastsenaariumid"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-examples-and-common-scenarios"></a>Loogika rakenduste näited ja Levinumad stsenaariumid

Selle dokumendi üksikasjad Levinumad stsenaariumid ja näited, mis aitavad teil mõista mõned viisid, kuidas te saate loogika rakenduste abil äriprotsesside automatiseerimine. 

## <a name="custom-triggers-and-actions"></a>Kohandatud päästikute ja toimingud

On mitu võimalust, võite käivitada loogika rakenduse teisest rakendusest. Siin on mõned levinud näited:

- [Luua kohandatud päästik või toimingu](app-service-logic-create-api-app.md)
- [Pikaajalisi toimingud](app-service-logic-create-api-app.md)
- [HTTP taotluse päästik (POSTITA)](app-service-logic-http-endpoint.md)
- [Webhook päästikute ja toimingud](app-service-logic-create-api-app.md)
- [Küsitlused päästikute](app-service-logic-create-api-app.md)

### <a name="scenarios"></a>Stsenaariumid

- [Taotluse sünkroonse vastus](app-service-logic-http-endpoint.md)
- [Vastus koos SMS taotlemine](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="error-handling-and-logging"></a>Vigade käsitlemise ja logimine

- [Erandi ja tõrge töötlemine](app-service-logic-exception-handling.md)
- [Diagnostika- ja Azure teatiste konfigureerimine](app-service-logic-monitor-your-logic-apps.md)

### <a name="scenarios"></a>Stsenaariumid

- [: Kasuta tõrke ja erandi töötlemine](app-service-logic-scenario-error-and-exception-handling.md)

## <a name="deploying-and-managing"></a>Juurutamine ja haldamine

- [Luua mõne automatiseeritud juurutamine](app-service-logic-create-deploy-template.md)
- [Luua ja juurutada loogika rakenduste Visual Studio](app-service-logic-deploy-from-vs.md)
- [Loogika rakenduste jälgimine](app-service-logic-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations"></a>Sisutüüpide ja dokumenditeisenduste teisendused

Loogika rakenduste [töövoo määratlus keel](http://aka.ms/logicappsdocs) sisaldab palju funktsioonide abil saate teisendada ja töötada eri tüüpi sisu.  Lisaks mootori teeb kõik selle saate säilitada sisutüübid – nii andmevahetuse töövoo kaudu.

- Näiteks rakenduse/json, rakendus-xml ja [Lihttekst/käitlemine sisutüübid](app-service-logic-content-type.md)
- [Loome töövoo määratlused](app-service-logic-author-definitions.md)
- [Töövoo määratlus keel viide](http://aka.ms/logicappsdocs)

## <a name="batches-and-looping"></a>Töölehtede ja hoidke

- [SplitOn](app-service-logic-loops-and-scopes.md)
- [ForEach](app-service-logic-loops-and-scopes.md)
- [Kuni](app-service-logic-loops-and-scopes.md)

## <a name="integrating-with-azure-functions"></a>Integreerimine Azure funktsioonid

- [Azure'i funktsioonide integreerimine](app-service-logic-azure-functions.md)

### <a name="scenarios"></a>Stsenaariumid

- [Azure'i funktsiooni teenuse siini käivitamiseks](app-service-logic-scenario-function-sb-trigger.md)

## <a name="http-rest-and-soap"></a>SOAP HTTP ja ülejäänud

 - [Helistamiseks SOAP](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)


Meil on säilitada lisada näited ja stsenaariumid, et see dokument. Andke meile teada, milliseid näiteid või stsenaariumid soovite näha siin jaotisse kommentaarid abil.