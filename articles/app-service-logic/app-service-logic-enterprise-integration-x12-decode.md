<properties 
    pageTitle="Lisateavet ettevõtte integreerimine Pack dekodeerida X12 sõnumi Connctor | Microsoft Azure'i rakendust Service | Microsoft Azure'i" 
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

# <a name="get-started-with-decode-x12-message"></a>Decode X12 sõnumi kasutamise alustamine

Kinnitatakse EDI ja partneri kohased atribuudid, loob XML-dokumendi iga tehingu kogumi ja loob töödeldud tehingu kinnitus.

## <a name="create-the-connection"></a>Ühenduse loomine

### <a name="prerequisites"></a>Eeltingimused

* Azure'i konto; [tasuta konto](https://azure.microsoft.com/free) loomisel

* Decode X12 sõnumi konnektor kasutamiseks on vajalik konto integreerimine. Üksikasju vt kohta, kuidas luua [Konto](./app-service-logic-enterprise-integration-create-integration-account.md), [partnerite](./app-service-logic-enterprise-integration-partners.md) ja [X12 lepingu](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-decode-x12-message-using-the-following-steps"></a>Ühenduse loomine Decode X12 sõnumi, tehes järgmist:

1. [Loogika rakenduse loomine](./app-service-logic-create-a-logic-app.md) pakub näide

2. See konnektor ei saa mis tahes päästikute. Käivitage rakendus loogika, nt taotluse päästik muude päästikute kasutamine.  Loogika rakenduse designer, lisage käivitamiseks ja toimingu lisada.  Valige Kuva Microsoft hallatava API-de ripploendis soovitud loend ja seejärel sisestage "x12" väljale Otsing.  Valige X12 – dekodeerida X12 sõnum

    ![x12 otsimine](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png)  

3. Kui te pole varem loonud kõik ühendused konto, küsitakse ühenduse üksikasjad

    ![integreerimine konto ühenduse](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage4.png)    

4. Sisestage konto üksikasjad.  Tärniga atribuudid on nõutav

  	| Atribuut | Üksikasjad |
  	| -------- | ------- |
  	| Ühenduse nimi * | Sisestage mis tahes ühenduse nimi |
  	| Konto * | Sisestage konto nimi. Veenduge, et teie konto ja loogika rakendus, Azure paigal |

    Kui olete lõpetanud, järgmine sarnanevad oma ühenduse üksikasjad
    
    ![integreerimine kontoga ühenduse loonud](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage5.png) 

5. Valige **loomine**
    
6. Pange tähele, et ühendus on loodud.

    ![integreerimine konto ühenduse üksikasjad](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage6.png) 

7. Valige X12 tasapinnalise faili sõnumi dekodeerida

    ![Sisestage kohustuslikud](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage7.png) 

## <a name="x12-decode-does-following"></a>X12 dekodeerida teeb jälgimine

* Kinnitatakse ümbrik börsipäev partneri lepingu vastu
* Loob XML-dokumendi iga tehingu kogumi.
* Kinnitatakse EDI ja partneri kohased atribuudid
    * EDI strukturaalset valideerimine ja laiendatud skeemi valideerimine
    * Ümbriku andmevahetuse struktuuri valideerimine.
    * Skeemi valideerimine ümbriku juhtelemendi skeemi suhtes.
    * Skeemi valideerimine tehingu seadistatud andmete elementide sõnumi skeemi suhtes.
    * Tehingu seadistatud andmed läbi EDI valideerimine 
* Kinnitab, et andmevahetuse, rühma ja tehingu määramine juhtelemendi arvud pole duplikaadid
    * Kontrollib andmevahetuse juhtelemendi number varem saadud tehnosiirdeid suhtes.
    * Kontrollib jaotises juhtelemendi number andmevahetuse muude rühma juhtelement arvude suhtes.
    * Kontrollib, kas tehingu juhtelemendi numbri saate määrata selle rühma teiste tehingu määramine juhtelemendi arvude suhtes.
* Teisendab kogu andmevahetuse XML-i 
    * Kui tehingu komplekti - tükeldatud andmevahetuse peatada tehingu komplekti tõrge: sõelub iga seadmine teabevahetus eraldi XML-i dokumenti tehingu. Kui üks või mitu tehingu seab andmevahetuse valideerimine, X12 nurjub Decode peatab ainult need tehingu komplekti.
    * Kui tehingu komplekti - tükeldatud andmevahetuse peatada interchange tõrge: sõelub iga seadmine teabevahetus eraldi XML-i dokumenti tehingu.  Kui üks või mitu tehingu seab andmevahetuse nurjuda valideerimist, X12 Decode peatab kogu andmevahetuse.
    * Säilitada Interchange - peatada tehingu komplekti tõrge: loob XML-dokumendi kogu batched andmevahetuse. X12 Decode peatab ainult need tehingu komplekti, mille valideerimine nurjub, jätkates kaudu saate töödelda kõik muud määrab
    * Säilitada Interchange - peatada interchange tõrge: loob XML-dokumendi kogu batched andmevahetuse. Kui üks või mitu tehingu seab andmevahetuse valideerimine, X12 nurjub Decode peatab kogu andmevahetuse, 
* Loob tehniliste ja/või funktsionaalne kinnitus (kui see on konfigureeritud).
    * Tehniline kinnitus loob tulemusena päise valideerimine. Tehniline kinnitus aruannete andmevahetuse päise-ja haagise saaja aadressi töötlemise olekut.
    * Otstarbekas kinnitus loob tulemusena keha valideerimine. Otstarbekas kinnitus aruannete iga tõrge ilmnes vastuvõetud dokumendi töötlemine

## <a name="next-steps"></a>Järgmised sammud

[Lisateavet leiate teemast hoolduspaketi Enterprise integreerimise kohta] (./app-service-logic-enterprise-integration-overview.md "Lisateavet ettevõtte integreerimine pakett") 


