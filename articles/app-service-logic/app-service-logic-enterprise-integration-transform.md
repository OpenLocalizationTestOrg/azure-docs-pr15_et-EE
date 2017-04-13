<properties 
    pageTitle="Ettevõtte integreerimine Pack ülevaade | Microsoft Azure'i rakendust Service | Microsoft Azure'i" 
    description="Kasutada Enterprise integreerimine Pack protsess ja integreerimise äriprotsessid Microsoft Azure'i rakenduse teenuse kasutamise lubamine" 
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

# <a name="enterprise-integration-with-xml-transforms"></a>XML-i teisendusteta Enterprise integreerimine

## <a name="overview"></a>Ülevaade
Ettevõtte integreerimine transformatsioon konnektor teisendab andmeid ühest vormingust mõnda muusse vormingusse. Näiteks võib teil sissetuleva sõnumi, mis sisaldab praeguse kuupäeva vormingus YearMonthDay. Saate vormindada, et MonthDayYear vormingus kuupäeva teisenduse.

## <a name="what-does-a-transform-do"></a>Mida teeb teisenduse?
Teisenduse, mis on ka kaardil, koosneb on allikas XML-skeemi (input) ja Target XML schema (väljund). Abil saate eri sisseehitatud funktsioonid aitavad töödelda või andmete, sh stringi toiminguid, tingimusvormingu ülesanded, aritmeetilisi avaldisi, kuupäev aja formatters ja isegi hoidke importida.

## <a name="how-to-create-a-transform"></a>Kuidas luua teisenduse?
Visual Studio [Enterprise integratsioon SDK](https://aka.ms/vsmapsandschemas)abil saate luua transformatsioon/kaarti. Kui olete lõpetanud, loomine ja testimine transformatsioon, laadige transformatsioon integreerimine kontole. 

## <a name="how-to-use-a-transform"></a>Kuidas kasutada teisenduse
Pärast üleslaadimist transformatsioon integreerimine kontole, saate luua rakenduse loogika. Loogika rakendus käivitub seejärel oma töötulemusi iga kord, kui käivitatakse loogika rakendus (ja Sisestuskeel sisu, mis tuleb muuta).

**Järgnevalt on toodud juhised ja transformatsiooni lisamiseks kasutada**.

### <a name="prerequisites"></a>Eeltingimused 
Klõpsake eelvaates peate:  

-  [Azure'i funktsioonid on container loomine] (https://ms.portal.azure.com/#create/Microsoft.FunctionApp "Azure'i funktsioonid on container loomine")  
-  [Azure'i funktsioonide container funktsiooni lisamine] (https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-transform-function%2Fazuredeploy.json "See mall loob webhook vastavalt C# azure funktsioon transformatsioon võimaluste kasutamine loogika rakenduste integreerimise stsenaariumid")    
-  Looge konto integreerimine ja kaardi lisamine  

>[AZURE.TIP] Märkige üles soovitud Azure'i funktsioonid ja Azure funktsiooni nime, peate need järgmises etapis.  

Nüüd, kui te tehtud hoolikalt eeltingimused, on aeg loogika rakenduse loomiseks:  

1. Saate luua loogika rakendus ja [link seda, et teie konto]sisaldab kaardi(./app-service-logic-enterprise-integration-accounts.md "Õpi loogika rakenduse integreerimise kontoga linkida") .
2. **Taotluse – kui an HTTP-päringu** käivitamiseks rakenduse loogika lisamine  
![](./media/app-service-logic-enterprise-integration-transforms/transform-1.png)    
3. **XML-i transformatsioon** toimingu, esimene valimise **lisada toimingu** lisamine   
![](./media/app-service-logic-enterprise-integration-transforms/transform-2.png)   
4. Sisestage sõna *muuta* otsinguväljale selleks, et kõik toimingud, mida soovite kasutada, kes filtreerimine  
![](./media/app-service-logic-enterprise-integration-transforms/transform-3.png)  
5. Valige toiming **XML-i transformatsioon**   
![](./media/app-service-logic-enterprise-integration-transforms/transform-4.png)  
6. Valige **Funktsioon ümbris** , kasutage funktsiooni sisaldav. See on nimi Azure funktsioonide s.o ümbris, mille lõite järgmist.
7. Valige **funktsioon** , mida soovite kasutada. See on varem loodud Azure'i funktsiooni nime.
8. Lisage XML-i **sisu** saate muuta. Pange tähele, mida saate kasutada mis tahes XML-andmed kuvatakse HTTP-päring nimega selle **sisu**. Selle näite puhul valige keha vallandanud loogika rakenduse HTTP-päring.
9. Valige **kaardi** abil teha transformatsiooni soovitud nimi. Kaardi peab olema juba oma konto. Varasemas etapis tuleb teil juba andis oma loogika rakenduse access integreerimine kontole kaarti sisaldavale.
10. Töö salvestamine  
![](./media/app-service-logic-enterprise-integration-transforms/transform-5.png) 

Selles etapis, kui olete lõpetanud, häälestada oma kaarti. Tõeline rakenduses, võite näiteks Salesforce'i rakenduses LOB ümber andmete talletamiseks. Saate hõlpsasti toimingu väljund transformatsioon saatmiseks Salesforce'i nimega. 

Nüüd saate oma transformatsioon, muutes taotluse HTTP lõpp-punkti testida.  

## <a name="features-and-use-cases"></a>Funktsioonid ja kasutamise juhtudel

- Teisendus, mis on loodud kaardil võivad olla lihtsad, näiteks kopeerimine teise ühe dokumendi nimi ja aadress. Või saate luua keerukamaid teisendused out-of-box kaardi toimingute abil.  
- Mitme kaardi toimingute või funktsioonid on käepärast, sh stringid, kuupäeva-ja kellaajafunktsioonid ja jne.  
- Saate teha otse andmete kopeerimine skeemide vahel. Plaanuri, SDK kaasatud, on sama lihtne kui tõmmata joone, mis ühendab skeemi allikas elemendid oma kolleegidega sihtkoha skeemi.  
- Kaardi loomisel saate vaadata graafiliselt kaart, mis näitavad seosed ja linkide loomist.
- Funktsiooni Test kaardi lisamiseks meilisõnumi näidis XML-i. Klõpsuga, saate testida loodud kaardi ja vaadata loodud väljundi.  
- Laadige üles olemasolevad kaardid  
- XML-vormingus toetab.


## <a name="learn-more"></a>Lisateave
- [Lisateavet leiate teemast hoolduspaketi Enterprise integreerimise kohta] (./app-service-logic-enterprise-integration-overview.md "Lisateavet ettevõtte integreerimine pakett")  
- [Lisateavet leiate teemast kaardid] (./app-service-logic-enterprise-integration-maps.md "Vaadake, kuidas ettevõtte integreerimine kaardid")  
 