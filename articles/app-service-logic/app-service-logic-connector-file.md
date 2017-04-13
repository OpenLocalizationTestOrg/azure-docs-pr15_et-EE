<properties
    pageTitle="Loogika rakendustes kasutamisega faili | Microsoft Azure'i rakendust Service"
    description="Kuidas luua ja faili konnektor või API rakenduse konfigureerimine ja kasutamine loogika rakenduse teenuses Azure rakenduse"
    authors="rajeshramabathiran"
    manager="erikre"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="rajram"/>

# <a name="get-started-with-the-file-connector-and-add-it-to-your-logic-app"></a>Alustamine faili konnektor ja lisada selle oma loogika rakenduse
>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2014-12-01-eelvaade skeemi versioon.

Ühenduse failisüsteemi üles, alla laadida ja muu host arvutisse teie failidele juurde. Loogika rakenduste käivitab põhjal erinevate andmeallikate ja pakkumise konnektorid saada ja andmete töötlemiseks. Saate lisada töövoog ja protsessi äriteabe osana töövoo loogika rakenduses fail konnektor. 

Faili konnektor kasutab hübriid ühenduvuse host failisüsteemi ühenduse hübriid ülemus.

## <a name="creating-a-file-connector-for-your-logic-app"></a>Faili konnektor oma loogika rakenduse loomine ##
Faili konnektor kasutamiseks peate esmalt looma faili konnektor API rakenduse eksemplari. Seda saab teha järgmiselt:

1.  Avage Azure'i turuplatsi, kasutades + uus suvand vasakul küljel Azure portaali.
2.  Otsige "faili konnektor".
3.  Valige otsingu tulemuste seast **Faili konnektor (eelvaade)** .
4.  Klõpsake nuppu **Loo**
5.  Konfigureerige faili konnektor järgmiselt:  
![][1]

    - **Nimi** – anna nimi oma faili konnektor
    - **Paketi sätted**
        - **Juurkausta** - juurkausta tee Määrake host teie arvutis. Nt. D:\FileConnectorTest
        - **Teenuse siini ühendusstringi** - teenuse siini ühendusstringi esitada. Kontrollige, kas teenuse siini nimeruumi on tüüpi Standard ja pole lihtsa teenuse siini releed ärakasutamiseks.  Teenuse siini Relay kasutatakse ühenduse hübriid ühenduse haldur.
    - **Rakenduse teenusleping** - valida või luua rakenduse teenusleping
    - **Esimese taseme hinnad** – valige hinnakirjad taseme konnektor
    - **Ressursirühm** – valige või kus konnektor peaks elavad ressursi rühma loomine
    - **Tellimuse** – valige tellimus, mida soovite luua selle konnektor
    - **Asukoht** – valige geograafiline asukoht, kuhu soovite kasutusele võtta konnektor

4. Klõpsake nuppu Loo. Luuakse uus fail konnektor

## <a name="configure-hybrid-connection-manager"></a>Hübriidjuurutuse haldur konfigureerimine ##
Kui API rakenduse eksemplari on loodud, liikuge selle armatuurlaua.  Seda saab teha, klõpsates nuppu Browse > API rakendused > valige oma faili konnektor API rakenduse.  Siin ühenduse hübriid ülemus peab olema konfigureeritud.
Konfigureerimise ja ühenduse hübriid ülemus tõrkeotsingu kohta lisateabe saamiseks lugege teemat [ühenduse hübriid halduris].

## <a name="using-the-file-connector-in-your-logic-app"></a>Loogika rakenduse kasutamisega fail ##
Kui teie API rakendus on loodud, nüüd abil saate faili konnektor toimingu oma loogika rakenduse. Selle tegemiseks peate:

1.  Uue loogika rakenduse loomine ja valige faili konnektor on sama ressursirühma. Järgige juhiseid [uue loogika rakenduse]loomine.

2.  Avage "Päästikute ja toimingute" loodud loogika rakendusest loogika rakenduste Designer avamiseks ja oma meilivoo konfigureerimine.

3.  Faili konnektor kuvatakse paremal pool galeriis jaotist "API rakendused selle ressursi jaotises".

4.  Te saate kukutage fail konnektor API rakenduse redaktori klõpsates "faili konnektor". faili konnektor pakub ühe päästik ja 4 toimingud:  
![][5]

6.  Igaühel neist seab teatud atribuudid. Alloleval pildil on loetletud päästiku atribuutide ja saada faili toiming:  
![][6]

7. Kui need on konfigureeritud, saab teie meilivoo päästik ja toiming. Samuti saate muude toimingute ka konfigureeritud.

> [AZURE.NOTE] Pärast seda, kui see on edukalt lugeda kaustast, kustutatakse faili käivitada faili.

## <a name="file-connector-rest-apis"></a>Faili konnektor REST API-d ##
Konnektor väljaspool loogika rakenduse kasutamiseks saate kasutada REST API-de esitatud konnektor. Te saate selle API määratlused abil Sirvi -> Api rakenduse vaade -> faili konnektor. Nüüd klõpsake API määratlus lens vaadata kõiki API, mis on esitatud selle konnektori jaotises Kokkuvõte:  
![][7]

Funktsiooni API-de üksikasjad leiate [faili konnektor API määratlus].

## <a name="do-more-with-your-connector"></a>Lisavõimalused oma konnektor
Nüüd, kui konnektor on loodud, saate selle lisada töövoos loogika rakenduse kasutamine. Vt [mis on loogika rakendused?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Kui soovite alustada Azure'i loogika rakendused enne Azure'i konto kasutajaks, minge [proovige loogika rakenduse](https://tryappservice.azure.com/?appservice=logic), kus saate kohe luua lühiajaline starter loogika rakendus App teenuses. Nõutav; krediitkaardid kohustusi.

Vaadata ärplema REST API viide [konnektorid ja API rakenduste viide](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Saate vaadata ka jõudluse statistika ja juhtelemendi turvalisus konnektor. Vaadata, [hallata ja jälgida oma sisseehitatud API rakendused ja konnektor](app-service-logic-monitor-your-connectors.md).

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-file/img1.PNG
[5]: ./media/app-service-logic-connector-file/img5.PNG
[6]: ./media/app-service-logic-connector-file/img6.PNG
[7]: ./media/app-service-logic-connector-file/img7.PNG

<!-- Links -->
[Uue loogika rakenduse loomine]: app-service-logic-create-a-logic-app.md
[Faili konnektor API määratlus]: https://msdn.microsoft.com/library/dn936296.aspx
[Ühenduse hübriid halduri abil]: app-service-logic-hybrid-connection-manager.md
