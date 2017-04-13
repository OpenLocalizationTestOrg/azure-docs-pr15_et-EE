<properties
    pageTitle="Azure'i Mobile kaasamine iOS SDK väljalaskemärkmed | Microsoft Azure'i"
    description="Uusimate värskenduste ja toimingute iOS-i SDK Azure Mobile kaasamine"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="piyushjo" />

#<a name="azure-mobile-engagement-ios-sdk-release-notes"></a>Azure'i SDK Mobile kaasamine iOS-i versioon märkmed

##<a name="400-09122016"></a>4.0.0 (09/12/2016)

-   Fikseeritud teatis ei actioned iOS 10-seadmetes.
-   Taunimine XCode 7.

##<a name="324-06302016"></a>3.2.4 (06/30/2016)

-   Fikseeritud koondamine tehnilise logid ja muude logide vahel.

##<a name="323-06072016"></a>3.2.3 (06/07/2016)

-   Fikseeritud viga, kus on teatatud kohaletoimetamise tagasiside kui rakendus töötab taustal.
-   Optimeeritud tehnilise logide saatmine.

##<a name="322-04072016"></a>3.2.2 (04/07/2016)

-   Fikseeritud vea HTTP taotluse tühistamine, mis viib mõnikord krahh.

##<a name="321-12112015"></a>3.2.1 (11/12/2015)

-   Fikseeritud viivituse uue rakenduse eksemplari käivitamisel teatega sügav lingid

##<a name="320-10082015"></a>3.2.0 (10/08/2015)

-   Lubatud Bitcode SDK **Xcode 7**koos töötada.
-   Fikseeritud vead seotud-rakenduste teatised.
-   Tehtud-rakenduse teatised usaldusväärne aku ja teiste selliste stsenaariumide korral.
-   3 tootja teegi genereeritud eest konsooli logide eemaldada.

##<a name="310-08262015"></a>3.1.0 (08/26/2015)

-   Kolmanda osapoole teegiga määrata iOS 9 ühilduvus viga. See põhjustas jookseb ajal saatmise küsitlused tulemused, teenuserakenduse teabe või lisaandmed.

##<a name="300-06192015"></a>3.0.0 (06/19/2015)

-   Mobile kaasamine kasutab vaikne tõuketeatised.
-   IOS-i tugi lähevad 4.X. Alates selle versiooni juurutamise target rakenduse peab olema vähemalt iOS 6.

##<a name="220-05212015"></a>2.2.0 (05/21/2015)

-   Mobile kaasamine seadme id seadmete jaoks < iOS 6 on nüüd põhjal loodud installimise ajal GUID.

##<a name="210-04242015"></a>2.1.0 (04/24/2015)

-   Lisatud kiire ühilduvus.
-   Teate klõpsamisel toimingu URL on nüüd täita paremale pärast seda, kui rakendus on avatud.
-   Lisatud puuduvad päise faili SDK pakett.
-   Fikseeritud küsimus, kui keelamist Mobile kaasamine krahh looma.

##<a name="200-02172015"></a>2.0.0 (02/17/2015)

-   Algse versiooni Azure mobiilsideseadmete kaasamine
-   appId/sdkKey konfiguratsiooni asendatakse ühenduse stringi konfiguratsiooni.
-   Sõnumite saatmiseks ja vastuvõtmiseks suvalise XMPP suvalise XMPP üksuste API eemaldada.
-   Eemaldatud API seadmete vahel sõnumite saatmiseks ja vastuvõtmiseks.
-   Turvalisuse täiustamine.
-   SmartAd jälitus eemaldada.
