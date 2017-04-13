<properties 
    pageTitle="Ettevõtte integreerimine Pack B2B lahenduste loomine | Microsoft Azure'i rakendust Service | Microsoft Azure'i" 
    description="Lisateavet ettevõtte integreerimine pakett B2B funktsioonid kaudu andmete" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="learn-about-receiving-data-using-the-b2b-features-of-the-enterprise-integration-pack"></a>Lisateavet ettevõtte integreerimine pakett B2B funktsioonid kaudu andmete#

## <a name="overview"></a>Ülevaade ##

See dokument kuulub loogika rakenduste Enterprise integreerimine pakett. Vaadake lisateavet [Enterprise integreerimine pakett võimaluste](./app-service-logic-enterprise-integration-overview.md)ülevaade.

## <a name="prerequisites"></a>Eeltingimused ##

Funktsiooni AS2 ja X12 kasutamiseks peate integreerimine Enterprise konto toimingud

[Kuidas luua konto Enterprise integreerimine](./app-service-logic-enterprise-integration-accounts.md)

## <a name="how-to-use-the-logic-apps-b2b-connectors"></a>Kuidas kasutada loogika rakenduste B2B konnektorid ##

Kui olete loonud konto integreerimine ja lisatud partnerid ja lepingute selle olete valmis loogika rakenduse loomine, mis rakendab töövoo ettevõttelt ettevõttele (B2B).

Selles walkthru näete, kuidas kasutada funktsiooni AS2 ja X12 toimingud ettevõttelt ettevõttele loogika rakendus, mida saab kaubanduspartner loomiseks.

1. Luua uue loogika rakendus ja [linkida selle integreerimine kontole](./app-service-logic-enterprise-integration-accounts.md).  
2. **Taotluse – kui an HTTP-päringu** käivitamiseks rakenduse loogika lisamine  
![](./media/app-service-logic-enterprise-integration-b2b/flatfile-1.png)  
3. Lisada **Dekodeerida AS2** toimingust esimene valimise **toimingu lisamine**  
![](./media/app-service-logic-enterprise-integration-b2b/transform-2.png)  
4. Sisestage otsinguväljale sõna **as2** selleks, et filtreerida kõik toimingud, mida soovite kasutada, kes  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-5.png)  
6. Valige toiming **AS2 - dekodeerida AS2 sõnumi**  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-6.png)  
7. Nagu on näidatud, lisada **sisu** , mis võtab sisendina. Selle näite puhul valige keha vallandanud loogika rakenduse HTTP-päring. Teise võimalusena võite sisestada avaldise sisestada päised väljale**PÄISED** .

    @triggerOutputs()['headers']

8. Saate lisada **päiseid** AS2 jaoks vajalike. Need on HTTP taotluse päised. Selle näite puhul valige HTTP-päringu vallandanud loogika rakenduse päised.
9. Nüüd Decode X12 sõnumi toimingu lisada, valides uuesti **toimingu lisamine**  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-9.png)   
10. Sisestage otsinguväljale sõna **x12** selleks, et filtreerida kõik toimingud, mida soovite kasutada, kes  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-10.png)  
11. Valige soovitud **X12-dekodeerida X12 sõnumi** toimingu lisamiseks loogika rakendus  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-as2message.png)  
12. Nüüd on vaja seda toimingut, mis on ülaltoodud AS2 toimingu väljund sisendi määramiseks. Tegelik sõnumi sisu on JSON objekt ja base64 kodeeringuga. Seetõttu tuleb määrata avaldise nagu sisend nii sisestage järgmine avaldis **X12 TASAPINNALISE faili sõnumi adressaat DECODE** sisestatud väli  

    @base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])  

13. See toiming on dekodeerida X12 andmed saadud äripartneri ja väljund JSON objekti üksuste arvu. Et lasta partneri leida andmete saamist saate saata tagasi vastust, mis sisaldab soovitud AS2 sõnumi likvideerimise teatis (lahti) klõpsake toimingu HTTP-vastus  
14. **Vastuse** toimingu lisada, valides **Lisa toiming**   
![](./media/app-service-logic-enterprise-integration-b2b/b2b-14.png)  
15. Sisestage otsinguväljale sõna **vastuse** selleks, et filtreerida kõik toimingud, mida soovite kasutada, kes  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-15.png)  
16. Valige **vastuse** toiming, et lisada see  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-16.png)  
17. Määrake väljal vastuse **keha** juurde pääseda **Decode X12 sõnumi** toimingu väljund selle lahti järgmine avaldis abil  

    @base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])  

![](./media/app-service-logic-enterprise-integration-b2b/b2b-17.png)  
18. Töö salvestamine  
![](./media/app-service-logic-enterprise-integration-b2b/transform-5.png)  

Selles etapis, kui olete lõpetanud oma B2B loogika rakenduse häälestamine. Tõeline rakenduses, võib-olla soovite talletada dekodeeritud X12 andmete LOB rakenduse või andmete poest. Saate hõlpsasti lisada toiming või neile kirjutamiseks kohandatud API-de loomiseks LOB rakenduste ja kasutada neid API-de loogika rakenduse lisatoiminguid.

## <a name="features-and-use-cases"></a>Funktsioonid ja kasutamise juhtudel ##

- Funktsiooni AS2 ja X12 dekodeerida ja kodeerida toimingud luba olukorrale, kasutades valdkonna standard Protokollid loogika rakenduste kasutamise andmeid saata ja vastu võtta andmeid  
- Saate AS2 ja X12 koos või ilma omavahel vahetada andmeid kaubanduspartneritega vastavalt vajadusele
- B2B toimingud oleks hõlpsam partnerid ja lepingute integreerimine konto loomine ja kasutada neid loogika rakendus  
- Pikendades rakenduse loogika muude toimingute saate saata ja vastu võtta andmeid ja muude rakenduste ja teenuste, nt Salesforce'i  

## <a name="learn-more"></a>Lisateave ##

[Lisateavet ettevõtte integreerimine pakett](./app-service-logic-enterprise-integration-overview.md)  