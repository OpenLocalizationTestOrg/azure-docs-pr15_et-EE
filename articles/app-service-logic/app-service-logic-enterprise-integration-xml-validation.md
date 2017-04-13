<properties 
    pageTitle="Ülevaade XML-i valideerimine Enterprise integreerimine Pack | Microsoft Azure'i rakendust Service | Microsoft Azure'i" 
    description="Siit saate teada, valideerimine Enterprise integreerimine keelepaketi ja loogika rakendused tööpõhimõtted" 
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

# <a name="enterprise-integration-with-xml-validation"></a>XML-i valideerimine Enterprise integreerimine

## <a name="overview"></a>Ülevaade
Sageli B2B stsenaariumid, partnerite lepinguga vaja kinnitada need omavahel vahetada sõnumid on lubatud enne andmete töötlemise alustamist. Ettevõtte integreerimine pakett, saate XML-i valideerimine konnektor valideerimiseks dokumentide eelmääratletud skeemi suhtes.  

## <a name="how-to-validate-a-document-with-the-xml-validation-connector"></a>Kuidas valideerimiseks dokumendi XML-i valideerimine konnektor
1. Saate luua loogika rakendus ja [link seda, et teie konto]sisaldab kasutate XML-andmete valideerimine skeemi(./app-service-logic-enterprise-integration-accounts.md "Õpi loogika rakenduse integreerimise kontoga linkida") .
2. **Taotluse – kui an HTTP-päringu** käivitamiseks rakenduse loogika lisamine  
![](./media/app-service-logic-enterprise-integration-xml/xml-1.png)    
3. **XML-i valideerimine** toimingu, esimene valimise **lisada toimingu** lisamine  
4. Sisestage väljale Otsing *XML-i* selleks, et kõik toimingud, mida soovite kasutada, kes filtreerimine 
5. Valige **XML-i valideerimine**     
![](./media/app-service-logic-enterprise-integration-xml/xml-2.png)   
6. Valige **sisu** tekstiväli  
![](./media/app-service-logic-enterprise-integration-xml/xml-1-5.png)
7. Valige sisu sildi sisu, mis on kinnitatud.   
![](./media/app-service-logic-enterprise-integration-xml/xml-3.png)  
8. Valige loendiboksi **SKEEMI nimi** ja valige soovitud valideerimiseks Sisestuskeel *sisu* ülaltoodud skeem     
![](./media/app-service-logic-enterprise-integration-xml/xml-4.png) 
9. Töö salvestamine  
![](./media/app-service-logic-enterprise-integration-xml/xml-5.png) 

Selles etapis, kui olete lõpetanud, häälestada oma valideerimine konnektor. Tõeline rakenduses, võite näiteks Salesforce'i rakenduses LOB valideeritud andmete talletamiseks. Toimingu väljund valideerimise saatmiseks Salesforce'i saate hõlpsasti lisada. 

Nüüd saate testida valideerimine toimingu, muutes taotluse HTTP lõpp-punkti.  

## <a name="next-steps"></a>Järgmised sammud

[Lisateavet leiate teemast hoolduspaketi Enterprise integreerimise kohta] (./app-service-logic-enterprise-integration-overview.md "Lisateavet ettevõtte integreerimine pakett")   