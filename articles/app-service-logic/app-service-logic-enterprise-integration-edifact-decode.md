<properties 
    pageTitle="Lisateavet ettevõtte integreerimine Pack dekodeerida EDIFACT sõnum konnektor | Microsoft Azure'i rakendust Service | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada partnerite Enterprise integreerimine keelepaketi ja loogika rakendustega" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="padmavc" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="padmavc"/>

# <a name="get-started-with-decode-edifact-message"></a>Alustamine dekodeerida EDIFACT sõnum

Kinnitatakse EDI ja partneri kohased atribuudid, loob XML-dokumendi iga tehingu kogumi ja loob töödeldud tehingu kinnitus.

## <a name="create-the-connection"></a>Ühenduse loomine

### <a name="prerequisites"></a>Eeltingimused

* Azure'i konto; [tasuta konto](https://azure.microsoft.com/free) loomisel

* Konto integreerimine on vajalik dekodeerida EDIFACT sõnum Connectori kasutamine. Lisateavet leiate teemast kohta, kuidas luua [Konto](./app-service-logic-enterprise-integration-create-integration-account.md), [partnerite](./app-service-logic-enterprise-integration-partners.md) ja [EDIFACT lepingu](./app-service-logic-enterprise-integration-edifact.md) üksikasjad

### <a name="connect-to-decode-edifact-message-using-the-following-steps"></a>Ühenduse loomine dekodeerida EDIFACT sõnum, tehes järgmist:

1. [Loogika rakenduse loomine](./app-service-logic-create-a-logic-app.md) pakub näide.

2. See konnektor ei saa mis tahes päästikute. Käivitage rakendus loogika, nt taotluse päästik muude päästikute kasutamine.  Loogika rakenduse designer, lisage käivitamiseks ja toimingu lisada.  Valige Kuva Microsoft hallatava API-d rippmenüüst loend ja sisestage otsinguväljale "EDIFACT".  Valige dekodeerida EDIFACT sõnum

    ![Otsingu EDIFACT](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage1.png)
    
3. Kui te pole varem loonud kõik ühendused konto, küsitakse ühenduse üksikasjad

    ![konto loomine](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage2.png)  

4. Sisestage konto üksikasjad.  Tärniga atribuudid on nõutav

  	| Atribuut | Üksikasjad |
  	| -------- | ------- |
  	| Ühenduse nimi * | Sisestage mis tahes ühenduse nimi |
  	| Konto * | Sisestage konto nimi. Veenduge, et teie konto ja loogika rakendus, Azure paigal |

    Kui olete lõpetanud, järgmine sarnanevad oma ühenduse üksikasjad

    ![konto loomine](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage3.png)  

5. Valige **loomine**

6. Pange tähele, et ühendus on loodud

    ![integreerimine konto ühenduse üksikasjad](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

7. EDIFACT lamefaili sõnumi dekodeerida valimine

    ![Sisestage kohustuslikud](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

## <a name="edifact-decode-does-following"></a>Kas EDIFACT dekodeerida jälgimine

* Lepingu lahendada sobitamine saatja kvalifikatsioon ja identifikaator ja vastuvõtja kvalifikatsioon ja identifikaator
* Jagab ühes sõnumis mitme tehnosiirdeid eraldi.
* Kinnitatakse ümbrik börsipäev partneri lepingu vastu
* Disassembles andmevahetuse.
* Kinnitatakse EDI ja partneri kohased atribuudid sisaldab
    * Ümbriku andmevahetuse struktuuri valideerimine.
    * Skeemi valideerimine ümbriku juhtelemendi skeemi suhtes.
    * Skeemi valideerimine tehingu seadistatud andmete elementide sõnumi skeemi suhtes.
    * Tehingu seadistatud andmed läbi EDI valideerimine
* Kinnitab, et andmevahetuse, rühma ja tehingu määramine juhtelemendi arvud pole duplikaadid (kui see on konfigureeritud) 
    * Kontrollib andmevahetuse juhtelemendi number varem saadud tehnosiirdeid suhtes. 
    * Kontrollib jaotises juhtelemendi number andmevahetuse muude rühma juhtelement arvude suhtes. 
    * Kontrollib, kas tehingu juhtelemendi numbri saate määrata selle rühma teiste tehingu määramine juhtelemendi arvude suhtes.
* Loob XML-dokumendi iga tehingu kogumi.
* Teisendab kogu andmevahetuse XML-i 
    * Kui tehingu komplekti - tükeldatud andmevahetuse peatada tehingu komplekti tõrge: sõelub iga seadmine teabevahetus eraldi XML-i dokumenti tehingu. Kui üks või mitu tehingu seab andmevahetuse valideerimine nurjub, siis EDIFACT dekodeerida peatab ainult need tehingu komplekti. 
    * Kui tehingu komplekti - tükeldatud andmevahetuse peatada interchange tõrge: sõelub iga seadmine teabevahetus eraldi XML-i dokumenti tehingu.  Kui üks või mitu tehingu seab andmevahetuse valideerimine nurjub, siis EDIFACT dekodeerida peatab kogu andmevahetuse.
    * Säilitada Interchange - peatada tehingu komplekti tõrge: loob XML-dokumendi kogu batched andmevahetuse. EDIFACT dekodeerida peatab ainult need tehingu komplekti, mille valideerimine jätkates töötlemine kõik muud tehingu komplekti nurjub
    * Säilitada Interchange - peatada interchange tõrge: loob XML-dokumendi kogu batched andmevahetuse. Kui üks või mitu tehingu seab andmevahetuse valideerimine nurjub, siis EDIFACT dekodeerida peatab kogu andmevahetuse, 
* Loob tehniliste (CTRL) ja/või otstarbekas kinnitus (kui see on konfigureeritud).
    * Tehniline kinnitus või CONTRL ACK aruannete süntaktilistest sisse täieliku vastuvõetud asenduse tulemused.
    * Otstarbekas kinnitus tunnustab aktsepteerimiseks või hülgamiseks vastuvõetud andmevahetuse või rühma

## <a name="next-steps"></a>Järgmised sammud

[Lisateavet leiate teemast hoolduspaketi Enterprise integreerimise kohta] (./app-service-logic-enterprise-integration-overview.md "Lisateavet ettevõtte integreerimine pakett") 