<properties 
    pageTitle="Lisateavet ettevõtte integreerimine Pack kodeerida X12 sõnumi Connctor | Microsoft Azure'i rakendust Service | Microsoft Azure'i" 
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

# <a name="get-started-with-encode-x12-message"></a>Encode X12 sõnumi kasutamise alustamine

Kinnitatakse EDI ja partneri kohased atribuudid, teisendab XML-kodeeringuga sõnumeid EDI tehingu komplekti rakenduses andmevahetuse ja taotleb tehniliste ja/või funktsionaalne kinnitus

## <a name="create-the-connection"></a>Ühenduse loomine

### <a name="prerequisites"></a>Eeltingimused

* Azure'i konto; [tasuta konto](https://azure.microsoft.com/free) loomisel

* Konto integreerimine on vajalik Encode x12 sõnumi Connectori kasutamine. Üksikasju vt kohta, kuidas luua [Konto](./app-service-logic-enterprise-integration-create-integration-account.md), [partnerite](./app-service-logic-enterprise-integration-partners.md) ja [X12 lepingu](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-encode-x12-message-using-the-following-steps"></a>Ühenduse loomine Encode X12 sõnumi, tehes järgmist:

1. [Loogika rakenduse loomine](./app-service-logic-create-a-logic-app.md) pakub näide

2. See konnektor ei saa mis tahes päästikute. Käivitage rakendus loogika, nt taotluse päästik muude päästikute kasutamine.  Loogika rakenduse designer, lisage käivitamiseks ja toimingu lisada.  Valige Kuva Microsoft hallatava API-de ripploendis soovitud loend ja seejärel sisestage "x12" väljale Otsing.  Valige kas X12-kodeerida X12 X12-X 12 sõnumile, identiteedid kodeerida või teade lepingu nime järgi.  

    ![x12 otsimine](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png) 

3. Kui te pole varem loonud kõik ühendused konto, küsitakse ühenduse üksikasjad

    ![integreerimine konto ühenduse](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage1.png) 


4. Sisestage konto üksikasjad.  Tärniga atribuudid on nõutav

  	| Atribuut | Üksikasjad |
  	| -------- | ------- |
  	| Ühenduse nimi * | Sisestage mis tahes ühenduse nimi |
  	| Konto * | Sisestage konto nimi. Veenduge, et teie konto ja loogika rakendus, Azure paigal |

    Kui olete lõpetanud, järgmine sarnanevad oma ühenduse üksikasjad

    ![integreerimine kontoga ühenduse loonud](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage2.png) 


5. Valige **loomine**

6. Pange tähele, et ühendus on loodud.

    ![integreerimine konto ühenduse üksikasjad](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage3.png) 

#### <a name="x12---encode-x12-message-by-agreement-name"></a>X12-kodeerida X12 sõnumi lepingu nime järgi

7. Valige X12 lepingu kodeerida sõnumist rippmenüü ja XML-i.

    ![Sisestage kohustuslikud](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage4.png) 

#### <a name="x12---encode-x12-message-by-identities"></a>X12-kodeerida X12 identiteedid sõnum

7.  Pakkuda saatja identifikaator, saatja kvalifikatsioon, vastuvõtja identifikaator ja vastuvõtja kvalifikatsioon konfigureeritud on X12 leping.  XML-i sõnumi kodeerida valimine

    ![Sisestage kohustuslikud](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage5.png) 

## <a name="x12-encode-does-following"></a>X12 kodeerida teeb jälgimise:

* Lepingu eraldusvõime sobitamine saatja ega vastuvõtja pilti kontekstis atribuudid.
* Serializes EDI andmevahetuse, XML-kodeeringuga sõnumeid teisendamisel EDI tehingu komplekti andmevahetuse sisse.
* Kehtib tehingu määramine päise- ja haagise segmente
* Loob rühma juhtelement arvu ja tehingu määramine juhtelemendi number iga väljamineva andmevahetuse andmevahetuse juhtelemendi rohkem
* Asendab eraldajad last andmed
* Kinnitatakse EDI ja partneri kohased atribuudid
    * Skeemi sõnumi skeemi elementide tehingu seadistatud andmete valideerimine
    * Tehingu seadistatud andmed läbi EDI valideerimine.
    * Laiendatud valideerimise läbi tehingu seadistatud andmete elemendid
* Taotleb tehniliste ja/või funktsionaalne kinnitus (kui see on konfigureeritud).
    * Tehniline kinnitus loob tulemusena päise valideerimine. Tehniline kinnitus aruannete oleku andmevahetuse päise-ja haagise saaja aadressi töötlemine
    * Otstarbekas kinnitus loob tulemusena keha valideerimine. Otstarbekas kinnitus aruannete iga tõrge ilmnes vastuvõetud dokumendi töötlemine

## <a name="next-steps"></a>Järgmised sammud

[Lisateavet leiate teemast hoolduspaketi Enterprise integreerimise kohta] (./app-service-logic-enterprise-integration-overview.md "Lisateavet ettevõtte integreerimine pakett") 

