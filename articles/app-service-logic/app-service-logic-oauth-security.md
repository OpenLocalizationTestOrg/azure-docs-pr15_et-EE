<properties
    pageTitle="OAUTHI turvalisuse SaaS konnektorid ja API rakendused | Azure'i"
    description="Lugege OAUTHI turvalisus konnektorid ja API rakenduste Azure rakenduse teenuses; microservices arhitektuur; Saas"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="dwrede"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="mandia"/>


# <a name="learn-about-oauth-security-in-saas-connectors"></a>OAUTHI turvalisusest sisse SaaS konnektorid

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2014-12-01-eelvaade skeemi versioon.

Paljud tarkvara kui teenus (SaaS) konnektorid nagu Facebooki, Twitteri, Dropboxi ja jne nõuavad kasutajad autentida OAUTHI protokolli abil.  Nende SaaS konnektorid loogika rakenduste kasutamisel pakume lihtsustatud kasutusvõimalused, kui klõpsate "Autoriseerin" Designeris loogika rakendused. Kui te **Autoriseerin**, palutakse logida sisse (kui see juba ei) ja sisestage nõusolekut ühenduse SaaS teenuse enda nimel. Kui teilt nõusolekut ja lubada, pääseb loogika rakenduste need SaaS teenused.

## <a name="create-your-own-saas-app"></a>Oma SaaS rakenduse loomine
See lihtsustatud kogemus on võimalik, kuna me varem loodud ja registreeritud meie rakenduse need SaaS teenused.  Teatud juhtudel võite registreerida ja kasutada oma rakendus.  See on vajalik, näiteks, kui soovite kasutada oma kohandatud rakendused neid SaaS konnektorid. Selles näites kasutatakse Dropboxi konnektor, kuid protsess on sama kõik ühendused, mis sõltuvad OAUTHI.

Isegi kontekstis loogika rakendused, saate oma rakenduse asemel pakume vaikimisi rakenduse abil. Kui nupp "Autoriseerin" ei saa ühendust luua, proovige oma rakenduse loomine. Järgmised loendid järgmist Twitteri konnektor:

1. Avage oma Twitteri konnektor portaalis Azure eelvaade. Valige **Sirvi** > **API rakendused**. Valige oma Twitteri konnektor.  
    ![][1]

2. Valige **sätted** > **autentimist**:  
    ![][2]

3. Kopeerige **Ümber suunata URI** väärtus.  
    ![][3]

4. Minge [Twitteri](http://apps.twitter.com) ja **luua uue rakenduse**. Atribuudi **Tagasihelistamise URL-i** Kleepige kopeeritud oma Twitteri konnektor **Ümber suunata URI** väärtus: ![][4]  
5. Twitteri rakenduse loomisel valige **võti ja juurdepääs märgid**. Kopeerige need väärtused.
6. Kleepige oma Twitteri konnektor autentimissätted **Kliendi ID** ja **Kliendi salajane** atribuutide need väärtused:   
    ![][5]  
7. Konnektor sätete salvestamine.  

Nüüd peaks olema saama kasutada oma konnektor loogika rakenduste kaudu. Kui kasutate selle konnektori loogika rakenduste kaudu, kasutab rakenduse vaikerakenduse asemel.  

> [AZURE.NOTE] Kui rakendus on varem lubatud, tuleb reauthorize rakendus.


<!--Image references-->
[1]: ./media/app-service-logic-oauth-security/TwitterConnector.png
[2]: ./media/app-service-logic-oauth-security/Authentication.png
[3]: ./media/app-service-logic-oauth-security/RedirectURI.png
[4]: ./media/app-service-logic-oauth-security/TwitterApp.png
[5]: ./media/app-service-logic-oauth-security/TwitterKeys.png
